---

title: "MSSQL - 拆分分隔符號變成多筆row"
author: starspiritstorm
type: posts
date: 2022-04-01T12:00:00+08:00
draft: false
categories: ["MSSQL"]
tags: ["MSSQL", "Function", "MSSQL 2008", "MSSQL 2017", "Split string"]
keywords: ["split comma-separated string into rows"]

---


拆分分隔符號變成多筆row


<!--more-->


我是用下面的關鍵字去 google 的

	sql separate string by comma into rows

找到第一筆 [stackoverflow](https://stackoverflow.com/questions/5493510/turning-a-comma-separated-string-into-individual-rows)

同篇文章有兩種寫法

一種寫法是在 MSSQL 2016 之前的寫法，CTE 遞迴方法

一種寫法是在 MSSQL 2016 之後的寫法，有內建 function，STRING_SPLIT


還有 [performance](http://sqlperformance.com/2016/03/t-sql-queries/string-split) 的比較


結論，MSSQL 2016 之後版本的人就乖乖用 STRING_SPLIT 吧..


## CTE 遞迴方法拆分 strings


	-- a sample temp table
	DECLARE @temp_origin TABLE
	(
		ID INT
	  , input_date DATETIME
	  , name VARCHAR(50)
	)

	INSERT INTO @temp_origin
	SELECT 1, '2021-05-05', 'Adam,Gray,Jacy'
	 UNION
	SELECT 2, '2021-05-08', 'Henry,Alice'
	 UNION
	SELECT 3, '2021-05-15', 'Ben'


	DECLARE @split_character VARCHAR(10)
		SET @split_character = ','



	;WITH split_temp(ID, input_date, splitted_name, step_by_step_to_split, origin_concatenated_name) AS
	(
		SELECT ID
			 , input_date
			 , CAST ( LEFT(name, CHARINDEX(@split_character, name + @split_character) - 1) AS VARCHAR(200)) 
			 , STUFF(name, 1, CHARINDEX(@split_character, name + @split_character), '')                     
			 , name
		  FROM @temp_origin
		  
			-- CTE 遞迴拆分名字
		 UNION all
		SELECT ID
			 , input_date
			 , CAST ( LEFT(step_by_step_to_split, CHARINDEX(@split_character, step_by_step_to_split + @split_character) - 1) AS VARCHAR(200))
			 , STUFF(step_by_step_to_split, 1, CHARINDEX(@split_character, step_by_step_to_split + @split_character), '')
			 , origin_concatenated_name
		  FROM split_temp
		 WHERE step_by_step_to_split > ''
	)
	SELECT ID 
		 , input_date 
		 , splitted_name
		 , step_by_step_to_split
		 , origin_concatenated_name
	  FROM split_temp
	 ORDER BY ID
	-- OPTION (maxrecursion 0)

	-- 通常 CTE 遞迴次數限制為100次，如果你知道你的 string 有這麼長，記得把 option 反註解掉

	/*
	-- 如果不明白 CHARINDEX, LEFT, STUFF 怎麼運作的，可以把這塊註解拿掉看一下OUTPUT

		SELECT ID
			 , input_date
			 , CHARINDEX(@split_character, name + @split_character) - 1
			 , CAST ( LEFT(name, CHARINDEX(@split_character, name + @split_character) - 1) AS VARCHAR(200))	-- 找到第一個分隔符號，然後用 LEFT 取出第一個分隔符號前的名字
			 , CHARINDEX(@split_character, name + @split_character)											  
			 , STUFF(name, 1, CHARINDEX(@split_character, name + @split_character), '')						-- 然後用 STUFF 把第一個分隔符號前的名字移除
			 , name
		  FROM @temp_origin
	*/


## STRING_SPLIT 方法拆分 strings


如果你是 MSSQL 2016 之後的版本，你可以使用 STRING_SPLIT ，在剛剛的同一篇 stackoverflow 裡面第二個回覆就有說明如何使用

	SELECT ID
		 , input_date
		 , cs.Value --SplitData
	  FROM @temp_origin
	 CROSS APPLY STRING_SPLIT (name, @split_character) cs
  

[測試結果](https://dbfiddle.uk/?rdbms=sqlserver_2017&fiddle=f9f488d3f8e9dd1ffd214fe07e92cad3)
	


