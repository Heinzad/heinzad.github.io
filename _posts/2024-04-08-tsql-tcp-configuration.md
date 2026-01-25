---
layout: post
tags: MSSQL
title: "T-SQL TCP Configuration"
author: "Adam Heinz"
date: 2024-04-08 01:00:00
---
How to set TCP Network Configurations in SQL Server.  


# Overview

TCP is a very common means of accessing SQL Server. To set it up on a Windows machine we will need to cover: 
- SQL Server Configuration Manager
- services
- firewalls


## 1. SQL Server Configuration Manager 

Open SQL Server Configuration Manager > SQL Server Network Configuration > Protocols for MSSQLSERVER. 

Ensure the TCP/IP protocol is enabled. Click on it to access the properties. For each of the listed IT Addresses: 
- delete any setting for "TCP Dynamic Ports" so that it is blank 
- set "TCP Port" to 1433. 

## 2. Services 
Restart both the SQL Server engine and brower services. 

## 3. Firewall 

Add an inbound rule to Windows Firewall for port 1433 TCP/1P.




Further Reading: 
=========== 

Ahmad Yaseen [SQL Server network configuration](https://www.sqlshack.com/sql-server-network-configuration/), SQL Shack, 14 June 2016.

SQL Server, [Configure SQL Server to listen on a specific TCP port](https://learn.microsoft.com/en-us/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port?view=sql-server-ver16), Microsoft Learn, 31 March 2023. 

Atif Shehzad, [Configure Windows Firewall to Work with SQL Server](https://www.mssqltips.com/sqlservertip/1929/configure-windows-firewall-to-work-with-sql-server/), MSSQL Tips.


QED 

© Adam Heinz 

9 April 2024 

