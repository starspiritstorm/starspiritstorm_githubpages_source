---

title: "MSSQL - 給定起始&結束日期，取得連續的每月月初跟每月月尾"
author: starspiritstorm
type: posts
date: 2022-03-31T12:00:00+08:00
draft: false
categories: ["MSSQL"]
tags: ["MSSQL", "Function", "MSSQL 2008", "MSSQL 2017", "Monthly date"]
keywords: ["start of month and end of month from a date range"]

---


Function 給定起始&結束日期，取得連續的每月月初跟每月月尾。


<!--more-->


Code 在此。


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
			 藉由 MSSQL 的系統 table，[master].dbo.spt_values 取得連續的數字，數字總共有2048個 (0~2047)
			 月份的情況下，可以有170年的間距，所以可以放心地使用
		 */

		 -- Update first date of month and last date of month for each row
		 UPDATE @Results
			SET MonthStart = DATEADD(MONTH, DATEDIFF(MONTH, 0, MonthStart), 0) 
			  , MonthEnd = DATEADD(MONTH, ((YEAR(MonthEnd) - 1900) * 12) + MONTH(MonthEnd), -1)
		 
		 /*
			 MonthStart
			 先取得從 1900-01-01 (0代表1900-01-01) 到當筆記錄 MonthStart 的月份差 ( DATEDIFF 的部分)
			 再從 1900-01-01 加上剛算完的月份差 ( DATEADD 的部分)
		   
			 MonthEnd
			 NOTE: -1 means 1899-12-31
			 先取得從當筆記錄 MonthEnd 的年份差後*12 再加上當筆的月份 (中間計算的部分)
			 再從 1899-12-31 加上剛算完的月份差 ( DATEADD 的部分)
			 
			 MonthEnd 的另一種寫法，如果你有保留 Function 內的 TheYear and TheMonth 的欄位，MonthEnd 可以改寫成下面的樣式
			 MonthEnd = DATEADD(MONTH, ((TheYear - 1900) * 12) + TheMonth, -1)
				
			 MonthEnd 的另一種寫法，但我不是很喜歡+1、-1的寫法，還需要腦內轉換。
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
 


[測試結果](https://dbfiddle.uk/?rdbms=sqlserver_2017&fiddle=167dd389fec46d8897cdd626e3f3631a)
	


