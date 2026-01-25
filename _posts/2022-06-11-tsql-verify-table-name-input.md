AIDE MEMOIRE 

How to verify a table name in tsql 
==================================== 
*Verify input values against metadata in SQL Server* 

When receiving input from a stored procedure, for security's sake it is a good thing to verify the inputs. 

Sometimes the inputs may be table names - particularly if the proc is generating dynamic sql. 
Other times, schema drift may mean that a proc was left calling a table that no longer exists.  

## Example 
Take an imaginary table "test"."testable" for example. 
If we try to query sys objects we will not get a particularly useful answer: 

```tsql 
SELECT is_valid = OBJECT_ID( '[test].[testable]') 
``` 

|     | is_valid | 
| --- | ---------------- | 
| 1 | *NULL* | 

Personally, I don't find a null result particularly helpful. 

More importantly, we haven't yet addressed a security consideration in verifying any input before it is used. 

That is, we need to escape or quote the input that is being verified. 

We can do that in a simple function. 

### Verify Table Name 
Create a function to validate input table names: 

```tsql 
CREATE OR ALTER FUNCTION dbo.verify_table_name ( 
     @p_schema_name nvarchar(128)
    ,@p_table_name nvarchar(128) 
) 
RETURNS BIT AS 
BEGIN 

    DECLARE 
     @v_schema_name nvarchar(130) =  QUOTENAME( @p_schema_name ) 
    ,@v_table_name nvarchar(130) =  QUOTENAME( @p_table_name )
    ,@is_valid bit = 0 
    ; 
    
    IF EXISTS ( 
        SELECT 1 
        FROM [INFORMATION_SCHEMA].[TABLES] as "ist" 
        WHERE QUOTENAME( "ist".[TABLE_SCHEMA] ) = @v_schema_name 
        AND QUOTENAME( "ist".[TABLE_NAME] ) = @v_table_name 
    )
    SET @is_valid = 1 ; 

    RETURN @is_valid ; 
END 
; 
GO 
``` 

In this function we have set some input parameters to the maximum length for schema and table names. 

Inside the function we have declared some internal variables to prevent parameter sniffing issues. 

The input variables are two digits longer than the input parameters because we immediately wrap the input in QUOTENAME() so that we are never dealing with the input directly. 

We also set the return value is_valid to zero. 

The function then queries the information schema table metadata against the quoted schema and table names. 

Lastly, the function changes the "is_valid" return value to 1 if the quoted inputs are identical to the quoted table metadata. 

### Example Usage: 
First, check if we can handle any net nasties: 

```tsql 
SELECT is_valid = dbo.verify_table_name( 'test', 'SELECT * FROM sys.databases')
``` 

|     | is_valid | 
| --- | ---------------- | 
| 1 | 0 | 

### Usage in Procedures 
This function is most useful in stopping a procedure before it digs itself into a hole. 

We can build a demonstration procedure for example, where we abandon the process if the target table does not exist. 

```tsql 
CREATE OR ALTER PROC #demo ( 
     @p_target_schema nvarchar(128)   
    ,@p_target_table nvarchar(128) 
) AS 
--<< process >>-- 
BEGIN 

    ------------------------------------ 
    SETTINGS: 

    SET NOCOUNT ON ; 
    SET XACT_ABORT ON ; 

    DECLARE 
     @v_target_schema nvarchar(128) = @p_target_schema   
    ,@v_target_table nvarchar(128) = @p_target_table 
    ; 

    DECLARE @do_proceed bit = 0 ; 
	DECLARE @txt_message nvarchar(500) = N'' ; 
    DECLARE @sql_string nvarchar(4000) = N'' ; 

    ------------------------------------ 
    VERIFICATION: 

    SET @do_proceed = dbo.verify_table_name( @v_target_schema, @v_target_table ) ; 

    IF ( @do_proceed != 1 ) 
    BEGIN 
        SET @txt_message = 'error: invalid object name ' + QUOTENAME(@v_target_schema) + CHAR(46) + QUOTENAME(@v_target_table) ; 
		RAISERROR( @txt_message, 0, 1) ; 
        RETURN 
    END 
    ELSE 
    --<< main body >>-- 
    BEGIN 

        SET @sql_string = N' SELECT TOP (1000) * FROM ' + QUOTENAME(@v_target_schema) + CHAR(46) + QUOTENAME(@v_target_table) + CHAR(32) + CHAR(59) ; 

        BEGIN TRY 
            EXEC sp_executesql @sql_string ; 
        END TRY 
        BEGIN CATCH 
            PRINT @sql_string ; 
            RETURN ; 
        END CATCH 
        ; 

    END 
    --<< main body >>-- 

END 
--<< process >>-- 
; 
GO 
``` 

### Example Usage 
Let's now run this proc against our imaginary table: 

```tsql 
EXEC #demo 
@p_target_schema = 'test'    
,@p_target_table = 'testable' 
; 
``` 

We get a message rather than results: 

```tsql 
error: invalid object name [test].[testable] 
``` 

### Demonstration table 
Having demonstrated this proc's operation against a non-existent table input, let's now create and populate a demonstration table to put the proc through its paces: 

```tsql 
CREATE TABLE dbo.demo ( 
 demo_id int identity not null constraint dbo_demo_pk primary key 
,demo_txt varchar(500) 
) ; 

INSERT INTO dbo.demo (demo_txt) 
VALUES ('Abandon hope all ye who enter here') ; 
``` 

Now if we re-run that proc against a real table we may now actually get a result: 

```tsql 
EXEC #demo 
 @p_target_schema = 'dbo'    
,@p_target_table = 'demo' 
; 
``` 

Results: 
|     | demo_id | demo_txt | 
| ------ | -------- | -------- | 
| 1 | 1 | Abandon hope all ye who enter here | 

### Clean Up 

Lastly, let's clean up our working environment before departing: 

```tsql 
DROP PROC IF EXISTS #demo ; 
DROP TABLE IF EXISTS [dbo].[demo] ; 
GO 
``` 

QED 