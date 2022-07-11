Aide Memoire 

Reseed an Identity Column with TSQL 
=================================== 

## Background 
After switching partitions into a new table, error messages started occurring because the identity column was trying to add new values starting from 1, which was colliding with the switched-in values that went millions of values higher. 

What was needed was to reseed the identity column one value higher than the top value in the column. 

The Microsoft documentation shows how to [force the current identity value to a new value] (https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-checkident-transact-sql?view=sql-server-ver16#c-forcing-the-current-identity-value-to-a-new-value) .

This example shows how to do it repeatedly with dynamic tsql: 

```tsql 

DECLARE @v_table_catalog nvarchar(128) = 'testDB'; 
DECLARE @v_table_schema nvarchar(128) = 'dbo'; 
DECLARE @v_table_name nvarchar(128) = 'testable'; 
DECLARE @v_column_name nvarchar(128) = 'id'; 

DECLARE @v_entity_name nvarchar(512) = QUOTENAME( @v_table_schema ) + CHAR(46) + QUOTENAME( @v_table_name ) ;
DECLARE @v_id_value nvarchar(50) = N'' ; 

DECLARE @sql_string_01 nvarchar(max) = N'' ;  
DECLARE @sql_string_02 nvarchar(max) = N'' ; 

DECLARE @sql_params nvarchar(4000) = N'' ; 
SET @sql_params = N' 
@out_id_value nvarchar(50)  
' 


IF EXISTS ( 
    SELECT 1 
    FROM INFORMATION_SCHEMA.COLUMNS isc 
    JOIN INFORMATION_SCHEMA.TABLES ist 
    ON isc.TABLE_CATALOG = ist.TABLE_CATALOG 
    AND isc.TABLE_SCHEMA = ist.TABLE_SCHEMA 
    AND isc.TABLE_NAME = ist.TABLE_NAME 
    WHERE QUOTENAME ist.TABLE_TYPE = 'BASE TABLE' 
    AND QUOTENAME( isc.TABLE_SCHEMA ) = QUOTENAME( @v_table_schema )  
    AND QUOTENAME( isc.TABLE_NAME ) = QUOTENAME( @v_table_name )  
    AND QUOTENAME( isc.COLUMN_NAME ) = QUOTENAME( @v_column_name )  
) 
BEGIN 

SET @sql_string_01 = N' 
SET @out_id_value = CONVERT(nvarchar(50),(
    SELECT MAX( ' + QUOTENAME( @v_column_name ) + ' ) + 1  
    FROM ' + @v_entity_name + ' 
))  
' ;  

EXEC sp_executesql @sql_string_01 , @sql_params, @out_id_value = @v_id_value ; 

PRINT @v_id_value ; 

SET @sql_string_02 = N' 
DBCC CHECKIDENT ( 
        ' + CHAR(39) + @v_entity_name + CHAR(39) + ' 
        , RESEED 
        , ' + @v_id_value + ' 
)  
' ; 

EXEC sp_executesql @sql_string_02 ; 

END ; 

``` 


QED 