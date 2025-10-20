# Executing Python Functions Inside Database Engine 20

This section explains different ways to execute a Python function without pulling the data outside of Database
Engine 20. Consider a scenario where you want to run analytics capabilities on the data residing in Database
Engine 20, that are not already present in teradataml built-in functionality.
teradataml provides functions where you can apply your own logic to process and transform data within
teradataml DataFrame. Use these functions/DataFrame methods to address specific data processing
requirements beyond the built-in functions provided by teradataml.
**Note:**
    * These functions avoid pulling data out of Database Engine 20. Instead, the Python function is
    * pushed to Database Engine 20, eliminating data movement between the client and Database
    * Engine 20.
    * teradataml UDF is a Python user defined function, and it is different from the Teradata UDF which
    * offers support for C++/C/Java UDF functions.
* udf - Function decorator
* UDF functions
    * register()
    * list_udfs()
    * call_udf()
    * deregister()
* DataFrame methods
    * apply  (supported in VantageCloud Lake)
    * map_row and map_partition  (supported in VantageCloud Enterprise)
Use of teradataml UDF versus DataFrame Methods  provides a breakdown differences between teradataml
UDF versus DataFrame methods, and when to use each.
## Use of teradataml UDF versus DataFrame Methods
### Difference between apply, map_row, map_partition, and udf
DataFrame.apply()
DataFrame.map_
row()
DataFrame.
map_partition()
udf()
Executes on
every teradataml
DataFrame row on
VantageCloud Lake.
Executes on every
teradataml DataFrame
row on VantageCloud
Enterprise.
Executes on group of
teradataml DataFrame
rows on VantageCloud
Enterprise.
Executes on every
teradataml DataFrame
row on VantageCloud
Enterprise.
5

DataFrame.apply()
DataFrame.map_
row()
DataFrame.
map_partition()
udf()
Returns
teradataml DataFrame
Returns
teradataml DataFrame
Returns
teradataml DataFrame
Returns teradataml
DataFrame Column
Teradata recommends
having the same Python
interpreter version and
same version of Python
libraries, that are used
inside the function, in the
local client environment
and the server-side
user environment.
Teradata recommends
having the same Python
interpreter version and
same version of Python
libraries, that are used
inside function, in the
local client environment
and VantageCloud
Enterprise.
Teradata recommends
having the same Python
interpreter version and
same version of Python
libraries, that are used
inside function, in the
local client environment
and VantageCloud
Enterprise.
Teradata recommends
having the same Python
interpreter version and
same version of Python
libraries, that are used
inside the function, in the
local client environment
and the server-side
user environment.
Lambda functions
are supported.
Lambda functions
are supported.
Lambda functions
are supported.
Lambda functions are
not supported.
### udf vs apply vs map_row vs map_partition: When to use what in teradataml
UDF/Method
When to use
udf()
• Use UDF for simplicity and ease of use, and use of functions over multiple sessions.
• You want to a run a Python function over every teradataml DataFrame row and return
a single value result for each row.
• You want to return teradataml DataFrame column instead of teradataml DataFrame.
You can directly access each row’s column data by specifying the column name as an
input to the Python function, unlike other function where you must design Python functions
to read the data from the Series object or iterator (TextFileReader object) and manipulate
it accordingly.
(VantageCloud Enterprise and VantageCore) With udf(), Python function can only return a
single values, while DataFrame.map_row() and DataFrame.map_partition() allow Python
functions to either print output to the standard output or return objects such as numpy 1-D
or 2-D arrays, pandas Series, or pandas DataFrames.
DataFrame.
apply()
• Supported on VantageCloud Lake.
• You want to a execute lambda function that returns numpy 1-D or 2-D arrays, pandas
Series, or pandas DataFrames.
• You want to apply a Python function to each teradataml DataFrame row and return the
result as a teradataml DataFrame.
• You want to run the function on the group or partition of data.
DataFrame.
map_row()
• Supported on VantageCloud Enterprise and VantageCore.
• You want to execute a lambda function that returns numpy 1-D or 2-D arrays, pandas
Series, or pandas DataFrames.
• You want to apply a Python function to each teradataml DataFrame row and return the
result as a teradataml DataFrame.
DataFrame.
map_partition()
• Supported on VantageCloud Enterprise and VantageCore.
• You want to execute a lambda function that returns numpy 1-D or 2-D arrays, pandas
Series, or pandas DataFrames.

UDF/Method
When to use
• You want to apply a Python function to a group or partition of rows in the teradataml
DataFrame and return the result as a teradataml DataFrame.
## teradataml UDF
teradataml enables execution of user defined Python functions on Database Engine 20 with simple
interfaces so you can write these functions and mark/register them to be used as the UDF with
teradataml DataFrame.
See  teradataml UDF Prerequisites  to configure Database Engine 20 to use the operator supported by your
database version.
### udf decorator
teradataml provides  udf - Function decorator  as a decorator to mark/register a UDF within a session.
### UDF functions
Use udf decorator to convert any regular Python function to a teradataml UDF, but the scope of the
teradataml UDF is limited to the session. You can work with a UDF to modify it in a session until it is finalized.
Once the UDF is finalized and ready to be used over multiple sessions, use register() and call_udf() to store
the teradataml UDF and execute the stored UDF.
**To work with the stored UDFs, teradataml provides following set of UDF functions:**
* register()  - Register the function in Database Engine 20, and store it as a runnable script.
* list_udfs()  - List all the UDFs registered in Database Engine 20.
* call_udf()  - Execute the registered UDF.
* deregister()  - Delete the registered UDF from Database Engine 20.
### Considerations
* UDF functions must be used with DataFrame.assign() method. See  udf() Examples .
* Lambda function are not supported. Re-write the lambda function as regular Python function to use
* with UDF.
* Do not run UDFs on the column names same as Teradata Reserved Keywords. Rename such column
* names using DataFrame.assign() and then use those columns.
* All Python objects used by the function, must be created/imported within the function.
### teradataml UDF Prerequisites
UDF functions are executed using Script table operator (STO) or Apply table operator (Open
Analytics Framework).
* For STO setup, refer to  Apply SET Operations on teradataml DataFrames .

UDF functions support all Vantage platforms where the appropriate table operator is supported.
### Database Engine 20 supports STO
* Make sure the script table operator setup is completed on the database.
* Make sure your administrator provides the standard permissions required by STO to the
* database <user>.
* Database Engine 20 supports multiple versions of Teradata In-nodes Python packages.
* If v1.0.x is installed, the Python executable is present in  /opt/teradata/languages/Python/ . Tell
* teradataml to invoke the interpreter by specifying the option:
```python
from teradataml import configure
configure.indb_install_location = "/opt/teradata/languages/Python/"
```
* If v.2.0.0 is installed, the Python executable is present in  /var/opt/teradata . teradataml uses this
* by default. If Database Engine 20 raises Python interpreter file not found exception, tell teradataml to
* invoke the interpreter by specifying the option:
```python
from teradataml import configure
configure.indb_install_location = "/var/opt/teradata/languages/
sles12sp3/Python/"
```
### Database Engine 20 supports Apply Table Operator (Open
### Analytics Framework)
* Once you create the environment, configure it to be used by teradataml UDF functions.
```python
from teradataml import configure
configure.openml_user_env = <Object of UserEnv class>
```
* If you don't specify a configuration option, teradataml UDF uses default openml_env, an Open
* Analytics Framework environment for execution. This default environment has the latest Python
* supported by Open Analytics Framework at the time of creating the environment.
* Required modules/packages to run the user defined function must be installed in a remote user
* environment while executing teradataml UDF on VantageCloud Lake.
* While working on date and time data types, you must format these to the supported formats.
### udf - Function decorator
teradataml provides a decorator udf to mark/register a Python function as a UDF to run on each row in
a teradataml DataFrame. The decorated function accepts the arguments, including the column name,
and returns the ColumnExpression (DataFrame Column). You can use the decorated function with
DataFrame.assign() like any other function.

### Input of Python function
The user defined Python function can accept as many arguments as required or no argument. It also
accepts column names corresponding to the DataFrame. Thus, the user function has access to the data
to process in a familiar format .
### Output of Python function
The user defined Python function returns a single value for each row in the DataFrame. This value
corresponds to the output of the function applied to each row’s input
### Function Information
* Using udf Decorator without Arguments
* Using udf Decorator with Arguments
* Examples: How to Use udf()
Using udf Decorator without Arguments
Create a Python function to_upper to convert values in a column to upper case, then convert the function
to UDF using @udf decorator.
### Example setup
```python
load_example_data("dataframe", "sales")
df = DataFrame("sales")
df
Feb    Jan    Mar    Apr    datetime
accounts
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Red Inc     200.0  150.0  140.0    NaN  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
```
### Convert a Python function to udf using @udf as decorator
```python
from teradataml.dataframe.functions import udf
@udf
def to_upper(s):
return s.upper()
```

```python
Assign the Column Expression returned by udf the DataFrame.
res = df.assign(upper_stats = to_upper('accounts'))
res
Feb    Jan    Mar    Apr  datetime upper_stats
accounts
Alpha Co    210.0  200.0  215.0  250.0  17/01/04    ALPHA CO
Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC
Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC
Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC
Orange Inc  210.0    NaN    NaN  250.0  17/01/04  ORANGE INC
Red Inc     200.0  150.0  140.0    NaN  17/01/04     RED INC
```
Using udf Decorator with Arguments
udf() accepts several arguments as input so you can tune a Python function and notify teradataml when
to use the function.
Argument
Required/
Optional
Type
Description
returns
Optional
teradatasqlalchemy
types object
Specifies the output column type.
When not specified, default is VARCHAR(1024).
env_name
Optional
string or object of
class UserEnv
(Applicable for use with Apply table operator)
Specifies the name of the remote user
environment or an object of class UserEnv for
VantageCloud Lake.
delimiter
Optional
One-character string
Specifies a delimiter to use when reading columns
from a row and writing result columns.
Default is comma (,). If data being processed
contains a comma, use a different delimiter.
quotechar
Optional
One-character string
Specifies a character that forces input of
the user function to be quoted using this
specified character.
debug
Optional
bool
Specifies whether to remove the temporary script
generated during execution and display the file
path or not. This argument is useful for debugging
if there are any failures when executing the
function. When set to True, function displays the
path of the script and does not remove the file from
local file system. Otherwise, file is removed from
the local file system.

Examples: How to Use udf()
* udf() Example Setup
* udf() Examples
udf() Example Setup
```python
load_example_data("dataframe", "sales")
df = DataFrame("sales")
df
Feb    Jan    Mar    Apr    datetime
accounts
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Red Inc     200.0  150.0  140.0    NaN  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
```
udf() Examples
### Example 1: Create a UDF to add the data in column 'Jan' with column 'Feb'
### and store result in Integer type column
```python
from teradatasqlalchemy.types import INTEGER
from teradataml.dataframe.functions import udf
@udf(returns=INTEGER())
def sum(x, y):
if not x:
x = 0
return x + y

Assign the Column Expression returned by user defined function
to the DataFrame.
res = df.assign(len_sum = sum('Jan', 'Feb'))
res
Feb    Jan    Mar    Apr  datetime  len_sum
accounts
```

```python
Alpha Co
.0  200.0  215.0  250.0  17/01/04      410
Blue Inc     90.0   50.0   95.0  101.0  17/01/04      140
Yellow Inc   90.0    NaN    NaN    NaN  17/01/04       90
Jones LLC   200.0  150.0  140.0  180.0  17/01/04      350
Orange Inc
.0    NaN    NaN  250.0  17/01/04
Red Inc     200.0  150.0  140.0    NaN  17/01/04      350
```
### Example 2: Create a function to get the values in 'accounts' to upper case
### and pass it to udf() as parameter to UDF
```python
from teradataml.dataframe.functions import udf
def to_upper(s):
if s is not None:
return s.upper()
upper_case = udf(to_upper)

Assign the Column Expression returned by user defined function
to the DataFrame.
res = df.assign(upper_stats = upper_case('accounts'))
res
Feb    Jan    Mar    Apr  datetime upper_stats
accounts
Alpha Co
.0  200.0  215.0  250.0  17/01/04    ALPHA CO
Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC
Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC
Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC
Orange Inc
.0    NaN    NaN  250.0  17/01/04  ORANGE INC
```
### Example 3: Create a UDF to add 4 to the 'datetime' column and store the
### result in DATE type column
While working on date and time data types one must format these to supported formats. See Requisite
Input and Output Structures in Open Analytics Framework for more details.
```python
from teradataml.dataframe.functions import udf
from teradatasqlalchemy.types import DATE
@udf(returns=DATE())
def add_date(x, y):
import datetime
return (datetime.datetime.strptime(x, "%y/%m/%d")
+datetime.timedelta(y)).strftime("%y/%m/%d")

```

```python
Assign the Column Expression returned by user defined function
to the DataFrame.
res = df.assign(new_date = add_date('datetime', 4))
res
Feb    Jan    Mar    Apr  datetime  new_date
accounts
Alpha Co    210.0  200.0  215.0  250.0  17/01/04  17/01/08
Blue Inc     90.0   50.0   95.0  101.0  17/01/04  17/01/08
Jones LLC   200.0  150.0  140.0  180.0  17/01/04  17/01/08
Orange Inc  210.0    NaN    NaN  250.0  17/01/04  17/01/08
Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  17/01/08
Red Inc     200.0  150.0  140.0    NaN  17/01/04  17/01/08
```
### Example 4: Create a user defined function 'to_upper' to get values
### in 'accounts' column to upper case using a UDF that runs on non
### default environment
Create a Python 3.10.5 environment with given name and description in Database Engine 20.
```python
env = create_env('test_udf', 'python_3.10.', 'Test environment for UDF')
User environment 'test_udf' created.

Create a user defined functions to 'to_upper' to get the values in
upper case
and pass the user env to run it on.
from teradataml.dataframe.functions import udf
@udf(env_name = env)
def to_upper(s):
if s is not None:
return s.upper()

Assign the Column Expression returned by user defined function
to the DataFrame.
df.assign(upper_stats = to_upper('accounts'))
Feb    Jan    Mar    Apr  datetime upper_stats
accounts
Alpha Co    210.0  200.0  215.0  250.0  17/01/04    ALPHA CO
Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC
Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC
Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC
```

```python
Orange Inc  210.0    NaN    NaN  250.0  17/01/04  ORANGE INC
Red Inc     200.0  150.0  140.0    NaN  17/01/04     RED INC
```
### Example 5: Create a UDF with required functions inside the UDF itself
Define a function 'inner_add_date' inside the UDF to create a date object by passing year, month, and
day and add 1 to that date. Call this function inside the UDF.
```python
from teradataml.dataframe.functions import udf
@udf
def add_date(y,m,d):
import datetime
def inner_add_date(y,m,d):
return datetime.date(y,m,d) + datetime.timedelta(1)
return inner_add_date(y,m,d)
Assign the Column Expression returned by user defined function
to the DataFrame.
res = df.assign(new_date = add_date(2021, 10, 5))
res
Feb    Jan    Mar    Apr  datetime    new_date
accounts
Jones LLC   200.0  150.0  140.0  180.0  17/01/04  2021-10-06
Blue Inc     90.0   50.0   95.0  101.0  17/01/04  2021-10-06
Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  2021-10-06
Orange Inc  210.0    NaN    NaN  250.0  17/01/04  2021-10-06
Alpha Co    210.0  200.0  215.0  250.0  17/01/04  2021-10-06
Red Inc     200.0  150.0  140.0    NaN  17/01/04  2021-10-06
```
### register()
register() function registers a Python function or teradataml udf and stores it in Database Engine 20 for
future use. This makes the function reusable across sessions, unlike  udf - Function decorator , which is
limited to a single session.
Registered UDFs are stored in Database Engine 20 as a runnable Python script, which is executed using
the Script table operator or Apply table operator. See  teradataml UDF Prerequisites .
When working with Open Analytics Framework, registered UDFs are stored in either the default user
environment or the environment specified by the user in the configuration option.
Registered UDFs are stored in the file system or user environment with the naming convention as
tdml_udf_name_<registered udf name>_udf_type_<return type>_register.py .
Be careful when deleting these files directly.

### Arguments
Argument
Required/
Optional
Type
Description
name
Required
string
Specifies the name of the user defined function
to register.
user_
function
Required
function, teradataml udf
Specifies the user defined function to create a
column for teradataml DataFrame.
returns
Optional
teradatasqlalchemy
types object
Specifies the output column type.
When not specified, default is
VARCHAR(1024).
If 'user_function' is a teradataml udf, then
return type of the udf is used as return type
of the registered UDF.
### list_udfs()
Use the list_udfs() function to list all the UDFs registered using register() function.
### Arguments
Argument
Required/
Optional
Type
Description
show_files
Optional
bool
Specifies whether to show file names or not.
Default value: false
If show_files is set to true, list_udfs() returns a Pandas
DataFrame containing the registered UDFs' name, return type,
and corresponding Python script name.
### call_udf()
call_udf() function lets you execute the registered UDF on each teradataml DataFrame row. call_udf()
returns a teradataml DataFrame column, meaning these functions can be passed to teradataml
DataFrame.assign() like a regular teradataml DataFrame column.
### Arguments
Argument
Required/
Optional
Type
Description
udf_name
Required
string
Specifies the name of the user-defined function.
func_args
Optional
tuple
Specifies the arguments to pass to the registered UDF.
Default is ( ).

Argument
Required/
Optional
Type
Description
delimiter
Optional
One-character
string
Specifies a delimiter to use when reading columns from a
row and writing result columns.
Default is comma (,). If data being processed contains a
comma, specify a different delimiter.
quotechar
Optional
One-character
string
Specifies a character that forces the input of the user
function to be quoted using this specified character.
### deregister()
Use the deregister() function to deregister a UDF.
### Arguments
Argument
Required/
Optional
Type
Description
name
Required
string
Specifies the name of the user defined function
to deregister.
returns
Optional
teradatasqlalchemy
types object
Specifies the type used to deregister the user
defined function.
When not specified, deregister deletes all
UDFs registered with the specified name.
### UDF Functions Examples
The following topics show the example setup and how you can use the teradataml UDF functions.
* Example Setup
* Examples: How to Use teradataml UDF Functions
Example Setup
Load the data and create a DataFrame on 'sales' table.
```python
load_example_data("dataframe", "sales")
import random
dfsales = DataFrame("sales")
df = dfsales.assign(id = case([(df.accounts == 'Alpha Co',
random.randrange(1, 9)),
```

```python
(df.accounts == 'Blue Inc',
random.randrange(1, 9)),
(df.accounts == 'Jones LLC',
random.randrange(1, 9)),
(df.accounts == 'Orange Inc',
random.randrange(1, 9)),
(df.accounts == 'Yellow Inc',
random.randrange(1, 9)),
(df.accounts == 'Red Inc',
random.randrange(1, 9))]))

```
Examples: How to Use teradataml UDF Functions
### Example 1: Register a UDF in Database Engine 20
Register a Python function with name “upper” to get the values upper case.
```python
from teradataml.dataframe.functions import register
def to_upper(s):
return s.upper()

Register the created Python function with name "upper".
register("upper", to_upper)

```
Register a teradataml udf with name “fact” to get factorial of a number and store the result in Integer
type column.
```python
from teradataml.dataframe.functions import udf
from teradatasqlalchemy.types import INTEGER
@udf(returns = INTEGER())
def factorial(n):
import math
return math.factorial(n)

Register the created user defined function with name "fact".
register("fact", factorial)

```

### Example 2: List all UDFs registered in Database Engine 20
```python
>> from teradataml.dataframe.functions import list_udfs
list_udfs(True)
id      name  return_type                                          file_name
0      upper  VARCHAR1024  tdml_udf_name_upper_udf_type_VARCHAR1024_register.py
1       fact      INTEGER       tdml_udf_name_fact_udf_type_INTEGER_register.py
2   add_date         DATE      tdml_udf_name_add_date_udf_type_DATE_register.py

```
### Example 3: Execute a registered UDF
Execute the UDF registered with name 'upper'.
```python
from teradataml.dataframe.functions import call_udf
Call the Python function registered with name "upper" and assign the
ColumnExpression returned to the DataFrame.
res = df.assign(upper_col = call_udf("upper", ('accounts',)))
res
Feb    Jan    Mar    Apr  datetime  id   upper_col
accounts
Yellow Inc   90.0    NaN    NaN    NaN  17/01/04   4  YELLOW INC
Alpha Co    210.0  200.0  215.0  250.0  17/01/04   2    ALPHA CO
Jones LLC   200.0  150.0  140.0  180.0  17/01/04   5   JONES LLC
Red Inc     200.0  150.0  140.0    NaN  17/01/04   3     RED INC
Blue Inc     90.0   50.0   95.0  101.0  17/01/04   1    BLUE INC
Orange Inc  210.0    NaN    NaN  250.0  17/01/04   4  ORANGE INC

```
Execute the UDF registered with name 'fact'.
```python
Call the user defined function registered with name "fact" and assign the
ColumnExpression returned to the DataFrame.
res = df.assign(fact_col = call_udf("fact", ('id',)))
res
Feb    Jan    Mar    Apr  datetime  id  fact_col
accounts
Jones LLC   200.0  150.0  140.0  180.0  17/01/04   5       120
Yellow Inc   90.0    NaN    NaN    NaN  17/01/04   4        24
Red Inc     200.0  150.0  140.0    NaN  17/01/04   3         6
```

```python
Blue Inc     90.0   50.0   95.0  101.0  17/01/04   1         1
Alpha Co    210.0  200.0  215.0  250.0  17/01/04   2         2
Orange Inc  210.0    NaN    NaN  250.0  17/01/04   4        24
```
### Example 4: Delete registered UDFs from Database Engine 20
```python
from teradataml.dataframe.functions import deregister, list_udfs
from teradatasqlalchemy.types import INTEGER
deregister("upper")
deregister("fact", returns=INTEGER()

list_udfs(True)
id      name  return_type                                         file_name
0  add_date         DATE     tdml_udf_name_add_date_udf_type_DATE_register.py
```
## DataFrame Methods
You can use teradataml UDFs to execute a Python function over each row in a teradataml DataFrame and
return the result as a DataFrameColumn.
You can use the following methods to apply that Python function on each row or group of rows in the
teradataml DataFrame and return a teradataml DataFrame.
* DataFrame.apply()  (supported in VantageCloud Lake)
* DataFrame.map_row() and DataFrame.map_partition()  (supported in VantageCloud Enterprise)
### DataFrame.apply()
apply() function applies a Python function on every teradataml DataFrame row by leveraging the
Apply table operator in Open Analytics Framework and returning the result to the user as a
teradataml DataFrame.
apply() supports Python functions including lambda functions.
### Function Information
* DataFrame.apply() Setup and Usage
* DataFrame.apply() Arguments
* Examples: How to use DataFrame.apply()

DataFrame.apply() Setup and Usage
* Set up the environment with required packages using Open Analytics Framework and use it for apply
* execution. Once the environment is created, pass the environment name or object of class UserEnv
* to the apply().
* The function requires dill package with same version in both remote environment and
* local environment.
* Teradata recommends using the same Python version in both the remote and local environments.
* Teradata recommends using the same version of Python libraries between client machine and
* Database Engine 20 machine.
### Input of Python function
The Python function can accept as many arguments as required, but must accept pandas Series object
as its first (positional) argument corresponding to a row in the DataFrame. Thus, the user function has
access to the data to process in a familiar format. Design the function to read from the Series object and
manipulate the data accordingly.
### Output of Python function
The Python function can either print the output to the standard output or return objects of any of the
**supported types so that they are printed to the standard output correctly, which include:**
* pandas DataFrame having the same number of columns as expected in the output.
* pandas Series representing a row in the output of the method and having the same number of
* columns as the expected in the output.
* numpy ndarray
    * One-dimensional: represents a row in the output, having the same number of columns as
    * expected in the output.
    * Two-dimensional: represents a dataset (like a pandas DataFrame) having the same number of
    * columns as expected in the output.
The object returned by the user function is printed to the standard output as delimited lines (rows), using
the specified delimiter and quotechar.
If the user function prints the output directly to the standard output (instead of returning an object of the
supported type), then it must take care of using the delimiter and quotechar, if and when specified, to
format the output printed.
Data in the standard output is stored in a table and the table is garbage collected at the end of the session.

DataFrame.apply() Arguments
Argument
Required/
Optional
Type
Description
user_function
Required
function or
functools.partial
Specifies the user-defined function to apply
to each row in the teradataml DataFrame.
env_name
Required
string or object of
class UserEnv
Specifies the name of the remote user
environment or an object of class UserEnv.
exec_mode
Optional
string
Specifies the mode of execution for the user-
defined function.
chunk_size
Optional
integer
Specifies the number of rows to be read
in each chunk, using an iterator to apply
the user-defined function to each row in
the chunk.
returns
Optional
Dictionary specifying
column name to
teradatasqlalchemy
type mapping
Specifies the output column definition
corresponding to the output of  user_function .
delimiter
Optional
One-character string
Specifies a delimiter to use when
reading columns from a row and writing
result columns.
quotechar
Optional
One-character string
Specifies a character that forces all input and
output of the user function to be quoted using
this specified character.
data_
partition_
column
Optional
string OR list of strings
Specifies the Partition By columns for the
teradataml DataFrame.
data_hash_
column
Optional
string
Specifies the column to be used for hashing.
data_order_
column
Optional
string OR list of strings
Specifies the Order By columns for the
teradataml DataFrame.
is_local_
order
Optional
bool
Specifies a boolean value to determine
whether the input data is to be ordered locally
or not.
nulls_first
Optional
bool
Specifies a boolean value to determine
whether NULLs are listed first or last
during ordering.
sort_
ascending
Optional
bool
Specifies a boolean value to determine if the
result set is to be sorted on the  data_order_
column  in ascending or descending order.
style
Optional
bool
Specifies how input is passed to and output
is generated.

Argument
Required/
Optional
Type
Description
debug
Optional
bool
Specifies whether to remove the temporary
script generated during execution and
display the file path or not. This argument is
useful for debugging if there are any failures
when executing the function. When set to
True, function displays the path of the script
and does not remove the file from local file
system. Otherwise, file is removed from the
local file system.
Examples: How to use DataFrame.apply()
### Example setup
Create a Python 3.10 environment with given name and description in Database Engine 20. This example
uses the 'admissions_train' dataset, to increase the 'gpa' by a give percentage.
```python
env = create_env('testenv', 'python_3.10', 'Test environment')
User environment testenv created.
Packages dill and pandas must be installed in remote user environment.
env.install_lib(['pandas','dill'])
Request to install libraries initiated successfully in the remote user
environment demo_env. Check the status using status() with the claim
id 'ef255030-1be2-4d4a-9d47-12cd4365a003'.
Check the status of installation.
env.status('ef255030-1be2-4d4a-9d47-12cd4365a003')
Claim Id     File/Libs  Method Name
Stage             Timestamp Additional Details
0  ef255030-1be2-4d4a-9d47-12cd4365a003  pandas, dill  install_lib
Started  2022-08-04T04:27:56Z
1  ef255030-1be2-4d4
a-9d47-12cd4365a003  pandas, dill  install_lib  Finished  2022-08-04T04:29:12Z

Load the example data.
load_example_data("dataframe", "admissions_train")
df = DataFrame('admissions_train')
print(df)
```

```python
masters   gpa     stats programming  admitted
id
22     yes  3.46    Novice    Beginner         0
36      no  3.00  Advanced      Novice         0
15     yes  4.00  Advanced    Advanced         1
38     yes  2.65  Advanced    Beginner         1
5       no  3.44    Novice      Novice         0
17      no  3.83  Advanced    Advanced         1
34     yes  3.85  Advanced    Beginner         0
13      no  4.00  Advanced      Novice         1
26     yes  3.57  Advanced    Advanced         1
19     yes  1.98  Advanced    Advanced         0
```
### Example 1: Create the user defined function to increase the 'gpa' by the #
### percentage provided
Note that the input to and the output from the function is a Pandas Series object.
```python
def increase_gpa(row, p=20):
row['gpa'] = row['gpa'] + row['gpa'] * p/100
return row

Apply the user defined function to the DataFrame.
Note that since the output of the user defined function
expects the same columns with the same types, we can skip
passing the 'returns' argument.
increase_gpa_20 = df.apply(increase_gpa, env_name='testenv')

Print the result.
print(increase_gpa_20)
masters    gpa     stats programming  admitted
id
22     yes  4.152    Novice    Beginner         0
36      no  3.600  Advanced      Novice         0
15     yes  4.800  Advanced    Advanced         1
38     yes  3.180  Advanced    Beginner         1
5       no  4.128    Novice      Novice         0
17      no  4.596  Advanced    Advanced         1
34     yes  4.620  Advanced    Beginner         0
13      no  4.800  Advanced      Novice         1
```

```python
26     yes  4.284  Advanced    Advanced         1
19     yes  2.376  Advanced    Advanced         0
```
### Example 2: Use the same user defined function with a lambda notation to
### pass the percentage, 'p = 40'
```python
increase_gpa_40 = df.apply(lambda row: increase_gpa(row,
p = 40),
env_name='testenv')

print(increase_gpa_40)
masters    gpa     stats programming  admitted
id
22     yes  4.844    Novice    Beginner         0
36      no  4.200  Advanced      Novice         0
15     yes  5.600  Advanced    Advanced         1
38     yes  3.710  Advanced    Beginner         1
5       no  4.816    Novice      Novice         0
17      no  5.362  Advanced    Advanced         1
34     yes  5.390  Advanced    Beginner         0
13      no  5.600  Advanced      Novice         1
26     yes  4.998  Advanced    Advanced         1
19     yes  2.772  Advanced    Advanced         0
```
### Example 3: Use the same user defined function with functools.partial to pass
### the percentage, 'p = 50'
```python
from functools import partial
increase_gpa_50 = df.apply(partial(increase_gpa, p = 50),
env_name='testenv')

print(increase_gpa_50)
masters    gpa     stats programming  admitted
id
13      no  6.000  Advanced      Novice         1
26     yes  5.355  Advanced    Advanced         1
5       no  5.160    Novice      Novice         0
19     yes  2.970  Advanced    Advanced         0
15     yes  6.000  Advanced    Advanced         1
40     yes  5.925    Novice    Beginner         0
```

```python
7      yes  3.495    Novice      Novice         1
22     yes  5.190    Novice    Beginner         0
36      no  4.500  Advanced      Novice         0
38     yes  3.975  Advanced    Beginner         1
```
### Example 4: Use a lambda function to double 'gpa', and # return
### numpy ndarray
```python
from numpy import asarray
inc_gpa_lambda = lambda row, p=20: asarray([row['id'],
row['masters'],
row['gpa'] + row['gpa'] * p/100,
row['stats'],
row['programming'],
row['admitted']])
increase_gpa_100 = df.apply(lambda row: inc_gpa_lambda(row,
p=100),
env_name='testenv')

print(increase_gpa_100)
masters   gpa     stats programming  admitted
id
13      no  8.00  Advanced      Novice         1
26     yes  7.14  Advanced    Advanced         1
5       no  6.88    Novice      Novice         0
19     yes  3.96  Advanced    Advanced         0
15     yes  8.00  Advanced    Advanced         1
40     yes  7.90    Novice    Beginner         0
7      yes  4.66    Novice      Novice         1
22     yes  6.92    Novice    Beginner         0
36      no  6.00  Advanced      Novice         0
38     yes  5.30  Advanced    Beginner         1
```
### DataFrame.map_row() and DataFrame.map_partition()
DataFrame.map_row() and DataFrame.map_partition() methods provide functionality to apply a Python
function to each row or group of rows in the teradataml DataFrame, and return the result as a
teradataml DataFrame.
These functions support Python functions including lambda functions.

### Function Information
* Setup and Usage
* DataFrame.map_row
* DataFrame.map_partition
Setup and Usage
### Setup
* Teradata recommends using the same Python version in both the database server and the
* environment where teradataml runs.
* The function requires dill package with same version in both the database server and the
* local environment.
* Teradata recommends using similar versions of Python libraries between the client machine and
* Database Engine 20 machine.
* The function being applied to the row or set of rows using map_row() or map_partition() must be
* defined in the current Python session. Any modules/packages being used by it must be available to
* use with Script Table Operator on the database servers. If the function is being imported from some
* package or module, that too must be available on the database server.
* Teradata recommends filling/replacing empty values in character columns with a known placeholder
* value for better readability with map_row() and map_parition() and avoid confusing NULL values with
* empty string
### Execute Mode
Specifies the mode of execution for the user defined function.
* IN-DB: Execute the function on data in the teradataml DataFrame in Database Engine 20 and returns
* a teradataml DataFrame. (Default execution mode)
* LOCAL: Execute the function locally on sample data from the teradataml DataFrame and returns a
* Pandas DataFrame.
### Input of Python Function
The user function can accept as many arguments as required, but must accept one of the following as its
**first (positional) argument:**
* pandas Series object corresponding to a row in the DataFrame when the method called is map_row()
* iterator (TextFileReader object) to read data from the partition of rows in chunks (pandas
* DataFrames) when the method called is map_partition()
As a result, the user function has access to the data to process in a familiar format. Design the functions
to read from the Series object or iterator and manipulate the data accordingly.

### Output of Python Function
The Python function can either print the output to the standard output (just like STO) or return objects of
**any of the supported types so that they are printed to the standard output correctly, which include:**
* pandas DataFrame having the same number of columns as expected in the output.
* pandas Series representing a row in the output of the method and having the same number of
* columns as the expected in the output.
* numpy ndarray
    * One-dimensional: represents a row in the output, having the same number of columns as
    * expected in the output.
    * Two-dimensional: represents a dataset (like a pandas DataFrame) having the same number of
    * columns as expected in the output.
The object returned by the user function is printed to the standard output as delimited lines (rows), using
the specified delimiter and quotechar.
If the user function prints the output directly to the standard output (instead of returning an object of the
supported type), then it must take care of using the delimiter and quotechar, if and when specified, to
format the output printed.
This data printed to the standard output then gets converted to and saved in a table in Database
Engine 20.
The table is deleted as a part of garbage collection as soon as a remove_context() call is issued. To persist
these results, the DataFrame.to_sql() method can be used.
DataFrame.map_row
DataFrame.map_row() method provides functionality to apply a Python function to each row in the
teradataml DataFrame, and return the result as a teradataml DataFrame.
This function supports Python functions including lambda functions.
* DataFrame.map_row() Arguments
* Examples: How to use DataFrame.map_row()
DataFrame.map_row() Arguments
Argument
Required/
Optional
Type
Description
user_
function
Required
function or
functools.partial
Specifies the user-defined function to apply to
each row in the teradataml DataFrame.
exec_mode
Optional
string
Specifies the mode of execution for the user-
defined function.

Argument
Required/
Optional
Type
Description
chunk_size
Optional
integer
Specifies the number of rows to be read
in each chunk, using an iterator to apply
the user-defined function to each row in
the chunk.
num_rows
Optional
integer
Specifies the maximum number of sample
rows from the teradataml DataFrame to apply
the user-defined function when  exec_mode
is 'LOCAL'.
returns
Optional
Dictionary specifying
column name to
teradatasqlalchemy
type mapping
Specifies the output column definition
corresponding to the output of  user_function .
delimiter
Optional
One-character string
Specifies a delimiter to use when
reading columns from a row and writing
result columns.
quotechar
Optional
One-character string
Specifies a character that forces all input and
output of the user function to be quoted using
this specified character.
auth
Optional
string
Specifies the authorization to use when
running the  user_function .
charset
Optional
string
Specifies the character encoding for data.
data_order_
column
Optional
string OR list of strings
Specifies the Order By columns for the
teradataml DataFrame.
is_local_
order
Optional
bool
Specifies a boolean value to determine
whether the input data is to be ordered locally
or not.
nulls_first
Optional
bool
Specifies a boolean value to determine
whether NULLs are listed first or last
during ordering.
sort_
ascending
Optional
bool
Specifies a boolean value to determine
if the result set is to be sorted on the
data_order_column  column in ascending or
descending order.
debug
Optional
bool
Specifies whether to remove the temporary
script generated during execution and display
the file path or not. This argument is useful
for debugging if there are any failures when
executing the function. When set to True,
function displays the path of the script and
does not remove the file from local file system.
Otherwise, file is removed from the local
file system.

Examples: How to use DataFrame.map_row()
### Example setup
```python
This example uses the 'admissions_train' dataset.
Load the example data.
load_example_data("dataframe", "admissions_train")
df = DataFrame('admissions_train')
print(df)
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
### Example 1: Create a user defined function to increase the 'gpa' by the
### percentage provided
**Note:**
Input to and output from the function is a Pandas Series object.
```python
def increase_gpa(row, p=20):
row['gpa'] = row['gpa'] + row['gpa'] * p/100
return row
Apply the user defined function to the DataFrame.
Note that since the output of the user defined function expects the
same columns
with the same types, we can skip passing the 'returns' argument.
increase_gpa_20 = df.map_row(increase_gpa)
Print the result.
print(increase_gpa_20)
```

```python
masters    gpa     stats programming  admitted
id
13      no  4.800  Advanced      Novice         1
36      no  3.600  Advanced      Novice         0
15     yes  4.800  Advanced    Advanced         1
40     yes  4.740    Novice    Beginner         0
22     yes  4.152    Novice    Beginner         0
38     yes  3.180  Advanced    Beginner         1
26     yes  4.284  Advanced    Advanced         1
5       no  4.128    Novice      Novice         0
7      yes  2.796    Novice      Novice         1
19     yes  2.376  Advanced    Advanced         0
```
### Example 2: Use the same user defined function with a lambda notation to
### pass the percentage 'p = 40'
```python
increase_gpa_40 = df.map_row(lambda row: increase_gpa(row, p = 40))
print(increase_gpa_40)
masters    gpa     stats programming  admitted
id
5       no  4.816    Novice      Novice         0
34     yes  5.390  Advanced    Beginner         0
13      no  5.600  Advanced      Novice         1
40     yes  5.530    Novice    Beginner         0
22     yes  4.844    Novice    Beginner         0
19     yes  2.772  Advanced    Advanced         0
36      no  4.200  Advanced      Novice         0
15     yes  5.600  Advanced    Advanced         1
7      yes  3.262    Novice      Novice         1
17      no  5.362  Advanced    Advanced         1
```
### Example 3: Use the same user defined function with functools.partial to
### pass the percentage 'p = 50'
```python
from functools import partial
increase_gpa_50 = df.map_row(partial(increase_gpa, p = 50))
print(increase_gpa_50)
masters    gpa     stats programming  admitted
id
5       no  5.160    Novice      Novice         0
```

```python
34     yes  5.775  Advanced    Beginner         0
13      no  6.000  Advanced      Novice         1
40     yes  5.925    Novice    Beginner         0
22     yes  5.190    Novice    Beginner         0
19     yes  2.970  Advanced    Advanced         0
36      no  4.500  Advanced      Novice         0
15     yes  6.000  Advanced    Advanced         1
7      yes  3.495    Novice      Novice         1
17      no  5.745  Advanced    Advanced         1
```
### Example 4: Use a lambda function to increase the 'gpa' by 50 percent, and
### return numpy ndarray
```python
from numpy import asarray
increase_gpa_lambda = lambda row, p=20: asarray([row['id'],
row['masters'], row['gpa'] + row['gpa'] * p/100,
row['stats'],
row['programming'], row['admitted']])
increase_gpa_100 = df.map_row(lambda row: increase_gpa_lambda(row, p=100))
print(increase_gpa_100)
masters   gpa     stats programming  admitted
id
5       no  6.88    Novice      Novice         0
34     yes  7.70  Advanced    Beginner         0
13      no  8.00  Advanced      Novice         1
40     yes  7.90    Novice    Beginner         0
22     yes  6.92    Novice    Beginner         0
19     yes  3.96  Advanced    Advanced         0
36      no  6.00  Advanced      Novice         0
15     yes  8.00  Advanced    Advanced         1
7      yes  4.66    Novice      Novice         1
17      no  7.66  Advanced    Advanced         1
```
DataFrame.map_partition
DataFrame.map_partition() function applies a Python function to a group or partition of rows in the
teradataml DataFrame by leveraging the STO and returning the result as a teradataml DataFrame.
* DataFrame.map_partition() Arguments
* Examples: How to use DataFrame.map_partition()
* Example Workflow

DataFrame.map_partition() Arguments
Argument
Required/
Optional
Type
Description
user_function
Required
function or
functools.partial
Specifies the user-defined function to apply
to each group in the teradataml DataFrame.
exec_mode
Optional
string
Specifies the mode of execution for the user-
defined function.
chunk_size
Optional
integer
Specifies the number of rows to be read
in each chunk, using an iterator to apply
the user-defined function to each row in
the chunk.
num_rows
Optional
integer
Specifies the maximum number of sample
rows from the teradataml DataFrame to
apply the user-defined function when  exec_
mode  is 'LOCAL'.
data_
partition_
column
Optional
string OR list of strings
Specifies the Partition By columns for the
teradataml DataFrame.
data_hash_
column
Optional
string
Specifies the column to be used for hashing.
returns
Optional
Dictionary specifying
column name to
teradatasqlalchemy
type mapping
Specifies the output column definition
corresponding to the output of  user_function .
delimiter
Optional
One-character string
Specifies a delimiter to use when
reading columns from a row and writing
result columns.
quotechar
Optional
One-character string
Specifies a character that forces all input and
output of the user function to be quoted using
this specified character.
auth
Optional
string
Specifies the authorization to use when
running the  user_function .
charset
Optional
string
Specifies the character encoding for data.
data_order_
column
Optional
string OR list of strings
Specifies the Order By columns for the
teradataml DataFrame.
is_local_
order
Optional
bool
Specifies a boolean value to determine
whether the input data is to be ordered locally
or not.
nulls_first
Optional
bool
Specifies a boolean value to determine
whether NULLs are listed first or last
during ordering.

Argument
Required/
Optional
Type
Description
sort_
ascending
Optional
bool
Specifies a boolean value to determine
if the result set is to be sorted on the
data_order_column  column in ascending or
descending order.
debug
Optional
bool
Specifies whether to remove the temporary
script generated during execution and
display the file path or not. This argument is
useful for debugging if there are any failures
when executing the function. When set to
True, function displays the path of the script
and does not remove the file from local file
system. Otherwise, file is removed from the
local file system.
Examples: How to use DataFrame.map_partition()
### Example setup
```python
This example uses the 'admissions_train' dataset.
Load the example data.
load_example_data("dataframe", "admissions_train")
df = DataFrame('admissions_train')
print(df)
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

### Example 1: Create a user defined function to calculate the average 'gpa', by
### reading data in chunks
**Note:**
The function accepts a TextFileReader object to iterate on data in chunks. The return type of the
function is a numpy ndarray.
```python
from numpy import asarray
def grouped_gpa_avg_iter(rows):
admitted = None
row_count = 0
gpa = 0
for chunk in rows:
for _, row in chunk.iterrows():
row_count += 1
gpa += row['gpa']
if admitted is None:
admitted = row['admitted']
if row_count > 0:
return asarray([admitted, gpa/row_count])
Apply the user defined function to the DataFrame.
from teradatasqlalchemy.types import INTEGER, FLOAT
avg_gpa_by_admitted = df.map_partition(grouped_gpa_avg_iter,
returns =
OrderedDict([('admitted', INTEGER()),
('avg_gpa', FLOAT())]),
data_partition_column = 'admitted')
Print the result.
print(avg_gpa_by_admitted)
avg_gpa
admitted
1         3.533462
0         3.557143
```

### Example 2: Create the user defined function to calculate the average 'gpa'
### by reading data into a Pandas DataFrame
**Note:**
The function accepts a TextFileReader object to iterate on data in chunks. The return type of the
function is a Pandas Series.
```python
def grouped_gpa_avg(rows):
pdf = rows.read()
if pdf.shape[0] > 0:
return pdf[['admitted', 'gpa']].mean()
Apply the user defined function to the DataFrame.
avg_gpa_pdf = df.map_partition(grouped_gpa_avg,
returns = OrderedDict([('admitted',
INTEGER()),('avg_gpa', FLOAT())]),
data_partition_column = 'admitted')
Print the result.
print(avg_gpa_pdf)
avg_gpa
admitted
1         3.533462
0         3.557143
```
### Example 3: Create a lambda function to calculate the average 'gpa' by
### reading data into a Pandas DataFrame
**Note:**
The function is written to accept an iterator (TextFileReader object) and return the result which is
of type Pandas Series.
```python
avg_gpa_pdf_lambda = df.map_partition(lambda rows: grouped_gpa_avg(rows),
returns =
OrderedDict([('admitted', INTEGER()),
('avg_gpa', FLOAT())]),
data_partition_column = 'admitted')
print(avg_gpa_pdf_lambda)
```

```python
avg_gpa
admitted
0         3.557143
1         3.533462
```
### Example 4: Using a function that returns input data
**Note:**
The function is written to accept an iterator (TextFileReader object) and returns the result which is
of type Pandas DataFrame.
```python
def echo(rows):
pdf = rows.read()
if pdf is not None:
return pdf
echo_out = df.map_partition(echo, data_partition_column = 'admitted')
print(echo_out)
masters   gpa     stats programming  admitted
id
15     yes  4.00  Advanced    Advanced         1
7      yes  2.33    Novice      Novice         1
22     yes  3.46    Novice    Beginner         0
17      no  3.83  Advanced    Advanced         1
13      no  4.00  Advanced      Novice         1
38     yes  2.65  Advanced    Beginner         1
26     yes  3.57  Advanced    Advanced         1
5       no  3.44    Novice      Novice         0
34     yes  3.85  Advanced    Beginner         0
40     yes  3.95    Novice    Beginner         0
```
Example Workflow
The following workflow demonstrates using GLM model fitting and scoring functions from statsmodels and
sklearn Python packages.
The example fits one or multiple GLM model to the housing data, and then uses it to prices for the houses
in the test data.
* Example setup
* Model Fitting Function

* Scoring Function
* Model Generated on Client and Scored in Database Engine 20
Example setup
```python
load_example_data("GLMPredict", ["housing_test","housing_train"])
Create the input DataFrames
train = DataFrame('housing_train')
test = DataFrame('housing_test')
```
Model Fitting Function
Define the user function that will try to fit multiple models to the partitions in housing train dataset using
the statsmodels functions.
### Define the function that we want to use to fit multiple GLM models, one for
### each home style.
```python
def glm_fit(rows):
"""
DESCRIPTION:
Function that accepts a iterator on a pandas DataFrame
(TextFileObject) created using
'chunk_size' with pandas.read_csv(), and fits a GLM model to it.
The underlying data is the housing data with 12 independent
variable (inluding the home style)
and one dependent variable (price).
RETURNS:
A numpy.ndarray object with two elements:
* The homestyle value (type: str)
* The GLM model that was fit to the corresponding data, which is
serialized using pickle
and base64 encoded. We use decode() to make sure it is of type
str, and not bytes.
"""
Read the entire partition/group of rows in a pandas DataFrame - pdf.
data = rows.read()
Add the 'intercept' column along with the features.
data['intercept'] = 1.0
```

```python
We will not process the partition if there are no rows here.
if data.shape[0] > 0:
Fit the model using R-style formula to specify categorical
variables as well.
We use 'disp=0' to prevent sterr output.
model = smf.glm('price ~ C(recroom) + lotsize + stories + garagepl
+ C(gashw) +'
' bedrooms + C(driveway) + C(airco) + C(homestyle)
+ bathrms +'
' C(fullbase) + C(prefarea)',
family=sm.families.Gaussian(), data=data).fit(disp=0)
We serialize and base64 encode the model in prepration to
output it.
modelSer = b64encode(dumps(model))
The user function can either return a value of supported type
(numpy array, Pandas Series, or Pandas DataFrame),
or just print it to find it's way to the output.
Here we return it as a numpy ndarray object.
Note that we use decode for the serialized model so that it is
represented in the ascii form (which is what base64
encoding does),
instead of bytes.
return asarray([data.loc[0]['homestyle'], modelSer.decode('ascii')])
```
### Use the function defined to fit the model on group of housing data where
### grouping is done by  homestyle
Apply the glm_fit() function defined in the previous section to create a model for every homestyle in the
training dataset. Specify the output column names and their types with the  returns  argument since the
output is not the similar to the input.
```python
model = train.map_partition(glm_fit, data_partition_column='homestyle',
returns=OrderedDict([('homestyle', train.homestyle.type),
('model', CLOB())]))
The model table has been created successfully.
print(model.head())
model
homestyle
```

```python
Eclectic        gANjc3RhdHNtb2RlbHMuZ2VubW9kLmdlbmVyYWxpemVkX2...
Classic         gANjc3RhdHNtb2RlbHMuZ2VubW9kLmdlbmVyYWxpemVkX2...
bungalow
gANjc3RhdHNtb2RlbHMuZ2VubW9kLmdlbmVyYWxpemVkX2...
```
Scoring Function
This scenario uses window functions to assign row numbers to each subset of data corresponding to a
particular homestyle. The intent is to extend the table to add the model corresponding to the homestyle
as the last column value for the first row in the partition. This makes it easier for the scoring function to
read the model and then score the input records based on it.
```python
Create row number column ('row_id') in the 'test' DataFrame.
test_with_row_num = test.assign(row_id =
func.row_number().over(partition_by=test.homestyle.expression,
order_by=test.sn.expression.desc()))
Join it with the model we created based on the value of homestyle.
temp = test_with_row_num.join(model, on = [(test_with_row_num.homestyle ==
model.homestyle)], rsuffix='r', lsuffix='l')
Set the model column to NULL when row_id is not 1.
temp = temp.assign(modeldata = case([(temp.row_id == 1,
literal_column(temp.model.name))], else_ = None))
Drop the extraneous columns created in the processing.
temp = temp.assign(homestyle = temp.l_homestyle).drop('l_homestyle',
axis=1).drop('r_homestyle',axis=1).drop('model', axis=1)
Reorder the columns to have the housing data columns positioned first,
followed by the row_id and modeldata.
new_test = temp.select(test.columns + ['row_id', 'modeldata'])
```
### Define the user function that will score test data to predict prices based
### on features
```python
DELIMITER = '\t'
QUOTECHAR = None
def glm_score(rows):
"""
DESCRIPTION:
Function that accepts a iterator on a pandas DataFrame
(TextFileObject) created using
```

```python
'chunk_size' with pandas.read_csv(), and scores it based on the
model found in the data.
The underlying data is the housing data with 12 independent
variable (inluding the home style)
and one dependent variable (price).
The function chooses to output the values itself, rather than
returning objects of supported type.
RETURNS:
None.
"""
model = None
for chunk in rows:
We process data only if there is any, i.e. only when the chunk
read has any rows.
if chunk.shape[0] > 0:
if model is None:
We read the model once (it is found only once)
per partition.
model = loads(b64decode(chunk.loc[0].iloc[-1]))
Exclude the row_id and modeldata columns from the scoring
dataset as they are not longer required.
chunk = chunk.iloc[:,:-2]
For prediction, exclude the first two column ('sn' - not
relevant, and 'price' - the dependent variable).
prediction = model.predict(chunk.iloc[:,2:])
We now concat the chunk with the prediction column (Pandas
Series) to form a DataFrame.
outdf = concat([chunk,
prediction], axis=1)
We just cannot return this DataFrame yet as we have more
chunks to process.
In such scenarios, we can either:
1. print the output here, or
2. keep concatenating the results of each chunk to create
a final resultant Pandas DataFrame to return.
We are opting for option1 here.
for _, row in outdf.iterrows():
```

```python
if QUOTECHAR is not None:
A NULL value should not be enclosed in quotes.
The CSV module has no support for such output with
writer, and hence the custom formatting.
values = ['' if isna(s) else "{}{}
{}".format(QUOTECHAR, str(s), QUOTECHAR) for s in row]
else:
values = ['' if isna(s) else str(s) for s in row]
print(DELIMITER.join(values), file=sys.stdout)
```
### Perform the actual scoring by calling the map_partition() method on the
### test data
```python
Note that here the output of the function is going to have one more
column than the input,
and we must specify the same.
returns = OrderedDict([(col.name, col.type) for col in test._metaexpr.c] +
[('prediction', FLOAT())])
Note that we are using the 'data_order_column argument' here to order by
the 'row_id'
column so that the model is read before any data that need to be scored.
prediction = new_test.map_partition(glm_score,
returns=returns,
data_partition_column='homestyle',
data_order_column='row_id')
Print the scoring result.
print(prediction.head())
price
lotsize bedrooms
bathrms stories driveway
recroom fullbase
gashw
airco
garagepl
prefarea
homestyle
prediction
sn
469
55000.0  2176.0
2
1
2
yes
yes
no
no
no
0
yes
Eclectic
64597.746106
301
55000.0  4080.0
2
1
1
yes
no
no
no
no
0
no
Eclectic
54979.762152
463
49000.0  2610.0
3
1
2
yes
no
yes
no
no
0
yes
Classic
46515.461314
177
70000.0  5400.0
4
1
2
yes
no
no
no
no
0
no
Eclectic
63607.229642
38
67000.0  5170.0
3
1
4
yes
no
no
no
yes
0
no
Eclectic
78029.766193
13
27000.0  1700.0
3
1
2
yes
no
no
no
no
0
no
Classic
39588.073581
255
61000.0  4360.0
4
1
2
yes
no
no
no
no
0
no
Eclectic
61320.393435
53
68000.0  9166.0
2
1
1
yes
no
yes
no
yes
2
no
Eclectic
76977.937496
364
72000.0 10700.0
3
1
2
yes
yes
yes
no
no
0
no
Eclectic
80761.658291
459
44555.0
8.0
3
1
1
yes
no
no
no
no
0
yes
Classic
42921.671929
```

Model Generated on Client and Scored in Database Engine 20
Fit the model on the entire training housing data using the sklearn package on the client machine.
```python
We will see how a model generated locally can be used to score data on
Vantage. We first create a sklearn GLM model and serialize and base64-encode
it just like we did earlier.
import pandas as pd
import teradataml
import os
Read the housing_train.csv file (shipped with the teradataml package)
into a pandas DataFrame.
with open(os.path.join(os.path.dirname(teradataml.__file__), "data",
"housing_train.csv"), 'r') as f:
housing_train = pd.read_csv(f)
Let's encode the categorical columns.
replace_dict = {'driveway': {'yes': 1, 'no': 0}, 'recroom': {'yes': 1,
'no': 0},
'fullbase': {'yes': 1, 'no': 0}, 'gashw': {'yes': 1, 'no':
0}, 'airco': {'yes': 1, 'no': 0},
'prefarea': {'yes': 1, 'no': 0}, 'homestyle': {'Classic':
1, 'Eclectic': 2, 'bungalow': 3}}
Replace the values inplace.
housing_train.replace(replace_dict, inplace=True)
Fit the GLM model.
model = LogisticRegression(max_iter=5000, solver='lbfgs',
multi_class='auto').fit(housing_train.iloc[:,2:], housing_train.price)
Serialize and base64-encode the GLM model.
modelSer = b64encode(dumps(model)).decode('ascii')
```
### Define a user function which will accept the model and dataset to use it with
### for scoring
```python
The function used for scoring with map_partition()
def glm_score_local_model(rows, model):
"""
DESCRIPTION:
Function that accepts a iterator on a pandas DataFrame
```

```python
(TextFileObject) created using
'chunk_size' with pandas.read_csv(), and scores it based on the model
passed to the function as
the second argument.
The underlying data is the housing data with 12 independent variable
(inluding the home style)
and one dependent variable (price).
The function concatenates the result of all chunk scoring operation
into a final pandas DataFrame to return.
RETURNS:
pandas DataFrame.
"""
Decode and deserialize the model.
model = loads(b64decode(model))
result_df = None
for chunk in rows:
We process data only if there is any, i.e. only when the chunk
read has any rows.
if chunk.shape[0] > 0:
Perform the encoding for the categorical columns.
chunk.replace(replace_dict, inplace=True)
For prediction, exclude the first two column ('sn' - not
relevant, and 'price' - the dependent variable).
prediction = pd.Series(model.predict(chunk.iloc[:,2:]))
We now concat the chunk with the prediction column (Pandas
Series) to form a DataFrame.
outdf = concat([chunk, prediction], axis=1)
We just cannot return this DataFrame yet as we have more chunks
to process.
In such scenarios, we can either:
1. print the output here, or
2. keep concatenating the results of each chunk to create a
final resultant Pandas DataFrame to return.
We are opting for option2 here.
if result_df is None:
result_df = outdf
else:
```

```python
result_df = concat([result_df, outdf], axis=0)
Return the result pandas DataFrame.
return result_df
```
### Call the map_partition() method for the test data to score the data and
### predict prices
```python
Note that here the output of the function is going to have one more
column than the input,
and we must specify the same.
Note that we are using the 'data_order_column argument' here to order by
the 'row_id'
column so that the model is read before any data that need to be scored.
prediction = test.map_partition(lambda rows:
glm_score_local_model(rows, modelSer),
returns=returns,
data_partition_column='homestyle')
Print the scoring result.
print(prediction.head())
price  lotsize  bedrooms  bathrms  stories driveway recroom fullbase gashw airco  garagepl prefarea
homestyle  prediction
sn
25   42000.0   4960.0         2        1        1        1       0        0     0     0         0
0         1     50000.0
53   68000.0   9166.0         2        1        1        1       0        1     0     1         2
0         2     70000.0
111  43000.0   5076.0         3        1        1        0       0        0     0     0         0
0         1     50000.0
117  93000.0   3760.0         3        1        2        1       0        0     1     0         2
0         2     62000.0
140  43000.0   3750.0         3        1        2        1       0        0     0     0         0
0         1     50000.0
142  40000.0   2650.0         3        1        2        1       0        1     0     0         1
0         1     48000.0
157  60000.0   2953.0         3        1        2        1       0        1     0     1         0
0         2     52000.0
161  63900.0   3162.0         3        1        2        1       0        0     0     1         1
0         2     52000.0
176  57500.0   3630.0         3        2        2        1       0        0     1     0         2
0         2     60000.0
177  70000.0   5400.0         4        1        2        1       0        0     0     0         0
0         2     60000.0
```