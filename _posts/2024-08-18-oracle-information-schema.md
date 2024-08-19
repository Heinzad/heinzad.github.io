AIDE MEMOIRE

How to Map Oracle to Information Schema
=======================================

Schemata:  
```sql
SELECT DISTINCT OWNER
FROM ALL_OBJECTS
WHERE OWNER NOT LIKE '%SYS'
```

Base Tables: 
```sql
SELECT * 
FROM ALL_TABLES
WHERE OWNER NOT LIKE '%SYS'
```

Views: 
```sql
SELECT * 
FROM ALL_VIEWS
WHERE OWNER NOT LIKE '%SYS'
```

Columns: 
```sql
SELECT * 
FROM ALL_TAB_COLUMNS
WHERE OWNER NOT LIKE '%SYS'
```

Routines: 
```sql
SELECT * 
FROM ALL_PROCEDURES
WHERE OWNER NOT LIKE '%SYS'
```



QED 

© Adam Heinz 

19 August 2024