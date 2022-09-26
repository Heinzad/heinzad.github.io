Aide Memoire 

How to get a date number from a date in MSSQL 
============================================= 

When working in Business Intelligence or data engineering it is often necessary to use a date key instead of a date value. 

We can do this by [converting date formats](https://www.sqlshack.com/sql-convert-date-functions-and-formats/). 

There are two steps to generating a date key from a date value: 
- convert to date format 112 -- YYYYMMDD 
- convert to int 

```sql 

DECLARE @date date = CONVERT(date, GETDATE()) ; 

SELECT date_key = CONVERT(int, CONVERT(varchar, @date, 112)) ; 

``` 

QED 
