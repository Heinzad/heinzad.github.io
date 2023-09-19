AIDE MEMOIRE

# How to monitor the progress of a pipeline in MS SQL Server 

When iterating through a sequence of steps you may need to monitor the progress through the pipeline in a manner that can be printed or logged. 

If a function is not appropriate, then executing a prepared sql statement may suffice. 

## Setup 
Configure sql statements and parameters: 

```tsql 

    DECLARE 
        /* Increment and print a given step number together with a given step name */ 
        @sql_step_params nvarchar(4000) = N'@step_no int OUTPUT, @step_name nvarchar(128), @step_rowcount int' 
        @sql_step_string nvarchar(4000) = N'SET @step_no += 1; PRINT( RIGHT(''000'' + CONVERT(varchar(15),@step_no)) + CHAR(32) + @step_name + CHAR(58) + CHAR(32) + CONVERT(varchar(15),@step_rowcount) )' 
        ; 

```

## Usage 
Use the dynamic sql: 

```tsql 

    DECLARE 
        /* initialise the variables */ 
         v_step_no int = 0
        ,@v_step_name nvarchar(128) = N'' 
        ,@v_step_counter int = 0 
        ; 


    SET @v_step_name = N'This is a demonstration'; 
    SET @v_step_rowcount = 0; 

        SELECT 1 as demo1 into #tbl1 
        SET @v_step_rowcount = @@ROWCOUNT() ; 

    EXEC sp_executesql @sql_step_string, @sql_step_params, 
        @step_no = @v_step_no, 
        @step_name = @v_step_name, 
        @step_rowcount = @v_step_rowcount 


```


QED 

© Adam Heinz 

19 September 2023 

