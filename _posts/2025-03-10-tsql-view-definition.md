---
layout: post
tags: MSSQL
title: "T-SQL View Definition"
author: "Adam Heinz"
date: 2025-03-10 10:00:00
---
How to get a View Definition in SQL Server.  


# Overview

In SQL Server, information schema views has a "view definition" column that is capped at 4000 characters. Here's how to overide that limit and get the full object definition: 

```sql 

    SELECT 
        v.TABLE_CATALOG, 
        v.TABLE_SCHEMA,
        v.TABLE_NAME,
        OBJECT_DEFINITION(OBJECT_ID(v.TABLE_SCHEMA +'.'+ v.TABLE_NAME)) AS VIEW_DEFINITION, 
        v.CHECK_OPTION, 
        v.IS_UPDATEABLE 
    FROM INFORMATION_SCHEMA.VIEWS as v
    ; 

```

QED 

© Adam Heinz 
11 March 2025