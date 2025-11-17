# teradataml Context Manager

create_context
teradataml.context.context.create_context = create_context(host=None, username=None, password=None, tdsqlengine=None, temp_database_name=None,
logmech=None, logdata=None, database=None, **kwargs)
    DESCRIPTION:
        Creates a connection to the Teradata Vantage using the teradatasql + teradatasqlalchemy DBAPI and dialect
        combination.
        Users can create a connection by passing the connection parameters using any one of the following methods:
            1. Pass all required parameters (host, username, password) directly to the function.
            2. Set the connection parameters in a configuration file (.cfg or .env) and
               pass the configuration file.
            3. Set the connection parameters in environment variables and create_context() reads from
               environment variables.
        Alternatively, users can pass a SQLAlchemy engine object to the `tdsqlengine` parameter to override the default DBAPI
        and dialect combination.
        Function also enables user to set the authentication token which is required to access services running
        on Teradata Vantage.
        Note:
            1. teradataml requires that the user has certain permissions on the user's default database or the initial
               default database specified using the database argument, or the temporary database when specified using
               temp_database_name. These permissions allow the user to:
                a. Create tables and views to save results of teradataml analytic functions.
                b. Create views in the background for results of DataFrame APIs such as assign(),
                   filter(), etc., whenever the result for these APIs are accessed using a print().
                c. Create view in the background on the query passed to the DataFrame.from_query() API.
               It is expected that the user has the correct permissions to create these objects in the database that
               will be used.
               The access to the views created may also require issuing additional GRANT SELECT ... WITH GRANT OPTION
               permission depending on which database is used and which object the view being created is based on.
            2. The temp_database_name and database parameters play a crucial role in determining which database
               is used by default to lookup for tables/views while creating teradataml DataFrame using 'DataFrame()'
               and 'DataFrame.from_table()' and which database is used to create all internal temporary objects.
               +------------------------------------------------------+---------------------------------------------+
               |                     Scenario                         |            teradataml behaviour             |
               +------------------------------------------------------+---------------------------------------------+
               | Both temp_database_name and database are provided    | Internal temporary objects are created in   |
               |                                                      | temp_database_name, and database table/view |
               |                                                      | lookup is done from database.               |
               +------------------------------------------------------+---------------------------------------------+
               | database is provided but temp_database_name is not   | Database table/view lookup and internal     |
               |                                                      | temporary objects are created in database.  |
               +------------------------------------------------------+---------------------------------------------+
               | temp_database_name is provided but database is not   | Internal temporary objects are created in   |
               |                                                      | temp_database_name, database table/view     |
               |                                                      | lookup from the users default database.     |
               +------------------------------------------------------+---------------------------------------------+
               | Neither temp_database_name nor database are provided | Database table/view lookup and internal     |
               |                                                      | temporary objects are created in users      |
               |                                                      | default database.                           |
               +------------------------------------------------------+---------------------------------------------+
            3. The function prioritizes parameters in the following order:
                1. Explicitly passed arguments (host, username, password).
                2. Environment variables (TD_HOST, TD_USERNAME, TD_PASSWORD, etc.).
                   Note:
                       * The environment variables should start with 'TD_' and all must be in upper case.
                         Example:
                                os.environ['TD_HOST'] = 'tdhost'
                                os.environ['TD_USERNAME'] = 'tduser'
                                os.environ['TD_PASSWORD'] = 'tdpassword'
                3. A configuration file if provided (such as a .env file).
            4. Points to note when user sets authentication token with create_context():
                * The username provided in create_context() is not case-sensitive. For example,
                  if a user is created with the username xyz, create_context() still establishes
                  a connection if user passes the username as XyZ. However, authentication token
                  generation requires the username to be in the same case as when it was created.
                  Therefore, Teradata recommends to pass the username with the same case as when
                  it was created.
                * User must have a privilege to login with a NULL password to use set_auth_token().
                  Refer to GRANT LOGON section in Teradata Documentation for more details.
                * When "auth_mech" is not specified, arguments are used in the following combination
                  to derive authentication mechanism.
                    * If "base_url" and "client_id" are specified then token generation is done through OAuth.
                    * If "base_url", "pat_token", "pem_file" are specified then token generation is done using PAT.
                    * If "base_url" and "auth_token" are specified then value provided for "auth_token" is used.
                    * If only "base_url" is specified then token generation is done through OAuth.
                * If Basic authentication mechanism is to be used then user must specify argument
                  "auth_mech" as "BASIC" along with "username" and "password".
                * Refresh token works only for OAuth authentication.
                * Use the argument "kid" only when key used during the pem file generation is different
                  from pem file name. For example, if you use the key as 'key1' while generating pem file
                  and the name of the pem file is `key1(1).pem`, then pass value 'key1' to the argument "kid".
    PARAMETERS:
        host:
            Optional Argument.
            Specifies the fully qualified domain name or IP address of the Teradata System.
            Types: str
        username:
            Optional Argument.
            Specifies the username for logging onto the Teradata Vantage.
            Types: str
        password:
            Optional Argument.
            Specifies the password required for the "username".
            Types: str
            Note:
                * Encrypted passwords can also be passed to this argument, using Stored Password Protection feature.
                  Examples section below demonstrates passing encrypted password to 'create_context'.
                  More details on Stored Password Protection and how to generate key and encrypted password file
                  can be found at https://pypi.org/project/teradatasql/#StoredPasswordProtection
                * Special characters used in the password are encoded by default.
        tdsqlengine:
            Optional Argument.
            Specifies Teradata Vantage sqlalchemy engine object that should be used to establish a Teradata Vantage
            connection.
            Types: str
        temp_database_name:
            Optional Argument.
            Specifies the temporary database name where temporary tables, views will be created.
            Types: str
        logmech:
            Optional Argument.
            Specifies the type of logon mechanism to establish a connection to Teradata Vantage. 
            Permitted Values: As supported by the teradata driver.
            Notes:
                1. teradataml expects the client environments are already setup with appropriate
                   security mechanisms and are in working conditions.
                2. User must have a valid ticket-granting ticket in order to use KRB5 (Kerberos) logon mechanism.
                3. User must use logdata parameter when using 'JWT' as the logon mechanism.
                4. Browser Authentication is supported for Windows and macOS.
                For more information please refer Teradata Vantage™ - Advanced SQL Engine
                Security Administration at https://www.info.teradata.com/
            Types: str
        logdata:
            Optional Argument.
            Specifies parameters to the LOGMECH command beyond those needed by the logon mechanism, such as 
            user ID, password and tokens (in case of JWT) to successfully authenticate the user.
            Types: str
        database:
            Optional Argument.
            Specifies the initial database to use after logon, instead of the user's default database.
            Types: str
        kwargs:
            Specifies optional keyword arguments accepted by create_context().
            Below are the supported keyword arguments:
            Connection parameters for Teradata SQL Driver:
                Specifies the keyword-value pairs of connection parameters that are passed to Teradata SQL Driver for
                Python. Please refer to https://github.com/Teradata/python-driver#ConnectionParameters to get information
                on connection parameters of the driver.
                Note:
                    * When type of a connection parameter is integer or boolean (eg: log, lob_support etc,.), pass
                      integer or boolean value, instead of quoted integer or quoted boolean as suggested in the
                      documentation. Please check the examples for usage.
                    * "sql_timeout" represents "request_timeout" connection parameter.
            config_file:
                Specifies the name of the configuration file to read the connection parameters.
                Notes:
                    * If user does not specify full path of file, then file look up is done at current working directory.
                    * The content of the file must be in '.env' format.
                    * Use parameters of create_context() as key in the configuration file.
                      Example:
                          host=tdhost
                          username=tduser
                          password=tdpassword
                          temp_database_name=tdtemp_database_name
                          logmech=tdlogmech
                          logdata=tdlogdata
                          database=tddatabase
                    * For more information please refer examples section.
                Default Value : td_properties.cfg
                Types: str
            base_url:
                Specifies the endpoint URL for a given environment on Teradata Vantage.
                Types: str
            client_id:
                Specifies the id of the application that requests the access token from
                VantageCloud Lake.
                Types: str
            pat_token:
                Specifies the PAT token generated from VantageCloud Lake Console.
                Types: str
            pem_file:
                Specifies the path to private key file which is generated from VantageCloud Lake Console.
                Types: str
            auth_token:
                Specifies the authentication token required to access services running
                on Teradata Vantage.
            expiration_time:
                Specifies the expiration time of the token in seconds. After expiry time JWT token expires and
                UserEnv methods does not work, user should regenerate the token.
                Note:
                    This option is used only for PAT and not for OAuth.
                Default Value: 31536000
                Types: int
            kid:
                Specifies the name of the key which is used while generating 'pem_file'.
                Note:
                    * Use the argument "kid" only when key used during the pem file generation is different
                      from pem file name. For example, if you use the key as 'key1' while generating pem file
                      and the name of the pem file is `key1(1).pem`, then pass value 'key1' to the argument "kid".
                Types: str
            auth_mech:
                Specifies the mechanism to be used for generating authentication token.
                Notes:
                    * User must use this argument if Basic authentication is to be used.
                    * When "auth_mech" is provided, other arguments are used in the following
                      combination as per value of "auth_mech":
                        * OAuth: Token generation is done through OAuth by using client id
                                 which can be sepcified by user in "client_id" argument or
                                 can be derived internally from "base_url".
                        * PAT  : Token generation is done using "pat_token" and "pem_file".
                        * BASIC: Authentication is done via Basic authentication mechanism
                                 using user credentials passed in "username" and "password"
                                 arguments.
                        * JWT  : Readily available token in "auth_token" argument is used.
                Permitted Values: "OAuth", "PAT", "BASIC", "JWT".
                Types: str
    RETURNS:
        A Teradata sqlalchemy engine object.
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> from teradataml.context.context import *
        # Example 1: Create context using hostname, username and password
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword')
        # Example 2: Create context using already created sqlalchemy engine
        >>> from sqlalchemy import create_engine
        >>> sqlalchemy_engine  = create_engine('teradatasql://'+ tduser +':' + tdpassword + '@'+tdhost)
        >>> td_context = create_context(tdsqlengine = sqlalchemy_engine)
        # Example 3: Creating context for Vantage with default logmech 'TD2'
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword', logmech = 'TD2')
       # Example 4: Creating context for Vantage with logmech as 'TDNEGO'
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword', logmech = 'TDNEGO')
        # Example 5: Creating context for Vantage with logmech as 'LDAP'
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword', logmech = 'LDAP')
        # Example 6: Creating context for Vantage with logmech as 'KRB5'
        >>> td_context = create_context(host = 'tdhost', logmech = 'KRB5')
        # Example 7: Creating context for Vantage with logmech as 'JWT'
        >>> td_context = create_context(host = 'tdhost', logmech = 'JWT', logdata = 'token=eyJpc...h8dA')
        # Example 8: Create context using encrypted password and key passed to 'password' parameter.
        #            The password should be specified in the format mentioned below:
        #            ENCRYPTED_PASSWORD(file:<PasswordEncryptionKeyFileName>, file:<EncryptedPasswordFileName>)
        #            The PasswordEncryptionKeyFileName specifies the name of a file that contains the password encryption key
        #            and associated information.
        #            The EncryptedPasswordFileName specifies the name of a file that contains the encrypted password and
        #            associated information.
        #            Each filename must be preceded by the 'file:' prefix. The PasswordEncryptionKeyFileName must be separated
        #            from the EncryptedPasswordFileName by a single comma.
        >>> encrypted_password = "ENCRYPTED_PASSWORD(file:PassKey.properties, file:EncPass.properties)"
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = encrypted_password)
        # Example 9: Create context using encrypted password in LDAP logon mechanism.
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = encrypted_password,
        ...                             logmech = 'LDAP')
       # Example 10: Create context using hostname, username, password and database parameters, and connect to a
       # different initial database by setting the database parameter.
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword', database =
        ...                            'database_name')
        # Example 11: Create context using already created sqlalchemy engine, and connect to a different initial
        # database by setting the database parameter.
        >>> from sqlalchemy import create_engine
        >>> sqlalchemy_engine = create_engine('teradatasql://'+ tduser +':' + tdpassword + '@'+tdhost +
        ...                                   '/?DATABASE=database_name')
        >>> td_context = create_context(tdsqlengine = sqlalchemy_engine)
        # Example 12: Create context for Vantage with logmech as 'LDAP', and connect to a different initial
        # database by setting the database parameter.
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword', logmech = 'LDAP',
        ...                             database = 'database_name')
        # Example 13: Create context using 'tera' mode with log value set to 8 and lob_support disabled.
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword', tmode = 'tera',
        ...                             log = 8, lob_support = False)
        # Example 14: Create context from config file with name 'td_properties.cfg'
        #             available under current working directory.
        #             td_properties.cfg content:
        #                 host=tdhost
        #                 username=tduser
        #                 password=tdpassword
        #                 temp_database_name=tdtemp_database_name
        #                 logmech=tdlogmech
        #                 logdata=tdlogdata
        #                 database=tddatabase
        >>> td_context = create_context()
        # Example 15: Create context using the file specified in user's home directory
        #             with name user_td_properties.cfg.
        #             user_td_properties.cfg content:
        #                 host=tdhost
        #                 username=tduser
        #                 password=tdpassword
        #                 temp_database_name=tdtemp_database_name
        #                 logmech=tdlogmech
        #                 logdata=tdlogdata
        #                 database=tddatabase
        >>> td_context = create_context(config_file = "user_td_properties.cfg")
        # Example 16: Create context using environment variables.
        #             Set these using os.environ and then run the example:
        #                  os.environ['TD_HOST'] = 'tdhost'
        #                  os.environ['TD_USERNAME'] = 'tduser'
        #                  os.environ['TD_PASSWORD'] = 'tdpassword'
        #                  os.environ['TD_TEMP_DATABASE_NAME'] = 'tdtemp_database_name'
        #                  os.environ['TD_LOGMECH'] = 'tdlogmech'
        #                  os.environ['TD_LOGDATA'] = 'tdlogdata'
        #                  os.environ['TD_DATABASE'] = 'tddatabase'
        >>> td_context = create_context()
        # Example 17: Create a context by providing username and password. Along with it,
        #             set authentication token by providing the pem file and pat token.
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword',
        ...                             base_url = 'base_url', pat_token = 'pat_token', pem_file = 'pem_file')
        # Example 18: Create a context by providing username and password. Along with it,
        #             generate authentication token by providing the client id.
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword',
        ...                             base_url = 'base_url', client_id = 'client_id')
        # Example 19: Create context and set authentication token by providing all the details in a config file.
        #             td_properties.cfg content:
        #                 host=tdhost
        #                 username=tduser
        #                 password=tdpassword
        #                 base_url=base_url
        #                 pat_token=pat_token
        #                 pem_file=pem_file
        >>> td_context = create_context()
        # Example 20: Create context and set authentication token by providing all the details in environment variables.
        #             Set these using os.environ and then run the example:
        #                  os.environ['TD_HOST'] = 'tdhost'
        #                  os.environ['TD_USERNAME'] = 'tduser'
        #                  os.environ['TD_PASSWORD'] = 'tdpassword'
        #                  os.environ['TD_BASE_URL'] = 'base_url'
        #                  os.environ['TD_PAT_TOKEN'] = 'pat_token'
        #                  os.environ['TD_PEM_FILE'] = 'pem_file'
        >>> td_context = create_context()
        # Example 21: Create context with sql_timeout set to 3.
        >>> td_context = create_context(host = 'tdhost', username = 'tduser', password = 'tdpassword', sql_timeout = 3)
        # Example 22: Create context and set authentication token via Basic authentication mechanism
        #             using username and password.
        >>> import getpass
        >>> create_context(host="host",
        ...                username=getpass.getpass("username : "),
        ...                password=getpass.getpass("password : "),
        ...                base_url=getpass.getpass("base_url : "),
        ...                auth_mech="BASIC")
        # Example 23: Create context and set authentication token by providing auth_mech argument.
        >>> import getpass
        >>> create_context(host="vcl_host",
        ...                username=getpass.getpass("username : "),
        ...                password=getpass.getpass("password : "),
        ...                base_url=getpass.getpass("base_url : "),
        ...                auth_mech="OAuth")
    get_context
    teradataml.context.context.get_context = get_context()
    DESCRIPTION:
        Returns the Teradata Vantage connection associated with the current context.
    PARAMETERS:
        None
    RETURNS:
        A Teradata sqlalchemy engine object.
    RAISES:
        None.
    EXAMPLES:
        td_sqlalchemy_engine = get_context()
    get_connection
    teradataml.context.context.get_connection = get_connection()
    DESCRIPTION:
        Returns the Teradata Vantage connection associated with the current context.
    PARAMETERS:
        None
    RETURNS:
        A Teradata dbapi connection object.
    RAISES:
        None.
    EXAMPLES:
        tdconnection = get_connection()
    set_context
    teradataml.context.context.set_context = set_context(tdsqlengine, temp_database_name=None)
    DESCRIPTION:
        Specifies a Teradata Vantage sqlalchemy engine as current context.
    PARAMETERS:
        tdsqlengine:
            Required Argument.
            Specifies Teradata Vantage sqlalchemy engine object that should be used to establish a Teradata Vantage connection.
            Types: str
        temp_database_name:
            Optional Argument.
            Specifies the temporary database name where temporary tables, views will be created.
            Types: str
    RETURNS:
        A Teradata Vantage connection object.
    RAISES:
        TeradataMlException
    EXAMPLES:
        set_context(tdsqlengine = td_sqlalchemy_engine)
    remove_context
    teradataml.context.context.remove_context = remove_context()
    DESCRIPTION:
        Removes the current context associated with the Teradata Vantage connection.
    PARAMETERS:
        None.
    RETURNS:
        None.
    RAISES:
        None.
    EXAMPLES:
        remove_context()