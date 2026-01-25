---
layout: post
tags: PowerBI
title: "Power Query Data Flows"
author: "Adam Heinz"
date: 2023-08-29 01:00:00
---
AIDE MEMOIRE 

# Data Flows with Power Query m 

At the time of writing, parameters do not work in power query dataflows in the same way as they work in power query for datasets. However, we can introduce some efficiencies to make a dataflow maintainable and deployable. It just requires editing the auto-generated code. 
 

## 0. Initial Import

If we start a new dataflow, connect to a database, import a table, then inspect the advanced editor, we will see the following query in m: 

``` 

let 
    Source = Sql.Database("my_server_connection_string", "database_name_string"), 
    #"Navigation 1" = Source{([Schema = "my_schema_name", Item = "my_table_name"])}
in 
    #"Navigation 1"

``` 

If we load 50 tables, then we will have 50 source connections to update whenever we need to refactor. 

The solution is to:
* split out the source connection as a stand-alone entity, 
* repoint all queries to navigate from that one source connection


## 1. Modify a copy 

Take a copy of the first table, rename it to "my_database_connection", and edit the advanced editor to reduce the m query to the source connection only: 

``` 

let 
    Source = Sql.Database("my_server_connection_string", "database_name_string") 
in 
    Source

``` 

## 2. Repoint dependant queries 

Now, repoint all queries to navigate from the source connection: 

``` 

let 
    Source = my_database_connection, 
    #"Navigation 1" = Source{([Schema = "my_schema_name", Item = "my_table_name"])}
in 
    #"Navigation 1"

``` 

Now we only have one connection to maintain for all the dependent tables.




QED 

© Adam Heinz 

29 August 2023 
