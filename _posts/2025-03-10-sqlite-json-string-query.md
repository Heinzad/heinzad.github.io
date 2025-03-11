AIDE MEMORE



How to Query Json Strings in SQLite 
=================================== 

SQLite can store and query strings in json format. 

Setup 
-----

```sql 
    CREATE TABLE testable (
        testid INTEGER PRIMARY KEY, 
        testcol TEXT 
    )
```

Insert json string 
------------------

```sql 
    INSERT INTO testable(testcol) 
    values('{"testfield": "Lorem Ipsum"}') 
    ; 
```


Selection  
---------

```sql
    SELECT json_extract(testcol, '$.testfield') as testfield
    FROM testable 
```


Teardown 
--------

```sql
    DROP TABLE IF EXISTS testable 
``` 

QED 

© Adam Heinz 
11 March 2025