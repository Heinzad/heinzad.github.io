---
title: "Get a Table Definition in SQL Server"
author: "Adam Heinz"
date: 2022-12-22T01:00:00-00:00
categories:
  - MSSQL
---
How to use a user-defined table type to create a table that can store the output of a query.  


# Overview

Object definitions are so handy for views and routines that everybody wants them for tables as well. 

It can be done with a little tsql thanks to Filip Popović's answer to the question [How to select from or store result of sp_describe_first_result_set](https://stackoverflow.com/questions/66833137/how-to-select-from-or-store-result-of-sp-describe-first-result-set) on StackOverflow. 


A user-defined table type is handy for just this sort of thing: 

```tsql 

CREATE TYPE [dbo].[tt_describe_first_result_set] as TABLE (
    [is_hidden] bit NOT NULL, 
    [column_ordinal] int NOT NULL, 
    [name] sysname NULL, 
    [is_nullable] bit NOT NULL, 
    [system_type_id] int NOT NULL, 
    [system_type_name] nvarchar(256) NULL, 
    [max_length] smallint NOT NULL, 
    [precision] tinyint NOT NULL, 
    [scale] tinyint NOT NULL, 
    [collation_name] sysname NULL, 
    [user_type_id] int NULL, 
    [user_type_database] sysname NULL, 
    [user_type_schema] sysname NULL, 
    [user_type_name] sysname NULL, 
    [assembly_qualified_type_name] nvarchar(4000), 
    [xml_collection_id] int NULL, 
    [xml_collection_database] sysname NULL, 
    [xml_collection_schema] sysname NULL, 
    [xml_collection_name] sysname NULL, 
    [is_xml_document] bit NOT NULL, 
    [is_case_sensitive] bit NOT NULL, 
    [is_fixed_length_clr_type] bit NOT NULL, 
    [source_server] nvarchar(128), 
    [source_database] nvarchar(128), 
    [source_schema] nvarchar(128), 
    [source_table] nvarchar(128), 
    [source_column] nvarchar(128), 
    [is_identity_column] bit NULL, 
    [is_part_of_unique_key] bit NULL, 
    [is_updateable] bit NULL, 
    [is_computed_column] bit NULL, 
    [is_sparse_column_set] bit NULL, 
    [ordinal_in_order_by_list] smallint NULL, 
    [order_by_list_length] smallint NULL, 
    [order_by_is_descending] smallint NULL, 
    [tds_type_id] int NOT NULL, 
    [tds_length] int NOT NULL, 
    [tds_collation_id] int NULL, 
    [tds_collation_sort_id] tinyint NULL 
        
) ; 
``` 

We can now stuff this into a table definition: 

```tsql 

GET_DESCRIPTION: 

DECLARE @tt_describe_first_result_set [dbo].[tt_describe_first_result_set] ; 

INSERT INTO @tt_describe_first_result_set 
EXEC sp_describe_first_result_set 
@tsql = N'SELECT * FROM [testing].[testable]' ; 



GET_DEFINITION: 

DECLARE @tbl_defn nvarchar(max) = N''; 

SELECT @tbl_defn = TRY_CONVERT( 
    nvarchar(max), 
    CHAR(32) + LTRIM(RTRIM( 
        STUFF(
            ( 
                SELECT TOP 100 PERCENT 
                CHAR(44) + QUOTENAME([name]) + CHAR(32) + [system_type_name] + CHAR(32) 
                + CASE WHEN [is_identity_column] = 1 THEN 'IDENTITY' + CHAR(32) ELSE '' END 
                + CASE WHEN [is_nullable] = 1 THEN 'NULL' + CHAR(32) ELSE 'NOT NULL' + CHAR(32) END 
                + CHAR(10) /* new line for printable layout */ 
                FROM @tt_describe_first_result_set 
                ORDER BY [column_ordinal] 
                FOR XML PATH(''), type 
            )
            .value('.', 'nvarchar(max)') 
            ,1,1,'' 
        ) 
    )) 
) ; 

PRINT( @tbl_defn ) 

``` 


QED 

© Adam Heinz 

12 December 2022 
