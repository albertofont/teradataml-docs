# DataFrames Setup and Basics (Sources, Non-Default DB, UAF)

This section provides detailed usage examples of the functions for data management, exploration,
preparation in teradataml.
teradataml DataFrame examples documented in this guide require certain datasets to be loaded. Use these
commands to load them:
```python
>>> from teradataml import load_example_data
>>> load_example_data("dataframe", ["scale_housing_test", "employee_info",
"sales", "admissions_train", "join_table1", "join_table2", "iris_test"])
```
See load_example_data() Function for more details about 'load_example_data()' utility.
In this section, assume you have a context created with these commands:
```python
>>> from teradataml import create_context, DataFrame
>>> create_context(host = "myhostname", username="myusername", password
= "mypassword")
```
* DataFrames from Teradata Vantage Data Sources
* Access Tables in a Non-Default Database in Vantage
* Input Classes for UAF Functions
* DataFrame Manipulation
* DataFrame Metadata
* Data Rotation
* Saving teradataml DataFrames to Vantage
* Exporting DataFrame to External Entities
## DataFrames from Teradata Vantage Data Sources
You can construct a teradataml DataFrame from either an existing Vantage table or view or a SQL query
result. The data source determines the DataFrame constructor function you use.
| Vantage Data Source | DataFrame Constructor Function |
| ------------------- | ------------------------------ |
| Table or view | DataFrame Constructor |
|  | DataFrame.from_table() Function |
| SQL query result | DataFrame.from_query() Function |
| pandas DataFrame | DataFrame.from_pandas Function |
| Vantage Data Source | DataFrame Constructor Function |
| ------------------- | ------------------------------ |
| Dictionary | DataFrame.from_dict Function |
| List of lists/tuples/dictionaries/numpy arrays | DataFrame.from_records Function |

### DataFrame Constructor
Use the DataFrame() function to create a teradataml DataFrame from the input data.
#### Optional Parameters
data
    Specifies the input data to create a teradataml DataFrame.
index
    If "data" is a string, then the argument specifies whether to use the index column for sorting
    or not.
    If "data" is a pandas DataFrame, then this argument specifies whether to save Pandas
    DataFrame index as a column or not.
index_label
    If "data" is a string, then the argument specifies column(s) used for sorting.
    If "data" is a pandas DataFrame, then the default behavior is applied.
    Note:
      * If the index label is not specified for a table name, the primary index of the base
      table is used as the index label.
      * If the index label is not specified for a view name, the index label is set to None.
      * Refer to the "index_label" parameter of copy_to_sql() for details on the
      default behavior.
query
    Specifies the SQL query for this DataFrame.
    Used by class method from_query.
materialize
    Specifies whether to materialize DataFrame or not when created.
    Used by class method from_query.
Use materialization when the query passed to from_query(), is expected to produce non-
    deterministic results, when it is executed multiple times. Using this option will help user to
    have deterministic results in the resulting teradataml DataFrame.
    Default value: False (No materialization)
kwargs
    table_name
        Specifies the table name or view name in Vantage referenced by
        this DataFrame.
        Note:
          If "data" and "table_name" are both specified, then the "table_name"
          argument is ignored.
    primary_index
        Specifies which columns to use as primary index for the
        teradataml DataFrame.
        Note:
          If "data" and "table_name" are both specified, then the "table_name"
          argument is ignored.
    types
        Specifies required data types for requested columns to be saved in Vantage.
        Note:
          * This argument is not applicable when "data" argument is of type str
            or in_schema.
          * Refer to the "types" parameter of copy_to_sql() for more details.
    columns
        Specifies the names of the columns to be used in the DataFrame.
Note:
          * This argument is not applicable when "data" argument is of type str
            or in_schema.
          * If "data" is a dictionary and this argument is specified, only the
            specified columns will be included in the DataFrame if the dictionary
            contains those keys. If the dictionary does not contain the specified
            keys, those columns will be added with NaN values.
#### Examples setup
Note:
teradataml DataFrame examples documented in this guide required certain datasets to be loaded.
Use the below commands to load them. More details about 'load_example_data()' utility can be found
at: load_example_data()
Load the example datasets for the examples.
```python
>>> from teradataml import load_example_data
>>> load_example_data("dataframe", ["scale_housing_test", "employee_info",
"sales", "admissions_train", "join_table1", "join_table2", "iris_test"])
```
Create a context using your database host and user credentials.
```python
>>> from teradataml.context.context import *
>>> from teradataml.dataframe.dataframe import DataFrame
>>> create_context(host = "myhostname", username="myusername", password
= "mypassword")
```
#### Example 1: Create a DataFrame from an existing table "sales" in Vantage
The index label is not specified, the primary index "accounts" is used.
```python
>>> df = DataFrame("sales")
>>> df
Feb   Jan   Mar   Apr  datetime
accounts
Alpha Co    210.0   200   215   250  04/01/2017
Red Inc     200.0   150   140  None  04/01/2017
Orange Inc  210.0  None  None   250  04/01/2017
Jones LLC   200.0   150   140   180  04/01/2017
Yellow Inc   90.0  None  None  None  04/01/2017
Blue Inc     90.0    50    95   101  04/01/2017
>>>
```
#### Example 2: Create a DataFrame from an existing table "sales" in Vantage with
#### an index label "Feb"
```python
>>> df = DataFrame("sales", index_label="Feb")
>>> df
accounts   Jan   Mar   Apr  datetime
Feb
210.0    Alpha Co   200   215   250  04/01/2017
200.0     Red Inc   150   140  None  04/01/2017
210.0  Orange Inc  None  None   250  04/01/2017
200.0   Jones LLC   150   140   180  04/01/2017
90.0   Yellow Inc  None  None  None  04/01/2017
90.0     Blue Inc    50    95   101  04/01/2017
```
#### Example 3: Create a DataFrame from an existing table "sales" in Vantage with
#### an index label "Jan" and "Feb"
```python
>>> df = DataFrame("sales", index_label=["Jan", "Feb"])
>>> df
accounts   Mar   Apr  datetime
Jan Feb
200 210.0    Alpha Co   215   250  04/01/2017
150 200.0     Red Inc   140  None  04/01/2017
NaN 210.0  Orange Inc  None   250  04/01/2017
150 200.0   Jones LLC   140   180  04/01/2017
NaN 90.0   Yellow Inc  None  None  04/01/2017
50  90.0     Blue Inc    95   101  04/01/2017
```
#### Example 4: Creates a DataFrame from an existing view "salesv" in Vantage
#### with an index_label "Mar"
You must create a view on sales table at the backend with name 'salesv' to use this example.
```python
>>> get_context().execute("CREATE VIEW salesv AS SELECT * FROM sales")
<sqlalchemy.engine.result.ResultProxy object at 0x11bbc3668>
>>> df = DataFrame("salesv", index_label="Mar")
>>> df
accounts    Feb   Jan   Apr  datetime
Mar
95     Blue Inc   90.0    50   101  04/01/2017
NaN  Orange Inc  210.0  None   250  04/01/2017
140     Red Inc  200.0   150  None  04/01/2017
NaN  Yellow Inc   90.0  None  None  04/01/2017
140   Jones LLC  200.0   150   180  04/01/2017
215    Alpha Co  210.0   200   250  04/01/2017
```
#### Example 5: Create a teradataml DataFrame from pandas DataFrame
```python
>>> import pandas as pd
>>> pdf = pd.DataFrame({"col1": [1, 2, 3], "col2": [4, 5, 6]})
>>> df = DataFrame(pdf)
>>> df
col1 col2 index_label
0      3    6           2
1      2    5           1
2      1    4           0
```
#### Example 6: Create a teradataml DataFrame from a pandas DataFrame without
#### index column
```python
>>> import pandas as pd
>>> pdf = pd.DataFrame({"col1": [1, 2, 3], "col2": [4, 5, 6]})
>>> df = DataFrame(data=pdf, index=False)
>>> df
col1 col2
0      3    6
1      2    5
2      1    4
```
#### Example 7: Create a teradataml DataFrame from a pandas DataFrame with
#### index label and primary index as 'id'
```python
>>> import pandas as pd
>>> df = DataFrame(pdf, index=True, index_label='id', primary_index='id')
>>> df
col1 col2
id
2      3    6
1      2    5
0      1    4
```
#### Example 8: Create a teradataml DataFrame from a pandas DataFrame with
#### index label and primary index as 'id'
```python
>>> pdf = pd.DataFrame({"col1": [1, 2, 3], "col2": [4, 5, 6]})
>>> df = DataFrame(pdf, index=True, index_label='id', primary_index='id')
>>> df
col1 col2
id
2      3    6
1      2    5
0      1    4
```
#### Example 9: Create a teradataml DataFrame from list of lists
```python
>>> df = DataFrame([[1, 2], [3, 4]])
>>> df
col_0 col_1 index_label
0       3     4           1
1       1     2           0
```
#### Example 10: Create a teradataml DataFrame from numpy array
```python
>>> import numpy as np
>>> df = DataFrame(np.array([[1, 2], [3, 4]]), index=True, index_label="id")
>>> df
col_0 col_1
id
1       3     4
0       1     2
```
#### Example 11: Create a teradataml DataFrame from a dictionary
```python
>>> df = DataFrame({"col1": [1, 2], "col2": [3, 4]},
index=True, index_label="id")
>>> df
col_0 col_1
id
1       2     4
0       1     3
```
#### Example 12: Create a teradataml DataFrame from list of dictionaries
```python
>>> df = DataFrame({"col1": [1, 2], "col2": [3, 4]},
index=True, index_label="id")
>>> df
col_0 col_1
id
1       2     4
0       1     3
```
#### Example 13: Create a teradataml DataFrame from list of tuples
```python
>>> df = DataFrame([("Alice", 1), ("Bob", 2)])
>>> df
col_0 col_1 index_label
0   Alice     1           1
1     Bob     2           0
```
### DataFrame.from_table() Function
You can also use the DataFrame.from_table() function to create a teradataml DataFrame from an existing
table or view in Vantage.
#### Example 1: Create a teradataml DataFrame
```python
>>> df = DataFrame.from_table("sales")
>>> df
Feb   Jan   Mar   Apr  datetime
accounts
Alpha Co    210.0   200   215   2
04/01/2017
Blue Inc     90.0
95   101  04/01/2017
Yellow Inc   90.0  None  None  None  04/01/2017
Jones LLC   200.0   1
140   180  04/01/2017
Red Inc     200.0   1
140  None  04/01/2017
Orange Inc  210.0  None  None   2
04/01/2017
```
#### Example 2: Create a teradataml DataFrame with 'index_label'
```python
>>> df = DataFrame.from_table("sales", index_label="Feb")
>>> df
accounts   Jan   Mar   Apr  datetime
Feb
90.0     Blue Inc
95   101  04/01/2017
210.0  Orange Inc  None  None   2
04/01/2017
200.0     Red Inc   150   140  None  04/01/2017
90.0   Yellow Inc  None  None  None  04/01/2017
200.0   Jones LLC   150   140   180  04/01/2017
210.0    Alpha Co   200   215   250  04/01/2017
```
### DataFrame.from_query() Function
The DataFrame.from_query() function constructs a teradataml DataFrame from the result of a SQL query.
Arguments:
The function takes a SQL query as an argument and creates a DataFrame based on the query.
The function also takes an index label as an optional argument. The index label is used for sorting. The
value of the index label can be a column name or a list of column names. If the index label is not specified
when creating a DataFrame for a query, the index label is set to None.
#### Example 1: DataFrame.from_query with no Index Label specified
This example creates the DataFrame "df" from the result of a SQL query of the table or view "sales".
```python
>>> df = DataFrame.from_query("select Jan, Feb, datetime from sales")
>>> df
Jan    Feb    datetime
0    NaN   90.0  2017-04-01
1  150.0  200.0  2017-04-01
2    NaN  210.0  2017-04-01
3  200.0  210.0  2017-04-01
4   50.0   90.0  2017-04-01
5  150.0  200.0  2017-04-01
```
#### Example 2: DataFrame.from_query with One-Column Index Label
This example creates the DataFrame "df" from the result of a SQL query of the table or view "sales". The
index_label is composed of one column of "sales", "Jan".
```python
>>> df = DataFrame.from_query("select Jan, Feb, datetime from
sales", index_label="Jan")
>>> df
Feb    datetime
Jan
150.0  200.0  2017-04-01
NaN      90.0  2017-04-01
NaN     210.0  2017-04-01
50.0    90.0  2017-04-01
200.0  210.0  2017-04-01
150.0  200.0  2017-04-01
```
## Access Tables in a Non-Default Database in Vantage
When you create a connection to Vantage, you have the option to either specify a default database, or rely
on the default database of the connecting user. However, it is also possible to interact with the tables in a
database other than the default using the following approaches.
#### Using the in_schema() function
The in_schema() function takes a schema name and a table name and creates a database object name in
the format "schema"."table_name".
Note:
Teradata recommends using this function to access tables and views from a database other than the
default database.
The following example creates a DataFrame from the existing Vantage view "dbcinfo" in the non-default
database "dbc" using the in_schema() function.
```python
>>> from teradataml.dataframe.dataframe import in_schema
>>> df = DataFrame(in_schema("dbc", "dbcinfo"))
>>> df
InfoKey     InfoData
0                RELEASE  16.20.27.01
1  LANGUAGE SUPPORT MODE     Standard
2                VERSION  16.20.27.01
```
#### Using fully qualified table name
Teradata recommends using in_schema() to create a dataframe, as shown in the previous section.
Teradata does not recommend using fully qualified table name while creating a dataframe.
Note:
If you want to continue using fully qualified table name, enclose the schema name and table name in
quotation marks ("schema_name"."table_name").
You can use fully qualified table name that can be passed to the from_table() function or fully qualified
table in SQL that can be passed to the from_query() function.
* Example: Using from_table() and fully qualified table name
```python
>>> from teradataml.dataframe.dataframe import DataFrame
>>> df = DataFrame.from_table('"dbc"."dbcinfo"')
>>> df
InfoKey     InfoData
0  LANGUAGE SUPPORT MODE     Standard
1                RELEASE  16.20.27.01
2                VERSION  16.20.27.01
>>>
```
* Example: Using from_query() and fully qualified table name in SQL
```python
>>> from teradataml.dataframe.dataframe import in_schema
>>> df = DataFrame.from_query("SELECT * FROM dbc.dbcinfo")
>>> df
InfoKey     InfoData
0                RELEASE  16.20.27.01
1  LANGUAGE SUPPORT MODE     Standard
2                VERSION  16.20.27.01
>>>
```
## Input Classes for UAF Functions
* TDSeries
* TDAnalyticResult
* TDMatrix
* TDGenSeries
### TDSeries
Create a TDSeries object from a teradataml DataFrame representing a SERIES in time series which is
used as input to Unbounded Array Framework (UAF), time series functions.
A series is a one-dimensional array. They are the basic input of UAF functions. A series is identified by its
series ID, that is, the id argument, and indexed by its row_index argument.
Series is passed to and returned from UAF functions as wavelets. Wavelets are collections of rows,
grouped by one or more fields, and ordered on the row_axis argument.
Any operations like filter, select, sum, and so on, over TDSeries returns a teradataml DataFrame.
Required Arguments:
* data: Specifies the teradataml DataFrame.
* id: Specifies the name of the column in data containing the identifier values.
* row_index: Specifies the name of the column in data containing the row indexing values.
Optional Arguments:
* row_index_style: Specifies the style of row indexing.
  Permitted values are "TIMECODE" (default value), "SEQUENCE".
* in_sequence: Specifies a sequence of series to plot.
* payload_field: Specifies the names of the fields for payload.
* payload_content: Specifies the payload content type.
  Permitted values:
  * "REAL"
  * "COMPLEX"
  * "AMPL_PHASE"
  * "AMPL_PHASE_RADIANS"
  * "AMPL_PHASE_DEGREES"
  * "MULTIVAR_REAL"
  * "MULTIVAR_COMPLEX"
  * "MULTIVAR_ANYTYPE"
  * "MULTIVAR_AMPL_PHASE"
  * "MULTIVAR_AMPL_PHASE_RADIANS "
  * "MULTIVAR_AMPL_PHASE_DEGREES"
* layer: Specifies the layer name of the ART table, if dataframe is created on ART table.
* interval: Specifies the indicator to divide a series into a collection of intervals along its row-axis.
  interval is categorized in to 4 types:
  * Values represent time-duration.
    Allowed values include:
    ▪ CAL_YEARS
    ▪ CAL_MONTHS
    ▪ CAL_DAYS
    ▪ WEEKS
    ▪ DAYS
    ▪ HOURS
    ▪ MINUTES
    ▪ SECONDS
    ▪ MILLISECONDS
    ▪ MICROSECONDS
  * Values represent time-zero.
    Allowed values include:
▪ DATE
    ▪ TIMESTAMP
    ▪ TIMESTAMP WITH TIME ZONE
  * Values represent an integer or floating number.
    Allowed values include a positive integer or float ranging from 1 to 32767, inclusively.
  * sequence-zero.
    This is an expression which evaluates to an INTEGER or FLOAT. Used when row_index_style
    is SEQUENCE.
  Permitted values for argument interval include individual values or combined values from
  the following:
  * time-duration
  * time-duration, time-zero
  * integer
  * float, integer
  * sequence-zero
  * float, sequence-zero
#### Example 1: Create TDSeries
```python
>>> from teradataml import create_context, load_example_data,
DataFrame, TDSeries
>>> con = create_context(host = host, user=user, password=passw)
>>> load_example_data("dataframe", "admissions_train")
# Create a DataFrame to be passed as input to TDSeries.
>>> data = DataFrame("admissions_train")
# Create TDSeries object which can be used as Series_Spec input in
UAF functions.
>>> result = TDSeries(data=data,
id="admitted",
row_index="admitted",
payload_field="abc",
payload_content="REAL")
>>> result
masters   gpa     stats programming  admitted
id
5       no  3.44    Novice      Novice         0
34     yes  3.85  Advanced    Beginner         0
13      no  4.00  Advanced      Novice         1
40     yes  3.95    Novice    Beginner         0
22     yes  3.46    Novice    Beginner         0
19     yes  1.98  Advanced    Advanced         0
36      no  3.00  Advanced      Novice         0
15     yes  4.00  Advanced    Advanced         1
7      yes  2.33    Novice      Novice         1
17      no  3.83  Advanced    Advanced         1
```
### TDAnalyticResult
Create a TDAnalyticResult object from a teradataml Dataframe created on an Analytic Result Table (ART)
which can be used as input to Unbounded Array Framework functions.
The primary use of an analytical result table (ART) is to associate function results with a name label,
enabling users to easily retrieve the result data and pass the result to another UAF function.
An ART can have multiple layers. Each layer has its own dedicated row composition for the series or matrix.
Any operations like filter, select, sum, and so on, over TDAnalyticResult returns a teradataml DataFrame.
Required Arguments:
* data: Specifies the teradataml DataFrame.
Optional Arguments
* id_sequence: Specifies a sequence of series to plot.
* payload_field: Specifies the names of the fields for payload.
* payload_content: Specifies the payload content type.
  Permitted values:
  * "REAL"
  * "COMPLEX"
  * "AMPL_PHASE"
  * "AMPL_PHASE_RADIANS"
  * "AMPL_PHASE_DEGREES"
  * "MULTIVAR_REAL"
  * "MULTIVAR_COMPLEX"
  * "MULTIVAR_ANYTYPE"
  * "MULTIVAR_AMPL_PHASE"
  * "MULTIVAR_AMPL_PHASE_RADIANS "
  * "MULTIVAR_AMPL_PHASE_DEGREES"
* layer: Specifies the layer name of the ART table, if dataframe is created on ART table.
#### Example 1: Prepare input for UAF function using an Analytic Result
#### Table (ART)
```python
>>> from teradataml import create_context
>>> con = create_context(host=host, user= user, password=password)
# Create a Analytic Result Table(ART) by executing SInfo function.
>>> from teradataml import load_example_data, SInfo
>>> load_example_data("uaf", ["ocean_buoys2"])
# Create teradataml DataFrame object.
>>> data = DataFrame.from_table("ocean_buoys2")
# Create teradataml TDSeries object.
>>> data_series_df = TDSeries(data=data,
id=["ocean_name","buoyid"],
row_index="TD_TIMECODE",
row_index_style="TIMECODE",
payload_field="jsoncol.Measure.salinity",
payload_content="REAL")
# Execute SInfo function and store the output in 'TSINFO_RESULTS'.
>>> uaf_out = SInfo(data=data_series_df, output_table_name='TSINFO_RESULTS')
# Create a teradataml dataframe on 'TSINFO_RESULTS' ART.
>>> art_df = DataFrame('TSINFO_RESULTS')
# Check if the DataFrame 'art_table' is created on an ART.
>>> art_df.is_art
True
# Create TDAnalyticResult object which can be used as input in UAF functions.
>>> result = TDAnalyticResult(data=art_df)
# Check if 'result' is created on an ART.
>>> result.is_art
True
>>> result
buoyid  ROW_I      INDEX_DT
INDEX_BEGIN                   INDEX_END  NUM_ENTRIES  DISCRETE
SAMPLE_INTERVAL CONTENT  MIN_MAG_salinity  MAX_MAG_salinity  AVG_MAG_salinity
RMS_MAG_salinity HAS_NULL_NAN_INF
0      44      1  TIMESTAMP(6)  2014-01-06 10:00:24.000000
2014-01-06 10:52:00.000009           13         0  MICROSECONDS(2
000001)
REAL              55.0              55.0              55.0
55.0                N
1       0      1  TIMESTAMP(6)  2014-01-06 08:00:00.000000
2014-01-06 08:10:00.000000            5         0             SECONDS(150)
REAL              55.0              55.0              55.0
55.0                N
2       2      1  TIMESTAMP(6)  2014-01-06 21:01:25.122200
2014-01-06 21:03:25.122200            3         1               MINUTES(1)
REAL              55.0              55.0              55.0
55.0                N
3       1      1  TIMESTAMP(6)  2014-01-06 09:01:25.122200
2014-01-06 09:03:25.122200            6         0              SECONDS(24)
REAL              55.0              55.0              55.0
55.0                N
```
### TDMatrix
Create a TDMatrix object from a teradataml DataFrame representing a MATRIX in time series which is
used as input to Unbounded Array Framework time series functions.
A matrix is a two-dimensional array that has rows and columns. A matrix is identified by its matrix id, that
is, the id argument, and is indexed by its row_index and column_index arguments.
A matrix can be a one of the following types:
* Row-major matrix: Each row is a wavelet that is grouped by its matrix id and row_index, and ordered
  by its column_index.
* Column-major matrix: Each column is a wavelet that is grouped by its matrix id and column_index, and
  ordered by its row_index.
Required Arguments:
* data: Specifies the teradataml DataFrame.
* id: Specifies the name of the column in data containing the identifier values.
* row_index: Specifies the name of the column in data containing the row indexing values.
* column_index: Specifies the name of the column in data containing the column indexing values.
Optional Arguments:
* row_index_style: Specifies the style of row indexing.
  Permitted values are "TIMECODE" (default value), "SEQUENCE".
* column_index_style: Specifies the style of column indexing.
  Permitted values are "TIMECODE" (default value), "SEQUENCE".
* id_sequence: Specifies a sequence of series to plot.
* payload_field: Specifies the names of the fields for payload.
* payload_content: Specifies the payload content type.
  Permitted values:
  * "REAL"
  * "COMPLEX"
  * "AMPL_PHASE"
  * "AMPL_PHASE_RADIANS"
  * "AMPL_PHASE_DEGREES"
  * "MULTIVAR_REAL"
  * "MULTIVAR_COMPLEX"
  * "MULTIVAR_ANYTYPE"
  * "MULTIVAR_AMPL_PHASE"
  * "MULTIVAR_AMPL_PHASE_RADIANS "
  * "MULTIVAR_AMPL_PHASE_DEGREES"
* layer: Specifies the layer name of the ART table, if dataframe is created on ART table.
#### Example 1: Create a TDMatrix object
```python
>>> from teradataml import create_context, load_example_data,
DataFrame, TDMatrix
>>> con = create_context(host = host, user=user, password=passw)
>>> load_example_data("dataframe", "admissions_train")
# Create a DataFrame to be passed as input to TDMatrix.
>>> data = DataFrame("admissions_train")
# Create a TDMatrix object to be passed as input to UAF functions.
>>> res = TDMatrix(data=data,
id='admitted',
row_index='id',
column_index = 'admitted',
row_index_style="TIMECODE",
payload_field='payload_field',
payload_content='REAL')
>>> res
masters   gpa     stats programming  admitted
id
15     yes  4.00  Advanced    Advanced         1
34     yes  3.85  Advanced    Beginner         0
13      no  4.00  Advanced      Novice         1
38     yes  2.65  Advanced    Beginner         1
5      no  3.44    Novice      Novice         0
40     yes  3.95    Novice    Beginner         0
7      yes  2.33    Novice      Novice         1
22     yes  3.46    Novice    Beginner         0
26     yes  3.57  Advanced    Advanced         1
17     no  3.83  Advanced    Advanced         1
```
### TDGenSeries
Generate a series to a UAF function rather than using a pre-existing series instance. It contains all the
information that would have been derivable from a TDSeries as well as the information required to generate
the series.
Note:
The TDGenSeries can only be passed to a function that accepts a single series as input.
Generated series have a indexing mechanism which starts at integer 0 and increments by 1 for each
additional generated entry.
Required Arguments:
* instances: Specifies the columns and values for the generated series.
* data_types: Specifies the data types of the identifying columns for the generated series.
* start: Specifies the start value for the information about how the series payload, containing successive
  real magnitude values.
* offset: Specifies the offset value for the information about how the series payload, containing
  successive real magnitude values.
* num_entries: Specifies the number of entries for the information about how the series payload,
  containing successive real magnitude values.
#### Example 1: Create a TDGenSeries object
```python
# Import TDGenSeries.
>>> from teradataml import TDGenSeries
# Import INTEGER type from teradatasqlalchemy.
>>> from teradatasqlalchemy.types import INTEGER
# Create a TDGenSeries object to be passed as input to UAF functions.
>>> series = TDGenSeries(instances = {"BuoyID": 3}, data_types = INTEGER(),
start=0, offset=1, num_entries=5)
```