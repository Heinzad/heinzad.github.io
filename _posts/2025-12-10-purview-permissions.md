AIDE MEMOIRE



Permissions in Microsoft Purview
================================

Role-Based Access Control (RBAC) applies at several levels in Microsoft Purview. 

- A role in Microsoft Purview permits users to perform specific actions in the platform. 

- A role group is a collection of one or more roles


Azure portal
------------

Many classic roles were moved to the new Microsoft Purview portal, but some still remain to be configured in the Azure portal. 

Navigate to the Microsoft Purview account in the Azure Portal. 

**Azure > Microsoft Purview accounts >**  
  
---
| Role | Description | Scope | Group Assignment | 
| ---- | ----------- | ----- | ---------------- | 
| Owner | Grants full access to manage all resources, including the ability to assign roles in Azure RBAC. | This resource |
| Contributor | Grants full access to manage all resources, but does not allow you to assign roles in Azure RBAC, manage assignments in Azure Blueprints, or share image galleries. | Resource Group (inherited) | {Service Principal} |


Microsoft Purview portal
------------------------

### Settings

Navigate to the Role group settings in the Microsoft Purview portal. 

**Microsoft Purview > Settings > Roles and scopes > Role Groups**

*Admin-level access for managing Data Map and Unified Catalog.*


| Role Group | Roles | 
| ---- | ----------- | 
| Purview Administrators | Admin Unit Extension Manager, Purview Domain Manager, Role Management |
| Data Source Administrators | Credential, Scan and Source Reader/Writer | 
| Data Governance | Data Governance Administrator | 


### Solution Settings

Navigate to the roles and permissions settings for the Unified Catalog in the Microsoft Purview portal.  

**Settings > Solution Settings > Unified Catalog > Roles and permissions >**

| Role | Description | 
| ---- | ----------- | 
| Data Governance Administrators | Assign top-level access to Governance Domain Creators. Manage other permissions at the application level. | 
| Governance Domain Creators | Create governance domains. Assign governance domain ownership | 
| Global Catalog Readers | View published artifacts in unrestricted governance domains (i.e. without Local Catalog Readers). | 
| Data Health Owners | Create update and view artifacts in Data Estate Health. | 
| Data Health Readers | View items in Data Estate Health. |



### Data Map Role assignments

*Permissions set at the domain level will cascade down through the child collections unless inheritance is restricted.*

**Microsoft Purview > Data Map > {Domain} >**


| Role | Description |
| ---- | ----------- |
| Domain admins | Edit the domain and its details. Add users and groups to roles in the domain. Create collections in the domain and assign collection admins.|

**Microsoft Purview > Data Map > {Domain} > {Collection} >**

If these permissions have not been set at the domain level they may be set at the collection level instead. 

*Inherited collection admins cannot be removed.* 

| Role | Description | 
| ---- | ----------- | 
| Collection admins | Edit collection and create subcollections. Add data curators, data readers, etc to the collection.  |
| Data source admins | Manage data sources and scans |
| Data curators | Create, read, update and delete data catalog objects and relationships. |
| Data readers | Access data catalog objects. |
| Insights readers | read data estate unsights reports. | 


### Data Governance Roles

**Microsoft Purview > Unified Catalog > Catalog Management > Governance Domains > {Domain Name} > Roles >**

*Local Catalog Reader is only used if it is necessary to restrict access to a particular governance domain. Open access is the default*  

| Role | Description | 
| ---- | ----------- | 
| Governance Domain Owners | Curate metadata and assign other permissions in the domain. | 
| Governance Domain Readers | View metadata in the domain. |
| Local Catalog Reader | (optional) Only listed users can view published concepts in the domain. |
| Data Product Owners | Create and update data products in the domain. |
| Data Steward | Create and update business concepts and policies in the domain. | 
| Data Quality Stewards | Data profiling, data quality rule management, scanning and scheduling. | 
| Data Quality Readers | Browse data quality rule definitions and data quality errors. |
| Data Profile Stewards | Monitor and run data profiling jobs. Browse data quality insights. | 
| Data Quality Metadata Readers | Browse data quality rule definitions and rule level scores. | 
| Data Profile Readers | Browse data quality insights and data profiling to column-level statistics. | 
  
  


Further Reading
---------------

Microsoft Learn: 
- ["Data Governance roles and permissions in Microsoft Purview"](https://learn.microsoft.com/en-us/purview/data-governance-roles-permissions) (18 June 2025), accessed December 2025. 

- ["Manage domains and collections in Microsoft Purview Data Map"](https://learn.microsoft.com/en-us/purview/data-map-domains-collections-manage) (25 June 2025), accessed December 2025.

- ["Metadata Roles"](https://learn.microsoft.com/en-us/rest/api/purview/metadatapolicydataplane/metadata-roles?view=rest-purview-metadatapolicydataplane-2021-07-01-preview) (2021-07-01), Microsoft Purview Data Map API, accessed December 2025. 
  
  
  
QED 

© Adam Heinz 

11 December 2025
