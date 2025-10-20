# Appendix A: Teradata Package for Python Limitations and Considerations

* Teradata Package for Python Limitations
* Teradata Package for Python Considerations
* Teradata Package for Python Issues with VIEW Creation
## Teradata Package for Python Limitations
This section provides information on limitations of the Teradata Package for Python.
* Open Analytics Login Issue with teradataml 17.20.00.05 and 17.20.00.06
* DataFrame Creation on Primary Time Index Tables is Partially Supported
* copy_to_sql and to_sql Functions Limitation
* Multithreading and Concurrent Usage Limitations
* print(dataframe.dtypes) Displays Column Types as String
* Error Calling teradataml Analytic Functions
* select() Method Requires Unique Column Names
* Limited Missing Value Support
* DataFrames Become Invalid When a Connection Context is Recreated
* Error Using responses Argument in MLE Function NaïveBayesTextClassifierPredict
* copy_to API When Loading Data into PTI Table
* Exploratory Data Analysis UI (EDA UI)
### Open Analytics Login Issue with teradataml 17.20.00.05
### and 17.20.00.06
teradataml 17.20.00.05 and 17.20.00.06 users cannot authenticate with VantageCloud Lake Open
Analytics Framework APIs.
The root cause of this issue is that VantageCloud Lake requires creation of Open Analytics
Framework OAuth client device before any client user can connect to Open Analytics Framework User
Environment Service.
### Interim Solution
1.
* Server side: Open a Teradata incident on this issue, or request your Teradata account team to
* submit a Change Request (CR) to create the VantageCloud Lake Open Analytics Framework OAuth
* client device.
2.
* Client side: Provide the  client_id  of the OAuth client device to the  set_auth_token()  teradataml API.
* For example:
A

```python
set_auth_token(ues_url=getpass.getpass("ues_url : "), client_id)
```
* client_id  value has the format  <org_name>-oaf-device , where  <org_name>  is the organization
* name, which can be taken from the URL to access the VantageCloud Lake Console. For example:
```python
https://<organization_name>.innovationlabs.teradata.com/
```
* Note:
* With teradataml 17.20.00.07,  client_id  is optional. teradataml will auto-populate the  client_id
* value from  ues_url .
### DataFrame Creation on Primary Time Index Tables is
### Partially Supported
DataFrame creation on Primary Time Index (PTI) tables is partially supported. DataFrame is created when
the underlying PTI table is not created with 'timebucket' duration.
### Example
Create a PTI table without 'timebucket_duration', using copy_to_sql and then create a DataFrame on it.
```python
load_example_data("sessionize", "sessionize_table")
df3 = DataFrame('sessionize_table')
copy_to_sql(df3, "test_copyto_pti",
timecode_column='clicktime',
columns_list='event')
DataFrame("test_copyto_pti")
TD_TIMECODE partition_id adid productid
event
click  2009-03-19 16:43:26.000000         1199    1      1001
click  2009-07-04 09:18:17.000000         1231    1      1001
click  2009-07-04 09:18:17.000000         1231    1      1001
click  2009-07-16 11:18:16.000000         1039    4      1001
click  2009-07-16 11:18:16.000000         1039    4      1001
click  2009-07-24 04:18:10.000000         1167    2      1001
view   2009-02-09 15:17:59.000000         1263    4      1001
view   2009-03-09 21:17:59.000000         1199    2      1001
view   2009-03-09 21:17:59.000000         1199    2      1001
view   2009-03-13 17:17:59.000000         1071    4      1001
```
### Example
Create a PTI table with 'timebucket_duration', DataFrame creation on the PTI table fails.
```python
load_example_data("sessionize", "sessionize_table")
df3 = DataFrame('sessionize_table')
copy_to_sql(df3, "test_copyto_pti1",
timecode_column='clicktime',
```
A: Teradata Package for Python Limitations and Considerations

```python
columns_list='event',
timebucket_duration="HOURS(2)")
DataFrame("test_copyto_pti1")
/anaconda3/lib/python3.6/site-packages/sqlalchemy/engine/reflection.py:888:
SAWarning: index key 'TD_TIMEBUCKET' was not located in columns for
table 'test_copyto_pti1'
"columns for table '%s'" % (flavor, c, table_name)
Traceback (most recent call last):
File "/Users/pp186043/Github_Repos/teradataml_repo/pyTeradata/teradataml/dataframe/
dataframe.py", line 163, in __init__
self._metaexpr = self._get_metaexpr()
File "/Users/pp186043/Github_Repos/teradataml_repo/pyTeradata/teradataml/dataframe/
dataframe.py", line 395, in _get_metaexpr
return _MetaExpression(t, column_order = self.columns)
File "/Users/pp186043/Github_Repos/teradataml_repo/pyTeradata/teradataml/dataframe/
sql.py", line 164, in __init__
self.__t = _SQLTableExpression(table, **kw)
File "/Users/pp186043/Github_Repos/teradataml_repo/pyTeradata/teradataml/dataframe/
sql.py", line 460, in __init__
raise ValueError('Reflected column names do not match those in DataFrame.columns')
ValueError: Reflected column names do not match those in DataFrame.columns
The above exception was the direct cause of the following exception:
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "/Users/pp186043/Github_Repos/teradataml_repo/pyTeradata/teradataml/dataframe/
dataframe.py", line 171, in __init__
raise TeradataMlException(Messages.get_message(MessageCodes.TDMLDF_CREATE_FAIL),
MessageCodes.TDMLDF_CREATE_FAIL) from err
teradataml.common.exceptions.TeradataMlException: [Teradata][teradataml](TDML_2010)
Failed to create Teradata DataFrame.
```
### copy_to_sql and to_sql Functions Limitation
### UTF-8 and Unicode support
UTF-8 and Unicode characters are not supported by the  copy_to_sql  function.
To work around this limitation, convert UTF-8 and Unicode characters to ASCII and then push the data into
the teradataml DataFrame.
### Multithreading and Concurrent Usage Limitations
The Teradata Package for Python does not support multithreading and concurrent usage of the
**following operations:**
* Analytic functions for model creations
* For example, calling an analytic function, such as glm, kmeans and so forth, from multiple threads by
* passing different functional parameters to the same API in each thread.
* Analytic functions for prediction
* For example, calling an analytic predict function, such as glmpredict, from multiple threads by passing
* different functional parameters to the same API in each thread.
* Transformations of data frames
A: Teradata Package for Python Limitations and Considerations

* For example, performing transformation on the same teradataml DataFrame from multiple threads.
### print(dataframe.dtypes) Displays Column Types as String
When a teradataml DataFrame is created from a table with the following column data types, the
**print( dataframe .dtypes) displays the column types as String:**
* Interval (Year, Month, Day, Hour)
* Period (Date, Time)
* CLOB
### Error Calling teradataml Analytic Functions
### When Connected to Teradata Vantage with Database Engine 20 Only
If you connect to Vantage without a ML Engine installed and request to run teradataml analytic functions
that execute on ML Engine, the system will return an error.
**For example:**
```python
TeradataMlException: [Teradata][teradataml](TDML_2102) Failed to execute SQL:
teradatasql.OperationalError: [Version 16.20.0.32] [Session 76702] [Teradata
Database] [Error 3707]
Syntax error, expected something like ')' between the word 'ConfusionMatrix'
and '('.")'
```
The Teradata Package for Python is fully featured when connected to Vantage with Database Engine 20
and ML Engine.
When connecting to Vantage with Database Engine 20 only, the only analytic functions available to the
teradataml users are the ones that execute on the Database Engine 20.
To invoke and use teradataml analytic functions available on the ML Engine, your Vantage system must
consist of both Database Engine 20 and ML Engine.
### Using Function with New Arguments on Vantage1.0
Following functions are updated by adding new arguments.
Analytic Functions
Newly added arguments
(Only supported on Vantage 1.1 or later)
AdaBoost
categorical_encoding
DecisionForest
categorical_encoding
DecisionTree
categorical_encoding
KNN
'accumulate' and 'output_prob'
RandomSample
setid_as_first_column
A: Teradata Package for Python Limitations and Considerations

Analytic Functions
Newly added arguments
(Only supported on Vantage 1.1 or later)
VarMax
'order_p', 'order_d', 'order_q', 'seasonal_order_p', 'seasonal_order_d', 'seasonal_
order_q'
**Note:**
These arguments are supported only on Vantage 1.1 or later. If used with Vantage 1.0, the following
**error will show:**
```python
[Teradata Database] [Error 4382] Argument {argument-name} is not defined in
the function mapping definition
```
### Using Sampling Function on Vantage1.0
Using Sampling function on Vantage 1.0 will result in following error. This function is now only supported
on Vantage1.1 or later.
```python
[Teradata Database] [Error 4381] Only one ON clause with either undefined or
empty correlation name can be mapped to ANY in function mapping definition.
```
### select() Method Requires Unique Column Names
The  select  method does not handle identical column names being passed as arguments.
**For example:**
```python
df = DataFrame('table1')
df.select(['col1', 'col1'])
```
To work around, use the  assign  method to provide an alias.
```python
c1 and c2 both refer to the col1 column
df.assign(c1 = df.col1, c2 = df.col1)
```
### Limited Missing Value Support
NaN and +/- Inf values can arise in floating point calculations. They are rendered when a DataFrame
is evaluated.
```python
df
value
row_id
```
A: Teradata Package for Python Limitations and Considerations

```python
2           -inf
1            inf
3            NaN
df.dtypes
row_id       str
value        float
```
NaN and +/- Inf values are not supported as  missing values . Particularly, there is no support to reference
these values in the Database Engine 20. Only the NULL value is supported as a  missing value , in which
case they are usually rendered as None. Floating point columns with NULL values can be rendered as
NaN. In this case, NaN is recognized as a  missing value .
```python
df[df.value.isna() == True]
value
row_id
3           None
df[df.value.isna() == False]
value
row_id
2           -inf
1            inf
```
### DataFrames Become Invalid When a Connection Context
### is Recreated
On recreating a connection context using the  create_context  command, most of the operations on the
DataFrames created using an earlier connection context fail.
The users receive a warning when the  create_context  command is rerun.
**For example:**
```python
create_context(host = "myhostname", username="myusername", password
= "mypassword")
df = DataFrame('table1')
create_context(host = "myhostname", username="myusername", password
= "mypassword")
```
A: Teradata Package for Python Limitations and Considerations

```python
UserWarning: [Teradata][teradataml](TDML_2002) Overwriting an existing context
associated with Teradata connection. Most of the operations on any teradataml
DataFrames created before this will not work.
df.select(['model','phrase_id'])
TeradataMlException: [Teradata][teradataml](TDML_2103) Internal Error: Non-zero
status returned from AED.
```
To work around this limitation, recreate the DataFrames with the new connection context.
For example, recreate the 'df' DataFrame after the new connection context is created in the
previous example.
```python
df = DataFrame('table1')
df.select(['model','phrase_id'])
phrase_id
model
1             1
1             2
1             2
1             1
1             1
1             1
(6, 3)
```
### Error Using responses Argument in MLE
### Function NaïveBayesTextClassifierPredict
The 'responses' argument in the ML Engine NaïveBayesTextClassifierPredict function ( teradataml.mle.
```python
NaiveBayesTextClassifierPredict ) is not supported in this release. Using this argument will result in
```
an error.
**For example:**
```python
nbt_predict_out = NaiveBayesTextClassifierPredict(object = nbt_out,
newdata =
complaints_tokens_test,
input_token_column =
'token',
doc_id_columns = 'doc_id',
model_type = "Bernoulli",
model_token_column =
'token',
```
A: Teradata Package for Python Limitations and Considerations

```python
model_category_column =
'category',
model_prob_column =
'prob',
newdata_partition_column =
'doc_id',
responses=['crash','no_crash'])
[Teradata Database] [Error 4382] Argument Responses is not defined in the function
mapping definition.
```
### copy_to API When Loading Data into PTI Table
When the copy_to API fails with the Database error message “4358 Time Series: TD_TIMECODE|
TD_SEQNO out of range for table”, you must check the permitted TD_TIMECODE and TD_SEQNO range
**using the following:**
```python
con = get_connection().connection
cur = con.cursor()
td_cur = cur.execute(“exec DBC.TD_TIMESERIES_RANGE('MyDB. table_name ')”)
td_cur.fetchall()
```
**Where:**
* MyDB  is the schema name
* table_name  is the name of the table to be created
And correct the data or the arguments like "seq.max" to suit the data.
**Note:**
When the data insertion fails, the table is already created.
If a table is to be created with the same table name after the error is handled, the existing table must
be dropped using the argument "if_exists = replace".
### Exploratory Data Analysis UI (EDA UI)
Exploratory Data Analysis UI (EDA UI) works based on ipywidgets. In some cases, ipywidgets do not work
on a Jupyter notebook. If this occurs, Teradata recommends choosing a kernel again.
A: Teradata Package for Python Limitations and Considerations

## Teradata Package for Python Considerations
This section provides information on topics that impact the use of the Teradata Package for Python.
* SQLAlchemy Compatibility Considerations
* User Permissions in Vantage
* Persistence of Tables Created by the teradataml Package in Vantage
* Filtering with Boolean Column Requires a Comparison
* String Comparisons may be Case Insensitive
* Install Pyyaml Python package for using SDK
### SQLAlchemy Compatibility Considerations
If SQLAlchemy version is 2.0.x and later, there are changes when using execute() method on objects
**returned by create_context(), get_context() and get_connection() APIs of teradataml:**
* Changes in execute() method of Engine object returned by create_context() and get_context() APIs
* of teradataml:
* execute() method of Engine object, returned by create_context() and get_context() APIs of
* teradataml, is not supported.
* Changes in execute() method of Connection objects returned by get_connection() API of teradataml:
* execute() method of Connection object, returned by get_connection() API of teradataml, only accepts
* executable SQL statements and doesn't support string SQL statements.
Teradata recommends using  execute_sql()  function provided by teradataml version 17.20.00.04 and later.
A: Teradata Package for Python Limitations and Considerations

teradataml 17.20.00.03 and earlier
teradataml 17.20.00.04 and later
create_context(<parameters>).
execute(<sql_query>)
create_context(<parameters>)
**Note:**
Required only once.
execute_sql(<sql_query>)
get_context().execute(<sql_query>)
execute_sql(<sql_query>)
**Note:**
Assumption is that connection is already established with
Vantage database.
get_connection().execute(<sql_query>)
execute_sql(<sql_query>)
**Note:**
Assumption is that connection is already established with
Vantage database.
### User Permissions in Vantage
To operate and interact with Vantage with the Teradata Package for Python, the user must have a series
of permissions granted.
A database user must be granted in advance the following permissions by the database administrator
before using the teradataml package.
* GRANT EXECUTE FUNCTION ON SYSLIB TO user;
* GRANT CONNECT THROUGH proxyuser TO PERMANENT user WITHOUT ROLE;
* GRANT SELECT ON TD_SERVER_DB.coprocessor TO user;
* GRANT INSERT ON TD_SERVER_DB.coprocessor TO user;
* GRANT EXECUTE FUNCTION ON TD_SERVER_DB.coprocessor TO user;
* GRANT CREATE SERVER ON TD_SERVER_DB TO user;
* GRANT EXECUTE FUNCTION ON TD_SYSFNLIB.QGEXECUTEFOREIGNQUERY TO user;
* GRANT EXECUTE FUNCTION ON TD_SYSFNLIB.QGINITIATOREXPORT TO user;
* GRANT EXECUTE FUNCTION ON TD_SYSFNLIB.QGINITIATORIMPORT TO user;
* GRANT EXECUTE FUNCTION ON TD_SYSFNLIB.QGREMOTEEXPORT TO user;
* GRANT EXECUTE FUNCTION ON TD_SYSFNLIB.QGREMOTEIMPORT TO user;
* GRANT CTCONTROL ON user TO proxy_user.
**Note:**
The proxyuser is a suitable database proxy user for the analytic functions as determined by the
database administrator.
A: Teradata Package for Python Limitations and Considerations

**Users must be granted the SELECT privilege to the following Data Dictionary views:**
* DBC.DatabasesV
* DBC.TablesV
* DBC.ColumnsV
* DBC.UsersV
* DBC.IndicesV
* DBC.All_RI_ChildrenV
* DBC.SessionInfoV
* DBC.DBCInfoV
teradataml requires that the user has certain permissions on the user's default database or the initial
default database specified using the  database  argument, or the temporary database when specified
using  temp_database_name .
**These permissions allow the user to:**
* Create tables and views to save results of teradataml analytic functions;
* Create views in the background for results of DataFrame APIs such as 'assign()', 'filter()', and so on,
* whenever the result for these APIs are accessed using a 'print()';
* Create view in the background on the query passed to the 'DataFrame.from_query()' API.
It is expected that the user has the required permissions to create these objects in the database that will
be used.
For views based on Vantage analytic functions, additional permissions may be required, which can be
**granted using:**
GRANT EXECUTE FUNCTION ON SYSLIB ... WITH GRANT OPTION
**For example:**
A user named ALICE connects to a non-default database named TOM to run the
NamedEntityFinder function.
```python
Load example data.
load_example_data("namedentityfinder",
['assortedtext_input', 'name_Find_configure'])
Create teradataml DataFrame objects.
nameFind_configure = DataFrame.from_table("name_Find_configure")
assortedtext_input = DataFrame.from_table("assortedtext_input")
Find entities using a configuration table containing model items.
NamedEntityFinder_out = NamedEntityFinder(newdata = assortedtext_input,
configure_table_data =
nameFind_configure,
text_column = 'content',
```
A: Teradata Package for Python Limitations and Considerations

```python
accumulate = ['id', 'source'],
entity_column = 'entity',
model = 'all',
show_entity_context = 0,
newdata_sequence_column = 'id',
configure_table_data_sequence_column='model_file')
teradatasql.OperationalError: [Version 17.0.0.2] [Session 37706] [Teradata
Database] [Error 3523] An owner referenced by user does not have EXECUTE FUNCTION
WITH GRANT OPTION access to SYSLIB.NamedEntityFinder.
```
**In order for this to work, the database TOM needs a certain permission, which can be granted using:**
```python
GRANT EXECUTE FUNCTION ON SYSLIB TO TOM WITH GRANT OPTION;
```
To let the function create the output views, or for the user to access such created views, additional
permissions may be required depending on which database is used and which object the view being
**created is based on, and can be granted using:**
GRANT SELECT ... WITH GRANT OPTION
**For Example:**
A user named ALICE connects to a non-default database named TOM to run the
NamedEntityFinder function.
```python
Connect to the default database to load the example dataset.
con = create_context(host="myhostname",
username="myusername", password="mypassword")
load_example_data("namedentityfinder",
['assortedtext_input', 'name_Find_configure'])
Reconnect to make sure all writes here on happen to the database
specified using 'temp_database_name'.
remove_context()
con = create_context(host="myhostname", username="myusername",
password="mypassword", temp_database_name="tom")
Create teradataml DataFrame objects.
nameFind_configure = DataFrame.from_table("name_Find_configure")
assortedtext_input = DataFrame.from_table("assortedtext_input")
Find entities using a configuration table containing model items.
NamedEntityFinder_out = NamedEntityFinder(newdata = assortedtext_input,
configure_table_data =
nameFind_configure,
```
A: Teradata Package for Python Limitations and Considerations

```python
text_column = 'content',
accumulate = ['id', 'source'],
entity_column = 'entity',
model = 'all',
show_entity_context = 0,
newdata_sequence_column = 'id',
configure_table_data_sequence_column='model_file')
teradatasql.OperationalError: [Version 17.0.0.2] [Session 37708] [Teradata
Database] [Error 3523] An owner referenced by user does not have SELECT WITH GRANT
OPTION access to alice.assortedtext_input.
```
**In order for this to work, the following permission must be granted:**
```python
GRANT SELECT ON ALICE.assortedtext_input TO TOM WITH GRANT OPTION;
GRANT SELECT ON ALICE.name_Find_configure TO TOM WITH GRANT OPTION;
```
### Persistence of Tables Created by the teradataml Package
### in Vantage
When users run a ML Engine analytic function, results are stored as tables in the database that is specified
in the Vantage connection.
However, not all of these resulting tables may be persistent (in permanent storage) in the connection
database. Specifically, tables that store models produced by analytic functions are non-persistent work
tables (temporary tables).
The difference is that tables in permanent storage persist across different sessions, whereas temporary
tables are automatically dropped at the end of a session.
Therefore, if the user establishes a Vantage connection in Python and calls an analytic function that creates
an analytic model table in the database, when the user eliminates the connection, the database session
will be terminated and the model table will be automatically dropped from the database.
To preserve a non-persistent model table created by teradataml, use the copy_to function with the model
as a table object input to the function, before disconnecting from the session where the model table
was created.
### Filtering with Boolean Column Requires a Comparison
The Database Engine 20 does not support the Boolean data type. So the values '0' and '1' are used instead.
To be consistent with pandas, '0' maps to 'False' and '1' maps to 'True'. This implies that the Boolean column
in teradataml is numeric and behaves like a numeric column.
**For example:**
A: Teradata Package for Python Limitations and Considerations

```python
import teradataml
df = df.assign(drop_columns = True,
Name= df.Name,
is_setosa = df.Name.str.contains('Setosa')
df
Name is_setosa
0  Iris-versicolor         0
1   Iris-virginica         0
2  Iris-versicolor         0
3   Iris-virginica         0
4   Iris-virginica         0
5      Iris-setosa         1
6   Iris-virginica         0
7  Iris-versicolor         0
8      Iris-setosa         1
9  Iris-versicolor         0
df[df.is_setosa == 1]
Name is_setosa
0  Iris-setosa         1
1  Iris-setosa         1
2  Iris-setosa         1
3  Iris-setosa         1
4  Iris-setosa         1
5  Iris-setosa         1
6  Iris-setosa         1
7  Iris-setosa         1
8  Iris-setosa         1
9  Iris-setosa         1
```
Using Boolean literals 'True' and 'False' when comparing is supported.
**For example:**
```python
df[df.is_setosa == True]
Name is_setosa
0  Iris-setosa         1
1  Iris-setosa         1
2  Iris-setosa         1
3  Iris-setosa         1
4  Iris-setosa         1
5  Iris-setosa         1
```
A: Teradata Package for Python Limitations and Considerations

```python
6  Iris-setosa         1
7  Iris-setosa         1
8  Iris-setosa         1
9  Iris-setosa         1
df[df.is_setosa == False]
Name is_setosa
0  Iris-versicolor         0
1   Iris-virginica         0
2  Iris-versicolor         0
3   Iris-virginica         0
4   Iris-virginica         0
5   Iris-virginica         0
6  Iris-versicolor         0
7  Iris-versicolor         0
8   Iris-virginica         0
9  Iris-versicolor         0
```
A comparison operator is needed whenever using a Boolean column, or else an error is thrown.
**For example:**
```python
df[df.is_setosa]
OperationalError: [Version 16.20.0.38] [Session 1961] [Teradata Database]
[Error 3707] Syntax error, expected something like a 'SUCCEEDS' keyword or a
'MEETS' keyword or a 'PRECEDES' keyword or an 'IN' keyword or a 'CONTAINS'
keyword between the word 'is_setosa' and ';'.
```
### String Comparisons may be Case Insensitive
Comparing string literals when filtering with the teradataml DataFrame is not necessarily case sensitive.
All character data, except for CLOBs, accessed in the execution of a Teradata SQL statement has
an attribute of CASESPECIFIC or NOT CASESPECIFIC, either by default or by explicit designation.
Character string comparisons use this attribute to determine whether the comparison is case blind or case
specific. Case specificity does not apply to CLOBs.
**Note:**
For more information, see the Character String Comparisons section in the  SQL Functions,
Expressions, and Predicates , B035-1145.
A: Teradata Package for Python Limitations and Considerations

**For example:**
```python
df.head(5)
SepalLength  SepalWidth  PetalLength  PetalWidth         Name
2                      4.7         3.2          1.3         0.2  Iris-setosa
4                      5.0         3.6          1.4         0.2  Iris-setosa
3                      4.6         3.1          1.5         0.2  Iris-setosa
1                      4.9         3.0          1.4         0.2  Iris-setosa
0                      5.1         3.5          1.4         0.2  Iris-setosa
df[df['Name'] == 'iris-SETOSA'].head(5)
SepalLength  SepalWidth  PetalLength  PetalWidth         Name
2                      4.7         3.2          1.3         0.2  Iris-setosa
4                      5.0         3.6          1.4         0.2  Iris-setosa
3                      4.6         3.1          1.5         0.2  Iris-setosa
1                      4.9         3.0          1.4         0.2  Iris-setosa
0                      5.1         3.5          1.4         0.2  Iris-setosa
```
A workaround is to use the  str.contains  method with  case = True .
```python
has_SETOSA = df['Name'].str.contains('iris-SETOSA', case = True)
df[has_SETOSA == True]
Empty DataFrame
Columns: [SepalLength, SepalWidth, PetalLength, PetalWidth, Name]
Index: []
```
### Install Pyyaml Python package for using SDK
To use any SDK from teradataml (for example, modelops SDK), install pyyaml package using  pip install
```python
PyYAML . Otherwise, importing required modules will fail with  ImportError .
```
## Teradata Package for Python Issues with VIEW Creation
* SQL versus teradataml Differences
* Issue while Creating a teradataml DataFrame from a Table in Non-Default Database
* Issue of not Having PERM SPACE to Create Temporary Tables
A: Teradata Package for Python Limitations and Considerations

* Issue while Accessing a View in other Schema
* CREATE VIEW and SELECT WITH [non_default_database].[any_view] Issue Solution
### SQL versus teradataml Differences
The main difference between SQL and teradataml is that teradataml internally creates temporary objects
like views, and in order to do so the user needs explicit permissions whereas while running plain SQL none
of these temporary objects are created.
teradataml requires that the user has certain permissions on the user's default database or the initial
default database specified using the  database  argument, or the temporary database when specified
using  temp_database_name .
**These permissions allow the user to do the following:**
* Create tables and views to save results of teradataml analytic functions.
* Create views in the background for results of DataFrame APIs such as assign(), filter(), and so on,
* whenever the result for these APIs are accessed using a print().
* Create view in the background on the query passed to the DataFrame.from_query() API.
It is expected that the user has the correct permissions to create these objects in the database that will
be used. The access to the views created may also require issuing additional  GRANT SELECT ... WITH
```python
GRANT OPTION  permission depending on which database is used and which object the view being created
```
is based on.
**Note:**
    * User created in Teradata, by default has CREATE VIEW and CREATE TABLE permissions in
    * own database.
    * Users created must have PERM space available to create tables.
    * VIEW creation does not require PERM space.
### Issue while Creating a teradataml DataFrame from a Table in
### Non-Default Database
### Description
teradataml user may encounter an error when the user tries to create a teradataml DataFrame from
a table in a non-default database as "[Error 3524] The user does not have CREATE VIEW access to
database [user_db]."
### Cause
User has revoked CREATE VIEW for each user on the user’s database.
A: Teradata Package for Python Limitations and Considerations

### Solution
Grant CREATE VIEW permission to the user who will work with zero PERM space.
```python
GRANT CREATE VIEW ON  userid  TO  userid
```
**User can use configure command to set the userid:**
```python
from teradataml import configure
configure.temp_view_database = " userid "
```
### Issue of not Having PERM SPACE to Create Temporary Tables
### Description
teradataml user may encounter the limitation of not having PERM SPACE to create temporary tables.
"[Error 3524] The user does not have CREATE VIEW access to database [user_db]."
### Cause
User does not have PERM SPACE or not allowed to create temporary tables on the database, or both.
### Solution
User can use configure command set the database or userid where they are allowed to create
temporary tables.
```python
from teradataml import configure
configure.temp_table_database = " database  or  userid "
```
### Issue while Accessing a View in other Schema
### Description
If a user created the context with the same default database, and tried to access the view in the other
schema. The user will get an error as - "An owner referenced by user does not have SELECT WITH GRANT
OPTION access to [ non_default_database ].[ any_view ]"
### Cause
User created context with the default database.
### Solution
Grant select to the USERNAME on the database where temporary objects are being created.
```python
GRANT SELECT ON USERNAME TO  database  WITH GRANT OPTION;
```
A: Teradata Package for Python Limitations and Considerations

teradataml allows user to specify different databases for creating tables and views. You can specify these
**databases using configuration options:**
```python
from teradataml import configure
configure.temp_table_database = " database "
configure.temp_view_database = " database "
```
Use these options in cases when user has permissions to create tables in one database and view in
other database.
For more details, see  temp_table_database and temp_view_database .
### CREATE VIEW and SELECT WITH [non_default_database].
### [any_view] Issue Solution
### Create the setup
* Create context using admin "dbc" user.
```python
from teradataml import *
create_context(host="myhostname", username="dbc", password="dbc")
```
* Create required users and grant the required permissions: two users with PERM space and one user
* with no PERM space.
```python
get_connection().execute("create user alice_views as PERM = 100000000 ,
PASSWORD = \"alice\"")
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000021EAD692630>
get_connection().execute("create user user_with_perm as PERM =
100000000 , PASSWORD = \"alice\"")
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000021EAD692A20>
get_connection().execute("create user user_no_perm as PERM = 0 ,
PASSWORD = \"alice\"")
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000021EAD692D68>
```
* Grant SELECT permission to users 'user_with_perm' and 'user_no_perm' on table 'titanic' in
* database 'alice'.
```python
get_connection().execute("grant select on alice.titanic
to user_no_perm;")
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000021EAD6B37B8>
```
A: Teradata Package for Python Limitations and Considerations

```python
get_connection().execute("grant select on alice.titanic
to user_with_perm;")
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000021EAD6B3780>
```
* Grant CREATE VIEW permission to users 'user_with_perm' and 'user_no_perm' on
* database 'alice_view'.
```python
get_connection().execute("grant create view on alice_views
to user_no_perm;")
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000021EAD6B3908>
get_connection().execute("grant create view on alice_views
to user_with_perm;")
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000021EAD6B38D0>
```
### Example 1: Case explaining user with no PERM space
* Create context using 'user_no_perm' user that does not have any perm space and pass
* "temp_database_name" as 'alice_view', so that internal objects are created in 'alice_views'.
```python
create_context(host="myhostname", username="user_no_perm",
password="mypassword", temp_database_name="alice_views")
C:\Users\workspace\pyTeradata\teradataml\context\context.py:404:
UserWarning: [Teradata][teradataml](TDML_2002) Overwriting an existing
context associated with Teradata Vantage C
onnection. Most of the operations on any teradataml DataFrames created
before this will not work.
warnings.warn(Messages.get_message(MessageCodes.OVERWRITE_CONTEXT))
```
* Create DataFrame using DataFrame.from_query(), this results in error "An owner referenced by user
* does not have any WITH GRANT OPTION access to alice.titanic."
```python
df = DataFrame.from_query("select * from alice.titanic")
Traceback (most recent call last):
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\sqlalchemy\engine\base.py", line 1800, in _execute_context
cursor, statement, parameters, context
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\sqlalchemy\engine\default.py", line 717, in do_execute
cursor.execute(statement, parameters)
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\teradatasql\__init__.py", line 686, in execute
self.executemany (sOperation, None, ignoreErrors)
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\teradatasql\__init__.py", line 933, in executemany
```
A: Teradata Package for Python Limitations and Considerations

```python
raise OperationalError (sErr)
teradatasql.OperationalError: [Version 17.10.0.16] [Session 3829] [Teradata
Database] [Error 3523] An owner referenced by user does not have any WITH
GRANT OPTION access to alice.titanic.
at gosqldriver/teradatasql.formatError ErrorUtil.go:88
at gosqldriver/teradatasql.
(*teradataConnection).formatDatabaseError ErrorUtil.go:216
at gosqldriver/teradatasql.
(*teradataConnection).makeChainedDatabaseError ErrorUtil.go:232
at gosqldriver/teradatasql.
(*teradataConnection).processErrorParcel TeradataConnection.go:803
at gosqldriver/teradatasql.
(*TeradataRows).processResponseBundle TeradataRows.go:2229
at gosqldriver/teradatasql.
(*TeradataRows).executeSQLRequest TeradataRows.go:814
at gosqldriver/teradatasql.newTeradataRows TeradataRows.go:673
at gosqldriver/teradatasql.
(*teradataStatement).QueryContext TeradataStatement.go:122
at gosqldriver/teradatasql.
(*teradataConnection).QueryContext TeradataConnection.go:1304
at database/sql.ctxDriverQuery ctxutil.go:48
at database/sql.(*DB).queryDC.func1 sql.go:1759
at database/sql.withLock sql.go:3437
at database/sql.(*DB).queryDC sql.go:1754
at database/sql.(*Conn).QueryContext sql.go:2013
at main.goCreateRows goside.go:666
at _cgoexp_7cdf4597d74c_goCreateRows _cgo_gotypes.go:340
at runtime.cgocallbackg1 cgocall.go:314
at runtime.cgocallbackg cgocall.go:233
at runtime.cgocallback asm_amd64.s:971
at runtime.goexit asm_amd64.s:1571
```
* Using 'configure' option from teradataml.
    * Set the 'temp_table_database' to 'alice_views' so that internal tables are created in the same.
```python
configure.temp_table_database = "alice_views"
```
    * Set the 'temp_view_database' to 'user_no_perm' so that internal views are created in the user
    * without perm space.
```python
configure.temp_view_database = "user_no_perm"
```
* Create DataFrame using DataFrame.from_query(), this works.
```python
df = DataFrame.from_query("select * from alice.titanic")
```
A: Teradata Package for Python Limitations and Considerations

```python
df
SURVIVED  pclass                                         name  gender   age  sibsp
parch             ticket     fare cabin embarked
PASSENGER
244               0       3                Maenpaa, Mr. Matti Alexanteri    male  22.0      0      0  STON/O
2. 3101275   7.1250  None        S
101               0       3                      Petranec, Miss. Matilda  female  28.0      0
0             349245   7.8958  None        S
570               1       3                            Jonsson, Mr. Carl    male  32.0      0
0             350417   7.8542  None        S
835               0       3                       Allum, Mr. Owen George    male  18.0      0
0               2223   8.3000  None        S
692               1       3                           Karun, Miss. Manca  female   4.0      0
1             349256  13.4167  None        C
284               1       3                   Dorking, Mr. Edward Arthur    male  19.0      0      0
A/5. 10482   8.0500  None        S
427               1       2  Clarke, Mrs. Charles V (Ada Maria Winfield)  female  28.0      1
0               2003  26.0000  None        S
305               0       3            Williams, Mr. Howard Hugh "Harry"    male   NaN      0      0
A/5 2466   8.0500  None        S
530               0       2                  Hocking, Mr. Richard George    male  23.0      2
1              29104  11.5000  None        S
265               0       3                           Henry, Miss. Delia  female   NaN      0
0             382649   7.7500  None        Q
```
* Sample few rows from DataFrame.
```python
df.sample(1)
SURVIVED  pclass                        name   gender  age  sibsp  parch   ticket    fare cabin
embarked  sampleid
PASSENGER
203               0       3  Johanson, Mr. Jakob Alfred     male   34      0      0  3101264  6.4958
None        S         1
```
### Example 2: Case explaining user with PERM space
* Create context using 'user_with_perm' user that have perm space and pass "temp_database_name"
* as 'alice_view', so that internal objects are created in 'alice_views'.
```python
create_context(host="myhostname", username="user_with_perm",
password="mypassword", temp_database_name="alice_views")
C:\Users\workspace\pyTeradata\teradataml\context\context.py:404:
UserWarning: [Teradata][teradataml](TDML_2002) Overwriting an existing
context associated with Teradata Vantage C
onnection. Most of the operations on any teradataml DataFrames created
before this will not work.
warnings.warn(Messages.get_message(MessageCodes.OVERWRITE_CONTEXT))
```
* Create DataFrame using DataFrame.from_query(), this results in error "The user does not have
* CREATE VIEW access to database user_no_perm."
```python
df = DataFrame.from_query("select * from alice.titanic sample 1")
Traceback (most recent call last):
File "C:\Users\workspace\pyTeradata\teradataml\dataframe\dataframe.py",
line 180, in __init__
UtilFuncs._create_view(self._table_name, self._query)
File "C:\Users\workspace\pyTeradata\teradataml\common\utils.py", line 671,
in _create_view
UtilFuncs._execute_ddl_statement(crt_view)
File "C:\Users\workspace\pyTeradata\teradataml\common\utils.py", line 528,
in _execute_ddl_statement
cursor.execute(ddl_statement)
```
A: Teradata Package for Python Limitations and Considerations

```python
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\teradatasql\__init__.py", line 686, in execute
self.executemany (sOperation, None, ignoreErrors)
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\teradatasql\__init__.py", line 933, in executemany
raise OperationalError (sErr)
teradatasql.OperationalError: [Version 17.10.0.16] [Session 3833] [Teradata
Database] [Error 3524] The user does not have CREATE VIEW access to
database user_no_perm.
at gosqldriver/teradatasql.formatError ErrorUtil.go:88
at gosqldriver/teradatasql.
(*teradataConnection).formatDatabaseError ErrorUtil.go:216
at gosqldriver/teradatasql.
(*teradataConnection).makeChainedDatabaseError ErrorUtil.go:232
at gosqldriver/teradatasql.
(*teradataConnection).processErrorParcel TeradataConnection.go:803
at gosqldriver/teradatasql.
(*TeradataRows).processResponseBundle TeradataRows.go:2229
at gosqldriver/teradatasql.
(*TeradataRows).executeSQLRequest TeradataRows.go:814
at gosqldriver/teradatasql.newTeradataRows TeradataRows.go:673
at gosqldriver/teradatasql.
(*teradataStatement).QueryContext TeradataStatement.go:122
at gosqldriver/teradatasql.
(*teradataConnection).QueryContext TeradataConnection.go:1304
at database/sql.ctxDriverQuery ctxutil.go:48
at database/sql.(*DB).queryDC.func1 sql.go:1759
at database/sql.withLock sql.go:3437
at database/sql.(*DB).queryDC sql.go:1754
at database/sql.(*Conn).QueryContext sql.go:2013
at main.goCreateRows goside.go:666
at _cgoexp_7cdf4597d74c_goCreateRows _cgo_gotypes.go:340
at runtime.cgocallbackg1 cgocall.go:314
at runtime.cgocallbackg cgocall.go:233
at runtime.cgocallback asm_amd64.s:971
at runtime.goexit asm_amd64.s:1571
```
* Using 'configure' option from teradataml.
    * Set the 'temp_table_database' to 'alice_views' so that internal tables are created in the same.
```python
configure.temp_table_database = "alice_views"
```
A: Teradata Package for Python Limitations and Considerations

* Set the 'temp_view_database' to 'user_no_perm' so that internal views are created in the user
    * without perm space.
```python
configure.temp_view_database = "user_no_perm"
```
* Create DataFrame using DataFrame.from_query(), this results in error " An owner referenced by user
* does not have any WITH GRANT OPTION access to alice.titanic."
```python
df = DataFrame.from_query("select * from alice.titanic sample 1")
Traceback (most recent call last):
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\sqlalchemy\engine\base.py", line 1800, in _execute_context
cursor, statement, parameters, context
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\sqlalchemy\engine\default.py", line 717, in do_execute
cursor.execute(statement, parameters)
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\teradatasql\__init__.py", line 686, in execute
self.executemany (sOperation, None, ignoreErrors)
File "C:\Users\.conda\envs\teradatamla\lib\site-
packages\teradatasql\__init__.py", line 933, in executemany
raise OperationalError (sErr)
teradatasql.OperationalError: [Version 17.10.0.16] [Session 3833] [Teradata
Database] [Error 3523] An owner referenced by user does not have any WITH
GRANT OPTION access to alice.titanic.
at gosqldriver/teradatasql.formatError ErrorUtil.go:88
at gosqldriver/teradatasql.
(*teradataConnection).formatDatabaseError ErrorUtil.go:216
at gosqldriver/teradatasql.
(*teradataConnection).makeChainedDatabaseError ErrorUtil.go:232
at gosqldriver/teradatasql.
(*teradataConnection).processErrorParcel TeradataConnection.go:803
at gosqldriver/teradatasql.
(*TeradataRows).processResponseBundle TeradataRows.go:2229
at gosqldriver/teradatasql.
(*TeradataRows).executeSQLRequest TeradataRows.go:814
at gosqldriver/teradatasql.newTeradataRows TeradataRows.go:673
at gosqldriver/teradatasql.
(*teradataStatement).QueryContext TeradataStatement.go:122
at gosqldriver/teradatasql.
(*teradataConnection).QueryContext TeradataConnection.go:1304
at database/sql.ctxDriverQuery ctxutil.go:48
at database/sql.(*DB).queryDC.func1 sql.go:1759
at database/sql.withLock sql.go:3437
at database/sql.(*DB).queryDC sql.go:1754
```
A: Teradata Package for Python Limitations and Considerations

```python
at database/sql.(*Conn).QueryContext sql.go:2013
at main.goCreateRows goside.go:666
at _cgoexp_7cdf4597d74c_goCreateRows _cgo_gotypes.go:340
at runtime.cgocallbackg1 cgocall.go:314
at runtime.cgocallbackg cgocall.go:233
at runtime.cgocallback asm_amd64.s:971
at runtime.goexit asm_amd64.s:1571
```
* Using 'configure' option from teradataml.
    * Set the 'temp_view_database' to 'user_with_perm' so that internal views are created in the same.
```python
configure.temp_view_database = "user_with_perm"
```
* Create DataFrame using DataFrame.from_query(), this works.
```python
df = DataFrame.from_query("select * from alice.titanic sample 1")
df
PASSENGER  SURVIVED  pclass                    name  gender  age  sibsp  parch ticket    fare cabin embarked
0        811         0       3  Alexander, Mr. William    male   26      0      0   3474  7.8875  None        S
```
A: Teradata Package for Python Limitations and Considerations