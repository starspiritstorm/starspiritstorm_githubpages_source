---

title: "MSSQL - Get continuous sequenced start of month and end of month from a date range"
author: starspiritstorm
type: posts
date: 2022-03-31T12:00:00+08:00
draft: false
categories: ["MSSQL"]
tags: ["MSSQL", "Function", "MSSQL 2008", "MSSQL 2017", "Monthly date"]
keywords: ["start of month and end of month from a date range"]

---


A Function of generating continuous sequenced start of month and end of month from a date range.


<!--more-->


Code below here.


	/*
	=============================================
	Author:	starspiritstorm
	Create date: 2021-08-17
	Description: A Function of generating continuous sequenced start of month and end of month from a date range 

	Reference: 
	https://stackoverflow.com/questions/21189369/better-way-to-generate-months-year-table
	https://stackoverflow.com/questions/1520789/how-can-i-select-the-first-day-of-a-month-in-sql?page=1&tab=votes#tab-top
	https://stackoverflow.com/questions/1051488/get-the-last-day-of-the-month-in-sql

	CTE version
	https://stackoverflow.com/questions/33510077/sql-split-period-by-months

	=============================================
	*/

	CREATE FUNCTION [dbo].[F_Get_Sequenced_MonthStart_And_MonthEnd]
	(
		  @FromDate DATETIME
		, @ToDate DATETIME
		, @UpdateBeginDate BIT
		, @UpdateEndDate BIT
	)
	RETURNS @Results TABLE 
	( 
		  MonthStart DATETIME
		, MonthEnd DATETIME 
		, TheMonth VARCHAR(2) 
		, TheYear VARCHAR(4)
	)
	AS
	BEGIN

		 -- Months in that period
		 INSERT INTO @Results
		 SELECT TOP (DATEDIFF(MONTH, @FromDate, @ToDate)+1) -- calculate how many rows needed
				  DATEADD(MONTH, number, @FromDate)
				, DATEADD(MONTH, number, @FromDate)
				, MONTH(DATEADD(MONTH, number, @FromDate))
				, YEAR(DATEADD(MONTH, number, @FromDate))
		   FROM [master].dbo.spt_values 
		  WHERE [type] = N'P' 
		  ORDER BY number

		 /*
			 by using [master].dbo.spt_values, the total series number are 2048 (0~2047), 
			 for month cases, there will be 2048/12 ≈ 170 year range
		 */

		 -- Update first date of month and last date of month for each row
		 UPDATE @Results
			SET MonthStart = DATEADD(MONTH, DATEDIFF(MONTH, 0, MonthStart), 0) 
			  , MonthEnd = DATEADD(MONTH, ((YEAR(MonthEnd) - 1900) * 12) + MONTH(MonthEnd), -1)
		 
		 /*
			 MonthStart
			 Caculate the month difference from 1900-01-01 (0 represent 1900-01-01) to the current row's MonthStart record (DATEDIFF part)
			 Then add the month difference from 1900-01-01 (DATEADD part)
		   
			 MonthEnd
			 Caculate the year difference from 1900 to the current row's MonthEnd record then multiply 12 and plus the months of the record (The middle part)
			 Then add the month difference from 1899-12-31 (DATEADD part)
			 NOTE: -1 means 1899-12-31
			 
			 The MonthEnd sentence is equivalent to this, if you keeped the record of TheYear and TheMonth
			 MonthEnd = DATEADD(MONTH, ((TheYear - 1900) * 12) + TheMonth, -1)
				
			 Or you can just write like this, but I don't really like those plus 1
			 MonthEnd = DATEADD(MONTH, DATEDIFF(MONTH, 0, MonthEnd) + 1, -1)
		 */

		 IF @UpdateBeginDate = 1
		 BEGIN
			 UPDATE @Results 
				SET MonthStart = @FromDate
			  WHERE MonthStart = (SELECT MIN(MonthStart) FROM @Results)
		 END


		 IF @UpdateEndDate = 1
		 BEGIN
			 UPDATE @Results 
				SET MonthEnd = @ToDate
			  WHERE MonthEnd = (SELECT MAX(MonthEnd) FROM @Results)
		 END


		 RETURN

	END
 


[Here's the testing result](https://dbfiddle.uk/?rdbms=sqlserver_2017&fiddle=167dd389fec46d8897cdd626e3f3631a)
	


