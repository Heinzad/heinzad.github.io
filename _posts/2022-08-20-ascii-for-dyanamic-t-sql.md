Aide Memoire

Using ASCII Symbols in T-SQL 
============================ 

Dynamic sql can be tricky at the best of times. Using [ASCII](https://www.asciitable.com) symbols can help make it clear what you are looking at. 

```sql 

    SELECT 
     "period" = CHAR(46) 
    ,"comma" = CHAR(44) 
    ,"space" = CHAR(32)
    ,"double_quote" = CHAR(34) 
    ,"single_qote" = CHAR(39) 
    ,"backtick" = CHAR(96) 
    ,"pipe" = CHAR(124) 

``` 

We can use this dynamically for constructing a table identifier, for example: 

```sql 

    DECLARE 
     @table_catalog nvarchar(128) = N'testdb' 
    ,@table_schema nvarchar(128) = N'testing' 
    ,@table_name nvarchar(128) = N'testable' 
    ; 

    DECLARE @entity_name nvarchar(512) = (
        QUOTENAME(@table_catalog) + CHAR(46) + 
        QUOTENAME(@table_schema) + CHAR(46) + 
        QUOTENAME(@table_name) 
    ) 
    ; 

    PRINT(@entity_name) ; 

``` 

This approach is quite useful when trying to construct dynamic sql to a linked server: 

```sql 

    DECLARE 
     @sql_string nvarchar(4000) = N'' 
    ,@lnk_server nvarchar(128) = N'test_server' 
    ,@var_code nvarchar(4) = N'test' 
    ; 

    SET @sql_string = N' 
        DROP TABLE IF EXISTS ##test_results ; 
        
        SELECT "qry".* 
        INTO ##test_results 
        FROM OPENQUERY ( 
            ' + QUOTENAME(@lnk_server) + CHAR(44) + CHAR(32) + ' 
            ' + CHAR(39) + ' 
                SELECT TOP 10 "tbl".* 
                FROM ' + @entity_name + ' as "tbl" 
                WHERE "tbl".[test_code] = ' + CHAR(39) + CHAR(39) + @var_code + CHAR(39) + CHAR(39) + ' ;  
            ' + CHAR(39) + ' 
        ) as "qry" ; 
    ' 
    ; 

    PRINT(@sql_string) ; 
    
    -- EXEC (@sql_string) ; /* only activate when ready */ 

``` 

This will generate the following sql ready for use: 

```sql 

DROP TABLE IF EXISTS ##test_results ; 
        
SELECT "qry".* 
INTO ##test_results 
FROM OPENQUERY ( 
    [test_server], 
    '
        SELECT TOP 10 "tbl".* 
        FROM [testdb].[testing].[testable] as "tbl" 
        WHERE "tbl".[test_code] = ''test'' ; 
    ' 
) as "qry ; 

``` 


QED 
