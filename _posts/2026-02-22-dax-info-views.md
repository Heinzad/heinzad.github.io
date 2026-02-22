---
title: "Power BI Data Dictionary"
author: "Adam Heinz"
date: 2026-02-22T01:00:00-00:00
categories:
  - Power BI
---
How to create a data dictionary for a semantic model in Power BI.  


# Overview

Developers can use the desktop designer to add descriptions to tables, columns, relationships and measures. We can store that metadata in the semantic model itself, so that it can be displayed as a data dictionary report. 


# Power BI Info functions

Power BI now provides a set of information schema functions that were previously only available as Dynamic Management Views (DMV) in SQL Server Analysis Services (SSAS).  

Four of them can be stored and displayed for users to automatically document the current state of the semantic model at every refresh.  

For each view, use the following DAX expression to create a calculated table. Display the data dictionary using the table visual in a report.  

This information can also be queried via the Power BI REST API.


## Tables

Materialise the table id, name, description, and other metadata.

```
INFO_TABLES = INFO.VIEW.TABLES()
```


## Columns

Materialise the column id, name, description, data type, and other metadata.

```
INFO_COLUMNS = INFO.VIEW.COLUMNS()
```


## Relationships

Materialise the relationship id, name, description, both table- and column-level sources and sinks, and other metadata. 

```
INFO_RELATIONSHIPS = INFO.VIEW.RELATIONSHIPS()
```

## Measures

Materialise the measure id, name, description, base table, data type, expression, and other metadata. 

```
INFO_MEASURES = INFO.VIEW.MEASURES()
```


# Further Reading

Microsoft

- Learn Data Analysis Expressions (DAX) (9 Jan 2026). ["INFO functions Overview"](https://learn.microsoft.com/en-us/dax/info-functions-dax). 

- Learn Power Platform Power BI (6 Feb 2024). ["Dynamic Management Views (DMVs)"](https://learn.microsoft.com/en-us/analysis-services/instances/use-dynamic-management-views-dmvs-to-monitor-analysis-services?view=sql-analysis-services-2025).

- Learn Power BI REST APIs. ["Datasets - Execute Queries"](https://learn.microsoft.com/en-us/rest/api/power-bi/datasets/execute-queries).  

MSSQL Tips

- Sean Lee (17 Feb 2025). Power BI. ["INFO.VIEW DAX Functions Usage and Examples"](https://www.mssqltips.com/sqlservertip/8185/info-view-dax-functions-usage-and-examples/). 

Radacad

- Reza Rad (12 Jun 2025). ["Power BI Model Analysis using DAX INFO Functions"](https://radacad.com/power-bi-model-analysis-using-dax-info-functions/).

Lazy Snail

- Power BI. ["Documenting Power BI Semantic Model sources Using DAX And AI"](https://www.lazysnail.net/power-bi/documenting-power-bi-semantic-model-sources-dax-ai).

QED 

© Adam Heinz 

22 February 2026 
