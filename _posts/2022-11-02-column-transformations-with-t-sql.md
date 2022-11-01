Aide Memoire 

How to Perform Column Transformations in MS SQL Server 
====================================================== 

It is useful to perform transformations that enable us to store data in several generic formats. 

If we think of the tool in which the information is going to be displayed, it is probably only going to handle a number of key data formats. 

Take the [Power BI data formats](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-data-types) for example: 
- float 
- money (fixed) 
- whole 
- text 
- date/time 
- boolean 

If you can remember back to the [MS Access data types](https://support.microsoft.com/en-us/office/data-types-for-access-desktop-databases-df2b83ba-cef6-436d-b679-3418f622e482), we had: 
- short text 
- long text 
- number 
- currency 
- boolean 


It is also useful to distinguish how to store data (in a few limited formats) from how to calculate it. 
Take decimal numbers, for example. It is handy to store them as floats, but for the sake of accuracy we probably need to avoid floating point calculations by converting them to something like numeric(19,4) before performing a calculation in the database. 


# Column-level transformations 

Scope: column  

Some transformations occur at the column level: 
- copy 
- reformat   


## Copy transformation: 
Information from one table column is copied to a different table column. 

Copy transformation data flow diagram: 

```mermaid 
graph LR; 
A[string]-- in -->T(copy); 
T-- out -->B[string];
``` 

Copy transformation example code:  

```tsql 

/*** copy transformation ***/ 

EXAMPLE_PREPARATION: 

DROP TABLE IF EXISTS [input_entity] ; 
DROP TABLE IF EXISTS [output_entity] ; 
GO 


EXAMPLE_SETUP: 

CREATE TABLE [input_entity]( in_string varchar(20) ) ; 
CREATE TABLE [output_entity] ( out_string char(20) ) ; 


EXAMPLE_INITIALISE: 

INSERT INTO [input_entity]( in_string ) VALUES ("Lorem Ipsum") ; 


EXAMPLE_TRANSFORMATION: 

INSERT INTO [output_entity]( out_string ) 
SELECT 
"string2string" = i.in_string     /* out_string */ 
FROM [input_entity] as i 
; 

``` 

## Reformat transformation: 
Information in one data format is changed to a different data format. 

Reformat data flow diagram: 

```mermaid 
graph LR; 
A[decimal]-- in -->T(reformat); 
T-- out -->B[float];
``` 


Reformat example code: 

```tsql 

/*** reformat transformation ***/ 

INSERT INTO [output_entity]( 
     [out_short_txt]
    ,[out_long_txt] 
    ,[out_blob_txt] 
    ,[out_whole_num] 
    ,[out_float_num] 
    ,[out_datestamp] 
    ,[out_timestamp]
) 
SELECT 

    "string2shorts" = TRY_CONVERT( nvarchar(255), i.[in_string_txt] )     /* out_short_txt */ 
    ,"string2longs" = TRY_CONVERT( nvarchar(8000), i.[in_string_txt] )     /* out_long_txt */ 
    ,"string2blobs" = TRY_CONVERT( nvarchar(max), i.[in_string_txt] )      /* out_blob_txt */ 
    ,"decimal2wholen" = TRY_CONVERT( int, i.[in_decimal_num] )           /* out_whole_num */ 
    ,"decimal2floatn" = TRY_CONVERT( float, i.[in_decimal_num] )         /* out_float_num */ 
    ,"date2dated" = TRY_CONVERT( datetime2(7), i.in_datetime_ymd )       /* out_datestamp */ 
    ,"time2timed" = TRY_CONVERT( time(7), i.[in_time_hms] )               /* out_timed */  

FROM [input_entity] i 
; 

``` 

