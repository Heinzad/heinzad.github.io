---
layout: post
tags: MSSQL
title: "Get a Date Number from a Date in SQL Server"
author: "Adam Heinz"
date: 2022-09-27 01:00:00
---

When working in Business Intelligence or data engineering it is often necessary to use a date key instead of a date value. 

We can do this by [converting date formats](https://www.sqlshack.com/sql-convert-date-functions-and-formats/). 

There are two steps to generating a date key from a date value: 
(1) convert to date format 112 -- YYYYMMDD 
(2) convert to int 

The t-sql is as follows: 

```sql 

/*** convert date value to date number ***/ 

GET_DATE_KEY_FROM_DATE_VAL: 

DECLARE 
  @date_val date 
 ,@date_key int 
 ; 

SET @date_val = CONVERT(date, GETDATE()) ; 
SET @date_key = CONVERT(int, CONVERT(varchar, @date_val, 112)) ; 

SELECT @date_key as date_key ;


``` 

QED 
