---

title: "MSSQL - Split comma-separated string into rows"
author: starspiritstorm
type: posts
date: 2022-04-01T12:00:00+08:00
draft: false
categories: ["MSSQL"]
tags: ["MSSQL", "Function", "MSSQL 2008", "MSSQL 2017", "Split string"]
keywords: ["split comma-separated string into rows"]

---


Split comma-separated string into rows.


<!--more-->


I used these key words searching it from google.

	sql separate string by comma into rows

Find first [stackoverflow](https://stackoverflow.com/questions/5493510/turning-a-comma-separated-string-into-individual-rows)

There are two ways to accomplish the job.

One method is MSSQL 2016 before, CTE recursion method

One method is MSSQL 2016 after, there's a build-in function, STRING_SPLIT


And [performance](http://sqlperformance.com/2016/03/t-sql-queries/string-split) comparison.


Conclusion, if you are using MSSQL 2016 later version, just use the STRING_SPLIT.


## CTE recursion method to split strings


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
		  
			-- CTE recursion to split name column
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

	-- normally CTE recursion is limited to 100. If you know you have very long strings, uncomment the option

	/*
	-- for someone who don't understand the CHARINDEX, LEFT, STUFF function work, you can uncomment this block

		SELECT ID
			 , input_date
			 , CHARINDEX(@split_character, name + @split_character) - 1
			 , CAST ( LEFT(name, CHARINDEX(@split_character, name + @split_character) - 1) AS VARCHAR(200))	-- finding the first @split_character and using LEFT to take out the "first string"
			 , CHARINDEX(@split_character, name + @split_character)											  
			 , STUFF(name, 1, CHARINDEX(@split_character, name + @split_character), '')						-- using STUFF to remove the "first string" with @split_character
			 , name
		  FROM @temp_origin
	*/


## STRING_SPLIT method to split strings


If you are using the version after MSSQL 2016, you can just using the STRING_SPLIT function

	SELECT ID
		 , input_date
		 , cs.Value --SplitData
	  FROM @temp_origin
	 CROSS APPLY STRING_SPLIT (name, @split_character) cs
	  


[Here's the testing result](https://dbfiddle.uk/?rdbms=sqlserver_2017&fiddle=f9f488d3f8e9dd1ffd214fe07e92cad3)
	


