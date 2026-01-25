---
layout: post
tags: MSSQL
title: "ColumnID vs Ordinal Position in Sql Server"
author: "Adam Heinz"
date: 2023-04-23 01:00:00
---
How to get SQL Server properties that are not in the information schema.  


# Overview

There are times when you need to extend the information schema columns. 
One example is obtaining the column id, as opposed to getting the ordinal position. 

We can get the column id by using the COLUMNPROPERTY function. This takes three arguments: the object id, the column name, and the property wanted. 

For example, we can demonstrate that the column id is a permanent reference, while the ordinal position can change after DDL operations. 


## 1 - Build the test table: 

```tsql 

CREATE TABLE test.testable ( 
    testable_seqn int identity not null, 
    testable_code varchar(50) not null, 
    testable_name varchar(50) not null, 
    testable_desc varchar(255) null 
) ; 
GO 

``` 

While the table is in a pristine state, if we query the information schema columns, we will find the Column ID and Ordinal Position are identical. 

## 2 - Query Column Info: 

```tsql 

SELECT 

    isc.TABLE_CATALOG 
    ,isc.TABLE_SCHEMA 
    ,isc.TABLE_NAME 
    ,isc.COLUMN_NAME 
    ,isc.ORDINAL_POSITION 
    ,COLUMN_ID = COLUMNPROPERTY(
        OBJECT_ID(isc.TABLE_SCHEMA + CHAR(46) + isc.TABLE_NAME) 
        ,isc.COLUMN_NAME 
        ,'ColumnID'  
    ) 

FROM INFORMATION_SCHEMA.COLUMNS as isc 

WHERE isc.TABLE_SCHEMA = 'test' 
    AND isc.TABLE_NAME = 'testable' 

ORDER BY isc.ORDINAL_POSITION 
; 
GO 

``` 

## 3  Inspect Results: 

| TABLE_CATALOG | TABLE_SCHEMA | TABLE_NAME | COLUMN_NAME | ORDINAL_POSITION | COLUMN_ID | 
| ------------- | ------------ | ---------- | ----------- | ---------------- | --------- |
| DataHub | test | testable | testable_seqn | 1 | 1 | 
| DataHub | test | testable | testable_code | 2 | 2 | 
| DataHub | test | testable | testable_name | 3 | 3 | 
| DataHub | test | testable | testable_desc | 4 | 4 | 


## 4 - Remove a column: 

```tsql 

ALTER TABLE test.testable 
DROP COLUMN testable_code 
; 
GO 

``` 

## 5 - Re-query column info: 


```tsql 

SELECT 

    isc.TABLE_CATALOG 
    ,isc.TABLE_SCHEMA 
    ,isc.TABLE_NAME 
    ,isc.COLUMN_NAME 
    ,isc.ORDINAL_POSITION 
    ,COLUMN_ID = COLUMNPROPERTY(
        OBJECT_ID(isc.TABLE_SCHEMA + CHAR(46) + isc.TABLE_NAME) 
        ,isc.COLUMN_NAME 
        ,'ColumnID'  
    )

FROM INFORMATION_SCHEMA.COLUMNS as isc 

WHERE isc.TABLE_SCHEMA = 'test' 
    AND isc.TABLE_NAME = 'testable' 

ORDER BY isc.ORDINAL_POSITION 
; 
GO 

``` 

## 6 - Inspect Results:  

| TABLE_CATALOG | TABLE_SCHEMA | TABLE_NAME | COLUMN_NAME | ORDINAL_POSITION | COLUMN_ID | 
| ------------- | ------------ | ---------- | ----------- | ---------------- | --------- | 
| DataHub | test | testable | testable_seqn | 1 | 1 | 
| DataHub | test | testable | testable_name | 2 | 3 | 
| DataHub | test | testable | testable_desc | 3 | 4 | 


In short, the column id remained fixed, while the ordinal position updated to reflect the changing table structure. 


# The trick in the question 

In SQL Server, the information schema views are derived from the system views. 

Examine the object definition of information schema columns: 

```tsql 

SELECT OBJECT_DEFINITION( OBJECT_ID('INFORMATION_SCHEMA.COLUMNS') ) 

``` 

Inspect the definition of ordinal position: 

```tsql 

SELECT 

    COLUMNPROPERTY(c.object_id, c.name, 'ordinal')  AS ORDINAL_POSITION 

FROM sys.objects o 

    JOIN sys.columns c 
    ON c.object_id = o.object_id   

    LEFT JOIN sys.types t 
    ON c.user_type_id = t.user_type_id  

WHERE o.type IN ('U', 'V')  
; 

``` 

Both ordinal position and column id are derived from applying the column property function to the sys columns table. 


QED 

© Adam Heinz 

22 April 2023

