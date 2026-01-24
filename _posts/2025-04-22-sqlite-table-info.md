---
layout: post
tags: SQLite
title: "SQLite Table Info"
author: "Adam Heinz"
date: 2025-04-22 10:00:00
---

AIDE MEMOIRE 

How to Get Table Information in SQLite 
====================================== 

SQLite does not have `INFORMATION_SCHEMA.TABLES`, but it does have `table_info` and other alternatives. 

Take the following table creation statement, for example. 

```sql 
    CREATE TABLE example 
    ( 
        example_id INTEGER NOT NULL PRIMARY KEY, 
        example_name TEXT
    );
```

We can obtain information about that table in a number of ways. 


Table Info 
---------- 

Run the `pragma` function to get table info: 

```sql 
    pragma table_info('example'); 
```

The result is: 

| cid | name | type | notnull | dflt_value | pk |
| --- | ---- | ---- | ------- | ---------- | --- |
| 0 | example_id | INTEGER | 1 | | 1 | 
| 1 | example_name | INTEGER | 0 | | 0 | 


Table Schema 
------------ 

Query the `sqlite_schema` to get a list of all tables in the database: 

```sql 
    SELECT name   
    FROM sqlite_schema 
    WHERE type = 'table'
    AND name NOT LIKE 'sqlite_%' -- exclude system tables 
```

Query the `sqlite_schema` to get an individual table creation statement: 

```sql 
    SELECT sql  
    FROM sqlite_schema 
    WHERE type = 'table'
    AND name = 'example';
```

Returns the create table sql statement: 

```sql 
    CREATE TABLE example 
    ( 
        example_id INTEGER NOT NULL PRIMARY KEY, 
        example_name TEXT
    )
```




QED 

© Adam Heinz 

22 April 2025