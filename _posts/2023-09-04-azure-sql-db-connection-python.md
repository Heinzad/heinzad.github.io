AIDE MEMOIRE 

# How to connect to an Azure SQL Database using Python 

It is a bit of a puzzle to connect to an Azure SQL Database as easily as in SQL Server Management Studio (SSMS), using Active Directory with MFA. 

## Install pyodbc 

First, install pyodbc, if you don't have it: 

```python

    pip install pyodbc 

``` 

## File connections.py 

Second, create a new file called "connections.py" 


## Stringify Connection  

Third, build a connection string, taking security into consideration by not hard-coding business settings: 

```python 

import pyodbc #driver for sql server 

def get_azure_sql_db_mfa_connection_string(
        username=None,
        server=None, 
        database=None,
        port='1433',
        authn='ActiveDirectoryInteractive', 
        driver='{ODBC Driver 17 for SQL Server}'
    ): 
        """Returns a string for connecting to a given Azure SQL Database with Multi-Factor Authentication""" 
        
        if username==None: 
            username = input("user name: ") 
        if server==None: 
            server = input("server name: ") 
        if database==None: 
            database = input("database name: ") 
        
        connection_string = f"DRIVER={driver};SERVER={server};PORT={port};DATABASE={database};UID={username};AUTHENTICATION={authn}" 

        return connection_string 

``` 
N.B. More recent Microsoft documentation on similar topics uses '{ODBC Driver 18 for SQL Server}', but at the time of writing that does not seem to work with MFA. 


## Call the connection 

Now, create another file "app.py" from which to call the function: 

```python 

    from connections import * 

    connection_string = get_azure_sql_db_mfa_connection_string() 
    conn = pyodbc.connect(connection_string)

    qry = """SELECT TOP 10 * FROM INFORMATION_SCHEMA.TABLES""" 

    cursor = conn.cursor() 
    cursor.execute(qry) 

    records = cursor.fetchall()
    for row in records: 
        print(f"{row.TABLE_NAME}") 

``` 


QED 

© Adam Heinz 

5 September 2023 
