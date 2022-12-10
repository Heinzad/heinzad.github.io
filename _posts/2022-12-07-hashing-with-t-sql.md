Aide Memoire 

How to fingerprint records with hashing in sql server 
===================================================== 

Hashing is a technique that can be used to identify records. It has come to particular prominence with its use in Data Vault techniques. 

* Thanks to the GDPR, there has been a need to remove customer information without destroying a database. Building a customer business key from the hash of the customer's details means that the hash key can remain in the database even after the customer's details have been erased. 

* Hashing can assist paralell loading as we are assured of uniqueness and do not need to lookup any keys. 

One other scenario is in a data store or ods that is receiving json values from Kafka or an api. Hashing the json value lets us determine unique records and change detection (if that is not already provided in the data supply). 

## Example Table 
--- 

Take the "Who's Who" in the JD Edward Adress Book tables. The table definition and querying are provided at [JDE Tables](https://jde.erpref.com/?schema=920&system=&table=F0111). It's prefix is "WW".  

```tsql 

/*** Address Book - Who's Who ***/ 

CREATE TABLE [F0111] ( 

   [WWAN8] int NULL,            /* Address Number */ 
   [WWIDLN] int NULL,           /* Who's Who Line Number - ID */ 
   [WWDSS5] int NULL,           /* Display Sequence- 5.2 */ 
   [WWMLNM] nchar(40) NULL,     /* Name - Mailing */ 
   [WWATTL] nchar(40) NULL,     /* Professional Title */ 
   [WWREM1] nchar(40) NULL,     /* Remark 1 */ 
   [WWSLNM] nchar(40) NULL,     /* Salutation Name */ 
   [WWALPH] nchar(40) NULL,     /* Name - Alpha */ 
   [WWDC] nchar(40) NULL,       /* Description - Compressed */ 
   [WWGNNM] nchar(25) NULL,     /* Name - Given */ 
   [WWMDNM] nchar(25) NULL,     /* Name - Middle */ 
   [WWSRNM] nchar(25) NULL,     /* Name - Surname */ 
   [WWTYC] nchar(1) NULL,       /* Type Code */ 
   [WWW001] nchar(3) NULL,      /* Category Code - Who's Who 001 */ 
   [WWW002] nchar(3) NULL,      /* Category Code - Who's Who 002 */ 
   [WWW003] nchar(3) NULL,      /* Category Code - Who's Who 003 */ 
   [WWW004] nchar(3) NULL,      /* Category Code - Who's Who 004 */ 
   [WWW005] nchar(3) NULL,      /* Category Code - Who's Who 005 */ 
   [WWW006] nchar(3) NULL,      /* Category Code - Who's Who 006 */ 
   [WWW007] nchar(3) NULL,      /* Category Code - Who's Who 007 */ 
   [WWW008] nchar(3) NULL,      /* Category Code - Who's Who 008 */ 
   [WWW009] nchar(3) NULL,      /* Category Code - Whos Who 009 */ 
   [WWW010] nchar(3) NULL,      /* Category Code - Who's Who 010 */ 
   [WWMLN1] nchar(40) NULL,     /* Secondary Mailing Name */ 
   [WWALP1] nchar(40) NULL,     /* Secondary Alpha Name */ 
   [WWUSER] nchar(10) NULL,     /* User ID */ 
   [WWPID] nchar(10) NULL,      /* Program ID */ 
   [WWUPMJ] numeric(6) NULL,    /* Date - Updated */ 
   [WWJOBN] nchar(10) NULL,     /* Work Station ID */ 
   [WWUPMT] int NULL,           /* Time - Last Updated */ 
   [WWNTYP] nchar(3) NULL,      /* Contact Type */ 
   [WWNICK] nchar(40) NULL,     /* Nickname */ 
   [WWGEND] nchar(1) NULL,      /* Gender (Male/Female) */ 
   [WWDDATE] int NULL,          /* Day of Birth */ 
   [WWDMON] int NULL,           /* Month of Birth */ 
   [WWDYR] int NULL,            /* Year of Birth */ 
   [WWWN001] nchar(3) NULL,     /* Category Code - Contact 01 */ 
   [WWWN002] nchar(3) NULL,     /* Category Code - Contact 02 */ 
   [WWWN003] nchar(3) NULL,     /* Category Code - Contact 03 */ 
   [WWWN004] nchar(3) NULL,     /* Category Code - Contact 04 */ 
   [WWWN005] nchar(3) NULL,     /* Category Code - Contact 05 */ 
   [WWWN006] nchar(3) NULL,     /* Category Code - Contact 06 */ 
   [WWWN007] nchar(3) NULL,     /* Category Code - Contact 07 */ 
   [WWWN008] nchar(3) NULL,     /* Category Code - Contact 08 */ 
   [WWWN009] nchar(3) NULL,     /* Category Code - Contact 09 */ 
   [WWWN010] nchar(3) NULL,     /* Category Code - Contact 10 */ 
   [WWFUCO] nchar(10) NULL,     /* Function Code */ 
   [WWPCM] nchar(10) NULL,      /* Preferred Contact Method */ 
   [WWPCF] nchar(3) NULL,       /* Primary Contact */ 
   [WWACTIN] nchar(1) NULL,     /* Future Use Indicator */ 
   [WWCFRGUID] nchar(36) NULL,  /* Unique Identifier */ 
   [WWSYNCS] int NULL,          /* Synchronization Status */ 
   [WWCAAD] int NULL            /* Server Status */ 

);

```

## Jsonified record 
--- 

We can query the table to get its records in json format. If we perform this operation as an outer apply we can re-use the output:  

```tsql 

/*** return super key in json format ***/ 

SELECT   

     "ww".[WWAN8]       /* Address Number */ 
    ,"ww".[WWIDLN]      /* Who's Who Line Number - ID */ 
    ,"ww".[WWTYC]       /* Type Code */ 
    ,"ww".[WWCFRGUID]   /* Unique Identifier */ 
    
    ,"jj".[json_record]

FROM [F0111] as "ww" 

OUTER APPLY (   /* jsonified record */ 

    SELECT "json_record" = ( 
        SELECT 
         "ww".[WWAN8]   /* Address Number */ 
        ,"ww".[WWIDLN]  /* Who's Who Line Number - ID */ 
        ,"ww".[WWDSS5]  /* Display Sequence- 5.2 */ 
        ,"ww".[WWMLNM]  /* Name - Mailing */ 
        ,"ww".[WWATTL]  /* Professional Title */ 
        ,"ww".[WWREM1]  /* Remark 1 */ 
        ,"ww".[WWSLNM]  /* Salutation Name */ 
        ,"ww".[WWALPH]  /* Name - Alpha */ 
        ,"ww".[WWDC]    /* Description - Compressed */ 
        ,"ww".[WWGNNM]  /* Name - Given */ 
        ,"ww".[WWMDNM]  /* Name - Middle */ 
        ,"ww".[WWSRNM]  /* Name - Surname */ 
        ,"ww".[WWTYC]   /* Type Code */ 
        ,"ww".[WWW001]  /* Category Code - Who's Who 001 */ 
        ,"ww".[WWW002]  /* Category Code - Who's Who 002 */ 
        ,"ww".[WWW003]  /* Category Code - Who's Who 003 */ 
        ,"ww".[WWW004]  /* Category Code - Who's Who 004 */ 
        ,"ww".[WWW005]  /* Category Code - Who's Who 005 */ 
        ,"ww".[WWW006]  /* Category Code - Who's Who 006 */ 
        ,"ww".[WWW007]  /* Category Code - Who's Who 007 */ 
        ,"ww".[WWW008]  /* Category Code - Who's Who 008 */ 
        ,"ww".[WWW009]  /* Category Code - Whos Who 009 */ 
        ,"ww".[WWW010]  /* Category Code - Who's Who 010 */ 
        ,"ww".[WWMLN1]  /* Secondary Mailing Name */ 
        ,"ww".[WWALP1]  /* Secondary Alpha Name */ 
        ,"ww".[WWUSER]  /* User ID */ 
        ,"ww".[WWPID]   /* Program ID */ 
        ,"ww".[WWUPMJ]  /* Date - Updated */ 
        ,"ww".[WWJOBN]  /* Work Station ID */ 
        ,"ww".[WWUPMT]  /* Time - Last Updated */ 
        ,"ww".[WWNTYP]  /* Contact Type */ 
        ,"ww".[WWNICK]  /* Nickname */ 
        ,"ww".[WWGEND]  /* Gender (Male/Female) */ 
        ,"ww".[WWDDATE] /* Day of Birth */ 
        ,"ww".[WWDMON]  /* Month of Birth */ 
        ,"ww".[WWDYR]   /* Year of Birth */ 
        ,"ww".[WWWN001] /* Category Code - Contact 01 */ 
        ,"ww".[WWWN002] /* Category Code - Contact 02 */ 
        ,"ww".[WWWN003] /* Category Code - Contact 03 */ 
        ,"ww".[WWWN004] /* Category Code - Contact 04 */ 
        ,"ww".[WWWN005] /* Category Code - Contact 05 */ 
        ,"ww".[WWWN006] /* Category Code - Contact 06 */ 
        ,"ww".[WWWN007] /* Category Code - Contact 07 */ 
        ,"ww".[WWWN008] /* Category Code - Contact 08 */ 
        ,"ww".[WWWN009] /* Category Code - Contact 09 */ 
        ,"ww".[WWWN010] /* Category Code - Contact 10 */ 
        ,"ww".[WWFUCO]  /* Function Code */ 
        ,"ww".[WWPCM]   /* Preferred Contact Method */ 
        ,"ww".[WWPCF]   /* Primary Contact */ 
        ,"ww".[WWACTIN] /* Future Use Indicator */ 
        ,"ww".[WWCFRGUID] /* Unique Identifier */ 
        ,"ww".[WWSYNCS] /* Synchronization Status */ 
        ,"ww".[WWCAAD]  /* Server Status */ 
        FOR JSON_PATH, WITHOUT_ARRAY_WRAPPER, INCLUDE_NULLS 
    ) 

) as "jj"   /* jsonified record */ 

; 

``` 

Note that we did not need to use a where clause in the outer apply. We go without the array wrapper so that we do not have to deal with unnecessary brackets. We include null values to be certain we are getting all of the records, warts and all. 



## Json Super Key  
--- 

In effect, that json_record forms a superkey - a maximial combination of columns uniquely identifying a record. As it is absolutely unique, it is also useful for versioning. 

If we are pulling json records into a data store, versioning will assist us in detecting if: 
* a record is a new version of an existing one, and an update needs to be applied in the store; 
* a record is new and needs to be added to the data store; 
*or in detecting that a record is identical and no change needs to be made. 


## Version Key Hash 
--- 

We can create a version key by hashing the json super key. Hashing has been a controversial topic among data vault practitioners because of the possibility of collisions. Using a stronger cryptographic has minimises the possibility of collisions and minimises the possibility of deducing the contents of the hash. 

We minimise storage sizes by returning the hash in binary format. 

Let's revisit the above example: 

```tsql 

/*** return version key hash in binary format ***/ 

SELECT   

     "ww".[WWAN8]       /* Address Number */ 
    ,"ww".[WWIDLN]      /* Who's Who Line Number - ID */ 
    ,"ww".[WWTYC]       /* Type Code */ 
    ,"ww".[WWCFRGUID]   /* Unique Identifier */     
    
    ,"vkh".[version_key_hash]

FROM [F0111] as "ww" 

OUTER APPLY (   /* version key */ 

    SELECT "version_key_hash" = TRY_CONVERT( 
        binary(65), 
        HASHBYTES(
            'SHA2_512', 
            ( 
                SELECT 
                "ww".[WWAN8]    /* Address Number */ 
                ,"ww".[WWIDLN]  /* Who's Who Line Number - ID */ 
                ,"ww".[WWDSS5]  /* Display Sequence- 5.2 */ 
                ,"ww".[WWMLNM]  /* Name - Mailing */ 
                ,"ww".[WWATTL]  /* Professional Title */ 
                ,"ww".[WWREM1]  /* Remark 1 */ 
                ,"ww".[WWSLNM]  /* Salutation Name */ 
                ,"ww".[WWALPH]  /* Name - Alpha */ 
                ,"ww".[WWDC]    /* Description - Compressed */ 
                ,"ww".[WWGNNM]  /* Name - Given */ 
                ,"ww".[WWMDNM]  /* Name - Middle */ 
                ,"ww".[WWSRNM]  /* Name - Surname */ 
                ,"ww".[WWTYC]   /* Type Code */ 
                ,"ww".[WWW001]  /* Category Code - Who's Who 001 */ 
                ,"ww".[WWW002]  /* Category Code - Who's Who 002 */ 
                ,"ww".[WWW003]  /* Category Code - Who's Who 003 */ 
                ,"ww".[WWW004]  /* Category Code - Who's Who 004 */ 
                ,"ww".[WWW005]  /* Category Code - Who's Who 005 */ 
                ,"ww".[WWW006]  /* Category Code - Who's Who 006 */ 
                ,"ww".[WWW007]  /* Category Code - Who's Who 007 */ 
                ,"ww".[WWW008]  /* Category Code - Who's Who 008 */ 
                ,"ww".[WWW009]  /* Category Code - Whos Who 009 */ 
                ,"ww".[WWW010]  /* Category Code - Who's Who 010 */ 
                ,"ww".[WWMLN1]  /* Secondary Mailing Name */ 
                ,"ww".[WWALP1]  /* Secondary Alpha Name */ 
                ,"ww".[WWUSER]  /* User ID */ 
                ,"ww".[WWPID]   /* Program ID */ 
                ,"ww".[WWUPMJ]  /* Date - Updated */ 
                ,"ww".[WWJOBN]  /* Work Station ID */ 
                ,"ww".[WWUPMT]  /* Time - Last Updated */ 
                ,"ww".[WWNTYP]  /* Contact Type */ 
                ,"ww".[WWNICK]  /* Nickname */ 
                ,"ww".[WWGEND]  /* Gender (Male/Female) */ 
                ,"ww".[WWDDATE] /* Day of Birth */ 
                ,"ww".[WWDMON]  /* Month of Birth */ 
                ,"ww".[WWDYR]   /* Year of Birth */ 
                ,"ww".[WWWN001] /* Category Code - Contact 01 */ 
                ,"ww".[WWWN002] /* Category Code - Contact 02 */ 
                ,"ww".[WWWN003] /* Category Code - Contact 03 */ 
                ,"ww".[WWWN004] /* Category Code - Contact 04 */ 
                ,"ww".[WWWN005] /* Category Code - Contact 05 */ 
                ,"ww".[WWWN006] /* Category Code - Contact 06 */ 
                ,"ww".[WWWN007] /* Category Code - Contact 07 */ 
                ,"ww".[WWWN008] /* Category Code - Contact 08 */ 
                ,"ww".[WWWN009] /* Category Code - Contact 09 */ 
                ,"ww".[WWWN010] /* Category Code - Contact 10 */ 
                ,"ww".[WWFUCO]  /* Function Code */ 
                ,"ww".[WWPCM]   /* Preferred Contact Method */ 
                ,"ww".[WWPCF]   /* Primary Contact */ 
                ,"ww".[WWACTIN] /* Future Use Indicator */ 
                ,"ww".[WWCFRGUID] /* Unique Identifier */ 
                ,"ww".[WWSYNCS] /* Synchronization Status */ 
                ,"ww".[WWCAAD]  /* Server Status */ 
                FOR JSON_PATH, WITHOUT_ARRAY_WRAPPER, INCLUDE_NULLS  
            ) 
        ) 
    ) 

) as "vkh"      /* version key */ 

; 


``` 

## Version Key No 
--- 

The version key hash provides a single-column encrypted value for the superkey, but if you are working in a database you will be left wondering how this gets sorted in queries or indexes. 

We can help address the question of sorting by using the lowest level of hashing to get a numeric value that is easy to sort in ascending order. If there are any collisions, the db only has to sort through a small set of binary values. 

We will need to use a fill factor in any indexing if we want to avoid page splits. 

Let's expand that code sample further to get a version key number: 

```tsql 

/*** return version key number as integer & hash in binary format ***/ 

SELECT  

     "ww".[WWAN8]       /* Address Number */ 
    ,"ww".[WWIDLN]      /* Who's Who Line Number - ID */ 
    ,"ww".[WWTYC]       /* Type Code */ 
    ,"ww".[WWCFRGUID]   /* Unique Identifier */ 
    
    ,"version_key_no" = ABS(CHECKSUM( "vkh".[version_key_hash] ))
    
    ,"vkh".[version_key_hash]

FROM [F0111] as "ww" 

OUTER APPLY (   /* version key */ 

    SELECT "version_key_hash" = TRY_CONVERT( 
        binary(65), 
        HASHBYTES(
            'SHA2_512', 
            ( 
                SELECT 
                "ww".[WWAN8]    /* Address Number */ 
                ,"ww".[WWIDLN]  /* Who's Who Line Number - ID */ 
                ,"ww".[WWDSS5]  /* Display Sequence- 5.2 */ 
                ,"ww".[WWMLNM]  /* Name - Mailing */ 
                ,"ww".[WWATTL]  /* Professional Title */ 
                ,"ww".[WWREM1]  /* Remark 1 */ 
                ,"ww".[WWSLNM]  /* Salutation Name */ 
                ,"ww".[WWALPH]  /* Name - Alpha */ 
                ,"ww".[WWDC]    /* Description - Compressed */ 
                ,"ww".[WWGNNM]  /* Name - Given */ 
                ,"ww".[WWMDNM]  /* Name - Middle */ 
                ,"ww".[WWSRNM]  /* Name - Surname */ 
                ,"ww".[WWTYC]   /* Type Code */ 
                ,"ww".[WWW001]  /* Category Code - Who's Who 001 */ 
                ,"ww".[WWW002]  /* Category Code - Who's Who 002 */ 
                ,"ww".[WWW003]  /* Category Code - Who's Who 003 */ 
                ,"ww".[WWW004]  /* Category Code - Who's Who 004 */ 
                ,"ww".[WWW005]  /* Category Code - Who's Who 005 */ 
                ,"ww".[WWW006]  /* Category Code - Who's Who 006 */ 
                ,"ww".[WWW007]  /* Category Code - Who's Who 007 */ 
                ,"ww".[WWW008]  /* Category Code - Who's Who 008 */ 
                ,"ww".[WWW009]  /* Category Code - Whos Who 009 */ 
                ,"ww".[WWW010]  /* Category Code - Who's Who 010 */ 
                ,"ww".[WWMLN1]  /* Secondary Mailing Name */ 
                ,"ww".[WWALP1]  /* Secondary Alpha Name */ 
                ,"ww".[WWUSER]  /* User ID */ 
                ,"ww".[WWPID]   /* Program ID */ 
                ,"ww".[WWUPMJ]  /* Date - Updated */ 
                ,"ww".[WWJOBN]  /* Work Station ID */ 
                ,"ww".[WWUPMT]  /* Time - Last Updated */ 
                ,"ww".[WWNTYP]  /* Contact Type */ 
                ,"ww".[WWNICK]  /* Nickname */ 
                ,"ww".[WWGEND]  /* Gender (Male/Female) */ 
                ,"ww".[WWDDATE] /* Day of Birth */ 
                ,"ww".[WWDMON]  /* Month of Birth */ 
                ,"ww".[WWDYR]   /* Year of Birth */ 
                ,"ww".[WWWN001] /* Category Code - Contact 01 */ 
                ,"ww".[WWWN002] /* Category Code - Contact 02 */ 
                ,"ww".[WWWN003] /* Category Code - Contact 03 */ 
                ,"ww".[WWWN004] /* Category Code - Contact 04 */ 
                ,"ww".[WWWN005] /* Category Code - Contact 05 */ 
                ,"ww".[WWWN006] /* Category Code - Contact 06 */ 
                ,"ww".[WWWN007] /* Category Code - Contact 07 */ 
                ,"ww".[WWWN008] /* Category Code - Contact 08 */ 
                ,"ww".[WWWN009] /* Category Code - Contact 09 */ 
                ,"ww".[WWWN010] /* Category Code - Contact 10 */ 
                ,"ww".[WWFUCO]  /* Function Code */ 
                ,"ww".[WWPCM]   /* Preferred Contact Method */ 
                ,"ww".[WWPCF]   /* Primary Contact */ 
                ,"ww".[WWACTIN] /* Future Use Indicator */ 
                ,"ww".[WWCFRGUID] /* Unique Identifier */ 
                ,"ww".[WWSYNCS] /* Synchronization Status */ 
                ,"ww".[WWCAAD]  /* Server Status */ 
                FOR JSON_PATH, WITHOUT_ARRAY_WRAPPER, INCLUDE_NULLS  
            ) 
        ) 
    ) 

) as "vkh"      /* version key */ 

; 


``` 
The json format was handy because all values are expressed as text, and even a change in the source column name will be detected. 

## Natural Business Key 
--- 

Business Keys are a big topic in integrating data across an enterprise. Json formatting is not handy for this because column names may differ between systems and over time. That means we will have to resolve how to contatenate all the values into a single text column. We will separate them with a pipe "|" to ensure this is a human-readable value. Pipe delimiters at the beginning and end are more a matter of taste. 

If we return to our example of [F0111], "Who Is Who" in the JDE Address Book, we might be able to find the beginnings of a customer business key in the Given, Middle, and Surname, plus the day, month, and year of birth. With an eye on the future, we will need to use nvarchar values to handle names with macrons, etc, even if they are not stored in the current database at the present time. 

```tsql 

/*** return natural business key as a human-readable text string ***/ 

SELECT   

     "ww".[WWAN8]       /* Address Number */ 
    ,"ww".[WWIDLN]      /* Who's Who Line Number - ID */ 
    ,"ww".[WWTYC]       /* Type Code */ 
    ,"ww".[WWCFRGUID]   /* Unique Identifier */  
    
    ,"bkn".[customer_business_key_txt]

FROM [F0111] as "ww" 

OUTER APPLY (   /* business key */ 

    SELECT "customer_business_key_txt" = CONVERT(nvarchar(500), 
        CHAR(124) +  LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWGNNM])))    /* Name - Given */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWMDNM])))   /* Name - Middle */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWSRNM])))   /* Name - Surname */   
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWDDATE])))  /* Day of Birth */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar, "ww".[WWDMON])))        /* Month of Birth */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar, "ww".[WWDYR])))         /* Year of Birth */ 
        + CHAR(124) 
    ) 

) as "cbk"      /* business key */ 

; 


``` 

Roelant Vos has written that [a natural business key is sufficient](https://roelantvos.com/blog/using-a-natural-business-key-the-end-of-hash-keys/), but in the example above, we had to use a column width that precludes its use in the principal clause of an index. 

As long as you can afford the storage space involved, there is something to be said for using every option available, by using both a natural business key and a hash of the business key. 


```tsql 

/*** return natural and hashed business key ***/ 

SELECT   

     "ww".[WWAN8]       /* Address Number */ 
    ,"ww".[WWIDLN]      /* Who's Who Line Number - ID */ 
    ,"ww".[WWTYC]       /* Type Code */ 
    ,"ww".[WWCFRGUID]   /* Unique Identifier */ 
    
    ,"bkn".[customer_business_key_txt] 
    
    ,"customer_business_key_no" = ABS(CHECKSUM( TRY_CONVERT(binary(65), HASHBYTES('SHA2_512', "bkn".[customer_business_key_txt] ))))  
    
    ,"customer_business_key_hash" = TRY_CONVERT(binary(65), HASHBYTES('SHA2_512', "bkn".[customer_business_key_txt] )) 

FROM [F0111] as "ww" 

OUTER APPLY (   /* business key */ 

    SELECT "customer_business_key_txt" = CONVERT(nvarchar(500), 
          CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWGNNM])))   /* Name - Given */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWMDNM])))   /* Name - Middle */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWSRNM])))   /* Name - Surname */   
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWDDATE])))  /* Day of Birth */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar, "ww".[WWDMON])))        /* Month of Birth */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar, "ww".[WWDYR])))         /* Year of Birth */ 
        + CHAR(124) 
    ) 

) as "cbk"      /* business key */ 

; 


``` 

Alternatively, we could run the checksum directly over the natural business key. 


## Shard Key 
--- 

If we are sharding our database, we now have a way to get a repeatable shard key with the modulo of the business key no. (Note that we cannot do this on the version key as that incorporated column names to assist with change detection). 

The value of the modulo will reflect the maximum number of shards: 


```tsql 

/*** return shard key, business key, and version key ***/ 

SELECT   

     "ww".[WWAN8]       /* Address Number */ 
    ,"ww".[WWIDLN]      /* Who's Who Line Number - ID */ 
    ,"ww".[WWTYC]       /* Type Code */ 
    ,"ww".[WWCFRGUID]   /* Unique Identifier */ 
    
    ,"bkn".[customer_business_key_txt] 
    
    ,"shard_key_no" = ABS(CHECKSUM( TRY_CONVERT(binary(65), HASHBYTES('SHA2_512', "bkn".[customer_business_key_txt] )))) % 128 
    
    ,"customer_business_key_no" = ABS(CHECKSUM( TRY_CONVERT(binary(65), HASHBYTES('SHA2_512', "bkn".[customer_business_key_txt] ))))  
    
    ,"customer_business_key_hash" = TRY_CONVERT(binary(65), HASHBYTES('SHA2_512', "bkn".[customer_business_key_txt] )) 

    ,"version_key_no" = ABS(CHECKSUM( "vkh".[version_key_hash] ))
    
    ,"vkh".[version_key_hash] 

FROM [F0111] as "ww" 

OUTER APPLY (   /* business key */ 

    SELECT "customer_business_key_txt" = CONVERT(nvarchar(500), 
          CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWGNNM])))   /* Name - Given */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWMDNM])))   /* Name - Middle */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWSRNM])))   /* Name - Surname */   
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar(100), "ww".[WWDDATE])))  /* Day of Birth */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar, "ww".[WWDMON])))        /* Month of Birth */ 
        + CHAR(124) + LTRIM(RTRIM(CONVERT(nvarchar, "ww".[WWDYR])))         /* Year of Birth */ 
        + CHAR(124) 
    ) 

) as "cbk"      /* business key */ 

OUTER APPLY (   /* version key */ 

    SELECT "version_key_hash" = TRY_CONVERT( 
        binary(65), 
        HASHBYTES(
            'SHA2_512', 
            ( 
                SELECT 
                "ww".[WWAN8]    /* Address Number */ 
                ,"ww".[WWIDLN]  /* Who's Who Line Number - ID */ 
                ,"ww".[WWDSS5]  /* Display Sequence- 5.2 */ 
                ,"ww".[WWMLNM]  /* Name - Mailing */ 
                ,"ww".[WWATTL]  /* Professional Title */ 
                ,"ww".[WWREM1]  /* Remark 1 */ 
                ,"ww".[WWSLNM]  /* Salutation Name */ 
                ,"ww".[WWALPH]  /* Name - Alpha */ 
                ,"ww".[WWDC]    /* Description - Compressed */ 
                ,"ww".[WWGNNM]  /* Name - Given */ 
                ,"ww".[WWMDNM]  /* Name - Middle */ 
                ,"ww".[WWSRNM]  /* Name - Surname */ 
                ,"ww".[WWTYC]   /* Type Code */ 
                ,"ww".[WWW001]  /* Category Code - Who's Who 001 */ 
                ,"ww".[WWW002]  /* Category Code - Who's Who 002 */ 
                ,"ww".[WWW003]  /* Category Code - Who's Who 003 */ 
                ,"ww".[WWW004]  /* Category Code - Who's Who 004 */ 
                ,"ww".[WWW005]  /* Category Code - Who's Who 005 */ 
                ,"ww".[WWW006]  /* Category Code - Who's Who 006 */ 
                ,"ww".[WWW007]  /* Category Code - Who's Who 007 */ 
                ,"ww".[WWW008]  /* Category Code - Who's Who 008 */ 
                ,"ww".[WWW009]  /* Category Code - Whos Who 009 */ 
                ,"ww".[WWW010]  /* Category Code - Who's Who 010 */ 
                ,"ww".[WWMLN1]  /* Secondary Mailing Name */ 
                ,"ww".[WWALP1]  /* Secondary Alpha Name */ 
                ,"ww".[WWUSER]  /* User ID */ 
                ,"ww".[WWPID]   /* Program ID */ 
                ,"ww".[WWUPMJ]  /* Date - Updated */ 
                ,"ww".[WWJOBN]  /* Work Station ID */ 
                ,"ww".[WWUPMT]  /* Time - Last Updated */ 
                ,"ww".[WWNTYP]  /* Contact Type */ 
                ,"ww".[WWNICK]  /* Nickname */ 
                ,"ww".[WWGEND]  /* Gender (Male/Female) */ 
                ,"ww".[WWDDATE] /* Day of Birth */ 
                ,"ww".[WWDMON]  /* Month of Birth */ 
                ,"ww".[WWDYR]   /* Year of Birth */ 
                ,"ww".[WWWN001] /* Category Code - Contact 01 */ 
                ,"ww".[WWWN002] /* Category Code - Contact 02 */ 
                ,"ww".[WWWN003] /* Category Code - Contact 03 */ 
                ,"ww".[WWWN004] /* Category Code - Contact 04 */ 
                ,"ww".[WWWN005] /* Category Code - Contact 05 */ 
                ,"ww".[WWWN006] /* Category Code - Contact 06 */ 
                ,"ww".[WWWN007] /* Category Code - Contact 07 */ 
                ,"ww".[WWWN008] /* Category Code - Contact 08 */ 
                ,"ww".[WWWN009] /* Category Code - Contact 09 */ 
                ,"ww".[WWWN010] /* Category Code - Contact 10 */ 
                ,"ww".[WWFUCO]  /* Function Code */ 
                ,"ww".[WWPCM]   /* Preferred Contact Method */ 
                ,"ww".[WWPCF]   /* Primary Contact */ 
                ,"ww".[WWACTIN] /* Future Use Indicator */ 
                ,"ww".[WWCFRGUID] /* Unique Identifier */ 
                ,"ww".[WWSYNCS] /* Synchronization Status */ 
                ,"ww".[WWCAAD]  /* Server Status */ 
                FOR JSON_PATH, WITHOUT_ARRAY_WRAPPER, INCLUDE_NULLS  
            ) 
        ) 
    ) 

) as "vkh"      /* version key */ 

; 


``` 



QED 

© Adam Heinz 

7 December 2022 
