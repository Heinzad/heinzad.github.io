---
layout: post
tags: Kimball
title: "Slowly Changing Dimensions"
author: "Adam Heinz"
date: 2024-06-16 01:00:00
---
How to apply Slowly Changing Dimensions with the Kimball method.  


# Overview

Dimensional modelling is used in many data warehouse schemas for fast reads by minimising the number of joins required in a reporting query. Most dimensional modelling applies either a star or snowflake schema.

__Star Schemas__ are divided into: 
- facts holding quantitative data, and 
- dimensions holding descriptive attributes

__Snowflake Schemas__ further extend into more normalised relations

__Slowly Changing Dimensions__ capture the history of a dimension through a number of techiques: 

| Type | Description | 
| ---- | ----------- | 
| SCD1 | current values only, overwrite historic values on any change | 
| SCD2 | complete history, each change is a new record | 
| SCD3 | refers to the previous value | 
| SCD4 | separates fast-changing attributes into an associated table | 
| SCD5 | refers to the current value in historical records |
| SCD6 | combines types 1, 2, 3, with a complete history that also references the current value | 


SCD 2 may use a number of tactics: 
- flags to indicate the current record 
- version numbering 
- date ranges with start and end dates

__Temporal tables__ offer similar functionality with a principal table for current values, and a history table for superseded values, with start and end dates for each record. 





QED 

© Adam Heinz 

16 April 2024