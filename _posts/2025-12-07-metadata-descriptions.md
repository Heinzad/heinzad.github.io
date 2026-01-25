---
layout: post
tags: Metadata
title: "Describing Database Objects"
author: "Adam Heinz"
date: 2025-03-10 10:00:00
---
How to write table and column descriptions to support data validation.  


# Overview

Good descriptions make it easy to write data validation rules. 

Avoid restating information that is already in the name. Focus on describing intent so that you can verify the results achieved their intended purpose.


Table Descriptions
------------------


### Caption:

The full name in Proper Case supports searchability. Separate the caption from the rest of the description with an em-rule —  

Example: 

- "inv_hdr": "Invoice Header" 


### Purpose:

Summarise the table's function.  

Example:

- "Stores top-level information for an invoice containing one or more lines. 


Colummn Descriptions
--------------------


### Caption: 

The full name in Proper Case supports searchability. Separate the caption from the rest of the description with an em-rule —

Example: 

- "inv_no": "Invoice Number"


### Source, Calculations, Defaults: 

Where does the data come from? Is it entered by user or calculated? What is the logic of those calculations? Is there a default value? 

Example: 

- "Invoice Amount": "Provided by user"
- "Invoice Status": "working copy"
- "Invoice Date": Null by default
- "Sales Tax": "Automatically calculated by a trigger"


### Formats & Validation: 

Are there restrictions on values or can they accept all values of their data type? 

Example: 

- "Invoice Date" should not be backdated more than a month from the date of creation and not forward-dated more than one week from the date of creation. 
- "Invoice Amount" should be stored as an integer and displayed as a float. 

- "Invoice Status" accepts only predefined values


### Lookup Values: 

Enumerate a (small) list of accepted values. 

Example: 

- "W": "working copy"
- "A": "approved invoice"
- "C": "cancelled"


### Foreign Keys: 

Explain references. 

Example: 

- "cust_no" is a foreign key to "customers" table


### Lifecycle: 

When is a column expected to be populated? At insert or at a later stage in its lifecycle? Does the data ever get updated? Who or what updates it? What is its logic? 

Example: 

- "Invoice Number: "autonumber is generated when the invoice is created"


### Use: 

The may be data fields used by external programs, interfaces or reports that are not part of the principal application. 


Further Reading
---------------
  
Piotr Kononow (29 August 2017). ["Captain Obvious' Guide to Column Descriptions - Data Dictionary Best Practices"]( https://dataedo.com/blog/captain-obivous-guide-to-column-descriptions-data-dictionary-best-practices ), Dataedo.


QED 

© Adam Heinz 

8 December 2025