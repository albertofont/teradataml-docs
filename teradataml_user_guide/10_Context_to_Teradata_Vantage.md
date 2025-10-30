# Context to Teradata Vantage

* create_context
* get_context
* get_connection
* set_context
* remove_context
## create_context
Use the create_context() function to create a connection to the Vantage using the teradatasql +
teradatasqlalchemy DBAPI and dialect combination.
You can create a connection by passing the connection parameters using any of the following methods:
* Pass all required parameters directly to the function, OR
* Set the connection parameters in a configuration file (.cfg or .env) and pass the configuration file OR
* Set the connection parameters in environment variables and create_context() reads from
  environment variables.
Alternatively, you can pass a SQLAlchemy engine to the tdsqlengine parameter to override the default
DBAPI and dialect combination.
The information in this topic provides details to establish connection from config file or through
environment variables.
#### Optional Arguments
host
    Specifies the fully qualified domain name or IP address of the Vantage system.
    Types: str
username
    Specifies the username for logging on to Vantage.
    Types: str
password
    Specifies the password for logging on to Vantage.
    Types: str
tdsqlengine
    Vantage sql-alchemy engine object that should be used to establish a Vantage connection.
    Types: str
temp_database_name
    Specifies the temporary database name where temporary tables or views will be created.
    Types: str
logmech
    Specifies the type of logon mechanism to establish a Vantage connection.
    Permitted Values: 'TD2', 'TDNEGO', 'LDAP', 'KRB5' & 'JWT'.
      TD2: The Teradata 2 (TD2) mechanism provides authentication using a Vantage
      username and password. This is the default logon mechanism using which the
      connection is established to Vantage.
      TDNEGO: A security mechanism that automatically determines the actual
      mechanism required, based on policy, without user's involvement. The actual
      mechanism is determined by the TDGSS server configuration and by the security
      policy's mechanism restrictions.
      LDAP: A directory-based user logon to Vantage with a directory username and
      password and is authenticated by the directory.
      KRB5 (Kerberos): A directory-based user logon to Vantage with a domain username
      and password and is authenticated by Kerberos (KRB5 mechanism).
      JWT: The JSON Web Token (JWT) authenticates mechanism enables Single
      sign-on to the Vantage after the user successfully authenticates to Teradata UDA
      user service.
    Types: str
logdata
    Specifies parameter to the LOGMECH command beyond those needed by the logon
    mechanism, such as user ID, password, and tokens (in case of JWT) to successfully
    authenticate the user.
    Types: str
database
    Specifies the initial database to use after logon instead of the user's default database.
    Types: str
config_file
    Specifies the name of the configuration file to read the connection parameters.
    Note:
      * If you do not specify full path of file, then file lookup is done at the current
      working directory.
      * The content of the file must be in '.env' format.
      * Use parameters of create_context() as key in the configuration file.
    For more information, refer to the following topics:
    * Create connection using config file
    * Create a connection using environment variables
    * Order of preference between config file, environment variables and arguments
      of create_context
    Default value: td_properties.cfg
    Types: str
sql_timeout
    Specifies the timeout for executing each SQL request.
    Note:
      Zero means no timeout.
base_url
    Specifies the end point for corresponding database environment.
ues_url
    Specifies the end point for User Environment Service (UES).
client_id
    Specifies the id of the application that requests the access token from VantageCloud Lake.
pat_token
    Required, if PAT authentication is to be used, optional otherwise.
    Specifies the PAT token generated from VantageCloud Lake Console.
pem_file
    Required, if PAT authentication is to be used, optional otherwise.
    Specifies the path to private key file which is generated from VantageCloud Lake Console.
    Note:
      Teradata recommends not to change the name of the file generated from VantageCloud
      Lake Console. If the name of the file is changed, the authentication token generated from
      this function will not work.
auth_token
    Specifies the authentication token to connect to VantageCloud Lake.
expiration_time
    Specifies the expiration time of the token in seconds. After expiry time JWT token expires and
    UserEnv methods does not work, user should regenerate the token.
    Note:
      This option is used only for PAT and not for OAuth.
    Default value: 31536000
kid
    Specifies the name of the key that is used while generating 'pem_file'.
#### Setting Tables or Views Permissions
If you do not have permissions to create tables or views in your own database, but are allowed to create
the required objects in other common database, you must use the argument temp_database_name to pass
the name of the other database where temporary tables and views will be created. If this argument is not
provided, intermediate database objects are created in your default database.
The temp_database_name and database arguments play a crucial role in determining which database is
used by default to lookup for tables and views while creating teradataml DataFrame using 'DataFrame()' and
'DataFrame.from_table()' and which database is used to create all internal temporary objects.
set_config_params() can be used to set temp_table_name and temp_view_name. See
temp_table_database and temp_view_database.
When configure.temp_object_type option is set to VT, then instead of temporary views, teradataml
creates volatile tables in the user’s database. Hence, temp_database_name and database arguments will
be ignored.
create_context configure or set_config_params() teradataml behavior
database temp_database_name temp_table_name temp_view_name TABLE created in VIEW created in TABLE and VIEW read from
Yes Yes No No temp_database_name temp_database_name database
Yes Yes Yes Yes temp_table_name temp_view_name database
Yes Yes Yes No temp_table_name temp_database_name database
Yes Yes No Yes temp_database_name temp_view_name database
Yes No No No database database database
Yes No Yes Yes temp_table_name temp_view_name database
Yes No Yes No temp_table_name database database
Yes No No Yes database temp_view_name database
No Yes No No temp_database_name temp_database_name User's default database
No Yes Yes Yes temp_table_name temp_view_name User's default database
No Yes Yes No temp_table_name temp_database_name User's default database
No Yes No Yes temp_database_name temp_view_name User's default database
No No No No User's default database User's default database User's default database
No No Yes Yes temp_table_name temp_view_name User's default database
No No Yes No temp_table_name User's default database User's default database
No No No Yes User's default database temp_view_name User's default database
teradataml requires that you have certain permissions on the user's default database or the initial default
database specified using database, or the temporary database specified using temp_database_name.
These permissions allow you to:
* Create tables and views to save results of teradataml analytic functions;
* Create views in the background for DataFrame APIs such as 'assign()', 'filter()', etc., whenever the
  result for these APIs are accessed using a 'print()';
* Create views in the background on the query passed to the 'DataFrame.from_query()' API.
Verify you have the correct permissions to create these objects in the database to be used.
The access to the views created may also require issuing additional GRANT SELECT ... WITH GRANT
```python
OPTION permission depending on which database is used and which object the view being created is
```
based on.
#### Passwords
Passwords passed as part of either password or logdata argument can be in encrypted format using Stored
Password Protection.
The following examples section demonstrates passing encrypted password to the password argument.
For more information on Stored Password Protection and how to generate key and encrypted password
file used in examples, see the Stored Password Protection section under the teradatasql project, on
https://pypi.org/user/teradata/.
Special characters that may be used in the password are encoded by default.
#### Usage Considerations
* When overwriting an existing context associated with a Vantage connection, most of the operations on
  any teradataml DataFrames created before will not work.
  Note:
    Teradata recommends calling remove_context() to end a session, so that intermediate views and
    tables created by teradataml are garbage collected.
* Executing create_context() triggers garbage collection in teradataml.
  See Garbage Collection in teradataml for more details.
* teradataml expects the client environments are already set up with appropriate security mechanisms
  and are in working conditions.
  For more information, refer to Security Administration.
#### Example 1: Create a context using hostname, username and password
```python
>>> from teradataml.context.context import *
>>> create_context(host = 'tdhost', username='tduser', password = 'tdpassword')
```
#### Example 2: Create a context using already created sqlalchemy engine
```python
>>> from sqlalchemy import create_engine
>>> sqlalchemy_engine  = create_engine('teradatasql://'+ tduser +':' +
tdpassword + '@'+tdhost
>>> from teradataml.context.context import *
>>> create_context(tdsqlengine = sqlalchemy_engine)
```
#### Example 3: Create a context with default logmech 'TD2'
```python
>>> from teradataml.context.context import *
>>> create_context(host = 'tdhost', username='tduser', password =
'tdpassword', logmech='TD2')
```
#### Example 4: Create a context with default logmech 'TDNEGO'
```python
>>> from teradataml.context.context import *
>>> create_context(host = 'tdhost', username='tduser', password =
'tdpassword', logmech='TDNEGO')
```
#### Example 5: Create a context with default logmech 'LDAP'
```python
>>> from teradataml.context.context import *
>>> create_context(host = 'tdhost', username='tduser', password =
'tdpassword', logmech='LDAP')
```
#### Example 6: Create a context with default logmech 'KRB5'
```python
>>> from teradataml.context.context import *
>>> create_context(host = 'tdhost', logmech='KRB5')
```
#### Example 7: Create a context with default logmech 'JWT'
Note:
You must use the 'logdata' argument when using 'JWT' as logon mechanism.
```python
>>> from teradataml.context.context import *
>>> create_context(host = 'tdhost', logmech='JWT', logdata='token=eyJpc...h8dA')
```
#### Example 8: Create a context using encrypted password and key passed to the
#### 'password' argument
Specify the password in the following format:
```python
ENCRYPTED_PASSWORD(file:PasswordEncryptionKeyFileName,
file:EncryptedPasswordFileName)
```
Where:
* PasswordEncryptionKeyFileName specifies the name of a file that contains the password encryption
  key and associated information
* EncryptedPasswordFileName specifies the name of a file that contains the encrypted password and
  associated information.
Each filename must be preceded by the 'file:' prefix. The PasswordEncryptionKeyFileName must be
separated from the EncryptedPasswordFileName by a single comma.
```python
>>> td_context = create_context(host = 'tdhost', username='tduser', password =
"ENCRYPTED_PASSWORD(file:PassKey.properties, file:EncPass.properties)")
```
#### Example 9: Create a context using encrypted password in LDAP
#### logon mechanism
```python
>>> td_context = create_context(host = 'tdhost',
username='tduser', password = "ENCRYPTED_PASSWORD(file:PassKey.properties,
file:EncPass.properties)", logmech='LDAP')
```
#### Example 10: Create a context to connect to a different initial database, using
#### hostname, username and password
This example creates a context using hostname, username and password, and connect to a different initial
database by setting the database argument.
```python
>>> td_context = create_context(host = 'tdhost', username='tduser', password =
'tdpassword', database = 'database_name')
```
#### Example 11: Creates a context to connect to a different initial database, using
#### already created sqlalchemy engine
This example creates a context using already created sqlalchemy engine, and connect to a different initial
database by setting the database argument.
```python
>>> from sqlalchemy import create_engine
>>> sqlalchemy_engine  = create_engine('teradatasql://'+ tduser +':' +
tdpassword + '@'+tdhost + '/?DATABASE=database_name')
>>> create_context(tdsqlengine = sqlalchemy_engine)
```
#### Example 12: Create a context to connect to a different initial database, using
#### 'LDAP' logmech
This example creates a context with 'LDAP' logmech, and connect to a different initial database by setting
the database argument.
```python
>>> td_context = create_context(host = 'tdhost', username='tduser', password =
'tdpassword', logmech='LDAP', database = 'database_name')
```
#### Example 13: Create a context using 'tera' mode
This example creates a context using 'tera' mode with log value set to 8 and lob_support disabled.
```python
>>> td_context = create_context(host = 'tdhost', username='tduser', password =
'tdpassword', tmode = 'tera', log = 8, lob_support = False)
```
#### Example 14: Create a context when password contains special characters
This example creates a context when password has special characters, and the example password
"alice@pass" is encoded by default.
```python
>>> td_context = create_context(host = 'tdhost', username='alice_pass', password
= 'alice@pass')
```
#### Example 15: Create a context with sql_timeout set to 3
```python
>>> td_context = create_context(host = 'tdhost', username = 'tduser', password =
'tdpassword', sql_timeout = 3)
```
### Create connection using config file
create_context() can establish the connection to Vantage using the configuration file.
#### Example 1
Create context from config file with name 'td_properties.cfg' available under current working directory.
The content of the config file must be in the following format:
```python
host=tdhost
username=tduser
password=tdpassword
temp_database_name=tdtemp_database_name
logmech=tdlogmech
logdata=tdlogdata
database=tddatabase
>>> td_context = create_context()
```
#### Example 2
Create context from config file with name 'td_config.cfg' which is saved under '/secrets/user' directory.
```python
>>> td_context = create_context(config_file='/secrets/user/td_config.cfg')
```
### Create a connection using environment variables
create_context() can establish connection to Vantage with the environment variables.
Note:
* Environment variables should always start with prefix "TD_". For example, to read "host" from
    environment variable, set the environment variable to "TD_HOST".
* You can specify any argument with the format specified in Create connection using config file and
    create_context() considers all these variables for establishing connection to Vantage.
#### Example
Set the following variables using os.environ, then run the example:
  os.environ['TD_HOST'] = 'tdhost'
  os.environ['TD_USERNAME'] = 'tduser'
  os.environ['TD_PASSWORD'] = 'tdpassword'
```python
>>> td_context = create_context()
```
### Order of preference between config file, environment variables
### and arguments of create_context
* create_context() first looks for arguments passed to the function.
* If no argument is passed, it then looks for environment variables which are prefixed with TD_
* If it does not find any environment variable with the mentioned prefix, then it looks for config file.
* If it did not find config file, an error is returned.
## get_context
The get_context function returns the Vantage engine associated with the current context.
#### Example
Create a context using username & password.
```python
>>> from teradataml.context.context import *
>>> from teradataml.dataframe.dataframe import DataFrame
>>> create_context(host = "myhostname", username="myusername", password
= "mypassword")
```
Get the connection engine.
```python
>>> get_context()
Engine(teradatasql://myusername:***@myhostname)
```
## get_connection
The get_connection function returns the Vantage connection associated with the current context.
#### Example
Create a context using username and password.
```python
>>> from teradataml.context.context import *
>>> from teradataml.dataframe.dataframe import DataFrame
>>> create_context(host = "myhostname", username="myusername", password
= "mypassword")
```
Get the connection object.
```python
>>> get_connection()
<sqlalchemy.engine.base.Connection at 0x258d86cd470>
```
## set_context
The set_context function specifies a Vantage sqlalchemy engine as current context.
Note:
* When overwriting an existing context associated with a Vantage connection, most of the
    operations on any teradataml DataFrames created before will not work.
* Running the set_context() triggers garbage collection in teradataml. For more details of Garbage
    Collection,see Garbage Collection in teradataml.
#### Example
The following example creates two contexts using different host, then sets the context to the first one.
```python
>>> from teradataml.context.context import *
>>> from teradataml.dataframe.dataframe import DataFrame
```
Create a connection:
```python
>>> td_connection1 = create_context(host = "myhostname", username="myusername",
password = "mypassword")
```
Overwrite the existing connection with new connection using 'create_context()':
```python
>>> td_connection2 = create_context(host = "myhostname2",
username="myusername2", password = "mypassword2")
302: UserWarning: [Teradata][teradataml](TDML_2002) Overwriting an existing
context associated with Teradata connection. Most of the operations on any
teradataml DataFrames created before this will not work.
warnings.warn(Messages.get_message(MessageCodes.OVERWRITE_CONTEXT))
```
Get the engine currently associated with the context:
```python
>>> get_context()
Engine(teradatasql://myusername2:***@myhostname2)
```
Overwrite the existing context, by passing a sqlalchemy engine object as input to 'set_context()', thereby
creating a new context:
```python
>>> set_context(tdsqlengine = td_connection1)
302: UserWarning: [Teradata][teradataml](TDML_2002) Overwriting an existing
context associated with Teradata connection. Most of the operations on any
teradataml DataFrames created before this will not work.
warnings.warn(Messages.get_message(MessageCodes.OVERWRITE_CONTEXT))
>>> get_context()
Engine(teradatasql://myusername:***@myhostname)
```
## remove_context
The remove_context function removes the current context associated with the Vantage connection.
remove_context() not only closes the connection but also garbage collects the intermediate views and
tables created by teradataml.
Teradata recommends calling remove_context() to end a session, so that intermediate views and tables
created by teradataml are garbage collected.
Note:
Executing remove_context() triggers garbage collection in teradataml. See Garbage Collection in
teradataml for more details.
#### Example
```python
>>> from teradataml.context.context import *
>>> from teradataml.dataframe.dataframe import DataFrame
>>> td_connection = create_context(host = "myhostname", username="myusername",
password = "mypassword")
>>> get_context()
Engine(teradatasql://myusername:***@myhostname)
>>> remove_context()
True
>>> get_context()
```