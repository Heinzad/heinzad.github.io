---
layout: post
tags: PowerBI
title: "Power BI Datasets from Data Flows"
author: "Adam Heinz"
date: 2023-08-30 01:00:00
---

When accessing Power BI dataflows when building datasets, the default behaviour is for Power BI to reference the dataflow guid directly. That may cause problems later if an old dataflow is replaced with a newer one with its own guid. Editing the power query to reference dataflow names may be the safer option. 

## 0 Initial Transformation 
Open Power BI, get data from Power Platform dataflow, and the auto-generated m query will look like this: 

```

let 
    Source = PowerPlatform.Dataflows(null), 
    Workspaces - Source{[Id="Workspaces"]}.Data, 
    #"abc361f4-03b2-471x-8c5f-28667d9fab28" = Workspaces{[workspaceId="abc361f4-03b2-471x-8c5f-28667d9fab28"]}[Data],
    #"df40ae11-150b-49f6-9e65-9f588a418e9a" = #"abc361f4-03b2-471x-8c5f-28667d9fab28"{[DataflowId="df40ae11-150b-49f6-9e65-9f588a418e9a"]}[Data], 
    #"dbo my_table_name" = #"df40ae11-150b-49f6-9e65-9f588a418e9a"{[entity="dbo my_table_name",version=""]}[Data]
in 
    #"dbo my_table_name" 

```

# 1 Parameterise Workspace and Dataflow Names 

Add a new parameter for the workspace name: 

pWorkspaceNameParam: 
```
"My Workspace Name" meta [IsParameterQuery=true, Type="Any", IsParameterQueryRequired=true] 

```

Add a new parameter for the dataflow name: 

pDataflowNameParam:
``` 
"My DataFlow Name" meta [IsParameterQuery=true, Type="Any", IsParameterQueryRequired=true] 

``` 

## 2 Create an info schema table 


INFO_SCHEMA:
``` 
let 
    Source = PowerPlatform.Dataflows(null), 
    Workspaces = Source{[Id="Workspaces"]}[Data], 
    // Workspace Navigation 
    myWorkspace = Workspaces{[workspaceName=pWorkspaceNameParam]}[Data], 
    // Dataflow Navigation 
    myDataflow = myWorkspace{[dataflowName=pDataflowNameParam]}[Data] 
in 
    myDataflow 

```

## 3 Navigate to the entity 

MY_TABLE: 
```
let 
    // Reference item
    Source = INFO_SCHEMA, 
    // Entity Navigation
    myEntity = Source{[entity="dbo my_table_name",version=""]}[Data] 
in 
    myEntity 

```

You now have a way of navigating to dataflows and their entites without using hard-coded guids in the m query. 


QED 

© Adam Heinz 

30 August 2023 
