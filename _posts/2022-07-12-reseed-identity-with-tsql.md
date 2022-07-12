Aide Memoire 

Reseed an Identity Column with TSQL 
=================================== 

## Background 
After switching partitions into a new table, error messages started occurring because the identity column was trying to add new values starting from 1, which was colliding with the switched-in values that went millions of values higher. 

What was needed was to reseed the identity column one value higher than the top value in the column. 

The Microsoft documentation shows how to [force the current identity value to a new value] (https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-checkident-transact-sql?view=sql-server-ver16#c-forcing-the-current-identity-value-to-a-new-value) .

This example shows how to do it repeatedly with dynamic tsql. There are several steps we need to work through: 
- Verify that the column exists on the target table. 
- Verify that the column in question is in fact an identity column before attempting to reseed it.
- Add 1 to the largest existing id value. 
- If the table is empty, default to the minimum value for the id column's data type (i.e. int or bigint). 
 

```tsql 

INPUT_PARAMETERS: 

DECLARE @p_table_catalog nvarchar(128) = DBNAME() ; 
DECLARE @p_table_schema nvarchar(128) = 'dbo'; 
DECLARE @p_table_name nvarchar(128) = 'testable'; 
DECLARE @p_column_name nvarchar(128) = 'id'; 


--<< process >>-- 
BEGIN 

SETTINGS: 

SET NOCOUNT ON ; 
SET XACT_ABORT ON ; 

DECLARE @v_table_catalog nvarchar(128) = QUOTENAME( @p_table_catalog ) ; 
DECLARE @v_table_schema nvarchar(128) = QUOTENAME( @p_table_schema ) ; 
DECLARE @v_table_name nvarchar(128) = QUOTENAME( @p_table_name ) ; 
DECLARE @v_column_name nvarchar(128) = QUOTENAME( @p_column_name ) ; 

DECLARE @min_bigint nvarchar(50) = N'-9223372036854775808' ; 
DECLARE @min_integer nvarchar(50) = N'-2147483648' ; 

DECLARE @tgt_entity_name nvarchar(512) 
DECLARE @tgt_column_name nvarchar(128) ; 
DECLARE @tgt_id_type nvarchar(50) = N'' ; 
DECLARE @tgt_id_flag bit ; 
DECLARE @tgt_id_value nvarchar(50) = N'' ; 
DECLARE @tgt_id_default nvarchar(50) = N'0' ;

DECLARE @sql_string_01 nvarchar(max) = N'' ;  
DECLARE @sql_string_02 nvarchar(max) = N'' ; 

DECLARE @sql_params nvarchar(4000) = N'' ; 
SET @sql_params = N' 
@out_id_value nvarchar(50) OUTPUT  
' 

VERIFICATION: 

SELECT TOP 1 
 @tgt_entity_name =  QUOTENAME(isc.TABLE_SCHEMA) + CHAR(46) + QUOTENAME( isc.TABLE_NAME ) 
,@tgt_column_name = QUOTENAME( isc.COLUMN_NAME ) 
,@tgt_id_type = isc.DATA_TYPE  
FROM INFORMATION_SCHEMA.COLUMNS isc 
JOIN INFORMATION_SCHEMA.TABLES ist 
ON isc.TABLE_CATALOG = ist.TABLE_CATALOG 
AND isc.TABLE_SCHEMA = ist.TABLE_SCHEMA 
AND isc.TABLE_NAME = ist.TABLE_NAME 
WHERE ist.TABLE_TYPE = 'BASE TABLE' 
AND QUOTENAME( isc.TABLE_SCHEMA ) = @v_table_schema  
AND QUOTENAME( isc.TABLE_NAME ) = @v_table_name 
AND QUOTENAME( isc.COLUMN_NAME ) = @v_column_name   
; 

IF( @tgt_id_type = 'bigint' ) 
SET @tgt_id_default = @min_bigint ; 

IF( @tgt_id_type IN ('int', 'integer') ) 
SET @tgt_id_default = @min_integer ;

SET @tgt_id_flag = COLUMNPROPERTY (
     OBJECT_ID( @tgt_entity_name ) 
    ,@tgt_column_name 
    ,'IsIdentity' 
)

MAIN_QUERY: 

--<< main >>-- 
IF ( @tgt_id_flag != 1 ) 
RETURN 
ELSE  
BEGIN 

SET @sql_string_01 = N' 
SET @out_id_value = CONVERT(nvarchar(50),(
    SELECT COALESCE( MAX( ' + @v_column_name + ' ), ' + @tgt_id_default + ' ) + 1  
    FROM ' + @tgt_entity_name + ' 
))  
' ;  

PRINT @sql_string_01 ; 

EXEC sp_executesql @sql_string_01 , @sql_params, @out_id_value = @tgt_id_value OUTPUT ;  

SET @sql_string_02 = N' 
DBCC CHECKIDENT ( 
        ' + CHAR(39) + @tgt_entity_name + CHAR(39) + ' 
        , RESEED 
        , ' + @tgt_id_value + ' 
)  
' ; 

PRINT @sql_string_02 ; 

EXEC sp_executesql @sql_string_02 ; 

END ; 
--<< main >>-- 

END ; 
--<< process >>-- 

GO 

``` 


QED 