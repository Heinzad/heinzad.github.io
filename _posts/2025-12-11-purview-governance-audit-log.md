---
title: "Purview Audit Log: Trace Data Governance Access"
author: "Adam Heinz"
date: 2026-04-06T01:00:00-00:00
categories:
  - Purview
---
How to discover who accessed Microsoft Purview.  


# Overview

Trace user access of the Data Governance solution using the Risk and Compliance Solution. Unified Audit Log search is turned on by default for M365 and O365 Enterprise licences. Audit Logs are retained for 180 days by default. 

# Quickstart  

0. Role: Login with an Audit Logs role.
1. Navigate: Microsoft Purview > Audit solution. 
2. Date range: Pick a timeframe. 
3. Activities: Friendly or operational names: Pick from (search, glossary, classification, etc).
3. Record types: Pick from (PurviewDataMapOperation, PurviewMetadataPolicyOperation, PurviewPolicyOperation). 
4. User:s UPN or app id.
5. File, folder or site. 
6. Workloads: (PurviewDataMapOperation, PurviewMetadataPolicyOperation, PurviewPolicyOperation). 
7. Export: download results to csv. 

---

# Role-Based Access Control (RBAC) 

Membership in Audit Logs or View-Only Audit Logs role. 
  

*Built-in Role Groups:* 

| role group | description | 
| ---------- | ----------- | 
| Audit Manager | Manage Audit log settings and Search, View, and Export Audit logs. | 
| Audit Reader | Search, View and Export Audit Logs. | 
| Global Reader | Read-only access to reports, alerts, and configutation and settings | 
| Organisation Management | Control permissions for accessing features | 
  

*Roles:* 

| role | description | default role group |
| ---- | ----------- | ------------------ | 
| Audit Logs | Turn on and configure auditing for the organisation, view the organisation's audit reports, and export reports to file. | Audit Manager | 
| View-Only Audit Logs | View and export audit reports. Note: Only assign this role to people with an explicit need to know this information. | Audit Reader | 


# Activities 

*Audited Microsoft Purview Governance Activities:*  

| name | description | 
| ---- | ----------- |  
| EntityCreated | Asset or entity created | 
| ClassificationAdded | Classification added | 
| ClassificationDefinitionCreated | Classification definition created  
| ClassificationDefinitionDeleted | Classification definition deleted | 
| ClassificationDefinitionUpdated | Classification definition updated | 
| ClassificationDeleted | Classification deleted | 
| ClassificationUpdated | Classification updated | 
| EntityDeleted | Entity deleted | 
| EntityUpdated | Entity updated | 
| GlossaryTermAssigned | Glossary term assigned | 
| GlossaryTermCreated | Glossary term created | 
| GlossaryTermDeleted | Glossary term deleted | 
| GlossaryTermDisassociated | Glossary term disassociated | 
| GlossaryTermUpdated | Glossary term updated | 
| SensitivityLabelChanged | Sensitivity label changed | 
  

*Microsoft Purview on-demand classification activities (Audit Premium):*  

| name | description | 
| ---- | ----------- | 
| DataScanClassification | Classified file in an on-demand classification scan for SharePoint or OneDrive. | 
| SensitiveInfoDiscovered | Classified file on Endpoint device in an on-demand classification scan. | 
| DataScanDeClassification | Declassified file in an on-demand classification scan. |


---
# Activity Types 

*Microsoft Purview-specific Activity Types:*  

| name | description | 
| ---- | ----------- | 
| OldValue | The value before a change, includes all properties updated or deleted. | 
| NewValue | The value after a change, includes all properties updated or deleted. | 
| ObjectFullyQualifiedName | Fully qualified name of an entity. | 
| ObjectName | Entity name. | 
| SecurityComplianceCenterEventType | 0 indicates a Microsoft Purview portal activity. |  
  

*Generic Activity Types that appply to Microsoft Purview:*  

| name | description | 
| ---- | ----------- | 
| CreationTime | UTC datetime when activity logged. |
| CurrentProtectionType | Fields: ProtectionType, Owner, TemplateID, DocumentEncrypted. | 
| ID | Uniquely identifies a Report entry. | 
| ModifiedProperties | Fields: Name, NewValue, OldValue. | 
| ObjectId | The name or URL of the modified object. | 
| Operation | An audited activity. | 
| OrganizationId | The GUID of the organisation. | 
| PreviousProtectionType | Fields: ProtectionType, Owner, TemplateId, DocumentEncrypted. | 
| ProtectionEventType | Protection changes indicator (0 - unchanged, 1 - added, 2, changed, 3 removed). | 
| RecordType | Indicator values (38 - DataGovernance retention policies and labels, 52 - DataInsightsRestApiAudit, 331 - DataCatalogAccessRequests, ). | 
| ResultStatus | Action success flag (True - successful, False - Failed). | 
| UserID | The user who performed the action. | 
| UserKey | A Microsoft Entra ID Object ID. | 
| UserType | The type of user that performed the operation. | 
| Version | The version number of the logged activity. | 
| Workload | The service where the activity occurred. | 

---
# Further Reading 

- IT Trip, [How to Audit Microsoft Purview Portal Access and API Calls](https://en.ittrip.xyz/microsoft-365/purview-audit-api-access), accessed 7 April 2026.  
- Dominique Hermans (15 April 2024). [Microsoft Purview 101: Mastering the (Unified) Audit Log](https://dominiquehermans.com/2024/08/15/microsoft-purview-101-mastering-the-unified-audit-log/), accessed 7 April 2026.  
- Learn Cloud Partner (28 March 2024). [Section 13 - Manage and analyze audit logs and reports in Microsoft Purview](https://learn.cloudpartner.fi/posts/section-13-manage-and-analyze-audit-logs-and-reports-in-microsoft-purview), accessed 7 April 2026.  
  

Microsoft:  

- Learn Microsoft Purview (18 February 2026). [Search the audit log](https://learn.microsoft.com/en-us/purview/audit-search), accessed 7 April 2026.  
- Learn Microsoft Purview (18 February 2026). [Detailed activity properties in the audit log](https://learn.microsoft.com/en-us/purview/audit-log-detailed-properties), accessed 7 April 2026.  
- Learn Microsoft Purview (4 February 2026). Audit Log Activities: ["Microsoft Purview governance activities"](https://learn.microsoft.com/en-us/purview/audit-log-activities#microsoft-purview-governance-activities), accessed 7 April 2026.  
- Learn Microsoft Purview (4 February 2026). Audit Log Activities: ["Microsoft Purview on-demand classification activities"](https://learn.microsoft.com/en-us/purview/audit-log-activities#microsoft-purview-on-demand-classification-activities), accessed 7 April 2026.  
- Learn Microsoft Purview (). [Learn about auditing solutions in Microsoft Purview](https://learn.microsoft.com/en-us/purview/audit-solutions-overview)

---

QED 

© Adam Heinz 

7 April 2026
