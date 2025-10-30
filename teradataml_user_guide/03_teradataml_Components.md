# teradataml Components

* Helper Functions
* Garbage Collection in teradataml
* Run SQL Queries using teradataml
## Helper Functions
* load_example_data() Function
### load_example_data() Function
The load_example_data() is a helper function that loads the sample datasets.
Teradata Package for Python offers various APIs and each API provides some examples. To test these
examples, users need the sample datasets loaded in Vantage.
The load_example_data() function can only be used in a restricted way. Function arguments only accept
predetermined values as shown in the given examples in this User Guide and the Teradata Package for
Python Function Reference.
* function_name
  This required argument contains the prefix name of the example JSON file to be used to load data.
  Note:
    You must specify the function_name values as specified in the example sections of
    corresponding teradataml APIs. If any other string is passed as prefix input, an error will
    be raised as 'prefix_str_example.json' file not found. This *_example.json file contains the
    schema information for the tables that can be loaded using this JSON file.
* table_name
  This required argument specifies the name(s) of the table to be created in the database.
  Note:
    Table names provided here must have an equivalent datafile (CSV) present at teradataml/data.
    Schema information for the same must also be present in <function_name>_example.json as
    shown in 'function_name' argument description.
Note:
* The function creates a new table in Vantage with the name specified in the table_name
    argument. You must manually drop the table if required.
* If a table with the name provided for table_name argument already exists, this function skips
    creation and loading of the dataset.
* Database used for loading table depends on the following conditions:
    * If the configuration option temp_table_database is set, then the tables are loaded in the
    database specified in this option.
    * If the configuration option temp_table_database is not set and the temp_database_name
    argument is used while creating context, then the tables are loaded in the database
    specified in the temp_database_name argument.
    * If none of them are specified, then the tables are created in the connecting users' default
    database or the connecting database.
#### Example 1: When connection is created without temp_database_name, table is
#### loaded in users' default database
```python
>>> from teradataml import *
>>> con = create_context(host = 'tdhost', username='tduser', password
= 'tdpassword')
>>> load_example_data("pack", "ville_temperature")
# Create a teradataml DataFrame.
>>> df = DataFrame("ville_temperature")
```
#### Example 2: When connection is created with temp_database_name, table is
#### loaded in that database
This examples shows when connection is created with the temp_database_name argument, then tables
are loaded in the database specified in the temp_database_name argument.
This example also demonstrates loading multiple tables in a single function call.
```python
>>> con = create_context(host = 'tdhost', username='tduser', password =
'tdpassword', temp_database_name = "temp_db")
>>> load_example_data("GLM", ["admissions_train", "housing_train"])
# Create teradataml DataFrames.
>>> admission_train = DataFrame(in_schema("temp_db", "admissions_train"))
>>> housing_train = DataFrame(in_schema("temp_db", "housing_train"))
```
#### Example 3: When connection is created with temp_database_name and the
#### configuration option temp_table_database is set
This example shows when connection is created with temp_database_name and the configuration option
temp_table_database is also specified, the table is created in the database specified in the configuration
option temp_table_database.
```python
>>> from teradataml import *
>>> con = create_context(host = 'tdhost', username='tduser', password =
'tdpassword', temp_database_name = "temp_db")
>>> configure.temp_table_database = "temp_db1"
>>> load_example_data("pack", "ville_temperature")
# Create a teradataml DataFrame.
>>> df = DataFrame(in_schema("temp_db1", "ville_temperature"))
```
## Garbage Collection in teradataml
teradataml offers various DataFrame API's and analytic functions that enables users to process the data and
run analytics on the data. While doing so, teradataml internally creates various database objects (Views and
Tables) on Vantage, whenever and wherever necessary. All these internally generated views and tables are
temporarily created and have a session wide scope. At the end of the session, these views and tables will
be removed by teradataml. teradataml GarbageCollector takes care of cleaning up these internally created
views and tables.
When a Garbage Collection is performed, cleanup is performed for:
* Current Session:
  When a user ends a session, Garbage Collection of current active session is done. All temporarily
  created objects within that session are removed.
* Old stale or inactive Python sessions:
  GarbageCollector also performs cleanup for other terminated Python sessions for which cleanup was
  not done properly due to abrupt interruption in the session.
Garbage Collection is triggered automatically upon execution of the following function calls.
* remove_context
This API must be called when user wants to end the current session.
  Calling this API triggers the Garbage Collection to cleanup objects from current session as well as old
  inactive sessions.
* create_context
  This API must be called when either user wants to create a new connection or wants to overwrite the
  existing context.
  Calling this API also triggers Garbage Collection. When a new context is created, Garbage Collection
  is performed for the objects created by teradataml in an old, abruptly terminated Python sessions. If a
  context is being recreated, then Garbage Collection is performed for objects created in previous context
  and objects created by old abruptly terminated Python sessions.
* set_context
  When this API is used, old context is overwritten.
  Calling this API triggers Garbage Collection for current context being overwritten and old
  inactive sessions.
* At Session Exit
  At the end of the session, if you issue quit(), then Garbage Collection is performed.
Note:
* Teradata recommends calling remove_context to end a teradataml session so that cleanup is
    done properly.
* GarbageCollector uses the current context to perform Garbage Collection of intermediate views
    and tables.
    If the connecting user does not have permission to drop the views and tables from old inactive
    sessions, then those sessions are not cleaned up.
* GarbageCollector does not remove the objects created by teradataml from different client. It only
    attempts to remove the objects created by the same client.
* While using Jupyter notebooks, if session is abruptly terminated, even though session is inactive,
    cleanup is not done until the kernel responsible for the Python session is shutdown. Same case is
    applicable for directly closing the notebook. Close the notebook only after calling remove_context
    at the end.
## Run SQL Queries using teradataml
teradataml offers a way to run SQL queries on Vantage from Python interface.
#### For teradataml version 17.20.00.04 and later
From release 17.20.00.04, teradataml introduces new function execute_sql() that supports to run SQL
queries on Vantage.
You can use the execute_sql() function to run the queries once the connection is established with
Vantage database.
The following example shows how to use the execute_sql() function to create a table, insert rows into the
table, select rows of the table, and alter rows of the table.
* Establish connection with Vantage.
  Note:
    This must be done before using execute_sql().
  * Establish connection.
```python
>>> eng = create_context("<td_host>", "<td_user>", "<td_pwd>")
```
  * Get the connection object.
```python
>>> conn = get_connection()
```
* Make sure the table to be created ('employee_info') does not exist in the database.
  Check if the table exists or not.
```python
>>> conn.dialect.has_table(connection=conn, table_name="employee_info")
False
```
  Note:
    If the table is already present, drop the table as follows:
```python
>>> db_drop_table("employee_info")
True
```
    This is not needed when the table is not present.
* Create a table 'employee_info' using execute_sql().
  * Define a "create table" statement.
```python
>>> create_stmt = "create table employee_info (employee_no BIGINT,
first_name VARCHAR(50), last_name VARCHAR(50), date_of_joining DATE
FORMAT 'YYYY-MM-DD', DOB DATE FORMAT 'YYYY-MM-DD');"
```
  * Execute the "create table" statement.
```python
>>> output = execute_sql(create_stmt)
```
  * Print the output of execution.
```python
>>> for row in output:
...     print(row)
```
    Note:
    Generally, the output of the execute_sql() function returns the cursor. For this example, as it
    is a create table statement, empty result set is generated.
  * Check if the table exists or not.
```python
>>> conn.dialect.has_table(connection=conn, table_name="employee_info")
True
```
  * Create teradataml DataFrame.
```python
>>> df = DataFrame("employee_info")
```
  * Check the columns of the DataFrame.
```python
>>> df.columns
['employee_no', 'first_name', 'last_name', 'date_of_joining', 'DOB']
```
  * Check the shape of the DataFrame.
```python
>>> df.shape
(0, 5)
```
* Insert rows using execute_sql().
  * Define "insert" statements.
```python
>>> ins_stmt = "insert into employee_info values (28, 'my_first_name',
'my_last_name', '2018-06-11', '1987-11-25');"
>>> ins_stmt1 = "insert into employee_info values (4, 'my_first_name_1',
'my_last_name_1', '2018-06-11', '1988-02-23');"
```
  * Execute the "insert" statements.
```python
>>> ins = execute_sql(ins_stmt)
>>> ins1 = execute_sql(ins_stmt1)
```
    Note:
    Both "ins" and "ins1" have empty result set.
  * Check the shape of the DataFrame.
```python
>>> df.shape
(2, 5)
```
  * Print the teradataml DataFrame.
    >>> df
    first_name       last_name date_of_joining         DOB
    employee_no
    4            my_first_name_1  my_last_name_1      2018-06-11  1988-02-23
    28           my_first_name    my_last_name      2018-06-11  1987-11-25
* Select data using execute_sql().
  Note:
    Teradata recommends using teradataml APIs, instead of running SELECT queries.
  * Execute SELECT query using execute_sql().
```python
>>> sel_result = execute_sql("select * from employee_info;")
```
  * Print the entire result set.
```python
>>> for row in sel_result:
...     print(row)
[28, 'my_first_name', 'my_last_name', datetime.date(2018, 6, 11),
datetime.date(1987, 11, 25)]
[4, 'my_first_name_1', 'my_last_name_1', datetime.date(2018, 6, 11),
datetime.date(1988, 2, 23)]
```
  * Fetch and print only certain columns.
```python
>>> sel_result = execute_sql("select * from employee_info;")
>>> for row in sel_result:
...     print(str(row[0]) + " : " + row[1])
4 : my_first_name_1
28 : my_first_name
```
* Alter data using execute().
  * Define an ALTER query statement.
```python
>>> alter_stmt = "update employee_info set first_name = 'my_first_name_2'
where employee_no = 4;"
```
  * Execute the ALTER statement.
```python
>>> alter = execute_sql(alter_stmt)
```
    This produces empty result set.
  * Print the teradataml DataFrame.
```python
>>> df
first_name       last_name date_of_joining         DOB
employee_no
28           my_first_name    my_last_name      2018-06-11  1987-11-25
4            my_first_name_2  my_last_name_1    2018-06-11  1988-02-23
```
    The first_name for the employee with employee_id 4 is changed.
#### For teradataml version 17.20.00.03 and earlier
You can use the SQLAlchemy execute() method of the connection object returned by get_connection()
function to execute the queries.
Note:
This approach is not suitable if SQLAlchemy version is 2.0.x and later.
The following example shows how to use execute() method to create a table, insert rows into table, select
rows of the table and alter rows of the table.
* Create a connection and get the connection parameters.
  * Create the connection and get the engine.
```python
>>> eng = create_context("td_host", "td_user", "td_pwd")
```
  * Print the engine.
```python
>>> eng
Engine(teradatasql://td_user:***@td_host)
```
  * Get the connection object.
```python
>>> conn = get_connection()
```
  * Print the connection object.
```python
>>> conn
<sqlalchemy.engine.base.Connection at 0x7fb598edb610>
```
* Make sure the table to be created ('employee_info') does not exist in the database.
  Check if the table exists or not.
```python
>>> eng.dialect.has_table(connection=conn, table_name="employee_info")
False
```
  Note:
    If the table is already present, drop the table as follows:
```python
>>> db_drop_table("employee_info")
True
```
    This is not needed when the table is not present.
* Create table 'employee_info' using execute().
  * Define a "create table" statement.
```python
>>> create_stmt = "create table employee_info (employee_no BIGINT,
first_name VARCHAR(50), last_name VARCHAR(50), date_of_joining DATE
FORMAT 'YYYY-MM-DD', DOB DATE FORMAT 'YYYY-MM-DD');"
```
  * Execute the "create table" statement.
```python
>>> output = conn.execute(create_stmt)
```
  * Print the output of execution.
```python
>>> for row in output:
...     print(row)
```
    Note:
    Generally, the output of the execute() function returns the result set. For this example, as it
    is a create table statement, empty result set is generated.
  * Check if the table exists or not.
```python
>>> eng.dialect.has_table(connection=conn, table_name="employee_info")
True
```
  * Create teradataml DataFrame.
```python
>>> df = DataFrame("employee_info")
```
  * Check the columns of the DataFrame.
```python
>>> df.columns
['employee_no', 'first_name', 'last_name', 'date_of_joining', 'DOB']
```
  * Check the shape of the DataFrame.
```python
>>> df.shape
(0, 5)
```
* Insert rows using execute().
  * Define "insert" statements.
```python
>>> ins_stmt = "insert into employee_info values (28, 'my_first_name',
'my_last_name', '2018-06-11', '1987-11-25');"
>>> ins_stmt1 = "insert into employee_info values (4, 'my_first_name_1',
'my_last_name_1', '2018-06-11', '1988-02-23');"
```
  * Execute the "insert" statements.
```python
>>> ins = conn.execute(ins_stmt)
>>> ins1 = conn.execute(ins_stmt1)
```
    Note:
    Both "ins" and "ins1" have empty result set.
  * Check the shape of the DataFrame.
```python
>>> df.shape
(2, 5)
```
  * Print the teradataml DataFrame.
    >>> df
    first_name       last_name date_of_joining         DOB
    employee_no
    4            my_first_name_1  my_last_name_1      2018-06-11  1988-02-23
    28           my_first_name    my_last_name      2018-06-11  1987-11-25
* Select data using execute().
  Note:
    Teradata recommends using teradataml APIs, instead of running SELECT queries.
  * Execute SELECT query using execute().
```python
>>> sel_result = conn.execute("select * from employee_info;")
```
  * Print the entire result set.
```python
>>> for row in sel_result:
...     print(row)
(4, 'my_first_name_1', 'my_last_name_1', datetime.date(2018, 6, 11),
datetime.date(1988, 2, 23))
(28, 'my_first_name', 'my_last_name', datetime.date(2018, 6, 11),
datetime.date(1987, 11, 25))
```
  * Fetch and print only certain columns.
```python
>>> sel_result = conn.execute("select * from employee_info;")
>>> for row in sel_result:
...     print(str(row['employee_no']) + " : " + row['first_name'])
4 : my_first_name_1
28 : my_first_name
```
* Alter data using execute().
  * Define an ALTER query statement.
```python
>>> alter_stmt = "update employee_info set first_name = 'my_first_name_2'
where employee_no = 4;"
```
  * Execute the ALTER statement.
```python
>>> alter = conn.execute(alter_stmt)
```
    This produces empty result set.
  * Print the teradataml DataFrame.
```python
>>> df
first_name       last_name date_of_joining         DOB
employee_no
28           my_first_name    my_last_name      2018-06-11  1987-11-25
4            my_first_name_2  my_last_name_1    2018-06-11  1988-02-23
```
    The first_name for the employee with employee_id 4 is changed.