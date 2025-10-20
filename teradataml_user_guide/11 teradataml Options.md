# teradataml Options

* Configuration Options
* Display Options
* set_config_params
## Configuration Options
* default_varchar_size
* temp_table_database and temp_view_database
* byom_install_location
* val_install_location
* database_version
* read_nos_function_mapping
* write_nos_function_mapping
* inline_plot
* certificate_file
* table_operator
* openml_user_env
* local_storage
* stored_procedure_install_location
* temp_object_type
* use_short_object_name
### default_varchar_size
The  default_varchar_size  option specifies the size of the varchar type of a column while creating a table
using 'copy_to_sql'.
The default size is 1024.
You can configure this parameter using options.
### Example
```python
teradataml.options.configure.default_varchar_size = 512
```

### temp_table_database and temp_view_database
teradataml internally creates database objects (tables and views) as required while executing several
functions. The  temp_table_database  and  temp_view_database  configurable options allow users to
specify database names where tables and views are created, respectively. These two options give users
flexibility to specify database names other than the default database to create views and tables.
The  create_context  function provides  temp_database_name  and  database  arguments that also allow
users to specify the database name, where database objects can be created. If none of them are specified
then the objects are created in users' default database.
While using these options,
* If the configuration option  temp_table_database  is None, then tables are created in the database
* specified at the time of context creation.
* If the configuration option  temp_view_database  is None, then views are created in the database
* specified at the time of context creation.
* Teradata recommends using these options when following conditions are true, otherwise, use the
* arguments in the create_context() function:
    * When a user has permissions to create tables in one database while views in other database.
    * In case a user wishes to create these temporary objects in a database different than the one
    * specified at the time of context creation. This allows the user to change the database name
    * without re-creating the context.
    * In case a user forgets to use the create_context() arguments to set the  temp_database_name ,
    * then use these options.
### Example
```python
from teradataml import *
Set the teradataml Configuration property 'temp_table_database' True
configure.temp_table_database = "tempdb"
load_example_data("dataframe", "admissions_train")
Load example loads sample tables to "tempdb" database as option is set.
admissions_train
= DataFrame.from_table(in_schema("tempdb","admissions_train"))
glm_out1 = GLM(formula = "admitted ~ stats + masters + gpa + programming",
family = "LOGISTIC",
linkfunction = "LOGIT",
data = admissions_train,
weights = "1",
```

```python
threshold = 0.01,
maxit = 25,
step = False,
intercept = True
If we observe query, we can find that output table is created in "tempdb"
glm_out1.output.show_query()
select * from "tempdb"."teradataml_td_glm_mle0_1623374476821412";
```
### byom_install_location
The  byom_install_location  configuration option indicates the name of the database where Bring Your
Own Model functions are installed. This option is internally used by BYOM functions like H2OPredict()
and PMMLPredict().
### Example
* Import all modules.
```python
from teradataml import *
```
* Set the configuration option  byom_install_location  to "mldb".
```python
Set the teradataml Configuration option 'byom_install_location'
to "mldb"
configure.byom_install_location = "mldb"
```
* Load example data.
```python
load_example_data("byom", "iris_test")
```
* Load model files.
```python
Load model file into Vantage.
model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "iris_mojo_glm_h2o_model")
```
* Save the model.
```python
save_byom("iris_mojo_glm_h2o_model", model_file, "byom_models")
```
* Retrieve the saved model and directly pass the output as an input to the H2OPredict function.
```python
modeldata =
retrieve_byom("iris_mojo_glm_h2o_model", table_name="byom_models")
```

```python
result = H2OPredict(newdata=iris_test,
newdata_partition_column='id',
newdata_order_column='id',
modeldata=modeldata,
modeldata_order_column='model_id',
model_output_fields=['label', 'classProbabilities'],
accumulate=['id', 'sepal_length', 'petal_length'],
overwrite_cached_models='*',
enable_options='stageProbabilities',
model_type='OpenSource'
```
* Print the results of the H2OPredict function.
```python
Print the results.
print(result.result)
```
### val_install_location
The  val_install_location  configuration option indicates the name of the database where Vantage Analytic
Library functions are installed. This option is internally used by VAL functions.
### Example
* Import all modules.
```python
from teradataml import *
```
* Set the configuration option  val_install_location  to "SYSLIB".
```python
Set the teradataml configuration option 'val_install_location'
to "SYSLIB"
configure.val_install_location = "SYSLIB"
```
* Create a data frame.
```python
df = DataFrame("credit_tran")
```
* Perform Association analysis using default values.
```python
Perform Association analysis using default values.
obj = valib.Association(data=df,
group_column="cust_id", item_column="channel")
```
* Print the affinity result.

```python
Print the affinity result. Only affinity result for default
combination 11 is produced.
print(obj.result_11)
```
### database_version
The  database_version  configuration option specifies the actual database version of the system
teradataml is connected to.
### Example
```python
teradataml.options.configure.database_version = "17.05a.00.147"
```
### read_nos_function_mapping
The  read_nos_function_mapping  configuration option specifies the function mapping name for the
READ_NOS table operator function.
### Example
```python
teradataml.options.configure.read_nos_function_mapping = "read_nos_fm"
```
### write_nos_function_mapping
The  write_nos_function_mapping  configuration option specifies the function mapping name for the
WRITE_NOS table operator function.
### Example
```python
teradataml.options.configure.write_nos_function_mapping = "write_nos_fm"
```
### inline_plot
The  inline_plot  option specifies whether to display the plot inline or not.
**Note:**
This option is not applicable for machines running linux and mac os.
### Example: Set the option to display plot in a separate window
```python
teradataml.options.configure.inline_plot = True
```

### certificate_file
The  certificate_file  specifies the path of the certificate file, which is used in encrypted REST service calls.
### Example
```python
teradataml.options.configure.certificate_file='certificate.crt'
```
### table_operator
Use the table_operator function to specify the name of the table operator.
Permitted values: "Apply, "Script".
### Example
```python
teradataml.options.configure.table_operator = "apply"
```
### openml_user_env
The  openml_user_env  option specifies the user environment to be used for OpenML.
### Example
```python
_env_name = "OpenAF"Name of the user defined environment.
teradataml.options.configure.openml_user_env = get_env(_env_name)
```
### local_storage
The  local_storage  option specifies the location on the client where garbage collector folder will be created.
### Example: Set the garbage collector location to "/Users/gc/"
```python
teradataml.options.configure.local_storage = "/Users/gc/"
```
### stored_procedure_install_location
The  stored_procedure_install_location  option specifies the name of the database where stored
procedures are installed.
### Example
Set the Stored Procedure install location to 'SYSLIB' when stored procedures are installed in 'SYSLIB'.

```python
teradataml.options.configure.stored_procedure_install_location = "SYSLIB"
```
### temp_object_type
The  temp_object_type  option specifies the type of temporary database objects created internally
by teradataml.
Permitted Values: * "VT" - Volatile tables.
**Note:**
If this option is set to "VT" and "persist" argument of analytic functions is set to True, then volatile
tables are not created as volatile table cannot be persisted and "persist" argument takes precedence.
### Example
Set the type of temporary database objects to "VT" to create volatile internal tables.
```python
teradataml.options.configure.temp_object_type = "VT"
```
## Display Options
You can control the display of a teradataml DataFrame by setting options in the display module.
```python
from teradataml.options.display import display
```
Use the help command to show the built-in document about the display module.
```python
help(display)
```
* byte_encoding
* max_rows
* precision
* print_sqlmr_query
* blob_length
* suppress_vantage_runtime_warnings
* geometry_column_length
* enable_ui
### byte_encoding
The  byte_encoding  option controls the encoding used for printing byte-like types in a DataFrame. The
**default is Base16. The following encodings are supported:**
* BaseX

* X is a power of 2 (for example, 2, 8, 16).
* BaseY
* Y is not a power of 2 (for example, 10 and 36).
* Base64M (MIME)
* ASCII
**Note:**
The from_bytes function is used to display byte-like types. See  SQL Functions, Expressions, and
Predicates  for more information on specifying arguments for FROM_BYTES.
### Example: Default is Base16
```python
display.byte_encoding
'base16'
DataFrame('byte')
byte      varbyte         blob
id
2   b'32616263'  b'32616263'  b'32616263'
1   b'31616263'  b'31616263'  b'31616263'
0   b'30616263'  b'30616263'  b'30616263'
```
### Example: Set the value to ascii
```python
display.byte_encoding = 'ascii'
DataFrame('byte')
byte  varbyte     blob
id
2   b'2abc'  b'2abc'  b'2abc'
1   b'1abc'  b'1abc'  b'1abc'
0   b'0abc'  b'0abc'  b'0abc'
```
### max_rows
The  max_rows  option controls the default value for the head() method of the DataFrame and the number
of rows printed from a DataFrame. The default value is 10.

### Example: Set the value to 5
```python
display.max_rows = 5
DataFrame('iris')
SepalLength  SepalWidth  PetalLength  PetalWidth             Name
primary_index
0                      5.1         3.5          1.4         0.2      Iris-setosa
40                     5.0         3.5          1.3         0.3      Iris-setosa
80                     5.5         2.4          3.8         1.1  Iris-versicolor
101                    5.8         2.7          5.1         1.9   Iris-virginica
141                    6.9         3.1          5.1         2.3   Iris-virginica
```
### precision
The  precision  option controls the number of places to round a numeric column when printing the
DataFrame. The default value is 3.
This option only affects data when printed. When retrieving actual values from the DataFrame (such as with
to_pandas()), the numeric column is returned with precision as specified in the schema.
**Note:**
The  precision  setting does not affect columns of type float.
### Example: Default value is 3
```python
display.precision
3
DataFrame('numeric')
integer smallint bigint decimal     float number number2 number3 byteint
id
2        2        3      4   4.860  5.313308  5.737     .46    .687       2
1        1        2      2   2.430  2.656654  2.869     .23    .344       1
3        3        4      8   7.290  7.969962  8.606     .69   1.031       3
```
### Example: Set the value to 1
```python
display.precision = 1
DataFrame('tips')
integer smallint bigint decimal     float number number2 number3 byteint
id
```

```python
2        2        3      4     4.9  5.313308    5.7      .5      .7       2
1        1        2      2     2.4  2.656654    2.9      .2      .3       1
3        3        4      8     7.3  7.969962    8.6      .7       1       3
```
### print_sqlmr_query
The  print_sql_mr_query  option is a boolean that controls whether or not to print the query being sent to
when an analytic function is called. The default is False.
### Example: Set the value to True
```python
display.print_sqlmr_query = True
from teradataml.analytics.KMeans import KMeans
computers_train1 = DataFrame.from_table("computers_train1")
kmeanssample_centroid = DataFrame.from_table("kmeanssample_centroid")
result = KMeans(data=computers_train1,
centroids_table=kmeanssample_centroid,
centers=8,
threshold=0.0395,
iter_max=10,
unpack_columns=False,
seed=10,
data_sequence_column='primary_index'
SELECT * FROM KMeans(
ON "computers_train1" AS InputTable
ON "kmeanssample_centroid" AS CentroidsTable
OUT TABLE OutputTable(TDML.ml__td_kmeans0_1538706050004791)
OUT TABLE ClusteredOutput(TDML.ml__td_kmeans1_1538702918024205)
USING
NumClusters('8')
MaxIterNum('10')
Seed('10')
SequenceInputBy('InputTable:primary_index')
) as sqlmr
```

### blob_length
The  blob_length  option specifies the default length of BLOB column to display in a teradataml DataFrame.
You can set this option to None to display the complete BLOB data. Default value is 10.
### Use the default value
```python
display.blob_length
10
df = DataFrame("byom_models")
df
model
model_id
model7    b'504B03041400080808...'
```
### Set the length to 20
```python
display.blob_length=20
df
model
model_id
model7    b'504B0304140008080800B36DC2520000000000...'
```
### suppress_vantage_runtime_warnings
The  suppress_vantage_runtime_warnings  option specifies whether to display the warnings raised by
Vantage or not.
When set to True, warnings raised by Vantage are not displayed. Otherwise, warnings are displayed.
Default value is False.
**Note:**
This option may also suppress valid warnings. So, Teradata does not recommend using this option
unless it is really necessary.
### Example 1: Suppress warnings
```python
display.suppress_vantage_runtime_warnings = True
```
Example: Compute the average values in a series, using the 'C' moving average type.

```python
obj1 = MovingAverage(data=titanic_data,
data_partition_column='fare',
data_order_column='passenger',
include_first=False,
alpha=0.1,
start_rows=2,
window_size=3,
mavgtype='C')
```
### Example 2: Enable runtime warnings
```python
display.suppress_vantage_runtime_warnings = False
```
Example: Compute the average values in a series, using the 'C' moving average type.
```python
obj2 = MovingAverage(data=titanic_data,
data_partition_column='fare',
data_order_column='passenger',
include_first=False,
alpha=0.1,
start_rows=2,
window_size=3,
mavgtype='C')
VantageRuntimeWarning: [Teradata][teradataml](TDML_2086) Following warning
raised from Vantage with warning code: 4862
[Teradata Database] [Warning 4862] Query executed on Primary Cluster as the
request is multi-statement request
warnings.warn(msg_.format(warnRes[5], warnRes[6]), VantageRuntimeWarning)
```
### geometry_column_length
The  geometry_column_length  option specifies the length of the Geometry column while displaying
the GeoDataFrame.
By default, geometry_column_length is set to 30. So, GeoDataFrame displays only first 30 characters in
every geometry column.
### Example 1: Display a GeoDataFrame with default geometry_column_length
```python
df = DataFrame("sample_shapes")
df
points
linestrings                        polygons
```

```python
geom_collections                     geosequence
skey
1006    POINT (235.52 54.546 7.4564)  LINESTRING (1.35 3.6456
4.5,3.  POLYGON ((0 0 0,0 0 20,0 20 0,
None                            None
1001                   POINT (10 20)    LINESTRING (1 1,2 2,3 3,4 4)  POLYGON
((0 0,0 20,20 20,20 0,  GEOMETRYCOLLECTION (POINT (10   GEOSEQUENCE((10 20,30
40,50 60
1010  MULTIPOINT (10.345 20.32 30.6,  MULTILINESTRING ((1 3 6,3
0 6,  MULTIPOLYGON (((0 0 0,0 0 20,0
None                            None
1008  MULTIPOINT (1.65 1.76,1.23 3.7  MULTILINESTRING ((1 3,3
0,0 1)  MULTIPOLYGON (((0 0,0 20,20 20
None                            None
1003           POINT (235.52 54.546)  LINESTRING (1.35
3.6456,3.6756  POLYGON ((0.6 0.8,0.6 20.8,20.
None                            None
1002                     POINT (1 3)        LINESTRING (1 3,3 0,0 1)  POLYGON
((0 0,0 20,20 20,20 0,                            None  GEOSEQUENCE((10 10,15
15,-2 0)
1004                POINT (10 20 30)  LINESTRING (10 20 30,40 50
60,  POLYGON ((0 0 0,0 10 20,20 20   GEOMETRYCOLLECTION (POINT
(10                             None
1005                   POINT (1 3 5)  LINESTRING (1 3 6,3 0 6,6
0 1)  POLYGON ((0 0 0,0 0 20.435,0.0  GEOMETRYCOLLECTION (POINT
(10                             None
1007  MULTIPOINT (1 1,1 3,6 3,10 5,2  MULTILINESTRING ((1 1,1
3,6 3)  MULTIPOLYGON (((1 1,1 3,6 3,6
None                            None
1009  MULTIPOINT (10 20 30,40 50 60,  MULTILINESTRING ((10 20
30,40   MULTIPOLYGON (((0 0 0,0 20 20,
None                            None

```
### Example 2: Set geometry_column_length to 35 and display the
### same GeoDataFrame
```python
display.geometry_column_length=35
df
points
linestrings                             polygons
geom_collections                          geosequence
skey
```

```python
1010  MULTIPOINT (10.345 20.32 30.6,40.23  MULTILINESTRING ((1 3 6,3 0 6,6
0 1  MULTIPOLYGON (((0 0 0,0 0 20,0 20 0
None                                 None
1004                     POINT (10 20 30)  LINESTRING (10 20 30,40 50 60,70
80  POLYGON ((0 0 0,0 10 20,20 20 30,20  GEOMETRYCOLLECTION (POINT (10 20
30                                 None
1003                POINT (235.52 54.546)  LINESTRING (1.35 3.6456,3.6756
0.23  POLYGON ((0.6 0.8,0.6 20.8,20.6 20.
None                                 None
1006         POINT (235.52 54.546 7.4564)  LINESTRING (1.35 3.6456 4.5,3.6756
POLYGON ((0 0 0,0 0 20,0 20 0,0 20
None                                 None
1001                        POINT (10 20)         LINESTRING (1 1,2 2,3 3,4
4)  POLYGON ((0 0,0 20,20 20,20 0,0 0))  GEOMETRYCOLLECTION (POINT (10 10),P
GEOSEQUENCE((10 20,30 40,50 60),(20
1002                          POINT (1 3)             LINESTRING (1 3,3 0,0
1)  POLYGON ((0 0,0 20,20 20,20 0,0 0),                                 None
GEOSEQUENCE((10 10,15 15,-2 0),(200
1005                        POINT (1 3 5)       LINESTRING (1 3 6,3 0 6,6 0
1)  POLYGON ((0 0 0,0 0 20.435,0.0 20.4  GEOMETRYCOLLECTION (POINT (10 20
30                                 None
1008  MULTIPOINT (1.65 1.76,1.23 3.76,6.2  MULTILINESTRING ((1 3,3 0,0 1),
(1.3  MULTIPOLYGON (((0 0,0 20,20 20,20 0
None                                 None
1007   MULTIPOINT (1 1,1 3,6 3,10 5,20 1)  MULTILINESTRING ((1 1,1 3,6
3),(10   MULTIPOLYGON (((1 1,1 3,6 3,6 0,1 1
None                                 None
1009  MULTIPOINT (10 20 30,40 50 60,70 80  MULTILINESTRING ((10 20 30,40
50 60  MULTIPOLYGON (((0 0 0,0 20 20,20 20
None                                 None

```
### enable_ui
The  enable_ui  option specifies whether to display exploratory data analysis UI when DataFrame is printed
or not. When set to True, EDA UI is displayed, otherwise it is disabled.
By default, display.enable_ui is set to True.
### Example 1: Set the option to display the titanic DataFrame
```python
df = DataFrame("titanic")
df
```

### Example 2: Set the option to hide the titanic DataFrame
```python
display.enable_ui=False
df
```
## set_config_params
Use the set_config_params function to set the configurations in Vantage.
Alternatively, you can set the configuration parameters independently using the  teradataml.
```python
configure  module.
```
**Optional arguments "kwargs" specifies keyword arguments:**
certificate_file
    * Specifies the path of the certificate file, which is used in encrypted REST service calls.

default_varchar_size
    * Specifies the size of varchar datatype in Vantage.
    * The default size is 1024.
vantage_version
    * Specifies the version of the Vantage system teradataml is connected to.
val_install_location
    * Specifies the database name where Vantage Analytic Library (VAL) functions are installed.
byom_install_location
    * Specifies the database name where Bring Your Own Model (BYOM) functions are installed.
database_version
    * Specifies the database version of the system teradataml is connected to.
read_nos_function_mapping
    * Specifies the function mapping name for the READ_NOS table operator function.
write_nos_function_mapping
    * Specifies the function mapping name for the WRITE_NOS table operator function.
inline_plot
    * Specifies whether to display the plot inline or not.
    * Note:
    * This option is not applicable for machines running linux and mac os.
openml_user_env
    * Specifies the user environment to be used for OpenML.
local_storage
    * Specifies the location on the client where garbage collector folder will be created.
stored_procedure_install_location
    * Specifies the name of the database where stored procedures are installed.
table_operator
    * Specifies the name of the table operator.

* Permitted values: "Apply, "Script".
temp_object_type
    * Specifies the type of temporary database objects created internally by teradataml.
    * Permitted Values: * "VT" - Volatile tables.
    * Note:
    * If this option is set to "VT" and "persist" argument of analytic functions is set to True,
    * then volatile tables are not created as volatile table cannot be persisted and "persist"
    * argument takes precedence.
### Example 1: Set configuration parameters using set_config_params() function
```python
from teradataml import set_config_params
set_config_params(auth_token="abc-pqr-123",
ues_url="https://teracloud/v1/accounts/xyz-234-76085/open-
analytics",
certificate_file="cert.crt",
default_varchar_size=512,
val_install_location="VAL_USER",
sandbox_container_id="bgf1233csdh123",
read_nos_function_mapping="read_nos_fm",
write_nos_function_mapping="write_nos_fm",
True
```
### Example 2: Set configuration parameters using configure module
```python
from teradataml import configure
configure.auth_token="abc-pqr-123"
configure.ues_url="https://teracloud/v1/accounts/xyz-234-76085/open-
analytics"
configure.certificate_file="cert.crt"
configure.default_varchar_size=512
configure.val_install_location="VAL_USER"
```

```python
configure.sandbox_container_id="bgf1233csdh123"
configure.read_nos_function_mapping="read_nos_fm"
configure.write_nos_function_mapping="write_nos_fm"
```