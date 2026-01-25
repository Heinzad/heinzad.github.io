---
layout: post
tags: MSSQL
title: "Generate Stored Procedures in SQL Server"
author: "Adam Heinz"
date: 2022-12-17 01:00:00
---
How to automate the creation of stored procedures from models with just a table or two.  


# Overview

Stored procedures have limitations in what they can check and enforce. We can usefully fill that gap by modelling a procedure's targets and sources as schemabound views. Such models ensure that a procedure's pre-requisites continue to exist, and can be inspected and queried as objects in their own right. For the sake of performance we do not want to use the views as such, but their definitions, which can be used as models of common table expressions inline in the body of the procedure, which is much more performant. 

All procedures will have a target model, but the copy operation is the only one that also requires a source model. 

With most of the work being performed in the models, there is a minimal number of operations we need to perform: 

## design patterns: 

| operation | description | detail | 
| copy | record transfer | INSERT INTO "tgt" SELECT "src".* FROM "src" | 
| prune | record deletion | DELETE d FROM "tgt" d | 
| revise | record update | UPDATE "tgt" SET {...} = {...} | 
| empty | table truncation | TRUNCATE TABLE {...} | 




QED 

© Adam Heinz 

17 December 2022 
