---
title: "T-SQL: Display Pipeline Progress"
author: "Adam Heinz"
date: 2024-04-08T01:00:00-00:00
categories:
  - MSSQL
---
How to display the progress of a pipeline in MS SQL Server.  


# Overview

When iterating through a sequence of steps you may need to monitor the progress through the pipeline in a manner that can be printed or logged. 

If a function is not appropriate, then executing a prepared sql statement may suffice. 

## Setup 
Configure sql statements and parameters: 

```tsql 

    DECLARE 
        /* Increment and print a given step number together with a given step name */ 
         @sql_step_params nvarchar(4000) = N'@step_no int OUTPUT, @step_name nvarchar(128), @step_rowcount int' 
        ,@sql_step_string nvarchar(4000) = N'SET @step_no += 1; PRINT( RIGHT(''000'' + CONVERT(varchar(15),@step_no), 3) + CHAR(32) + @step_name + CHAR(58) + CHAR(32) + CONVERT(varchar(15),@step_rowcount) )' 
        ; 

```

## Usage 
Use the dynamic sql: 

```tsql 

    DECLARE 
        /* initialise the variables */ 
         @p_step_no int = 0
        ,@p_step_name nvarchar(128) = N'' 
        ,@p_step_rowcount int = 0 
        ; 


    SET @p_step_name = N'This is a demonstration'; 
    SET @p_step_rowcount = 0; 

        SELECT 1 as demo1 into #tbl1 
        SET @p_step_rowcount = @@ROWCOUNT ; 

    EXEC sp_executesql @sql_step_string, @sql_step_params, 
        @step_no = @p_step_no, 
        @step_name = @p_step_name, 
        @step_rowcount = @p_step_rowcount 


```


QED 

© Adam Heinz 

19 September 2023 

