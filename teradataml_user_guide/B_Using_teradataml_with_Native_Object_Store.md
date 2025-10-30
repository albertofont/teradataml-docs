# Appendix B: Using teradataml with Native Object Store

Native Object Store (NOS) is a new capability included with Teradata Vantage that makes it easy for users
to explore datasets that have been stored in JSON, comma-separated values (CSV), or Parquet format,
located on external object storage like AWS S3 and Azure Blob Storage, using standard SQL. Because
the data is being processed and analyzed inside the database, Native Object Store can take advantage
of the scalability, performance, and workload management that is part of the Vantage platform. No special
object storage-side compute infrastructure is required to use Native Object Store. Simply create a Native
Object Store table definition within the Database Engine 20, point it at any external object storage bucket
that you are authorized to access, and within minutes, you can explore the data located in that bucket using
all the analytics functionality of Vantage. As a result, Native Object Store is ideally suited for data scientists,
analysts, and other business users that want to use object stores on an ad-hoc basis to store interim results,
prior versions, and other data sets as part of their analytics workflow.
You can use teradataml to explore this external data made available on Vantage via Native Object Store
capability. This section shows how to use teradataml to explore data stored on external objects with the help
of Native Object Store. The first step is to create a foreign table. The foreign table allows the external data to
be easily referenced within the Database Engine 20 and makes the data available in a structured relational
format which could include complex data types. Once the data is in a relational format, either persistently or
virtually, it can be aggregated or joined to other relational tables.
Refer to the Native Object Store documentation for more details about NOS.
The following sections show different ways to explore foreign table data in Vantage via teradataml, based on
the actual format of the data. You may refer to the examples in the Teradata Vantage™ - Native Object Store
Getting Started Guide.
## Data in JSON or CSV Format
Foreign table created in JSON or comma-separated values (CSV) format typically contains two columns:
* Location
* Payload
You can create a teradataml DataFrame on a foreign table using "DataFrame()" or
"DataFrame.from_table()", the same way to create a teradataml DataFrame on a regular table. With
the created DataFrame, you can easily access the data in these columns and process the data using
teradataml DataFrame API or other Python packages.
* Example: Create teradataml DataFrame on NOS foreign table in JSON data format
* Example: Create teradataml DataFrame on NOS foreign table in CSV data format
Though teradataml provides direct access to the data in foreign tables, actual data that resides in 'Payload'
column and path variables for the foreign table can be accessed in one of the following ways:
* Accessing Columns and Path Variables using DataFrame.from_query()
* Accessing Columns and Path Variables by Creating a View on Foreign Table
* Accessing Columns and Path Variables by Creating a Table from Foreign Table
### Example: Create teradataml DataFrame on NOS foreign table in
### JSON data format
Assume that the following foreign table has been created on JSON data in Amazon S3 bucket:
```python
CREATE MULTISET FOREIGN TABLE riverflow ,FALLBACK ,
EXTERNAL SECURITY DEFINER TRUSTED AUTH_OBJECT ,
MAP = TD_MAP1
Location VARCHAR(2048) CHARACTER SET UNICODE CASESPECIFIC,
Payload JSON(8388096) INLINE LENGTH 32000 CHARACTER SET UNICODE)
USING
LOCATION  ('/s3/s3.amazonaws.com/td-usgs/DATA/')
PATHPATTERN  ('$data/$siteno/$year/$month/$day')
);
```
* Create a dataframe on foreign table.
```python
# Create dataframe on foreign table which contains data in JSON format on S3
>>> riverflow = DataFrame.from_table("riverflow")
```
* Display the columns in the DataFrame.
```python
# As seen from create table statement, table has two columns 'Location'
and 'Payload'
>>> riverflow.columns
['Location', 'Payload']
```
* Check the types of the columns.
```python
# Let's check their types
>>> riverflow.dtypes
Location    str
Payload     str
```
* Print the content of the table.
```python
# Let's print the DataFrame.
# Equivalent SQL:
#      SELECT * FROM riverflow
>>> riverflow.head().to_pandas()
```
    Location                                                Payload
  0 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
  1 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
  2 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
  3 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
  4 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
  5 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
  6 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
  7 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
  8 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
  9 /S3/s3.amazonaws.com/td-usgs/DATA/09380000/201... { "site_no":"09380000",
  "datetime":"2018-06-27...
### Example: Create teradataml DataFrame on NOS foreign table in
### CSV data format
Assume that the following foreign table has been created on CSV data in Amazon S3 bucket:
```python
CREATE FOREIGN TABLE riverflowcsv
, EXTERNAL SECURITY DEFINER TRUSTED AUTH_OBJECT
Location VARCHAR(2048) CHARACTER SET UNICODE CASESPECIFIC,
PAYLOAD DATASET INLINE LENGTH 64000 STORAGE FORMAT CSV)
USING
LOCATION('/s3/s3.amazonaws.com/td-usgs/CSVDATA/')
PATHPATTERN  ('$data/$siteno/$year/$month/$day')
);
```
* Create a dataframe on foreign table.
```python
# Create dataframe on foreign table which contains data in CSV format on S3
>>> riverflow = DataFrame.from_table("riverflowcsv")
```
* Display the columns in the DataFrame.
```python
# As seen from create table statement, table has two columns 'Location'
and 'Payload'
>>> riverflow.columns
['Location', 'Payload']
```
* Check the types of the columns.
```python
# Let's check their types
>>> riverflow.dtypes
Location    str
Payload     str
```
* Print the content of the table.
```python
# Let's print the DataFrame.
# Equivalent SQL:
#      SELECT * FROM riverflow
>>> riverflow.head().to_pandas()
```
    Location                                   PAYLOAD
  0 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
  1 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
  2 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
  3 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
  4 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
  5 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
  6 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
  7 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
  8 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
  9 /S3/s3.amazonaws.com/td-usgs/CSVDATA/09380000/...
  Temp,Flow,site_no,datetime,Conductance,Precipi...
### Accessing Columns and Path Variables
### using DataFrame.from_query()
User can access actual columns and path variables using DataFrame.from_query() that pass a SELECT
query with each column from JSON or CSV data projected from foreign table. Each column must be
typecast to a valid type and then aliased to the appropriate column name. This allows user to access
actual columns and keys in the JSON or CSV data. It is up to the user on what must be selected in
SELECT query passed to "DataFrame.from_query()": columns, attributes, keys from JSON or CSV data
and path variables.
#### Example for JSON data
SELECT query with each column from JSON selected.
```python
# Select query with each column from JSON selected. Each column is type casted
to a valid type and
# then aliased to the required column name. Notice, we are selecting each
attribute including path variables.
query = """SELECT CAST($path.$siteno AS CHAR(10)) TheSite,
CAST($path.$year AS CHAR(4)) TheYear,
CAST($path.$month AS CHAR(2)) TheMonth,
CAST($path.$day AS CHAR(2)) TheDay,
CAST(payload.site_no AS CHAR(8)) Site_no,
CAST(payload.Flow AS FLOAT) Flow,
CAST(payload.GageHeight AS FLOAT) GageHeight1,
CAST(payload.Precipitation AS FLOAT) Precipitation,
CAST(payload.Temp AS FLOAT) Temperature,
CAST(payload.Velocity AS FLOAT) Velocity,
CAST(payload.BatteryVoltage AS FLOAT) BatteryVoltage,
CAST(payload.GageHeight2 AS FLOAT) GageHeight2
FROM riverflow"""
```
Create DataFrame from the query and display the head of the DataFrame.
```python
>>> wrk1df = DataFrame.from_query(query)
>>> wrk1df.head().to_pandas()
```
TheSite   TheYear TheMonth TheDay Site_no  Flow GageHeight1 Precipitation Temperature Velocity BatteryVoltage
GageHeight2
0 09380000 2018 07 14 09380000 11400.0 8.99  0.0  11.4     None
None        None
1 09380000 2018 07 27 09380000 11100.0 8.91  0.0  11.0     None
None        None
2 09380000 2018 07 25 09380000 18600.0 10.41  0.0 11.7     None
None        None
3 09380000 2018 06 29 09380000 16700.0 10.07  0.0 11.3     None
None        None
4 09380000 2018 07 03 09380000 18900.0 10.46  0.0 11.0     None
None        None
5 09380000 2018 07 13 09380000 11000.0 8.90  0.0  10.8     None
None        None
6 09380000 2018 07 15 09380000 11400.0 8.99  0.0  12.0     None
None        None
7 09380000 2018 07 18 09380000 11300.0 8.96  0.0  10.7     None
None        None
8 09380000 2018 06 30 09380000     0.0 0.00  0.0  11.6     None
None        None
9 09380000 2018 07 16 09380000 16700.0 10.07  0.0 11.0     None
None        None
#### Example for CSV data
SELECT query with each column from CSV selected.
```python
# Select query with each column from CSV selected. Each column is type casted
to a valid type and
# then aliased to the required column name. Notice, we are selecting each
attribute including path variables.
query = """SELECT CAST($path.$siteno AS CHAR(10)) TheSite,
CAST($path.$year AS CHAR(4)) TheYear,
CAST($path.$month AS CHAR(2)) TheMonth,
CAST($path.$day AS CHAR(2)) TheDay,
CAST(payload..site_no AS CHAR(8)) Site_no,
CAST(payload..Flow AS FLOAT) Flow,
CAST(payload..GageHeight AS FLOAT) GageHeight1,
CAST(payload..Precipitation AS FLOAT) Precipitation,
CAST(payload..Temp AS FLOAT) Temperature,
CAST(payload..Velocity AS FLOAT) Velocity,
CAST(payload..BatteryVoltage AS FLOAT) BatteryVoltage,
CAST(payload..GageHeight2 AS FLOAT) GageHeight2
FROM riverflowcsv"""
```
Create DataFrame from the query and display the head of the DataFrame.
```python
>>> wrk1df = DataFrame.from_query(query)
>>> wrk1df.head().to_pandas()
```
    TheSite TheYear TheMonth TheDay Site_no Flow GageHeight1 Precipitation Temperature Velocity
BatteryVoltage GageHeight2
0 09380000 2018 06 30 09380000 11700.0 9.04   0.0 10.0     None
None        None
1 09380000 2018 07 04 09380000 11400.0 8.99   0.0 11.9     None
None        None
2 09380000 2018 07 11 09380000 18700.0 10.42   0.0 11.2     None
None        None
3 09380000 2018 06 29 09380000 11600.0 9.02   0.0 10.2     None
None        None
4 09380000 2018 07 06 09380000 16600.0 10.05   0.0 10.8     None
None        None
5 09380000 2018 07 22 09380000 11500.0 9.00   0.0 10.9     None
None        None
6 09380000 2018 07 11 09380000 15200.0 9.79   0.0 10.9     None
None        None
7 09380000 2018 06 30 09380000 10000.0 8.65   0.0 11.7     None
None        None
8 09380000 2018 07 15 09380000 11400.0 8.98   0.0 11.8     None
None        None
9 09380000 2018 06 28 09380000 17000.0 10.12  0.0 11.6     None
None        None
### Accessing Columns and Path Variables by Creating a View on
### Foreign Table
User can access actual columns and path variables by creating a view using a SELECT query with each
column from JSON or CSV data projected from foreign table. Each column must be typecast to a valid
type and then aliased to the appropriate column name. This allows user to access actual columns and
keys in the JSON or CSV data. It is up to the user on what must be selected in SELECT query passed to
"DataFrame.from_query()": columns, attributes, keys from JSON or CSV data and path variables.
#### Example for JSON data
Create a view.
```python
# While creating a view select each column is type casted to a valid type and
# then aliased to the required column name. Notice, we are selecting each
attribute including path variables.
# Following is the VIEW created at the backend:
"""
REPLACE VIEW riverflowview AS (
SELECT CAST($path.$siteno AS CHAR(10)) TheSite,
CAST($path.$year AS CHAR(4)) TheYear,
CAST($path.$month AS CHAR(2)) TheMonth,
CAST($path.$day AS CHAR(2)) TheDay,
CAST(payload.site_no AS CHAR(8)) Site_no,
CAST(payload.Flow AS FLOAT) Flow,
CAST(payload.GageHeight AS FLOAT) GageHeight1,
CAST(payload.Precipitation AS FLOAT) Precipitation,
CAST(payload.Temp AS FLOAT) Temperature,
CAST(payload.Velocity AS FLOAT) Velocity,
CAST(payload.BatteryVoltage AS FLOAT) BatteryVoltage,
CAST(payload.GageHeight2 AS FLOAT) GageHeight2
FROM riverflow);
"""
```
Create a DataFrame on the view and display the head of the DataFrame.
```python
# Create a DataFrame on a view.
>>> wrk2dfview = DataFrame("riverflowview")
>>> wrk2dfview.head().to_pandas()
```
    TheSite TheYear TheMonth TheDay Site_no Flow GageHeight1 Precipitation Temperature
Velocity BatteryVoltage GageHeight2
0 09380000 2018 07 17 09380000 18300.0 10.36 0.0 12.0 None None None
1 09380000 2018 07 11 09380000 18000.0 10.31 0.0 11.2 None None None
2 09380000 2018 07 11 09380000 18500.0 10.40 0.0 11.5 None None None
3 09380000 2018 07 06 09380000 19000.0 10.47 0.0 11.5 None None None
4 09380000 2018 07 14 09380000 11500.0 9.01 0.0 10.3 None None None
5 09380000 2018 07 04 09380000 11400.0 8.98 0.0 11.9 None None None
6 09380000 2018 07 01 09380000 11100.0 8.92 0.0 10.7 None None None
7 09380000 2018 06 29 09380000 16700.0 10.07 0.0 11.3 None None None
8 09380000 2018 07 25 09380000 18100.0 10.33 0.0 11.9 None None None
9 09380000 2018 07 02 09380000 18800.0 10.44 0.0 11.4 None None None
#### Example for CSV data
Create a view.
```python
# While creating a view select each column is type casted to a valid type and
# then aliased to the required column name. Notice, we are selecting each
attribute including path variables.
# Following is the VIEW created at the backend:
"""
REPLACE VIEW riverflowcsvview AS (
SELECT CAST($path.$siteno AS CHAR(10)) TheSite,
CAST($path.$year AS CHAR(4)) TheYear,
CAST($path.$month AS CHAR(2)) TheMonth,
CAST($path.$day AS CHAR(2)) TheDay,
CAST(payload..site_no AS CHAR(8)) Site_no,
CAST(payload..Flow AS FLOAT) Flow,
CAST(payload..GageHeight AS FLOAT) GageHeight1,
CAST(payload..Precipitation AS FLOAT) Precipitation,
CAST(payload..Temp AS FLOAT) Temperature,
CAST(payload..Velocity AS FLOAT) Velocity,
CAST(payload..BatteryVoltage AS FLOAT) BatteryVoltage,
CAST(payload..GageHeight2 AS FLOAT) GageHeight2
FROM riverflowcsv);
"""
```
Create DataFrame on the view and display the head of the DataFrame.
```python
# Create a DataFrame on a view.
>>> wrk2dfview = DataFrame("riverflowcsvview")
>>> wrk2dfview.head().to_pandas()
```
TheSite TheYear TheMonth TheDay Site_no Flow GageHeight1 Precipitation Temperature Velocity
BatteryVoltage GageHeight2
0 09380000 2018 07 18 09380000 18700.0 10.43 0.0 11.4 None None None
1 09380000 2018 06 29 09380000 16800.0 10.09 0.0 11.0 None None None
2 09380000 2018 07 06 09380000 11100.0 8.90 0.0 10.5 None None None
3 09380000 2018 07 10 09380000 15900.0 9.92 0.0 11.0 None None None
4 09380000 2018 07 12 09380000 18400.0 10.37 0.0 11.2 None None None
5 09380000 2018 07 12 09380000 11500.0 9.00 0.0 11.0 None None None
6 09380000 2018 06 28 09380000 11500.0 9.00 0.0 10.5 None None None
7 09380000 2018 07 12 09380000 11000.0 8.89 0.0 11.0 None None None
8 09380000 2018 07 10 09380000 15400.0 9.82 0.0 10.5 None None None
9 09380000 2018 07 26 09380000 18800.0 10.45 0.0 11.6 None None None
### Accessing Columns and Path Variables by Creating a Table from
### Foreign Table
User can access actual columns and path variables by creating a regular table on a SELECT query (Create
Table as SELECT, CTAS) with each column from JSON or CSV data selected from foreign table. Each
column must be typecast to a valid type and then aliased to the appropriate column name. This allows user
to access actual columns and keys in the JSON or CSV data. It is up to the user on what must be selected
in SELECT query passed to "DataFrame.from_query()": columns, attributes, keys from JSON or CSV data
and path variables.
#### Example for JSON data
Create a regular table.
```python
# Here is how we created a regular table from foreign table.
"""
CREATE TABLE riverflowprecip AS (
SELECT CAST($path.$year AS CHAR(4)) TheYear,
CAST($path.$month AS CHAR(2)) TheMonth,
CAST($path.$day AS CHAR(2)) TheDay,
CAST(payload.site_no AS CHAR(8)) SiteNo,
CAST(payload.Flow AS FLOAT) Flow,
CAST(payload.GageHeight AS FLOAT) GageHeight
FROM riverflow
WHERE payload.Precipitation IS NOT NULL )
WITH DATA
PRIMARY INDEX(SiteNo)
"""
```
Create a DataFrame on the table and display the head of the DataFrame.
```python
# Create a DataFrame on a table.
>>> wrk2dfview = DataFrame("riverflowprecip")
>>> wrk2dfview.head().to_pandas()
TheMonth TheDay SiteNo Flow GageHeight
TheYear
2018 07 25 09400815 0.00 0.00
2018 07 20 09400815 0.00 0.42
2018 07 20 09400815 0.00 0.00
2018 07 20 09400815 0.00 0.41
2018 07 20 09400815 0.27 0.54
2018 07 20 09400815 0.19 0.51
2018 07 20 09400815 0.05 0.45
2018 07 25 09400815 0.00 0.36
2018 07 25 09400815 0.00 0.37
2018 07 01 09400815 0.00 -0.01
```
#### Example for CSV data
Create a regular table.
```python
# Here is how we created a regular table from foreign table.
"""
CREATE TABLE riverflowcsvprecip AS (
SELECT CAST($path.$year AS CHAR(4)) TheYear,
CAST($path.$month AS CHAR(2)) TheMonth,
CAST($path.$day AS CHAR(2)) TheDay,
CAST(payload..site_no AS CHAR(8)) SiteNo,
CAST(payload..Flow AS FLOAT) Flow,
CAST(payload..GageHeight AS FLOAT) GageHeight
FROM riverflowcsv
WHERE payload..Precipitation IS NOT NULL )
WITH DATA
PRIMARY INDEX(SiteNo)
"""
```
Create a DataFrame on the table and display the head of the DataFrame.
```python
# Create a DataFrame on a table.
>>> wrk2dfview = DataFrame("riverflowcsvprecip")
>>> wrk2dfview.head().to_pandas()
TheYear TheMonth TheDay   Flow GageHeight
SiteNo
09380000 2018 07 25 13900.0 9.52
09380000 2018 07 25 12800.0 9.30
09380000 2018 07 25 12500.0 9.24
09380000 2018 07 25 12300.0 9.18
09380000 2018 07 25 11600.0 9.04
09380000 2018 07 25 11500.0 9.00
09380000 2018 07 25 11800.0 9.08
09380000 2018 07 25 13500.0 9.44
09380000 2018 07 25 14300.0 9.61
09380000 2018 07 25 15500.0 9.85
```
## Data in Parquet Format
Foreign table created in Parquet format typically contains the following columns:
* Location
* Other user-specified columns in Parquet format specified while creating foreign table
You can create a teradataml DataFrame on a foreign table using "DataFrame()" or
"DataFrame.from_table()", the same way to create a teradataml DataFrame on a regular table. With
the created DataFrame, you can easily access the data in these columns and process the data using
teradataml DataFrame API or other Python packages.
Unlike foreign tables on JSON and CSV format data, teradataml DataFrame on Parquet data provides
direct access to the actual data in Parquet files, as described in Accessing Foreign Table Created On
Parquet Data.
Path variables from the foreign table can be accessed in one of the following ways:
* Accessing Path Variables using DataFrame.from_query()
* Accessing Path Variables by Creating a View on Foreign Table in Vantage
* Accessing Path Variables by Creating a Table from Foreign Table
### Accessing Foreign Table Created On Parquet Data
The following examples show how to create a teradataml DataFrame on this table and process this
DataFrame with other teradataml API's.
Assume that the following foreign table has been created on Parquet data in Amazon S3 bucket:
```python
CREATE MULTISET FOREIGN TABLE t1l,FALLBACK,
EXTERNAL SECURITY INVOKER TRUSTED AUTH_OBJECT(
Location VARCHAR(2048) CHARACTER SET UNICODE CASESPECIFIC,
a INTEGER,
b INTEGER,
c INTEGER,
d INTEGER,
e VARCHAR(10) CHARACTER SET UNICODE CASESPECIFIC,
f DATE FORMAT 'YYYY-MM-DD',
g  Decimal(8, 3),
h FLOAT,
i BYTEINT,
j SMALLINT)
USING(
LOCATION  ('/s3/ceph2-s3.teradata.com/mk250104/csoj_files/t1l.parquet')
PATHPATTERN  ('$Var1/$Var2/$var3/$Var4')
STOREDAS  ('PARQUET')
MANIFEST  ('FALSE')
NO PRIMARY INDEX
PARTITION BY COLUMN;
```
#### Create a dataframe on the foreign table
```python
# Create dataframe on foreign table which contains data in PARQUET data
>>> t1l = DataFrame.from_table("t1l")
```
#### Check Properties of the DataFrame
* Display the columns in the DataFrame.
```python
# Let's take a peek at the columns in a DataFrame.
>>> t1l.columns
['Location', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
```
* Check the types of the columns.
```python
# Let's check their types
>>> t1l.dtypes
Location                str
a                       int
b                       int
c                       int
d                       int
e                       str
f             datetime.date
g           decimal.Decimal
h                     float
i                       int
j                       int
```
* Keys
```python
# Keys
>>> t1l.keys
```
  <bound method DataFrame.keys of                                             Location   a    b    c  d
  e           f        g     h  i   j
  0  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...   7   70  107  1  CScs      1963-02-03  100.333  19.0
  4  10
  1  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...   3   30  103  1  CScs      1961-06-05  300.333  19.0
  4  10
  2  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...  10  100  110  1  CScs      1963-02-03  100.333  19.0
  4  10
  3  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...   4   40  104  1  CScs      1961-06-05  100.333  19.0
  4  10
  4  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...   8   80  108  1  CScs      1963-02-03  200.333  19.0
  4  10
  5  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...   2   20  102  1  CScs      1961-06-05  200.333  19.0
  4  10
  6  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...   1   10  101  1  CScs      1961-06-05  100.333  19.0
  4  10
  7  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...   6   60  106  1  CScs      1963-02-03  300.333  19.0
  4  10
  8  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...   9   90  109  1  CScs      1963-02-03  300.333  19.0
  4  10
  9  /S3/ceph2-s3.teradata.com/mk250104/csoj_files/...   5   50  105  1  CScs      1961-06-05  200.333  19.0
  4  10>
* Shape
```python
# shape
>>> t1l.shape
(10, 11)
```
#### Explore Data
* Select some columns from the DataFrame.
```python
>>> t1l.select(["a", "b", "e", "f", "i", "j"]).to_pandas()
a b e f i j
0 7 70 CScs 1963-02-03 4 10
1 3 30 CScs 1961-06-05 4 10
2 10 100 CScs 1963-02-03 4 10
3 4 40 CScs 1961-06-05 4 10
4 8 80 CScs 1963-02-03 4 10
5 2 20 CScs 1961-06-05 4 10
6 1 10 CScs 1961-06-05 4 10
7 6 60 CScs 1963-02-03 4 10
8 9 90 CScs 1963-02-03 4 10
9 5 50 CScs 1961-06-05 4 10
```
* Filter data.
```python
>>> t1l[t1l.b == 70].to_pandas()
```
    Location a b c d e f g h i j
  0 /S3/ceph2-s3.teradata.com/mk250104/csoj_files/... 7 70 107 1 CScs
  1963-02-03 100.333 19.0 4 10
* Sample data.
```python
>>> t1l.sample(5).to_pandas()
```
  Location a b c d e f g h i j sampleid
  0 /S3/ceph2-s3.teradata.com/mk250104/csoj_files/... 6 60 106 1 CScs
  1963-02-03 300.333 19.0 4 10 1
  1 /S3/ceph2-s3.teradata.com/mk250104/csoj_files/... 2 20 102 1 CScs
  1961-06-05 200.333 19.0 4 10 1
  2 /S3/ceph2-s3.teradata.com/mk250104/csoj_files/... 4 40 104 1 CScs
  1961-06-05 100.333 19.0 4 10 1
  3 /S3/ceph2-s3.teradata.com/mk250104/csoj_files/... 9 90 109 1 CScs
  1963-02-03 300.333 19.0 4 10 1
  4 /S3/ceph2-s3.teradata.com/mk250104/csoj_files/... 5 50 105 1 CScs
  1961-06-05 200.333 19.0 4 10 1
### Accessing Path Variables using DataFrame.from_query()
User can access actual column and path variables using DataFrame.from_query() that pass a SELECT
query with each required column and path variable selected from foreign table. Each path variable must be
typecast to a valid type and then aliased to the appropriate column name. This allows user to access path
variables along with Parquet data. It is up to the user on what must be selected in SELECT query passed
to "DataFrame.from_query()": columns, attributes, keys from foreign table and path variables.
#### Example
SELECT query with each column and path variable selected.
```python
# Select query with each column from PARQUET file selected. Each column is type
casted to a valid type and
# then aliased to the required column name. Notice, we are selecting each
attribute including path variables.
query = """SELECT CAST($path.$var1 AS CHAR(10)) Var1,
CAST($path.$var2 AS CHAR(4)) Var2,
CAST($path.$var3 AS CHAR(2)) var3,
CAST($path.$var4 AS CHAR(2)) Var4,
a, b, c, d, e, f, g, h, i, j
FROM t1l"""
```
Create DataFrame from the query and display the head of the DataFrame.
```python
>>> wrk1df = DataFrame.from_query(query)
>>> wrk1df.head().to_pandas()
```
    Var1 Var2 var3 Var4 a b c d e f g h i j
0 csoj_files t1l. None None 10 100 110 1 CScs 1963-02-03 100.333 19.0
4 10
1 csoj_files t1l. None None 8 80 108 1 CScs 1963-02-03 200.333 19.0
4 10
2 csoj_files t1l. None None 2 20 102 1 CScs 1961-06-05 200.333 19.0
4 10
3 csoj_files t1l. None None 1 10 101 1 CScs 1961-06-05 100.333 19.0
4 10
4 csoj_files t1l. None None 9 90 109 1 CScs 1963-02-03 300.333 19.0
4 10
5 csoj_files t1l. None None 5 50 105 1 CScs 1961-06-05 200.333 19.0
4 10
6 csoj_files t1l. None None 6 60 106 1 CScs 1963-02-03 300.333 19.0
4 10
7 csoj_files t1l. None None 4 40 104 1 CScs 1961-06-05 100.333 19.0
4 10
8 csoj_files t1l. None None 3 30 103 1 CScs 1961-06-05 300.333 19.0
4 10
9 csoj_files t1l. None None 7 70 107 1 CScs 1963-02-03 100.333 19.0
4 10
### Accessing Path Variables by Creating a View on Foreign Table
### in Vantage
User can access actual columns and path variables by creating a view on a SELECT query with each
required column and path variable selected from a foreign table. Each path variable must be typecast to
a valid type and then aliased to the appropriate column name. This allows user to access path variables
along with Parquet data. It is up to the user on what must be selected in SELECT query passed to
"DataFrame.from_query()": columns, attributes, keys from foreign table and path variables.
#### Example
Create a view.
```python
# While creating a view select each column is type casted to a valid type and
# then aliased to the required column name. Notice, we are selecting each
attribute including path variables.
# Following is the VIEW created at the backend:
"""
REPLACE VIEW t1lview AS (
SELECT CAST($path.$var1 AS CHAR(10)) Var1,
CAST($path.$var2 AS CHAR(4)) Var2,
CAST($path.$var3 AS CHAR(2)) var3,
CAST($path.$var4 AS CHAR(2)) Var4,
a, b, c, d, e, f, g, h, i, j
FROM t1l);
"""
```
Create a DataFrame on the view and display the head of the DataFrame.
```python
# Create a DataFrame on a view.
>>> wrk2dfview = DataFrame("t1lview")
>>> wrk2dfview.head().to_pandas()
```
Var1 Var2 var3 Var4 a b c d e f g h i j
0 csoj_files t1l. None None 10 100 110 1 CScs 1963-02-03 100.333 19.0
4 10
1 csoj_files t1l. None None 8 80 108 1 CScs 1963-02-03 200.333 19.0
4 10
2 csoj_files t1l. None None 2 20 102 1 CScs 1961-06-05 200.333 19.0
4 10
3 csoj_files t1l. None None 1 10 101 1 CScs 1961-06-05 100.333 19.0
4 10
4 csoj_files t1l. None None 9 90 109 1 CScs 1963-02-03 300.333 19.0
4 10
5 csoj_files t1l. None None 5 50 105 1 CScs 1961-06-05 200.333 19.0
4 10
6 csoj_files t1l. None None 6 60 106 1 CScs 1963-02-03 300.333 19.0
4 10
7 csoj_files t1l. None None 4 40 104 1 CScs 1961-06-05 100.333 19.0
4 10
8 csoj_files t1l. None None 3 30 103 1 CScs 1961-06-05 300.333 19.0
4 10
9 csoj_files t1l. None None 7 70 107 1 CScs 1963-02-03 100.333 19.0
4 10
### Accessing Path Variables by Creating a Table from Foreign Table
User can access actual columns and path variables by creating a regular table on a SELECT query (Create
Table as SELECT, CTAS) with each required column and path variables selected from the foreign table.
Each path variable must be typecast to a valid type and then aliased to the appropriate column name. This
allows user to access path variables along with Parquet data. It is up to the user on what must be selected
in SELECT query passed to "DataFrame.from_query()": columns, attributes, keys from foreign table and
path variables.
#### Example
Create a regular table.
```python
# Let's see an example. Here is how we created a regular table from
foreign table.
"""
CREATE TABLE t1lctas AS (
SELECT CAST($path.$var1 AS CHAR(10)) Var1,
CAST($path.$var2 AS CHAR(4)) Var2,
CAST($path.$var3 AS CHAR(2)) var3,
CAST($path.$var4 AS CHAR(2)) Var4,
a, b, c, d, e, f, g, h, i, j
FROM t1l)
WITH DATA
PRIMARY INDEX(a)
"""
```
Create a DataFrame on the table and display the head of the DataFrame.
```python
# Create a DataFrame on a table.
>>> wrk2dfview = DataFrame("t1lctas")
>>> wrk2dfview.head().to_pandas()
```
Var1 Var2 var3 Var4 b c d e f g h i j
a
3 csoj_files t1l. None None 30 103 1 CScs 1961-06-05 300.333 19.0 4
5 csoj_files t1l. None None 50 105 1 CScs 1961-06-05 200.333 19.0 4
6 csoj_files t1l. None None 60 106 1 CScs 1963-02-03 300.333 19.0 4
7 csoj_files t1l. None None 70 107 1 CScs 1963-02-03 100.333 19.0 4
9 csoj_files t1l. None None 90 109 1 CScs 1963-02-03 300.333 19.0 4
10 csoj_files t1l. None None 100 110 1 CScs 1963-02-03 100.333 19.0 4
8 csoj_files t1l. None None 80 108 1 CScs 1963-02-03 200.333 19.0 4
4 csoj_files t1l. None None 40 104 1 CScs 1961-06-05 100.333 19.0 4
2 csoj_files t1l. None None 20 102 1 CScs 1961-06-05 200.333 19.0 4
1 csoj_files t1l. None None 10 101 1 CScs 1961-06-05 100.333 19.0 4