---
title: "Purview Permissions 2: Tenant-Level Permissions"
author: "Adam Heinz"
date: 2025-12-11T01:00:00-00:00
categories:
  - Purview
---
How to apply tenant-level RBAC in Microsoft Purview.  


# Overview

Role-Based Access Control (RBAC) applies at several levels in Microsoft Purview. 

- A role in Microsoft Purview permits users to perform specific actions in the platform. 

- A role group is a collection of one or more roles

Microsoft provides several built-in role groups. 
Many organisations prefer to create their own custom role groups. 

0. MS Purview Portal Managers
1. MS Purview Data Governance Administrators
2. MS Purview Data Governance Contributors
3. MS Purview Data Governance Readers  

This can help distinguish the permissions needed for the Data Governance solution from those applied to the Security and Risk & Compliance solutions. 


Custom Role Groups
------------------
*Create custom role groups based on built-in models*

Navigate to the Role group settings in the Microsoft Purview portal. 

**Microsoft Purview > Settings > Roles and scopes > Role Groups**  



### (0) MS Purview Portal Managers

- Control permissions for accessing features in these portals, and also manage settings for device management, data loss prevention, reports, and preservation (Organisation Management).  
  
| Item | Role Name | Role Description | Built-in Role Group |
| ---- | --------- | ---------------- | ------------------ |
| 1 | Admin Unit Extension Manager | . | Organisation Management |
| 2 | Role Management | Manage role group membership and create or delete custom role groups. | Purview Administrators |
  

### (1) MS Purview Data Governance Administrators

- Create, edit, and delete domains and perform role assignments (Purview Administrators).
- Grant access to data governance roles within Microsoft Purview (Data Governance).  
  
| Item | Role Name | Role Description | Built-in Role Group |
| ---- | --------- | ---------------- | ------------------ |
| 1 | Admin Unit Extension Manager | . | Purview Administrators |
| 2 | Purview Domain Manager | Create, edit, and delete domains and perform role assignments. | Purview Administrators |
| 3 | Role Management | Manage role group membership and create or delete custom role groups. | Purview Administrators |
| 4 | Data Governance Administrator | Delegates the first level of access for business domain creators and other application-level permissions. | Data Governance |
  


### (2) MS Purview Data Governance Contributors

- Manage data sources and data scans (Data Source Administrators).
- Create, read, modify, and delete actions on catalog data objects and establish relationships between objects (Data Catalog Curators). 
- Admin access to all insights reports across platforms and providers (Data Estate Insights Admins).  
  
| Item | Role Name | Role Description | Built-in Role Group |
| ---- | --------- | ---------------- | ------------------ |
| 1 | Data Map Writer | . | Data Catalog Curators |
| 2 | Insights Writer | . | Data Estate Insights Admins |
| 3 | Credential Writer | Create and edit credentials. | Data Source Administrators |
| 4 | Scan Writer | Create, update, and delete scans in the tenant. | Data Source Administrators |
| 5 | Source Writer | Create, update, and delete sources in the tenant. | Data Source Administrators |
| 6 | Data Map Reader | . | Data Catalog Curators |
| 7 | Insights Reader | read-only access to all Insights reports in the Data Estate Insights app. | Data Estate Insights Admins |
| 8 | Credential Reader | Read the different credentials created in the tenant. | Data Source Administrators |
| 9 | Scan Reader | Read the different scans created in the tenant. | Data Source Administrators |
| 10 | Source Reader | Read the different sources created in the tenant. | Data Source Administrators |
  


### (3) MS Purview Data Governance Readers

- Read-only access to all insights reports across platforms and providers (Data Estate Insights Readers).  
  
| Item | Role Name | Role Description | Built-in Role Group |
| ---- | --------- | ---------------- | ------------------ |
| 1 | Data Map Reader | . | Data Estate Insights Readers |
| 2 | Insights Reader | read-only access to all Insights reports in the Data Estate Insights app. | Data Estate Insights Readers |
  

Note: Insights readers need to have at least data reader role access to a collection to view reports about that specific collection.  


QED 

© Adam Heinz 

12 December 2025
