---

title: "MSSQL - 多筆 row 字串連接"
author: starspiritstorm
type: posts
date: 2022-04-01T12:00:00+08:00
draft: false
categories: ["MSSQL"]
tags: ["MSSQL", "Function", "MSSQL 2008", "MSSQL 2017", "Concatenate string"]
keywords: ["concatenate string"]

---


多筆 row 字串連接。


<!--more-->


我是用下面的關鍵字去 google 的

	SQL string Concatenate 


3種[方法](https://www.mytecbits.com/microsoft/sql-server/concatenate-multiple-rows-into-single-string)

1. for xml path

2. COALESCE

3. STRING_AGG



用 for xml path 遇到特殊字元的處理[方式](https://blogs.lobsterpot.com.au/2010/04/15/handling-special-characters-with-for-xml-path/)。


中文的 for xml path 的參考，[如何透過for xml path 搭配STUFF將多ROW資料合併同一ROW](https://coolmandiary.blogspot.com/2020/11/t-sql12for-xml-path-stuffrowrow.html)，
以及[食譜好菜 SQL Server 使用「FOR XML」語法做欄位合併](https://dotblogs.com.tw/supershowwei/2016/01/26/145353)。


還有 performance 的比較

[for xml path vs CTE](https://blog.darkthread.net/blog/col-merge-benchmark)


for xml path vs STRING_AGG

[sql-server-v-next-string_agg-performance](https://sqlperformance.com/2016/12/sql-performance/sql-server-v-next-string_agg-performance)

[sql-server-v-next-string_agg-performance](https://sqlperformance.com/2017/01/sql-performance/sql-server-v-next-string_agg-performance-part-2)


結論，MSSQL 2017 之後版本的人就乖乖用 STRING_AGG 吧..



## XML PATH 方法連接 strings



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
	-- 如果不明白 xml path 怎麼運作的，可以把這塊註解拿掉看一下OUTPUT

	SELECT DISTINCT ID 
		 , STUFF((SELECT ',' + name
					FROM @temp_origin AS inner_list
				   WHERE inner_list.ID = outer_list.ID 
				   for xml path(''), root('MyString'), type
				 ).value('/MyString[1]','varchar(max)')
				,1,1,'') AS inner_concatenate_name
	  FROM @temp_origin AS outer_list

	*/


> NOTE: 這塊是在處理 "&" 之類的特殊字元。
>
>                              , root('MyString'), type
>     ).value('/MyString[1]','varchar(max)')
>  
> 如果你沒加這塊，"&" 在 XML path 轉換過後會變成 "&amp;"


## COALESCE 方法連接 strings

	/*
	-- COALESCE 方法沒辦法一個 SELECT 搞定，所以就不用了
	DECLARE @val Varchar(MAX); 

	SELECT @val = COALESCE(@val + ', ' + name, name) 
	  FROM @temp_origin 
	  
	SELECT @val;
	*/


## STRING_AGG 方法連接 strings


	-- STRING_AGG method
	SELECT ID
		 , input_date
		 , STRING_AGG( ISNULL(name, ' '), ',') As name 
	  From @temp_origin
	 GROUP BY ID, input_date



[測試結果](https://dbfiddle.uk/?rdbms=sqlserver_2017&fiddle=bfc3fb737203a5b7e23f0aaa9bf4423b)

