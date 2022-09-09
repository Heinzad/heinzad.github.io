Aide Memoire 

How to match Oracle, SQL Server, and Postgres data types 
======================================================== 

Any data warehousing operation crossing source systems is probably going to need to map data types across the source systems if they are all to land in the same platform. 

## Data Types Mapping 

| Oracle | SQL Server | PostgreSQL | 
| ------ | ---------- | ---------- | 
| char | char | char | 
| varchar | varchar | varchar | 
| varchar2 | nvarchar | ... | 
| number | numeric | numeric | 
| text | nvarchar(max)* | text | 
| raw | binary | ... | 
| long raw | varbinary | bytea | 
| rowid | integer** | ... | 
| date | date | ... | 
| timestamp | datetime2() | ... | 
| ... | bit | boolean | 
| ... | smallmoney | money | 
| ... | uniqueidentifier | uuid | 
| date | smalldatetime | timestamp | 
| ... | rowversion*** | bytea |  




- * MSSQL text is deprecated
- ** MSSQL integer stored as hexadecimal 
- *** MSSQL timestamp is deprecated for rowversion 

## Further Reading: 

Microsoft SQL Docs, [Data Type Mapping for Oracle Publishers](https://docs.microsoft.com/en-us/sql/relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers?view=sql-server-ver16), Microsoft, dated 2020-07-03. 

Ben Snaidero, [Comparing SQL Server and Oracle Data Types](https://www.mssqltips.com/sqlservertip/2944/comparing-sql-server-and-oracle-datatypes/), MSSQL Tips, Dated 2022-03-07. 

EDB, [PostgreSQL vs SQL Server (MSSQL): Extremely Detailed Comparison](https://www.enterprisedb.com/blog/microsoft-sql-server-mssql-vs-postgresql-comparison-details-what-differences), EnterpriseDB, dated 2020-07-31. 

