---
title: "Data Architecture"
excerpt: "Saving an ERP from month-end reporting — BI Developer, Corporate IT, PGW, 2018-2020"
author: "Adam Heinz"
date: 2026-03-30T20:18:00-00:00
ordinal: 400
---
*Solving ERP overload.*   

Title: Business Intelligence Developer  
Department: Corporate IT  
Organisation: PGG Wrightson

Achievements:  
  
- Near-Realtime Operational Reporting. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ERP_ODS_AdamHeinz.png" alt="Near-Realtime Data Architecture" class="full">
*Photo by James Lee on Unsplash* 

---

**Timing is everything** in collecting Accounts Payable.  

Contact a late-paying customer early and you meet your monthly KPIs. Contact a paying customer late - after clearing their bill - and you cause enough offence to lose the customer. 

Month-end is a critical time. AP staff constantly query the ERP system and run operational reports. 
At month-end the system slowed to a crawl. The database administrators disconnected operational reporting as there was a serious risk that transactions might be blocked.  

The situation was that several hundred operational reports could not be run or redeveloped as long as the DBAs banned connections to the ERP. 
My task was to enable operational reporting again. 

My actions were to diagnose the issue, then design and deliver a near-realtime reporting system that solved the whitespace problem. 

Whitespace was the problem. Database indexes can ignore whitespace at the end of a line (trailing spaces), but cannot handle whitespace at the beginning of a line (leading spaces). This meant that database queries could not use the indexes and had to brute-force their way through millions of records to discover the one required. 

My solution was to design and develop an Operational Data Store (ODS) populated by Change Data Capture (CDC) pushed from the source system. Data cleansing and reindexing meant that all existing operational reports could be repointed to the ODS and be able to run again without impacting the source system. 

I implemented a snowflake schema for a tabular model with partitioning to support both hot and cold data. This supported both Power BI dashboards and Excel with near-realtime data updated every 15 minutes. 

The result was to enable Accounts Payable staff to run every query they needed, with confidence, before contacting customers with outstanding debts. 

--- 
Responsibilities:  

- SQL Database design and development (tables, views, functions, and stored procedures). 
- ETL development (Wherescape Red, Microsoft Dynamics 365). 
- Tabular model design and development (SSAS, Power BI). 
- Operational reporting from JD Edwards EnterpriseOne ERP for Accounting, Payables, Receivables, Procurement, Distribution Centres (SSRS, Power BI)
- Human resources reporting and analytics from Payglobal (SSRS).   

  
Technology:  

- JD Edwards ERP. 
- Microsoft SQL Server database engine (MS SQL), Integration Services (SSIS), Analysis Services (SSAS), Report Server and Power BI Service (Power BI).  
  

Outcomes:  

- Achieved near real-time performance to supply fresh and accurate data within 15 minutes of a new record being created.  

- Design and Development of an operational data store (ODS) and Power BI data model to support operational reporting and dashboards from JD Edwards EnterpriseOne ERP (JDE E1).


© Adam Heinz 
2017
