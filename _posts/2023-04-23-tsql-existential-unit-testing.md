Aide Memoire 

How To Perform Basic Unit Testing In MS SQL Server 
=================================================== 

When using migration scripts for deployments, it is essential to first test if an object exists before attempting to create it. If the test fails, we can then run the script to create the object. Afterwards, we need to run the same test again to verify the action succeeded. 

If we encapsulate the logic in functions we have something we can repeat ad nauseum. 

## Test if a database exists before attempting to create it: 

```tsql 

/* test fails if database does not exist  */ 

DECLARE @test_database nvarchar(128) = 'testdb' 

DECLARE @assert bit = 0 ; 
IF EXISTS ( 
    SELECT 1 
    FROM [sys].[databases] as d 
    WHERE QUOTENAME(d.[name]) = QUOTENAME(@test_database) 
) 
SET @assert = 1

``` 



## Test if a schema exists before we attempt to create it: 

```tsql 

/* test fails if schema does not exist */ 

DECLARE @test_schema nvarchar(128) = 'utest' 

DECLARE @assert bit = 0 ; 
IF EXISTS ( 
    SELECT 1  
    FROM [sys].[schemas] as s 
    WHERE QUOTENAME(s.[name]) = QUOTENAME(@test_schema) 
) 
SET @assert = 1

``` 


## Test if a table exists before attempting to create or modify it:  

```tsql 

/* test fails if table does not exist */ 

DECLARE @test_schema nvarchar(128) = 'utest'
DECLARE @test_table nvarchar(128) = 'testable' 

DECLARE @assert bit = 0 ; 
IF EXISTS ( 
    SELECT 1 
    FROM sys.objects o 
    INNER JOIN sys.schemas s   
    ON s.schema_id = o.schema_id  
     WHERE o.type IN ('U', 'V') /* U=Table, V=View */ 
    AND QUOTENAME(s.[name]) = QUOTENAME(@test_schema) 
    AND QUOTENAME(o.[name]) = QUOTENAME(@test_table)  
) 
SET @assert = 1 

``` 


## Test if a column exists before attempting to create or modify it: 

```tsql 

/* test fails if column does not exist */ 

DECLARE @test_schema nvarchar(128) = 'utest'
DECLARE @test_table nvarchar(128) = 'testable' 
DECLARE @test_column nvarchar(128) = 'testator'

DECLARE @assert bit = 0 ; 
IF EXISTS ( 
    SELECT 1 
    FROM sys.objects o 
    INNER JOIN sys.schemas s   
    ON s.schema_id = o.schema_id 
    INNER JOIN sys.columns c 
    ON c.object_id = o.object_id   
    INNER JOIN sys.types t 
    ON c.user_type_id = t.user_type_id  
    WHERE o.type IN ('U', 'V') /* U=Table, V=View */ 
    AND QUOTENAME(s.[name]) = QUOTENAME(@test_schema) 
    AND QUOTENAME(o.[name]) = QUOTENAME(@test_table)  
    AND QUOTENAME(c.[name]) = QUOTENAME(@test_column) 
) 
SET @assert = 1  

``` 

Notes: 
- These queries are derived from the object definitions of the applicable information schema views. 
- The parameters are wrapped in quotenames to prevent sql injection. 


QED 

© Adam Heinz 

22 April 2023
