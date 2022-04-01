---

title: "MSSQL - Multi-rows string concatenating"
author: starspiritstorm
type: posts
date: 2022-04-01T12:00:00+08:00
draft: false
categories: ["MSSQL"]
tags: ["MSSQL", "Function", "MSSQL 2008", "MSSQL 2017", "Concatenate string"]
keywords: ["concatenate string"]

---


Multi-rows string concatenating.


<!--more-->


I used these key words searching it from google.

	SQL string Concatenate 


3 [Ways](https://www.mytecbits.com/microsoft/sql-server/concatenate-multiple-rows-into-single-string)

1. for xml path

2. COALESCE

3. STRING_AGG



for xml path, special character handling [method](https://blogs.lobsterpot.com.au/2010/04/15/handling-special-characters-with-for-xml-path/).


Some chinese for xml path reference，[如何透過for xml path 搭配STUFF將多ROW資料合併同一ROW](https://coolmandiary.blogspot.com/2020/11/t-sql12for-xml-path-stuffrowrow.html)
and [食譜好菜 SQL Server 使用「FOR XML」語法做欄位合併](https://dotblogs.com.tw/supershowwei/2016/01/26/145353)


And performance comparison.

[for xml path vs CTE](https://blog.darkthread.net/blog/col-merge-benchmark)


for xml path vs STRING_AGG

[sql-server-v-next-string_agg-performance](https://sqlperformance.com/2016/12/sql-performance/sql-server-v-next-string_agg-performance)

[sql-server-v-next-string_agg-performance](https://sqlperformance.com/2017/01/sql-performance/sql-server-v-next-string_agg-performance-part-2)


Conclusion, if you are using MSSQL 2017 later version, just use the STRING_AGG.



## XML PATH method string concatenating



	DECLARE @temp_origin TABLE
	(
		ID INT
	  , input_date DATETIME
	  , name VARCHAR(50)
	) 
	-- a sample temp table
	INSERT INTO @temp_origin
	SELECT 1, '2021-05-05', 'Adam%'
	 UNION
	SELECT 1, '2021-05-05', 'Gray&'
	 UNION
	SELECT 1, '2021-05-05', 'Jacy'
	 UNION
	SELECT 2, '2021-05-08', 'Henry'
	 UNION
	SELECT 2, '2021-05-08', 'Alice'
	 UNION
	SELECT 3, '2021-05-15', 'Ben'



	-- xml path method
	SELECT DISTINCT 
		   temp.ID
		 , input_date
		 , inner_concatenate_name
	  FROM @temp_origin AS temp
	 OUTER APPLY (SELECT DISTINCT ID 
			   , STUFF((SELECT ',' + name
					  FROM @temp_origin AS inner_list
					 WHERE inner_list.ID = outer_list.ID 
					   for xml path(''), root('MyString'), type
					).value('/MyString[1]','varchar(max)')
				  ,1,1,'') AS inner_concatenate_name
			   FROM @temp_origin AS outer_list
			 ) AS T
	 WHERE temp.ID = T.ID


	/*
	-- for someone who don't understand the xml path work, you can uncomment this block

	SELECT DISTINCT ID 
		 , STUFF((SELECT ',' + name
					FROM @temp_origin AS inner_list
				   WHERE inner_list.ID = outer_list.ID 
				   for xml path(''), root('MyString'), type
				 ).value('/MyString[1]','varchar(max)')
				,1,1,'') AS inner_concatenate_name
	  FROM @temp_origin AS outer_list

	*/


> NOTE: The part of is handling the special character like  "&"
>
>                              , root('MyString'), type
>     ).value('/MyString[1]','varchar(max)')
>  
> If you don't add, the "&" will become "&amp;" in XML path


## COALESCE method string concatenating

	/*
	-- COALESCE method seems cannot simply do it in one shot
	DECLARE @val Varchar(MAX); 

	SELECT @val = COALESCE(@val + ', ' + name, name) 
	  FROM @temp_origin 
	  
	SELECT @val;
	*/


## STRING_AGG method string concatenating


	-- STRING_AGG method
	SELECT ID
		 , input_date
		 , STRING_AGG( ISNULL(name, ' '), ',') As name 
	  From @temp_origin
	 GROUP BY ID, input_date



[Here’s the testing result](https://dbfiddle.uk/?rdbms=sqlserver_2017&fiddle=bfc3fb737203a5b7e23f0aaa9bf4423b)

