---
title: "Information Schema Columns in SQL Server"
author: "Adam Heinz"
date: 2022-05-03T01:00:00-00:00
categories:
  - MSSQL
---
When lost in data space, it can be handy to find a column in a hurry from the information schema. 


# Overview

Most databases will have an id column somewhere, so let's use that as an example. 

Let's begin by creating a demonstation table in our test database: 

```tsql 
USE [test] ; 
GO 

IF NOT EXISTS ( 
	SELECT 1 
	FROM [sys].[objects] as "o"  
	WHERE "o".[object_id] = OBJECT_ID(N'[dbo].[testable]') 
	AND "o".[type] in (N'U') 
)
BEGIN 

	CREATE TABLE [dbo].[testable] ( 	 
		[testable_id] [int] identity(-2147483648 ,1 ) not null, 
		[testable_name] [varchar](50) not null, 
		CONSTRAINT [dbo_testable_pk] PRIMARY KEY CLUSTERED ( [testable_id] ) 
	) ; 

END ; 

GO 
```

We can now search for any column with 'id' in its name: 

```tsql 
USE [test] ; 
GO 

DECLARE @column_search nvarchar(128) = 'id' ; 
SET @column_search = CHAR(37) + @column_search + CHAR(37) ; 
PRINT @column_search ; 

SELECT TOP 1000  
 isc_catalog_name = "isc".[TABLE_CATALOG]
,isc_schema_name = "isc".[TABLE_SCHEMA]
,isc_table_name = "isc".[TABLE_NAME] 
,isc_column_name = "isc".[COLUMN_NAME] 
,isc_data_type = "isc".[DATA_TYPE] 
,isc_character_maximum_length = "isc".[CHARACTER_MAXIMUM_LENGTH] 
,isc_numeric_precision = "isc".[NUMERIC_PRECISION]   
FROM [INFORMATION_SCHEMA].[COLUMNS] as "isc" 
WHERE "isc".[COLUMN_NAME] like @column_search ; 

GO 
``` 

If we inspect the results grid we can see our example: 

| isc_catalog_name	| isc_schema_name	| isc_table_name	| isc_column_name	| isc_data_type	| isc_character_maximum_length	| isc_numeric_precision |
| ----	| ---	| --------	| -----------	| ---	| ---	| --- |
| test	| dbo	| testable	| testable_id	| int	| NULL	| 10 |

We finish by cleaning out this example: 

```tsql 
USE [test] ; 
GO 

DROP TABLE IF EXISTS [dbo].[testable] ; 

GO 
```
QED 
