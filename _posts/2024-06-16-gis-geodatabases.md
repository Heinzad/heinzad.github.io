---
layout: post
tags: GIS
title: "Geodatabases"
author: "Adam Heinz"
date: 2024-06-16 01:00:00
---

AIDE MEMOIRE

What is a Geodatabase? 
======================


Geodatabases are a term utilised by ArcGIS. They can usefully be contrasted with shapefiles and spatial databases. 

## Shapefiles 

Shapefiles are a longstanding open specification of data interoperability for vector data types. 

A shapefile may only contain one vector data type:  

- points
- lines
- polygons


### Files in a shapefile: 

The various files can be distinguished by their extensions: 
- shp - shape geometry 
- shx - shape index
- dbf - attributes (dBase format)
- prj - projection (crs wkt)

### Feature Layer: 

A Feature layer is a zipped shapefile collating all the files that make up the "shapefile" itself. 


## Spatial Database Servers

Spatial databases, such as Postgres with the PostGIS extension, provide their own spatial indexing and spatial functions (e.g. ST_Contains(), ST_Intersects(), etc.) 

## Geodatabases 

Geodatabases are ESRI-specific. They are either server- or file-based systems. They are managed using ArcGIS Catalog. 

### File-based geodatabases:

The file-based geodatabases can be distinguished by their extensions: 

- **mdb** - *Personal Geodatabase* - An MS Access RDBMS (deprecated)
- **geodatabase** - *Mobile Geodatabase* - An SQLite RDBMS. 
- **gdb** - *File Geodatabase* - A collection of files in a folder. Can store non-vector datatypes. 

### Server-based geodatabases: 

*Enterprise Geodatabases* - also known as multi-user geodatabses

ArcGIS acts as an application server, utilising storage in a database server: 

- DB2
- Oracle
- PostgreSQL
- SAP HANA
- SQL Server 




# Further Reading" 

ESRI, [Shapefiles](https://doc.arcgis.com/en/arcgis-online/reference/shapefiles.htm) 

--- [What is a geodatabase?](https://pro.arcgis.com/en/pro-app/latest/help/data/geodatabases/overview/what-is-a-geodatabase-.htm). ArcGIS Pro 3.3, ESRI. 

--- [File geodatabases](https://pro.arcgis.com/en/pro-app/latest/help/data/geodatabases/manage-file-gdb/file-geodatabases.htm). ArcGIS Pro 3.3, ESRI. 

--- [Types of geodatabases](https://pro.arcgis.com/en/pro-app/latest/help/data/geodatabases/overview/types-of-geodatabases.htm). ArcGIS Pro 3.3, ESRI. 

--- Elaine Evans (18 may 2023). [It's Not Personal: A brief history of the geodatabase and why personal geodatabases are not in ArcGIS Pro](https://www.esri.com/arcgis-blog/products/arcgis-pro/data-management/its-not-personal/). ArgGIS Pro, Data Management, ESRI.  

--- Donald Rees (14 January 2021) [Look at Mobile Geodatabases go! (What’s new in Pro 2.7)](https://www.esri.com/arcgis-blog/products/arcgis-pro/data-management/look-at-mobile-geodatabases-go-whats-new-in-pro-2-7/). ArgGIS Pro, Data Management, ESRI.

Wikipedia [Shapefile](https://en.wikipedia.org/wiki/Shapefile) 



QED 

© Adam Heinz 

17 June 2024