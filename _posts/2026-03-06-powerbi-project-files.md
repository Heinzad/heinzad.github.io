---
title: "Power BI Project Files"
author: "Adam Heinz"
date: 2026-03-06T01:00:00-00:00
categories:
  - Power BI
---
How to use Power BI Project Files with Git.  


# Overview

Power BI Project files can be saved in source control and published in CI/CD pipelines.

*New Power BI File Types:*  
- pbip: Power BI Project
- pbir: Power BI Enhanced Report

*Classic Power BI File Types:*  
- bpit: Power BI Template
- pbix: Power BI Desktop

*Classic Report Server File Types:*  
- rdl: Power BI Paginated Report

# Use Git Ignore

Do not pubhlish data to github. Force git to ignore the data file: 
```
**/.pbi/cache.abf
```

# Power BI Project File Structure Overview

Power BI creates two top-level folders and one file when using the Power BI Desktop designer to save a pbip file to a local directory.  

```
.
├── projectName.Report/                           # Report Directory
├── projectName.SemanticModel/                    # Semantic Model Directory
└── projectName.pbip                              # Power BI Desktop file
```

# Power BI Project File Structure Detail

```
.
├── projectName.Report/                           # Report Directory
|   ├── StaticResources/
|   |   └── SharedResources/
|   |      └── BaseThemes/ 
|   |         └── {themeIdentifier}.json          # styling
|   ├── definition/ 
|   |   ├── pages/
|   |   |   ├── {pageIdentifier}/
|   |   |   |   ├── visuals/ 
|   |   |   |   |   └── {visualIdentifier}.json   # One file for each visual 
|   |   |   |   └── page.json
|   |   |   └── pages.json
|   |   ├── report.json   
|   |   └── version.json
|   ├── .platform
|   └── definition.pbir                           # Power BI enhanced report format
|
├── projectName.SemanticModel/                    #Semantic Model Directory
|   ├── .pbi/
|   |   └── editorSettings.json 
|   ├── DAXQueries/
|   |   ├── .pbi/
|   |   |   └── daxQueries.json
|   |   └── {identifier}.dax
|   └── definition/
|   |   ├── cultures/ 
|   |   |   └── {cultureIdentifier}.tmdl          # One file for each culture
|   |   ├── tables/
|   |       └── {tableIdentifier}.tmdl            # One file for each table 
|   ├── .platform 
|   ├── definition.pbism
|   └── diabramLayout.json
|
└── projectName.pbip                              # Power BI Desktop file
```


# Further Reading

Microsoft

- Learn Microsoft Fabric Power BI (15 Dec 2025). ["Power BI Desktop Projects](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-overview).

QED 

© Adam Heinz 

8 March 2026 
