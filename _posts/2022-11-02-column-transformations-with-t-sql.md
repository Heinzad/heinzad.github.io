Aide Memoire 

Column Transformations for data storage in MS SQL Server 
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

# Transformation graph 

We can conceptualise transformations in graph format: 

```mermaid 
graph LR; 
S((source)) -- input --> X((transformation)) 
X -- output --> T((target))
S -. from .-> ST((src tbl)) 
ST -- select --- SC((src cols))   
X -. calls .-> XF((function)) 
XF -- does --- XO((operation)) 
XF -- has --- XP((parameters))
T -. from .-> TT((tgt tbl))   
TT -- select --> TC((tgt cols))
subgraph src_cte  
SC
ST
end 
subgraph procedure  
XF 
XO 
XP
end
subgraph tgt_cte 
TC
TT 
end 
``` 

### Primary Transformations 

Scope: column  

Some transformations occur at the column level: 
- duplicate  
- reformat   

Some column-level transformations tag the output name to indicate its usage: 
- key 
- code 


| code | value | class | description | 
| ---- | ----- | ----- | ----------- | 
| copy | NULL | duplicate | preserves original format | 
| string2code | varchar(50) | reformat | Tags the column name for lookups with 'code' | 
| string2blobs | nvarchar(max) | reformat | converts to blob string | 
| string2longs | nvarchar(4000) | reformat | converts to long string | 
| string2shorts | nvarchar(255) | reformat | converts to short string | 
| number2key | int | duplicate | Tags the column name for joining to a dimension with 'key' | 
| number2bign | bigint | reformat | converts to large number | 
| number2wholen | int | reformat | converts to whole number | 
| number2floatn | float | reformat | converts to floating point number | 
| number2shorts | varchar(50) | reformat | converts non-additive numbers to short string | 
| float2shorts | varchar(50) | reformat | converts non-additive floating point numbers to short string |
| date2dated | datetime2(7) | reformat | converts to datestamp | 
| time2timed | time(7) | reformat | converts to timestamp | 
 

### Column Classification 

Subsequent working with column data will be more efficient if the columns are classified: 

| Classification | Description | 
| -------------- | ----------- | 
| key | whole number used in dimensions or lookups | 
| code | string used in dimensions or lookups | 
| metadata | date time or string information about a record | 
| id | record identifier that is not used in dimensions or lookups |  



### Secondary Transformations 

Some secondary transformations can now be considered, based on the data type and classification: 

| code | value | class | description | 
| ---- | ----- | ----- | ----------- | 
| number2dated | date | reformat | date number converted to date | 
| string2dated | date | reformat | string number converted to date | 



## Transformation Composition  

As a developer, I want to overwrite auto-identified transformations and auto-generated target column names. 
- Given a transform column in a tranformation form, when I choose to manually override, then I should be able select from a drop-down list. 
- Given a target column name in a transformation form, when I choose to manually override, then I should be able to rename the column. 

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

COPY_PREPARATION_EXAMPLE: 

DROP TABLE IF EXISTS [source_input] ; 
DROP TABLE IF EXISTS [target_output] ; 
GO 


COPY_SETUP_EXAMPLE: 

CREATE TABLE [source_input]( in_string varchar(20) ) ; 
CREATE TABLE [target_output] ( out_string char(20) ) ; 


COPY_INITIALISE_EXAMPLE: 

INSERT INTO [source_input]( in_string ) VALUES ("Lorem Ipsum") ; 


COPY_TRANSFORMATION_EXAMPLE: 

INSERT INTO [target_output]( out_string ) 
SELECT 
"string2string" = i.in_string     /* out_string */ 
FROM [source_input] as i 
; 


COPY_RESULT_EXAMPLE: 

SELECT TOP 1 o.out_string FROM [target_output] as o ; 

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

INSERT INTO [target_output]( 
     [out_short_txt]
    ,[out_long_txt] 
    ,[out_blob_txt] 
    ,[out_whole_num] 
    ,[out_float_num] 
    ,[out_datestamp] 
    ,[out_timestamp]
) 
SELECT 

    "string2shorts" = TRY_CONVERT( nvarchar(255), i.[in_string_txt] ) /* out_short_txt */ 
    ,"string2longs" = TRY_CONVERT( nvarchar(8000), i.[in_string_txt] ) /* out_long_txt */ 
    ,"string2blobs" = TRY_CONVERT( nvarchar(max), i.[in_string_txt] ) /* out_blob_txt */ 
    ,"decimal2wholen" = TRY_CONVERT( int, i.[in_decimal_num] ) /* out_whole_num */ 
    ,"decimal2floatn" = TRY_CONVERT( float, i.[in_decimal_num] ) /* out_float_num */ 
    ,"date2dated" = TRY_CONVERT( datetime2(7), i.in_datetime_ymd ) /* out_datestamp */ 
    ,"time2timed" = TRY_CONVERT( time(7), i.[in_time_hms] ) /* out_timed */  

FROM [source_input] i 
; 

``` 

