# teradataml Utility and General Functions

* Data Transfer Utility
* Database Utility
* File Management Functions
* Miscellaneous Functions
* Apply SET Operations on teradataml DataFrames
## Data Transfer Utility
* Saving DataFrame to Vantage
* Loading Data from CSV to Vantage
* Data Transfer between Vantage and Cloud Storage
### Saving DataFrame to Vantage
* copy_to_sql()
* fastload()
* fastexport()
#### copy_to_sql()
Use the copy_to_sql() function to create a table in Vantage based on a teradataml DataFrame or a
pandas DataFrame.
The function takes a teradataml DataFrame or a pandas DataFrame and a table name as arguments, and
generates DDL and DML commands that creates a table in Vantage. You can also specify the name of the
schema in the database to write the table to. If no schema name is specified, the default database schema
is used.
Required Arguments:
* df: Specifies the Pandas or teradataml DataFrame object to be saved.
* table_name: Specifies the name of the table to be created in Vantage.
Optional Arguments:
* schema_name: Specifies the name of the SQL schema in Vantage to write to.
* if_exists: Specifies the action to take when table already exists in the database.
  Possible values are:
  * 'fail': If table exists, do nothing;
* 'replace': If table exists, drop it, recreate it, and insert data;
  * 'append': If table exists, insert data. Create if does not exist.
  The default value is 'append'.
  Note:
    Replacing a table with the contents of a teradataml DataFrame based on the same underlying
    table is not supported.
* index: Specifies whether to save pandas DataFrame index as a column or not. Possible values are
  True or False.
  The default value is False.
  Note:
    Only use as True when attempting to save Pandas DataFrames (and not on
    teradataml DataFrames).
* index_label: Specifies the column labels for pandas DataFrame index columns. If no value is given
  and index is True, then a default label 'index_label' is used.
  The default value is None.
  Note:
    Only use this argument when attempting to save Pandas DataFrames (and not on
    teradataml DataFrames).
  Note:
    If this argument is not specified (defaulted to None or is empty) and argument index is set to
    True, then the names property of the DataFrames index is used as the label(s), and if that too
    is None or empty, then:
    * A default label 'index_label' or 'level_0' (when 'index_label' is already taken) is used when
    index is standard.
    * Default labels 'level_0', 'level_1', etc. are used when index is multi-level index.
* primary_index: Specifies the column(s) to use as primary index while creating tables in Vantage.
  When None (default value), No Primary Index (NOPI) tables are created.
  For example:
  * primary_index = 'my_primary_index'
  * primary_index = ['my_primary_index1', 'my_primary_index2', 'my_primary_index3']
* temporary: Specifies whether to create Vantage tables as permanent or volatile.
Possible values are:
  * True: Creates volatile tables, and schema_name is ignored.
  * False: Creates permanent tables.
  The default value is False.
* types: Specifies required data types for requested columns to be saved in Vantage.
  The default value is None.
  This argument accepts a dictionary of column names and their required teradatasqlalchemy types as
  key-value pairs, allowing to specify a subset of the columns of a specific type.
  Note:
    * When only a subset of all columns are provided, the rest are defaulted to appropriate types.
    * When types argument is not provided, all column types are appropriately assigned and
    defaulted. The column types are assigned as listed in the following table.
| pandas/NumPy Type | teradatasqlalchemy Type |
| ----------------- | ----------------------- |
| int32 | INTEGER |
| int64 | BIGINT |
| bool | BYTEINT |
| float32/float64 | FLOAT |
| datetime64/datatime64[ns] | TIMESTAMP |
| datatime64[ns, time_zone | ] TIMESTAMP(time_zone=True) |
| Any other data type | VARCHAR(configure.default_varchar_size) |

* primary_time_index_name: Specifies a name for the Primary Time Index (PTI) when the table to be
  created must be a PTI table.
  Note:
    This argument is not required or used when the table to be created is not a PTI table. It is ignored
    if specified without the argument timecode_column.
* timecode_column: Specifies the column in the DataFrame that reflects the form of the timestamp
  data in the time series.
  Note:
    This argument is required when the DataFrame must be saved as a PTI table.
This column is the TD_TIMECODE column in the table created. It must be of the SQL type
  TIMESTAMP(n), TIMESTAMP(n) WITH TIME ZONE, or DATE, corresponding to the Python types
  datetime.datetime or datetime.date, or Pandas dtype datetime64[ns].
  Note:
    This argument is not required when the table to be created is not a PTI table. When specifying
    this argument, an attempt to create a PTI table is made. If this argument is specified,
    primary_index is ignored.
* timezero_date: Specifies the earliest time series data that the PTI accepts; a date that precedes the
  earliest date in the time series data. Value specified must be of the following format: DATE 'YYYY-
  MM-DD'.
  The default value is: DATE '1970-01-01'.
  Note:
    * This argument is used when the DataFrame must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column argument.
* timebucket_duration: Specifies a duration that serves to break up the time continuum in the time
  series data into discrete groups or buckets.
  Specified using the formal form time_unit(n), where n is a positive integer, and time_unit can be any
  of the following: CAL_YEARS, CAL_MONTHS, CAL_DAYS, WEEKS, DAYS, HOURS, MINUTES,
  SECONDS, MILLISECONDS, OR MICROSECONDS.
  Note:
    * This argument is required if columns_list is not specified or is empty.
    * This argument is used when the DataFrame must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column argument.
* columns_list: Specifies a list of one or more PTI table column names.
  Note:
    * This argument is required if timebucket_duration is not specified or is empty.
    * This argument is used when the DataFrame must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column argument.
* sequence_column: Specifies the column of type Integer containing the unique identifier for time
  series data reading when they are not unique in time.
  * When specified, implies SEQUENCED, meaning more than one reading from the same sensor
    may have the same timestamp. This column is the TD_SEQNO column in the table created.
  * When not specified, implies NONSEQUENCED, meaning there is only one sensor reading per
    timestamp. This is the default.
  Note:
    * This argument is used when the DataFrame must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column argument.
* seq_max: Specifies the maximum number of sensor data rows that can have the same timestamp.
  Can be used when 'sequenced' is True.
  Accepted values range from 1 to 2147483647, with default value 20000.
  Note:
    * This argument is used when the DataFrame must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column argument.
* set_table: Specifies a flag to determine whether to create a SET or a MULTISET table.
  * When True, a SET table is created.
  * When False, a MULTISET table is created. This is the default value.
  Note:
    * Specifying set_table=True also requires specifying primary_index or timecode_column.
    * Creating SET table (set_table=True) may result in
    ▪ an error if the source is a Pandas DataFrame having duplicate rows;
    ▪ loss of duplicate rows if the source is a teradataml DataFrame.
    * This argument has no effect if the table already exists and if_exists='append'.
* match_column_order: Specifies whether the order of the columns in existing table matches the order
  of the columns in the "df" or not.
  When set to False, the DataFrame to be loaded can have any order and number of columns.
  The default value is True.
* chunksize: Specifies the number of rows to be loaded in a batch, if DataFrame is pandas DataFrame.
The default value is 16383.
#### Example 1: Create a new table in Vantage based on a pandas DataFrame
You must import pandas and copy_to_sql first.
```python
>>>import pandas as pd
>>>from teradataml.dataframe.copy_to import copy_to_sql
```
Assume you create the "sales" table as follows:
```python
>>>sales = [{'accounts': 'Jones LLC', 'Jan': 150, 'Feb': 200.0, 'Mar':
140, 'datetime':'2017-04-01'},
{'accounts': 'Alpha Co',  'Jan': 200, 'Feb': 210.0, 'Mar': 215,
'datetime': '2017-04-01'},
{'accounts': 'Blue Inc',  'Jan': 50,  'Feb': 90.0,  'Mar': 95,
'datetime': '2017-04-01' }]
```
And a pandas DataFrame "pdf" is created from the table "sales", using command:
```python
pdf = pd.DataFrame(sales)
```
Enter pdf to display the pandas DataFrame:
```python
>>>pdf
Feb  Jan  Mar   accounts    datetime
0  200.0  150  140  Jones LLC  2017-04-01
1  210.0  200  215   Alpha Co  2017-04-01
2   90.0   50   95   Blue Inc  2017-04-01
```
Use the copy_to_sql() function to create a new Vantage table "pandas_sales" based on the pandas
DataFrame "pdf" that is created in the prerequisite.
```python
>>>copy_to_sql(df = pdf, table_name = "pandas_sales",
primary_index="accounts", if_exists="replace")
```
Verify that the new table "pandas_sales" exists in Vantage using the DataFrame() function which creates
a DataFrame based on an existing table.
```python
>>>df = DataFrame("pandas_sales")
>>> df
Feb  Jan  Mar                    datetime
accounts
Jones LLC  200.0  150  140  2017-04-01 00:00:00.000000
Alpha Co   210.0  200  215  2017-04-01 00:00:00.000000
Blue Inc    90.0   50   95  2017-04-01 00:00:00.000000
```
#### Example 2: Use the optional index and index_label parameters, and use the
#### index as Primary Index
```python
>>> copy_to_sql(df = pdf, table_name = "pandas_sales", index=True,
index_label="idx", primary_index="idx", if_exists="replace")
>>> df = DataFrame("pandas_sales")
>>> df
Feb  Jan  Mar   accounts                    datetime
idx
0    200.0  150  140  Jones LLC  2017-04-01 00:00:00.000000
2     90.0   50   95   Blue Inc  2017-04-01 00:00:00.000000
1    210.0  200  215   Alpha Co  2017-04-01 00:00:00.000000
```
#### Example 3: Create a new table in Vantage based on a teradataml DataFrame
```python
>>> df = DataFrame("sales")
>>> df
Feb   Jan   Mar   Apr    datetime
accounts
Blue Inc     90.0    50    95   101  04/01/2017
Alpha Co    210.0   200   215   250  04/01/2017
Jones LLC   200.0   150   140   180  04/01/2017
Yellow Inc   90.0  None  None  None  04/01/2017
Orange Inc  210.0  None  None   250  04/01/2017
Red Inc     200.0   150   140  None  04/01/2017
>>> copy_to_sql(df = df, table_name = "sales_df1",
primary_index="accounts", if_exists="replace")
>>> df1 = DataFrame("sales_df1")
>>> df1
Feb   Jan   Mar   Apr  datetime
accounts
Blue Inc     90.0    50    95   101  17/01/04
Orange Inc  210.0  None  None   250  17/01/04
Red Inc     200.0   150   140  None  17/01/04
Yellow Inc   90.0  None  None  None  17/01/04
Jones LLC   200.0   150   140   180  17/01/04
Alpha Co    210.0   200   215   250  17/01/04
```
#### Example 4: Create a new Primary Time Index Table in Vantage from a
#### Pandas DataFrame
```python
>>> copy_to_sql(df = pdf, table_name = "pandas_sales_pti",
timecode_column="datetime", columns_list="accounts")
>>> pandas_sales_pti = DataFrame('pandas_sales_pti')
>>> pandas_sales_pti
TD_TIMECODE    Feb  Jan  Mar
accounts
Jones LLC  2017-04-01 00:00:00.000000  200.0  150  140
Alpha Co   2017-04-01 00:00:00.000000  210.0  200  215
Blue Inc   2017-04-01 00:00:00.000000   90.0   50   95
```
Note:
See also to_sql() DataFrame Method.
#### Example 5: Save a Pandas DataFrame with VECTOR datatype
```python
>>> import pandas as pd
>>> VECTOR_data = {
... 'id': [10, 11, 12, 13],
... 'array_col': ['1,1', '2,2', '3,3', '4,4']
... }
>>> df = pd.DataFrame(VECTOR_data)
>>> from teradatasqlalchemy import VECTOR
>>> copy_to_sql(df=df, table_name='my_vector_table',
types={'array_col': VECTOR})
>>>
```
#### fastload()
The fastload() function writes records from a Pandas DataFrame to Vantage using Fastload, and can be
used to quickly load large amounts of data in an empty table on Vantage.
FastLoad opens multiple data transfer connections to the database. The number of data transfer sessions
can be set using argument open_sessions. If this argument is not set, by default, data transfer sessions
opened by teradataml is the smaller of 8 and the number of available AMPs in Vantage.
Teradata recommends using fastload() function when number of rows in the Pandas DataFrame is greater
than 100,000 for better performance. To insert lesser rows, you can use the copy_to_sql() function for
optimized performance. The data is loaded in batches.
Note:
* FastLoad API cannot load duplicate rows in the DataFrame if the table is a MULTISET table with
    Primary Index.
* FastLoad API does not support all Database Engine 20 data types.
    For example, target table having BLOB and CLOB data type columns cannot be loaded.
* If there are any incorrect rows due to constraint violations, data type conversion errors, etc.,
    FastLoad protocol ignores those rows and inserts all valid rows.
* Rows in the DataFrame that failed to get inserted are categorized into errors and warnings by
    FastLoad protocol and these errors and warnings are stored into respective error and warning
    tables by FastLoad API.
* fastload() creates 2 error tables when data is erroneous. These error tables are referred to as
    ERR_1 and ERR_2 tables.
    * ERR_1 table captures rows that violate the constraints or have format errors. It typically
    contains information about rows that could not be inserted into the target table due to data
    conversion errors, constraint violations, and so on.
    * ERR_2 table logs any duplicate rows found during the load process and which are not
    loaded in target table, since fastLoad does not allow duplicate rows to be loaded into the
    target table.
* When save_errors argument is set to True, ERR_1 and ERR_2 tables are persisted. The fully
    qualified names of ERR_1, ERR_2 and warning tables are shown once the fastload operation
    is complete.
* If user wants both error and warnings information from pandas dataframe to be persisted
    rather than that from ERR_1 and ERR_2 tables, then save_errors should be set to True and
    err_tbl_name must be provided.
See the FastLoad section of https://pypi.org/project/teradatasql/ for more information about FastLoad
protocol through teradatasql driver.
#### Arguments
Required Arguments:
* df: Specifies the pandas or teradataml DataFrame object to be saved in Vantage.
* table_name: Specifies the name of the table to be created in Vantage.
Optional Arguments:
* schema_name: Specifies the name of the SQL schema in Vantage to write to.
  The default value is None, which means use the default Vantage schema.
* if_exists: Specifies the action to take when table already exists in the database.
  Possible values are:
  * 'fail': If table exists, do nothing;
  * 'replace': If table exists, drop it, recreate it, and insert data;
  * 'append': If table exists, insert data. Create if does not exist.
  The default value is 'replace'.
* index: Specifies whether to save pandas DataFrame index as a column or not. Possible values are
  True or False.
  The default value is False.
* index_label: Specifies the column labels for pandas DataFrame index columns.
  The default value is None.
* primary_index: Specifies the column(s) to use as primary index while creating tables in Vantage.
  When None (default value), No Primary Index (NOPI) tables are created.
  For example:
  * primary_index = 'my_primary_index'
  * primary_index = ['my_primary_index1', 'my_primary_index2', 'my_primary_index3']
* types: Specifies required data types for requested columns to be saved in Vantage.
  The default value is None.
  This argument accepts a dictionary of column names and their required teradatasqlalchemy types
  as key-value pairs (({column_name1: type_value1, ... column_nameN: type_valueN})), allowing to
  specify a subset of the columns of a specific type.
Note:
    * When only a subset of all columns are provided, the rest are defaulted to appropriate types.
    * When types argument is not provided, all column types are appropriately assigned and
    defaulted. The column types are assigned as listed in the following table.
| pandas/NumPy Type | teradatasqlalchemy Type |
| ----------------- | ----------------------- |
| int32 | INTEGER |
| int64 | BIGINT |
| bool | BYTEINT |
| float32/float64 | FLOAT |
| datetime64/datatime64[ns] | TIMESTAMP |
| datatime64[ns, time_zone | ] TIMESTAMP(timezone=True) |
| Any other data type | VARCHAR(configure.default_varchar_size) |

    * This argument does not have any effect when the table specified using table_name and
    schema_name exists and if_exists = 'append'.
* batch_size: Specifies the number of rows to be loaded in a batch. The value of batch_size must be
  a positive integer.
  For better performance, Teradata recommends batch size to be at least 100,000.
  If this argument is None, which is the default value, there are two cases based on the number of rows,
  say N, in the dataframe 'df':
  * If N is greater than 100,000, the rows are divided into batches of equal size with each batch
    having at least 100,000 rows (except the last batch which might have more rows).
  * If N is less than 100,000, the rows are inserted in one batch after notifying the user that insertion
    happens with degradation of performance.
  If this argument is not None, the rows are inserted in batches of size given in the argument,
  irrespective of the recommended batch size.
  The last batch will have rows less than the batch size specified, if the number of rows is not an integral
  multiples of the argument batch_size.
* save_errors: Specifies whether to persist the error and warning information in Vantage or not.
  * If set to True, ERR_1 and ERR_2 tables are persisted. The fully qualified names of ERR_1,
    ERR_2 and warning table are returned in a dictionary containing keys named as ERR_1_table,
    ERR_2_table, and warnings_table, respectively.
* When save_errors is set to True and err_tbl_name is also provided, err_tbl_name takes
    precedence and error information is persisted into a single table using pandas Dataframe rather
    than in ERR_1 and ERR_2 tables.
  * When save_errors is set to False, errors and warnings information is not persisted as tables, but
    it is returned as pandas DataFrames in a dictionary containing keys named as errors_dataframe
    and warnings_dataframe, respectively.
  The default value is False.
* open_sessions: Specifies the number of Teradata data transfer sessions to be opened for
  fastload operation.
  If this argument is not provided, the default value is the smaller of 8 or the number of AMPs available.
  See the FastLoad section of https://pypi.org/project/teradatasql/ for additional information about
  number of Teradata data transfer sessions opened during fastload.
* err_tbl_1_suffix: Specifies the suffix for error table 1 created by a fastload job.
  The default value is _ERR_1.
* err_tbl_2_suffix: Specifies the suffix for error table 2 created by a fastload job.
  The default value is _ERR_2.
* err_tbl_name: Specifies the name for error table. This argument takes precedence over save_errors
  and saves error information in a single table, rather than ERR_1 and ERR_2 error tables.
  The default value is td_fl_<table_name>_err_<unique_id> where table_name is the name of the
  target/staging table and unique_id is the logon sequence number of the fastload job.
* warn_tbl_name: Specifies the name for warning table.
  The default value is td_fl_<table_name>_warn_<unique_id> where table_name is the name of the
  target/staging table and unique_id is the logon sequence number of the fastload job.
* err_staging_db: Specifies the name of the database to be used for creating staging table and
  error/warning tables.
  Note:
    The current session user must have CREATE, DROP and INSERT table permissions on
    err_staging_db database.
#### Returns
FastLoad returns a dict containing the following attributes:
* errors_dataframe: It is a Pandas DataFrame containing error messages thrown by fastload.
  DataFrame is empty if there are no errors or save_errors is set to True.
* warnings_dataframe: It is a Pandas DataFrame containing warning messages thrown by fastload.
  DataFrame is empty if there are no warnings.
* errors_table: Fully qualified name of the table containing errors..
  It is an empty string (''), if argument save_errors is set to False.
* warnings_table: Fully qualified name of the table containing warnings.
  It is an empty string (''), if argument save_errors is set to False.
* ERR_1_table: Fully qualified name of the ERR 1 table created by fastload job.
  It is an empty string (''), if argument save_errors is set to False.
* ERR_2_table: Fully qualified name of the ERR 2 table created by fastload job.
  It is an empty string (''), if argument save_errors is set to False.
#### Minimum version requirements for fastload()
teradatasql version 16.20.00.48 or later is required for fastload() function to work properly. If you have
a lower version installed, then teradatasql raises OperationalError and fastload() call ends with the
following error:
```python
[Teradata Database] [Error 3706] Syntax error: expected something between the
beginning of the request and the word 'teradata_require_fastloadINSERT'.
```
pandas version 0.24 or later is required for fastload() API to work properly. If you have a lower version,
fastload() API will fail with following error:
```python
AttributeError: 'Index' object has no attribute 'to_list'
```
Install pandas >= 0.24 to solve this issue.
#### Example Setup
```python
>>> from teradataml.dataframe.fastload import fastload
>>> from teradatasqlalchemy.types import *
>>> import pandas as pd
>>> df = {'emp_name': ['A1', 'A2', 'A3', 'A4'],
'emp_sage': [100, 200, 300, 400],
'emp_id': [133, 144, 155, 177],
'marks': [99.99, 97.32, 94.67, 91.00]
}
>>> pandas_df = pd.DataFrame(df)
```
#### Example 1: Save a Pandas DataFrame with default signature
```python
>>> fastload(df = pandas_df, table_name = 'my_table')
```
#### Example 2: Save a Pandas DataFrame with primary_index
```python
>>> pandas_df = pandas_df.set_index(['emp_id'])
>>> fastload(df = pandas_df, table_name = 'my_table_1', primary_index='emp_id')
```
#### Example 3: Save a Pandas DataFrame with index and primary_index
```python
>>> fastload(df = pandas_df, table_name = 'my_table_2',
index=True, primary_index='index_label')
```
#### Example 4: Save a Pandas DataFrame with types, appending to an
#### existing table
```python
>>> fastload(df = pandas_df, table_name = 'my_table_3', schema_name = 'alice',
index = True, index_label = 'my_index_label',
primary_index = ['emp_id'], if_exists = 'append',
types = {'emp_name': VARCHAR, 'emp_sage':INTEGER,
'emp_id': BIGINT, 'marks': DECIMAL})
```
#### Example 5: Save a Pandas DataFrame using levels in index of type MultiIndex
```python
>>> pandas_df = pandas_df.set_index(['emp_id', 'emp_name'])
>>> fastload(df = pandas_df, table_name = 'my_table_4', schema_name = 'alice',
index = True, index_label = ['index1', 'index2'],
primary_index = ['index1'], if_exists = 'replace'
```
#### Example 6: Save a Pandas DataFrame by opening given number of Teradata
#### data transfer sessions
This example saves a Pandas DataFrame by opening two Teradata data transfer sessions.
```python
>>> fastload(df = pandas_df, table_name = 'my_table_5', open_sessions = 2)
```
#### Example 7: Save a Pandas Dataframe to a table in specified target
#### database schema_name and errors and warnings to database specified
#### with err_staging_db
This example saves errors to table named err_tbl_name and warnings to warn_tbl_name. This example
assumes you are connected to a database different from schema_name and err_staging_db.
Create a pandas dataframe having one duplicate and one faulty row.
```python
>>>> data_dict = {"C_ID": [301, 301, 302, 303, 304, 305, 306, 307, 308],
"C_timestamp": ['2014-01-06 09:01:25',
'2014-01-06 09:01:25',
'2015-01-06
09:01:25.25.122200', '2017-01-06 09:01:25.11111',
'2013-01-06
09:01:25', '2019-03-06 10:15:28',
'2014-01-06
09:01:25.1098', '2014-03-06 10:01:02',
'2014-03-06
10:01:20.0000']}
>>> my_df = pd.DataFrame(data_dict)
```
Fastload data in non-default schema target_db and save errors and warnings in given tables.
```python
>>> fastload(df=my_df, table_name='fastload_with_err_warn_tbl_stag_db',
if_exists='replace', primary_index='C_ID',
schema_name='target_db',
types={'C_ID': INTEGER, 'C_timestamp': TIMESTAMP(6)},
err_tbl_name='fld_errors', warn_tbl_name='fld_warnings',
err_staging_db='stage_db')
Processed 9 rows in batch 1.
{'errors_dataframe':
batch_no                                      error_message
0         1   [Session 14527] [Teradata Database] [Error 26...,
'warnings_dataframe':
batch_no                                      error_message
0  batch_summary   [Session 14526] [Teradata SQL Driver] [Warnin...,
'errors_table': 'stage_db.fld_errors',
'warnings_table': 'stage_db.fld_warnings',
'ERR_1_table': '',
'ERR_2_table': ''}
```
Validate the loaded data table.
```python
>>>  DataFrame(in_schema("target_db", "fastload_with_err_warn_tbl_stag_db"))
C_ID    C_timestamp
303     2017-01-06 09:01:25.111110
306     2014-01-06 09:01:25.109800
304     2013-01-06 09:01:25.000000
307     2014-03-06 10:01:02.000000
305     2019-03-06 10:15:28.000000
301     2014-01-06 09:01:25.000000
308     2014-03-06 10:01:20.000000
```
Validate the error and warning tables.
```python
>>> DataFrame(in_schema("stage_db", "fld_errors"))
batch_no      error_message
1             [Session 14527] [Teradata Database] [Error 2673]
FastLoad failed to insert 1 of 9 batched rows. Batched row
3 failed to insert because of Teradata Database error 2673
in "target_db"."fastload_with_err_warn_tbl_stag_db"."C_timestamp"
>>> DataFrame(in_schema("stage_db", "fld_warnings")
batch_no        error_message
batch_summary   [Session 14526] [Teradata SQL Driver] [Warning 518] Found
1 duplicate or faulty row(s) while ending FastLoad of database table
"target_db"."fastload_with_err_warn_tbl_stag_db": expected a row count of 8,
got a row count of 7
```
#### Example 8: Save a Pandas Dataframe to a table in specified target database
schema_name and save errors in ERR_1 and ERR_2 tables having user defined
#### suffixes provided in err_tbl_1_suffix and err_tbl_2_suffix
The source Pandas Dataframe is the same as Example 7.
```python
>>> fastload(df = my_df, table_name = 'fastload_with_err_warn_tbl_stag_db',
schema_name = 'target_db',
if_exists = 'append',
types={'C_ID': INTEGER, 'C_timestamp': TIMESTAMP(6)},
err_staging_db='stage_db', save_errors=True,
err_tbl_1_suffix="_user_err_1",
err_tbl_2_suffix="_user_err_2")
{'errors_dataframe': Empty DataFrame
Columns: []
Index: [],
'warnings_dataframe':         batch_no
0  batch_summary   [Session 14699] [Teradata SQL Driver] [Warnin...,
'errors_table': '',
'warnings_table':
'stage_db.td_fl_fastload_with_err_warn_tbl_stag_db_warn_1730',
'ERR_1_table': 'stage_db.ml__fl_stag_1716272404181579_user_err_1',
'ERR_2_table': 'stage_db.ml__fl_stag_1716272404181579_user_err_2'}
# Validate ERR_1 and ERR_2 tables.
>>> DataFrame(in_schema("stage_db", "ml__fl_stag_1716270574550744_user_err_1"))
ErrorCode ErrorFieldName DataParcel
2673 F_C_timestamp b'12E...'
>>> DataFrame(in_schema("stage_db", "ml__fl_stag_1716270574550744_user_err_2"))
C_ID C_timestamp
```
#### fastexport()
The fastexport() function exports teradataml DataFrame to pandas DataFrame or CSV file using the
FastExport data transfer protocol.
Teradata recommends using fastexport() function when number of rows in the teradataml DataFrame
is at least 100,000. To extract lesser rows, you can ignore this function and use regular to_pandas() or
to_csv() functions.
FastExport opens multiple data transfer connections to the database.The number of data transfer
sessions can be set using the keyword argument open_sessions. If open_sessions argument is not set,
by default, data transfer sessions opened by teradataml is the smaller of 8 and the number of available
AMPs in Vantage.
Note:
* FastExport does not support all database data types.
    For example, tables with BLOB and CLOB type columns cannot be extracted.
* FastExport cannot be used to extract data from a volatile or temporary table.
* For best efficiency, do not use DataFrame.groupby() and DataFrame.sort() with FastExport.
See the FastExport section of https://pypi.org/project/teradatasql/ for more information about FastExport
protocol through teradatasql driver.
Required Arguments:
* df: Specifies the pandas DataFrame object to be saved in Vantage.
Optional Arguments:
* export_to: Specifies a value that notifies where to export the data.
  Permitted values are:
  * "pandas": Export data to a pandas DataFrame.
  * "csv": Export data to a given CSV file.
  The default value is 'panda'.
* index_column: Specifies column(s) to be used as index column for the converted object.
  Default values is None.
  Note:
    This argument is applicable only when export_to is set to "pandas".
* catch_errors_warnings: Specifies whether to catch errors and warnings (if any) raised by FastExport
  protocol while converting teradataml DataFrame.
  Default values is False.
  * When export_to is set to "pandas" and catch_errors_warnings is set to True, fastexport() returns
    a tuple containing:
    a. Pandas DataFrame.
    b. Errors(if any) in a list thrown by fastexport.
    c. Warnings(if any) in a list thrown by fastexport.
    When set to False, prints the fastexport errors and warnings to the standard output, if there
    are any.
  * When export_to is set to "csv" and catch_errors_warnings is set to True, fastexport() returns a
    tuple containing:
    a. Errors (if any) in a list thrown by fastexport.
    b. Warnings(if any) in a list thrown by fastexport.
* csv_file: Specifies the name of CSV file to which data is to be exported.
  Note:
    This argument is required when export_to is set to "csv".
* kwargs specifies keyword arguments.
  * sep: Specifies a single character string used to separate fields in a CSV file, with default value
    ' , '.
  * quotechar: Specifies a single character string used to quote fields in a CSV file, with default value
    ' \" ' (double quote).
  * coerce_float: Specifies whether to convert non-string, non-numeric objects to floating point.
  * parse_dates: Specifies columns to parse as dates.
  * open_sessions: Specifies the number of Teradata data transfer sessions to be opened for
    fastexport. This argument is only applicable in fastexport mode.
    Note:
    If open_sessions argument is not set, by default, the number of data transfer sessions
    opened by teradataml is the smaller of 8 and the number of available AMPs in Vantage.
Note:
    * sep and quotechar cannot be line feed ('\\n') or carriage return ('\\r').
    * sep and quotechar cannot be the same.
    * Length of sep and quotechar must be 1.
  See the FastExport section of https://pypi.org/project/teradatasql/ for more information about number
  of data transfer session opened during fastexport.
  See https://pandas.pydata.org/docs/reference/api/pandas.read_sql.html for more information about
  the coerce_float and parse_dates arguments.
#### Returns
The fastexport() function returns:
* When export_to is set to 'pandas' and catch_errors_warnings is set to 'True', the fastexport() function
  returns a tuple containing:
  * pandas DataFrame;
  * Errors, if any, in a list of strings thrown by fastexport;
  * Warnings, if any, in a list of strings thrown by fastexport.
* When export_to is set to 'pandas' and catch_errors_warnings is set to 'False', the fastexport()
  function returns a pandas DataFrame, and prints the fastexport errors and warnings to the standard
  output, if there is any.
* When export_to is set to 'csv' and catch_errors_warnings is set to 'True', fastexport() function returns
  a CSV file with name specified by argument csv_file and a tuple containing:
  * Errors, if any, in a list of strings thrown by fastexport;
  * Warnings, if any, in a list of strings thrown by fastexport.
#### Example Setup
```python
>>> from teradataml import fastexport
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame("admissions_train")
```
#### Example 1: Export teradataml DataFrame to pandas DataFrame
```python
>>> fastexport(df)
```
#### Example 2: Export teradataml DataFrame to pandas DataFrame with settings
This example exports the teradataml DataFrame 'df' to pandas DataFrame, setting index column with
argument index_column, converting non-string, non-numeric objects to floating point using argument
coerce_float, and catching errors and warnings thrown by fastexport.
```python
>>> pandas_df, err, warn = fastexport(df, index_column="gpa", coerce_float=True)
# Print pandas DataFrame.
>>> pandas_df
# Print errors list.
>>> err
# Print warnings list.
>>> warn
```
#### Example 3: Export teradataml DataFrame to pandas DataFrame by opening
#### specific number of sessions
This example exports the teradataml DataFrame 'df' to pandas DataFrame using two sessions.
```python
>>> fastexport(df, open_sessions=2)
```
#### Example 4: Export teradataml DataFrame to a given CSV file
```python
>>> fastexport(df, export_to="csv", csv_file="Test.csv")
```
#### Example 5: Export teradataml DataFrame to a given CSV file by opening
#### specific number of sessions
```python
>>> fastexport(df, export_to="csv", csv_file="Test_1.csv", open_sessions=2)
```
#### Example 6: Export teradataml DataFrame to a given CSV file and catch errors
#### and warnings
```python
>>> err, warn = fastexport(df, export_to="csv",
catch_errors_warnings=True, csv_file="Test_3.csv")
# Print errors list.
>>> err
# Print warnings list.
>>> warn
```
#### Example 7: Export teradataml DataFrame to CSV file with field separator and
#### field quote character
This example exports the teradataml DataFrame 'df' to a CSV file with (|) as field separator and single
quote (') as field quote character.
```python
>>> fastexport(df, export_to="csv", csv_file="Test_4.csv",  sep =
"|", quotechar="'")
```
### Loading Data from CSV to Vantage
* read_csv
#### read_csv
The read_csv() function loads data from CSV file into Teradata Vantage. This function is used to quickly
load large amounts of data in a table on Vantage using FastLoadCSV protocol.
When load data from CSV file, optional arguments can be used to identify fields in a CSV file.
* sep specifies a single character string used to separate fields in a CSV file, with default value ' , '.
* quotechar specifies a single character string used to quote fields in a CSV file, with default value ' "
  ' (double quote).
Considerations when using a CSV file:
* Each record is on a separate line of the CSV file. Records are delimited by line breaks (CRLF).
* The last record in the file may or may not have an ending line break.
* The first line in the CSV must be header line. The header line lists the column names separated by
  the field separator (e.g. col1,col2,col3).
Limitations when using a CSV file with FastLoad:
* read_csv function cannot load duplicate rows in a DataFrame if the table is a MULTISET table having
  primary index.
* read_csv function does not support all Teradata Database Engine 20 data types.
  For example, target table having BLOB and CLOB data type columns cannot be loaded.
* If there are some incorrect rows due to constraint violations, data type conversion errors, and so on,
  FastLoad protocol ignores those rows and inserts all valid rows.
* Rows in the DataFrame that failed to get inserted are categorized into errors and warnings by
  FastLoad protocol and these errors and warnings are stored into respective error and warning tables
  by FastLoad API.
Teradata recommends using FastLoad protocol when number of rows to be loaded is at least 100,000.
FastLoad opens multiple data transfer connections to the database, when the argument use_fastload
is set to 'True'. The number of data transfer sessions can be set using argument open_sessions. If this
argument is not set, by default, data transfer sessions opened by teradataml is the smaller of 8 and the
number of available AMPs in Vantage.
See the CSV BATCH INSERTS section and FastLoad section of https://pypi.org/project/teradatasql/ for
more information.
Required Arguments:
* filepath: Specifies the CSV filepath including name of the file to load the data from.
* table_name: Specifies the table name to load the data into.
* types: Specifies the data types for requested columns to be saved in Vantage.
  The default value is None.
  Note:
    This argument is optional when if_exists=append and non-PTI table already exists; and is
    required otherwise.
  This argument accepts a dictionary of column names and their required teradatasqlalchemy types as
  key-value pairs.
  Note:
    This must be OrderedDict, if CSV file does not contain header.
Optional Arguments:
* sep: Specifies a single character string used to separate fields in a CSV file, with default value ' , '.
* quotechar: Specifies a single character string used to quote fields in a CSV file, with default value '
  \" ' (double quote).
  Note:
    * sep and quotechar cannot be line feed ('\\n') or carriage return ('\\r').
    * sep and quotechar cannot be the same.
    * Length of sep and quotechar should be 1.
* schema_name: Specifies the name of the SQL schema in Vantage to write to.
  The default value is None, which means use the default Vantage schema.
* if_exists: Specifies the action to take when table already exists in the database.
  Permitted values are:
  * 'fail': If table exists, raise TeradataMlException;
  * 'replace': If table exists, drop it, recreate it, and insert data;
  * 'append': If table exists, append the existing table.
The default value is 'replace'.
* primary_index: Specifies the column(s) to use as primary index while creating tables in Vantage.
  When None (default value), No Primary Index (NOPI) tables are created.
  The default value is None.
  For example:
  * primary_index = 'my_primary_index'
  * primary_index = ['my_primary_index1', 'my_primary_index2', 'my_primary_index3']
* set_table: Specifies a flag to determine whether to create a SET or a MULTISET table.
  * When True, a SET table is created.
  * When False, a MULTISET table is created. This is the default value.
  Note:
    * Specifying set_table=True also requires specifying primary_index.
    * Creating SET table (set_table=True) may result in loss of duplicate rows, if CSV contains
    duplicate rows.
    * This argument has no effect if the table already exists and if_exists='append'.
* temporary: Specifies whether to create a table as volatile.
  The default value is False.
  When set to True:
  * FastloadCSV protocol is not used for loading the data.
  * schema_name is ignored.
* primary_time_index_name: Specifies a name for the Primary Time Index (PTI) when the table is to
  be created as PTI table.
  Note:
    This argument is not required or used when the table to be created is not a PTI table. It is ignored
    if specified without the timecode_column.
* timecode_column: Specifies the column in the DataFrame that reflects the form of the timestamp
  data in the time series.
  This column will be the TD_TIMECODE column in the table created. It should be of SQL type
  TIMESTAMP(n), TIMESTAMP(n) WITH TIMEZONE, or DATE, corresponding to Python types
  datetime.datetime or datetime.date.
Note:
    * This argument is required when the DataFrame must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * When this argument is specified, an attempt to create a PTI table will be made.
    * If this argument is specified, primary_index is ignored.
* timezero_date: Specifies the earliest time series data that the PTI will accept; a date that precedes
  the earliest date in the time series data.
  Value specified must be of the following format: DATE 'YYYY-MM-DD'. Default value is
  DATE '1970-01-01'.
  Note:
    * This argument is used when the DataFrame must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column.
* timebucket_duration: Specifies a duration that serves to break up the time continuum in the time
  series data into discrete groups or buckets.
  Value specified must use the formal form time_unit(n), where n is a positive integer, and time_unit
  can be any of the following: CAL_YEARS, CAL_MONTHS, CAL_DAYS, WEEKS, DAYS, HOURS,
  MINUTES, SECONDS, MILLISECONDS, or MICROSECONDS.
  Note:
    * This argument is required if columns_list is not specified or is None.
    * It is used when the DataFrame must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column.
* column_list: Specifies a list of one or more PTI table column names.
  Note:
    * This argument is required if timebucket_duration not specified.
    * It is used when the CSV data must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column.
* sequence_column: Specifies the column of type Integer containing the unique identifier for time
  series data reading when they are not unique in time.
  * When specified, implies SEQUENCED, meaning more than one reading from the same sensor
    may have the same timestamp. This column will be the TD_SEQNO column in the table created.
  * When not specified, implies NONSEQUENCED, meaning there is only one sensor reading per
    timestamp. This is the default.
  Note:
    * This argument is used when the CSV data must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column.
* seq_max: Specifies the maximum number of data rows that can have the same timestamp. Can be
  used when 'sequenced' is True.
  Permitted values range from 1 to 2147483647, with default value 20000.
  Note:
    * This argument is used when the CSV data must be saved as a PTI table.
    * This argument is not required or used when the table to be created is not a PTI table.
    * It is ignored if specified without the timecode_column argument.
* save_errors: Specifies whether to persist the error and warning information in Vantage or not.
  * If set to False, which is the default value,
    - Errors or warnings (if any) are not persisted into tables;
    - Errors table generated by FastloadCSV are not persisted.
  * If set to True,
    - The errors or warnings information is persisted and names of error and warning tables are
    returned. Otherwise, the function returns None for the names of the tables.
    - The errors tables generated by FastloadCSV are persisted and name of error tables are
    returned. Otherwise, the function returns None for the names of the tables.
* catch_errors_warnings: Specifies whether to catch errors and warnings (if any) raised by FastLoad
  protocol while loading data into the Vantage table.
  When set to False, which is the default value, function does not catch any errors and
  warnings, otherwise catches errors and warnings, if any, and returns as a dictionary along with
  teradataml DataFrame.
  See the following Returns section for more details.
* use_fastload: Specifies whether to use Fastload CSV protocol or not. Default value is True.
Teradata recommends to use Fastload when number of rows to be loaded are at least 100,000. To
  load lesser rows set this argument to 'False'. Fastload opens multiple data transfer connections to
  the database.
  Note:
    * When this argument is set to True, you can load the data into table using
    FastloadCSV protocol:
    - Set table
    - Multiset table
    * When this argument is set to False, you can load the data in following types of tables:
    - Set table
    - Multiset table
    - PTI table
    - Volatile table
* open_sessions: Specifies the number of Teradata data transfer sessions to be opened for
  fastload operation.
  If this argument is not provided, the default value is the smaller of 8 or the number of AMPs available.
  See the FastLoad section of https://pypi.org/project/teradatasql/ for additional information about
  number of Teradata data transfer sessions opened during fastload.
#### Returns
Based on the input, the read_csv function returns different output:
* If use_fastload is set to 'False', it returns teradataml DataFrame.
* If use_fastload is set to 'True' and catch_errors_warnings is set to 'False', it returns only
  teradataml DataFrame.
* If use_fastload is set to 'True' and catch_errors_warnings is set to 'True', it returns a tuple containing
  teradataml DataFrame and a dict containing the following attributes:
  * errors_dataframe: It is a Pandas DataFrame containing error messages thrown by fastload.
    DataFrame is empty if there are no errors.
  * warnings_dataframe: It is a Pandas DataFrame containing warning messages thrown
    by fastload.
    DataFrame is empty if there are no warnings.
  * errors_table: Name of the table containing errors.
    It is None, if argument save_errors is set to 'False'.
  * warnings_table: Name of the table containing warnings.
It is None, if argument save_errors is set to 'False'.
  * fastloadcsv_error_tables: Name of the tables containing errors generated by FastLoadCSV.
    It is empty list, if argument save_errors is set to 'False'.
#### Example Setup
```python
>>> import csv
>>> from collections import OrderedDict
>>> from teradataml.dataframe.data_transfer import read_csv
>>> from teradatasqlalchemy.types import *
>>> csv_filename = "test_file.csv"
>>> table_name = "read_csv_table"
>>> record = [
'id,fname,lname,marks',
'101,abc,def,200',
'102,lmn,pqr,105',
'103,def,xyz,492',
'101,abc,def,300'
]
>>> # Function to write list data into csv file.
>>> def write_data_into_csv(filename, record):
with open(filename, 'w', newline='') as csvFile:
csvWriter = csv.writer(csvFile, delimiter='\n')
csvWriter.writerow(record)
>>>
>>> write_data_into_csv(csv_filename, record)
```
#### Example 1: Default execution with types passed as OrderedDict
This example loads the data from CSV file into a table, with argument types passed as OrderedDict.
```python
>>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
>>> read_csv('test_file.csv', 'my_first_table', types)
```
#### Example 2: Catch all errors and warnings, and store in the table
This example loads the data from CSV file into a table using fastload CSV protocol, and catch all errors
and warnings and store those in the table.
```python
>>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table1',
types=types,
save_errors=True,
catch_errors_warnings=True)
```
#### Example 3: Load data into an existing table, replace the table
This example loads the data from CSV file into a table using fastload CSV protocol. If table exists, then
replace the table.
```python
>>>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table',
types=types,
if_exists='replace')
```
#### Example 4: Load data into an existing table, append the table
This example loads the data from CSV file into a table using fastload CSV protocol. If table exists in
specified schema, then append the table.
```python
>>> # Create new table in Vantage.
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table',
types=types,
if_exists='fail')
>>> # Write different data into csv file.
>>> record = [
'id,fname,lname,marks',
'501,orl,mal,740'
]
>>> write_data_into_csv(csv_filename, record)
>>>
>>> # Append the existing table.
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table',
if_exists='append')
```
#### Example 5: Load data into a SET table, catch all errors and warnings
This example loads the data from CSV file into a SET table using fastload CSV protocol. Catch all errors
and warnings as well as store those in the table.
```python
>>> # Write duplicate rows in csv file.
>>> record = [
'id,fname,lname,marks',
'101,abc,def,200',
'102,lmn,pqr,105',
'103,def,xyz,492',
'101,abc,def,200'
]
>>> write_data_into_csv(csv_filename, record)
>>> # Create SET table.
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table',
types=types,
if_exists='replace',
set_table=True,
primary_index='id',
save_errors=True,
catch_errors_warnings=True)
```
#### Example 6: Load data into a table without FastLoad protocol
This example loads the data from CSV file with DATE and TIMESTAMP columns into a table without
FastLoad protocol. If table exists in specified schema, then append to the table.
```python
>>> # Write date and timestamp in csv file.
>>> record = [
'id,fname,lname,marks,admission_date,admission_time',
'101,abc,def,200,2009-07-29,2009-07-29 20:17:59',
'102,lmn,pqr,105,2009-07-29,2009-07-29 20:17:59',
'103,def,xyz,492,2009-07-29,2009-07-29 20:17:59',
'101,abc,def,200,2009-07-29,2009-07-29 20:17:59'
]
>>> write_data_into_csv(csv_filename, record)
>>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT,
admission_date=DATE, admission_time=TIMESTAMP)
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table',
types=types,
if_exists='replace',
use_fastload=False)
>>> # Append the existing table.
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table',
if_exists='append',
use_fastload=False)
```
#### Example 7: Load data into a temporary table without FastLoad protocol
This example loads the data from CSV file into a temporary table without FastLoad protocol. If table exists
in specified schema, then append to the table.
```python
>>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT,
admission_date=DATE, admission_time=TIMESTAMP)
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table',
types=types,
if_exists='replace',
temporary=True)
>>> # Append the existing table.
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table',
if_exists='append',
temporary=True)
```
#### Example 8: Load data into a PTI table
This example loads data from CSV file with TIMESTAMP columns into a PTI table. If specified table exists,
then append to the table; otherwise, creates new table.
```python
>>> types = OrderedDict(partition_id=INTEGER, adid=INTEGER, productid=INTEGER,
event=VARCHAR, clicktime=TIMESTAMP)
>>> read_csv(filepath='test_file.csv',
table_name='my_first_read_csv_pti_table',
types=types,
if_exists='append',
timecode_column='clicktime',
columns_list='event',
use_fastload=False)
```
#### Example 9: Load data into a SET PTI table
This example loads data from CSV file with TIMESTAMP columns into a SET PTI table. If specified table
exists, then append to the table; otherwise, creates new table.
```python
>>> types = OrderedDict(partition_id=INTEGER, adid=INTEGER, productid=INTEGER,
event=VARCHAR, clicktime=TIMESTAMP)
>>> read_csv(filepath='test_file.csv',
table_name='my_first_read_csv_pti_table',
types=types,
if_exists='append',
timecode_column='clicktime',
columns_list='event',
set_table=True)
```
#### Example 10: Load data into a temporary PTI table
This example loads data from CSV file with TIMESTAMP columns into a temporary PTI table. If specified
table exists, then append to the table; otherwise, creates new table.
```python
>>> types = OrderedDict(partition_id=INTEGER, adid=INTEGER, productid=INTEGER,
event=VARCHAR, clicktime=TIMESTAMP)
>>> read_csv(filepath='test_file.csv',
table_name='my_first_read_csv_pti_table',
types=types,
if_exists='append',
timecode_column='clicktime',
columns_list='event',
temporary=True)
```
#### Example 11: Load data into a table by opening given number of Teradata data
#### transfer sessions
This example loads the data from CSV file into a Vantage table by opening two Teradata data
transfer sessions.
```python
>>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table_with_open_sessions',
types=types,
open_sessions=2)
```
#### Example 12: Load data into table and set primary index
This example loads data from CSV file into a Vantage table and set primary index provided through the
primary_index argument.
```python
>>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
>>> read_csv(filepath='test_file.csv',
table_name='my_first_table_with_primary_index',
types=types,
primary_index = ['fname'])
```
#### Example 13: Load the data from CSV file into VECTOR datatype in table
This example writes data from CSV file into a Vantage table using the VECTOR data type.
```python
>>> record = [
'id,array_col'
'10,"1,1"',
'11,"2,2"',
'12,"3,3"',
'13,"4,4"'
]
>>> write_data_into_csv('vector_data.csv', record)
>>> from teradatasqlalchemy.types import VECTOR, BIGINT
>>> read_csv(filepath='vector_data.csv',
... table_name='my_first_table_with_vector',
... types=types,
... use_fastload=False)
```
### Data Transfer between Vantage and Cloud Storage
Python users with data in external object storage can transfer the data to or from the Database Engine
20 using the Native Object Store feature in teradataml. External object storage includes AWS S3, Google
Cloud Storage and Azure Data Lake Storage Gen2, and so on.
The Native Object Store feature in teradataml provides ability for Python users to:
* Search and query CSV, JSON, and Parquet format datasets located on external object
  storage platforms.
* Write data stored on Vantage to external object storage platforms.
The ReadNOS and WriteNOS APIs in teradataml enable reading and writing datasets to external object
storage platforms.
Note:
* Import these functions only after establishing the connection to Vantage. Importing these
    functions before context creation raises import error.
* You can check whether these functions are available or not by
    running display_analytic_functions().
    * If these functions are available for the corresponding Vantage version, then
```python
display_analytic_functions() displays the functions.
```
    * Otherwise, these functions are not displayed in display_analytic_functions() output.
* Prerequisites
* ReadNOS
* WriteNOS
#### Prerequisites
Before proceeding to examples, verify with your database administrator that you have the correct
privileges for the following:
* Create Authorization Object
* Create Function Mapping
Create Authorization Object
An authorization object is used to control who can access external object storage. Before creating an
authorization object, user must have permission from external object storage to access the data. The
credentials are configured on the external object storage that you want to access.
For example, to access an Amazon S3 bucket, an Access Key and ID or an AWS Identity and Access
Management (IAM) user credential is required, depending on your external object storage.
Once your external object storage allows user to access it, set up an authorization object with the
credentials for your external object storage.
Note:
Public buckets or containers in external object storage do not require credentials for access. To
access a public bucket or container, put an empty string for USER and PASSWORD.
The following example demonstrates creation of authorization object within teradataml environment.
```python
from teradataml import create_context
td_context = create_context("Your-host", "Your-user", "Your-password")
auth_obj_cmd = """CREATE AUTHORIZATION authorization_object
USER 'YOUR-ACCESS-KEY-ID'
PASSWORD 'YOUR-SECRET-ACCESS-KEY';"""
td_context.execute(auth_obj_cmd)
```
Create Function Mapping
Function mapping is a new database object you can use to specify a simple name to execute a function.
An authorized database can be used to create function mappings for READ_NOS and WRITE _NOS
functions with name user desires. User can also store argument value which the user believes to stay
constant, for example, "authorization".
The following example demonstrates creation of function mapping for both functions.
```python
# Create definer trusted authorization object.
def_trustes_auth_cmd = """CREATE AUTHORIZATION DefAuth
AS DEFINER TRUSTED
USER 'YOUR-ACCESS-KEY-ID'
PASSWORD 'YOUR-SECRET-ACCESS-KEY';"""
fm_name = "<CUSTOM READ_NOS FUNCTION MAPPING NAME>"
location = "<EXTERNAL STORE LOCATION>"
ReadNOS_fm_create_cmd = '''CREATE FUNCTION MAPPING {fm_name}
FOR READ_NOS
EXTERNAL SECURITY DEFINER TRUSTED DefAuth
USING
ANY in TABLE,
LOCATION('{location}'),
BUFFERSIZE,
RETURNTYPE,
SAMPLE_PERC,
STOREDAS,
FULLSCAN,
MANIFEST,
ROWFORMAT,
HEADER;'''.format(fm_name, location)
td_context.execute(auth_obj_cmd)
fm_name = "<CUSTOM WRITE_NOS FUNCTION MAPPING NAME>"
WriteNOS_fm_create_cmd = '''CREATE FUNCTION MAPPING {fm_name}
FOR WRITE_NOS
EXTERNAL SECURITY DEFINER TRUSTED DefAuth
USING
ANY in TABLE,
LOCATION('{location}'),
STOREDAS('PARQUET'),
NAMING,
MANIFESTFILE,
MANIFESTONLY,
OVERWRITE,
INCLUDE_ORDERING,
INCLUDE_HASHBY,
MAXOBJECTSIZE,
COMPRESSION;'''.format(fm_name, location)
td_context.execute(auth_obj_cmd)
```
Set the function mappings created by setting configuration options.
```python
from teradataml.options.configure import configure
configure.write_nos_function_mapping = "<CUSTOM WRITE_NOS FUNCTION
MAPPING NAME>"
configure.read_nos_function_mapping = "<CUSTOM READ_NOS FUNCTION MAPPING NAME>"
```
Use function without specifying argument that is stored in function mapping.
```python
wn_obj =  WriteNOS(data=titanic_data, stored_as='PARQUET')
# Print the result DataFrame.
print(wn_obj.result)
rn_obj =  ReadNOS()
# Print the result DataFame.
print(obj.result)
```
#### ReadNOS
ReadNOS allows users to do the following:
* Perform an ad hoc query on all data formats with the data in-place on external object storage;
* List all the objects and path structure of external object storage;
* List the external object storage;
* Discover the schema of the data;
* Read CSV, JSON, and Parquet data;
* Bypass creating a foreign table in the user.
#### Example 1: Read Parquet file from AWS S3 bucket
```python
obj =  ReadNOS(location='/S3/s3.amazonaws.com/Your-Bucket/location/',
authorization='authorization_object',
stored_as='PARQUET')
# Print the result DataFame.
print(obj.result)
```
#### Example 2: Read CSV file with Authorization using Access_Key
#### and Access_ID
```python
# Create a table to store the data.
td_context.execute("CREATE TABLE read_nos_support_tab (payload dataset storage
format csv) NO PRIMARY INDEX;")
read_nos_support_tab = DataFrame("read_nos_support_tab")
# Read the CSV data using "data" argument.
obj = ReadNOS(data=read_nos_support_tab,
authorization='{"Access_ID": "YOUR-ID", "Access_Key": "YOUR-
KEY"}',
location='/S3/s3.amazonaws.com/Your-Bucket/csv-location/',
stored_as='TEXTFILE')
# Print the result DataFame.
print(obj.result)
```
#### WriteNOS
WriteNOS allows users to do the following:
* Extract selected or all columns from an user table or from derived results and write to an external
  object storage in Parquet data format;
* Write to Teradata supported external object storage, such as Amazon S3.
#### Example setup
```python
from teradataml import DataFrame, load_example_data
# Load the example data.
load_example_data("teradataml", "titanic")
# Create teradataml DataFrame objects.
titanic_data = DataFrame.from_table("titanic")
```
#### Example 1: Write a DataFrame to Amazon S3 bucket
```python
obj =  WriteNOS(data=titanic_data,
location='/S3/s3.amazonaws.com/Your-Bucket/location/',
authorization='{"Access_ID": "YOUR-ID", "Access_Key": "YOUR-
KEY"}',
stored_as='PARQUET')
```
#### Example 2: Write a DataFrame to Amazon S3 bucket as CSV with
#### authorization using Access_Key and Access_ID
```python
obj =  WriteNOS(data=titanic_data,
location='/S3/s3.amazonaws.com/Your-Bucket/location/',
authorization='authorization_object',
stored_as='CSV')
```
## Database Utility
* db_drop_table()
* db_drop_view()
* db_list_tables()
* db_python_package_details()
* db_python_package_version_diff
* db_python_version_diff
* display_analytic_functions()
* list_td_reserved_keywords()
* set_session_param()
* unset_session_param
* view_log()
### db_drop_table()
Use the db_drop_table() function to drop a table from a given schema. Returns True if the operation
is successful.
#### Required Argument
table_name
    Specifies the table name to be dropped.
#### Optional Arguments
schema_name
    Specifies the database of the table to be dropped. If a database is not specified, the function
    drops table from the current database.
suppress_error
    Specifies whether to raise an error or not.
    Default value is False.
#### Example Prerequisites
* Import the teradataml module:
```python
>>> from teradataml import *
```
* Load example data:
```python
>>> load_example_data("dataframe", "admissions_train")
```
#### Example 1: Drop table "admissions_train" from current database
```python
>>> db_drop_table(table_name = "admissions_train")
True
```
#### Example 2: Drop table "admissions_train" from schema "alice"
```python
>>> db_drop_table(table_name = "admissions_train", schema_name = "alice")
True
```
### db_drop_view()
Use the db_drop_view() function to drop a view from a given schema. Returns True if the operation
is successful.
Required arguments:
* view_name specifies the view name to be dropped.
Optional arguments:
* schema_name specifies the database of the view to be dropped. If a database is not specified, the
  function drops view from the current database.
#### Example Prerequisites
Create a temporary view:
```python
>>> connection_object.execute("create view temporary_view as (select 1 as
dummy_col1, 2 as dummy_col2);")
```
#### Example 1: Drop a view "temporary_view" from current database
```python
>>> db_drop_view(view_name = 'temporary_view')
True
```
#### Example 2: Drop a view "temporary_view" from schema "alice"
```python
>>> db_drop_view(view_name = 'temporary_view', schema_name = 'alice')
True
```
### db_list_tables()
Use the db_list_tables() function to list tables and views from a specified schema.
#### Optional Arguments
schema_name
    Specifies the name of the schema in the database. If the schema is not specified, the function
    lists tables and views from the current database. Default value is None.
object_name
    Specifies a table or view name or pattern to be used for filtering them from the database.
    Pattern may contain '%' or '_' as pattern matching characters:
    * A '%' represents any string of zero or more arbitrary characters. Any string of characters
      is acceptable as replacement for the percent sign.
* A '_' represents exactly one arbitrary character. Any single character is acceptable in the
      position in which the underscore character appears.
    Note:
      If '%' is specified in 'object_name', then the '_' character is not evaluated for an
      arbitrary character.
    Example:
    * '%abc' will return all table and view object names starting with any character and ending
      with 'abc'.
    * 'a_c' will return all table and view object names starting with 'a', ending with 'c' and has
      length of 3.
Object_type
    Specifies object type to apply the filter. Valid value includes:
    * all: list all the object types, which is the default value
    * table: list only tables
    * view: list only views
    * volatile: list only volatile tables
    * temp: list all teradataml temporary objects created in the specified database
    Default value is all.
#### Example 1: List all the objects in the default schema
```python
>>> load_example_data("dataframe", "admissions_train")
>>> db_list_tables()
```
#### Example 2: List all the views in the default schema
```python
>>> connection_object.execute("create view temporary_view as (select 1 as
dummy_col1, 2 as dummy_col2);")
>>> db_list_tables(None , None, 'view')
```
#### Example 3: List all the objects in the default schema with specific conditions
This example lists all the objects in the default schema whose names begin with 'abc' followed by one
arbitrary character and any number of characters in the end.
```python
>>> connection_object.execute("create view abcd123 as (select 1 as dummy_col1,
2 as dummy_col2);")
>>> db_list_tables(None, 'abc_%', None)
```
#### Example 4: List all the tables in the default schema with specific conditions
This example lists all the tables in the default schema whose names begin with 'adm' followed by one
arbitrary character and any number of characters in the end.
```python
>>> load_example_data("dataframe", "admissions_train")
>>> db_list_tables(None, 'adm_%', 'table')
```
#### Example 5: List all the views in the default schema with specific conditions
This example lists all the views in the default schema whose names begin with any character but end
with 'abc'.
```python
>>> connection_object.execute("create view view_abc as (select 1 as dummy_col1,
2 as dummy_col2);")
>>> db_list_tables(None, '%abc', 'view')
```
#### Example 6: List all the volatile tables in the default schema with
#### specific conditions
This example lists all the volatile tables in the default schema whose names begin with 'abc' and ends with
any arbitrary character and has a length of 4.
```python
>>> connection_object.execute("CREATE volatile TABLE abcd(col0 int, col1 float)
NO PRIMARY INDEX;")
>>> db_list_tables(None, 'abc_', 'volatile')
```
#### Example 7: List all the temporary objects created by teradataml in the default
#### schema with specific conditions
This example lists all the temporary objects created by teradataml in the default schema whose names
begin and end with any number of arbitrary characters but contains 'filter' in between.
```python
>>> db_list_tables(None, '%filter%', 'temp')
```
### db_python_package_details()
Use the db_python_package_details() function to get the Python packages installed on Vantage, and their
corresponding versions.
Note:
Use this function only when Python interpreter and add-on packages are installed on the
Vantage node.
Optional arguments:
* names specifies the name(s) and pattern(s) of the Python package(s) for which version information is
  to be fetched from Vantage.
  If this argument is not specified or None, versions of all installed Python packages are returned.
#### Example 1: Get the details of a Python package 'dill'
```python
>>> db_python_package_details("dill")
package  version
0    dill  0.2.8.2
```
#### Example 2: Get the details of all Python packages having string 'mpy'
```python
>>> db_python_package_details(names = "mpy")
package  version
0          simpy   3.0.11
1          numpy   1.16.1
2          gmpy2    2.0.8
3  msgpack-numpy  0.4.3.2
4          sympy      1.3
```
#### Example 3: Get the details of all Python packages having string 'numpy',
#### string 'learn', or both
```python
>>> db_python_package_details(["numpy", "learn"])
package  version
0   scikit-learn   0.20.3
1          numpy   1.16.1
2  msgpack-numpy  0.4.3.2
```
#### Example 4: Get the details of all Python packages installed on Vantage
```python
>>> db_python_package_details()
package  version
0       packaging     18.0
1          cycler   0.10.0
2           simpy   3.0.11
3  more-itertools    4.3.0
4          mpmath    1.0.0
5           toolz    0.9.0
6       wordcloud    1.5.0
7         mistune    0.8.4
8  singledispatch  3.4.0.3
9           attrs   18.2.0
```
### db_python_package_version_diff
Use the db_python_package_version_diff() function to get the difference between the Python packages
installed on Vantage and in the current environment for the packages mentioned in argument “packages”.
Note:
* Use this function only when Python interpreter and add-on packages are installed on the
    Vantage node.
* This function also checks for differences in Python packages versions given part of package
    name as a string.
#### Optional Argument
packages
    Specifies the names of the Python packages for which the difference in the versions is to be
    fetched from Vantage.
    Note:
      * If this argument is None, all the packages installed on Vantage are considered.
      * If any package is present in Vantage but not in the current environment, then None
      is shown as the version of the package in the current environment.
    Types: str or list of str
    Returns: pandas DataFrame
#### Example 1: Get the difference in the versions of Python package 'dill' installed
#### on Vantage
```python
>>> db_python_package_version_diff("dill")
package   vantage  local
0       dill   0.10.0  0.11.2
```
#### Example 2: Get the difference in the versions of Python package 'dill' and
#### 'matplotlib' installed on Vantage
```python
>>> db_python_package_version_diff(["dill", "matplotlib"])
package   vantage  local
0         dill   0.10.0  0.11.2
1    matplotlib   3.6.2   3.8.0
```
#### Example 3: Get the difference in the versions of all Python packages installed
#### on Vantage
```python
>>> db_python_package_version_diff()
package  vantage  local
0   scikit-learn   1.3.3  0.24.2
1           dill  0.10.0  0.11.2
532        attrs  18.2.0  17.0.0
```
#### Example 4: Get the difference in the versions of all Python packages installed
#### on Vantage that contain 'sci' in their name.
```python
>>> db_python_package_version_diff("sci")
package  vantage   local
0       scikit-learn   1.3.3  0.24.2
1    scikit-optimize   0.9.0    None
2        scikit-plot   0.3.7    None
3              scipy  1.10.0  1.15.1
4       scikit-image  0.19.3    None
```
### db_python_version_diff
Use the db_python_version_diff() function to get the difference of the Python interpreter major version
installed on Vantage and the Python version used in the current environment.
Note:
Use this function only when Python interpreter and add-on packages are installed on the
Vantage node.
The db_python_version_diff() returns an empty dictionary when the Python major version is same between
Vantage and the current environment. Otherwise, it returns a dictionary with the following keys and their
corresponding values:
* 'vantage_version': The Python major version installed on Vantage (e.g., 3.8, 3.9).
* 'local_version': The Python major version used in the current environment (e.g., 3.10, 3.11).
#### Example
Get the difference in the Python version installed on Vantage and the current environment.
```python
>>> db_python_version_diff()
{"vantage_version": "3.7", "local_version": "3.8"}
```
### display_analytic_functions()
Use the display_analytic_functions() function to display a list of analytic functions available to use on the
Teradata Vantage system that the user is connected to.
#### Example Setup
```python
>>> from teradataml import create_context, display_analytic_functions
```
#### Example 1: Display a list of available analytic functions
```python
>>> create_context(host = host, username=user, password=password)
>>> display_analytic_functions()
List of available SQLE analytic functions:
```
#### Example 2: When no analytic functions are available on the cluster
```python
>>> display_analytic_functions()
No analytic functions available with connected Teradata Vantage system.
```
### list_td_reserved_keywords()
Use the list_td_reserved_keywords() function to validate if the specified string or list of strings is a Teradata
reserved keyword or not.
Note:
If a key is not specified in the function call, all the Teradata reserved keywords are displayed.
Optional arguments:
* key specifies a string or list of strings to validate if it is a Teradata reserved keyword.
* raise_error specifies whether to raise exception or not.
  When set to True, an exception is raised, if specified "key" is a Teradata reserved keyword,
  otherwise not.
  The default value is False.
#### Example Setup
```python
>>> from teradataml import list_td_reserved_keywords
```
#### Example 1: List all available Teradata reserved keyword
```python
>>> list_td_reserved_keywords()
restricted_word
0             ABS
1         ACCOUNT
2            ACOS
3           ACOSH
4      ADD_MONTHS
5           ADMIN
6             ADD
7     ACCESS_LOCK
8    ABORTSESSION
9           ABORT
```
#### Example 2: Validate if keyword "account" is a Teradata reserved keyword
#### or not
```python
>>> list_td_reserved_keywords("account")
True
```
#### Example 3: Validate and raise exception if keyword "account" is a Teradata
#### reserved keyword
```python
>>> list_td_reserved_keywords("account", raise_error=True)
TeradataMlException: [Teradata][teradataml](TDML_2121) 'account' is a Teradata
reserved keyword.
```
#### Example 4: Validate if the list of keywords contains Teradata reserved
#### keyword or not
```python
>>> list_td_reserved_keywords(["account", 'add', 'abc'])
True
```
#### Example 5: Example 5: Validate and raise exception if the list of keywords
#### contains Teradata reserved keyword
```python
>>> list_td_reserved_keywords(["account", 'add', 'abc'], raise_error=True)
TeradataMlException: [Teradata][teradataml](TDML_2121) '['ADD', 'ACCOUNT']' is
a Teradata reserved keyword.
```
### set_session_param()
Use the set_session_param() function to set the database session parameters. Returns True if session
parameter is set successfully.
Note:
Refer to "SET SESSIONS" in SQL Data Definition Language Syntax and Examples, B035-1144 for
session parameters.
Raises ValueError, teradatasql.OperationalError
Required arguments:
* name specifies the name of the parameter to set.
  Permitted values: timezone, calendar, account, character_set_unicode, collation, constraint,
  database, dateform, debug_function, dot_notation, isolated_loading, function_trace,
  json_ignore_errors, searchuifdbpath, transaction_isolation_level, query_band, udfsearchpath
  Types: str
* value specifies the value for the parameter "name" to set.
  Permitted values:
  * timezone: timezone strings
  * calendar: Teradata, ISO, Compatible
  * character_set_unicode: ON, OFF
  * account: should be a list in which first item should be "account string" second should be either
    SESSION or REQUEST.
  * collation: ASCII, CHARSET_COLL, EBCDIC, HOST, JIS_COLL, MULTINATIONAL
  * constraint: row_level_security_constraint_name {( level_name | category_name [,...] | NULL )}
    where:
    ▪ row_level_security_constraint_name: Name of an existing constraint.
    The specified constraint_name must be currently assigned to the user. You can specify a
    maximum of 6 hierarchical constraints and 2 non-hierarchical constraints per SET SESSION
    CONSTRAINT statement.
    ▪ level_name: Name of a hierarchical level, valid for the constraint_name, that is to replace the
    default level.
    The specified level_name must be currently assigned to the user. Otherwise, Vantage returns
    an error to the requestor.
    ▪ category_name: A set of one or more existing non-hierarchical category names valid for
    the constraint_name.
    Because all assigned category (non-hierarchical) constraint values assigned to a user are
    automatically active, "set_session_param" is only useful to specify a subset of the assigned
    categories for the constraint.
    For example, assume that User BOB has 3 country codes, and wants to load a table with
    data that is to be made available to User CARL who only has rights to see data for his own
    country. User BOB can use "set_session_param" to specify only the country code for User
    CARL when loading the data so Carl can access the data later.
  * database: Name of the new default database for the remainder of the current session.
  * dateform: ANSIDATE, INTEGERDATE
  * debug_function: Specifies a list in which the first item is "function_name" and second item is either
    ON or OFF.
  * dot_notation: DEFAULT, LIST, NULL ERROR
  * isolated_loading: NO, '', CONCURRENT
  * function_trace: Specifies a list in which the first item is "mask_string" and second is the
    table name.
  * json_ignore_errors: ON, OFF
  * searchuifdbpath: String in format 'database_name, user_name'
  * transaction_isolation_level: READ UNCOMMITTED, RU, SERIALIZABLE, SR
* query_band: Should be a list first item should be "band_specification" and second should be
    either SESSION or TRANSACTION.
  * udfsearchpath: Should be a list first item should be "database_name" and second should
    be "udf_name".
  Types: str or list of strings
#### Example 1: Set time zone offset for the session as the system default
```python
>>> set_session_param('timezone', "'LOCAL'")
True
```
#### Example 2: Set time zone to "AMERICA PACIFIC"
```python
>>> set_session_param('timezone', "'AMERICA PACIFIC'")
True
```
#### Example 3: Set time zone to "-07:00"
```python
>>> set_session_param('timezone', "'-07:00'")
True
```
#### Example 4: Set time zone to 3 hours ahead of 'GMT'
```python
>>> set_session_param('timezone', "3")
True
```
#### Example 5: Set calendar to 'COMPATIBLE'
```python
>>> set_session_param('calendar', "COMPATIBLE")
True
```
#### Example 6: Dynamically change account to 'dbc' for remainder of session
```python
>>> set_session_param('account', ['dbc', 'SESSION'])
True
```
#### Example 7: Enable Unicode Pass Through processing
```python
>>> set_session_param('character_set_unicode', 'ON')
True
```
#### Example 8: Set session to ASCII collation
```python
>>> set_session_param('collation', 'ASCII')
True
```
#### Example 9: Resulting session has row-level security label consisting of
#### unclassified level and nato category
```python
>>> set_session_param('constraint', 'classification_category (norway)')
True
```
#### Example 10: Change default database for the session
```python
>>> set_session_param('database', 'alice')
True
```
#### Example 11: Change the DATE format to 'INTEGERDATE'
```python
>>> set_session_param('dateform', 'INTEGERDATE')
True
```
#### Example 12: Enable debugging for the session
```python
>>> set_session_param('debug_function', ['function_name', 'ON'])
True
```
#### Example 13: Set session response for dot notation query result
```python
>>> set_session_param('dot_notation', 'DEFAULT')
True
```
#### Example 14: DML operations are not performed as concurrent load
#### isolated operation
```python
>>> set_session_param('isolated_loading', 'NO')
True
```
#### Example 15: Enable function trace output for debugging external user-defined
#### functions and external SQL procedures for current session
```python
>>> set_session_param('function_trace', ["'diag,3'", 'titanic'])
True
```
#### Example 16: Enable validation of JSON data on INSERT operations
```python
>>> set_session_param('json_ignore_errors', 'ON')
True
```
#### Example 17: Set database search path for SCRIPT execution in
#### SessionTbl.SearchUIFDBPath column
```python
>>> set_session_param('SEARCHUIFDBPATH', 'dbc, alice')
True
```
#### Example 18: Use PROXYROLE name:value pair in a query band to set proxy
#### role in a trusted session to a specific role
```python
>>> set_session_param('query_band',
["'PROXYUSER=fred;PROXYROLE=administration;'", 'SESSION'])
True
```
#### Example 21: Specify custom UDF search path
When you run a UDF, Database Engine 20 searches this path first, before looking in the default Database
Engine 20 search path for the UDF.
```python
>>> set_session_param('udfsearchpath', ["alice, SYSLIB, TD_SYSFNLIB", 'bitor'])
True
```
### unset_session_param
Use the unset_session_param() function to unset the database session parameters. Returns True if
session parameter is unset successfully.
Raises ValueError, teradatasql.OperationalError
Required argument:
* name specifies the name of the parameter to set.
  Permitted values: timezone, account, calendar, collation, database,
  dataform, character_set_unicode, debug_function, isolated_loading, function_trace,
  json_ignore_errors, query_band
  Types: str
#### Example 1: Unset session to previous time zone
```python
>>> set_session_param('timezone', "'GMT+1'")
True
>>> unset_session_param("timezone")
True
```
### view_log()
Use the view_log() function to view script, apply or byom logs on Vantage.
Note:
* Logs files are downloaded based on log_dir.
* teradataml creates a subdirectory with the name as query_id and downloads the logs to
    the subdirectory.
* It takes few seconds to generate files from query_id. Teradata recommends providing query_id
    to function view_log() after few seconds; otherwise, it will return empty subdirectory.
Optional arguments:
* log_type specifies the log to view.
  Permitted values:
  * 'script', script logs are pulled from 'scriptlog' file on database node.
    This is useful when Script.execute() is executed to run user scripts in Vantage.
  * 'byom', byom log is pulled from 'byom.log' file on database node.
  * 'apply', logs are pulled from kubernetes container, function downloads the log file to a folder.
  Default value is 'script'.
* num_lines specifies the number of lines to be read and displayed from log.
  Note:
    This argument is applicable only when log_type is 'script'. Otherwise, it is ignored.
  The default value is 1000.
* query_id: Specifies the id of the query for which logs are to be retrieved. This query id is part of the
  error message received when Apply class or DataFrame apply method calls fail to execute the Apply
  table operator query.
  Note:
    This argument is required only when log_type is 'apply'. Otherwise, it is ignored.
* log_dir: Specifies the directory path to store all the log files for query_id.
Note:
    This argument is applicable only when log_type is 'apply'. Otherwise, it is ignored.
  When this argument is not provided, function creates a temporary folder and store the log files in this
  temp folder.
The return of this function depends on the log_type:
* When log_type is 'apply', it returns log file;
* For other types of logs, it returns teradataml DataFrame.
#### Example 1: View script log
```python
view_log(log_type="script", num_lines=200)
```
#### Example 2: View byom log
```python
view_log(log_type="byom", num_lines=200)
```
#### Example 3: Download the Apply query logs to a default temp folder
This example uses query id from the error messages returned by Apply class.
```python
view_log(log_type="apply", query_id='307161028465226056')
Logs for query_id "307191028465562578" is stored at "C:\\local_repo\\AppData\
\Local\\Temp\\tmp00kuxlgu\\307161028465226056"
```
#### Example 4: Download the Apply query logs to a specific folder
This example uses query id from the error messages returned by Apply class.
```python
view_log(log_type="apply", query_id='307161028465226056',log_dir='C:\
\local_repo\\workspace')
Logs for query_id "307191028465562578" is stored at "C:\\local_repo\
\workspace\\307161028465226056"
```
## File Management Functions
* install_file()
* remove_file()
* list_files()
### install_file()
Use the install_file() function to install or replace external language script or model files in Vantage. On
success, it prints a message confirming that file is installed or replaced.
Installed language script can be executed using execute_script method in Script.
Required arguments:
* file_identifier specifies the name associated with the user-installed file. It cannot have a database
  name associated with it, as the file is always installed in the current database. The name should be
  unique within the database. It can be any valid Teradata identifier.
* file_path specifies the absolute or relative path of the file (including file name) to be installed. This file
  is identified in Vantage by file_identifier.
  Note:
    File can be on client or remote server. The file location should be specified accordingly.
Optional arguments:
* file_on_client specifies whether the file is present on client or at remote location on Vantage. The
  default value is True, which means on client. Set to False if the file to be installed is present at remote
  location on Vantage.
* is_binary specifies if the file to be installed is a binary file.
* replace specifies if the file is to be installed or replaced.
  * If set to True: the file is replaced based on value of force_replace;
  * If set to False: the file is installed.
* force_replace specifies if system should check for the file being used before replacing it:
  * If set to True: the file is replaced even if it is being executed.
  * If set to False: an error is thrown if it is being executed.
  Note:
    This argument is ignored if replace is set to False.
#### Example 1: Install a file using default text mode
Note:
To run this example, "mapper.py" is required on client at the path specified by argument file_path.
* Create the "mapper.py" file:
```python
#!/usr/bin/python
import sys
for line in sys.stdin:
line = line.strip()
words = line.split()
for word in words:
print ('%s\t%s' % (word, 1))
```
* Install the file:
```python
>>> install_file (file_identifier='mapper', file_path='data/
scripts/mapper.py')
File mapper.py installed in Vantage
```
#### Example 2: Install a file using binary mode
```python
>>> install_file (file_identifier='binaryfile', file_path='data/scripts/
binary_file.dms', file_on_client = True, is_binary = True)
File binary_file.dms installed in Vantage
```
#### Example 3: Replace a file with another one using default text mode
This example replaces the file 'mapper.py' with 'mapper_replace.py' found at the relative path using the
default text mode.
Note:
To run this example, "mapper_replace.py" is required on client at the path specified by
argument file_path.
* Create the "mapper_replace.py" file:
```python
#!/usr/bin/python
import sys
for line in sys.stdin:
line = line.strip()
words = line.split()
for newword in words:
print ('%s,%s' % (newword, 1))
```
* Replace the existing file with the new file:
```python
>>> install_file (file_identifier='mapper', file_path='data/scripts/
mapper_replace.py', file_on_client=True, is_binary= False,
replace=True, force_replace=True)
File mapper_replace.py replaced in Vantage
```
### remove_file()
Use the remove_file() function to remove user installed files and scripts from Vantage.
Required arguments:
* file_identifier specifies the name associated with the user installed file. It cannot have a database
  name associated with it, as the file is always installed in the current database.
* force_remove specifies if system checks for the file being used before removing it.
  * If set to True, then the file is removed even if it is being executed.
  * If set to False, which is the default value, then an error is thrown if it is being executed.
#### Example 1: Remove an installed file using default text mode
* See "Example 1: Install a file using default text mode" in install_file() to install the ''mapper.py" file.
* Remove the installed "mapper.py" file:
```python
>>> remove_file (file_identifier='mapper', force_remove=True)
File mapper removed from Vantage
```
#### Example 2: Remove a binary file
* See "Example 2: Install a file using binary mode" in install_file() to install the ''binary_file.dms" file.
* Remove the installed ''binary_file.dms" file:
```python
>>> remove_file (file_identifier='binaryfile', force_remove=False)
File binaryfile removed from Vantage
```
### list_files()
Use the list_files() function to list all files installed in Vantage.
#### Example: Install the file mapper.py found at the relative path and then list the
#### installed file
Run the install_file example before listing the installed files.
The file can be on the client or remote server. "mapper.py" must be present on client for the following
example to run. Provide the path of "mapper.py" in the "file_path" argument.
* Create the "mapper.py" file:
```python
#!/usr/bin/python
import sys
for line in sys.stdin:
line = line.strip()
words = line.split()
for word in words:
print ('%s\t%s' % (word, 1))
```
* Install the file:
```python
>>> install_file (file_identifier='mapper', file_path='data/
scripts/mapper.py')
File mapper.py installed in Vantage
```
* List the files:
```python
>>> list_files()
Files
0 mapper.py
```
## Miscellaneous Functions
* print_options()
* show_versions()
* execute_sql()
* async_run_status()
* td_range()
* get_formatters()
### print_options()
Use the print_options() function to display both configure and display options set in the current session.
#### Example 1: Run 'print_options()' after creating a connection object
If you run the print_options() function after creating a connection object, it displays values set for all
the options.
```python
>>> import teradataml as tdml
>>> tdml.print_options()
Display Options
byte_encoding = base16
max_rows = 10
precision = 3
print_sqlmr_query = False
suppress_vantage_runtime_warnings = False
Configure Options
column_casesensitive_handler = False
default_varchar_size = 1024
val_install_location = None
vantage_version = vantage1.3
```
#### Example 2: Run 'print_options()' without a working connection
If you run the print_options() function without a working connection, it displays values set for all the options
except vantage_verison. A warning is thrown for vantage_version option.
```python
>>> import teradataml as tdml
>>> tdml.print_options()
Display Options
byte_encoding = base16
max_rows = 10
precision = 3
print_sqlmr_query = False
suppress_vantage_runtime_warnings = False
Configure Options
column_casesensitive_handler = False
default_varchar_size = 1024
val_install_location = None
Option 'vantage_version' information requires a working connection object.
```
### show_versions()
Use the show_versions() function to display client information, version information for client package
dependencies and server information.
Server information is displayed only when a context is already created, otherwise only client information
is displayed.
#### Example 1: Run 'show_versions()' without initiating connection
If you run show_versions() function without initiating connection, i.e., creating context, the function
displays client information, version information for client package dependencies, and warning is thrown for
server information.
```python
>>> show_versions()
UserWarning: Server information requires a working connection object.
warnings.warn("Server information requires a working connection object.")
INSTALLED VERSIONS: Client
python: 3.7.3.final.0
python-bits: 64
OS: Windows
OS-release: 10
machine: AMD64
processor: Intel64 Family 6 Model 94 Stepping 3, GenuineIntel
byteorder: little
LC_ALL: None
LANG: None
LOCALE: None.None
pandas: 0.24.2
sqlalchemy: 1.3.5
teradatasqlalchemy: 16.20.0.8
teradatasql: 16.20.0.55
teradataml: 16.20.0.4
INSTALLED VERSIONS: Server
```
#### Example 2: Run 'show_versions()' after initiating connection
If you run the show_versions() function after initiating connection, i.e., creating context, the function
displays client information, version information for client package dependencies and server information.
```python
>>> from teradataml import show_versions
>>> ## Make sure to execute create_context() before this.
>>> show_versions()
INSTALLED VERSIONS: Client
python: 3.7.3.final.0
python-bits: 64
OS: Windows
OS-release: 10
machine: AMD64
processor: Intel64 Family 6 Model 94 Stepping 3, GenuineIntel
byteorder: little
LC_ALL: None
LANG: None
LOCALE: None.None
pandas: 0.24.2
sqlalchemy: 1.3.5
teradatasqlalchemy: 16.20.0.8
teradatasql: 16.20.0.55
teradataml: 16.20.0.4
INSTALLED VERSIONS: Server
BUILD_VERSION: 08.10.01.00-4180efe
RELEASE: Vantage 1.1 GA
```
### execute_sql()
Use the execute_sql() function to run a SQL statement provided in string format at the connected database
by using provided parameters.
Required argument:
* statement: Specifies the SQL statement to execute.
Optional Argument:
* parameters: Specifies parameters to be used in case of parameterized query.
#### Example 1: Create table and insert data using execute_sql()
* Import required modules.
```python
>>> import teradataml
>>> from teradataml import execute_sql
```
* Connect to database
```python
>>>create_context(host = host, username=user, password=password)
```
* Create a table.
```python
>>> execute_sql("Create table table1 (col_1 int, col_2 varchar(10),
col_3 float);")
```
* Insert values in the table created.
```python
>>> execute_sql("Insert into table1 values (1, 'col_val', 2.0);")
```
* Insert values in the table using a parameterized query.
```python
>>> execute_sql(statement="Insert into table1 values (?, ?, ?);",
parameters=[[1, 'col_val_1', 10.0],
[2, 'col_val_2', 20.0]])
```
#### Example 2: Select data from table using execute_sql()
* Execute parameterized 'SELECT' query.
```python
>>> result_cursor = execute_sql(statement="Select * from table1 where
col_1=? and col_3=?;",
parameters=[(1, 10.0),(1, 20.0)])
```
* Print the fetched rows.
```python
>>> for row in result_cursor:
...    print(row)
[1, 'col_val_1', 10.0]
```
#### Example 3: Run Help Column query on table using execute_sql()
* Execute parameterized 'SELECT' query.
```python
>>> result_cursor = execute_sql('Help column table1.*;')
```
* Print the fetched rows.
```python
>>> for row in result_cursor:
...     print(row)
['col_1
', 'I   ', 'Y ',
'-(10)9
', 4, None, None, None, None, 'N ', 'T ', 'Y ', 'N ', 'P ', None, None, None,
None, None, 'N ', None, None, None, None, 'N ', 0, 'NA  ', 'NA  ', 'N ', None,
None, 'col_1', 'col_1', None, None, None, None, None, None, None, None, None,
None, None, None, None, None, None, None, None]
['col_2
', 'CV  ', 'Y ',
'X(10)
', 10, None, None, None, None, 'N ', 'T ', 'N ', None, None, None, None, 1,
None, None, 'N ', None, None, None, None, 'N ', 0, 'NA  ', 'NA  ', 'N ', None,
None, 'col_2', 'col_2', None, None, None, None, None, None, None, None, None,
None, None, None, None, None, None, None, None]
['col_3
', 'F   ', 'Y ',
'-9.99999999999999E-999
', 8, None, None, None, None, 'N ', 'T ', 'N ', None, None, None,
None, None, None, None, 'N ', None, None, None, None, 'N ', 0, 'NA  ', 'NA
', 'N ', None, None, 'col_3', 'col_3', None, None, None, None, None, None,
None, None, None, None, None, None, None, None, None, None, None]
```
### async_run_status()
Use the async_run_status() function to check the status of asynchronous run(s) using the unique run id(s).
Required argument:
* run_ids: Specifies the unique identifier(s) of the asynchronous run.
The following examples use the async_run_status() function to show the status of asynchronous run id for
Open Analytics Framework.
#### Example 1: Get the status of a single async run id
This example gets the status of an environment that has been removed asynchronously.
```python
>>> from teradataml import create_env, remove_env, async_run_status
>>> env = create_env("testenv1", "python_3.7.13","test env 1")
User environment 'testenv1' created.
>>> remove_env("testenv1", asynchronous=True)
Request to remove environment initiated successfully.
Check the status using list_user_envs(). If environment
is not removed, check the status of asynchronous
call using async_run_status('1ba43e0a-4285-41e1-8738-5f8895c180ee')
or get_env('testenv1').status('1ba43e0a-4285-41e1-8738-5f8895c180ee')
'1ba43e0a-4285-41e1-8738-5f8895c180ee'
>>> async_run_status('1ba43e0a-4285-41e1-8738-5f8895c180ee')
Run Id                      Run Description
Status             Timestamp Additional Details
0  1ba43e0a-4285-41e1-8738-5f8895c180ee  Remove 'testenv1' user environment.
Started  2023-08-31T09:27:06Z
```
#### Example 2: Get the status of multiple async run ids
This example gets the status of multiple asynchronous run ids for removed environments.
```python
>>> env1 = create_env("testenv1", "python_3.7.13","test env 1")
>>> env2 = create_env("testenv2", "python_3.7.13","test env 2")
User environment 'testenv1' created.
User environment 'testenv2' created.
# Remove 'testenv1' environment asynchronously.
>>> remove_env("testenv1", asynchronous=True)
Request to remove environment initiated successfully.
Check the status using list_user_envs(). If environment
is not removed, check the status of asynchronous
call using async_run_status('24812988-b124-45c7-80b1-6a4a826dc110')
or get_env('testenv1').status('24812988-b124-45c7-80b1-6a4a826dc110')
'24812988-b124-45c7-80b1-6a4a826dc110'
# Remove 'testenv2' environment asynchronously.
>>> remove_env("testenv2", asynchronous=True)
Request to remove environment initiated successfully.
Check the status using list_user_envs(). If environment
is not removed, check the status of asynchronous
call using async_run_status('f686d756-58bb-448b-81e2-979155cb8140')
or get_env('testenv2').status('f686d756-58bb-448b-81e2-979155cb8140')
'f686d756-58bb-448b-81e2-979155cb8140'
# Check the status of claim IDs for asynchronously installed libraries and
removed environments.
>>> async_run_status(['24812988-
b124-45c7-80b1-6a4a826dc110', 'f686d756-58bb-448b-81e2-979155cb8140'])
Run Id                       Run
Description    Status               Timestamp    Additional Details
0   24812988-b124-45c7-80b1-6a4a826dc110    Remove 'testenv1' user
environment.  Started    2023-08-31T04:00:44Z
1   24812988-b124-45c7-80b1-6a4a826dc110    Remove 'testenv1' user environment.
Finished    2023-08-31T04:00:45Z
2   f686d756-58bb-448b-81e2-979155cb8140    Remove 'testenv2' user
environment.  Started    2023-08-31T04:00:47Z
3   f686d756-58bb-448b-81e2-979155cb8140    Remove 'testenv2' user environment.
Finished    2023-08-31T04:00:48Z
```
### td_range()
Use the td_range() function to create a DataFrame with a specified range of numbers.
Note:
* The range is inclusive of the start and exclusive of the end.
* If only start is provided, then end is set to start and start is set to 0.
#### Required Parameter
start
    Specifies the starting number of the range.
#### Optional Parameters
end
    Specifies the end number of the range (exclusive).
    Default value: None
step
    Specifies the step size of the range.
    Default value: 1
#### Example 1: Create a DataFrame with a range of numbers from 0 to 5
```python
from teradataml.dataframe.functions import td_range
df = td_range(5)
df.sort('id')
id
0  0
1  1
2  2
3  3
4  4
```
#### Example 2: Create a DataFrame with a range of numbers from 5 to 1 with
#### default step size of -2
```python
from teradataml.dataframe.functions import td_range
td_range(5, 1, -2)
id
0  3
1  5
```
#### Example 3: Create a DataFrame with a range of numbers from 1 to 5 with
#### default step size of 1
```python
from teradataml.dataframe.functions import td_range
td_range(1, 5)
id
0  3
1  4
2  2
3  1
```
### get_formatters()
Use the get_formatters() function to get the formatters for NUMERIC, DATE and CHAR types.
#### Optional Parameter
formatter_type
    Specifies the category of formatter to format data.
    Default value: None
    Permitted values:
    * "NUMERIC" - Formatters to convert given data to a numeric type.
    * "DATE" - Formatters to convert given data to a date type.
    * "CHAR" - Formatters to convert given data to a character type.
#### Example: Get the formatters for the NUMERIC type
```python
>>> from teradataml.dataframe.functions import get_formatters
>>> get_formatters("NUMERIC")
```
## Apply SET Operations on teradataml DataFrames
teradataml offers various API's that allow users to perform set operations which internally uses Teradata
SET operators, to combine similar data sets or find difference of a list of teradataml DataFrames
or GeoDataFrames.
The SET operators are similar to the JOINs, the only difference is that JOIN combines the columns
from different DataFrames or GeoDataFrames, whereas SET operators combine rows from different
DataFrames or GeoDataFrames.
Supported SET operators include:
* concat
* td_except
* td_intersect
* td_minus
Note:
The data types of the columns which are being used in the SET operators must match.
### concat
Use the concat() API to concatenate a list of teradataml DataFrames, GeoDataFrames, or both, along the
index axis. The operation is performed by carrying out a database-style UNION or UNION ALL operation.
Note:
If the list contains both teradataml DataFrames and GeoDataFrames, that is, it contains geometry
data, the function returns a GeoDataFrame. See example 6.
#### Example Prerequisites
```python
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming admitted
id
22     yes  3.46    Novice    Beginner        0
36      no  3.00  Advanced      Novice        0
15     yes  4.00  Advanced    Advanced        1
38     yes  2.65  Advanced    Beginner        1
5       no  3.44    Novice      Novice        0
17      no  3.83  Advanced    Advanced        1
34     yes  3.85  Advanced    Beginner        0
13      no  4.00  Advanced      Novice        1
26     yes  3.57  Advanced    Advanced        1
19     yes  1.98  Advanced    Advanced        0
>>> df1 = df[df.gpa == 4].select(['id', 'stats', 'masters', 'gpa'])
>>> df1
stats masters  gpa
id
13  Advanced      no  4.0
29    Novice     yes  4.0
15  Advanced     yes  4.0
>>> df2 = df[df.gpa < 2].select(['id', 'stats', 'programming', 'admitted'])
>>> df2
stats programming admitted
id
24  Advanced      Novice        1
19  Advanced    Advanced        0
```
#### Example 1: Run concat() with default values for optional arguments
```python
>>> cdf = concat([df1,df2])
>>> cdf
stats masters  gpa programming admitted
id
19  Advanced    None  NaN    Advanced        0
24  Advanced    None  NaN      Novice        1
13  Advanced      no  4.0        None     None
29    Novice     yes  4.0        None     None
15  Advanced     yes  4.0        None     None
```
#### Example 2: Run concat() with optional argument "join"
Set join = inner
```python
>>> cdf = concat([df1,df2], join='inner')
>>> cdf
stats
id
19  Advanced
24  Advanced
13  Advanced
29    Novice
15  Advanced
```
#### Example 3: Run concat() with optional argument "allow_duplicates"
* Set allow_duplicates = True (default)
```python
>>> cdf = concat([df1,df2])
>>> cdf
stats masters  gpa programming admitted
id
19  Advanced    None  NaN    Advanced        0
24  Advanced    None  NaN      Novice        1
13  Advanced      no  4.0        None     None
29    Novice     yes  4.0        None     None
15  Advanced     yes  4.0        None     None
>>> cdf = concat([cdf,df2])
>>> cdf
stats masters  gpa programming admitted
id
19  Advanced    None  NaN    Advanced        0
13  Advanced      no  4.0        None     None
24  Advanced    None  NaN      Novice        1
24  Advanced    None  NaN      Novice        1
19  Advanced    None  NaN    Advanced        0
29    Novice     yes  4.0        None     None
15  Advanced     yes  4.0        None     None
```
* Set allow_duplicates = False
```python
>>> cdf = concat([cdf,df2], allow_duplicates=False)
>>> cdf
stats masters  gpa programming admitted
id
19  Advanced    None  NaN    Advanced        0
29    Novice     yes  4.0        None     None
24  Advanced    None  NaN      Novice        1
15  Advanced     yes  4.0        None     None
13  Advanced      no  4.0        None     None
```
#### Example 4: Run concat() with optional argument "sort"
Set sort=True
```python
>>> cdf = concat([df1,df2], sort=True)
>>> cdf
admitted  gpa masters programming     stats
id
19        0  NaN    None    Advanced  Advanced
24        1  NaN    None      Novice  Advanced
13     None  4.0      no        None  Advanced
29     None  4.0     yes        None    Novice
15     None  4.0     yes        None  Advanced
```
#### Example 5: Perform concatenation of two GeoDataFrames
* Create GeoDataFrames
```python
>>> geo_dataframe = GeoDataFrame('sample_shapes')
>>> geo_dataframe1 = geo_dataframe[geo_dataframe.skey
== 1004].select(['skey','linestrings'])
>>> geo_dataframe1
skey            linestrings
1004  LINESTRING (10 20 30,40 50 60,70 80 80)
```
  >>> geo_dataframe2 = geo_dataframe[geo_dataframe.skey < 1010].select(['skey','polygons'])
  >>> geo_dataframe2
  skey                                                              polygons
  1009                               MULTIPOLYGON (((0 0 0,0 20 20,20 20 20,20 0 20,0 0 0)),((50 50 50,50 90
  90,90 90 90,90 50 90,50 50 50)))
  1005  POLYGON ((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.435,20.435 20.435
  0,20.435 20.435 20.435,0 0 0))
  1004                                                POLYGON ((0 0 0,0 10 20,20 20 30,20 10 0,0 0 0),(5 5 5,5
  10 10,10 10 10,10 10 5,5 5 5))
  1002                                                                          POLYGON ((0 0,0 20,20 20,20 0,0
  0),(5 5,5 10,10 10,10 5,5 5))
  1001
  POLYGON ((0 0,0 20,20 20,20 0,0 0))
  1003                                                                                POLYGON ((0.6 0.8,0.6
  20.8,20.6 20.8,20.6 0.8,0.6 0.8))
  1007                                                                  MULTIPOLYGON (((1 1,1 3,6 3,6 0,1 1)),
  ((10 5,10 10,20 10,20 5,10 5)))
  1006                                                          POLYGON ((0 0 0,0 0 20,0 20 0,0 20 20,20 0 0,20
  0 20,20 20 0,20 20 20,0 0 0))
  1008                                             MULTIPOLYGON (((0 0,0 20,20 20,20 0,0 0)),((0.6 0.8,0.6
  20.8,20.6 20.8,20.6 0.8,0.6 0.8)))
* Perform concatenation
  >>> concat([geo_dataframe1,geo_dataframe2])
  skey                                    linestrings                                 polygons
  1009                                     None                               MULTIPOLYGON (((0 0 0,0 20 20,20 20
  20,20 0 20,0 0 0)),((50 50 50,50 90 90,90 90 90,90 50 90,50 50 50)))
  1005                                     None  POLYGON ((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435
  0.0 0,20.435 0.0 20.435,20.435 20.435 0,20.435 20.435 20.435,0 0 0))
  1004  LINESTRING (10 20 30,40 50 60,70 80
  80)
  None
  1004                                     None                                                POLYGON ((0 0 0,0
  10 20,20 20 30,20 10 0,0 0 0),(5 5 5,5 10 10,10 10 10,10 10 5,5 5 5))
  1003                                     None
  POLYGON ((0.6 0.8,0.6 20.8,20.6 20.8,20.6 0.8,0.6 0.8))
  1001
  None                                                                                                    POLYGON
  ((0 0,0 20,20 20,20 0,0 0))
  1002                                     None
  POLYGON ((0 0,0 20,20 20,20 0,0 0),(5 5,5 10,10 10,10 5,5 5))
  1007                                     None
  MULTIPOLYGON (((1 1,1 3,6 3,6 0,1 1)),((10 5,10 10,20 10,20 5,10 5)))
  1006                                     None                                                          POLYGON
  ((0 0 0,0 0 20,0 20 0,0 20 20,20 0 0,20 0 20,20 20 0,20 20 20,0 0 0))
  1008                                     None                                             MULTIPOLYGON (((0 0,0
  20,20 20,20 0,0 0)),((0.6 0.8,0.6 20.8,20.6 20.8,20.6 0.8,0.6 0.8)))
#### Example 6: Perform concatenation of a DataFrame and GeoDataFrame
```python
>>> normal_df=df.select(['id','stats'])
>>> normal_df
stats
id
34  Advanced
32  Advanced
11  Advanced
40    Novice
38  Advanced
36  Advanced
7     Novice
26  Advanced
19  Advanced
13  Advanced
```
  >>> geo_df = geo_dataframe[geo_dataframe.skey < 1010].select(['skey', 'polygons'])
  >>> geo_df
  skey                                                                            polygons
  1003                                                                                POLYGON ((0.6 0.8,0.6
  20.8,20.6 20.8,20.6 0.8,0.6 0.8))
  1008                                             MULTIPOLYGON (((0 0,0 20,20 20,20 0,0 0)),((0.6 0.8,0.6
  20.8,20.6 20.8,20.6 0.8,0.6 0.8)))
  1006                                                          POLYGON ((0 0 0,0 0 20,0 20 0,0 20 20,20 0 0,20
  0 20,20 20 0,20 20 20,0 0 0))
  1009                               MULTIPOLYGON (((0 0 0,0 20 20,20 20 20,20 0 20,0 0 0)),((50 50 50,50 90
  90,90 90 90,90 50 90,50 50 50)))
  1005  POLYGON ((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.435,20.435 20.435
  0,20.435 20.435 20.435,0 0 0))
  1007                                                                  MULTIPOLYGON (((1 1,1 3,6 3,6 0,1 1)),
  ((10 5,10 10,20 10,20 5,10 5)))
  1001
  POLYGON ((0 0,0 20,20 20,20 0,0 0))
  1002                                                                          POLYGON ((0 0,0 20,20 20,20 0,0
  0),(5 5,5 10,10 10,10 5,5 5))
  1004                                                POLYGON ((0 0 0,0 10 20,20 20 30,20 10 0,0 0 0),(5 5 5,5
  10 10,10 10 10,10 10 5,5 5 5))
```python
>>> idf = concat([normal_df,geo_df])
>>> idf
stats     skey   polygons
id
38  Advanced  None     None
7     Novice  None     None
26  Advanced  None     None
17  Advanced  None     None
34  Advanced  None     None
13  Advanced  None     None
32  Advanced  None     None
11  Advanced  None     None
15  Advanced  None     None
36  Advanced  None     None
```
### td_except
Use the td_except() function to return the rows that appear in the first teradataml DataFrame or
GeoDataFrame, and not in other teradataml DataFrames or GeoDataFrames, along the index axis.
Note:
This function must be applied to data frames of the same type: either all teradataml DataFrames, or
all GeoDataFrames.
#### Example Prerequisites
```python
>>> from teradataml import load_example_data
>>> load_example_data("dataframe", "setop_test1")
>>> load_example_data("dataframe", "setop_test2")
>>> from teradataml.dataframe import dataframe
>>> from teradataml.dataframe.setop import td_except
```
#### Example 1: Run td_except() on rows from two DataFrames, using
#### default signature
This example applies the except operation on rows from two teradataml DataFrames when using default
signature of the function.
```python
>>> df1 = DataFrame('setop_test1')
>>> df1
masters   gpa     stats programming  admitted
id
62      no  3.70  Advanced    Advanced         1
53     yes  3.50  Beginner      Novice         1
69      no  3.96  Advanced    Advanced         1
61     yes  4.00  Advanced    Advanced         1
58      no  3.13  Advanced    Advanced         1
51     yes  3.76  Beginner    Beginner         0
68      no  1.87  Advanced      Novice         1
66      no  3.87    Novice    Beginner         1
60      no  4.00  Advanced      Novice         1
59      no  3.65    Novice      Novice         1
>>> df2 = DataFrame('setop_test2')
>>> df2
masters   gpa     stats programming  admitted
id
12      no  3.65    Novice      Novice         1
15     yes  4.00  Advanced    Advanced         1
14     yes  3.45  Advanced    Advanced         0
20     yes  3.90  Advanced    Advanced         1
18     yes  3.81  Advanced    Advanced         1
17      no  3.83  Advanced    Advanced         1
13      no  4.00  Advanced      Novice         1
11      no  3.13  Advanced    Advanced         1
60      no  4.00  Advanced      Novice         1
19     yes  1.98  Advanced    Advanced         0
>>> idf = td_except([df1[df1.id<55] , df2])
>>> idf
masters   gpa     stats programming  admitted
id
51     yes  3.76  Beginner    Beginner         0
50     yes  3.95  Beginner    Beginner         0
54     yes  3.50  Beginner    Advanced         1
52      no  3.70    Novice    Beginner         1
53     yes  3.50  Beginner      Novice         1
53     yes  3.50  Beginner      Novice         1
```
#### Example 2: Run td_except() on rows from two DataFrames, discarding
#### duplicate rows
This examples applies the except operation on rows from the two teradataml DataFrames from previous
example, discarding duplicate rows from the result by passing allow_duplicates = False.
```python
>>> idf = td_except([df1[df1.id<55] , df2], allow_duplicates=False)
>>> idf
masters   gpa     stats programming  admitted
id
54     yes  3.50  Beginner    Advanced         1
51     yes  3.76  Beginner    Beginner         0
53     yes  3.50  Beginner      Novice         1
50     yes  3.95  Beginner    Beginner         0
52      no  3.70    Novice    Beginner         1
```
#### Example 3: Run td_except() on more than two DataFrames
This example shows what happens when td_except is used on more than two teradataml DataFrames. In
this example, you have three teradataml DataFrames as df1, df2 & df3, the operation is applied on df1 and
df2 first, and then the operation is applied again on the result and df3.
```python
>>> df3 = df1[df1.gpa <= 3.9]
>>> # Effective operation here would be, (df1-df2)-df3
>>> idf = td_except([df1, df2, df3])
>>> idf
masters   gpa     stats programming  admitted
id
61     yes  4.00  Advanced    Advanced         1
50     yes  3.95  Beginner    Beginner         0
69      no  3.96  Advanced    Advanced         1
```
#### Example 4: Run td_except on two GeoDataFrames
* Create GeoDataFrames
```python
>>> geo_dataframe = GeoDataFrame('sample_shapes')
>>> geo_dataframe1 = geo_dataframe[geo_dataframe.skey
== 1004].select(['skey','linestrings'])
>>> geo_dataframe1
skey            linestrings
1004  LINESTRING (10 20 30,40 50 60,70 80 80)
>>> geo_dataframe2 = geo_dataframe[geo_dataframe.skey
< 1010].select(['skey','linestrings'])
>>> geo_dataframe2
skey                        linestrings
1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
1004                                LINESTRING (10 20 30,40 50 60,70 80 80)
1002                                               LINESTRING (1 3,3 0,0 1)
1001                                           LINESTRING (1 1,2 2,3 3,4 4)
1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
```
* Run td_except
```python
>>> td_except([geo_dataframe2,geo_dataframe1])
skey                    linestrings
1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
1001                                           LINESTRING (1 1,2 2,3 3,4 4)
1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
1002                                               LINESTRING (1 3,3 0,0 1)
```
### td_intersect
Use the td_intersect() function to find the data at the intersection of the list of teradataml DataFrames or
GeoDataFrames along the index axis, and returns a DataFrame or a GeoDataFrame with rows common
to all input DataFrames or GeoDataFrames.
Note:
This function must be applied to data frames of the same type: either all teradataml DataFrames, or
all GeoDataFrames.
#### Example Prerequisites
```python
>>> from teradataml import load_example_data
>>> load_example_data("dataframe", "setop_test1")
>>> load_example_data("dataframe", "setop_test2")
>>> from teradataml.dataframe import dataframe
>>> from teradataml.dataframe.setop import td_intersect
```
#### Example 1: Run td_intersect() on rows from two DataFrames, using
#### default signature
This example gets the intersection of rows from two teradataml DataFrames when using default signature
of the function.
```python
>>> df1 = DataFrame('setop_test1')
>>> df1
masters   gpa     stats programming  admitted
id
62      no  3.70  Advanced    Advanced         1
53     yes  3.50  Beginner      Novice         1
69      no  3.96  Advanced    Advanced         1
61     yes  4.00  Advanced    Advanced         1
58      no  3.13  Advanced    Advanced         1
51     yes  3.76  Beginner    Beginner         0
68      no  1.87  Advanced      Novice         1
66      no  3.87    Novice    Beginner         1
60      no  4.00  Advanced      Novice         1
59      no  3.65    Novice      Novice         1
>>> df2 = DataFrame('setop_test2')
>>> df2
masters   gpa     stats programming  admitted
id
12      no  3.65    Novice      Novice         1
15     yes  4.00  Advanced    Advanced         1
14     yes  3.45  Advanced    Advanced         0
20     yes  3.90  Advanced    Advanced         1
18     yes  3.81  Advanced    Advanced         1
17      no  3.83  Advanced    Advanced         1
13      no  4.00  Advanced      Novice         1
11      no  3.13  Advanced    Advanced         1
60      no  4.00  Advanced      Novice         1
19     yes  1.98  Advanced    Advanced         0
>>> idf = td_intersect([df1, df2])
>>> idf
masters   gpa     stats programming  admitted
id
64     yes  3.81  Advanced    Advanced         1
60      no  4.00  Advanced      Novice         1
58      no  3.13  Advanced    Advanced         1
68      no  1.87  Advanced      Novice         1
66      no  3.87    Novice    Beginner         1
60      no  4.00  Advanced      Novice         1
62      no  3.70  Advanced    Advanced         1
```
#### Example 2: Run td_intersect() on rows from two DataFrames, discarding
#### duplicate rows
This examples applies the intersect operation on rows from the two teradataml DataFrames from previous
example, discarding duplicate rows from the result by passing allow_duplicates = False.
```python
>>> idf = td_intersect([df1, df2], allow_duplicates=False)
>>> idf
masters   gpa     stats programming  admitted
id
64     yes  3.81  Advanced    Advanced         1
60      no  4.00  Advanced      Novice         1
58      no  3.13  Advanced    Advanced         1
68      no  1.87  Advanced      Novice         1
66      no  3.87    Novice    Beginner         1
62      no  3.70  Advanced    Advanced         1
```
#### Example 3: Run td_intersect() on more than two DataFrames
This example shows what happens when td_intersect is used on more than two teradataml DataFrames.
In this example, you have three teradataml DataFrames as df1, df2 & df3, the operation is applied on df1
& df2 first, and then the operation is applied again on the result & df3.
```python
>>> df3 = df1[df1.gpa <= 3.5]
>>> df3
masters   gpa     stats programming  admitted
id
58      no  3.13  Advanced    Advanced         1
67     yes  3.46    Novice    Beginner         0
54     yes  3.50  Beginner    Advanced         1
68      no  1.87  Advanced      Novice         1
53     yes  3.50  Beginner      Novice         1
>>> # Effective operation here would be, (df1-df2)-df3
>>> idf = td_intersect([df1, df2, df3])
>>> idf
masters   gpa     stats programming  admitted
id
58      no  3.13  Advanced    Advanced         1
68      no  1.87  Advanced      Novice         1
```
#### Example 4: Perform intersection of two GeoDataFrames
* Create GeoDataFrames
```python
>>> geo_dataframe = GeoDataFrame('sample_shapes')
>>> geo_dataframe1 = geo_dataframe[geo_dataframe.skey
== 1004].select(['skey','linestrings'])
>>> geo_dataframe1
skey            linestrings
1004  LINESTRING (10 20 30,40 50 60,70 80 80)
>>> geo_dataframe2 = geo_dataframe[geo_dataframe.skey
< 1010].select(['skey','linestrings'])
>>> geo_dataframe2
skey                        linestrings
1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
1004                                LINESTRING (10 20 30,40 50 60,70 80 80)
1002                                               LINESTRING (1 3,3 0,0 1)
1001                                           LINESTRING (1 1,2 2,3 3,4 4)
1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
```
* Perform intersection
```python
>>> td_intersect([geo_dataframe1,geo_dataframe2])
skey            linestrings
1004  LINESTRING (10 20 30,40 50 60,70 80 80)
```
### td_minus
Use the td_minus() function to return the rows that appear in the first teradataml DataFrame or
GeoDataFrame, and not in other teradataml DataFrames or GeoDataFrames, along the index axis.
Note:
This function must be applied to data frames of the same type: either all teradataml DataFrames, or
all GeoDataFrames.
#### Example Prerequisites
```python
>>> from teradataml import load_example_data
>>> load_example_data("dataframe", "setop_test1")
>>> load_example_data("dataframe", "setop_test2")
>>> from teradataml.dataframe import dataframe
>>> from teradataml.dataframe.setop import td_minus
```
#### Example 1: Run td_minus() on rows from two DataFrames, using
#### default signature
This example applies the minus operation on rows from two teradataml DataFrames when using default
signature of the function.
```python
>>> df1 = DataFrame('setop_test1')
>>> df1
masters   gpa     stats programming  admitted
id
62      no  3.70  Advanced    Advanced         1
53     yes  3.50  Beginner      Novice         1
69      no  3.96  Advanced    Advanced         1
61     yes  4.00  Advanced    Advanced         1
58      no  3.13  Advanced    Advanced         1
51     yes  3.76  Beginner    Beginner         0
68      no  1.87  Advanced      Novice         1
66      no  3.87    Novice    Beginner         1
60      no  4.00  Advanced      Novice         1
59      no  3.65    Novice      Novice         1
>>> df2 = DataFrame('setop_test2')
>>> df2
masters   gpa     stats programming  admitted
id
12      no  3.65    Novice      Novice         1
15     yes  4.00  Advanced    Advanced         1
14     yes  3.45  Advanced    Advanced         0
20     yes  3.90  Advanced    Advanced         1
18     yes  3.81  Advanced    Advanced         1
17      no  3.83  Advanced    Advanced         1
13      no  4.00  Advanced      Novice         1
11      no  3.13  Advanced    Advanced         1
60      no  4.00  Advanced      Novice         1
19     yes  1.98  Advanced    Advanced         0
>>> idf = td_minus([df1[df1.id<55] , df2])
>>> idf
masters   gpa     stats programming  admitted
id
51     yes  3.76  Beginner    Beginner         0
50     yes  3.95  Beginner    Beginner         0
54     yes  3.50  Beginner    Advanced         1
52      no  3.70    Novice    Beginner         1
53     yes  3.50  Beginner      Novice         1
53     yes  3.50  Beginner      Novice         1
```
#### Example 2: Run td_minus() on rows from two DataFrames, discarding
#### duplicate rows
This examples applies the minus operation on rows from the two teradataml DataFrames from previous
example, discarding duplicate rows from the result by passing allow_duplicates = False.
```python
>>> idf = td_minus([df1[df1.id<55] , df2], allow_duplicates=False)
>>> idf
masters   gpa     stats programming  admitted
id
54     yes  3.50  Beginner    Advanced         1
51     yes  3.76  Beginner    Beginner         0
53     yes  3.50  Beginner      Novice         1
50     yes  3.95  Beginner    Beginner         0
52      no  3.70    Novice    Beginner         1
```
#### Example 3: Run td_minus() on more than two DataFrames
This example shows what happens when td_minus is used on more than two teradataml DataFrames. In
this example, you have three teradataml DataFrames as df1, df2 & df3, the operation is applied on df1 and
df2 first, and then the operation is applied again on the result and df3.
```python
>>> df3 = df1[df1.gpa <= 3.9]
>>> idf = td_minus([df1, df2, df3])
>>> idf
masters   gpa     stats programming  admitted
id
61     yes  4.00  Advanced    Advanced         1
50     yes  3.95  Beginner    Beginner         0
69      no  3.96  Advanced    Advanced         1
>>>
```
#### Example 4: Run td_minus on two GeoDataFrames
* Create GeoDataFrames
```python
>>> geo_dataframe = GeoDataFrame('sample_shapes')
>>> geo_dataframe1 = geo_dataframe[geo_dataframe.skey
== 1004].select(['skey','linestrings'])
>>> geo_dataframe1
skey            linestrings
1004  LINESTRING (10 20 30,40 50 60,70 80 80)
>>> geo_dataframe2 = geo_dataframe[geo_dataframe.skey
< 1010].select(['skey','linestrings'])
>>> geo_dataframe2
skey                        linestrings
1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
1004                                LINESTRING (10 20 30,40 50 60,70 80 80)
1002                                               LINESTRING (1 3,3 0,0 1)
1001                                           LINESTRING (1 1,2 2,3 3,4 4)
1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
```
* Run td_minus
```python
>>> td_minus([geo_dataframe2,geo_dataframe1])
skey                    linestrings
1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
1002                                               LINESTRING (1 3,3 0,0 1)
1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
1001                                           LINESTRING (1 1,2 2,3 3,4 4)
```