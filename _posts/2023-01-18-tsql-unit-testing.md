---
layout: post
tags: MSSQL
title: "Unit Testing in SQL Server"
author: "Adam Heinz"
date: 2023-01-18 01:00:00
---

Unit testing in databases is a notoriously difficult challenge. 

There are several levels involved in testing a user stored procedure (USP). 


## Verifying Object Existence 

One form of unit testing focuses on whether or not an object exists. This form of unit testing will fail before you build the nominated object, and pass after a successful build. 

### Routines 

This test will fail before we build the nominated proc and pass after we have built it.  

```tsql 

DECLARE 
 @assert bit = 'false' 
,@schema nvarchar(128) = 'test' 
,@routine nvarchar(128) = 'testable_usp' 
; 

 IF EXISTS (
    SELECT 1 
    FROM [INFORMATION_SCHEMA].[ROUTINES] as "isr"  
    WHERE QUOTENAME("isr".[ROUTINE_SCHEMA]) = QUOTENAME(@schema)
    AND QUOTENAME("isr".[ROUTINE_NAME]) = QUOTENAME(@routine)  
) 
SET @assert = 'true'; 

PRINT('test result: ' + TRY_CONVERT(varchar, @assert)) ; 

``` 


The usp build process in SQL Server does detect a number of issues, but is not sufficient in itself. 

The testing is also not able to ensure that its dependencies in the sources and targets of the usp still exist in the same form as at the time the usp was created. 

We can address both these concerns by creating the sources and targets as objects in their own right. 


--- 
## Virtual Tables 

Building the sources and targets of a usp as views or virtual tables gives us objects that can be tested in the same way as for routines. Applying schemabinding ensures that the column dependencies cannot be accidentally changed. 



QED 

© Adam Heinz 

18 January 2023
