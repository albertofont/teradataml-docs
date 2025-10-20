# Script Methods (SCRIPT Table Operator)

## Script
The teradataml Script function is an interface to the SCRIPT table operator (STO) object in the Database
Engine 20. See the SCRIPT section of Table Operators in  SQL Operators and User-Defined Functions ,
B035-1210 for details about SCRIPT table operator.
You can use Script function to execute natively in Vantage a user-installed script against data from an
Database Engine 20 table via the SCRIPT table operator. To do this, the Teradata In-nodes Interpreter and
Add-ons language packages must be installed in advance in the target Database Engine 20 nodes.
**Required arguments:**
* script_command : specifies the command or script to run,.
* script_name : specifies the name of the user script.
* files_local_path  specifies the absolute local path where user script and all supporting files like model
* files, input data file reside.
* returns : specifies output column definition, which is a dictionary specifying column-name to
* teradatasqlalchemy-type mapping.
* Note:
    * Users can pass a dictionary (dict or OrderedDict) to the  returns  argument, with the keys ordered
    * to represent the order of the output columns. The preferred type is OrderedDict.
**Optional arguments:**
* data : specifies a teradataml DataFrame containing the input data for the script.
* data_hash_column : specifies the column to be used for hashing.
* The rows in the data will be redistributed to AMPs based on the hash value of the column specified.
* The user-installed script file then runs once on each AMP. If there is no  data_partition_column , then the
* entire result set, delivered by the function, constitutes a single group or partition.
* Note:
    * data_hash_column  can not be specified along with  data_partition_column ,  is_local_order
    * and  data_order_column .
* data_partition_column : specifies Partition By columns for data.
12

* Note:
    * data_partition_column  can not be specified along with  data_hash_column .
    * data_partition_column  can not be specified along with " is_local_order  = True".
* is_local_order : specifies a boolean value to determine whether the input data is to be ordered locally
* or not.
* Default value is False. When set to True, data is ordered locally.
* Note:
    * This argument is ignored, if  data_order_column  is None.
    * is_local_order  can not be specified along with  data_hash_column .
    * When  is_local_order  is set to 'True', specify  data_order_column , and the columns specified
    * in  data_order_column  are used for local ordering.
* data_order_column : specifies the Order By column for  data , and can be used no matter  is_local_order
* is set to 'True' or 'False'.
* Note:
    * data_order_column  can not be specified along with  data_hash_column .
* sort_ascending : specifies a boolean value to determine if the result set is to be sorted on the
* data_order_column  column in ascending or descending order.
* When set to the default value 'True', the sorting is ascending. When set to 'False', the sorting
* is descending.
* Note:
    * This argument is ignored, if  data_order_column  is None.
* nulls_first : specifies a boolean value to determine whether NULLS are listed first or last during ordering.
* NULLS are listed first when this argument is set to 'True', and last when set to 'False'.
* Note:
    * This argument is ignored, if  data_order_column  is None.
* delimiter : specifies a delimiter to use when reading columns from a row and writing result columns.
* The default value is '\t' (tab).
12: Script Methods

* Note:
    * This argument cannot be the same as  quotechar  argument.
    * This argument cannot be a newline character, which is '\\n'.
* auth : specifies an authorization to use when running the script.
* charset : specifies the character encoding for data, could be either 'utf16' or 'latin'.
* quotechar : specifies a character that forces all input and output of the script to be quoted using this
* specified character.
* Using this argument enables the Database Engine 20 to distinguish between NULL fields and empty
* strings. A string with length zero is quoted, while NULL fields are not.
* If this character is found in the data, it will be escaped by a second quote character.
* Note:
    * This argument cannot be the same as  delimiter  argument.
    * This argument cannot be a newline character, which is '\\n'.
For details of all arguments, see  Teradata Package for Python Function Reference .
### Required permissions
**Prior to running Script functions, specify search path and permissions as listed here:**
**Note:**
Assume the user is 'TDAPUSER', and the database is 'TDAPUSERDB'.
* Teradata user needs permission to execute Script Table Operator and the default authentication.
```python
GRANT EXECUTE FUNCTION ON td_sysfnlib.script TO TDAPUSER;
GRANT EXECUTE ON SYSUIF.DEFAULT_AUTH to TDAPUSER;
```
* Additional permissions required to run Script on the database.
```python
GRANT SELECT ON TDAPUSERDB TO TDAPUSER;
```
* Additional permissions required to install, remove and replace files.
```python
GRANT EXECUTE PROCEDURE ON SYSUIF.INSTALL_FILE TO TDAPUSER;
GRANT EXECUTE PROCEDURE ON SYSUIF.REPLACE_FILE TO TFDAPUSER;
```
12: Script Methods

```python
GRANT EXECUTE PROCEDURE ON SYSUIF.REMOVE_FILE TO TDAPUSER;
```
* You must set SEARCHUIFDBPATH prior to accessing any installed scripts in the target database, so
* that the Linux system user 'tdatuser' can find the scripts and run them through the STO. If the scripts
* reside in 'TDAPUSERDB', then set search path in corresponding Python script or Python console
* as follows:
```python
get_connection().execute("SET SESSION SEARCHUIFDBPATH = TDAPUSERDB;")
```
* If the user ('me' in the example here) is using another database ('TDAPUSERDB' in the example
* here) as default database, the user must have the execute permissions on the Script function to the
* default database.
Here is the quick example.
1.
* Create the required users and databases as database administrator.
```python
-- Create a user 'sysdba' with default database as 'sysdba'.
CREATE USER sysdba FROM DBC
AS PERM = 14e9
PASSWORD = sysdba
DEFAULT DATABASE = sysdba
NO BEFORE JOURNAL
NO AFTER JOURNAL;
-- Create a new database from the newly created user 'sysdba'.
CREATE DATABASE TDAPUSERDB FROM sysdba
AS PERM=1e9
NO BEFORE JOURNAL
NO AFTER JOURNAL;
-- Create a user with default database as newly created database 'TDAPUSERDB'.
CREATE USER me FROM sysdba
AS PERM = 100e6
PASSWORD = me
DEFAULT DATABASE = TDAPUSERDB
NO BEFORE JOURNAL
NO AFTER JOURNAL;
```
2.
* Make sure the file 'mapper.py' is installed in the database 'TDAPUSERDB'. Refer to the  Database Utility
* section for how to use install_file() to install file in Vantage. Ignore if the file is already present in Vantage.
3.
* The database administrator must provide the following permissions to the user 'me', if not
* already provided.
```python
-- Permissions to the user 'me'.
GRANT EXECUTE FUNCTION ON td_sysfnlib.script TO me;
```
12: Script Methods

```python
GRANT EXECUTE ON SYSUIF.DEFAULT_AUTH to me;
-- Additional permissions on the database 'TDAPUSERDB'.
GRANT CREATE TABLE ON TDAPUSERDB TO me; -- Needed if the table and required
permissions are not present.
GRANT CREATE VIEW ON TDAPUSERDB TO me; -- Needed if the permissions for view
creation is not present.
GRANT SELECT ON TDAPUSERDB TO me;
```
4.
* Set the search path and run the Script function as the user 'me'.
```python
-- Set the search path.
SET SESSION SEARCHUIFDBPATH = TDAPUSERDB;
-- Create view on top of Script function.
-- Note that view creation is successful.
create view vw3 as SELECT * FROM Script(
ON "TDAPUSERDB"."usecase5" AS "input"
PARTITION BY ANY
SCRIPT_COMMAND('Rscript ./TDAPUSERDB/mapper.R')
delimiter(',')
returns('oc1 VARCHAR(10), oc2 VARCHAR(10)')
) as sqlmr;
*** View has been created.
*** Total elapsed time was 1 second.
-- Seeing the contents of the view is not possible as the user 'me'
don't have execute permissions on TD_SYSFNLIB.SCRIPT and default auth
i.e., SYSUIF.DEFAULT_AUTH.
select * from vw3;
*** Failure 3523 An owner referenced by user does not have EXECUTE FUNCTION
WITH GRANT OPTION access to TD_SYSFNLIB.SCRIPT.
```
5.
* The user 'me' has to be given the following permissions, for the SELECT on view 'vw3' to work in step 4.
```python
GRANT EXECUTE FUNCTION ON td_sysfnlib.script TO TDAPUSERDB WITH GRANT OPTION;
GRANT EXECUTE ON SYSUIF.DEFAULT_AUTH TO TDAPUSERDB WITH GRANT OPTION;
```
12: Script Methods

## teradataml Script Class for SCRIPT Table Operator
As an interface to the SCRIPT table operator object in Database Engine 20, Script offers execution in
**two modes:**
* Test/Debug mode: to test user scripts locally.
* Supported methods:
    * test_script : test user script.
    * set_data : set test data parameters.
* In-Database Script Execution mode: to execute user scripts in database.
* Supported methods:
    * execute_script : execute user script in Vantage.
    * install_file : install or replace file in database.
    * remove_file : remove installed files from database.
    * set_data : set test data parameters.
### set_data
Use the set_data method to set data and data related arguments without having to re-create Script object.
**Some of the set_data method arguments used in the examples:**
* Required argument  data  specifies a teradataml DataFrame containing the input data for the script.
* Optional argument  is_local_order  specifies a boolean value to determine whether the input data is to
* be ordered locally or not.
* Optional argument  data_order_column  specifies the Order By column for  data , and can be used no
* matter  is_local_order  is set to 'True' or 'False'.
* Optional argument  sort_ascending  specifies a boolean value to determine if the result set is to
* be sorted on the column specified in  data_order_column , in ascending or descending order. This
* argument is ignored, if  data_order_column  is None.
* Optional argument  nulls_first  specifies a boolean value to determine whether NULLS are listed first or
* last during ordering.
For details of all arguments, see  Teradata Package for Python Function Reference .
### Example Prerequisites
* To run the examples, "mapper.py" and "barrier.csv" are required and must be present under the same
* location specified by the argument  files_local_path .
    * "barrier.csv" is present under  <teradataml_install_location>/teradataml/data  directory.
    * "mapper.py" can be created as follows:
```python
!/usr/bin/python
import sys
```
12: Script Methods

```python
for line in sys.stdin:
line = line.strip()
words = line.split()
for word in words:
print ('%s\t%s' % (word, 1))
```
* Before running the examples, import required packages:
```python
from collections import OrderedDict
from teradatasqlalchemy import (VARCHAR)
```
### Example 1
This example uses  test_script  to test a Python script, then reset data and related arguments using set_data
method and run Python script on Vantage with new parameters.
**Note:**
All data related arguments that are not specified in set_data() will be reset to default values.
* Create teradataml DataFrame.
```python
barrierdf = DataFrame.from_table("barrier")
```
* Create a Script object without data and its arguments.
```python
sto = Script(
script_name='mapper.py',
files_local_path= 'data/scripts',
script_command='python3 ./<database name>/mapper.py',
charset='latin',
returns=OrderedDict([("word", VARCHAR(15)),
("count_input", VARCHAR(2))])
```
* Test script using data from file.
```python
sto.test_script(input_data_file='../barrier.csv')
STDOUT Output
word count_input
0  Macdonald           1
1          A           1
2       Farm           1
3        Had           1
```
12: Script Methods

```python
4        Old           1
5          1           1
```
* Set data and related arguments to run and test the script on actual data on Vantage.
```python
sto.set_data(data='barrier',
data_order_column="Id",
is_local_order=False,
nulls_first=False,
sort_ascending=False
```
* Set the search path to the database where the file is installed.
```python
get_context().execute("SET SESSION SEARCHUIFDBPATH = <database name>;")
```
* Run the user script on Vantage.
```python
sto.execute_script()
STDOUT Output
word count_input
0  Macdonald           1
1          A           1
2       Farm           1
3        Had           1
4        Old           1
5          1           1
```
### Example 2
In this example, user resets data and related arguments, and execute script on Vantage again.
**Note:**
All data related arguments that are not specified in set_data() will be reset to default values.
* Create a Script object that allows user to run script on Vantage.
```python
sto = Script(data=barrierdf,
script_name='mapper.py',
files_local_path= 'data/scripts',
script_command='python3 ./<database name>/mapper.py',
data_order_column="Id",
is_local_order=False
delimiter=',',
nulls_first=False,
```
12: Script Methods

```python
sort_ascending=False,
charset='latin', returns=OrderedDict([("word", VARCHAR(15)),
("count_input", VARCHAR(2))])
```
* Set the search path to the database where the file is installed.
```python
get_context().execute("SET SESSION SEARCHUIFDBPATH = <database name>;")
```
* Execute the script on Vantage.
```python
sto.execute_script()
STDOUT Output
word count_input
0  Macdonald           1
1          A           1
2       Farm           1
3        Had           1
4        Old           1
5          1           1
```
* Run the set_data() to reset data and some related parameter, to run the script with a different dataset.
    * Create a new DataFrame.
```python
barrierdf_new = DataFrame.from_table("barrier_new")
```
    * Set data and related arguments with different values.
```python
sto.set_data(data=barrierdf_new, data_order_column='Id',
is_local_order=True, nulls_first=True)
```
    * Run the script again.
```python
sto.execute_script()
word  count_input
0  Macdonald            1
1          A            1
2       Farm            1
3          2            1
4        his            1
5       farm            1
6         On            1
7        Had            1
8        Old            1
9          1            1
```
12: Script Methods

### Example 3
In this example, user run the script again with same dataset but different data related arguments by using
set_data() to reset arguments.
**Note:**
All data related arguments that are not specified in set_data() will be reset to default values.
* Set related arguments with different values, but use same data.
```python
sto.set_data(data=barrierdf, data_order_column='Id',
is_local_order=True, nulls_first=True)
```
* Set the search path to the database where the file is installed.
```python
get_context().execute("SET SESSION SEARCHUIFDBPATH = <database name>;")
```
* Run the script again.
```python
sto.execute_script()
STDOUT Output
word count_input
0  Macdonald           1
1          A           1
2       Farm           1
3        Had           1
4        Old           1
5          1           1
```
### test_script
Use the test_script method to run script locally outside Vantage. Input data for user script is read from a csv
file or from a table in the database.
**Note:**
The purpose of test_script() function is to enable you to test your scripts for possible errors without
installing it on Vantage.
    * If called with input data file, test_script() function will simply read the data from the file and provide
    * the data as input to the user script.
    * There is no data partitioning when input is from a file.
    * The test_script() function may produce different output if input is read from a file than input
    * from database.
12: Script Methods

**Required arguments:**
* input_data_file : Specifies the name of the input data file. It should have a path relative to the location
* specified in  file_local_path .
* If set to None, data is read from AMP; otherwise, data is from the file passed in the
* argument  input_data_file .
* Note:
    * The file should have at least permission of mode 644.
**Optional arguments:**
* supporting_files : Specifies a file or list of supporting files like model files to be copied to local system.
* script_args : Specifies command line arguments required by the user script.
* exec_mode : Specifies the mode in which user wants to test the script.
* When set to 'local', it will run locally on user's system.
* Note:
    * When 'local' execution mode is used, Teradata recommends having the same Python packages
    * and versions installed on local machine as those installed on the Vantage cluster. Python
    * packages installed on Vantage and the corresponding version information can be found using the
    * db_python_package_details()  function.
* Permitted value is 'local'.
* Default value is 'local'.
* **kwargs : Specifies the keyword arguments required for testing.
* Possible keys:
    * data_row_limit : Specifies the number of rows to be taken from all AMPs when reading
    * from Vantage.
    * It is ignored when data is read from file.
    * password : Specifies the password to connect to Vantage where the data resides.
    * It is required when reading from database.
    * data_file_delimiter : Specifies the delimiter used in the input data file.
    * It can be specified when data is read from file.
    * data_file_header : Specifies whether the input data file contains header.
    * It can be specified when data is read from file.
    * data_file_quote_char : Specifies the quotechar used in the input data file.
    * It can be specified when data is read from file.
12: Script Methods

* logmech : Specifies the type of logon mechanism to establish a connection to Vantage.
    * Permitted values include:
    * ▪
    * TD2: The Teradata 2 (TD2) mechanism provides authentication using a Vantage username
    * and password.
    * This is the default logon mechanism using which the connection is established to Vantage.
    * ▪
    * TDNEGO: This is a security mechanism that automatically determines the actual mechanism
    * required, based on policy, without user's involvement. The actual mechanism is determined
    * by the TDGSS server configuration and by the security policy's mechanism restrictions.
    * ▪
    * LDAP: This is a directory-based user logon to Vantage with a directory username and
    * password and is authenticated by the directory.
    * ▪
    * KRB5 (Kerberos): This is a directory-based user logon to Vantage with a domain username
    * and password and is authenticated by Kerberos (KRB5 mechanism).
    * Note:
    * User must have a valid ticket-granting ticket in order to use this logon mechanism.
    * ▪
    * JWT: The JSON Web Token (JWT) authentication mechanism enables single sign-on (SSO)
    * to the Vantage after the user successfully authenticates to Teradata UDA User Service.
    * Note:
    * User must use  logdata  parameter when using 'JWT' as the logon mechanism.
    * Note:
    * teradataml expects the client environments are already set up with appropriate security
    * mechanisms and are in working conditions. See  Security Administration , B035-1100 for
    * more information.
    * logdata : Specifies parameters to the LOGMECH command beyond those needed by the logon
    * mechanism, such as user ID, password and tokens (in case of JWT) to successfully authenticate
    * the user.
    * Note:
    * When data is read from file, even if these arguments are passed, they will be ignored.
For details of all arguments, see  Teradata Package for Python Function Reference .
### Example 1: Test script in local mode with input from table
In this example, the script "mapper.py" reads in a line of text input ("Old Macdonald Had A Farm") from csv
and splits the line into individual words, emitting a new row for each word.
1.
* Load example data.
12: Script Methods

```python
load_example_data("Script", ["barrier"])
```
2.
* Import required packages.
```python
from collections import OrderedDict
from teradatasqlalchemy import (VARCHAR)
```
3.
* Create teradataml DataFrame objects.
```python
barrierdf = DataFrame.from_table("barrier")
```
4.
* Create a Script object that allows user to execute script on Vantage.
```python
sto = Script(data=barrierdf,
script_name='mapper.py',
files_local_path= 'data/scripts',
script_command='python3 ./<database name>/mapper.py',
data_order_column="Id",
is_local_order=False
delimiter=',',
nulls_first=False,
sort_ascending=False,
charset='latin', returns=OrderedDict([("word", VARCHAR(15)),
("count_input", VARCHAR(2))])
```
5.
* Run user script in local mode with input from table.
```python
sto.test_script(data_row_limit=300,
password='<password>', exec_mode='local')
STDOUT Output
word  count_input
0          1            1
1        Old            1
2  Macdonald            1
3        Had            1
4          A            1
5       Farm            1
```
### execute_script
Use the execute_script to run script on Vantage.
This method does not have any arguments.
12: Script Methods

### Example
This example shows a workflow to create a Script object, install Python script on Vantage, runs it and then
removes the script from Vantage.
The script "mapper.py" reads in a line of text input ("Old Macdonald Had A Farm") from csv and splits the
line into individual words, emitting a new row for each word.
To run this example, "mapper.py" and "barrier.csv" are required and must be present under the same
location specified by the argument  files_local_path .
* "barrier.csv" is present under  <teradataml_install_location>/teradataml/data  directory.
* "mapper.py" can be created as follows:
```python
!/usr/bin/python
import sys
for line in sys.stdin:
line = line.strip()
words = line.split()
for word in words:
print ('%s\t%s' % (word, 1))
```
* Load example data.
```python
load_example_data("Script", ["barrier"])
```
* Import required packages.
```python
from collections import OrderedDict
from teradatasqlalchemy import (VARCHAR)
```
* Create teradataml DataFrame.
```python
barrierdf = DataFrame.from_table("barrier")
```
* Create a Script object that allows user to run script on Vantage.
```python
sto = Script(data=barrierdf,
script_name='mapper.py',
files_local_path= 'data/scripts',
script_command='python3 ./<database name>/mapper.py',
data_order_column="Id",
is_local_order=False
delimiter=',',
nulls_first=False,
sort_ascending=False,
charset='latin', returns=OrderedDict([("word", VARCHAR(15)),
```
12: Script Methods

```python
("count_input", VARCHAR(2))])
```
* Install script on Vantage.
```python
sto.install_file(file_identifier='mapper',
file_name='mapper.py', is_binary=False)
```
* Set the search path to the database where the file is installed.
```python
get_context().execute("SET SESSION SEARCHUIFDBPATH = <database name>;")
```
* Run the user script on Vantage.
```python
sto.execute_script()
STDOUT Output
word count_input
0  Macdonald           1
1          A           1
2       Farm           1
3        Had           1
4        Old           1
5          1           1
```
* Remove the installed file from Vantage.
```python
sto.remove_file(file_identifier='mapper', force_remove=True)
```
### install_file
Use the install_file method to install or replace script on Vantage.
**Required arguments:**
* file_identifier  specifies the name associated with the user-installed file. It cannot have a database
* name associated with it, as the file is always installed in the current database. The name must be
* unique within the database. It can be any valid Teradata identifier.
* file_name  specifies the name of the file user wants to install.
**Optional arguments:**
* is_binary  specifies if the file to be installed is a binary file.
* force_replace  specifies if system checks for the file being used before replacing it:
    * If set to True, then the file is replaced even if it is being executed.
    * If set to False, then an error is thrown if it is being executed.
12: Script Methods

**Note:**
File can be on client or remote server. Specify the file location accordingly.
### Example 1: Install a file found at the relative path using default text mode
**Note:**
To run this example, "mapper.py" is required on client at the path specified by
argument  file_local_path .
* Create the "mapper.py" file.
```python
!/usr/bin/python
import sys
for line in sys.stdin:
line = line.strip()
words = line.split()
for word in words:
print ('%s\t%s' % (word, 1))
```
* Set the search path to the database where the file is installed.
```python
get_context().execute("SET SESSION SEARCHUIFDBPATH = <database name>;")
```
* Import required packages.
```python
from collections import OrderedDict
from teradatasqlalchemy import (VARCHAR)
```
* Create a Script object that allows user to run script on Vantage.
```python
sto = Script(data=barrierdf,
script_name='mapper.py',
files_local_path= 'data/scripts',
script_command='python3 ./<database name>/mapper.py',
data_order_column="Id",
is_local_order=False
delimiter=',',
nulls_first=False,
sort_ascending=False,
charset='latin', returns=OrderedDict([("word", VARCHAR(15)),
("count_input", VARCHAR(2))])
```
* Install the file on Vantage.
12: Script Methods

```python
sto.install_file(file_identifier='mapper',
file_name='mapper.py', is_binary=False)
File mapper.py installed in Vantage
```
* After making changes to the file "mapper.py", user can use the install_file method with " replace =True"
* to replace the existing file with the new file.
```python
sto.install_file(file_identifier='mapper', file_name='mapper.py',
is_binary=False, replace=True, force_replace=True)
File mapper.py replaced in Vantage
```
### Example 2: Install a file found at the relative path using binary mode
```python
sto.install_file (file_identifier='binaryfile',
file_name='binary_file.dms', is_binary = True)
File binary_file.dms installed in Vantage
```
### remove_file
Use the remove_file method to remove user installed files or scripts from Vantage.
**Required arguments:**
* file_identifier  specifies the name associated with the user installed file. It cannot have a database
* name associated with it, as the file is always installed in the current database.
* force_remove  specifies if system checks for the file being used before removing it.
    * If set to True, then the file is removed even if it is being executed.
    * If set to False, which is the default value, then an error is thrown if it is being executed.
### Example 1: Install a file found at the relative path using default text mode and
### then remove the file
* Follow steps in  install_file  to install a file using default text mode.
* Remove the installed file.
```python
sto.remove_file (file_identifier='mapper', force_remove=True)
File mapper removed from Vantage
```
### Example 2: Install a file found at the relative path using binary mode and then
### remove the file
* Follow steps in  install_file  to install a file using binary mode.
* Remove the installed file.
```python
sto.remove_file (file_identifier='binaryfile', force_remove=False)
File binaryfile removed from Vantage
```
12: Script Methods

### deploy
Use the  deploy  method to deploy a model generated using  execute_script()  in database.
**Required Arguments:**
* model_column : Specifies the column name in which model is present.
* Supported types of model in this column are CLOB and BLOB.
* Note:
    * The column mentioned in this argument should be present in <script_obj>.result.
**Optional Arguments:**
* partition_columns : Specifies the columns on which data is partitioned.
* Note:
    * The columns mentioned in this argument should be present in <script_obj>.result.
* model_file_prefix : Specifies the prefix to be used to the generated model file.
* If this argument is None, prefix is auto-generated.
* When the argument  model_column  contains multiple models:
    * If  partition_columns  is None, model file prefix is appended with underscore(_) and numbers
    * starting from one (1) to get model file names.
    * If  partition_columns  is not None, model file prefix is appended with underscore(_) and unique
    * values in  partition_columns  to generate model file names.
### Examples Setup
* Load example data.
```python
load_example_data("openml", "multi_model_classification")
```
* Create teradataml DataFrame from the data.
```python
df = DataFrame("multi_model_classification")
```
* Install Script file.
```python
file_location = os.path.join(os.path.dirname(teradataml.__file__),
"data", "scripts", "deploy_script.py")
install_file("deploy_script", file_location, replace=True)
```
* Create variables needed for Script execution.
12: Script Methods

```python
script_command = '/opt/teradata/languages/Python/bin/python3 ./
ALICE/deploy_script.py'
partition_columns = ["partition_column_1", "partition_column_2"]
columns = ["col1", "col2", "col3", "col4", "label",
"partition_column_1", "partition_column_2"]
returns = OrderedDict([("partition_column_1", INTEGER()),
("partition_column_2", INTEGER()),
("model", CLOB())])
```
* Run the Script.
```python
obj = Script(data=df.select(columns),
script_command=script_command,
data_partition_column=partition_columns,
returns=returns)
opt = obj.execute_script()
opt
partition_column_1  partition_column_2               model
0                  10   b'gAejc1.....drIr'
0                  11   b'gANjcw.....qWIu'
1                  10   b'abdwcd.....dWIz'
1                  11   b'gA4jc4.....agfu'
```
### Example 1: Provide only  partition_columns  argument
In this example, only  partition_columns  is provided,  model_file_prefix  is auto-generated.
```python
obj.deploy(model_column="model",
partition_columns=["partition_column_1", "partition_column_2"])
['model_file_1710436227163427__0_10',
'model_file_1710436227163427__1_10',
'model_file_1710436227163427__0_11',
'model_file_1710436227163427__1_11']
```
12: Script Methods

### Example 2: Provide only  model_file_prefix  argument
In this example, only  model_file_prefix  is provided, the file names are suffixed with 1, 2, 3, ... for
multiple models.
```python
obj.deploy(model_column="model", model_file_prefix="my_prefix_new_")
['my_prefix_new__1',
'my_prefix_new__2',
'my_prefix_new__3',
'my_prefix_new__4']
```
### Example 3: Neither  partition_columns  nor  model_file_prefix  argument is provided
```python
obj.deploy(model_column="model")
['model_file_1710438346528596__1',
'model_file_1710438346528596__2',
'model_file_1710438346528596__3',
'model_file_1710438346528596__4']
```
### Example 4: Provide both  partition_columns  and  model_file_prefix  arguments
```python
obj.deploy(model_column="model", model_file_prefix="my_prefix_new_",
partition_columns=["partition_column_1", "partition_column_2"])
['my_prefix_new__0_10',
'my_prefix_new__0_11',
'my_prefix_new__1_10',
'my_prefix_new__1_11']
```
12: Script Methods