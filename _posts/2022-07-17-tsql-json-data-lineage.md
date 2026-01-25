Aide Memoire 

How to record data lineage in json format with T-SQL 
============================================================== 

ETL is a big topic, but one critical aspect is to record the transformations that have been done to a piece of data. 

When performing etl (elt, let, etc) in traditional databases, it is quite common to record the "load date" in one column, the "stage date" in another, and so on. Those columns quickly add up, and no matter how many columns you created there is always something else that in hindsight really ought to have been recorded. 

Even in traditional databases, however, tracing the data lineage across transformations can quickly get quite complex. One datum may end up in a completely different place from its original column and row in a table. 

In this context, recording the transformation history in json format is a handy option that can travel with the data wherever it goes. Json format is human-readable and if strict layout as rows and columns is wanted at a later stage, that can still be done with data stored in json format. 

We can walk through a transact-sql example with SQL Server. 

## Core method 

The heart of this approach is that once we have a json, we can modify it repeatedly to record the transformations that have been perfomed on the data. 

The Microsoft documentations for [json_modify](https://docs.microsoft.com/en-us/sql/t-sql/functions/json-modify-transact-sql?view=sql-server-ver16) gives a number of options, such as append, etc. 

The example given here utilises the simplest option of all: 

```tsql 
DECLARE @json nvarchar(max) = N'{}' ; 

SET @json = JSON_MODIFY( @json, '$.readme', 'All transformation operations applied to this data are recorded here' ) ; 

PRINT @json ; 
``` 

The technique for performing multiple json modify operations is conceptually similar to performing multiple text replace operations. Multiple json modifications are demonstrated in a gist on [SQL to JSON datetime modifications.sql](https://gist.github.com/bertwagner/0096952bb76f12e40dc1c85c683ac331) by Bert Wagner.  

## Example Tables 
Let's illustrate the concept of recording the data lineage with a very basic progression through three temporary tables - load, stage, and store. 
Each table will have a data lineage column in which to record transformation metadata: 

```tsql 
CREATE TABLE #tmp_load_tbl ( 
 load_id int identity(-2147483648,1) not null, 
 load_detail nvarchar(255), 
 load_data_lineage nvarchar(4000),
 CONSTRAINT tmp_load_tbl_chk_transformation_isjson CHECK( ISJSON(load_data_lineage)=1 ), 
 CONSTRAINT tmp_load_tbl_pk PRIMARY KEY CLUSTERED ( load_id ) WITH(DATA_COMPRESSION=PAGE)    
) ; 

CREATE TABLE #tmp_stage_tbl ( 
 stage_id int identity(-2147483648,1) not null, 
 stage_detail nvarchar(255), 
 stage_data_lineage nvarchar(4000), 
 CONSTRAINT tmp_stage_tbl_chk_transformation_isjson CHECK( ISJSON(stage_data_lineage)=1 ), 
 CONSTRAINT tmp_stage_tbl_pk PRIMARY KEY CLUSTERED ( stage_id ) WITH(DATA_COMPRESSION=PAGE)      
) ; 

CREATE TABLE #tmp_store_tbl ( 
 store_id int identity(-2147483648,1) not null,  
 store_detail nvarchar(255), 
 store_data_lineage nvarchar(4000), 
 CONSTRAINT tmp_store_tbl_chk_transformation_isjson CHECK( ISJSON(store_data_lineage)=1 ), 
 CONSTRAINT tmp_store_tbl_pk PRIMARY KEY CLUSTERED ( store_id ) WITH(DATA_COMPRESSION=PAGE)    
) ;  

GO 
``` 

## Procedures 
Each table will have a corresponding procedure to populate it. They are almost - but not quite - identical. 

```tsql 
CREATE PROC #tmp_load_upsert AS 

BEGIN 
SET NOCOUNT ON 
SET XACT_ABORT ON 
	
	 
	TRANSFORMATION_SEED: 
	
	DECLARE @new_transformation_record nvarchar(4000) = N'{}' ; 

	SELECT @new_transformation_record 
	
	TRANSFORMATION_DETAILS: 
	
	DECLARE @mod_transformation_record nvarchar(4000) = N'{}' ; 

	DECLARE @tbl_transformation  table ( 
	 transform_id int
	,transform_name nvarchar(550) 
	,transform_date_local datetime 
	,transform_date_utc datetime2(7) 
	) 

	INSERT INTO @tbl_transformation ( 
	 transform_id 
	,transform_name
	,transform_date_local 
	,transform_date_utc 
	) 
	SELECT 
	 transform_id = @@PROCID 
	,transform_name = OBJECT_NAME( @@PROCID ) 
	,transform_date_local = GetDate() 
	,transform_date_utc = SYSUTCDATETIME() 
	; 

	
	SET @mod_transformation_record = ( 
		SELECT 
		 transform_id as load_transform_id
		,transform_name as load_transform_name  
		,transform_date_local as load_transform_date_local  
		,transform_date_utc as load_transform_date_utc  
		FROM @tbl_transformation t 
		FOR JSON PATH, WITHOUT_ARRAY_WRAPPER, INCLUDE_NULL_VALUES  
	) 
	; 

	PRINT @mod_transformation_record 

	MAIN_QUERY: 

	TRUNCATE TABLE #tmp_load_tbl ; 
 

	INSERT INTO #tmp_load_tbl ( 
		 load_detail 
		,load_data_lineage 
	) 
	SELECT 
	 load_detail = 'this is a test record' 
	--,load_data_lineage = JSON_MODIFY( @new_transformation_record, '$.load', JSON_QUERY(@mod_transformation_record) ) 
	,load_data_lineage = 
		JSON_MODIFY( 
			JSON_MODIFY(
				JSON_MODIFY( 
					JSON_MODIFY( @new_transformation_record,  
					'$.load_transform_id', @@PROCID 
					), 
				'$.load_transform_name', OBJECT_NAME( @@PROCID ) 
				) , 
			'$.load_transform_date_local', CONVERT(nvarchar, GetDate(), 127)  /* --FORMAT( GetDate(), 'yyyy-MM-ddTHH:mm:ss.fff') --'yyyy-MM-ddTHH:mm:ssZZ' */  
			),  
		'$.load_transform_date_utc', CONVERT(nvarchar, SYSUTCDATETIME(), 127)  ----FORMAT( SYSUTCDATETIME(), 'yyyy-MM-ddTHH:mm:ss.fffZZ') -- 'O'  
		) 

END ; 


CREATE PROC #tmp_stage_upsert AS 

BEGIN 
SET NOCOUNT ON 
SET XACT_ABORT ON 
	 
	
	TRANSFORMATION_DETAILS: 
	
	DECLARE @mod_transformation_record nvarchar(4000) = N'' ;

	DECLARE @tbl_transformation  table ( 
	 transform_id int
	,transform_name nvarchar(550) 
	,transform_date_local datetime 
	,transform_date_utc datetime2(7) 
	) 

	INSERT INTO @tbl_transformation ( 
	 transform_id 
	,transform_name
	,transform_date_local 
	,transform_date_utc 
	) 
	SELECT 
	 transform_id = @@PROCID 
	,transform_name = OBJECT_NAME( @@PROCID ) 
	,transform_date_local = GetDate() 
	,transform_date_utc = SYSUTCDATETIME() 
	; 

	
	SET @mod_transformation_record = ( 
		SELECT 
		 transform_id as stage_transform_id
		,transform_name as stage_transform_name  
		,transform_date_local as stage_transform_date_local  
		,transform_date_utc as stage_transform_date_utc  
		FROM @tbl_transformation t 
		FOR JSON PATH, WITHOUT_ARRAY_WRAPPER, INCLUDE_NULL_VALUES  
	) 
	; 

	PRINT @mod_transformation_record  

	MAIN_QUERY: 

	TRUNCATE TABLE #tmp_stage_tbl ; 

	INSERT INTO #tmp_stage_tbl ( 
		 stage_detail 
		,stage_data_lineage 
	) 
	SELECT 
	 stage_detail = l.load_detail 
	,stage_data_lineage = 
		JSON_MODIFY( 
			JSON_MODIFY(
				JSON_MODIFY( 
					JSON_MODIFY( load_data_lineage,  
					'$.stage_transform_id', @@PROCID 
					), 
				'$.stage_transform_name', OBJECT_NAME( @@PROCID ) 
				) , 
			'$.stage_transform_date_local', CONVERT(nvarchar, GetDate(), 127)  
			),  
		'$.stage_transform_date_utc', CONVERT(nvarchar, SYSUTCDATETIME(), 127) 
		) 
	
	FROM #tmp_load_tbl as l 

END ; 


CREATE OR ALTER PROC #tmp_store_upsert AS 

BEGIN 
SET NOCOUNT ON 
SET XACT_ABORT ON 
	 
	

	TRANSFORMATION_DETAILS: 
	
	
	DECLARE @mod_transformation_record nvarchar(4000) = N'' ;

	DECLARE @tbl_transformation  table ( 
	 transform_id int
	,transform_name nvarchar(550) 
	,transform_date_local datetime 
	,transform_date_utc datetime2(7) 
	) 

	INSERT INTO @tbl_transformation ( 
	 transform_id 
	,transform_name
	,transform_date_local 
	,transform_date_utc 
	) 
	SELECT 
	 transform_id = @@PROCID 
	,transform_name = OBJECT_NAME( @@PROCID ) 
	,transform_date_local = GetDate() 
	,transform_date_utc = SYSUTCDATETIME() 
	; 

	
	SET @mod_transformation_record = ( 
		SELECT 
		 transform_id as store_transform_id
		,transform_name as store_transform_name  
		,transform_date_local as store_transform_date_local  
		,transform_date_utc as store_transform_date_utc  
		FROM @tbl_transformation t 
		FOR JSON PATH, WITHOUT_ARRAY_WRAPPER, INCLUDE_NULL_VALUES  
	) 
	;  

	PRINT @mod_transformation_record  

	MAIN_QUERY: 

	TRUNCATE TABLE #tmp_store_tbl ; 
 

	INSERT INTO #tmp_store_tbl ( 
		 store_detail 
		,store_data_lineage 
	) 
	SELECT 
	 store_detail = s.stage_detail 
	,store_data_lineage = 
		JSON_MODIFY( 
			JSON_MODIFY(
				JSON_MODIFY( 
					JSON_MODIFY( stage_data_lineage,  
					'$.store_transform_id', @@PROCID 
					), 
				'$.store_transform_name', OBJECT_NAME( @@PROCID ) 
				) , 
			'$.store_transform_date_local', CONVERT(nvarchar, GetDate(), 127)  
			),  
		'$.store_transform_date_utc', CONVERT(nvarchar, SYSUTCDATETIME(), 127) 
		) 
	
	FROM #tmp_stage_tbl as s

END ;

GO  
``` 

Let's now run the procedures: 

```tsql 
EXEC #tmp_load_upsert ; 
EXEC #tmp_stage_upsert ; 
EXEC #tmp_store_upsert ; 
``` 

## Viewing the data lineage 

While json is human-readable at a push, you really need a json-aware text editor to be able to see it laid out comprehensibly. 

### Query: 

```tsql 

SELECT store_data_lineage FROM #tmp_store_tbl ; 

``` 

### Result: 

```json 
{
   "load_transform_id":-1499977385, 
   "load_transform_date_local":"2022-07-17T16:24:37.747", 
   "load_transform_date_utc":"2022-07-17T04:24:37.7486210", 
   "stage_transform_id":-1467977271, 
   "stage_transform_date_local":"2022-07-17T16:24:37.753", 
   "stage_transform_date_utc":"2022-07-17T04:24:37.7566145", 
   "store_transform_id":-1435977157, 
   "store_transform_date_local":"2022-07-17T16:24:37.760", 
   "store_transform_date_utc":"2022-07-17T04:24:37.7626108"
}
``` 

However we can also shred the json in tsql using the [with clause to the openjson](https://docs.microsoft.com/en-us/sql/relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server?view=sql-server-ver16) method.  

### View Definition: 

```tsql 
SELECT 
 tbl.store_id 
,tbl.store_detail   

,js.load_transform_id 
,js.load_transform_name 
,js.load_transform_date_local 
,js.load_transform_date_utc 

,js.stage_transform_id 
,js.stage_transform_name 
,js.stage_transform_date_local 
,js.stage_transform_date_utc

,js.store_transform_id 
,js.store_transform_name 
,js.store_transform_date_local 
,js.store_transform_date_utc  

FROM #tmp_store_tbl as tbl
OUTER APPLY OPENJSON ( tbl.store_data_lineage ) 
WITH ( 
	 load_transform_id int N'$.load_transform_id' 
	,load_transform_name nvarchar(550) N'$.load_transform_name' 
	,load_transform_date_local datetime N'$.load_transform_date_local' 
	,load_transform_date_utc datetime2(7) N'$.load_transform_date_utc' 

	,stage_transform_id int N'$.stage_transform_id' 
	,stage_transform_name nvarchar(550) N'$.stage_transform_name' 
	,stage_transform_date_local datetime N'$.stage_transform_date_local' 
	,stage_transform_date_utc datetime2(7) N'$.stage_transform_date_utc' 

	,store_transform_id int N'$.store_transform_id' 
	,store_transform_name nvarchar(550) N'$.store_transform_name' 
	,store_transform_date_local datetime N'$.store_transform_date_local' 
	,store_transform_date_utc datetime2(7) N'$.store_transform_date_utc'

) as js   

``` 

### View Result Table: 

| store_id | store_detail | load_transform_id | load_transform_name | load_transform_date_local | load_transform_date_utc | stage_transform_id | stage_transform_name | stage_transform_date_local | stage_transform_date_utc | store_transform_id | store_transform_name | store_transform_date_local | store_transform_date_utc | 
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
| -2147483648 | this is a test record | -1499977385 | NULL | 2022-07-17 16:24:37.747 | 2022-07-17 04:24:37.7486210 | -1467977271 | NULL | 2022-07-17 16:24:37.753 | 2022-07-17 04:24:37.7566145 | -1435977157 | NULL | 2022-07-17 16:24:37.760 | 2022-07-17 04:24:37.7626108 | 

QED 