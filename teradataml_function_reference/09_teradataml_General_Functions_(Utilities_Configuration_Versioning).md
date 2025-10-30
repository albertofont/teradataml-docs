# teradataml General Functions (Utilities, Configuration, Versioning)

print_options
teradataml.utils.print_versions.print_options = print_options()
DESCRIPTION:
    Displays both configure and display options set in the current user's session.
PARAMETERS:
    None.
RETURNS:
    None.
RAISES:
     Warnings
EXAMPLES:
    >>> import teradataml as tdml
    >>> tdml.print_options()
    Display Options
    ------------------
    display.max_rows = 10
    display.precision = 3
    display.byte_encoding = base16
    display.print_sqlmr_query = False
    display.suppress_vantage_runtime_warnings = False
    Configure Options
    ------------------
    configure.default_varchar_size = 1024
    configure.column_casesensitive_handler = False
    configure.vantage_version = vantage2.0
    configure.val_install_location = None
show_versions
teradataml.utils.print_versions.show_versions = show_versions()
Displays client information, version information for client package 
dependencies and server information.
PARAMETERS:
    None.
RETURNS:
    None.
RAISES:
    ImportError, Warnings
EXAMPLES:
    >>> import teradataml as tdml
    >>> tdml.show_versions()
    >>> INSTALLED VERSIONS: Client
        --------------------------
        python: 3.5.4.final.0
        python-bits: 64
        OS: Windows
        OS-release: 10
        machine: AMD64
        processor: Intel64 Family 6 Model 142 Stepping 10, GenuineIntel
        byteorder: little
        LC_ALL: None
        LANG: en
        LOCALE: None.None
        pandas: 0.24.2
        sqlalchemy: 1.3.2
        teradatasqlalchemy: 16.20.0.5
        teradatasql: 16.20.0.41
        teradataml: 16.20.0.4
        INSTALLED VERSIONS: Server
        --------------------------
        BUILD_VERSION: 08.10.00.00-e84ce5f7
        RELEASE: Vantage 1.1 GA
view_log
teradataml.dbutils.dbutils.view_log = view_log(log_type='script', num_lines=1000, query_id=None, log_dir=None)
DESCRIPTION:
    Function for viewing script, apply or byom log on Vantage.
    Logs are pulled from 'script_log' or 'byom.log' file on database node.
    When log_type is "script", logs are pulled from 'scriptlog' file on database node.
    This is useful when Script.execute() is executed to run user scripts in Vantage.
    When log_type is set to "apply", function downloads the log files to a folder.
    Notes:
        * Logs files will be downloaded based on "log_dir".
        * teradataml creates a sub directory with the name as "query_id"
          and downloads the logs to the sub directory.
        * files generated from "query_id" requires few seconds to generate,
          provide "query_id" to function view_log() after few seconds else
          it will return empty sub directory.
PARAMETERS:
    log_type:
        Optional Argument.
        Specifies which logs to view.
        If set to 'script', script log is pulled from database node.
        If set to 'byom', byom log is pulled from database node.
        If set to 'apply' logs are pulled from kubernetes container.
        Permitted Values: 'script', 'apply', 'byom'
        Default Value: 'script'
        Types: str
    num_lines:
        Optional Argument.
        Specifies the number of lines to be read and displayed from log.
        Note:
            This argument is applicable when log_type is 'script' otherwise ignored.
        Default Value: 1000
        Types: int
    query_id:
        Required Argument when log_type is 'apply', otherwise ignored.
        Specifies the id of the query for which logs are to be retrieved.
        This query id is part of the error message received when Apply class
        or Dataframe apply method calls fail to execute the Apply table operator
        query.
        Types: str
    log_dir:
        Optional Argument.
        Specifies the directory path to store all the log files for "query_id".
        Notes:
            * This argument is applicable when log_type is 'apply' otherwise ignored.
            * when "log_dir" is not provided, function creates temporary folder
              and store the log files in the temp folder.
        Types: str
RETURNS:
    when log_type="apply" returns log files, otherwise teradataml dataframe.
RAISES:
    TeradataMLException.
EXAMPLES:
    # Example 1: View script log.
    >>> view_log(log_type="script", num_lines=200)
    >>> view_log(log_type="byom", num_lines=200)
    # Example 2: Download the Apply query logs to a default temp folder.
    # Use query id from the error messages returned by Apply class.
    >>> view_log(log_type="apply", query_id='307161028465226056')
    Logs for query_id "307191028465562578" is stored at "C:\local_repo\AppData\Local\Temp\tmp00kuxlgu\307161028465226056"
    # Example 3: Download the Apply query logs to a specific folder.
    # Use query id from the error messages returned by Apply class.
    >>> view_log(log_type="apply", query_id='307161028465226056',log_dir='C:\local_repo\workspace')
    Logs for query_id "307191028465562578" is stored at "C:\local_repo\workspace\307161028465226056"
in_schema
teradataml.dataframe.dataframe.in_schema.__init__ = __init__(self, schema_name, table_name, datalake_name=None)
Constructor for in_schema class.
PARAMETERS:
    schema_name:
        Required Argument.
        Specifies the schema where the table resides in.
        Types: str
    table_name:
        Required Argument.
        Specifies the table name or view name in Vantage.
        Types: str
    datalake_name:
        Optional Argument.
        Specifies the datalake name.
        Types: str
EXAMPLES:
    from teradataml.dataframe.dataframe import in_schema, DataFrame
    # Example 1: The following example creates a DataFrame from the
    #            existing Vantage table "dbcinfo" in the non-default
    #            database "dbc" using the in_schema instance.
    df = DataFrame(in_schema("dbc", "dbcinfo"))
    # Example 2: The following example uses from_table() function, existing
    #            Vantage table "dbcinfo" and non-default database "dbc" to
    #            create a teradataml DataFrame.
    df = DataFrame.from_table(in_schema("dbc","dbcinfo"))
    # Example 3: The following example uses "in_schema" object created
    #            with "datalake_name" argument to create DataFrame on OTF table.
    otf_df = DataFrame(in_schema("datalake_db","datalake_table","datalake"))
Database Transfer Utility Functions
copy_to_sql
teradataml.dataframe.copy_to.copy_to_sql = copy_to_sql(df, table_name, schema_name=None, if_exists='append', index=False, index_label=None, primary_index=None,
temporary=False, types=None, primary_time_index_name=None, timecode_column=None, timebucket_duration=None, timezero_date=None, columns_list=None,
sequence_column=None, seq_max=None, set_table=False, chunksize=16383, match_column_order=True)
Writes records stored in a Pandas DataFrame or a teradataml DataFrame to Teradata Vantage.
PARAMETERS:
    df:
        Required Argument. 
        Specifies the Pandas or teradataml DataFrame object to be saved.
        Types: pandas.DataFrame or teradataml.dataframe.dataframe.DataFrame
    table_name:
        Required Argument.
        Specifies the name of the table to be created in Vantage.
        Types : String
    schema_name:
        Optional Argument. 
        Specifies the name of the SQL schema in Teradata Vantage to write to.
        Types: String
        Default: None (Uses default database schema).
        Note: schema_name will be ignored when temporary=True.
    if_exists:
        Optional Argument.
        Specifies the action to take when table already exists in Vantage.
        Types: String
        Possible values: {'fail', 'replace', 'append'}
            - fail: If table exists, do nothing.
            - replace: If table exists, drop it, recreate it, and insert data.
            - append: If table exists, insert data. Create if does not exist.
        Default : append
        Note: Replacing a table with the contents of a teradataml DataFrame based on
              the same underlying table is not supported.
    index:
        Optional Argument.
        Specifies whether to save Pandas DataFrame index as a column or not.
        Types : Boolean (True or False)
        Default : False
        Note: Only use as True when attempting to save Pandas DataFrames (and not with teradataml DataFrames).
    index_label: 
        Optional Argument.
        Specifies the column label(s) for Pandas DataFrame index column(s).
        Types : String or list of strings
        Default : None
        Note: If index_label is not specified (defaulted to None or is empty) and `index` is True, then
              the 'names' property of the DataFrames index is used as the label(s),
              and if that too is None or empty, then:
              1) a default label 'index_label' or 'level_0' (when 'index_label' is already taken) is used
                 when index is standard.
              2) default labels 'level_0', 'level_1', etc. are used when index is multi-level index.
              Only use as True when attempting to save Pandas DataFrames (and not on teradataml DataFrames).
    primary_index:
        Optional Argument.
        Specifies which column(s) to use as primary index while creating Teradata table(s) in Vantage.
        When None, No Primary Index Teradata tables are created.
        Types : String or list of strings
        Default : None
        Example:
            primary_index = 'my_primary_index'
            primary_index = ['my_primary_index1', 'my_primary_index2', 'my_primary_index3']
    temporary:
        Optional Argument.
        Specifies whether to creates Vantage tables as permanent or volatile.
        Types : Boolean (True or False)
        Default : False
        Note: When True:
              1. volatile Tables are created, and
              2. schema_name is ignored.
              When False, permanent tables are created.
    types 
        Optional Argument.
        Specifies required data-types for requested columns to be saved in Vantage.
        Types: Python dictionary ({column_name1: type_value1, ... column_nameN: type_valueN})
        Default: None
        Note:
            1. This argument accepts a dictionary of columns names and their required teradatasqlalchemy types
               as key-value pairs, allowing to specify a subset of the columns of a specific type.
               i)  When the input is a Pandas DataFrame:
                   - When only a subset of all columns are provided, the column types for the rest are assigned
                     appropriately.
                   - When types argument is not provided, the column types are assigned
                     as listed in the following table:
                     +---------------------------+-----------------------------------------+
                     |     Pandas/Numpy Type     |        teradatasqlalchemy Type          |
                     +---------------------------+-----------------------------------------+
                     | int32                     | INTEGER                                 |
                     +---------------------------+-----------------------------------------+
                     | int64                     | BIGINT                                  |
                     +---------------------------+-----------------------------------------+
                     | bool                      | BYTEINT                                 |
                     +---------------------------+-----------------------------------------+
                     | float32/float64           | FLOAT                                   |
                     +---------------------------+-----------------------------------------+
                     | datetime64/datetime64[ns] | TIMESTAMP                               |
                     +---------------------------+-----------------------------------------+
                     | datetime64[ns,<time_zone>]| TIMESTAMP(timezone=True)                |
                     +---------------------------+-----------------------------------------+
                     | Any other data type       | VARCHAR(configure.default_varchar_size) |
                     +---------------------------+-----------------------------------------+
               ii) When the input is a teradataml DataFrame:
                   - When only a subset of all columns are provided, the column types for the rest are retained.
                   - When types argument is not provided, the column types are retained.
            2. This argument does not have any effect when the table specified using table_name and schema_name
               exists and if_exists = 'append'.
    primary_time_index_name:
        Optional Argument.
        Specifies a name for the Primary Time Index (PTI) when the table
        to be created must be a PTI table.
        Type: String
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    timecode_column:
        Optional argument.
        Required when the DataFrame must be saved as a PTI table.
        Specifies the column in the DataFrame that reflects the form
        of the timestamp data in the time series.
        This column will be the TD_TIMECODE column in the table created.
        It should be of SQL type TIMESTAMP(n), TIMESTAMP(n) WITH TIMEZONE, or DATE,
        corresponding to Python types datetime.datetime or datetime.date, or Pandas dtype datetime64[ns].
        Type: String
        Note: When you specify this parameter, an attempt to create a PTI table
              will be made. This argument is not required when the table to be created
              is not a PTI table. If this argument is specified, primary_index will be ignored.
    timezero_date:
        Optional Argument.
        Used when the DataFrame must be saved as a PTI table.
        Specifies the earliest time series data that the PTI table will accept;
        a date that precedes the earliest date in the time series data.
        Value specified must be of the following format: DATE 'YYYY-MM-DD'
        Default Value: DATE '1970-01-01'.
        Type: String
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    timebucket_duration:
        Optional Argument.
        Required if columns_list is not specified or is None.
        Used when the DataFrame must be saved as a PTI table.
        Specifies a duration that serves to break up the time continuum in
        the time series data into discrete groups or buckets.
        Specified using the formal form time_unit(n), where n is a positive
        integer, and time_unit can be any of the following:
        CAL_YEARS, CAL_MONTHS, CAL_DAYS, WEEKS, DAYS, HOURS, MINUTES,
        SECONDS, MILLISECONDS, or MICROSECONDS.
        Type:  String
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    columns_list:
        Optional Argument.
        Used when the DataFrame must be saved as a PTI table.
        Required if timebucket_duration is not specified.
        A list of one or more PTI table column names.
        Type: String or list of Strings
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    sequence_column:
        Optional Argument.
        Used when the DataFrame must be saved as a PTI table.
        Specifies the column of type Integer containing the unique identifier for
        time series data readings when they are not unique in time.
        * When specified, implies SEQUENCED, meaning more than one reading from the same
          sensor may have the same timestamp.
          This column will be the TD_SEQNO column in the table created.
        * When not specified, implies NONSEQUENCED, meaning there is only one sensor reading
          per timestamp.
          This is the default.
        Type: str
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    seq_max:
        Optional Argument.
        Used when the DataFrame must be saved as a PTI table.
        Specifies the maximum number of sensor data rows that can have the
        same timestamp. Can be used when 'sequenced' is True.
        Accepted range:  1 - 2147483647.
        Default Value: 20000.
        Type: int
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    set_table:
        Optional Argument.
        Specifies a flag to determine whether to create a SET or a MULTISET table.
        When True, a SET table is created.
        When False, a MULTISET table is created.
        Default Value: False
        Type: boolean
        Note: 1. Specifying set_table=True also requires specifying primary_index or timecode_column.
              2. Creating SET table (set_table=True) may result in
                 a. an error if the source is a Pandas DataFrame having duplicate rows.
                 b. loss of duplicate rows if the source is a teradataml DataFrame.
              3. This argument has no effect if the table already exists and if_exists='append'.
    chunksize:
        Optional Argument.
        Specifies the number of rows to be loaded in a batch.
        Note:
            This is argument is used only when argument "df" is pandas DataFrame.
        Default Value: 16383
        Types: int
    match_column_order:
        Optional Argument.
        Specifies whether the order of the columns in existing table matches the order of
        the columns in the "df" or not. When set to False, the dataframe to be loaded can
        have any order and number of columns.
        Default Value: True
        Types: bool
RETURNS:
    None
RAISES:
    TeradataMlException
EXAMPLES:
    1. Saving a Pandas DataFrame:
        >>> from teradataml.dataframe.copy_to import copy_to_sql
        >>> from teradatasqlalchemy.types import *
        >>> df = {'emp_name': ['A1', 'A2', 'A3', 'A4'],
        ...       'emp_sage': [100, 200, 300, 400],
        ...       'emp_id': [133, 144, 155, 177],
        ...       'marks': [99.99, 97.32, 94.67, 91.00]
        ...    }
        >>> pandas_df = pd.DataFrame(df)
        a) Save a Pandas DataFrame using a dataframe & table name only:
        >>> copy_to_sql(df = pandas_df, table_name = 'my_table')
        b) Saving as a SET table
        >>> copy_to_sql(df = pandas_df, table_name = 'my_set_table', index=True,
                        primary_index='index_label', set_table=True)
        c) Save a Pandas DataFrame by specifying additional parameters:
        >>> copy_to_sql(df = pandas_df, table_name = 'my_table_2', schema_name = 'alice',
        ...             index = True, index_label = 'my_index_label', temporary = False,
        ...             primary_index = ['emp_id'], if_exists = 'append',
        ...             types = {'emp_name': VARCHAR, 'emp_sage':INTEGER,
        ...                      'emp_id': BIGINT, 'marks': DECIMAL})
        d) Saving with additional parameters as a SET table
        >>> copy_to_sql(df = pandas_df, table_name = 'my_table_3', schema_name = 'alice',
        ...             index = True, index_label = 'my_index_label', temporary = False,
        ...             primary_index = ['emp_id'], if_exists = 'append',
        ...             types = {'emp_name': VARCHAR, 'emp_sage':INTEGER,
        ...                       'emp_id': BIGINT, 'marks': DECIMAL},
        ...             set_table=True)
        e) Saving levels in index of type MultiIndex
        >>> pandas_df = pandas_df.set_index(['emp_id', 'emp_name'])
        >>> copy_to_sql(df = pandas_df, table_name = 'my_table_4', schema_name = 'alice',
        ...             index = True, index_label = ['index1', 'index2'], temporary = False,
        ...             primary_index = ['index1'], if_exists = 'replace')
        f) Save a Pandas DataFrame with VECTOR datatype:
        >>> import pandas as pd
        >>> VECTOR_data = {
        ...        'id': [10, 11, 12, 13],
        ...        'array_col': ['1,1', '2,2', '3,3', '4,4']
        ...        }
        >>> df = pd.DataFrame(VECTOR_data)
        >>> from teradatasqlalchemy import VECTOR
        >>> copy_to_sql(df=df, table_name='my_vector_table', types={'array_col': VECTOR})
    2. Saving a teradataml DataFrame:
        >>> from teradataml.dataframe.dataframe import DataFrame
        >>> from teradataml.dataframe.copy_to import copy_to_sql
        >>> from teradatasqlalchemy.types import *
        >>> from teradataml.data.load_example_data import load_example_data
        >>> # Load the data to run the example.
        >>> load_example_data("glm", "admissions_train")
        >>> # Create teradataml DataFrame(s)
        >>> df = DataFrame('admissions_train')
        >>> df2 = df.select(['gpa', 'masters'])
        a) Save a teradataml DataFrame by using only a table name:
        >>> df2.to_sql('my_tdml_table')
        b) Save a teradataml DataFrame by using additional parameters:
        >>> df2.to_sql(table_name = 'my_tdml_table', if_exists='append',
                       primary_index = ['gpa'], temporary=False, schema_name='alice')
        c) Alternatively, save a teradataml DataFrame by using copy_to_sql:
        >>> copy_to_sql(df2, 'my_tdml_table_2')
        d) Save a teradataml DataFrame by using copy_to_sql with additional parameters:
        >>> copy_to_sql(df = df2, table_name = 'my_tdml_table_3', schema_name = 'alice',
        ...             temporary = False, primary_index = None, if_exists = 'append',
        ...             types = {'masters': VARCHAR, 'gpa':INTEGER})
        e) Saving as a SET table
        >>> copy_to_sql(df = df2, table_name = 'my_tdml_set_table', schema_name = 'alice',
        ...             temporary = False, primary_index = ['gpa'], if_exists = 'append',
        ...             types = {'masters': VARCHAR, 'gpa':INTEGER}, set_table = True)
    3. Saving a teradataml DataFrame as a PTI table:
        >>> from teradataml.dataframe.dataframe import DataFrame
        >>> from teradataml.dataframe.copy_to import copy_to_sql
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("sessionize", "sessionize_table")
        >>> df3 = DataFrame('sessionize_table')
        a) Using copy_to_sql
        >>> copy_to_sql(df3, "test_copyto_pti",
        ...             timecode_column='clicktime',
        ...             columns_list='event')
        b) Alternatively, using DataFrame.to_sql
        >>> df3.to_sql(table_name = "test_copyto_pti_1",
        ...            timecode_column='clicktime',
        ...            columns_list='event')
        c) Saving as a SET table
        >>> copy_to_sql(df3, "test_copyto_pti_2",
        ...             timecode_column='clicktime',
        ...             columns_list='event',
        ...             set_table=True)
fastexport
teradataml.dataframe.data_transfer.fastexport = fastexport(df, export_to='pandas', index_column=None, catch_errors_warnings=False, csv_ﬁle=None, **kwargs)
DESCRIPTION:
    The fastexport() API exports teradataml DataFrame to Pandas DataFrame
    or CSV file using FastExport data transfer protocol.
    Note:
        1. Teradata recommends to use FastExport when number of rows in
           teradataml DataFrame are at least 100,000. To extract lesser rows
           ignore this function and go with regular to_pandas() or to_csv()
           function. FastExport opens multiple data transfer connections to the
           database.
        2. FastExport does not support all Teradata Database data types.
           For example, tables with BLOB and CLOB type columns cannot
           be extracted.
        3. FastExport cannot be used to extract data from a volatile or
           temporary table.
        4. For best efficiency, do not use DataFrame.groupby() and
           DataFrame.sort() with FastExport.
    For additional information about FastExport protocol through
    teradatasql driver, please refer to FASTEXPORT section of
    https://pypi.org/project/teradatasql/#FastExport driver documentation.
PARAMETERS:
    df:
        Required Argument.
        Specifies teradataml DataFrame that needs to be exported.
        Types: teradataml DataFrame
    export_to:
        Optional Argument.
        Specifies a value that notifies where to export the data.
        Permitted Values:
            * "pandas": Export data to a Pandas DataFrame.
            * "csv": Export data to a given CSV file.
        Default Value: "pandas"
        Types: String
    index_column:
        Optional Argument.
        Specifies column(s) to be used as index column for the converted object.
        Note:
            Applicable only when 'export_to' is set to "pandas".
        Default Value: None.
        Types: String OR list of Strings (str)
    catch_errors_warnings:
        Optional Argument.
        Specifies whether to catch errors/warnings(if any) raised by
        fastexport protocol while converting teradataml DataFrame.
        Notes :
            1.  When 'export_to' is set to "pandas" and 'catch_errors_warnings' is set to True,
                fastexport() returns a tuple containing:
                    a. Pandas DataFrame.
                    b. Errors(if any) in a list thrown by fastexport.
                    c. Warnings(if any) in a list thrown by fastexport.
                When set to False, prints the fastexport errors/warnings to the
                standard output, if there are any.
            2.  When 'export_to' is set to "csv" and 'catch_errors_warnings' is set to True,
                fastexport() returns a tuple containing:
                    a. Errors(if any) in a list thrown by fastexport.
                    b. Warnings(if any) in a list thrown by fastexport.
        Default Value: False
        Types: bool
    csv_file:
        Optional Argument.
        Specifies the name of CSV file to which data is to be exported.
        Note:
            This is required argument when 'export_to' is set to "csv".
        Types: String
    kwargs:
        Optional Argument.
        Specifies keyword arguments. Accepts following keyword arguments:
        sep:
            Optional Argument.
            Specifies a single character string used to separate fields in a CSV file.
            Default Value: ","
            Notes:
                1. "sep" cannot be line feed ('\n') or carriage return ('\r').
                2. "sep" should not be same as "quotechar".
                3. Length of "sep" argument should be 1.
            Types: String
        quotechar:
            Optional Argument.
            Specifies a single character string used to quote fields in a CSV file.
            Default Value: """
            Notes:
                1. "quotechar" cannot be line feed ('\n') or carriage return ('\r').
                2. "quotechar" should not be same as "sep".
                3. Length of "quotechar" argument should be 1.
            Types: String
        coerce_float:
            Optional Argument.
            Specifies whether to convert non-string, non-numeric objects to floating point.
            Note:
                For additional information about "coerce_float" please refer to:
                https://pandas.pydata.org/docs/reference/api/pandas.read_sql.html
        parse_dates:
            Optional Argument.
            Specifies columns to parse as dates.
            Note:
                For additional information about "parse_date" please refer to:
                https://pandas.pydata.org/docs/reference/api/pandas.read_sql.html
        open_sessions:
            Optional Argument.
            specifies the number of Teradata sessions to be opened for fastexport.
            Note:
                If "open_sessions" argument is not provided, the default value
                is the smaller of 8 or the number of AMPs avaialble.
                For additional information about number of Teradata data-transfer
                sessions opened during fastexport, please refer to:
                https://pypi.org/project/teradatasql/#FastExport
            Types: Integer
RETURNS:
    1.  When 'export_to' is set to "pandas" and "catch_errors_warnings" is set to True,
        then the function returns a tuple containing:
            a. Pandas DataFrame.
            b. Errors, if any, thrown by fastexport in a list of strings.
            c. Warnings, if any, thrown by fastexport in a list of strings.
        When 'export_to' is set to "pandas" and "catch_errors_warnings" is set to False,
        then the function returns a Pandas DataFrame.
    2.  When 'export_to' is set to "csv" and "catch_errors_warnings" is set to True,
        then the function returns a tuple containing:
            a. Errors, if any, thrown by fastexport in a list of strings.
            b. Warnings, if any, thrown by fastexport in a list of strings.
EXAMPLES:
    >>> from teradataml import *
    >>> load_example_data("dataframe", "admissions_train")
    >>> df = DataFrame("admissions_train")
    # Print dataframe.
    >>> df
          masters   gpa     stats programming admitted
       id
       13      no  4.00  Advanced      Novice        1
       26     yes  3.57  Advanced    Advanced        1
       5       no  3.44    Novice      Novice        0
       19     yes  1.98  Advanced    Advanced        0
       15     yes  4.00  Advanced    Advanced        1
       40     yes  3.95    Novice    Beginner        0
       7      yes  2.33    Novice      Novice        1
       22     yes  3.46    Novice    Beginner        0
       36      no  3.00  Advanced      Novice        0
       38     yes  2.65  Advanced    Beginner        1
    # Example 1: Export teradataml DataFrame df to Pandas DataFrame using
    #            fastexport().
    >>> fastexport(df)
        Errors: []
        Warnings: []
           masters   gpa     stats programming  admitted
        id
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        20     yes  3.90  Advanced    Advanced         1
        18     yes  3.81  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        25      no  3.96  Advanced    Advanced         1
        ...
    # Example 2: Export teradataml DataFrame df to Pandas DataFrame,
    #            set index column, coerce_float and catch errors/warnings thrown by
    #            fastexport.
    >>> pandas_df, err, warn = fastexport(df, index_column="gpa",
                                          catch_errors_warnings=True,
                                          coerce_float=True)
    # Print pandas DataFrame.
    >>> pandas_df
             id masters     stats programming  admitted
        gpa
        2.65  38     yes  Advanced    Beginner         1
        3.57  26     yes  Advanced    Advanced         1
        3.44   5      no    Novice      Novice         0
        1.87  24      no  Advanced      Novice         1
        3.70   3      no    Novice    Beginner         1
        3.95   1     yes  Beginner    Beginner         0
        3.90  20     yes  Advanced    Advanced         1
        3.81  18     yes  Advanced    Advanced         1
        3.60   8      no  Beginner    Advanced         1
        3.96  25      no  Advanced    Advanced         1
        3.76   2     yes  Beginner    Beginner         0
        3.83  17      no  Advanced    Advanced         1
        ...
    # Print errors list.
    >>> err
        []
    # Print warnings list.
    >>> warn
        []
    # Example 3: Following example exports teradataml DataFrame df
    #            to Pandas DataFrame using fastexport() by opening specified
    #            number of Teradata data-transfer sessions.
    >>> fastexport(df, open_sessions=2)
        Errors: []
        Warnings: []
           masters   gpa     stats programming  admitted
        id
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        20     yes  3.90  Advanced    Advanced         1
        18     yes  3.81  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        25      no  3.96  Advanced    Advanced         1
        ...
    # Example 4: Following example exports teradataml DataFrame df
    #            to a given CSV file using fastexport().
    >>> fastexport(df, export_to="csv", csv_file="Test.csv")
        Data is successfully exported into Test.csv
    # Example 5: Following example exports teradataml DataFrame df
    #            to a given CSV file using fastexport() by opening specified
    #            number of Teradata data-transfer sessions.
    >>> fastexport(df, export_to="csv", csv_file="Test_1.csv", open_sessions=2)
        Data is successfully exported into Test_1.csv
    # Example 6: Following example exports teradataml DataFrame df
    #            to a given CSV file using fastexport() and catch errors/warnings
    #            thrown by fastexport.
    >>> err, warn = fastexport(df, export_to="csv", catch_errors_warnings=True,
                               csv_file="Test_3.csv")
        Data is successfully exported into Test_3.csv
    # Print errors list.
    >>> err
        []
    # Print warnings list.
    >>> warn
        []
    # Example 7: Export teradataml DataFrame to CSV file with '|' as field separator
    #            and single quote(') as field quote character.
    >>> fastexport(df, export_to="csv", csv_file="Test_4.csv",  sep = "|", quotechar="'")
        Data is successfully exported into Test_4.csv
fastload
fastload.fastload = fastload(df, table_name, schema_name=None, if_exists='replace', index=False, index_label=None, primary_index=None, types=None, batch_size=None,
save_errors=False, open_sessions=None, err_tbl_1_sufﬁx=None, err_tbl_2_sufﬁx=None, err_tbl_name=None, warn_tbl_name=None, err_staging_db=None)
The fastload() API writes records from a Pandas DataFrame to Teradata Vantage 
using Fastload. FastLoad API can be used to quickly load large amounts of data
in an empty table on Vantage.
1. Teradata recommends to use this API when number rows in the Pandas DataFrame
   is greater than 100,000 to have better performance. To insert lesser rows, 
   please use copy_to_sql for optimized performance. The data is loaded in batches.
2. FastLoad API cannot load duplicate rows in the DataFrame if the table is a
   MULTISET table having primary index.
3. FastLoad API does not support all Teradata Advanced SQL Engine data types. 
   For example, target table having BLOB and CLOB data type columns cannot be
   loaded.
4. If there are any incorrect rows i.e. due to constraint violations, data type 
   conversion errors, etc., FastLoad protocol ignores those rows and inserts 
   all valid rows.
5. Rows in the DataFrame that failed to get inserted are categorized into errors 
   and warnings by FastLoad protocol and these errors and warnings are stored 
   into respective error and warning tables by FastLoad API.
6. fastload() creates 2 error tables when data is erroneous. These error tables are
   refered as ERR_1 and ERR_2 tables.
   * ERR_1 table is used to capture rows that violate the constraints or have format
     errors. It typically contains information about rows that could not be inserted
     into the target table due to data conversion errors, constraint violations, etc.
   * ERR_2 table is used to log any duplicate rows found during the load process and
     which are not loaded in target table, since fastLoad does not allow duplicate
     rows to be loaded into the target table.
7. When "save_errors" argument is set to True, ERR_1 and ERR_2 tables are presisted.
   The fully qualified names of ERR_1, ERR_2 and warning tables are shown once the
   fastload operation is complete.
8. If user wants both error and warnings information from pandas dataframe to be
   persisted rather than that from ERR_1 and ERR_2 tables, then "save_errors" should
   be set to True and "err_tbl_name" must be provided.
For additional information about FastLoad protocol through teradatasql driver, 
please refer the FASTLOAD section of https://pypi.org/project/teradatasql/#FastLoad
driver documentation for more information.
PARAMETERS:
    df:
        Required Argument. 
        Specifies the Pandas DataFrame object to be saved in Vantage.
        Types: pandas.DataFrame
    table_name:
        Required Argument.
        Specifies the name of the table to be created in Vantage.
        Types: String
    schema_name:
        Optional Argument. 
        Specifies the name of the database schema in Vantage to write to.
        Types: String
        Default: None (Uses default database schema).
    if_exists:
        Optional Argument.
        Specifies the action to take when table already exists in Vantage.
        Types: String
        Possible values: {'fail', 'replace', 'append'}
            - fail: If table exists, raise TeradataMlException.
            - replace: If table exists, drop it, recreate it, and insert data.
            - append: If table exists, insert data. Create if does not exist.
        Default: replace
    index:
        Optional Argument.
        Specifies whether to save Pandas DataFrame index as a column or not.
        Types: Boolean (True or False)
        Default: False
    index_label: 
        Optional Argument.
        Specifies the column label(s) for Pandas DataFrame index column(s).
        Types: String or list of strings
        Default: None
    primary_index:
        Optional Argument.
        Specifies which column(s) to use as primary index while creating table 
        in Vantage. When set to None, No Primary Index (NoPI) tables are created.
        Types: String or list of strings
        Default: None
        Example:
            primary_index = 'my_primary_index'
            primary_index = ['my_primary_index1', 'my_primary_index2', 'my_primary_index3']
    types: 
        Optional Argument.
        Specifies the data types for requested columns to be saved in Vantage.
        Types: Python dictionary ({column_name1: type_value1, ... column_nameN: type_valueN})
        Default: None
        Note:
            1. This argument accepts a dictionary of columns names and their required 
            teradatasqlalchemy types as key-value pairs, allowing to specify a subset
            of the columns of a specific type.
               i)  When only a subset of all columns are provided, the column types
                   for the rest are assigned appropriately.
               ii) When types argument is not provided, the column types are assigned
                   as listed in the following table:
                   +---------------------------+-----------------------------------------+
                   |     Pandas/Numpy Type     |        teradatasqlalchemy Type          |
                   +---------------------------+-----------------------------------------+
                   | int32                     | INTEGER                                 |
                   +---------------------------+-----------------------------------------+
                   | int64                     | BIGINT                                  |
                   +---------------------------+-----------------------------------------+
                   | bool                      | BYTEINT                                 |
                   +---------------------------+-----------------------------------------+
                   | float32/float64           | FLOAT                                   |
                   +---------------------------+-----------------------------------------+
                   | datetime64/datetime64[ns] | TIMESTAMP                               |
                   +---------------------------+-----------------------------------------+
                   | datetime64[ns,<time_zone>]| TIMESTAMP(timezone=True)                |
                   +---------------------------+-----------------------------------------+
                   | Any other data type       | VARCHAR(configure.default_varchar_size) |
                   +---------------------------+-----------------------------------------+
            2. This argument does not have any effect when the table specified using
               table_name and schema_name exists and if_exists = 'append'.
    batch_size:
        Optional Argument.
        Specifies the number of rows to be loaded in a batch. For better performance,
        recommended batch size is at least 100,000. batch_size must be a positive integer. 
        If this argument is None, there are two cases based on the number of 
        rows, say N in the dataframe 'df' as explained below:
        If N is greater than 100,000, the rows are divided into batches of 
        equal size with each batch having at least 100,000 rows (except the 
        last batch which might have more rows). If N is less than 100,000, the
        rows are inserted in one batch after notifying the user that insertion 
        happens with degradation of performance.
        If this argument is not None, the rows are inserted in batches of size 
        given in the argument, irrespective of the recommended batch size. 
        The last batch will have rows less than the batch size specified, if the 
        number of rows is not an integral multiples of the argument batch_size.
        Default Value: None
        Types: int            
    save_errors:
        Optional Argument.
        Specifies whether to persist the error/warning information in Vantage 
        or not.
        Notes:
           *  When "save_errors" is set to True, ERR_1 and ERR_2 tables are presisted.
             The fully qualified names of ERR_1, ERR_2 and warning table are returned
             in a dictionary containing keys named as "ERR_1_table", "ERR_2_table",
             "warnings_table" respectively.
           * When "save_errors" is set to True and "err_tbl_name" is also provided,
             "err_tbl_name" takes precedence and error information is persisted into
              a single table using pandas dataframe rather than in ERR_1 and ERR_2 tables.
           * When "save_errors" is set to False, errors and warnings information is
             not persisted as tables, but it is returned as pandas dataframes in a
             dictionary containing keys named as "errors_dataframe" and "warnings_dataframe"
             respectively.
        Default Value: False
        Types: bool
    open_sessions:
        Optional Argument.
        Specifies the number of Teradata data transfer sessions to be opened for fastload operation.
        Note : If "open_sessions" argument is not provided, the default value is the smaller of 8 or the
               number of AMPs available.
               For additional information about number of Teradata data-transfer
               sessions opened during fastload, please refer to:
               https://pypi.org/project/teradatasql/#FastLoad
        Default Value: None
        Types: int
    err_tbl_1_suffix:
        Optional Argument.
        Specifies the suffix for error table 1 created by fastload job.
        Default Value: "_ERR_1"
        Types: String
    err_tbl_2_suffix:
        Optional Argument.
        Specifies the suffix for error table 2 created by fastload job.
        Default Value: "_ERR_2"
        Types: String
    err_tbl_name:
        Optional Argument.
        Specifies the name for error table. This argument takes precedence
        over "save_errors" and saves error information in single table,
        rather than ERR_1 and ERR_2 error tables.
        Default value: "td_fl_<table_name>_err_<unique_id>" where table_name
                       is name of target/staging table and unique_id is logon
                       sequence number of fastload job.
        Types: String
    warn_tbl_name:
        Optional Argument.
        Specifies the name for warning table.
        Default value: "td_fl_<table_name>_warn_<unique_id>" where table_name
                       is name of target/staging table and unique_id is logon
                       sequence number of fastload job.
        Types: String
    err_staging_db:
        Optional Argument.
        Specifies the name of the database to be used for creating staging
        table and error/warning tables.
        Note:
            Current session user must have CREATE, DROP and INSERT table
            permissions on err_staging_db database.
        Types: String
RETURNS:
    A dict containing the following attributes:
        1. errors_dataframe: It is a Pandas DataFrame containing error messages
           thrown by fastload. DataFrame is empty if there are no errors or
           "save_errors" is set to True.
        2. warnings_dataframe: It is a Pandas DataFrame containing warning messages 
           thrown by fastload. DataFrame is empty if there are no warnings.
        3. errors_table: Fully qualified name of the table containing errors. It is
           an empty string (''), if argument "save_errors" is set to False.
        4. warnings_table: Fully qualified name of the table containing warnings. It is
           an empty string (''), if argument "save_errors" is set to False.
        5. ERR_1_table: Fully qualified name of the ERR 1 table created by fastload
           job. It is an empty string (''), if argument "save_errors" is set to False.
        6. ERR_2_table: Fully qualified name of the ERR 2 table created by fastload
           job. It is an empty string (''), if argument "save_errors" is set to False.
RAISES:
    TeradataMlException
EXAMPLES:
    Saving a Pandas DataFrame using Fastload:
    >>> from teradataml.dataframe.fastload import fastload
    >>> from teradatasqlalchemy.types import *
    >>> df = {'emp_name': ['A1', 'A2', 'A3', 'A4'],
              'emp_sage': [100, 200, 300, 400],
              'emp_id': [133, 144, 155, 177],
              'marks': [99.99, 97.32, 94.67, 91.00]
              }
    >>> pandas_df = pd.DataFrame(df)
    # Example 1: Default execution.
    >>> fastload(df = pandas_df, table_name = 'my_table')
    # Example 2: Save a Pandas DataFrame with primary_index.
    >>> pandas_df = pandas_df.set_index(['emp_id'])
    >>> fastload(df = pandas_df, table_name = 'my_table_1', primary_index='emp_id')
    # Example 3: Save a Pandas DataFrame using fastload() with index and primary_index.
    >>> fastload(df = pandas_df, table_name = 'my_table_2', index=True,
                 primary_index='index_label')
    # Example 4: Save a Pandas DataFrame using types, appending to the table if it already exists.
    >>> fastload(df = pandas_df, table_name = 'my_table_3', schema_name = 'alice',
                 index = True, index_label = 'my_index_label',
                 primary_index = ['emp_id'], if_exists = 'append',
                 types = {'emp_name': VARCHAR, 'emp_sage':INTEGER,
                'emp_id': BIGINT, 'marks': DECIMAL})
    # Example 5: Save a Pandas DataFrame using levels in index of type MultiIndex.
    >>> pandas_df = pandas_df.set_index(['emp_id', 'emp_name'])
    >>> fastload(df = pandas_df, table_name = 'my_table_4', schema_name = 'alice',
                 index = True, index_label = ['index1', 'index2'],
                 primary_index = ['index1'], if_exists = 'replace')
    # Example 6: Save a Pandas DataFrame by opening specified number of teradata data transfer sessions.
    >>> fastload(df = pandas_df, table_name = 'my_table_5', open_sessions = 2)
    # Example 7: Save a Pandas Dataframe to a table in specified target database "schema_name".
    #            Save errors and warnings to database specified with "err_staging_db".
    #            Save errors to table named as "err_tbl_name" and warnings to "warn_tbl_name".
    #            Given that, user is connected to a database different from "schema_name"
    #            and "err_staging_db".
    # Create a pandas dataframe having one duplicate and one fualty row.
    >>>> data_dict = {"C_ID": [301, 301, 302, 303, 304, 305, 306, 307, 308],
                     "C_timestamp": ['2014-01-06 09:01:25', '2014-01-06 09:01:25',
                                     '2015-01-06 09:01:25.25.122200', '2017-01-06 09:01:25.11111',
                                     '2013-01-06 09:01:25', '2019-03-06 10:15:28',
                                     '2014-01-06 09:01:25.1098', '2014-03-06 10:01:02',
                                     '2014-03-06 10:01:20.0000']}
    >>> my_df = pd.DataFrame(data_dict)
    # Fastlaod data in non-default schema "target_db" and save erors and warnings in given tables.
    >>> fastload(df=my_df, table_name='fastload_with_err_warn_tbl_stag_db',
                if_exists='replace', primary_index='C_ID', schema_name='target_db',
                types={'C_ID': INTEGER, 'C_timestamp': TIMESTAMP(6)},
                err_tbl_name='fld_errors', warn_tbl_name='fld_warnings',
                err_staging_db='stage_db')
    Processed 9 rows in batch 1.
    {'errors_dataframe':    batch_no                                      error_message
    0         1   [Session 14527] [Teradata Database] [Error 26...,
    'warnings_dataframe':         batch_no                                      error_message
    0  batch_summary   [Session 14526] [Teradata SQL Driver] [Warnin...,
    'errors_table': 'stage_db.fld_errors',
    'warnings_table': 'stage_db.fld_warnings',
    'ERR_1_table': '',
    'ERR_2_table': ''}
    # Validate loaded data table.
    >>>  DataFrame(in_schema("target_db", "fastload_with_err_warn_tbl_stag_db"))
    C_ID    C_timestamp
    303     2017-01-06 09:01:25.111110
    306     2014-01-06 09:01:25.109800
    304     2013-01-06 09:01:25.000000
    307     2014-03-06 10:01:02.000000
    305     2019-03-06 10:15:28.000000
    301     2014-01-06 09:01:25.000000
    308     2014-03-06 10:01:20.000000
    # Validate error and warning tables.
    >>> DataFrame(in_schema("stage_db", "fld_errors"))
    batch_no      error_message
    1             [Session 14527] [Teradata Database] [Error 2673] FastLoad failed to insert 1 of 9 batched rows. Batched row 3
    >>> DataFrame(in_schema("stage_db", "fld_warnings"))
    batch_no        error_message
    batch_summary   [Session 14526] [Teradata SQL Driver] [Warning 518] Found 1 duplicate or faulty row(s) while ending FastLoa
    # Example 8: Save a Pandas Dataframe to a table in specified target database "schema_name".
    #            Save errors in ERR_1 and ERR_2 tables having user defined suffixes provided
    #            in "err_tbl_1_suffix" and "err_tbl_2_suffix".
    #            Source Pandas dataframe is same as Example 7.
    >>> fastload(df=my_df, table_name = 'fastload_with_err_warn_tbl_stag_db',
                 schema_name = 'target_db', if_exists = 'append',
                 types={'C_ID': INTEGER, 'C_timestamp': TIMESTAMP(6)},
                 err_staging_db='stage_db', save_errors=True,
                 err_tbl_1_suffix="_user_err_1", err_tbl_2_suffix="_user_err_2")
    {'errors_dataframe': Empty DataFrame
     Columns: []
     Index: [],
     'warnings_dataframe':         batch_no                                      error_message
     0  batch_summary   [Session 14699] [Teradata SQL Driver] [Warnin...,
     'errors_table': '',
     'warnings_table': 'stage_db.td_fl_fastload_with_err_warn_tbl_stag_db_warn_1730',
     'ERR_1_table': 'stage_db.ml__fl_stag_1716272404181579_user_err_1',
     'ERR_2_table': 'stage_db.ml__fl_stag_1716272404181579_user_err_2'}
    # Validate ERR_1 and ERR_2 tables.
    >>> DataFrame(in_schema("stage_db", "ml__fl_stag_1716270574550744_user_err_1"))
    ErrorCode       ErrorFieldName  DataParcel
    2673    F_C_timestamp   b'12E...'
    >>> DataFrame(in_schema("stage_db", "ml__fl_stag_1716270574550744_user_err_2"))
    C_ID    C_timestamp
read_csv
teradataml.dataframe.data_transfer.read_csv = read_csv(ﬁlepath, table_name, types=None, sep=',', quotechar='"', schema_name=None, if_exists='replace',
primary_index=None, set_table=False, temporary=False, primary_time_index_name=None, timecode_column=None, timebucket_duration=None, timezero_date=None,
columns_list=None, sequence_column=None, seq_max=None, catch_errors_warnings=False, save_errors=False, use_fastload=True, open_sessions=None)
The read_csv() API loads data from CSV file into Teradata Vantage.
Function can be used to quickly load large amounts of data in a table on Vantage
using FastloadCSV protocol.
Considerations when using a CSV file:
* Each record is on a separate line of the CSV file. Records are delimited
  by line breaks (CRLF). The last record in the file may or may not have an
  ending line break.
* First line in the CSV must be header line. The header line lists
  the column names separated by the field separator (e.g. col1,col2,col3).
* Using a CSV file with FastLoad has limitations as follows:
    1. read_csv API cannot load duplicate rows in the DataFrame if the table is a
       MULTISET table having primary index.
    2. read_csv API does not support all Teradata Advanced SQL Engine data types.
       For example, target table having BLOB and CLOB data type columns cannot be
       loaded.
    3. If there are any incorrect rows, i.e. due to constraint violations, data type
       conversion errors, etc., FastLoad protocol ignores those rows and inserts
       all valid rows.
    4. Rows in the DataFrame that failed to get inserted are categorized into errors
       and warnings by FastLoad protocol and these errors and warnings are stored
       into respective error and warning tables by FastLoad API.
    5. Teradata recommends to use Fastload protocol when number of rows to be loaded
       are at least 100,000. Fastload opens multiple data transfer connections to the
       database.
For additional information about FastLoadCSV protocol through teradatasql driver,
please refer the CSV BATCH INSERTS section of https://pypi.org/project/teradatasql/#CSVBatchInserts
driver documentation for more information.
PARAMETERS:
    filepath:
        Required Argument.
        Specifies the CSV filepath including name of the file to load the data from.
        Types: String
    table_name:
        Required Argument.
        Specifies the table name to load the data into.
        Types: String
    types:
        Optional Argument when if_exists=append and non-PTI table already exists, Required otherwise.
        Specifies the data types for requested columns to be saved in Vantage.
        Keys of this dictionary should be the name of the columns and values should be
        teradatasqlalchemy.types.
        Default Value: None
        Note:
            * If specified when "if_exists" is set to append and table exists, then argument is ignored.
        Types: OrderedDict
    sep:
        Optional Argument.
        Specifies a single character string used to separate fields in a CSV file.
        Default Value: ","
        Notes:
            * "sep" cannot be line feed ('\n') or carriage return ('\r').
            * "sep" should not be same as "quotechar".
            * Length of "sep" argument should be 1.
        Types: String
    quotechar:
        Optional Argument.
        Specifies a single character string used to quote fields in a CSV file.
        Default Value: """
        Notes:
            * "quotechar" cannot be line feed ('\n') or carriage return ('\r').
            * "quotechar" should not be same as "sep".
            * Length of "quotechar" argument should be 1.
        Types: String
    schema_name:
        Optional Argument.
        Specifies the name of the database/schema in Vantage to write to.
        Default Value: None (Uses default database/schema).
        Types: String
    if_exists:
        Optional Argument.
        Specifies the action to take when table already exists in Vantage.
        Permitted Values: 'fail', 'replace', 'append'
            - fail: If table exists, raise TeradataMlException.
            - replace: If table exists, drop it, recreate it, and insert data.
            - append: If table exists, append the existing table.
        Default Value: replace
        Types: String
    primary_index:
        Optional Argument.
        Specifies which column(s) to use as primary index while creating table
        in Vantage. When set to None, No Primary Index (NoPI) tables are created.
        Default Value: None
        Types: String or list of strings
        Example:
            primary_index = 'my_primary_index'
            primary_index = ['my_primary_index1', 'my_primary_index2', 'my_primary_index3']
    set_table:
        Optional Argument.
        Specifies a flag to determine whether to create a SET or a MULTISET table.
        When set to True, a SET table is created, otherwise MULTISET table is created.
        Default Value: False
        Notes:
            1. Specifying set_table=True also requires specifying primary_index.
            2. Creating SET table (set_table=True) results in
                a. loss of duplicate rows, if CSV contains any duplicate.
            3. This argument has no effect if the table already exists and if_exists='append'.
        Types: Boolean
    temporary:
        Optional Argument.
        Specifies whether to create table as volatile.
        Default Value: False
        Notes:
            When set to True
             1. FastloadCSV protocol is not used for loading the data.
             2. "schema_name" is ignored.
       Types : Boolean
    primary_time_index_name:
        Optional Argument.
        Specifies the name for the Primary Time Index (PTI) when the table
        is to be created as PTI table.
        Note:
            This argument is not required or used when the table to be created
            is not a PTI table. It will be ignored if specified without the "timecode_column".
        Types: String
    timecode_column:
        Optional argument.
        Required when the CSV data must be saved as a PTI table.
        Specifies the column in the csv that reflects the form
        of the timestamp data in the time series.
        This column will be the TD_TIMECODE column in the table created.
        It should be of SQL type TIMESTAMP(n), TIMESTAMP(n) WITH TIMEZONE, or DATE,
        corresponding to Python types datetime.datetime or datetime.date.
        Note:
            When "timecode_column" argument is specified, an attempt to create a PTI table
            will be made. This argument is not required when the table to be created
            is not a PTI table. If this argument is specified, "primary_index" will be ignored.
        Types: String
    timezero_date:
        Optional Argument.
        Used when the CSV data must be saved as a PTI table.
        Specifies the earliest time series data that the PTI table will accept,
        a date that precedes the earliest date in the time series data.
        Value specified must be of the following format: DATE 'YYYY-MM-DD'
        Default Value: DATE '1970-01-01'.
        Note:
            This argument is not required or used when the table to be created
            is not a PTI table. It will be ignored if specified without the "timecode_column".
        Types: String
    timebucket_duration:
        Optional Argument.
        Required if "columns_list" is not specified or is None.
        Used when the CSV data must be saved as a PTI table.
        Specifies a duration that serves to break up the time continum in
        the time series data into discrete groups or buckets.
        Specified using the formal form time_unit(n), where n is a positive
        integer, and time_unit can be any of the following:
        CAL_YEARS, CAL_MONTHS, CAL_DAYS, WEEKS, DAYS, HOURS, MINUTES,
        SECONDS, MILLISECONDS, or MICROSECONDS.
        Note:
            This argument is not required or used when the table to be created
            is not a PTI table. It will be ignored if specified without the "timecode_column".
        Types: String
    columns_list:
        Optional Argument.
        Used when the CSV data must be saved as a PTI table.
        Required if "timebucket_duration" is not specified.
        A list of one or more PTI table column names.
        Note:
            This argument is not required or used when the table to be created
            is not a PTI table. It will be ignored if specified without the "timecode_column".
        Types: String or list of Strings
    sequence_column:
        Optional Argument.
        Used when the CSV data must be saved as a PTI table.
        Specifies the column of type Integer containing the unique identifier for
        time series data readings when they are not unique in time.
        * When specified, implies SEQUENCED, meaning more than one reading from the same
          sensor may have the same timestamp.
          This column will be the TD_SEQNO column in the table created.
        * When not specified, implies NONSEQUENCED, meaning there is only one sensor reading
          per timestamp.
          This is the default.
        Note:
            This argument is not required or used when the table to be created
            is not a PTI table. It will be ignored if specified without the "timecode_column".
        Types: String
    seq_max:
        Optional Argument.
        Used when the CSV data must be saved as a PTI table.
        Specifies the maximum number of data rows that can have the
        same timestamp. Can be used when 'sequenced' is True.
        Permitted range:  1 - 2147483647.
        Default Value: 20000.
        Note:
            This argument is not required or used when the table to be created
            is not a PTI table. It will be ignored if specified without the "timecode_column".
        Types: Integer
    save_errors:
        Optional Argument.
        Specifies whether to persist the errors/warnings(if any) information in Vantage
        or not.
        If "save_errors" is set to False:
         1. Errors or warnings (if any) are not persisted into tables.
         2. Errors table genarated by FastloadCSV are not persisted.
        If "save_errors" is set to True:
         1. The errors or warnings information is persisted and names of error and
            warning tables are returned. Otherwise, the function returns None for
            the names of the tables.
         2. The errors tables generated by FastloadCSV are persisted and name of
            error tables are returned. Otherwise, the function returns None for
            the names of the tables.
        Default Value: False
        Types: Boolean
    catch_errors_warnings:
        Optional Argument.
        Specifies whether to catch errors/warnings(if any) raised by fastload
        protocol while loading data into the Vantage table.
        When set to False, function does not catch any errors and warnings,
        otherwise catches errors and warnings, if any, and returns
        as a dictionary along with teradataml DataFrame.
        Please see 'RETURNS' section for more details.
        Default Value: False
        Types: Boolean
    use_fastload:
        Optional Argument.
        Specifies whether to use Fastload CSV protocol or not.
        Default Value: True
        Notes:
            1. Teradata recommends to use Fastload when number of rows to be loaded
               are atleast 100,000. To load lesser rows set this argument to 'False'.
               Fastload opens multiple data transfer connections to the database.
            2. When "use_fastload" is set to True, one can load the data into table
               using FastloadCSV protocol:
                a. Set table
                b. Multiset table
            3. When "use_fastload" is set to False, one can load the data in following
               types of tables:
                a. Set table
                b. Multiset table
                c. PTI table
                d. Volatile table
        Types: Boolean
    open_sessions:
        Optional Argument.
        Specifies the number of Teradata data transfer sessions to be opened for fastload operation.
        Note : If "open_sessions" argument is not provided, the default value is the smaller of 8 or the
               number of AMPs available.
               For additional information about number of Teradata data-transfer
               sessions opened during fastload, please refer to:
               https://pypi.org/project/teradatasql/#FastLoad
        Default Value: None
        Types: int
RETURNS:
    When "use_fastload" is set to False, returns teradataml dataframe.
    When "use_fastload" is set to True, read_csv() returns below:
        When "catch_errors_warnings" is set to False, returns only teradataml dataframe.
        When "catch_errors_warnings" is set to True, read_csv() returns a tuple containing:
            a. teradataml DataFrame.
            b. a dict containing the following attributes:
                a. errors_dataframe: It is a Pandas DataFrame containing error messages
                   thrown by fastload. DataFrame is empty if there are no errors.
                b. warnings_dataframe: It is a Pandas DataFrame containing warning messages
                   thrown by fastload. DataFrame is empty if there are no warnings.
                c. errors_table: Name of the table containing errors. It is None, if
                   argument save_errors is False.
                d. warnings_table: Name of the table containing warnings. It is None, if
                   argument save_errors is False.
                e. fastloadcsv_error_tables: Name of the tables containing errors generated
                   by FastloadCSV. It is empty list, if argument "save_errors" is False.
RAISES:
    TeradataMlException
EXAMPLES:
    >>> from teradataml.dataframe.data_transfer import read_csv
    >>> from teradatasqlalchemy.types import *
    >>> from collections import OrderedDict
    # Example 1: Default execution with types argument is passed as OrderedDict.
    >>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
    >>> read_csv('test_file.csv', 'my_first_table', types)
    # Example 2: Load the data from CSV file into a table using fastload CSV protocol,
    #            while doing so catch all errors and warnings as well as store those in the table.
    >>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_table1', types=types,
    ...          save_errors=True, catch_errors_warnings=True)
    # Example 3: Load the data from CSV file into a table using fastload CSV protocol.
    #            If table exists, then replace the same. Catch all errors and warnings as well as
    #            store those in the table.
    >>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_table',
    ...          types=types, if_exists='replace',
    ...          save_errors=True, catch_errors_warnings=True)
    # Example 4: Load the data from CSV file into a table using fastload CSV protocol.
    #            If table exists in specified schema, then append the same. Catch all
    #            errors and warnings as well as store those in the table.
    >>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_table',
    ...          types=types, if_exists='fail',
    ...          save_errors=True, catch_errors_warnings=True)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_table',
    ...          if_exists='append',
    ...          save_errors=True, catch_errors_warnings=True)
    # Example 5: Load the data from CSV file into a SET table using fastload CSV protocol.
    #            Catch all errors and warnings as well as store those in the table.
    >>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_table',
    ...          types=types, if_exists='replace',
    ...          set_table=True, primary_index='id',
    ...          save_errors=True, catch_errors_warnings=True)
    # Example 6: Load the data from CSV file into a temporary table without fastloadCSV protocol.
    #            If table exists, then append to the same.
    >>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_table',
    ...          types=types, if_exists='replace',
    ...          temporary=True)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_table',
    ...          if_exists='append',
    ...          temporary=True)
    # Example 7: Load the data from CSV file with DATE and TIMESTAMP columns into
    #            a table without Fastload protocol. If table exists in specified
    #            schema, then append to the table.
    >>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT,
    ...                     admission_date=DATE, admission_time=TIMESTAMP)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_table',
    ...          types=types, if_exists='fail',
    ...          use_fastload=False)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_table',
    ...          if_exists='append',
    ...          use_fastload=False)
    # Example 8: Load the data from CSV file with TIMESTAMP columns into
    #            a PTI table. If specified table exists then append to the table,
    #            otherwise creates new table.
    >>> types = OrderedDict(partition_id=INTEGER, adid=INTEGER, productid=INTEGER,
    ...                     event=VARCHAR, clicktime=TIMESTAMP)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_read_csv_pti_table',
    ...          types=types, if_exists='append',
    ...          timecode_column='clicktime',
    ...          columns_list='event',
    ...          use_fastload=False)
    # Example 9: Load the data from CSV file with TIMESTAMP columns into
    #            a SET PTI table. If specified table exists then append to the table,
    #            otherwise creates new table.
    >>> types = OrderedDict(partition_id=INTEGER, adid=INTEGER, productid=INTEGER,
                            event=VARCHAR, clicktime=TIMESTAMP)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_read_csv_pti_table',
    ...          types=types, if_exists='append',
    ...          timecode_column='clicktime',
    ...          columns_list='event',
    ...          set_table=True)
    # Example 10: Load the data from CSV file with TIMESTAMP columns into
    #            a temporary PTI table. If specified table exists then append to the table,
    #            otherwise creates new table.
    >>> types = OrderedDict(partition_id=INTEGER, adid=INTEGER, productid=INTEGER,
                            event=VARCHAR, clicktime=TIMESTAMP)
    >>> read_csv(filepath='test_file.csv',
    ...          table_name='my_first_read_csv_pti_table',
    ...          types=types, if_exists='append',
    ...          timecode_column='clicktime',
    ...          columns_list='event',
    ...          temporary=True)
    # Example 11: Load the data from CSV file into Vantage table by opening specified
    #             number of Teradata data transfer sesions.
    >>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
    >>> read_csv(filepath='test_file.csv', table_name='my_first_table_with_open_sessions',
                types=types, open_sessions=2)
    # Example 12: Load the data from CSV file into Vantage table and set primary index provided
    #             through primary_index argument.
    >>> types = OrderedDict(id=BIGINT, fname=VARCHAR, lname=VARCHAR, marks=FLOAT)
    >>> read_csv(filepath='test_file.csv', table_name='my_first_table_with_primary_index',
    ...          types=types, primary_index = ['fname'])
    # Example 13: Load the data from CSV file into VECTOR datatype in Vantage table.
    >>> from teradatasqlalchemy import VECTOR
    >>> from pathlib import Path
    >>> types = OrderedDict(id=BIGINT, array_col=VECTOR)
    # Get the absolute path of the teradataml module
    >>> import teradataml
    >>> base_path = Path(teradataml.__path__[0])
    # Append the relative path to the CSV file
    >>> csv_path = os.path.join(base_path, "data", "hnsw_alter_data.csv")
    >>> read_csv(filepath=csv_path, 
    ...          table_name='my_first_table_with_vector',
    ...          types=types,
    ...          use_fastload=False)
Database Utility and Management Functions
db_drop_table
teradataml.dbutils.dbutils.db_drop_table = db_drop_table(table_name, schema_name=None, suppress_error=False, datalake_name=None, purge=None)
DESCRIPTION:
    Drops the table from the given schema.
PARAMETERS:
    table_name:
        Required Argument
        Specifies the table name to be dropped.
        Types: str
    schema_name:
        Optional Argument
        Specifies schema of the table to be dropped. If schema is not specified, function drops table from the
        current database.
        Default Value: None
        Types: str
    suppress_error:
        Optional Argument
        Specifies whether to raise error or not.
        Default Value: False
        Types: str
    datalake_name:
        Optional Argument
        Specifies name of the datalake to drop table from.
        Note:
             "schema_name" must be provided while using this argument.
        Default Value: None
        Types: str
    purge:
        Optional Argument
        Specifies whether to use purge clause or not while dropping datalake table.
        It is only applicable when "datalake_name" argument is used. When "datalake_name" is specified,
        but "purge" is not specified, data is purged by default.
        Default Value: None
        Types: bool
RETURNS:
    True - if the operation is successful.
RAISES:
    TeradataMlException - If the table doesn't exist.
EXAMPLES:
    >>> load_example_data("dataframe", "admissions_train")
    # Example 1: Drop table in current database.
    >>> db_drop_table(table_name = 'admissions_train')
    # Example 2: Drop table from the given schema.
    >>> db_drop_table(table_name = 'admissions_train', schema_name = 'alice')
    #Example 3: Drop a table from datalake and purge the data.
    >>> db_drop_table(table_name = 'datalake_table', schema_name = 'datalake_db',
    ...               datalake_name='datalake', purge=True)
db_drop_view
teradataml.dbutils.dbutils.db_drop_view = db_drop_view(view_name, schema_name=None)
DESCRIPTION:
    Drops the view from the given schema.
PARAMETERS:
    view_name:
        Required Argument
        Specifies view name to be dropped.
        Types: str
    schema_name:
        Optional Argument
        Specifies schema of the view to be dropped. If schema is not specified, function drops view from the current
        database.
        Default Value: None
        Types: str
RETURNS:
    True - if the operation is successful.
RAISES:
    TeradataMlException - If the view doesn't exist.
EXAMPLES:
    # Create a view
    >>> connection_object.execute("create view temporary_view as (select 1 as dummy_col1, 2 as dummy_col2);")
    # Drop view in current schema
    >>> db_drop_view(view_name = 'temporary_view')
    # Drop view from the given schema
    >>> db_drop_view(view_name = 'temporary_view', schema_name = 'alice')
db_list_tables
teradataml.dbutils.dbutils.db_list_tables = db_list_tables(schema_name=None, object_name=None, object_type='all', datalake_name=None)
DESCRIPTION:
    Lists the Vantage objects(table/view) names for the specified schema name.
PARAMETERS:
    schema_name:
        Optional Argument.
        Specifies the name of schema in the database. If schema is not specified, function lists tables/views from
        the current database.
        Default Value: None
        Types: str
    object_name:
        Optional Argument.
        Specifies a table/view name or pattern to be used for filtering them from the database.
        Pattern may contain '%' or '_' as pattern matching characters.
        - '%' represents any string of zero or more arbitrary characters. Any string of characters is acceptable as
          a replacement for the percent.
        - '_' represents exactly one arbitrary character. Any single character is acceptable in the position in
          which the underscore character appears.
        Note:
            * If '%' is specified in 'object_name', then the '_' character is not evaluated for an arbitrary character.
        Default Value: None
        Types: str
        Example:
            1. '%abc' will return all table/view object names starting with any character and ending with abc.
            2. 'a_c' will return all table/view object names starting with 'a', ending with 'c' and has length of 3.
    object_type:
        Optional Argument.
        Specifies object type to apply the filter. Valid values for this argument are 'all','table','view',
        'volatile','temp'.
            * all - List all the object types.
            * table - List only tables.
            * view - List only views.
            * volatile - List only volatile tables.
            * temp - List all teradataml temporary objects created in the specified database.
        Default Value: 'all'
        Types: str
    datalake_name:
        Optional Argument.
        Specifies the name of datalake to list tables from.
        Note:
            "schema_name" must be provided while using this argument.
        Default Value: None
        Types: str
RETURNS:
    Pandas DataFrame
RAISES:
    TeradataMlException - If the object_type argument is provided with invalid values.
    OperationalError    - If any errors are raised from Vantage.
EXAMPLES:
    # Example 1: List all object types in the default schema
    >>> load_example_data("dataframe", "admissions_train")
    >>> db_list_tables()
    # Example 2: List all the views in the default schema
    >>> execute_sql("create view temporary_view as (select 1 as dummy_col1, 2 as dummy_col2);")
    >>> db_list_tables(None , None, 'view')
    # Example 3: List all the object types in the default schema whose names begin with 'abc' followed by any number
    # of characters in the end.
    >>> execute_sql("create view abcd123 as (select 1 as dummy_col1, 2 as dummy_col2);")
    >>> db_list_tables(None, 'abc%', None)
    # Example 4: List all the tables in the default schema whose names begin with 'adm' followed by any number of
    # characters and ends with 'train'.
    >>> load_example_data("dataframe", "admissions_train")
    >>> db_list_tables(None, 'adm%train', 'table')
    # Example 5: List all the views in the default schema whose names begin with any character but ends with 'abc'
    >>> execute_sql("create view view_abc as (select 1 as dummy_col1, 2 as dummy_col2);")
    >>> db_list_tables(None, '%abc', 'view')
    # Example 6: List all the volatile tables in the default schema whose names begin with 'abc' and ends with any
    # arbitrary character and has a length of 4
    >>> execute_sql("CREATE volatile TABLE abcd(col0 int, col1 float) NO PRIMARY INDEX;")
    >>> db_list_tables(None, 'abc_', 'volatile')
    # Example 7: List all the temporary objects created by teradataml in the default schema whose names begins and
    # ends with any number of arbitrary characters but contains 'filter' in between.
    >>> db_list_tables(None, '%filter%', 'temp')
    # Example 8: List all the tables in datalake's database.
    >>> db_list_tables(schema_name='datalake_db_name', datalake_name='datalake_name')
db_python_package_details
teradataml.dbutils.db_python_package_details = db_python_package_details(names=None)
DESCRIPTION:
    Function to get the Python packages, installed on Vantage, and their corresponding
    versions.
    Note:
        Using this function is valid only when Python interpreter and add-on packages
        are installed on the Vantage node.
PARAMETERS:
    names:
        Optional Argument.
        Specifies the name(s)/pattern(s) of the Python package(s) for which version
        information is to be fetched from Vantage. If this argument is not specified
        or None, versions of all installed Python packages are returned.
        Default Value: None
        Types: str
RETURNS:
    teradataml DataFrame, if package(s) is/are present in the Vantage.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Note:
    #   These examples will work only when the Python packages are installed on Vantage.
    # Example 1: Get the details of a Python package 'dill' from Vantage.
    >>> db_python_package_details("dill")
      package  version
    0    dill  0.2.8.2
    # Example 2: Get the details of Python packages, having string 'mpy', installed on Vantage.
    >>> db_python_package_details(names = "mpy")
             package  version
    0          simpy   3.0.11
    1          numpy   1.16.1
    2          gmpy2    2.0.8
    3  msgpack-numpy  0.4.3.2
    4          sympy      1.3
    # Example 3: Get the details of Python packages, having string 'numpy' and 'learn',
    #            installed on Vantage.
    >>> db_python_package_details(["numpy", "learn"])
             package  version
    0   scikit-learn   0.20.3
    1          numpy   1.16.1
    2  msgpack-numpy  0.4.3.2
    # Example 4: Get the details of all Python packages installed on Vantage.
    >>> db_python_package_details()
              package  version
    0       packaging     18.0
    1          cycler   0.10.0
    2           simpy   3.0.11
    3  more-itertools    4.3.0
    4          mpmath    1.0.0
    5           toolz    0.9.0
    6       wordcloud    1.5.0
    7         mistune    0.8.4
    8  singledispatch  3.4.0.3
    9           attrs   18.2.0
db_python_package_version_diff
teradataml.dbutils.dbutils.db_python_package_version_diff = db_python_package_version_diff(packages=None)
DESCRIPTION:
    Function to get the difference of the Python packages installed on Vantage and
    in the current environment mentioned in the argument "packages".
    Notes:
        * Using this function is valid only when Python interpreter and add-on packages
          are installed on the Vantage node.
        * This function also checks for differences in Python packages versions given
          part of package name as string.
PARAMETERS:
    packages:
        Optional Argument.
        Specifies the name(s) of the Python package(s) for which the difference
        in the versions is to be fetched from Vantage.
        Notes:
            * If this argument is None, all the packages installed on Vantage are considered.
            * If any package is present in Vantage but not in the current environment, then None
              is shown as the version of the package in the current environment.
        Types: str or list of str
RETURNS:
    pandas DataFrame
RAISES:
    TeradataMlException.
EXAMPLES:
    # Note:
    # These examples will work only when the Python packages are installed on Vantage.
    # Example 1: Get the difference in the versions of Python package 'dill' installed on Vantage.
    >>> db_python_package_version_diff("dill")
              package   vantage    local
    0            dill    0.10.0   0.11.2
    # Example 2: Get the difference in the versions of all Python packages installed on Vantage.
    >>> db_python_package_version_diff()
              package   vantage    local
    0    scikit-learn     1.3.3   0.24.2
    1            dill    0.10.0   0.11.2
    ...
    532         attrs    18.2.0   17.0.0
db_python_version_diff
teradataml.dbutils.dbutils.db_python_version_diff = db_python_version_diff()
DESCRIPTION:
    Function to get the difference of the Python intepreter major version installed on Vantage
    and the Python version used in the current environment.
    Note:
        * Using this function is valid only when Python interpreter and add-on packages
          are installed on the Vantage node.
RETURNS:
    Empty dictionary when Python major version is same on Vantage and the current environment.
    Otherwise, returns a dictionary with the following keys:
        - 'vantage_version': Python major version installed on Vantage.
        - 'local_version': Python major version used in the current environment.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Note:
    # These examples will work only when the Python packages are installed on Vantage.
    # Example 1: Get the difference in the Python version installed on Vantage and the current environment.
    >>> db_python_version_diff()
    {"vantage_version": "3.7", "local_version": "3.8"}
install_file
teradataml.dbutils.ﬁlemgr.install_ﬁle = install_ﬁle(ﬁle_identiﬁer, ﬁle_path, ﬁle_on_client=True, is_binary=False, replace=False, force_replace=False, suppress_output=False)
DESCRIPTION:
    Function installs or replaces external language script or model files in Vantage.
    On success, prints a message that file is installed or replaced.
    This language script can be executed via execute_script() method in Script.
PARAMETERS:
    file_identifier:
        Required Argument.
        Specifies the name associated with the user-installed file.
        It cannot have a schema name associated with it,
        as the file is always installed in the current schema.
        The name should be unique within the schema. It can be any valid Teradata identifier.
        Types: str
    file_path:
        Required Argument.
        Specifies the absolute/relative path of file (including file name) to be installed.
        This file is identified in Vantage by file_identifier.
        Types: str
    file_on_client:
        Optional Argument.
        Specifies whether the file is present on client or remote location on Vantage.
        Set this to False if the file to be installed is present at remote location on Vantage.
        Default Value: True
        Types: bool
    is_binary:
        Optional Argument.
        Specifies if file to be installed is a binary file.
        Default Value: False
        Types: bool
    replace:
        Optional Argument.
        Specifies if the file is to be installed or replaced.
        If set to True, then the file is replaced based on the value of force_replace.
        If set to False, then the file is installed.
        Default Value: False
        Types: bool
    force_replace:
        Optional Argument.
        Specifies if system should check for the file being used before replacing it.
        If set to True, then the file is replaced even if it is being executed.
        If set to False, then an error is thrown if it is being executed.
        Default Value: False
        Types: bool
        Note:
            This argument is ignored if replace is set to False.
    suppress_output:
        Optional Argument.
        Specifies whether to print the output message or not.
        If set to True, then the output message is not printed.
        Default Value: False
        Types: bool
RETURNS:
     True, if success.
RAISES:
    TeradataMLException.
EXAMPLES:
    # Note: File can be on client or remote server.
    # Files mentioned in the examples are part of the package and can be found at package install location
    # followed by the file path mentioned in examples.
    # In first example, file_path is data/scripts/mapper.py, to use this file for installation,
    # one should pass ${tdml_install_location}/data/scripts/mapper.py to file location.
    # Example 1: Install the file mapper.py found at the relative path data/scripts/ using
    #            the default text mode.
    >>> install_file (file_identifier='mapper', file_path='data/scripts/mapper.py')
    File mapper.py installed in Vantage
    # Example 2: Install the file binary_file.dms found at the relative path data/scripts/
    #            using the binary mode.
    >>> install_file (file_identifier='binaryfile', file_path='data/scripts/binary_file.dms', file_on_client = True, is_binary 
    File binary_file.dms installed in Vantage
    # Example 3: Replace the file mapper.py with mapper_replace.py found at the relative path data/scripts/
    #            using the default text mode.
    >>> install_file (file_identifier='mapper', file_path='data/scripts/mapper_replace.py', file_on_client=True, is_binary= Fal
    File mapper_replace.py replaced in Vantage
list_files
teradataml.dbutils.ﬁlemgr.list_ﬁles = list_ﬁles()
DESCRIPTION:
    List all the files installed in Vantage or in Vantage Languages Ecosystem.
PARAMETERS:
    None
RETURNS:
    teradataml DataFrame
RAISES:
    TeradataMLException.
EXAMPLES:
# Example 1: List files installed in the Vantage Ecosystem.
# Install the file mapper.py found at the relative path data/scripts/ 
>>> install_file (file_identifier='mapper', file_path='data/scripts/mapper.py')
File mapper.py installed in Vantage
# List file installed in the Vantage Ecosystem.
>>> list_files()
       Files
0  mapper.py
remove_file
teradataml.dbutils.ﬁlemgr.remove_ﬁle = remove_ﬁle(ﬁle_identiﬁer, force_remove=False, suppress_output=False)
DESCRIPTION:
    Function to remove user installed files/scripts from Vantage.
PARAMETERS:
    file_identifier:
        Required Argument.
        Specifies the name associated with the user-installed file.
        It cannot have a database name associated with it,
        as the file is always installed in the current database.
        Types: str
    force_remove:
        Required Argument.
        Specifies if system should check for the file being used before removing it.
        If set to True, then the file is removed even if it is being executed.
        If set to False, then an error is thrown if it is being executed.
        Default Value: False
        Types: bool
    suppress_output:
        Optional Argument.
        Specifies whether to print the output message or not.
        If set to True, then the output message is not printed.
        Default Value: False
        Types: bool
RETURNS:
     True, if success.
RAISES:
    TeradataMLException.
EXAMPLES:
    # Run install_file example before removing file.
    # Note: File can be on client or remote server. The file location should be specified accordingly.
    # Example 1: Install the file mapper.py found at the relative path data/scripts/ using the default
    #            text mode.
    >>> install_file (file_identifier='mapper', file_path='data/scripts/mapper.py')
    File mapper.py installed in Vantage
    # Example 2: Install the file binary_file.dms found at the relative path data/scripts/
    #            using the binary mode.
    >>> install_file (file_identifier='binaryfile', file_path='data/scripts/binary_file.dms', file_on_client = True, is_binary 
    File binary_file.dms installed in Vantage
    # Remove the installed files.
    # Example 1: Remove text file
    >>> remove_file (file_identifier='mapper', force_remove=True)
    File mapper removed from Vantage
    # Example 2: Remove binary file
    >>> remove_file (file_identifier='binaryfile', force_remove=False)
    File binaryfile removed from Vantage
display_analytic_functions
teradataml.analytics.utils.display_analytic_functions = display_analytic_functions(type=None, name=None)
DESCRIPTION:
    Display list of analytic functions available to use on the Teradata Vantage system, user is connected to.
PARAMETERS:
    type:
        Optional Argument.
        Specifies the type(s) of functions to list down.
        Permitted Values: "BYOM", "TABLE OPERATOR", "SQLE", "UAF", "VAL".
        Types: str or list of str(s)
    name:
        Optional Argument.
        Specifies the search string for function name. When this argument 
        is used, all functions matching the name are listed.
        Types: str or list of str(s)
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    >>> from teradataml import create_context, display_analytic_functions
    # Example 1: Displaying a list of available analytic functions
    >>> connection_name = create_context(host = host, username=user, password=password)
    >>> display_analytic_functions()
        List of available functions:
            Analytics Database Functions:
                * PATH AND PATTERN ANALYSIS functions:
                     1. Attribution
                     2. NPath
                     3. Sessionize
                     .
                     .
                     .
                     5. VectorDistance
                * FEATURE ENGINEERING UTILITY functions:
                     1. FillRowId
                     2. NumApply
                     3. RoundColumns
                     4. StrApply
                     .
                     .
            Unbounded Array Framework (UAF) Functions:
                * MODEL PREPARATION AND PARAMETER ESTIMATION functions:
                     1. ACF
                     2. ArimaEstimate
                     3. ArimaValidate
                     4. DIFF
                     5. LinearRegr
                     6. MultivarRegr
                     7. PACF
                     8. PowerTransform
                     9. SeasonalNormalize
                     .
                     .
            TABLE OPERATOR Functions:
                 1. ReadNOS
                 2. WriteNOS
            BYOM Functions:
                 1. H2OPredict
                 2. ONNXPredict
                 3. PMMLPredict
            Vantage Analytic Library (VAL) Functions:
                * DESCRIPTIVE STATISTICS functions:
                     1. AdaptiveHistogram
                     2. Explore
                     3. Frequency
         ...
     # Example 2: When no analytic functions are available on the cluster.
     >>> display_analytic_functions(name="Func_does_not_exists")
     No analytic functions available with connected Teradata Vantage system with provided filters.
     # Example 3: List all available SQLE analytic functions.
     >>> display_analytic_functions(type="SQLE")
        List of available functions:
            Analytics Database Functions:
                * PATH AND PATTERN ANALYSIS functions:
                     1. Attribution
                     2. NPath
                     3. Sessionize
                * MODEL SCORING functions:
                     1. DecisionForestPredict
                     2. DecisionTreePredict
                     3. GLMPredict
                     4. KMeansPredict
                     .
                     .
                     .
                     4. KNN
                     5. VectorDistance
                * FEATURE ENGINEERING UTILITY functions:
                     1. FillRowId
                     2. NumApply
                     3. RoundColumns
                     4. StrApply
      ...
     # Example 4: List all functions with function name containing string "fit".
     >>> display_analytic_functions(name="fit")
        List of available functions:
            Analytics Database Functions:
                * FEATURE ENGINEERING TRANSFORM functions:
                     1. BincodeFit
                     2. Fit
                     3. NonLinearCombineFit
                     4. OneHotEncodingFit
                     5. OrdinalEncodingFit
                     6. PolynomialFeaturesFit
                     7. RandomProjectionFit
                     8. RowNormalizeFit
                     9. ScaleFit
                     10. SimpleImputeFit
                * DATA CLEANING functions:
                     1. OutlierFilterFit
            Unbounded Array Framework (UAF) Functions:
                * DIAGNOSTIC STATISTICAL TEST functions:
                     1. FitMetrics
     # Example 5: List all SQLE functions with function name containing string "fit".
     >>> display_analytic_functions(type="SQLE", name="fit")
        List of available functions:
            Analytics Database Functions:
                * FEATURE ENGINEERING TRANSFORM functions:
                     1. BincodeFit
                     2. Fit
                     3. NonLinearCombineFit
                     4. OneHotEncodingFit
                     5. OrdinalEncodingFit
                     6. PolynomialFeaturesFit
                     7. RandomProjectionFit
                     8. RowNormalizeFit
                     9. ScaleFit
                     10. SimpleImputeFit
                * DATA CLEANING functions:
                     1. OutlierFilterFit
     # Example 6: List all functions of type "TABLE OPERATOR" and "SQLE" containing "fit" and "nos".
     >>> display_analytic_functions(type=["SQLE", "TABLE OPERATOR"], name=["fit", "nos"])
        List of available functions:
            Analytics Database Functions:
                * FEATURE ENGINEERING TRANSFORM functions:
                     1. BincodeFit
                     2. Fit
                     3. NonLinearCombineFit
                     4. OneHotEncodingFit
                     5. OrdinalEncodingFit
                     6. PolynomialFeaturesFit
                     7. RandomProjectionFit
                     8. RowNormalizeFit
                     9. ScaleFit
                     10. SimpleImputeFit
                * DATA CLEANING functions:
                     1. OutlierFilterFit
            TABLE OPERATOR Functions:
                 1. ReadNOS
                 2. WriteNOS
    # Example 7: List all functions of type "UAF" containing "estimate".
    >>> display_analytic_functions(type = "UAF", name = "estimate")
    List of available functions:
            Unbounded Array Framework (UAF) Functions:
            * MODEL PREPARATION AND PARAMETER ESTIMATION functions:
                 1. ArimaEstimate
list_td_reserved_keywords
teradataml.dbutils.dbutils.list_td_reserved_keywords = list_td_reserved_keywords(key=None, raise_error=False)
DESCRIPTION:
    Function validates if the specified string or the list of strings is Teradata reserved keyword or not.
    If key is not specified or is a empty list, list all the Teradata reserved keywords.
PARAMETERS:
    key:
        Optional Argument.
        Specifies a string or list of strings to validate for Teradata reserved keyword.
        Types: string or list of strings
    raise_error:
        Optional Argument.
        Specifies whether to raise exception or not.
        When set to True, an exception is raised,
        if specified "key" contains Teradata reserved keyword, otherwise not.
        Default Value: False
        Types: bool
RETURNS:
    teradataml DataFrame, if "key" is None or a empty list.
    True, if "key" contains Teradata reserved keyword, False otherwise.
RAISES:
    TeradataMlException.
EXAMPLES:
   >>> from teradataml import list_td_reserved_keywords
   >>> # Example 1: List all available Teradata reserved keywords.
   >>> list_td_reserved_keywords()
     restricted_word
   0             ABS
   1         ACCOUNT
   2            ACOS
   3           ACOSH
   4      ADD_MONTHS
   5           ADMIN
   6             ADD
   7     ACCESS_LOCK
   8    ABORTSESSION
   9           ABORT
   >>>
   >>> # Example 2: Validate if keyword "account" is a Teradata reserved keyword or not.
   >>> list_td_reserved_keywords("account")
   True
   >>>
   >>> # Example 3: Validate and raise exception if keyword "account" is a Teradata reserved keyword.
   >>> list_td_reserved_keywords("account", raise_error=True)
   TeradataMlException: [Teradata][teradataml](TDML_2121) '['ACCOUNT']' is a Teradata reserved keyword.
   >>> # Example 4: Validate if the list of keywords contains Teradata reserved keyword or not.
   >>> list_td_reserved_keywords(["account", 'add', 'abc'])
   True
   >>> # Example 5: Validate and raise exception if the list of keywords contains Teradata reserved keyword.
   >>> list_td_reserved_keywords(["account", 'add', 'abc'], raise_error=True)
   TeradataMlException: [Teradata][teradataml](TDML_2121) '['ADD', 'ACCOUNT']' is a Teradata reserved keyword.
set_session_param
teradataml.dbutils.dbutils.set_session_param = set_session_param(name, value)
DESCRIPTION:
    Function to set the session parameter.
    Note:
        * Look at Vantage documentation for session parameters.
PARAMETERS:
    name:
        Required Argument.
        Specifies the name of the parameter to set.
        Permitted Values: timezone, calendar, account, character_set_unicode,
                          collation, constraint, database, dateform, debug_function,
                          dot_notation, isolated_loading, function_trace, json_ignore_errors,
                          searchuifdbpath, transaction_isolation_level, query_band, udfsearchpath
        Types: str
    value:
        Required Argument.
        Specifies the value for the parameter "name" to set.
        Permitted Values:
            1. timezone: timezone strings
            2. calendar: Teradata, ISO, Compatible
            3. character_set_unicode: ON, OFF
            4. account: should be a list in which first item should be "account string" second should be
                        either SESSION or REQUEST.
            5. collation: ASCII, CHARSET_COLL, EBCDIC, HOST, JIS_COLL, MULTINATIONAL
            6. constraint: row_level_security_constraint_name {( level_name | category_name [,...] | NULL )}
                           where,
                           row_level_security_constraint_name:
                               Name of an existing constraint.
                               The specified constraint_name must be currently assigned to the user.
                               User can specify a maximum of 6 hierarchical constraints and 2 non-hierarchical
                               constraints per SET SESSION CONSTRAINT statement.
                           level_name:
                               Name of a hierarchical level, valid for the constraint_name, that is to replace the
                               default level.
                               The specified level_name must be currently assigned to the user. Otherwise, Vantage
                               returns an error to the requestor.
                           category_name:
                               A set of one or more existing non-hierarchical category names valid for the
                               constraint_name.
                               Because all assigned category (non-hierarchical) constraint values assigned to a
                               user are automatically active, "set_session_param" is only useful to specify a
                               subset of the assigned categories for the constraint.
                               For example, assume that User BOB has 3 country codes, and wants to load a table
                               with data that is to be made available to User CARL who only has rights to see data
                               for his own country. User BOB can use "set_session_param" to specify only the
                               country code for User CARL when loading the data so Carl can access the data later.
            7. database: Name of the new default database for the remainder of the current session.
            8. dateform: ANSIDATE, INTEGERDATE
            9. debug_function: should be a list in which first item should be "function_name" second should be
                        either ON or OFF.
            10. dot_notation: DEFAULT, LIST, NULL ERROR
            11. isolated_loading: NO, '', CONCURRENT
            12. function_trace: Should be a list. First item should be "mask_string" and second should be table name.
            13. json_ignore_errors: ON, OFF
            14. searchuifdbpath: String in format 'database_name, user_name'
            15. transaction_isolation_level: READ UNCOMMITTED, RU, SERIALIZABLE, SR
            16. query_band: Should be a list. First item should be "band_specification" and second should be either
                            SESSION or TRANSACTION
            17. udfsearchpath: Should be a list. First item should be "database_name" and second should be "udf_name"
        Types: str or list of strings
Returns:
    True, if session parameter is set successfully.
RAISES:
    ValueError, teradatasql.OperationalError
EXAMPLES:
    # Example 1: Set time zone offset for the session as the system default.
    >>> set_session_param('timezone', 'LOCAL')
    True
    # Example 2: Set time zone to "AMERICA PACIFIC".
    >>> set_session_param('timezone', "'AMERICA PACIFIC'")
    True
    # Example 3: Set time zone to "-07:00".
    >>> set_session_param('timezone', "'-07:00'")
    True
    # Example 4: Set time zone to 3 hours ahead of 'GMT'.
    >>> set_session_param('timezone', "3")
    True
    # Example 6: Set calendar to 'COMPATIBLE'.
    >>> set_session_param('calendar', "COMPATIBLE")
    True
    # Example 7: Dynamically changes your account to 'dbc' for the remainder of the session.
    >>> set_session_param('account', ['dbc', 'SESSION'])
    True
    # Example 8: Enables Unicode Pass Through processing.
    >>> set_session_param('character_set_unicode', 'ON')
    True
    # Example 9: Session set to ASCII collation.
    >>> set_session_param('collation', 'ASCII')
    True
    # Example 10: The resulting session has a row-level security label consisting of an unclassified level
    #             and nato category.
    >>> set_session_param('constraint', 'classification_category (norway)')
    True
    # Example 11: Changes the default database for the session.
    >>> set_session_param('database', 'alice')
    True
    # Example 12: Changes the  DATE format to 'INTEGERDATE'.
    >>> set_session_param('dateform', 'INTEGERDATE')
    True
    # Example 13: Enable Debugging for the Session.
    >>> set_session_param('debug_function', ['function_name', 'ON'])
    True
    # Example 14: Sets the session response for dot notation query result.
    >>> set_session_param('dot_notation', 'DEFAULT')
    True
    # Example 15: DML operations are not performed as concurrent load isolated operations.
    >>> set_session_param('isolated_loading', 'NO')
    True
    # Example 16: Enables function trace output for debugging external user-defined functions and
    #             external SQL procedures for the current session.
    >>> set_session_param('function_trace', ["'diag,3'", 'titanic'])
    True
    # Example 17: Enables the validation of JSON data on INSERT operations.
    >>> set_session_param('json_ignore_errors', 'ON')
    True
    # Example 18: Sets the database search path for the SCRIPT execution in the SessionTbl.SearchUIFDBPath column.
    >>> set_session_param('SEARCHUIFDBPATH', 'dbc, alice')
    True
    # Example 19: Sets the read-only locking severity for all SELECT requests made against nontemporal tables,
    #             whether they are outer SELECT requests or subqueries, in the current session to READ regardless
    #             of the setting for the DBS Control parameter AccessLockForUncomRead.
    #             Note: SR and SERIALIZABLE are synonyms.
    >>> set_session_param('TRANSACTION_ISOLATION_LEVEL', 'SR')
    True
    # Example 20: This example uses the PROXYROLE name:value pair in a query band to set the proxy
    #             role in a trusted session to a specific role.
    >>> set_session_param('query_band', ["'PROXYUSER=fred;PROXYROLE=administration;'", 'SESSION'])
    True
    # Example 21: Allows you to specify a custom UDF search path. When you execute a UDF,
    #             Vantage searches this path first, before looking in the default Vantage
    #             search path for the UDF.
    >>> set_session_param('udfsearchpath', ["alice, SYSLIB, TD_SYSFNLIB", 'bitor'])
    True
unset_session_param
teradataml.dbutils.dbutils.unset_session_param = unset_session_param(name)
DESCRIPTION:
    Function to unset the session parameter.
PARAMETERS:
    name:
        Required Argument.
        Specifies the parameter to unset for the session.
        Permitted Values: timezone, account, calendar, collation,
                          database, dataform, character_set_unicode,
                          debug_function, isolated_loading, function_trace,
                          json_ignore_errors, query_band
        Type: str
Returns:
    True, if successfully unsets the session parameter.
RAISES:
ValueError, teradatasql.OperationalError
EXAMPLES:
    # Example 1: Unset session's time zone to previous time zone.
    >>> set_session_param('timezone', "'GMT+1'")
    True
    >>> unset_session_param("timezone")
    True
DataFrameColumn Functions
case
teradataml.dataframe.sql_functions.case = case(whens, value=None, else_=None)
Returns a ColumnExpression based on the CASE expression.
PARAMETERS:
    whens:
        Required Argument.
        Specifies the criteria to be compared against. It accepts two different forms,
        based on whether or not the value argument is used.
        In the first form, it accepts a list of 2-tuples; each 2-tuple consists of (<sql expression>, <value>),
        where the <sql expression> is a boolean expression and “value” is a resulting value.
        For example:
            case([
                    (df.first_name == 'wendy', 'W'),
                    (df.first_name == 'jack', 'J')
                ])
        In the second form, it accepts a Python dictionary of comparison values mapped to a resulting value;
        this form requires 'value' argument to be present, and values will be compared using the '==' operator.
        For example:
            case(
                {"wendy": "W", "jack": "J"},
                  value=df.first_name
                )
        Types: List of 2-tuples or Dictionary of comparison value mapped to a resulting value.
    value:
        Optional Argument. Required when 'whens' is of dictionary type.
        Specifies a SQL expression (ColumnExpression or literal) which will be used as a fixed “comparison point”
        for candidate values within a dictionary passed to the 'whens' argument.
        Types: ColumnExpression or SQL Expression (Python literal)
    else_:
        Optional Argument.
        Specifies a SQL expression (ColumnExpression or literal) which will be the evaluated result of
        the CASE construct if all expressions within 'whens' evaluate to False.
        When omitted, will produce a result of NULL if none of the 'when' expressions evaluate to True.
        Types: ColumnExpression or SQL Expression (Python literal)
RETURNS:
    ColumnExpression
EXAMPLES:
    >>> from teradataml.dataframe.sql_functions import case
    >>> load_example_data("GLM", ["admissions_train"])
    >>> df = DataFrame("admissions_train")
    >>> print(df)
       masters   gpa     stats programming  admitted
    id
    5       no  3.44    Novice      Novice         0
    3       no  3.70    Novice    Beginner         1
    1      yes  3.95  Beginner    Beginner         0
    20     yes  3.90  Advanced    Advanced         1
    8       no  3.60  Beginner    Advanced         1
    25      no  3.96  Advanced    Advanced         1
    18     yes  3.81  Advanced    Advanced         1
    24      no  1.87  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    >>> print(df.shape)
    (40, 6)
    >>> # Example showing 'whens' passed a 2-tuple - assign rating based on GPA
    >>> # gpa > 3.0        = 'good'
    >>> # 2.0 < gpa <= 3.0 = 'average'
    >>> # gpa <= 2.0       = 'bad'
    >>> # Filtering all the 'good' scores only.
    >>> good_df = df[case([(df.gpa > 3.0, 'good'),
                           (df.gpa > 2.0, 'average')],
                           else_='bad') == 'good']
    >>> print(good_df)
       masters   gpa     stats programming  admitted
    id
    13      no  4.00  Advanced      Novice         1
    11      no  3.13  Advanced    Advanced         1
    9       no  3.82  Advanced    Advanced         1
    26     yes  3.57  Advanced    Advanced         1
    3       no  3.70    Novice    Beginner         1
    1      yes  3.95  Beginner    Beginner         0
    20     yes  3.90  Advanced    Advanced         1
    18     yes  3.81  Advanced    Advanced         1
    5       no  3.44    Novice      Novice         0
    32     yes  3.46  Advanced    Beginner         0
    >>> print(good_df.shape)
    (35, 6)
    >>> # Use DataFrame.assign() to create a new column with the rating
    >>> whens_df = df.assign(rating = case([(df.gpa > 3.0, 'good'),
                                            (df.gpa > 2.0, 'average')],
                                            else_='bad'))
    >>> print(whens_df)
       masters   gpa     stats programming  admitted   rating
    id
    5       no  3.44    Novice      Novice         0     good
    3       no  3.70    Novice    Beginner         1     good
    1      yes  3.95  Beginner    Beginner         0     good
    20     yes  3.90  Advanced    Advanced         1     good
    8       no  3.60  Beginner    Advanced         1     good
    25      no  3.96  Advanced    Advanced         1     good
    18     yes  3.81  Advanced    Advanced         1     good
    24      no  1.87  Advanced      Novice         1      bad
    26     yes  3.57  Advanced    Advanced         1     good
    38     yes  2.65  Advanced    Beginner         1  average
    >>> print(whens_df.shape)
    (40, 7)
    >>> # Example not specifying 'else_'
    >>> no_else =  df.assign(rating = case([(df.gpa > 3.0, 'good')]))
    >>> print(no_else)
       masters   gpa     stats programming  admitted  rating
    id
    5       no  3.44    Novice      Novice         0    good
    3       no  3.70    Novice    Beginner         1    good
    1      yes  3.95  Beginner    Beginner         0    good
    20     yes  3.90  Advanced    Advanced         1    good
    8       no  3.60  Beginner    Advanced         1    good
    25      no  3.96  Advanced    Advanced         1    good
    18     yes  3.81  Advanced    Advanced         1    good
    24      no  1.87  Advanced      Novice         1    None
    26     yes  3.57  Advanced    Advanced         1    good
    38     yes  2.65  Advanced    Beginner         1    None
    >>> print(no_else.shape)
    (40, 7)
    >>> # Example showing 'whens' passed a dictionary along with 'value'
    >>> whens_value_df = df.assign(admitted_text = case({ 1 : "admitted", 0 : "not admitted"},
                                                        value=df.admitted,
                                                        else_="don't know"))
    >>> print(whens_value_df)
       masters   gpa     stats programming  admitted  admitted_text
    id
    13      no  4.00  Advanced      Novice         1       admitted
    11      no  3.13  Advanced    Advanced         1       admitted
    9       no  3.82  Advanced    Advanced         1       admitted
    28      no  3.93  Advanced    Advanced         1       admitted
    33      no  3.55    Novice      Novice         1       admitted
    10      no  3.71  Advanced    Advanced         1       admitted
    16      no  3.70  Advanced    Advanced         1       admitted
    32     yes  3.46  Advanced    Beginner         0   not admitted
    34     yes  3.85  Advanced    Beginner         0   not admitted
    17      no  3.83  Advanced    Advanced         1       admitted
    >>> print(whens_value_df.shape)
    (40, 7)
    >>> # Example showing how you can decide on projecting a column based on the value of expression.
    >>> # In this example, you end up projecting values from column 'average_rating' if 2.0 < gpa <= 3.0,
    >>> # and the values from column 'good_rating' when gpa > 3.0, naming the column 'ga_rating'.
    >>> from sqlalchemy.sql import literal_column
    >>> whens_new_df = df.assign(good_rating = case([(df.gpa > 3.0, 'good')]))
    >>> whens_new_df = whens_new_df.assign(avg_rating = case([((whens_new_df.gpa > 2.0) & (whens_new_df.gpa <= 3.0),
                                                              'average')]))
    >>> literal_df = whens_new_df.assign(ga_rating = case([(whens_new_df.gpa > 3.0, literal_column('good_rating')),
                                                           (whens_new_df.gpa > 2.0, literal_column('avg_rating'))]))
    >>> print(literal_df)
       masters   gpa     stats programming  admitted good_rating   avg_rating ga_rating
    id
    5       no  3.44    Novice      Novice         0        good         None      good
    3       no  3.70    Novice    Beginner         1        good         None      good
    1      yes  3.95  Beginner    Beginner         0        good         None      good
    20     yes  3.90  Advanced    Advanced         1        good         None      good
    8       no  3.60  Beginner    Advanced         1        good         None      good
    25      no  3.96  Advanced    Advanced         1        good         None      good
    18     yes  3.81  Advanced    Advanced         1        good         None      good
    24      no  1.87  Advanced      Novice         1        None         None      None
    26     yes  3.57  Advanced    Advanced         1        good         None      good
    38     yes  2.65  Advanced    Beginner         1        None      average   average
to_numeric
teradataml.dataframe.sql_functions.to_numeric = to_numeric(arg, **kw)
Convert a string-like representation of a number to a Numeric type.
PARAMETERS:
  arg: DataFrame column
  kw: optional keyword arguments
    format_: string. Specifies the format of a string-like number to convert to numeric
    nls: dict where 'param' and 'value' are keys:
         - param specifies one of the following string values:
             -'CURRENCY', 'NUMERIC_CHARACTERS', 'DUAL_CURRENCY', 'ISO_CURRENCY'
         - value: specifies characters that are returned by number format elements.
                  See References for more information
REFERENCES:
  Chapter 14: Data Type Conversion Functions
  Teradata® Database SQL Functions, Operators, Expressions, and
  Predicates, Release 16.20
RETURNS:
  A DataFrame column of numeric type
NOTES:
  - If the arg column input is a numeric type, it is returned as is
  - Nulls may be introduced in the result if the parsing fails
  - You may need to strip() columns that have leading or trailing spaces
    in order for to_numeric to parse correctly
EXAMPLES:
  >>> df = DataFrame('numeric_strings')
                hex decimal commas numbers
      0        19FF   00.77   08,8       1
      1        abcd    0.77   0,88       1
      2  ABCDEFABCD   0.7.7   ,088     999
      3        2018    .077   088,       0
  >>> df.dtypes
      hex        str
      decimal    str
      commas     str
      numbers    str
  # converting string numbers to numeric
  >>> df.assign(drop_columns = True,
                numbers = df.numbers,
                numeric = to_numeric(df.numbers))
        numbers numeric
      0       1       1
      1       1       1
      2     999     999
      3       0       0
  # converting decimal-like strings to numeric
  # Note that strings not following the format return None
  >>> df.assign(drop_columns = True,
               decimal = df.decimal,
               numeric_dec = to_numeric(df.decimal))
        decimal numeric_dec
      0   00.77         .77
      1    0.77         .77
      2   0.7.7        None
      3    .077        .077
  # converting comma (group separated) strings to numeric
  # Note that strings not following the format return None
  >>> df.assign(drop_columns = True,
                commas = df.commas,
                numeric_commas = to_numeric(df.commas, format_ = '9G99'))
        commas numeric_commas
      0   08,8           None
      1   0,88             88
      2   ,088           None
      3   088,           None
  # converting hex strings to numeric
  >>> df.assign(drop_columns = True,
                hex = df.hex, 
                numeric_hex = to_numeric(df.hex, format_ = 'XXXXXXXXXX'))
                hex   numeric_hex
      0        19FF          6655
      1        abcd         43981
      2  ABCDEFABCD  737894443981
      3        2018          8216
  # converting literals to numeric
  >>> df.assign(drop_columns = True,
                a = to_numeric('123,456',format_ = '999,999'),
                b = to_numeric('1,333.555', format_ = '9,999D999'),
                c = to_numeric('2,333,2',format_ = '9G999G9'),
                d = to_numeric('3E20'),
                e = to_numeric('$41.99', format_ = 'L99.99'),
                f = to_numeric('$.12', format_ = 'L.99'),
                g = to_numeric('dollar123,456.00',
                               format_ = 'L999G999D99', 
                               nls = {'param': 'currency', 'value': 'dollar'})).head(1)
          a         b      c                         d      e    f       g
      0  123456  1333.555  23332 300000000000000000000  41.99  .12  123456
  # For more information on format elements and parameters, see the Reference.
translate
teradataml.dataframe.sql_functions.translate = translate(x, source='UNICODE', target='LATIN')
Returns a TRANSLATE(x USING source_TO_target) expression
PARAMETERS:
  x: A ColumnExpression instance coming from the DataFrame
     or output of other functions in sql_functions. A python
     string literal may also be used.
  source, target: str with values:
    - 'UNICODE'
    - 'LATIN'
REFERENCES:
  Chapter 28: String Operators and Functions
  Teradata® Database SQL Functions, Operators, Expressions, and
  Predicates, Release 16.20
EXAMPLES:
  >>> from teradataml.dataframe.sql_functions import translate
  >>> df = DataFrame('df')
  >>> tvshow = df['tvshow']
  >>> res = df.assign(tvshow = translate(tvshow))
Util Functions
async_run_status
teradataml.utils.utils.async_run_status = async_run_status(run_ids)
DESCRIPTION:
    Function to check the status of asynchronous run(s)
    using the unique run id(s).
PARAMETERS:
    run_ids:
        Required Argument.
        Specifies the unique identifier(s) of the asynchronous run.
        Types: str OR list of Strings (str)
RETURNS:
    Pandas DataFrame with columns as below:
        * Run Id: Unique identifier of the asynchronous run.
        * Run Description: Description of the asynchronous run.
        * Status: Status of the asynchronous run.
        * Timestamp: Timestamp for 'status'.
        * Additional Details: Addition information of the asynchronous run.
RAISES:
    None
EXAMPLES:
    # Examples to showcase the status of asynchronous run ids for OpenAF.
    # Example 1: Get the status of an environment that has been removed asynchronously.
    >>> env = create_env("testenv1", "python_3.7.13","test env 1")
    User environment 'testenv1' created.
    >>> remove_env("testenv1", asynchronous=True)
    Request to remove environment initiated successfully. Check the status using list_user_envs(). If environment is not remove
4285-41e1-8738-5f8895c180ee') or get_env('testenv1').status('1ba43e0a-4285-41e1-8738-5f8895c180ee')
    '1ba43e0a-4285-41e1-8738-5f8895c180ee'
    >>> async_run_status('1ba43e0a-4285-41e1-8738-5f8895c180ee')
                                     Run Id                      Run Description    Status             Timestamp Additional Det
    0  1ba43e0a-4285-41e1-8738-5f8895c180ee  Remove 'testenv1' user environment.   Started  2023-08-31T09:27:06Z
    1  1ba43e0a-4285-41e1-8738-5f8895c180ee  Remove 'testenv1' user environment.  Finished  2023-08-31T09:27:07Z
    # Example 2: Get the status of multiple asynchronous run ids for removed environments.
    >>> env1 = create_env("testenv1", "python_3.7.13","test env 1")
    >>> env2 = create_env("testenv2", "python_3.7.13","test env 2")
    User environment 'testenv1' created.
    User environment 'testenv2' created.
    # Remove 'testenv1' environment asynchronously.
    >>> remove_env("testenv1", asynchronous=True)
    Request to remove environment initiated successfully. Check the status using list_user_envs(). If environment is not remove
b124-45c7-80b1-6a4a826dc110') or get_env('testenv1').status('24812988-b124-45c7-80b1-6a4a826dc110')
    '24812988-b124-45c7-80b1-6a4a826dc110'
    # Remove 'testenv2' environment asynchronously.
    >>> remove_env("testenv2", asynchronous=True)
    Request to remove environment initiated successfully. Check the status using list_user_envs(). If environment is not remove
58bb-448b-81e2-979155cb8140') or get_env('testenv2').status('f686d756-58bb-448b-81e2-979155cb8140')
    'f686d756-58bb-448b-81e2-979155cb8140'
    # Check the status of claim IDs for asynchronously installed libraries and removed environments.
    >>> async_run_status(['24812988-b124-45c7-80b1-6a4a826dc110', 'f686d756-58bb-448b-81e2-979155cb8140'])
                                      Run Id                           Run Description        Status                   Timestam
    0       24812988-b124-45c7-80b1-6a4a826dc110    Remove 'testenv1' user environment.      Started        2023-08-
31T04:00:44Z
    1       24812988-b124-45c7-80b1-6a4a826dc110    Remove 'testenv1' user environment.     Finished        2023-08-
31T04:00:45Z
    2       f686d756-58bb-448b-81e2-979155cb8140    Remove 'testenv2' user environment.      Started        2023-08-
31T04:00:47Z
    3       f686d756-58bb-448b-81e2-979155cb8140    Remove 'testenv2' user environment.     Finished        2023-08-
31T04:00:48Z
execute_sql
teradataml.utils.utils.execute_sql = execute_sql(statement, parameters=None)
DESCRIPTION:
    Executes the SQL statement by using provided parameters.
    Note:
        Execution of stored procedures and user defined functions is not supported.
PARAMETERS:
    statement:
        Required Argument.
        Specifies the SQL statement to execute.
        Types: str
    parameters:
        Optional Argument.
        Specifies parameters to be used in case of parameterized query.
        Types: list of list, list of tuple
RETURNS:
    Cursor object.
RAISES:
    TeradataMlException, teradatasql.OperationalError, TypeError, ValueError
EXAMPLES:
    # Example 1: Create a table and insert values into the table using SQL.
    # Create a table.
    execute_sql("Create table table1 (col_1 int, col_2 varchar(10), col_3 float);")
    # Insert values in the table created above.
    execute_sql("Insert into table1 values (1, 'col_val', 2.0);")
    # Insert values in the table using a parameterized query.
    execute_sql(statement="Insert into table1 values (?, ?, ?);",
                parameters=[[1, 'col_val_1', 10.0],
                            [2, 'col_val_2', 20.0]])
    # Example 2: Execute parameterized 'SELECT' query.
    result_cursor = execute_sql(statement="Select * from table1 where col_1=? and col_3=?;",
                                parameters=[(1, 10.0),(1, 20.0)])
    # Example 3: Run Help Column query on table.
    result_cursor = execute_sql('Help column table1.*;')
Operators
Set Operator Functions
concat
teradataml.dataframe.setop.concat = concat(df_list, join='OUTER', allow_duplicates=True, sort=False, ignore_index=False)
DESCRIPTION:
    Concatenates a list of teradataml DataFrames, GeoDataFrames, or both along the index axis.
PARAMETERS:
    df_list:
        Required argument.
        Specifies a list of teradataml DataFrames, GeoDataFrames, or both on which the
        concatenation is to be performed.
        Types: list of teradataml DataFrames and/or GeoDataFrames
    join:
        Optional argument.
        Specifies how to handle indexes on columns axis.
        Supported values are:
        • 'OUTER': It instructs the function to project all columns from all the DataFrames.
                   Columns not present in any DataFrame will have a SQL NULL value.
        • 'INNER': It instructs the function to project only the columns common to all DataFrames.
        Default value: 'OUTER'
        Permitted values: 'INNER', 'OUTER'
        Types: str
    allow_duplicates:
        Optional argument.
        Specifies if the result of concatenation can have duplicate rows.
        Default value: True
        Types: bool
    sort:
        Optional argument.
        Specifies a flag to sort the columns axis if it is not already aligned when 
        the join argument is set to 'outer'.
        Default value: False
        Types: bool
    ignore_index:
        Optional argument.
        Specifies whether to ignore the index columns in resulting DataFrame or not.
        If True, then index columns will be ignored in the concat operation.
        Default value: False
        Types: bool
RETURNS:
    teradataml DataFrame, if result does not contain any geometry data, otherwise returns teradataml GeoDataFrame.
RAISES:
    TeradataMlException
EXAMPLES:
    >>> from teradataml import load_example_data
    >>> load_example_data("dataframe", "admissions_train")
    >>> load_example_data("geodataframe", ["sample_shapes"])
    >>> from teradataml.dataframe import concat
    >>>
    >>> # Default options
    >>> df = DataFrame('admissions_train')
    >>> df1 = df[df.gpa == 4].select(['id', 'stats', 'masters', 'gpa'])
    >>> df1
           stats masters  gpa
    id
    13  Advanced      no  4.0
    29    Novice     yes  4.0
    15  Advanced     yes  4.0
    >>> df2 = df[df.gpa < 2].select(['id', 'stats', 'programming', 'admitted'])
    >>> df2
           stats programming admitted
    id
    24  Advanced      Novice        1
    19  Advanced    Advanced        0
    >>> cdf = concat([df1, df2])
    >>> cdf
           stats masters  gpa programming admitted
    id
    19  Advanced    None  NaN    Advanced        0
    24  Advanced    None  NaN      Novice        1
    13  Advanced      no  4.0        None     None
    29    Novice     yes  4.0        None     None
    15  Advanced     yes  4.0        None     None
    >>>
    >>> # concat more than two DataFrames
    >>> df3 = df[df.gpa == 3].select(['id', 'stats', 'programming', 'gpa'])
    >>> df3
           stats programming  gpa
    id
    36  Advanced      Novice  3.0
    >>> cdf = concat([df1, df2, df3])
    >>> cdf
         stats masters  gpa programming  admitted
    id
    15  Advanced     yes  4.0        None       NaN
    19  Advanced    None  NaN    Advanced       0.0
    36  Advanced    None  3.0      Novice       NaN
    29    Novice     yes  4.0        None       NaN
    13  Advanced      no  4.0        None       NaN
    24  Advanced    None  NaN      Novice       1.0
    >>> # join = 'inner'
    >>> cdf = concat([df1, df2], join='inner')
    >>> cdf
           stats
    id
    19  Advanced
    24  Advanced
    13  Advanced
    29    Novice
    15  Advanced
    >>>
    >>> # allow_duplicates = True (default)
    >>> cdf = concat([df1, df2])
    >>> cdf
           stats masters  gpa programming admitted
    id
    19  Advanced    None  NaN    Advanced        0
    24  Advanced    None  NaN      Novice        1
    13  Advanced      no  4.0        None     None
    29    Novice     yes  4.0        None     None
    15  Advanced     yes  4.0        None     None
    >>> cdf = concat([cdf, df2])
    >>> cdf
           stats masters  gpa programming admitted
    id
    19  Advanced    None  NaN    Advanced        0
    13  Advanced      no  4.0        None     None
    24  Advanced    None  NaN      Novice        1
    24  Advanced    None  NaN      Novice        1
    19  Advanced    None  NaN    Advanced        0
    29    Novice     yes  4.0        None     None
    15  Advanced     yes  4.0        None     None
    >>>
    >>> # allow_duplicates = False
    >>> cdf = concat([cdf, df2], allow_duplicates=False)
    >>> cdf
           stats masters  gpa programming admitted
    id
    19  Advanced    None  NaN    Advanced        0
    29    Novice     yes  4.0        None     None
    24  Advanced    None  NaN      Novice        1
    15  Advanced     yes  4.0        None     None
    13  Advanced      no  4.0        None     None
    >>>
    >>> # sort = True
    >>> cdf = concat([df1, df2], sort=True)
    >>> cdf
       admitted  gpa masters programming     stats
    id
    19        0  NaN    None    Advanced  Advanced
    24        1  NaN    None      Novice  Advanced
    13     None  4.0      no        None  Advanced
    29     None  4.0     yes        None    Novice
    15     None  4.0     yes        None  Advanced
    >>> 
    >>> # ignore_index = True
    >>> cdf = concat([df1, df2], ignore_index=True)
    >>> cdf
          stats masters  gpa programming  admitted
    0  Advanced     yes  4.0        None       NaN
    1  Advanced    None  NaN    Advanced       0.0
    2    Novice     yes  4.0        None       NaN
    3  Advanced    None  NaN      Novice       1.0
    4  Advanced      no  4.0        None       NaN
    # Perform concatenation of two GeoDataFrames
    >>> geo_dataframe = GeoDataFrame('sample_shapes')
    >>> geo_dataframe1 = geo_dataframe[geo_dataframe.skey == 1004].select(['skey','linestrings'])
    >>> geo_dataframe1
    skey            linestrings
    1004  LINESTRING (10 20 30,40 50 60,70 80 80)
    >>> geo_dataframe2 = geo_dataframe[geo_dataframe.skey < 1010].select(['skey','polygons'])
    >>> geo_dataframe2
    skey                                                              polygons
    1009                               MULTIPOLYGON (((0 0 0,0 20 20,20 20 20,20 0 20,0 0 0)),
((50 50 50,50 90 90,90 90 90,90 50 90,50 50 50)))
    1005  POLYGON ((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.435,20.435 20.435 0,20.435 20.43
    1004                                                POLYGON ((0 0 0,0 10 20,20 20 30,20 10 0,0 0 0),
(5 5 5,5 10 10,10 10 10,10 10 5,5 5 5))
    1002                                                                          POLYGON ((0 0,0 20,20 20,20 0,0 0),
(5 5,5 10,10 10,10 5,5 5))
    1001                                                                                                    POLYGON ((0 0,0 20,
    1003                                                                                POLYGON ((0.6 0.8,0.6 20.8,20.6 20.8,20
    1007                                                                  MULTIPOLYGON (((1 1,1 3,6 3,6 0,1 1)),
((10 5,10 10,20 10,20 5,10 5)))
    1006                                                          POLYGON ((0 0 0,0 0 20,0 20 0,0 20 20,20 0 0,20 0 20,20 20 0,
    1008                                             MULTIPOLYGON (((0 0,0 20,20 20,20 0,0 0)),
((0.6 0.8,0.6 20.8,20.6 20.8,20.6 0.8,0.6 0.8)))
    >>> concat([geo_dataframe1,geo_dataframe2])
    skey                                    linestrings                                 polygons
    1009                                     None                               MULTIPOLYGON (((0 0 0,0 20 20,20 20 20,20 0 20,
((50 50 50,50 90 90,90 90 90,90 50 90,50 50 50)))
    1005                                     None  POLYGON ((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.43
    1004  LINESTRING (10 20 30,40 50 60,70 80 80)                                                                              
    1004                                     None                                                POLYGON ((0 0 0,0 10 20,20 20 
(5 5 5,5 10 10,10 10 10,10 10 5,5 5 5))
    1003                                     None                                                                              
    1001                                     None                                                                              
    1002                                     None                                                                          POLY
(5 5,5 10,10 10,10 5,5 5))
    1007                                     None                                                                  MULTIPOLYGON
((10 5,10 10,20 10,20 5,10 5)))
    1006                                     None                                                          POLYGON ((0 0 0,0 0 
    1008                                     None                                             MULTIPOLYGON (((0 0,0 20,20 20,20
((0.6 0.8,0.6 20.8,20.6 20.8,20.6 0.8,0.6 0.8)))
    # Perform concatenation of a DataFrame and GeoDataFrame which returns a GeoDataFrame.
    >>> normal_df=df.select(['id','stats'])
    >>> normal_df
        stats
    id
    34  Advanced
    32  Advanced
    11  Advanced
    40    Novice
    38  Advanced
    36  Advanced
    7     Novice
    26  Advanced
    19  Advanced
    13  Advanced
    >>> geo_df = geo_dataframe[geo_dataframe.skey < 1010].select(['skey', 'polygons'])
    >>> geo_df
    skey                                                                            polygons
    1003                                                                                POLYGON ((0.6 0.8,0.6 20.8,20.6 20.8,20
    1008                                             MULTIPOLYGON (((0 0,0 20,20 20,20 0,0 0)),
((0.6 0.8,0.6 20.8,20.6 20.8,20.6 0.8,0.6 0.8)))
    1006                                                          POLYGON ((0 0 0,0 0 20,0 20 0,0 20 20,20 0 0,20 0 20,20 20 0,
    1009                               MULTIPOLYGON (((0 0 0,0 20 20,20 20 20,20 0 20,0 0 0)),
((50 50 50,50 90 90,90 90 90,90 50 90,50 50 50)))
    1005  POLYGON ((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.435,20.435 20.435 0,20.435 20.43
    1007                                                                  MULTIPOLYGON (((1 1,1 3,6 3,6 0,1 1)),
((10 5,10 10,20 10,20 5,10 5)))
    1001                                                                                                    POLYGON ((0 0,0 20,
    1002                                                                          POLYGON ((0 0,0 20,20 20,20 0,0 0),
(5 5,5 10,10 10,10 5,5 5))
    1004                                                POLYGON ((0 0 0,0 10 20,20 20 30,20 10 0,0 0 0),
(5 5 5,5 10 10,10 10 10,10 10 5,5 5 5))
    >>> idf = concat([normal_df, geo_df])
    >>> idf
        stats     skey   polygons
    id
    38  Advanced  None     None
    7     Novice  None     None
    26  Advanced  None     None
    17  Advanced  None     None
    34  Advanced  None     None
    13  Advanced  None     None
    32  Advanced  None     None
    11  Advanced  None     None
    15  Advanced  None     None
    36  Advanced  None     None
>>>
td_intersect
teradataml.dataframe.setop.td_intersect = td_intersect(df_list, allow_duplicates=True)
DESCRIPTION:
    Function intersects a list of teradataml DataFrames or GeoDataFrames along the index axis and
    returns a DataFrame with rows common to all input DataFrames.
    Note:
         This function should be applied to data frames of the same type: either all teradataml DataFrames,
         or all GeoDataFrames.
PARAMETERS:
    df_list:
        Required argument.
        Specifies the list of teradataml DataFrames or GeoDataFrames on which the intersection is to be performed.
        Types: list of teradataml DataFrames or GeoDataFrames
    allow_duplicates:
        Optional argument.
        Specifies if the result of intersection can have duplicate rows.
        Default value: True
        Types: bool
RETURNS:
    teradataml DataFrame when intersect is performed on teradataml DataFrames.
    teradataml GeoDataFrame when operation is performed on teradataml GeoDataFrames.
RAISES:
    TeradataMlException, TypeError
EXAMPLES:
    >>> from teradataml import load_example_data
    >>> load_example_data("dataframe", "setop_test1")
    >>> load_example_data("dataframe", "setop_test2")
    >>> load_example_data("geodataframe", ["sample_shapes"])
    >>> from teradataml.dataframe.setop import td_intersect
    >>>
    >>> df1 = DataFrame('setop_test1')
    >>> df1
       masters   gpa     stats programming  admitted
    id                                              
    62      no  3.70  Advanced    Advanced         1
    53     yes  3.50  Beginner      Novice         1
    69      no  3.96  Advanced    Advanced         1
    61     yes  4.00  Advanced    Advanced         1
    58      no  3.13  Advanced    Advanced         1
    51     yes  3.76  Beginner    Beginner         0
    68      no  1.87  Advanced      Novice         1
    66      no  3.87    Novice    Beginner         1
    60      no  4.00  Advanced      Novice         1
    59      no  3.65    Novice      Novice         1
    >>> df2 = DataFrame('setop_test2')
    >>> df2
       masters   gpa     stats programming  admitted
    id                                              
    12      no  3.65    Novice      Novice         1
    15     yes  4.00  Advanced    Advanced         1
    14     yes  3.45  Advanced    Advanced         0
    20     yes  3.90  Advanced    Advanced         1
    18     yes  3.81  Advanced    Advanced         1
    17      no  3.83  Advanced    Advanced         1
    13      no  4.00  Advanced      Novice         1
    11      no  3.13  Advanced    Advanced         1
    60      no  4.00  Advanced      Novice         1
    19     yes  1.98  Advanced    Advanced         0
    >>> idf = td_intersect([df1, df2])
    >>> idf
       masters   gpa     stats programming  admitted
    id                                              
    64     yes  3.81  Advanced    Advanced         1
    60      no  4.00  Advanced      Novice         1
    58      no  3.13  Advanced    Advanced         1
    68      no  1.87  Advanced      Novice         1
    66      no  3.87    Novice    Beginner         1
    60      no  4.00  Advanced      Novice         1
    62      no  3.70  Advanced    Advanced         1
    >>>
    >>> idf = td_intersect([df1, df2], allow_duplicates=False)
    >>> idf
       masters   gpa     stats programming  admitted
    id                                              
    64     yes  3.81  Advanced    Advanced         1
    60      no  4.00  Advanced      Novice         1
    58      no  3.13  Advanced    Advanced         1
    68      no  1.87  Advanced      Novice         1
    66      no  3.87    Novice    Beginner         1
    62      no  3.70  Advanced    Advanced         1
    >>> # intersecting more than two DataFrames
    >>> df3 = df1[df1.gpa <= 3.5]
    >>> df3
       masters   gpa     stats programming  admitted
    id                                              
    58      no  3.13  Advanced    Advanced         1
    67     yes  3.46    Novice    Beginner         0
    54     yes  3.50  Beginner    Advanced         1
    68      no  1.87  Advanced      Novice         1
    53     yes  3.50  Beginner      Novice         1
    >>> idf = td_intersect([df1, df2, df3])
    >>> idf
       masters   gpa     stats programming  admitted
    id                                              
    58      no  3.13  Advanced    Advanced         1
    68      no  1.87  Advanced      Novice         1
    # Perform intersection of two GeoDataFrames.
    >>> geo_dataframe = GeoDataFrame('sample_shapes')
    >>> geo_dataframe1 = geo_dataframe[geo_dataframe.skey == 1004].select(['skey','linestrings'])
    >>> geo_dataframe1
    skey            linestrings
    1004  LINESTRING (10 20 30,40 50 60,70 80 80)
    >>> geo_dataframe2 = geo_dataframe[geo_dataframe.skey < 1010].select(['skey','linestrings'])
    >>> geo_dataframe2
    skey                        linestrings
    1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
    1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
    1004                                LINESTRING (10 20 30,40 50 60,70 80 80)
    1002                                               LINESTRING (1 3,3 0,0 1)
    1001                                           LINESTRING (1 1,2 2,3 3,4 4)
    1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
    1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
    1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
    >>> td_intersect([geo_dataframe1,geo_dataframe2])
    skey            linestrings
    1004  LINESTRING (10 20 30,40 50 60,70 80 80)
td_except
teradataml.dataframe.setop.td_except = td_except(df_list, allow_duplicates=True)
DESCRIPTION:
    This function returns the resulting rows that appear in first teradataml DataFrame or GeoDataFrame
    and not in other teradataml DataFrames or GeoDataFrames along the index axis.
    Note:
         This function should be applied to data frames of the same type: either all teradataml DataFrames,
         or all GeoDataFrames.
PARAMETERS:
    df_list:
        Required argument.
        Specifies the list of teradataml DataFrames or GeoDataFrames on which the except
        operation is to be performed.
        Types: list of teradataml DataFrames or GeoDataFrames
    allow_duplicates:
        Optional argument.
        Specifies if the result of except operation can have duplicate rows.
        Default value: True
        Types: bool
RETURNS:
    teradataml DataFrame when operation is performed on teradataml DataFrames.
    teradataml GeoDataFrame when operation is performed on teradataml GeoDataFrames.
RAISES:
    TeradataMlException, TypeError
EXAMPLES:
    >>> from teradataml import load_example_data
    >>> load_example_data("dataframe", "setop_test1")
    >>> load_example_data("dataframe", "setop_test2")
    >>> load_example_data("geodataframe", ["sample_shapes"])
    >>> from teradataml.dataframe.setop import td_except
    >>>
    >>> df1 = DataFrame('setop_test1')
    >>> df1
       masters   gpa     stats programming  admitted
    id                                              
    62      no  3.70  Advanced    Advanced         1
    53     yes  3.50  Beginner      Novice         1
    69      no  3.96  Advanced    Advanced         1
    61     yes  4.00  Advanced    Advanced         1
    58      no  3.13  Advanced    Advanced         1
    51     yes  3.76  Beginner    Beginner         0
    68      no  1.87  Advanced      Novice         1
    66      no  3.87    Novice    Beginner         1
    60      no  4.00  Advanced      Novice         1
    59      no  3.65    Novice      Novice         1
    >>> df2 = DataFrame('setop_test2')
    >>> df2
       masters   gpa     stats programming  admitted
    id                                              
    12      no  3.65    Novice      Novice         1
    15     yes  4.00  Advanced    Advanced         1
    14     yes  3.45  Advanced    Advanced         0
    20     yes  3.90  Advanced    Advanced         1
    18     yes  3.81  Advanced    Advanced         1
    17      no  3.83  Advanced    Advanced         1
    13      no  4.00  Advanced      Novice         1
    11      no  3.13  Advanced    Advanced         1
    60      no  4.00  Advanced      Novice         1
    19     yes  1.98  Advanced    Advanced         0
    >>> idf = td_except([df1[df1.id<55] , df2])
    >>> idf
       masters   gpa     stats programming  admitted
    id                                              
    51     yes  3.76  Beginner    Beginner         0
    50     yes  3.95  Beginner    Beginner         0
    54     yes  3.50  Beginner    Advanced         1
    52      no  3.70    Novice    Beginner         1
    53     yes  3.50  Beginner      Novice         1
    53     yes  3.50  Beginner      Novice         1
    >>>
    >>> idf = td_except([df1[df1.id<55] , df2], allow_duplicates=False)
    >>> idf
       masters   gpa     stats programming  admitted
    id                                              
    54     yes  3.50  Beginner    Advanced         1
    51     yes  3.76  Beginner    Beginner         0
    53     yes  3.50  Beginner      Novice         1
    50     yes  3.95  Beginner    Beginner         0
    52      no  3.70    Novice    Beginner         1
    >>> # applying except on more than two DataFrames
    >>> df3 = df1[df1.gpa <= 3.9]
    >>> idf = td_except([df1, df2, df3])
    >>> idf
       masters   gpa     stats programming  admitted
    id                                              
    61     yes  4.00  Advanced    Advanced         1
    50     yes  3.95  Beginner    Beginner         0
    69      no  3.96  Advanced    Advanced         1
    # td_except on GeoDataFrames
    >>> geo_dataframe = GeoDataFrame('sample_shapes')
    >>> geo_dataframe1 = geo_dataframe[geo_dataframe.skey == 1004].select(['skey','linestrings'])
    >>> geo_dataframe1
    skey        linestrings
    1004  LINESTRING (10 20 30,40 50 60,70 80 80)
    >>> geo_dataframe2 = geo_dataframe[geo_dataframe.skey < 1010].select(['skey','linestrings'])
    >>> geo_dataframe2
    skey                                linestrings
    1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
    1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
    1004                                LINESTRING (10 20 30,40 50 60,70 80 80)
    1002                                               LINESTRING (1 3,3 0,0 1)
    1001                                           LINESTRING (1 1,2 2,3 3,4 4)
    1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
    1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
    1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
    >>> td_except([geo_dataframe2,geo_dataframe1])
    skey                    linestrings
    1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
    1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
    1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
    1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
    1001                                           LINESTRING (1 1,2 2,3 3,4 4)
    1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
    1002                                               LINESTRING (1 3,3 0,0 1)
td_minus
teradataml.dataframe.setop.td_minus = td_minus(df_list, allow_duplicates=True)
DESCRIPTION:
    This function returns the resulting rows that appear in first teradataml DataFrame or GeoDataFrame
    and not in other teradataml DataFrames or GeoDataFrames along the index axis.
    Note:
         This function should be applied to data frames of the same type: either all teradataml DataFrames,
         or all GeoDataFrames.
PARAMETERS:
    df_list:
        Required argument.
        Specifies the list of teradataml DataFrames or GeoDataFrames on which the minus
        operation is to be performed.
        Types: list of teradataml DataFrames or GeoDataFrames
    allow_duplicates:
        Optional argument.
        Specifies if the result of minus operation can have duplicate rows.
        Default value: True
        Types: bool
RETURNS:
    teradataml DataFrame when operation is performed on teradataml DataFrames.
    teradataml GeoDataFrame when operation is performed on teradataml GeoDataFrames.
RAISES:
    TeradataMlException, TypeError
EXAMPLES:
    >>> from teradataml import load_example_data
    >>> load_example_data("dataframe", "setop_test1")
    >>> load_example_data("dataframe", "setop_test2")
    >>> load_example_data("geodataframe", ["sample_shapes"])
    >>> from teradataml.dataframe.setop import td_minus
    >>>
    >>> df1 = DataFrame('setop_test1')
    >>> df1
       masters   gpa     stats programming  admitted
    id                                              
    62      no  3.70  Advanced    Advanced         1
    53     yes  3.50  Beginner      Novice         1
    69      no  3.96  Advanced    Advanced         1
    61     yes  4.00  Advanced    Advanced         1
    58      no  3.13  Advanced    Advanced         1
    51     yes  3.76  Beginner    Beginner         0
    68      no  1.87  Advanced      Novice         1
    66      no  3.87    Novice    Beginner         1
    60      no  4.00  Advanced      Novice         1
    59      no  3.65    Novice      Novice         1
    >>> df2 = DataFrame('setop_test2')
    >>> df2
       masters   gpa     stats programming  admitted
    id                                              
    12      no  3.65    Novice      Novice         1
    15     yes  4.00  Advanced    Advanced         1
    14     yes  3.45  Advanced    Advanced         0
    20     yes  3.90  Advanced    Advanced         1
    18     yes  3.81  Advanced    Advanced         1
    17      no  3.83  Advanced    Advanced         1
    13      no  4.00  Advanced      Novice         1
    11      no  3.13  Advanced    Advanced         1
    60      no  4.00  Advanced      Novice         1
    19     yes  1.98  Advanced    Advanced         0
    >>> idf = td_minus([df1[df1.id<55] , df2])
    >>> idf
       masters   gpa     stats programming  admitted
    id                                              
    51     yes  3.76  Beginner    Beginner         0
    50     yes  3.95  Beginner    Beginner         0
    54     yes  3.50  Beginner    Advanced         1
    52      no  3.70    Novice    Beginner         1
    53     yes  3.50  Beginner      Novice         1
    53     yes  3.50  Beginner      Novice         1
    >>>
    >>> idf = td_minus([df1[df1.id<55] , df2], allow_duplicates=False)
    >>> idf
       masters   gpa     stats programming  admitted
    id                                              
    54     yes  3.50  Beginner    Advanced         1
    51     yes  3.76  Beginner    Beginner         0
    53     yes  3.50  Beginner      Novice         1
    50     yes  3.95  Beginner    Beginner         0
    52      no  3.70    Novice    Beginner         1
    >>> # applying minus on more than two DataFrames
    >>> df3 = df1[df1.gpa <= 3.9]
    >>> idf = td_minus([df1, df2, df3])
    >>> idf
       masters   gpa     stats programming  admitted
    id                                              
    61     yes  4.00  Advanced    Advanced         1
    50     yes  3.95  Beginner    Beginner         0
    69      no  3.96  Advanced    Advanced         1
    # td_minus on GeoDataFrame
    >>> geo_dataframe = GeoDataFrame('sample_shapes')
    >>> geo_dataframe1 = geo_dataframe[geo_dataframe.skey == 1004].select(['skey','linestrings'])
    >>> geo_dataframe1
    skey        linestrings
    1004  LINESTRING (10 20 30,40 50 60,70 80 80)
    >>> geo_dataframe2 = geo_dataframe[geo_dataframe.skey < 1010].select(['skey','linestrings'])
    >>> geo_dataframe2
    skey                                linestrings
    1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
    1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
    1004                                LINESTRING (10 20 30,40 50 60,70 80 80)
    1002                                               LINESTRING (1 3,3 0,0 1)
    1001                                           LINESTRING (1 1,2 2,3 3,4 4)
    1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
    1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
    1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
    >>> td_minus([geo_dataframe2,geo_dataframe1])
                                                                linestrings
    skey
    1005                                         LINESTRING (1 3 6,3 0 6,6 0 1)
    1009            MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
    1002                                               LINESTRING (1 3,3 0,0 1)
    1007                            MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
    1008  MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
    1006           LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
    1003                       LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    1001                                           LINESTRING (1 1,2 2,3 3,4 4)
Table Operators: Script
__init__
teradataml.table_operators.Script.__init__ = __init__(self, data=None, script_name=None, ﬁles_local_path=None, script_command=None, delimiter='\t', returns=None,
auth=None, charset=None, quotechar=None, data_partition_column=None, data_hash_column=None, data_order_column=None, is_local_order=False, sort_ascending=True,
nulls_ﬁrst=True)
DESCRIPTION:
    The Script table operator function executes a user-installed script or
    any LINUX command inside database on Teradata Vantage.
PARAMETERS:
    script_command:
        Required Argument.
        Specifies the command/script to run.
        Types: str
    script_name:
        Required Argument.
        Specifies the name of user script.
        User script should have at least permissions of mode 644.
        Types: str
    files_local_path:
        Required Argument.
        Specifies the absolute local path where user script and all supporting
        files like model files, input data file reside.
        Types: str
    returns:
        Required Argument.
        Specifies output column definition.
        Types: Dictionary specifying column name to teradatasqlalchemy type mapping.
        Default: None
        Note:
            User can pass a dictionary (dict or OrderedDict) to the "returns" argument,
            with the keys ordered to represent the order of the output columns.
            Preferred type is OrderedDict.
    data:
        Optional Argument.
        Specifies a teradataml DataFrame containing the input data for the
        script.
    data_hash_column:
        Optional Argument.
        Specifies the column to be used for hashing.
        The rows in the data are redistributed to AMPs based on the hash value of
        the column specified.
        The user-installed script file then runs once on each AMP.
        If there is no "data_partition_column", then the entire result set,
        delivered by the function, constitutes a single group or partition.
        Types: str
        Note:
            "data_hash_column" can not be specified along with
            "data_partition_column", "is_local_order" and "data_order_column".
    data_partition_column:
        Optional Argument.
        Specifies Partition By columns for "data".
        Values to this argument can be provided as a list, if multiple
        columns are used for partition.
        Default Value: ANY
        Types: str OR list of Strings (str)
        Note:
            1) "data_partition_column" can not be specified along with
               "data_hash_column".
            2) "data_partition_column" can not be specified along with
               "is_local_order = True".
    is_local_order:
        Optional Argument.
        Specifies a boolean value to determine whether the input data is to be
        ordered locally or not. Order by specifies the order in which the
        values in a group, or partition, are sorted. Local Order By specifies
        orders qualified rows on each AMP in preparation to be input to a table
        function. This argument is ignored, if "data_order_column" is None. When
        set to True, data is ordered locally.
        Default Value: False
        Types: bool
        Note:
            1) "is_local_order" can not be specified along with "data_hash_column".
            2) When "is_local_order" is set to True, "data_order_column" should be
               specified, and the columns specified in "data_order_column" are
               used for local ordering.
    data_order_column:
        Optional Argument.
        Specifies Order By columns for "data".
        Values to this argument can be provided as a list, if multiple
        columns are used for ordering. This argument is used with in both cases:
        "is_local_order = True" and "is_local_order = False".
        Types: str OR list of Strings (str)
        Note:
            "data_order_column" can not be specified along with "data_hash_column".
    sort_ascending:
        Optional Argument.
        Specifies a boolean value to determine if the result set is to be sorted
        on the "data_order_column" column in ascending or descending order.
        The sorting is ascending when this argument is set to True, and descending
        when set to False. This argument is ignored, if "data_order_column" is
        None.
        Default Value: True
        Types: bool
    nulls_first:
        Optional Argument.
        Specifies a boolean value to determine whether NULLS are listed first or
        last during ordering. This argument is ignored, if "data_order_column" is
        None. NULLS are listed first when this argument is set to True, and last
        when set to False.
        Default Value: True
        Types: bool
    delimiter:
        Optional Argument.
        Specifies a delimiter to use when reading columns from a row and
        writing result columns.
        Default Value: "        " (tab)
        Types: str of length 1 character
        Notes:
            1) This argument cannot be same as "quotechar" argument.
            2) This argument cannot be a newline character i.e., '\n'.
    auth:
        Optional Argument.
        Specifies an authorization to use when running the script.
        Types: str
    charset:
        Optional Argument.
        Specifies the character encoding for data.
        Permitted Values: utf-16, latin
        Types: str
    quotechar:
        Optional Argument.
        Specifies a character that forces all input and output of the script
        to be quoted using this specified character.
        Using this argument enables the Advanced SQL Engine to distinguish
        between NULL fields and empty strings. A string with length zero is
        quoted, while NULL fields are not.
        If this character is found in the data, it will be escaped by a second
        quote character.
        Types: character of length 1
        Notes:
            1) This argument cannot be same as "delimiter" argument.
            2) This argument cannot be a newline character i.e., '\n'.
RETURNS:
    Script Object
RAISES:
    TeradataMlException
EXAMPLES:
    # Note - Refer to User Guide for setting search path and required permissions.
    # Load example data.
    load_example_data("Script", ["barrier"])
    # Example - The script mapper.py reads in a line of text input
    # ("Old Macdonald Had A Farm") from csv and splits the line into individual
    # words, emitting a new row for each word.
    # Create teradataml DataFrame objects.
    >>> barrierdf = DataFrame.from_table("barrier")
    # Set SEARCHUIFDBPATH.
    >>> execute_sql("SET SESSION SEARCHUIFDBPATH = alice;")
    # Create a Script object that allows us to execute script on Vantage.
    >>> import teradataml, os
    >>> from teradatasqlalchemy import VARCHAR
    >>> td_path = os.path.dirname(teradataml.__file__)
    >>> sto = Script(data=barrierdf,
    ...              script_name='mapper.py',
    ...              files_local_path= os.path.join(td_path, 'data', 'scripts'),
    ...              script_command='tdpython3 ./alice/mapper.py',
    ...              data_order_column="Id",
    ...              is_local_order=False,
    ...              nulls_first=False,
    ...              sort_ascending=False,
    ...              charset='latin',
    ...              returns=OrderedDict([("word", VARCHAR(15)),("count_input", VARCHAR(2))]))
    # Run user script locally and using data from csv.
    >>> sto.test_script(input_data_file='../barrier.csv', data_file_delimiter=',')
    ############ STDOUT Output ############
            word  count_input
    0          1            1
    1        Old            1
    2  Macdonald            1
    3        Had            1
    4          A            1
    5       Farm            1
    >>>
    # Script results look good. Now install file on Vantage.
    >>> sto.install_file(file_identifier='mapper',
    ...                  file_name='mapper.py',
    ...                  is_binary=False)
    File mapper.py installed in Vantage
    # Execute the user script on Vantage.
    >>> sto.execute_script()
    ############ STDOUT Output ############
            word count_input
    0  Macdonald           1
    1          A           1
    2       Farm           1
    3        Had           1
    4        Old           1
    5          1           1
    # Remove the installed file from Vantage.
    >>> sto.remove_file(file_identifier='mapper', force_remove=True)
    File mapper removed from Vantage
deploy
teradataml.table_operators.Script.deploy = deploy(self, model_column, partition_columns=None, model_ﬁle_preﬁx=None, retry=3, retry_timeout=30)
DESCRIPTION:
    Function deploys the models generated after running `execute_script()` in database in
    VantageCloud Enterprise or in user environment in VantageCloud Lake.
    If deployed files are not needed, these files can be removed using `remove_file()` in
    database or `UserEnv.remove_file()` in lake.
    Note:
        If the models (one or many) fail to get deployed in Vantage even after retries,
        try deploying them again using `install_file()` function or remove installed
        files using `remove_file()` function.
PARAMETERS:
    model_column:
        Required Argument.
        Specifies the column name in which models are present.
        Supported types of model in this column are CLOB and BLOB.
        Note:
            The column mentioned in this argument should be present in
            <apply_obj/script_obj>.result.
        Types: str
    partition_columns:
        Optional Argument.
        Specifies the columns on which data is partitioned.
        Note:
            The columns mentioned in this argument should be present in
            <apply_obj/script_obj>.result.
        Types: str OR list of str
    model_file_prefix:
        Optional Argument.
        Specifies the prefix to be used to the generated model file.
        If this argument is None, prefix is auto-generated.
        If the argument "model_column" contains multiple models and
            * "partition_columns" is None - model file prefix is appended with
              underscore(_) and numbers starting from one(1) to get model file
              names.
            * "partition_columns" is NOT None - model file prefix is appended
              with underscore(_) and unique values in partition_columns are joined
              with underscore(_) to generate model file names.
        Types: str
    retry:
        Optional Argument.
        Specifies the maximum number of retries to be made to deploy the models.
        This argument helps in retrying the deployment of models in case of network issues.
        This argument should be a positive integer.
        Default Value: 3
        Types: int
    retry_timeout:
        Optional Argument. Used along with retry argument. Ignored otherwise.
        Specifies the time interval in seconds between each retry.
        This argument should be a positive integer.
        Default Value: 30
        Types: int
RETURNS:
    List of generated file identifiers in database or file names in lake.
RAISES:
    - TeradatamlException
    - Throws warning when models failed to deploy even after retries.
EXAMPLES:
    >>> import teradataml
    >>> from teradataml import load_example_data
    >>> load_example_data("openml", "multi_model_classification")
    >>> df = DataFrame("multi_model_classification")
    >>> df
                   col2      col3      col4  label  group_column  partition_column_1  partition_column_2
    col1
    -1.013454  0.855765 -0.256920 -0.085301      1             9                   0                  10
    -3.146552 -1.805530 -0.071515 -2.093998      0            10                   0                  10
    -1.175097 -0.950745  0.018280 -0.895335      1            10                   0                  11
     0.218497 -0.968924  0.183037 -0.303142      0            11                   0                  11
    -1.471908 -0.029195 -0.166141 -0.645309      1            11                   1                  10
     1.082336  0.846357 -0.012063  0.812633      1            11                   1                  11
    -1.132068 -1.209750  0.065422 -0.982986      0            10                   1                  10
    -0.440339  2.290676 -0.423878  0.749467      1             8                   1                  10
    -0.615226 -0.546472  0.017496 -0.488720      0            12                   0                  10
     0.579671 -0.573365  0.160603  0.014404      0             9                   1                  10
    ## Run in VantageCloud Enterprise using Script object.
    # Install Script file.
    >>> file_location = os.path.join(os.path.dirname(teradataml.__file__), "data", "scripts", "deploy_script.py")
    >>> install_file("deploy_script", file_location, replace=True)
    >>> execute_sql("SET SESSION SEARCHUIFDBPATH = <db_name>;")
    # Variables needed for Script execution.
    >>> from teradataml import configure
    >>> script_command = f'{configure.indb_install_location} ./<db_name>/deploy_script.py enterprise'
    >>> partition_columns = ["partition_column_1", "partition_column_2"]
    >>> columns = ["col1", "col2", "col3", "col4", "label",
                   "partition_column_1", "partition_column_2"]
    >>> returns = OrderedDict([("partition_column_1", INTEGER()),
                               ("partition_column_2", INTEGER()),
                               ("model", CLOB())])
    # Script execution.
    >>> obj = Script(data=df.select(columns),
                     script_command=script_command,
                     data_partition_column=partition_columns,
                     returns=returns
                     )
    >>> opt = obj.execute_script()
    >>> opt
    partition_column_1  partition_column_2               model                                                                 
                    0                  10   b'gAejc1.....drIr'
                    0                  11   b'gANjcw.....qWIu'
                    1                  10   b'abdwcd.....dWIz'
                    1                  11   b'gA4jc4.....agfu'
    # Example 1: Provide only "partition_columns" argument. Here, "model_file_prefix" 
    #            is auto generated.
    >>> obj.deploy(model_column="model",
                   partition_columns=["partition_column_1", "partition_column_2"])
    ['model_file_1710436227163427__0_10',
     'model_file_1710436227163427__1_10',
     'model_file_1710436227163427__0_11',
     'model_file_1710436227163427__1_11']
    # Example 2: Provide only "model_file_prefix" argument. Here, filenames are suffixed 
    #            with 1, 2, 3, ... for multiple models.
    >>> obj.deploy(model_column="model", model_file_prefix="my_prefix_new_")
    ['my_prefix_new__1',
     'my_prefix_new__2',
     'my_prefix_new__3',
     'my_prefix_new__4']
    # Example 3: Without both "partition_columns" and "model_file_prefix" arguments.
    >>> obj.deploy(model_column="model")
    ['model_file_1710438346528596__1',
     'model_file_1710438346528596__2',
     'model_file_1710438346528596__3',
     'model_file_1710438346528596__4']
    # Example 4: Provide both "partition_columns" and "model_file_prefix" arguments.
    >>> obj.deploy(model_column="model", model_file_prefix="my_prefix_new_", 
                   partition_columns=["partition_column_1", "partition_column_2"])
    ['my_prefix_new__0_10',
     'my_prefix_new__0_11',
     'my_prefix_new__1_10',
     'my_prefix_new__1_11']
    # Example 5: Assuming that 2 model files fail to get installed due to network issues,
    #            the function retries installing the failed files twice with timeout between
    #            retries of 10 secs.
    >>> opt = obj.deploy(model_column="model", model_file_prefix="my_prefix_",
                         partition_columns=["partition_column_1", "partition_column_2"],
                         retry=2, retry_timeout=10)
    RuntimeWarning: The following model files failed to get installed in Vantage:
    ['my_prefix__1_10', 'my_prefix__1_11'].
    Try manually deploying them from the path '<temp_path>' using:
        - `install_file()` when connected to Enterprise/On-Prem system or
        - `UserEnv.install_file()` when connected to Lake system.
    OR
    Remove the returned installed files manually using `remove_file()` or `UserEnv.remove_file()`.
    >>> opt
    ['my_prefix__0_10',
     'my_prefix__0_11']
    ## Run in VantageCloud Lake using Apply object.
    # Let's assume an user environment named "user_env" already exists in VantageCloud Lake,
    # which will be used for the examples below.
    # ApplyTableOperator returns BLOB type for model column as per deploy_script.py.
    >>> returns = OrderedDict([("partition_column_1", INTEGER()),
                               ("partition_column_2", INTEGER()),
                               ("model", BLOB())])
    # Install the script file which returns model and partition columns.
    >>> user_env.install_file(file_location)
    >>> script_command = 'python3 deploy_script.py lake'
    >>> obj = Apply(data=df.select(columns),
                    script_command=script_command,
                    data_partition_column=partition_columns,
                    returns=returns,
                    env_name="user_env"
                    )
    >>> opt = obj.execute_script()
    >>> opt
    partition_column_1  partition_column_2               model                                                                 
                    0                  10   b'gAejc1.....drIr'
                    0                  11   b'gANjcw.....qWIu'
                    1                  10   b'abdwcd.....dWIz'
                    1                  11   b'gA4jc4.....agfu'
    # Example 6: Provide both "partition_columns" and "model_file_prefix" arguments.
    >>> obj.deploy(model_column="model", model_file_prefix="my_prefix_",
                   partition_columns=["partition_column_1", "partition_column_2"])
    ['my_prefix__0_10',
     'my_prefix__0_11',
     'my_prefix__1_10',
     'my_prefix__1_11']
    # Other examples are similar to the examples provided for VantageCloud Enterprise.
execute_script
teradataml.table_operators.Script.execute_script = execute_script(self, output_style='VIEW')
DESCRIPTION:
    Function enables user to run script on Vantage.
PARAMETERS:
    output_style:
        Specifies the type of output object to create - a table or a view.
        Permitted Values: 'VIEW', 'TABLE'.
        Default value: 'VIEW'
        Types: str
RETURNS:
    Output teradataml DataFrames can be accessed using attribute
    references, such as ScriptObj.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, ValueError
EXAMPLES:
    Refer to help(Script)
install_ﬁle
teradataml.table_operators.Script.install_ﬁle = install_ﬁle(self, ﬁle_identiﬁer, ﬁle_name, is_binary=False, replace=False, force_replace=False)
DESCRIPTION:
    Function to install script on Vantage.
    On success, prints a message that file is installed.
    This language script can be executed via execute_script() function.
PARAMETERS:
    file_identifier:
        Required Argument.
        Specifies the name associated with the user-installed file.
        It cannot have a schema name associated with it,
        as the file is always installed in the current schema.
        The name should be unique within the schema. It can be any valid Teradata
        identifier.
        Types: str
    file_name:
        Required Argument:
        Specifies the name of the file user wnats to install.
        Types: str
    is_binary:
        Optional Argument.
        Specifies if file to be installed is a binary file.
        Default Value: False
        Types: bool
    replace:
        Optional Argument.
        Specifies if the file is to be installed or replaced.
        If set to True, then the file is replaced based on value the of
        force_replace.
        If set to False, then the file is installed.
        Default Value: False
        Types: bool
    force_replace:
        Optional Argument.
        Specifies if system should check for the file being used before
        replacing it.
        If set to True, then the file is replaced even if it is being executed.
        If set to False, then an error is thrown if it is being executed.
        Default Value: False
        Types: bool
RETURNS:
     True, if success
RAISES:
    TeradataMLException.
EXAMPLES:
    # Note - Refer to User Guide for setting search path and required permissions.
    # Example 1: Install the file mapper.py found at the relative path
    # data/scripts/ using the default text mode.
    # Set SEARCHUIFDBPATH.
    >>> execute_sql("SET SESSION SEARCHUIFDBPATH = alice;")
    # Create a Script object that allows us to execute script on Vantage.
    >>> import os
    >>> from teradatasqlalchemy import VARCHAR
    >>> td_path = os.path.dirname(teradataml.__file__)
    >>> sto = Script(data=barrierdf,
    ...              script_name='mapper.py',
    ...              files_local_path= os.path.join(td_path, 'data', "scripts"),
    ...              script_command='tdpython3 ./alice/mapper.py',
    ...              data_order_column="Id",
    ...              is_local_order=False,
    ...              nulls_first=False,
    ...              sort_ascending=False,
    ...              charset='latin',
    ...              returns=OrderedDict([("word", VARCHAR(15)),("count_input", VARCHAR(2))]))
    >>>
    # Install file on Vantage.
    >>> sto.install_file(file_identifier='mapper',
    ...                  file_name='mapper.py',
    ...                  is_binary=False)
    File mapper.py installed in Vantage
    # Replace file on Vantage.
    >>> sto.install_file(file_identifier='mapper',
    ...                  file_name='mapper.py',
    ...                  is_binary=False,
    ...                  replace=True,
    ...                  force_replace=True)
    File mapper.py replaced in Vantage
remove_ﬁle
teradataml.table_operators.Script.remove_ﬁle = remove_ﬁle(self, ﬁle_identiﬁer, force_remove=False)
DESCRIPTION:
    Function to remove user installed files/scripts from Vantage.
PARAMETERS:
    file_identifier:
        Required Argument.
        Specifies the name associated with the user-installed file.
        It cannot have a database name associated with it,
        as the file is always installed in the current database.
        Types: str
    force_remove:
        Required Argument.
        Specifies if system should check for the file being used before
        removing it.
        If set to True, then the file is removed even if it is being executed.
        If set to False, then an error is thrown if it is being executed.
        Default value: False
        Types: bool
RETURNS:
     True, if success.
RAISES:
    TeradataMLException.
EXAMPLES:
    # Note - Refer to User Guide for setting search path and required permissions.
    # Run install_file example before removing file.
    # Set SEARCHUIFDBPATH.
    >>> execute_sql("SET SESSION SEARCHUIFDBPATH = alice;")
    # Create a Script object that allows us to execute script on Vantage.
    >>> sto = Script(data=barrierdf,
    ...              script_name='mapper.py',
    ...              files_local_path= os.path.join(td_path, 'data', "scripts"),
    ...              script_command='tdpython3 ./alice/mapper.py',
    ...              data_order_column="Id",
    ...              is_local_order=False,
    ...              nulls_first=False,
    ...              sort_ascending=False,
    ...              charset='latin',
    ...              returns=OrderedDict([("word", VARCHAR(15)),("count_input", VARCHAR(2))]))
    >>>
    # Install file on Vantage.
    >>> sto.install_file(file_identifier='mapper',
    ...                  file_name='mapper.py',
    ...                  is_binary=False,
    ...                  replace=True,
    ...                  force_replace=True)
    File mapper.py replaced in Vantage
    # Remove the installed file.
    >>> sto.remove_file(file_identifier='mapper', force_remove=True)
    File mapper removed from Vantage
set_data
teradataml.table_operators.Script.set_data = set_data(self, data, data_partition_column=None, data_hash_column=None, data_order_column=None, is_local_order=False,
sort_ascending=True, nulls_ﬁrst=True)
DESCRIPTION:
    Function enables user to set data and data related arguments without having to
    re-create Script object.
PARAMETERS:
    data:
        Required Argument.
        Specifies a teradataml DataFrame containing the input data for the script.
    data_hash_column:
        Optional Argument.
        Specifies the column to be used for hashing.
        The rows in the data are redistributed to AMPs based on the
        hash value of the column specified.
        The user installed script then runs once on each AMP.
        If there is no data_partition_column, then the entire result set delivered
        by the function, constitutes a single group or partition.
        Types: str
        Note:
            "data_hash_column" can not be specified along with
            "data_partition_column", "is_local_order" and "data_order_column".
    data_partition_column:
        Optional Argument.
        Specifies Partition By columns for data.
        Values to this argument can be provided as a list, if multiple
        columns are used for partition.
        Default Value: ANY
        Types: str OR list of Strings (str)
        Note:
            1) "data_partition_column" can not be specified along with
               "data_hash_column".
            2) "data_partition_column" can not be specified along with
               "is_local_order = True".
    is_local_order:
        Optional Argument.
        Specifies a boolean value to determine whether the input data is to be
        ordered locally or not. Order by specifies the order in which the
        values in a group or partition are sorted.  Local Order By specifies
        orders qualified rows on each AMP in preparation to be input to a table
        function. This argument is ignored, if "data_order_column" is None. When
        set to True, data is ordered locally.
        Default Value: False
        Types: bool
        Note:
            1) "is_local_order" can not be specified along with
               "data_hash_column".
            2) When "is_local_order" is set to True, "data_order_column" should be
               specified, and the columns specified in "data_order_column" are
               used for local ordering.
    data_order_column:
        Optional Argument.
        Specifies Order By columns for data.
        Values to this argument can be provided as a list, if multiple
        columns are used for ordering.
        This argument is used in both cases:
        "is_local_order = True" and "is_local_order = False".
        Types: str OR list of Strings (str)
        Note:
            "data_order_column" can not be specified along with
            "data_hash_column".
    sort_ascending:
        Optional Argument.
        Specifies a boolean value to determine if the result set is to be sorted
        on the column specified in "data_order_column", in ascending or descending
        order.
        The sorting is ascending when this argument is set to True, and descending
        when set to False.
        This argument is ignored, if "data_order_column" is None.
        Default Value: True
        Types: bool
    nulls_first:
        Optional Argument.
        Specifies a boolean value to determine whether NULLS are listed first or
        last during ordering.
        This argument is ignored, if "data_order_column" is None.
        NULLS are listed first when this argument is set to True, and
        last when set to False.
        Default Value: True
        Types: bool
RETURNS:
    None.
RAISES:
    TeradataMlException
EXAMPLES:
    # Note - Refer to User Guide for setting search path and required permissions.
    # Load example data.
    load_example_data("Script", ["barrier"])
    # Example 1
    # Create teradataml DataFrame objects.
    >>> barrierdf = DataFrame.from_table("barrier")
    >>> barrierdf
                            Name
    Id
    1   Old Macdonald Had A Farm
    >>>
    # Set SEARCHUIFDBPATH
    >>> execute_sql("SET SESSION SEARCHUIFDBPATH = alice;")
    >>> import teradataml
    >>> from teradatasqlalchemy import VARCHAR
    >>> td_path = os.path.dirname(teradataml.__file__)
    # The script mapper.py reads in a line of text input
    # ("Old Macdonald Had A Farm") from csv and
    # splits the line into individual words, emitting a new row for each word.
    # Create a Script object without data and its arguments.
    >>> sto = Script(data = barrierdf,
    ...              script_name='mapper.py',
    ...              files_local_path= os.path.join(td_path,'data', 'scripts'),
    ...              script_command='tdpython3 ./alice/mapper.py',
    ...              charset='latin',
    ...              returns=OrderedDict([("word", VARCHAR(15)),("count_input", VARCHAR(2))]))
    # Test script using data from file
    >>> sto.test_script(input_data_file='../barrier.csv', data_file_delimiter=',')
    ############ STDOUT Output ############
            word  count_input
    0          1            1
    1        Old            1
    2  Macdonald            1
    3        Had            1
    4          A            1
    5       Farm            1
    >>>
    # Test script using data from DB.
    >>> sto.test_script(password='alice')
    ############ STDOUT Output ############
            word  count_input
    0          1            1
    1        Old            1
    2  Macdonald            1
    3        Had            1
    4          A            1
    5       Farm            1
    # Test script using data from DB and with data_row_limit.
    >>> sto.test_script(password='alice', data_row_limit=5)
    ############ STDOUT Output ############
            word  count_input
    0          1            1
    1        Old            1
    2  Macdonald            1
    3        Had            1
    4          A            1
    5       Farm            1
    # Now in order to test / run script on actual data on Vantage user must
    # set data and related arguments.
    # Note:
    #    All data related arguments that are not specified in set_data() are
    #    reset to default values.
    >>> sto.set_data(data=barrierdf,
    ...              data_order_column="Id",
    ...              is_local_order=False,
    ...              nulls_first=False,
    ...              sort_ascending=False)
    # Execute the user script on Vantage.
    >>> sto.execute_script()
    ############ STDOUT Output ############
            word count_input
    0  Macdonald           1
    1          A           1
    2       Farm           1
    3        Had           1
    4        Old           1
    5          1           1
    # Example 2 -
    # Input data is barrier_new and script is executed on Vantage.
    # use set_data() to reset arguments.
    # Create teradataml DataFrame objects.
    >>> load_example_data("Script", ["barrier_new"])
    >>> barrierdf_new = DataFrame.from_table("barrier_new")
    >>> barrierdf_new
                            Name
    Id
    2   On his farm he had a cow
    1   Old Macdonald Had A Farm
    >>>
    # Create a Script object that allows us to execute script on Vantage.
    >>> sto = Script(data=barrierdf_new,
    ...              script_name='mapper.py',
    ...              files_local_path= os.path.join(td_path, 'data', 'scripts'),
    ...              script_command='tdpython3 ./alice/mapper.py',
    ...              data_order_column="Id",
    ...              is_local_order=False,
    ...              nulls_first=False,
    ...              sort_ascending=False,
    ...              charset='latin',
    ...              returns=OrderedDict([("word", VARCHAR(15)),("count_input", VARCHAR(2))]))
    # Script is executed on Vantage.
    >>> sto.execute_script()
    ############ STDOUT Output ############
       word count_input
    0   his           1
    1    he           1
    2   had           1
    3     a           1
    4     1           1
    5   Old           1
    6   cow           1
    7  farm           1
    8    On           1
    9     2           1
    # Now in order to run the script with a different dataset,
    # user can use set_data().
    # Re-set data and some data related parameters.
    # Note:
    #     All data related arguments that are not specified in set_data() are
    #     reset to default values.
    >>> sto.set_data(data=barrierdf,
    ...              data_order_column='Id',
    ...              is_local_order=True,
    ...              nulls_first=True)
    >>> sto.execute_script()
            word count_input
    0  Macdonald           1
    1          A           1
    2       Farm           1
    3        Had           1
    4        Old           1
    5          1           1
    # Example 3
    # In order to run the script with same dataset but different data related
    # arguments, use set_data() to reset arguments.
    # Note:
    #     All data related arguments that are not specified in set_data() are
    #     reset to default values.
    >>> sto.set_data(data=barrierdf_new,
    ...              data_order_column='Id',
    ...              is_local_order = True,
    ...              nulls_first = True)
    >>> sto.execute_script()
    ############ STDOUT Output ############
            word count_input
    0  Macdonald           1
    1          A           1
    2       Farm           1
    3          2           1
    4        his           1
    5       farm           1
    6         On           1
    7        Had           1
    8        Old           1
    9          1           1
test_script
teradataml.table_operators.Script.test_script = test_script(self, supporting_ﬁles=None, input_data_ﬁle=None, script_args='', exec_mode='local', **kwargs)
DESCRIPTION:
    Function enables user to run script locally outside Vantage.
    Input data for user script is either read from a file or from database.
    Note:
        1. Purpose of test_script() function is to enable the user to test their scripts for any errors without
           installing it on Vantage, using the input data provided.
        2. Data is not partitioned for testing the script if read from input data file.
        3. Function can produce different output if input is read from a file than input from database.
PARAMETERS:
    supporting_files:
        Optional Argument.
        Specifies a file or list of supporting files like model files to be
        copied to the container.
        Types: string or list of str
    input_data_file:
        Optional Argument.
        Specifies the name of the input data file.
        It should have a path relative to the location specified in
        "files_local_path" argument.
        If set to None, read data from AMP, else from file passed in the argument
        'input_data_file'.
        File should have at least permissions of mode 644.
        Types: str
    script_args:
        Optional Argument.
        Specifies command line arguments required by the user script.
        Types: str
    exec_mode:
        Optional Argument.
        Specifies the mode in which user wants to test the script.
        When set to 'local', the user script will run locally on user's system.
        Permitted Values: 'local'
        Default Value: 'local'
        Types: str
    kwargs:
        Optional Argument.
        Specifies the keyword arguments required for testing.
        Keys can be:
            data_row_limit:
                Optional Argument. Ignored when data is read from file.
                Specifies the number of rows to be taken from all amps when
                reading from a table or view on Vantage.
                Default Value: 1000
                Types: int
            password:
                Optional Argument. Required when reading from database.
                Specifies the password to connect to vantage where the data
                resides.
                Types: str
            data_file_delimiter:
                Optional Argument.
                Specifies the delimiter used in the input data file. This
                argument can be specified when data is read from file.
                Default Value: '        '
                Types: str
            data_file_header:
                Optional Argument.
                Specifies whether the input data file contains header. This
                argument can be specified when data is read from file.
                Default Value: True
                Types: bool
            data_file_quote_char:
                Optional Argument.
                Specifies the quotechar used in the input data file.
                This argument can be specified when data is read from file.
                Default Value: '"'
            logmech:
                Optional Argument.
                Specifies the type of logon mechanism to establish a connection to
                Teradata Vantage.
                Permitted Values: 'TD2', 'TDNEGO', 'LDAP', 'KRB5' & 'JWT'.
                    TD2:
                        The Teradata 2 (TD2) mechanism provides authentication
                        using a Vantage username and password. This is the default
                        logon mechanism using which the connection is established
                        to Vantage.
                    TDNEGO:
                        A security mechanism that automatically determines the
                        actual mechanism required, based on policy, without user's
                        involvement. The actual mechanism is determined by the
                        TDGSS server configuration and by the security policy's
                        mechanism restrictions.
                    LDAP:
                        A directory-based user logon to Vantage with a directory
                        username and password and is authenticated by the directory.
                    KRB5 (Kerberos):
                        A directory-based user logon to Vantage with a domain
                        username and password and is authenticated by
                        Kerberos (KRB5 mechanism).
                        Note:
                            User must have a valid ticket-granting ticket in
                            order to use this logon mechanism.
                    JWT:
                        The JSON Web Token (JWT) authentication mechanism enables
                        single sign-on (SSO) to the Vantage after the user
                        successfully authenticates to Teradata UDA User Service.
                        Note:
                            User must use logdata parameter when using 'JWT' as
                            the logon mechanism.
                Default Value: TD2
                Types: str
                Note:
                    teradataml expects the client environments are already setup with appropriate
                    security mechanisms and are in working conditions.
                    For more information please refer Teradata Vantage™ - Advanced SQL Engine
                    Security Administration at https://www.info.teradata.com/
            logdata:
                Optional Argument.
                Specifies parameters to the LOGMECH command beyond those needed by
                the logon mechanism, such as user ID, password and tokens
                (in case of JWT) to successfully authenticate the user.
                Types: str
        Types: dict
RETURNS:
    Output from user script.
RAISES:
    TeradataMlException
EXAMPLES:
    # Assumption - sto is Script() object. Please refer to help(Script)
    # for creating Script object.
    # Run user script in local mode with input from data file.
    >>> sto.test_script(input_data_file='../barrier.csv',
    ...                 data_file_delimiter=',',
    ...                 data_file_quote_char='"',
    ...                 data_file_header=True,
    ...                 exec_mode='local')
    ############ STDOUT Output ############
            word  count_input
    0          1            1
    1        Old            1
    2  Macdonald            1
    3        Had            1
    4          A            1
    5       Farm            1
    >>>
    # Run user script in local mode with input from table.
    >>> sto.test_script(data_row_limit=300, password='alice', exec_mode='local')
    ############ STDOUT Output ############
            word  count_input
    0          1            1
    1        Old            1
    2  Macdonald            1
    3        Had            1
    4          A            1
    5       Farm            1
Table Operators: NOS
Supported on Database Version 17.00.xx
ReadNOS
ReadNOS
Functions
ReadNOS(data=None, location=None, access_id=None, access_key=None, buffer_size=16, return_type='NOSREAD_RECORD', sample_perc=1.0,
stored_as='TEXTFILE', full_scan=False, manifest=False, row_format=None, header=True, **generic_arguments)
DESCRIPTION:
    ReadNOS() function enables access to external files in JSON, CSV, or Parquet format.
    You must have the EXECUTE FUNCTION privilege on TD_SYSFNLIB.READ_NOS.
PARAMETERS:
    data:
        Optional Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml DataFrame
    location:
        Optional Argument.
        Specifies the location value, which is a Uniform Resource Identifier
        (URI) pointing to the data in the external object storage system. The
        location value includes the following components:
        Amazon S3:
            /connector/bucket.endpoint/[key_prefix].
        Azure Blob storage and Azure Data Lake Storage Gen2:
            /connector/container.endpoint/[key_prefix].
        Google Cloud Storage:
            /connector/endpoint/bucket/[key_prefix].
        connector:
            Identifies the type of external storage system where the data is located.
            Teradata requires the storage location to start with the following for
            all external storage locations:
                * Amazon S3 storage location must begin with /S3 or /s3
                * Azure Blob storage location (including Azure Data Lake Storage Gen2
                  in Blob Interop Mode) must begin with /AZ or /az.
                * Google Cloud Storage location must begin with /GS or /gs.
        storage-account:
            Used by Azure. The Azure storage account contains your
            Azure storage data objects.
        endpoint:
            A URL that identifies the system-specific entry point for
            the external object storage system.
        bucket (Amazon S3, Google Cloud Storage) or container (Azure Blob
        storage and Azure Data Lake Storage Gen2):
            A container that logically groups stored objects in the
            external storage system.
        key_prefix:
            Identifies one or more objects in the logical organization of
            the bucket data. Because it is a key prefix, not an actual
            directory path, the key prefix may match one or more objects
            in the external storage. For example, the key prefix
            "/fabrics/cotton/colors/b/" would match objects
            /fabrics/cotton/colors/blue, /fabrics/cotton/colors/brown, and
            /fabrics/cotton/colors/black. If there are organization levels below
            those, such as /fabrics/cotton/colors/blue/shirts, the same key
            prefix would gather those objects too.
            Note:
                Vantage validates only the first file it encounters from the
                location key prefix.
        For example, this location value might specify all objects on an
        Amazon cloud storage system for the month of December, 2001:
        location = "/S3/YOUR-BUCKET.s3.amazonaws.com/csv/US-Crimes/csv-files/2001/Dec/"
            connector: S3
            bucket: YOUR-BUCKET
            endpoint: s3.amazonaws.com
            key_prefix: csv/US-Crimes/csv-files/2001/Dec/
        Following location could specify an individual storage object (or file), Day1.csv:
        location = "/S3/YOUR-BUCKET.s3.amazonaws.com/csv/US-Crimes/csv-files/2001/Dec/Day1.csv"
            connector: S3
            bucket: YOUR-BUCKET
            endpoint: s3.amazonaws.com
            key_prefix: csv/US-Crimes/csv-files/2001/Dec/Day1.csv
        Following location specifies an entire container in an Azure external
        object store (Azure Blob storage or Azure Data Lake Storage Gen2).
        The container may contain multiple file objects:
        location = "/AZ/YOUR-STORAGE-ACCOUNT.blob.core.windows.net/nos-csv-data"
            connector: AZ
            bucket: YOUR-STORAGE-ACCOUNT
            endpoint: blob.core.windows.net
            key_prefix: nos-csv-data
        This is an example of a Google Cloud Storage location:
        location = "/gs/storage.googleapis.com/YOUR-BUCKET/CSVDATA/RIVERS/rivers.csv"
            connector: GS
            bucket: YOUR-BUCKET
            endpoint: storage.googleapis.com,
            key_prefix: CSVDATA/RIVERS/rivers.csv
        Types: str
    access_id:
        Optional Argument.
        Specifies the identification for accessing external storage.
        This option is not necessary if an authentication object is defined,
        for example, in the EXTERNAL SECURITY clause of a function mapping
        or if IAM roles are defined for S3 repositories.
        See "CREATE FUNCTION MAPPING" in Teradata Vantage - SQL Data
        Definition Language Syntax and Examples.
        To set the function mapping for ReadNOS() function, set the following
        configuration options with function mapping name.
            teradataml.options.configure.read_nos_function_mapping = "<function mapping name>"
        Types: str
    access_key:
        Optional Argument.
        Specifies the password for accessing external storage.
        This option is not necessary if an authentication object is defined,
        for example, in the EXTERNAL SECURITY clause of a function mapping
        or if IAM roles are defined for S3 repositories.
        See "CREATE FUNCTION MAPPING" in Teradata Vantage - SQL Data
        Definition Language Syntax and Examples.
        To set the function mapping for ReadNOS() function, set the following
        configuration options with function mapping name.
            teradataml.options.configure.read_nos_function_mapping = "<function mapping name>"
        Types: str
    buffer_size:
        Optional Argument.
        Specifies the size of the network buffer to allocate when retrieving
        data from the external storage repository. 16 MB, which is the maximum
        value.
        Default Value: 16
        Types: int
    return_type:
        Optional Argument.
        Specifies the format in which data is returned.
        Permitted Values:
            * NOSREAD_RECORD:
                Returns one row for each external record along with its metadata.
                Access external records by specifying one of the following:
                    - Input teradataml DataFrame, location, and teradataml DataFrame
                      on an empty table. For CSV, you can include a schema definition.
                    - Input teradataml DataFrame with a row for each external file. For
                      CSV, this method does not support a schema definition.
                For an empty single-column input table, do the following:
                    - Define an input teradataml DataFrame with a single column, Payload,
                      with the appropriate data type:
                        JSON and CSV
                      This column determines the output Payload column return type.
                    - Specify the filepath in the "location" argument.
                For a multiple-column input table, define an input teradataml
                DataFrame with the following columns:
                    +------------------+-------------------------------------+
                    | Column Name      | Data Types                          |
                    +------------------+-------------------------------------+
                    | Location         | VARCHAR(2048) CHARACTER SET UNICODE |
                    +------------------+-------------------------------------+
                    | ObjectVersionID  | VARCHAR(1024) CHARACTER SET UNICODE |
                    +------------------+-------------------------------------+
                    | OffsetIntoObject | BIGINT                              |
                    +------------------+-------------------------------------+
                    | ObjectLength     | BIGINT                              |
                    +------------------+-------------------------------------+
                    | Payload          | JSON                                |
                    +------------------+-------------------------------------+
                    | CSV              | VARCHAR                             |
                    +------------------+-------------------------------------+
                This teradataml DataFrame can be populated using the output of the
                'NOSREAD_KEYS' return type.
            * NOSREAD_KEYS:
                Retrieve the list of files from the path specified in the "location" argument.
                A schema definition is not necessary.
                'NOSREAD_KEYS' returns Location, ObjectVersionID, ObjectTimeStamp,
                and ObjectLength (size of external file).
            * NOSREAD_PARQUET_SCHEMA:
                Returns information about the Parquet data schema. If you are using
                the "full_scan" option, use 'NOSREAD_PARQUET_SCHEMA'; otherwise you can use
                'NOSREAD_SCHEMA' to get information about the Parquet schema. For information
                about the mapping between Parquet data types and Teradata data types, see
                Parquet External Files in Teradata Vantage - SQL Data Definition Language
                Syntax and Examples.
            * NOSREAD_RAW:
                 Retrieves file data from the external storage services, not specific records.
                 Retrieved data is returned as CLOB/BLOB. You can retrieve a complete file from
                 external storage and save in Teradata CLOB/BLOB format. The maximum amount of
                 data that can be retrieved from the external storage and saved in the DataFrame
                 column is 2GB, the Vantage limit for LOBs. The ObjectLength corresponds to the
                 length of CLOB/BLOB column read from the external storage.
                 This information is provided in the form of a DataFrame returned to the ReadNOS()
                 function. The Payload column in the input data is only used to determine the
                 datatype of the column in which the returned data is stored.
                 Define the input data with the following columns:
                    - Location VARCHAR(2048) CHARACTER SET UNICODE
                    - ObjectVersionID VARCHAR(1024) CHARACTER SET UNICODE
                    - ObjectTimeStamp TIMESTAMP(6)
                    - OffsetIntoObject BIGINT
                    - BytesToRead BIGINT
                    - Payload CLOB/BLOB
                 ReadNOS() function returns a DataFrame with the following columns:
                    - Location VARCHAR(2048) CHARACTER SET UNICODE
                    - ObjectVersionID VARCHAR(1024) CHARACTER SET UNICODE
                    - ObjectTimeStamp TIMESTAMP(6)
                    - OffsetIntoObject BIGINT
                    - ObjectLength BIGINT
                    - Payload CLOB/BLOB, based on input table CLOB/BLOB Column.
        Default Value: "NOSREAD_RECORD"
        Types: str
    sample_perc:
        Optional Argument.
        Specifies the percentage of rows to retrieve from the external
        storage repository when "return_type" is 'NOSREAD_RECORD'. The valid
        range of values is from 0.0 to 1.0, where 1.0 represents 100%
        of the rows.
        Default Value: 1.0
        Types: float
    stored_as:
        Optional Argument.
        Specifies the formatting style of the external data.
        Permitted Values:
            * PARQUET-
                The external data is formatted as Parquet. This is a
                required parameter for Parquet data.
            * TEXTFILE-
                The external data uses a text-based format, such as
                CSV or JSON.
        Default Value: "TEXTFILE"
        Types: str
    full_scan:
        Optional Argument.
        Specifies whether ReadNOS() function scans columns of variable length
        types (CHAR, VARCHAR, BYTE, VARBYTE, JSON, and BSON) to discover the
        maximum length.
        When set to True, the size of variable length data is determined
        from the Parquet data.
        Note:
            Choosing this value can impact performance because all variable length
            data type columns in each Parquet file at the location must be scanned
            to assess the value having the greatest length.
        When set to False, variable length field sizes are assigned the
        Vantage maximum value for the particular data type.
        Note:
            "full_scan" is only used with a "return_type" of 'NOSREAD_PARQUET_SCHEMA'.
        Default Value: False
        Types: bool
    manifest:
        Optional Argument.
        Specifies whether the location value points to a manifest file (a
        file containing a list of files to read) or object name. The object
        name can include the full path or a partial path. It must identify a
        single file containing the manifest.
        Note:
            Individual entries within the manifest file must show
            complete paths.
        Below is an example of a manifest file that contains a list of locations 
        in JSON format.
        {
          "entries": [
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-8_9_02-10.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-8_9_02-101.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-10-01/data-8_9_02-102.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-10-01/data-8_9_02-103.json"}
           ]
        }
        Default Value: False
        Types: bool
    row_format:
        Optional Argument.
        Specifies the encoding format of the external row.
        For example:
            row_format = {'field_delimiter': ',', 'record_delimiter': '\n', 'character_set': 'LATIN'}
        If string value is used, JSON format must be used to specify the row format.
        For example:
            row_format = '{"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}'
        Format can include only the three keys shown above. Key names and values 
        are case-specific, except for the value for "character_set", which can 
        use any combination of letter cases.
        The character set specified in "row_format" must be compatible with the character 
        set of the Payload column.
        Do not specify "row_format" for Parquet format data.
        For a JSON column, these are the default values:
            UNICODE:
                row_format = {"record_delimiter":"\n", "character_set":"UTF8"}
            LATIN:
                row_format = {"record_delimiter":"\n", "character_set":"LATIN"}
        For a CSV column, these are the default values:
            UNICODE:
                row_format = '{"character_set":"UTF8"}'
            LATIN:
                row_format = '{"character_set":"LATIN"}'
        User can specify the following options:
            field_delimiter: The default is "," (comma). User can also specify a
                             custom field delimiter, such as tab "\t".
            record_delimiter: New line feed character: "\n". A line feed
                              is the only acceptable record delimiter.
            character_set: "UTF8" or "LATIN". If you do not specify a "row_format"
                           or payload column, Vantage assumes UTF8 Unicode.
        Types: str or dict
    header:
        Optional Argument.
        Specifies whether the first row of data in an input CSV file is
        interpreted as column headings for the subsequent rows of data. Use
        this parameter only when a CSV input file is not associated with a
        separate schema object that defines columns for the CSV data.
        Default Value: True
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept.
        Below are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the function in table or not.
                When set to True, results are persisted in table; otherwise, results
                are garbage collected at the end of the session.
                Default Value: False
                Types: boolean
            volatile:
                Optional Argument.
                Specifies whether to put the results of the function in volatile table or not.
                When set to True, results are stored in volatile table, otherwise not.
                Default Value: False
                Types: boolean
        Function allows the user to partition, hash, order or local order the input
        data. These generic arguments are available for each argument that accepts
        teradataml DataFrame as input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if the underlying Analytic Database
            function supports, else an exception is raised.
RETURNS:
    Instance of ReadNOS.
    Output teradataml DataFrames can be accessed using attribute
    references, such as ReadNOS.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function will raise error if not supported on the Vantage
    #        user is connected to.
    # Check the list of available analytic functions.
    display_analytic_functions()
    # Example 1: Reading PARQUET file from AWS S3 location.
    obj =  ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/location/",
                   access_id="YOUR-ID",
                   access_key="YOUR-KEY",
                   stored_as="PARQUET")
    # print the result DataFame.
    print(obj.result)
    # Example 2: Read PARQUET file in external storage with one row for each external 
    #            record along with its metadata.
    obj = ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/location/",
                  access_id="YOUR-ID",
                  access_key="YOUR-KEY",
                  return_type="NOSREAD_KEYS")
    # print the result DataFame.
    print(obj.result)
    # Example 3: Read CSV file from external storage.
    obj = ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/csv-location/",
                  access_id="YOUR-ID",
                  access_key="YOUR-KEY",
                  stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
    # Example 4: Read CSV file in external storage using "data" argument.
    # Create a table to store the data.
    con = create_context(host=host, user=user, password=password)
    execute_sql("CREATE TABLE read_nos_support_tab (payload dataset storage format csv) NO PRIMARY INDEX;")
    read_nos_support_tab = DataFrame("read_nos_support_tab")
    # Read the CSV data using "data" argument.
    obj = ReadNOS(data=read_nos_support_tab,
                  location="/S3/s3.amazonaws.com/Your-Bucket/csv-location/",
                  access_id="YOUR-ID",
                  access_key="YOUR-KEY",
                  row_format={"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}
                  stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
    # Note: 
    #   Before proceeding, verify with your database administrator that you have the 
    #   correct privileges, an authorization object, and a function mapping (for READ_NOS 
    #   function).
    # If function mapping for READ_NOS Analytic database function is created 
    # as 'READ_NOS_FM' and location and authorization object are mapped,
    # then set function mapping with teradataml options as below.
    # Example 5: Setting function mapping using configuration options.
    from teradataml.options.configure import configure
    configure.read_nos_function_mapping = "READ_NOS_FM"
    obj =  ReadNOS(data=read_nos_support_tab,
                   row_format={"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}
                   stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
Supported on Database Version 17.05.xx
ReadNOS
ReadNOS
Functions
ReadNOS(data=None, location=None, access_id=None, access_key=None, buffer_size=16, return_type='NOSREAD_RECORD', sample_perc=1.0,
stored_as='TEXTFILE', full_scan=False, manifest=False, row_format=None, header=True, **generic_arguments)
DESCRIPTION:
    ReadNOS() function enables access to external files in JSON, CSV, or Parquet format.
    You must have the EXECUTE FUNCTION privilege on TD_SYSFNLIB.READ_NOS.
PARAMETERS:
    data:
        Optional Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml DataFrame
    location:
        Optional Argument.
        Specifies the location value, which is a Uniform Resource Identifier
        (URI) pointing to the data in the external object storage system. The
        location value includes the following components:
        Amazon S3:
            /connector/bucket.endpoint/[key_prefix].
        Azure Blob storage and Azure Data Lake Storage Gen2:
            /connector/container.endpoint/[key_prefix].
        Google Cloud Storage:
            /connector/endpoint/bucket/[key_prefix].
        connector:
            Identifies the type of external storage system where the data is located.
            Teradata requires the storage location to start with the following for
            all external storage locations:
                * Amazon S3 storage location must begin with /S3 or /s3
                * Azure Blob storage location (including Azure Data Lake Storage Gen2
                  in Blob Interop Mode) must begin with /AZ or /az.
                * Google Cloud Storage location must begin with /GS or /gs.
        storage-account:
            Used by Azure. The Azure storage account contains your
            Azure storage data objects.
        endpoint:
            A URL that identifies the system-specific entry point for
            the external object storage system.
        bucket (Amazon S3, Google Cloud Storage) or container (Azure Blob
        storage and Azure Data Lake Storage Gen2):
            A container that logically groups stored objects in the
            external storage system.
        key_prefix:
            Identifies one or more objects in the logical organization of
            the bucket data. Because it is a key prefix, not an actual
            directory path, the key prefix may match one or more objects
            in the external storage. For example, the key prefix
            "/fabrics/cotton/colors/b/" would match objects
            /fabrics/cotton/colors/blue, /fabrics/cotton/colors/brown, and
            /fabrics/cotton/colors/black. If there are organization levels below
            those, such as /fabrics/cotton/colors/blue/shirts, the same key
            prefix would gather those objects too.
            Note:
                Vantage validates only the first file it encounters from the
                location key prefix.
        For example, this location value might specify all objects on an
        Amazon cloud storage system for the month of December, 2001:
        location = "/S3/YOUR-BUCKET.s3.amazonaws.com/csv/US-Crimes/csv-files/2001/Dec/"
            connector: S3
            bucket: YOUR-BUCKET
            endpoint: s3.amazonaws.com
            key_prefix: csv/US-Crimes/csv-files/2001/Dec/
        Following location could specify an individual storage object (or file), Day1.csv:
        location = "/S3/YOUR-BUCKET.s3.amazonaws.com/csv/US-Crimes/csv-files/2001/Dec/Day1.csv"
            connector: S3
            bucket: YOUR-BUCKET
            endpoint: s3.amazonaws.com
            key_prefix: csv/US-Crimes/csv-files/2001/Dec/Day1.csv
        Following location specifies an entire container in an Azure external
        object store (Azure Blob storage or Azure Data Lake Storage Gen2).
        The container may contain multiple file objects:
        location = "/AZ/YOUR-STORAGE-ACCOUNT.blob.core.windows.net/nos-csv-data"
            connector: AZ
            bucket: YOUR-STORAGE-ACCOUNT
            endpoint: blob.core.windows.net
            key_prefix: nos-csv-data
        This is an example of a Google Cloud Storage location:
        location = "/gs/storage.googleapis.com/YOUR-BUCKET/CSVDATA/RIVERS/rivers.csv"
            connector: GS
            bucket: YOUR-BUCKET
            endpoint: storage.googleapis.com,
            key_prefix: CSVDATA/RIVERS/rivers.csv
        Types: str
    access_id:
        Optional Argument.
        Specifies the identification for accessing external storage.
        This option is not necessary if an authentication object is defined,
        for example, in the EXTERNAL SECURITY clause of a function mapping
        or if IAM roles are defined for S3 repositories.
        See "CREATE FUNCTION MAPPING" in Teradata Vantage™ - SQL Data
        Definition Language Syntax and Examples, B035-1144.
        To set the function mapping for ReadNOS() function, set the following
        configuration options with function mapping name.
            teradataml.options.configure.read_nos_function_mapping = "<function mapping name>"
        Types: str
    access_key:
        Optional Argument.
        Specifies the password for accessing external storage.
        This option is not necessary if an authentication object is defined,
        for example, in the EXTERNAL SECURITY clause of a function mapping
        or if IAM roles are defined for S3 repositories.
        See "CREATE FUNCTION MAPPING" in Teradata Vantage - SQL Data
        Definition Language Syntax and Examples.
        To set the function mapping for ReadNOS() function, set the following
        configuration options with function mapping name.
            teradataml.options.configure.read_nos_function_mapping = "<function mapping name>"
        Types: str
    buffer_size:
        Optional Argument.
        Specifies the size of the network buffer to allocate when retrieving
        data from the external storage repository. 16 MB, which is the maximum
        value.
        Default Value: 16
        Types: int
    return_type:
        Optional Argument.
        Specifies the format in which data is returned.
        Permitted Values:
            * NOSREAD_RECORD:
                Returns one row for each external record along with its metadata.
                Access external records by specifying one of the following:
                    - Input teradataml DataFrame, location, and teradataml DataFrame
                      on an empty table. For CSV, you can include a schema definition.
                    - Input teradataml DataFrame with a row for each external file. For
                      CSV, this method does not support a schema definition.
                For an empty single-column input table, do the following:
                    - Define an input teradataml DataFrame with a single column, Payload,
                      with the appropriate data type:
                        JSON and CSV
                      This column determines the output Payload column return type.
                    - Specify the filepath in the "location" argument.
                For a multiple-column input table, define an input teradataml
                DataFrame with the following columns:
                    +------------------+-------------------------------------+
                    | Column Name      | Data Types                          |
                    +------------------+-------------------------------------+
                    | Location         | VARCHAR(2048) CHARACTER SET UNICODE |
                    +------------------+-------------------------------------+
                    | ObjectVersionID  | VARCHAR(1024) CHARACTER SET UNICODE |
                    +------------------+-------------------------------------+
                    | OffsetIntoObject | BIGINT                              |
                    +------------------+-------------------------------------+
                    | ObjectLength     | BIGINT                              |
                    +------------------+-------------------------------------+
                    | Payload          | JSON                                |
                    +------------------+-------------------------------------+
                    | CSV              | VARCHAR                             |
                    +------------------+-------------------------------------+
                This teradataml DataFrame can be populated using the output of the
                'NOSREAD_KEYS' return type.
            * NOSREAD_KEYS:
                Retrieve the list of files from the path specified in the "location" argument.
                A schema definition is not necessary.
                'NOSREAD_KEYS' returns Location, ObjectVersionID, ObjectTimeStamp,
                and ObjectLength (size of external file).
            * NOSREAD_PARQUET_SCHEMA:
                Returns information about the Parquet data schema. If you are using
                the "full_scan" option, use 'NOSREAD_PARQUET_SCHEMA'; otherwise you can use
                'NOSREAD_SCHEMA' to get information about the Parquet schema. For information
                about the mapping between Parquet data types and Teradata data types, see
                Parquet External Files in Teradata Vantage - SQL Data Definition Language
                Syntax and Examples.
            * NOSREAD_RAW:
                 Retrieves file data from the external storage services, not specific records.
                 Retrieved data is returned as CLOB/BLOB. You can retrieve a complete file from
                 external storage and save in Teradata CLOB/BLOB format. The maximum amount of
                 data that can be retrieved from the external storage and saved in the DataFrame
                 column is 2GB, the Vantage limit for LOBs. The ObjectLength corresponds to the
                 length of CLOB/BLOB column read from the external storage.
                 This information is provided in the form of a DataFrame returned to the ReadNOS()
                 function. The Payload column in the input data is only used to determine the
                 datatype of the column in which the returned data is stored.
                 Define the input data with the following columns:
                    - Location VARCHAR(2048) CHARACTER SET UNICODE
                    - ObjectVersionID VARCHAR(1024) CHARACTER SET UNICODE
                    - ObjectTimeStamp TIMESTAMP(6)
                    - OffsetIntoObject BIGINT
                    - BytesToRead BIGINT
                    - Payload CLOB/BLOB
                 ReadNOS() function returns a DataFrame with the following columns:
                    - Location VARCHAR(2048) CHARACTER SET UNICODE
                    - ObjectVersionID VARCHAR(1024) CHARACTER SET UNICODE
                    - ObjectTimeStamp TIMESTAMP(6)
                    - OffsetIntoObject BIGINT
                    - ObjectLength BIGINT
                    - Payload CLOB/BLOB, based on input table CLOB/BLOB Column.
        Default Value: "NOSREAD_RECORD"
        Types: str
    sample_perc:
        Optional Argument.
        Specifies the percentage of rows to retrieve from the external
        storage repository when "return_type" is 'NOSREAD_RECORD'. The valid
        range of values is from 0.0 to 1.0, where 1.0 represents 100%
        of the rows.
        Default Value: 1.0
        Types: float
    stored_as:
        Optional Argument.
        Specifies the formatting style of the external data.
        Permitted Values:
            * PARQUET-
                The external data is formatted as Parquet. This is a
                required parameter for Parquet data.
            * TEXTFILE-
                The external data uses a text-based format, such as
                CSV or JSON.
        Default Value: "TEXTFILE"
        Types: str
    full_scan:
        Optional Argument.
        Specifies whether ReadNOS() function scans columns of variable length
        types (CHAR, VARCHAR, BYTE, VARBYTE, JSON, and BSON) to discover the
        maximum length.
        When set to True, the size of variable length data is determined
        from the Parquet data.
        Note:
            Choosing this value can impact performance because all variable length
            data type columns in each Parquet file at the location must be scanned
            to assess the value having the greatest length.
        When set to False, variable length field sizes are assigned the
        Vantage maximum value for the particular data type.
        Note:
            "full_scan" is only used with a "return_type" of 'NOSREAD_PARQUET_SCHEMA'.
        Default Value: False
        Types: bool
    manifest:
        Optional Argument.
        Specifies whether the location value points to a manifest file (a
        file containing a list of files to read) or object name. The object
        name can include the full path or a partial path. It must identify a
        single file containing the manifest.
        Note:
            Individual entries within the manifest file must show
            complete paths.
        Below is an example of a manifest file that contains a list of locations 
        in JSON format.
        {
          "entries": [
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-8_9_02-10.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-8_9_02-101.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-10-01/data-8_9_02-102.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-10-01/data-8_9_02-103.json"}
           ]
        }
        Default Value: False
        Types: bool
    row_format:
        Optional Argument.
        Specifies the encoding format of the external row.
        For example:
            row_format = {'field_delimiter': ',', 'record_delimiter': '\n', 'character_set': 'LATIN'}
        If string value is used, JSON format must be used to specify the row format.
        For example:
            row_format = '{"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}'
        Format can include only the three keys shown above. Key names and values 
        are case-specific, except for the value for "character_set", which can 
        use any combination of letter cases.
        The character set specified in "row_format" must be compatible with the character 
        set of the Payload column.
        Do not specify "row_format" for Parquet format data.
        For a JSON column, these are the default values:
            UNICODE:
                row_format = {"record_delimiter":"\n", "character_set":"UTF8"}
            LATIN:
                row_format = {"record_delimiter":"\n", "character_set":"LATIN"}
        For a CSV column, these are the default values:
            UNICODE:
                row_format = '{"character_set":"UTF8"}'
            LATIN:
                row_format = '{"character_set":"LATIN"}'
        User can specify the following options:
            field_delimiter: The default is "," (comma). User can also specify a
                             custom field delimiter, such as tab "\t".
            record_delimiter: New line feed character: "\n". A line feed
                              is the only acceptable record delimiter.
            character_set: "UTF8" or "LATIN". If you do not specify a "row_format"
                           or payload column, Vantage assumes UTF8 Unicode.
        Types: str or dict
    header:
        Optional Argument.
        Specifies whether the first row of data in an input CSV file is
        interpreted as column headings for the subsequent rows of data. Use
        this parameter only when a CSV input file is not associated with a
        separate schema object that defines columns for the CSV data.
        Default Value: True
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept.
        Below are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the function in table or not.
                When set to True, results are persisted in table; otherwise, results
                are garbage collected at the end of the session.
                Default Value: False
                Types: boolean
            volatile:
                Optional Argument.
                Specifies whether to put the results of the function in volatile table or not.
                When set to True, results are stored in volatile table, otherwise not.
                Default Value: False
                Types: boolean
        Function allows the user to partition, hash, order or local order the input
        data. These generic arguments are available for each argument that accepts
        teradataml DataFrame as input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if the underlying Analytic Database
            function supports, else an exception is raised.
RETURNS:
    Instance of ReadNOS.
    Output teradataml DataFrames can be accessed using attribute
    references, such as ReadNOS.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function will raise error if not supported on the Vantage
    #        user is connected to.
    # Check the list of available analytic functions.
    display_analytic_functions()
    # Example 1: Reading PARQUET file from AWS S3 location.
    obj =  ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/location/",
                   access_id="YOUR-ID",
                   access_key="YOUR-KEY",
                   stored_as="PARQUET")
    # print the result DataFame.
    print(obj.result)
    # Example 2: Read PARQUET file in external storage with one row for each external 
    #            record along with its metadata.
    obj = ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/location/",
                  access_id="YOUR-ID",
                  access_key="YOUR-KEY",
                  return_type="NOSREAD_KEYS")
    # print the result DataFame.
    print(obj.result)
    # Example 3: Read CSV file from external storage.
    obj = ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/csv-location/",
                  access_id="YOUR-ID",
                  access_key="YOUR-KEY",
                  stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
    # Example 4: Read CSV file in external storage using "data" argument.
    # Create a table to store the data.
    con = create_context(host=host, user=user, password=password)
    execute_sql("CREATE TABLE read_nos_support_tab (payload dataset storage format csv) NO PRIMARY INDEX;")
    read_nos_support_tab = DataFrame("read_nos_support_tab")
    # Read the CSV data using "data" argument.
    obj = ReadNOS(data=read_nos_support_tab,
                  location="/S3/s3.amazonaws.com/Your-Bucket/csv-location/",
                  access_id="YOUR-ID",
                  access_key="YOUR-KEY",
                  row_format={"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}
                  stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
    # Note: 
    #   Before proceeding, verify with your database administrator that you have the 
    #   correct privileges, an authorization object, and a function mapping (for READ_NOS 
    #   function).
    # If function mapping for READ_NOS Analytic database function is created 
    # as 'READ_NOS_FM' and location and authorization object are mapped,
    # then set function mapping with teradataml options as below.
    # Example 5: Setting function mapping using configuration options.
    from teradataml.options.configure import configure
    configure.read_nos_function_mapping = "READ_NOS_FM"
    obj =  ReadNOS(data=read_nos_support_tab,
                   row_format={"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}
                   stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
WriteNOS
WriteNOS
Functions
WriteNOS(data=None, location=None, authorization=None, stored_as='PARQUET', naming='RANGE', manifest_ﬁle=None, manifest_only=False, overwrite=False,
include_ordering=None, include_hashby=None, max_object_size='16MB', compression=None, **generic_arguments)
DESCRIPTION:
    WriteNOS() function enables access to write input teradataml DataFrame to external storage,
    like Amazon S3, Azure Blob storage, or Google Cloud Storage.
    You must have the EXECUTE FUNCTION privilege on TD_SYSFNLIB.WRITE_NOS.
PARAMETERS:
    data:
        Required Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml DataFrame
    location:
        Optional Argument.
        Specifies the location value, which is a Uniform Resource Identifier
        (URI) pointing to location in the external object storage system.
        A URI identifying the external storage system in the format:
            /connector/endpoint/bucket_or_container/prefix
        The "location" string cannot exceed 2048 characters.
        Types: str
    authorization:
        Optional Argument.
        Specifies the authorization for accessing the external storage.
        Following are the options for using "authorization":
        * Passing access id and key as dictionary:
            authorization = {'Access_ID': 'access_id', 'Access_Key': 'secret_key'}
        * Passing access id and key as string in JSON format:
            authorization = '{"Access_ID":"access_id", "Access_Key":"secret_key"}'
        * Using Authorization Object:
            On any platform, authorization object can be specified as
            authorization = "[DatabaseName.]AuthorizationObjectName" 
            EXECUTE privilege on AuthorizationObjectName is needed.
        Below table specifies the USER (identification) and PASSWORD (secret_key) 
        for accessing external storage. The following table shows the supported 
        credentials for USER and PASSWORD (used in the CREATE AUTHORIZATION SQL command):
        +-----------------------+--------------------------+--------------------------+
        |     System/Scheme     |           USER           |         PASSWORD         |
        +-----------------------+--------------------------+--------------------------+
        |          AWS          |      Access Key ID       |    Access Key Secret     |
        |                       |                          |                          |
        |   Azure / Shared Key  |   Storage Account Name   |   Storage Account Key    |
        |                       |                          |                          |
        |  Azure Shared Access  |   Storage Account Name   |    Account SAS Token     |
        |    Signature (SAS)    |                          |                          |
        |                       |                          |                          |
        |      Google Cloud     |      Access Key ID       |    Access Key Secret     |
        |   (S3 interop mode)   |                          |                          |
        |                       |                          |                          |
        | Google Cloud (native) |       Client Email       |       Private Key        |
        |                       |                          |                          |
        |  Public access object |      <empty string>      |      <empty string>      |
        |        stores         | Enclose the empty string | Enclose the empty string |
        |                       |    in single straight    |    in single straight    |
        |                       |      quotes: USER ''     |    quotes: PASSWORD ''   |
        +-----------------------+--------------------------+--------------------------+
        When accessing GCS, Analytics Database uses either the S3-compatible connector or 
        the native Google connector, depending on the user credentials.
        Notes:
            * If using AWS IAM credentials, "authorization" can be omitted.
            * If S3 user account requires the use of physical or virtual security, 
              session token can be used with Access_ID and Access_Key in this syntax:
                  authorization = {"Access_ID":"access_id", 
                                   "Access_Key":"secret_key",
                                   "Session_Token":"session_token"}
              In which, the session token can be obtained using the AWS CLI. 
              For example: 
                  aws sts get-session-token
        Types: str or dict
    stored_as:
        Optional Argument.
        Specifies the formatting style of the external data.
        PARQUET means the external data is formatted as Parquet. Objects
        created in external storage by WriteNOS are written only in Parquet
        format.
        Permitted Values:
            * PARQUET
        Default Value: "PARQUET"
        Types: str
    naming:
        Optional Argument.
        Specifies how the objects containing the rows of data are named
        in the external storage:
        Permitted Values:
            * DISCRETE-
                Discrete naming uses the ordering column values as part of the object
                names in external storage. For example, if the "data_partition_column"
                has "data_order_column = ["dateColumn", "intColumn"]", the discrete form name
                of the objects written to external storage would include the values for
                those columns as part of the object name, which would look similar to this:
                    S3/ceph-s3.teradata.com/xz186000/2019-03-01/13/object_33_0_1.parquet
                2019-03-01 is the value for dateColumn, the first ordering column, and 13 is
                the value for the second ordering column, intColumn.
                All rows stored in this external Parquet-formatted object contain those
                two values.
            * RANGE-
                Range naming, the default, includes as part of the object name the
                range of values included in the partition for each ordering column.
                For example, using the same "data_order_column" as above the object
                names would look similar to this:
                    S3/ceph-s3.teradata.com/xz186000/2019-01-01/2019-03-02/9/10000/object_33_0_1.parquet
                where 2019-01-01 is the minimum value in that object for the first
                ordering column, dateColumn, 2019-03-02 is the maximum value for the
                rows stored in this external Parquet-formatted object. Value 9 is the
                minimum value for the second ordering column, intColumn, and 10000 is
                the maximum value for that column.
        Default Value: "RANGE"
        Types: str
    manifest_file:
        Optional Argument.
        Specifies the fully qualified path and file name where the manifest
        file is written. Use the format
            "/connector/end point/bucket_or_container/prefix/manifest_file_name"
        For example:
            "/S3/ceph-s3.teradata.com/xz186000/manifest/manifest.json"
        If "manifest_file" argument is not included, no manifest file is
        written.
        Types: str
    manifest_only:
        Optional Argument.
        Specifies wheather to write only a manifest file in external storage
        or not. No actual data objects are written to external storage when
        "manifest_only" is set to True. One must also use the "manifest_file" 
        option to create a manifest file in external storage. Use this option 
        to create a new manifest file in the event that a WriteNOS()
        fails due to a database abort or restart, or when network connectivity 
        issues interrupt and stop a WriteNOS() before all data has 
        been written to external storage. The manifest is created from the 
        teradataml DataFrame that is input to WriteNOS(). 
        The input must be a DataFrame of storage object names and sizes, with 
        one row per object.
        Note: 
            The input to WriteNOS() with "manifest_only" can itself incorporate
            ReadNOS(), similar to this, which uses function mappings for WriteNOS()
            and ReadNOS():
                read_nos_obj = ReadNOS(return_type="NOSREAD_KEYS")
                WriteNOS(data=read_nos_obj.result.filter(items =['Location', 'ObjectLength']),
                         manifest_file=manifest_file,
                         manifest_only=True)
        Function calls like above can be used if a WriteNOS() fails before
        it can create a manifest file. The new manifest file created using
        ReadNOS() reflects all data objects currently in the external
        storage location, and can aid in determining which data objects
        resulted from the incomplete WriteNOS(). 
        For more information, see Teradata Vantage - Native Object Store 
        Getting Started Guide.
        Default Value: False
        Types: bool
    overwrite:
        Optional Argument.
        Specifies whether an existing manifest file in external storage is
        overwritten with a new manifest file that has the same name.
        When set to False, WriteNOS() returns an error if a manifest file exists in
        external storage that is named identically to the value of
        "manifest_file".
        Note: 
            Overwrite must be used with "manifest_only" set to True.
        Default Value: False
        Types: bool
    include_ordering:
        Optional Argument.
        Specifies whether column(s) specified in argument "data_order_column" and
        their values are written to external storage.
        Types: bool
    include_hashby:
        Optional Argument.
        Specifies whether column(s) specified in argument "data_hash_column" and
        their values are written to external storage.
        Types: bool
    max_object_size:
        Optional Argument.
        Specifies the maximum output object size in megabytes, where
        maximum object size can range between 4 and 16. The default is the
        value of the DefaultRowGroupSize field in DBS Control. 
        For more information on DBS Control, see Teradata Vantage - Database
        Utilities.
        Default Value: "16MB"
        Types: str
    compression:
        Optional Argument.
        Specifies the compression algorithm used to compress the objects
        written to external storage.
        Note:
            For Parquet files the compression occurs inside parts of the parquet
            file instead of for the entire file, so the file extension on
            external objects remains '.parquet'.
        Permitted Values: GZIP, SNAPPY
        Types: str
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept.
        Below are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the function in table or not.
                When set to True, results are persisted in table; otherwise, results
                are garbage collected at the end of the session.
                Default Value: False
                Types: boolean
            volatile:
                Optional Argument.
                Specifies whether to put the results of the function in volatile table or not.
                When set to True, results are stored in volatile table, otherwise not.
                Default Value: False
                Types: boolean
        Function allows the user to partition, hash, order or local order the input
        data. These generic arguments are available for each argument that accepts
        teradataml DataFrame as input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if the underlying
            Analytic Database function supports it, else an exception is raised.
RETURNS:
    Instance of WriteNOS.
    Output teradataml DataFrames can be accessed using attribute
    references, such as WriteNOSObj.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function will raise error if not supported on the Vantage
    #        user is connected to.
    # Check the list of available analytic functions.
    display_analytic_functions()
    # Load the example data.
    load_example_data("teradataml", "titanic")
    # Create teradataml DataFrame object.
    titanic_data = DataFrame.from_table("titanic")
    # Example 1: Writing the DataFrame to an AWS S3 location.
    obj =  WriteNOS(data=titanic_data,
                    location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                    authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                    stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 2: Write DataFrame to external storage with partition and order by
    #            column 'sex'.
    obj = WriteNOS(data=titanic_data,
                   location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"}, 
                   data_partition_columns="sex",
                   data_order_columns="sex",
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 3: Write DataFrame to external storage with hashing and order by
    #            column 'sex'.
    obj = WriteNOS(data=titanic_data,
                   location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                   data_hash_columns="sex",
                   data_order_columns="sex",
                   local_order_data=True,
                   include_hashing=True,
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 4: Write DataFrame to external storage with max object size as 4MB.
    obj = WriteNOS(data=titanic_data,
                   location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"}, 
                   include_ordering=True,
                   max_object_size='4MB',
                   compression='GZIP',
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 5: Write DataFrame of manifest table into a manifest file.
    import pandas as pd
    obj_names = ['/S3/s3.amazonaws.com/YOUR-STORAGE-ACCOUNT/20180701/ManifestFile/object_33_0_1.parquet',
                 '/S3/s3.amazonaws.com/YOUR-STORAGE-ACCOUNT/20180701/ManifestFile/object_33_6_1.parquet',
                 '/S3/s3.amazonaws.com/YOUR-STORAGE-ACCOUNT/20180701/ManifestFile/object_33_1_1.parquet']
    obj_size = [2803, 2733, 3009]
    manifest_df = pd.DataFrame({'ObjectName':obj_names, 'ObjectSize': obj_size})
    from teradataml import copy_to_sql
    copy_to_sql(manifest_df, "manifest_df")
    manifest_df = DataFrame("manifest_df")
    obj = WriteNOS(data=manifest_df,
                   location='YOUR-STORAGE-ACCOUNT/20180701/ManifestFile2/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                   manifest_file='YOUR-STORAGE-ACCOUNT/20180701/ManifestFile2/manifest2.json',
                   manifest_only=True,
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Note: 
    #   Before proceeding, verify with your database administrator that you have the 
    #   correct privileges, an authorization object, and a function mapping (for WRITE_NOS 
    #   function).
    # If function mapping for WRITE_NOS Analytic database function is created 
    # as 'WRITE_NOS_FM' and location and authorization object are mapped,
    # then set function mapping with teradataml options as below.
    # Example 7: Writing the DataFrame using function mapping.
    from teradataml.options.configure import configure
    configure.write_nos_function_mapping = "WRITE_NOS_FM"
    obj =  WriteNOS(data=titanic_data, stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
Supported on Database Version 17.10.xx
ReadNOS
ReadNOS
Functions
ReadNOS(data=None, location=None, authorization=None, buffer_size=16, return_type='NOSREAD_RECORD', sample_perc=1.0, stored_as='TEXTFILE',
full_scan=False, manifest=False, row_format=None, header=True, **generic_arguments)
DESCRIPTION:
    ReadNOS() function enables access to external files in JSON, CSV, or Parquet format.
    You must have the EXECUTE FUNCTION privilege on TD_SYSFNLIB.READ_NOS.
PARAMETERS:
    data:
        Optional Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml DataFrame
    location:
        Optional Argument.
        Specifies the location value, which is a Uniform Resource Identifier
        (URI) pointing to the data in the external object storage system. The
        location value includes the following components:
        Amazon S3:
            /connector/bucket.endpoint/[key_prefix].
        Azure Blob storage and Azure Data Lake Storage Gen2:
            /connector/container.endpoint/[key_prefix].
        Google Cloud Storage:
            /connector/endpoint/bucket/[key_prefix].
        connector:
            Identifies the type of external storage system where the data is located.
            Teradata requires the storage location to start with the following for
            all external storage locations:
                * Amazon S3 storage location must begin with /S3 or /s3
                * Azure Blob storage location (including Azure Data Lake Storage Gen2
                  in Blob Interop Mode) must begin with /AZ or /az.
                * Google Cloud Storage location must begin with /GS or /gs.
        storage-account:
            Used by Azure. The Azure storage account contains your
            Azure storage data objects.
        endpoint:
            A URL that identifies the system-specific entry point for
            the external object storage system.
        bucket (Amazon S3, Google Cloud Storage) or container (Azure Blob
        storage and Azure Data Lake Storage Gen2):
            A container that logically groups stored objects in the
            external storage system.
        key_prefix:
            Identifies one or more objects in the logical organization of
            the bucket data. Because it is a key prefix, not an actual
            directory path, the key prefix may match one or more objects
            in the external storage. For example, the key prefix
            "/fabrics/cotton/colors/b/" would match objects
            /fabrics/cotton/colors/blue, /fabrics/cotton/colors/brown, and
            /fabrics/cotton/colors/black. If there are organization levels below
            those, such as /fabrics/cotton/colors/blue/shirts, the same key
            prefix would gather those objects too.
            Note:
                Vantage validates only the first file it encounters from the
                location key prefix.
        For example, this location value might specify all objects on an
        Amazon cloud storage system for the month of December, 2001:
        location = "/S3/YOUR-BUCKET.s3.amazonaws.com/csv/US-Crimes/csv-files/2001/Dec/"
            connector: S3
            bucket: YOUR-BUCKET
            endpoint: s3.amazonaws.com
            key_prefix: csv/US-Crimes/csv-files/2001/Dec/
        Following location could specify an individual storage object (or file), Day1.csv:
        location = "/S3/YOUR-BUCKET.s3.amazonaws.com/csv/US-Crimes/csv-files/2001/Dec/Day1.csv"
            connector: S3
            bucket: YOUR-BUCKET
            endpoint: s3.amazonaws.com
            key_prefix: csv/US-Crimes/csv-files/2001/Dec/Day1.csv
        Following location specifies an entire container in an Azure external
        object store (Azure Blob storage or Azure Data Lake Storage Gen2).
        The container may contain multiple file objects:
        location = "/AZ/YOUR-STORAGE-ACCOUNT.blob.core.windows.net/nos-csv-data"
            connector: AZ
            bucket: YOUR-STORAGE-ACCOUNT
            endpoint: blob.core.windows.net
            key_prefix: nos-csv-data
        This is an example of a Google Cloud Storage location:
        location = "/gs/storage.googleapis.com/YOUR-BUCKET/CSVDATA/RIVERS/rivers.csv"
            connector: GS
            bucket: YOUR-BUCKET
            endpoint: storage.googleapis.com,
            key_prefix: CSVDATA/RIVERS/rivers.csv
        Types: str
    authorization:
        Optional Argument.
        Specifies the authorization for accessing the external storage.
        Following are the options for using "authorization":
        * Passing access id and key as dictionary:
            authorization = {'Access_ID': 'access_id', 'Access_Key': 'secret_key'}
        * Passing access id and key as string in JSON format:
            authorization = '{"Access_ID":"access_id", "Access_Key":"secret_key"}'
        * Using Authorization Object:
            On any platform, authorization object can be specified as
            authorization = "[DatabaseName.]AuthorizationObjectName" 
            EXECUTE privilege on AuthorizationObjectName is needed.
        Below table specifies the USER (identification) and PASSWORD (secret_key) 
        for accessing external storage. The following table shows the supported 
        credentials for USER and PASSWORD (used in the CREATE AUTHORIZATION SQL command):
        +-----------------------+--------------------------+--------------------------+
        |     System/Scheme     |           USER           |         PASSWORD         |
        +-----------------------+--------------------------+--------------------------+
        |          AWS          |      Access Key ID       |    Access Key Secret     |
        |                       |                          |                          |
        |   Azure / Shared Key  |   Storage Account Name   |   Storage Account Key    |
        |                       |                          |                          |
        |  Azure Shared Access  |   Storage Account Name   |    Account SAS Token     |
        |    Signature (SAS)    |                          |                          |
        |                       |                          |                          |
        |      Google Cloud     |      Access Key ID       |    Access Key Secret     |
        |   (S3 interop mode)   |                          |                          |
        |                       |                          |                          |
        | Google Cloud (native) |       Client Email       |       Private Key        |
        |                       |                          |                          |
        |  Public access object |      <empty string>      |      <empty string>      |
        |        stores         | Enclose the empty string | Enclose the empty string |
        |                       |    in single straight    |    in single straight    |
        |                       |      quotes: USER ''     |    quotes: PASSWORD ''   |
        +-----------------------+--------------------------+--------------------------+
        When accessing GCS, Analytics Database uses either the S3-compatible connector or 
        the native Google connector, depending on the user credentials.
        Notes:
            * If using AWS IAM credentials, "authorization" can be omitted.
            * If S3 user account requires the use of physical or virtual security, 
              session token can be used with Access_ID and Access_Key in this syntax:
                  authorization = {"Access_ID":"access_id", 
                                   "Access_Key":"secret_key",
                                   "Session_Token":"session_token"}
              In which, the session token can be obtained using the AWS CLI. 
              For example: 
                  aws sts get-session-token
        Types: str or dict
    buffer_size:
        Optional Argument.
        Specifies the size of the network buffer to allocate when retrieving
        data from the external storage repository. 16 MB, which is the maximum
        value.
        Default Value: 16
        Types: int
    return_type:
        Optional Argument.
        Specifies the format in which data is returned.
        Permitted Values:
            * NOSREAD_RECORD:
                Returns one row for each external record along with its metadata.
                Access external records by specifying one of the following:
                    - Input teradataml DataFrame, location, and teradataml DataFrame
                      on an empty table. For CSV, you can include a schema definition.
                    - Input teradataml DataFrame with a row for each external file. For
                      CSV, this method does not support a schema definition.
                For an empty single-column input table, do the following:
                    - Define an input teradataml DataFrame with a single column, Payload,
                      with the appropriate data type:
                        JSON and CSV
                      This column determines the output Payload column return type.
                    - Specify the filepath in the "location" argument.
                For a multiple-column input table, define an input teradataml
                DataFrame with the following columns:
                    +------------------+-------------------------------------+
                    | Column Name      | Data Types                          |
                    +------------------+-------------------------------------+
                    | Location         | VARCHAR(2048) CHARACTER SET UNICODE |
                    +------------------+-------------------------------------+
                    | ObjectVersionID  | VARCHAR(1024) CHARACTER SET UNICODE |
                    +------------------+-------------------------------------+
                    | OffsetIntoObject | BIGINT                              |
                    +------------------+-------------------------------------+
                    | ObjectLength     | BIGINT                              |
                    +------------------+-------------------------------------+
                    | Payload          | JSON                                |
                    +------------------+-------------------------------------+
                    | CSV              | VARCHAR                             |
                    +------------------+-------------------------------------+
                This teradataml DataFrame can be populated using the output of the
                'NOSREAD_KEYS' return type.
            * NOSREAD_KEYS:
                Retrieve the list of files from the path specified in the "location" argument.
                A schema definition is not necessary.
                'NOSREAD_KEYS' returns Location, ObjectVersionID, ObjectTimeStamp,
                and ObjectLength (size of external file).
            * NOSREAD_PARQUET_SCHEMA:
                Returns information about the Parquet data schema. If you are using
                the "full_scan" option, use 'NOSREAD_PARQUET_SCHEMA'; otherwise you can use
                'NOSREAD_SCHEMA' to get information about the Parquet schema. For information
                about the mapping between Parquet data types and Teradata data types, see
                Parquet External Files in Teradata Vantage - SQL Data Definition Language
                Syntax and Examples.
            * NOSREAD_SCHEMA:
                 Returns the name and data type of each column of the file specified in
                 the "location" argument. Schema format can be JSON, CSV, or Parquet.
        Default Value: "NOSREAD_RECORD"
        Types: str
    sample_perc:
        Optional Argument.
        Specifies the percentage of rows to retrieve from the external
        storage repository when "return_type" is 'NOSREAD_RECORD'. The valid
        range of values is from 0.0 to 1.0, where 1.0 represents 100%
        of the rows.
        Default Value: 1.0
        Types: float
    stored_as:
        Optional Argument.
        Specifies the formatting style of the external data.
        Permitted Values:
            * PARQUET-
                The external data is formatted as Parquet. This is a
                required parameter for Parquet data.
            * TEXTFILE-
                The external data uses a text-based format, such as
                CSV or JSON.
        Default Value: "TEXTFILE"
        Types: str
    full_scan:
        Optional Argument.
        Specifies whether ReadNOS() function scans columns of variable length
        types (CHAR, VARCHAR, BYTE, VARBYTE, JSON, and BSON) to discover the
        maximum length.
        When set to True, the size of variable length data is determined
        from the Parquet data.
        Note:
            Choosing this value can impact performance because all variable length
            data type columns in each Parquet file at the location must be scanned
            to assess the value having the greatest length.
        When set to False, variable length field sizes are assigned the
        Vantage maximum value for the particular data type.
        Note:
            "full_scan" is only used with a "return_type" of 'NOSREAD_PARQUET_SCHEMA'.
        Default Value: False
        Types: bool
    manifest:
        Optional Argument.
        Specifies whether the location value points to a manifest file (a
        file containing a list of files to read) or object name. The object
        name can include the full path or a partial path. It must identify a
        single file containing the manifest.
        Note:
            Individual entries within the manifest file must show
            complete paths.
        Below is an example of a manifest file that contains a list of locations 
        in JSON format.
        {
          "entries": [
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-8_9_02-10.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-8_9_02-101.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-10-01/data-8_9_02-102.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-10-01/data-8_9_02-103.json"}
           ]
        }
        Default Value: False
        Types: bool
    row_format:
        Optional Argument.
        Specifies the encoding format of the external row.
        For example:
            row_format = {'field_delimiter': ',', 'record_delimiter': '\n', 'character_set': 'LATIN'}
        If string value is used, JSON format must be used to specify the row format.
        For example:
            row_format = '{"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}'
        Format can include only the three keys shown above. Key names and values 
        are case-specific, except for the value for "character_set", which can 
        use any combination of letter cases.
        The character set specified in "row_format" must be compatible with the character 
        set of the Payload column.
        Do not specify "row_format" for Parquet format data.
        For a JSON column, these are the default values:
            UNICODE:
                row_format = {"record_delimiter":"\n", "character_set":"UTF8"}
            LATIN:
                row_format = {"record_delimiter":"\n", "character_set":"LATIN"}
        For a CSV column, these are the default values:
            UNICODE:
                row_format = '{"character_set":"UTF8"}'
            LATIN:
                row_format = '{"character_set":"LATIN"}'
        User can specify the following options:
            field_delimiter: The default is "," (comma). User can also specify a
                             custom field delimiter, such as tab "\t".
            record_delimiter: New line feed character: "\n". A line feed
                              is the only acceptable record delimiter.
            character_set: "UTF8" or "LATIN". If you do not specify a "row_format"
                           or payload column, Vantage assumes UTF8 Unicode.
        Types: str or dict
    header:
        Optional Argument.
        Specifies whether the first row of data in an input CSV file is
        interpreted as column headings for the subsequent rows of data. Use
        this parameter only when a CSV input file is not associated with a
        separate schema object that defines columns for the CSV data.
        Default Value: True
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept.
        Below are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the function in table or not.
                When set to True, results are persisted in table; otherwise, results
                are garbage collected at the end of the session.
                Default Value: False
                Types: boolean
            volatile:
                Optional Argument.
                Specifies whether to put the results of the function in volatile table or not.
                When set to True, results are stored in volatile table, otherwise not.
                Default Value: False
                Types: boolean
        Function allows the user to partition, hash, order or local order the input
        data. These generic arguments are available for each argument that accepts
        teradataml DataFrame as input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if the underlying Analytic Database
            function supports, else an exception is raised.
RETURNS:
    Instance of ReadNOS.
    Output teradataml DataFrames can be accessed using attribute
    references, such as ReadNOS.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function will raise error if not supported on the Vantage
    #        user is connected to.
    # Check the list of available analytic functions.
    display_analytic_functions()
    # Example 1: Reading PARQUET file from AWS S3 location.
    obj =  ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/location/",
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                   stored_as="PARQUET")
    # print the result DataFame.
    print(obj.result)
    # Example 2: Read PARQUET file in external storage with one row for each external 
    #            record along with its metadata.
    obj = ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/location/",
                  authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                  return_type="NOSREAD_KEYS")
    # print the result DataFame.
    print(obj.result)
    # Example 3: Read CSV file from external storage.
    obj = ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/csv-location/",
                  authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                  stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
    # Example 4: Read CSV file in external storage using "data" argument.
    # Create a table to store the data.
    con = create_context(host=host, user=user, password=password)
    execute_sql("CREATE TABLE read_nos_support_tab (payload dataset storage format csv) NO PRIMARY INDEX;")
    read_nos_support_tab = DataFrame("read_nos_support_tab")
    # Read the CSV data using "data" argument.
    obj = ReadNOS(data=read_nos_support_tab,
                  location="/S3/s3.amazonaws.com/Your-Bucket/csv-location/",
                  authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                  row_format={"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}
                  stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
    # Note: 
    #   Before proceeding, verify with your database administrator that you have the 
    #   correct privileges, an authorization object, and a function mapping (for READ_NOS 
    #   function).
    # If function mapping for READ_NOS Analytic database function is created 
    # as 'READ_NOS_FM' and location and authorization object are mapped,
    # then set function mapping with teradataml options as below.
    # Example 5: Setting function mapping using configuration options.
    from teradataml.options.configure import configure
    configure.read_nos_function_mapping = "READ_NOS_FM"
    obj =  ReadNOS(data=read_nos_support_tab,
                   row_format={"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}
                   stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
WriteNOS
WriteNOS
Functions
WriteNOS(data=None, location=None, authorization=None, stored_as='PARQUET', naming='RANGE', manifest_ﬁle=None, manifest_only=False, overwrite=False,
include_ordering=None, include_hashby=None, max_object_size='16MB', compression=None, **generic_arguments)
DESCRIPTION:
    WriteNOS() function enables access to write input teradataml DataFrame to external storage,
    like Amazon S3, Azure Blob storage, or Google Cloud Storage.
    You must have the EXECUTE FUNCTION privilege on TD_SYSFNLIB.WRITE_NOS.
PARAMETERS:
    data:
        Required Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml DataFrame
    location:
        Optional Argument.
        Specifies the location value, which is a Uniform Resource Identifier
        (URI) pointing to location in the external object storage system.
        A URI identifying the external storage system in the format:
            /connector/endpoint/bucket_or_container/prefix
        The "location" string cannot exceed 2048 characters.
        Types: str
    authorization:
        Optional Argument.
        Specifies the authorization for accessing the external storage.
        Following are the options for using "authorization":
        * Passing access id and key as dictionary:
            authorization = {'Access_ID': 'access_id', 'Access_Key': 'secret_key'}
        * Passing access id and key as string in JSON format:
            authorization = '{"Access_ID":"access_id", "Access_Key":"secret_key"}'
        * Using Authorization Object:
            On any platform, authorization object can be specified as
            authorization = "[DatabaseName.]AuthorizationObjectName" 
            EXECUTE privilege on AuthorizationObjectName is needed.
        Below table specifies the USER (identification) and PASSWORD (secret_key) 
        for accessing external storage. The following table shows the supported 
        credentials for USER and PASSWORD (used in the CREATE AUTHORIZATION SQL command):
        +-----------------------+--------------------------+--------------------------+
        |     System/Scheme     |           USER           |         PASSWORD         |
        +-----------------------+--------------------------+--------------------------+
        |          AWS          |      Access Key ID       |    Access Key Secret     |
        |                       |                          |                          |
        |   Azure / Shared Key  |   Storage Account Name   |   Storage Account Key    |
        |                       |                          |                          |
        |  Azure Shared Access  |   Storage Account Name   |    Account SAS Token     |
        |    Signature (SAS)    |                          |                          |
        |                       |                          |                          |
        |      Google Cloud     |      Access Key ID       |    Access Key Secret     |
        |   (S3 interop mode)   |                          |                          |
        |                       |                          |                          |
        | Google Cloud (native) |       Client Email       |       Private Key        |
        |                       |                          |                          |
        |  Public access object |      <empty string>      |      <empty string>      |
        |        stores         | Enclose the empty string | Enclose the empty string |
        |                       |    in single straight    |    in single straight    |
        |                       |      quotes: USER ''     |    quotes: PASSWORD ''   |
        +-----------------------+--------------------------+--------------------------+
        When accessing GCS, Analytics Database uses either the S3-compatible connector or 
        the native Google connector, depending on the user credentials.
        Notes:
            * If using AWS IAM credentials, "authorization" can be omitted.
            * If S3 user account requires the use of physical or virtual security, 
              session token can be used with Access_ID and Access_Key in this syntax:
                  authorization = {"Access_ID":"access_id", 
                                   "Access_Key":"secret_key",
                                   "Session_Token":"session_token"}
              In which, the session token can be obtained using the AWS CLI. 
              For example: 
                  aws sts get-session-token
        Types: str or dict
    stored_as:
        Optional Argument.
        Specifies the formatting style of the external data.
        PARQUET means the external data is formatted as Parquet. Objects
        created in external storage by WriteNOS are written only in Parquet
        format.
        Permitted Values:
            * PARQUET
        Default Value: "PARQUET"
        Types: str
    naming:
        Optional Argument.
        Specifies how the objects containing the rows of data are named
        in the external storage:
        Permitted Values:
            * DISCRETE-
                Discrete naming uses the ordering column values as part of the object
                names in external storage. For example, if the "data_partition_column"
                has "data_order_column = ["dateColumn", "intColumn"]", the discrete form name
                of the objects written to external storage would include the values for
                those columns as part of the object name, which would look similar to this:
                    S3/ceph-s3.teradata.com/xz186000/2019-03-01/13/object_33_0_1.parquet
                2019-03-01 is the value for dateColumn, the first ordering column, and 13 is
                the value for the second ordering column, intColumn.
                All rows stored in this external Parquet-formatted object contain those
                two values.
            * RANGE-
                Range naming, the default, includes as part of the object name the
                range of values included in the partition for each ordering column.
                For example, using the same "data_order_column" as above the object
                names would look similar to this:
                    S3/ceph-s3.teradata.com/xz186000/2019-01-01/2019-03-02/9/10000/object_33_0_1.parquet
                where 2019-01-01 is the minimum value in that object for the first
                ordering column, dateColumn, 2019-03-02 is the maximum value for the
                rows stored in this external Parquet-formatted object. Value 9 is the
                minimum value for the second ordering column, intColumn, and 10000 is
                the maximum value for that column.
        Default Value: "RANGE"
        Types: str
    manifest_file:
        Optional Argument.
        Specifies the fully qualified path and file name where the manifest
        file is written. Use the format
            "/connector/end point/bucket_or_container/prefix/manifest_file_name"
        For example:
            "/S3/ceph-s3.teradata.com/xz186000/manifest/manifest.json"
        If "manifest_file" argument is not included, no manifest file is
        written.
        Types: str
    manifest_only:
        Optional Argument.
        Specifies wheather to write only a manifest file in external storage
        or not. No actual data objects are written to external storage when
        "manifest_only" is set to True. One must also use the "manifest_file" 
        option to create a manifest file in external storage. Use this option 
        to create a new manifest file in the event that a WriteNOS()
        fails due to a database abort or restart, or when network connectivity 
        issues interrupt and stop a WriteNOS() before all data has 
        been written to external storage. The manifest is created from the 
        teradataml DataFrame that is input to WriteNOS(). 
        The input must be a DataFrame of storage object names and sizes, with 
        one row per object.
        Note: 
            The input to WriteNOS() with "manifest_only" can itself incorporate
            ReadNOS(), similar to this, which uses function mappings for WriteNOS()
            and ReadNOS():
                read_nos_obj = ReadNOS(return_type="NOSREAD_KEYS")
                WriteNOS(data=read_nos_obj.result.filter(items =['Location', 'ObjectLength']),
                         manifest_file=manifest_file,
                         manifest_only=True)
        Function calls like above can be used if a WriteNOS() fails before
        it can create a manifest file. The new manifest file created using
        ReadNOS() reflects all data objects currently in the external
        storage location, and can aid in determining which data objects
        resulted from the incomplete WriteNOS(). 
        For more information, see Teradata Vantage - Native Object Store 
        Getting Started Guide.
        Default Value: False
        Types: bool
    overwrite:
        Optional Argument.
        Specifies whether an existing manifest file in external storage is
        overwritten with a new manifest file that has the same name.
        When set to False, WriteNOS() returns an error if a manifest file exists in
        external storage that is named identically to the value of
        "manifest_file".
        Note: 
            Overwrite must be used with "manifest_only" set to True.
        Default Value: False
        Types: bool
    include_ordering:
        Optional Argument.
        Specifies whether column(s) specified in argument "data_order_column" and
        their values are written to external storage.
        Types: bool
    include_hashby:
        Optional Argument.
        Specifies whether column(s) specified in argument "data_hash_column" and
        their values are written to external storage.
        Types: bool
    max_object_size:
        Optional Argument.
        Specifies the maximum output object size in megabytes, where
        maximum object size can range between 4 and 16. The default is the
        value of the DefaultRowGroupSize field in DBS Control. 
        For more information on DBS Control, see Teradata Vantage - Database
        Utilities.
        Default Value: "16MB"
        Types: str
    compression:
        Optional Argument.
        Specifies the compression algorithm used to compress the objects
        written to external storage.
        Note:
            For Parquet files the compression occurs inside parts of the parquet
            file instead of for the entire file, so the file extension on
            external objects remains '.parquet'.
        Permitted Values: GZIP, SNAPPY
        Types: str
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept.
        Below are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the function in table or not.
                When set to True, results are persisted in table; otherwise, results
                are garbage collected at the end of the session.
                Default Value: False
                Types: boolean
            volatile:
                Optional Argument.
                Specifies whether to put the results of the function in volatile table or not.
                When set to True, results are stored in volatile table, otherwise not.
                Default Value: False
                Types: boolean
        Function allows the user to partition, hash, order or local order the input
        data. These generic arguments are available for each argument that accepts
        teradataml DataFrame as input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if the underlying
            Analytic Database function supports it, else an exception is raised.
RETURNS:
    Instance of WriteNOS.
    Output teradataml DataFrames can be accessed using attribute
    references, such as WriteNOSObj.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function will raise error if not supported on the Vantage
    #        user is connected to.
    # Check the list of available analytic functions.
    display_analytic_functions()
    # Load the example data.
    load_example_data("teradataml", "titanic")
    # Create teradataml DataFrame object.
    titanic_data = DataFrame.from_table("titanic")
    # Example 1: Writing the DataFrame to an AWS S3 location.
    obj =  WriteNOS(data=titanic_data,
                    location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                    authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                    stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 2: Write DataFrame to external storage with partition and order by
    #            column 'sex'.
    obj = WriteNOS(data=titanic_data,
                   location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"}, 
                   data_partition_columns="sex",
                   data_order_columns="sex",
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 3: Write DataFrame to external storage with hashing and order by
    #            column 'sex'.
    obj = WriteNOS(data=titanic_data,
                   location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                   data_hash_columns="sex",
                   data_order_columns="sex",
                   local_order_data=True,
                   include_hashing=True,
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 4: Write DataFrame to external storage with max object size as 4MB.
    obj = WriteNOS(data=titanic_data,
                   location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"}, 
                   include_ordering=True,
                   max_object_size='4MB',
                   compression='GZIP',
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 5: Write DataFrame of manifest table into a manifest file.
    import pandas as pd
    obj_names = ['/S3/s3.amazonaws.com/YOUR-STORAGE-ACCOUNT/20180701/ManifestFile/object_33_0_1.parquet',
                 '/S3/s3.amazonaws.com/YOUR-STORAGE-ACCOUNT/20180701/ManifestFile/object_33_6_1.parquet',
                 '/S3/s3.amazonaws.com/YOUR-STORAGE-ACCOUNT/20180701/ManifestFile/object_33_1_1.parquet']
    obj_size = [2803, 2733, 3009]
    manifest_df = pd.DataFrame({'ObjectName':obj_names, 'ObjectSize': obj_size})
    from teradataml import copy_to_sql
    copy_to_sql(manifest_df, "manifest_df")
    manifest_df = DataFrame("manifest_df")
    obj = WriteNOS(data=manifest_df,
                   location='YOUR-STORAGE-ACCOUNT/20180701/ManifestFile2/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                   manifest_file='YOUR-STORAGE-ACCOUNT/20180701/ManifestFile2/manifest2.json',
                   manifest_only=True,
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Note: 
    #   Before proceeding, verify with your database administrator that you have the 
    #   correct privileges, an authorization object, and a function mapping (for WRITE_NOS 
    #   function).
    # If function mapping for WRITE_NOS Analytic database function is created 
    # as 'WRITE_NOS_FM' and location and authorization object are mapped,
    # then set function mapping with teradataml options as below.
    # Example 7: Writing the DataFrame using function mapping.
    from teradataml.options.configure import configure
    configure.write_nos_function_mapping = "WRITE_NOS_FM"
    obj =  WriteNOS(data=titanic_data, stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
Supported on Database Version 17.20.xx
ReadNOS
ReadNOS
Functions
ReadNOS(data=None, location=None, authorization=None, return_type='NOSREAD_RECORD', sample_perc=1.0, stored_as='TEXTFILE', scan_pct=None,
manifest=False, table_format=None, row_format=None, header=True, **generic_arguments)
DESCRIPTION:
    ReadNOS() function enables access to external files in JSON, CSV, or Parquet format.
    User connected to Vanatge must have must have the EXECUTE FUNCTION privilege 
    on TD_SYSFNLIB.READ_NOS.
PARAMETERS:
    data:
        Optional Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml DataFrame
    location:
        Optional Argument.
        Specifies the location, which is a Uniform Resource Identifier
        (URI) pointing to the data in the external object storage system. 
        For different external storage locations, each has its own structure and components.
            For Amazon S3:
                /connector/bucket.endpoint/[key_prefix].
            For Azure Blob storage and Azure Data Lake Storage Gen2:
                /connector/storage-account.endpoint/[key_prefix].
            For Google Cloud Storage (GCS):
                /connector/endpoint/bucket/[key_prefix].
            connector:
                Identifies the type of external storage system where the data is located.
                Teradata requires the storage location to start with the following for
                all external storage locations:
                    * Amazon S3 storage location must begin with /S3 or /s3
                    * Azure Blob storage location (including Azure Data Lake Storage Gen2
                    in Blob Interop Mode) must begin with /AZ or /az.
                    * Google Cloud Storage location must begin with /GS or /gs.
            storage-account:
                The Azure storage account contains Azure storage data objects.
            endpoint:
                A URL that identifies the system-specific entry point for
                the external object storage system.
            bucket (Amazon S3, Google Cloud Storage) or container (Azure Blob
            storage and Azure Data Lake Storage Gen2):
                A container that logically groups stored objects in the
                external storage system.
            key_prefix:
                Identifies one or more objects in the logical organization of
                the bucket data. Because it is a key prefix, not an actual
                directory path, the key prefix may match one or more objects
                in the external storage. For example, the key prefix
                "/fabrics/cotton/colors/b/" would match objects
                /fabrics/cotton/colors/blue, /fabrics/cotton/colors/brown, and
                /fabrics/cotton/colors/black. If there are organization levels below
                those, such as /fabrics/cotton/colors/blue/shirts, the same key
                prefix would gather those objects too.
                Note:
                    Vantage validates only the first file it encounters from the
                    location key prefix.
            For example, following location value might specify all objects on an
            Amazon cloud storage system for the month of December, 2001:
            location = "/S3/YOUR-BUCKET.s3.amazonaws.com/csv/US-Crimes/csv-files/2001/Dec/"
                connector: S3
                bucket: YOUR-BUCKET
                endpoint: s3.amazonaws.com
                key_prefix: csv/US-Crimes/csv-files/2001/Dec/
            Following location could specify an individual storage object (or file), Day1.csv:
            location = "/S3/YOUR-BUCKET.s3.amazonaws.com/csv/US-Crimes/csv-files/2001/Dec/Day1.csv"
                connector: S3
                bucket: YOUR-BUCKET
                endpoint: s3.amazonaws.com
                key_prefix: csv/US-Crimes/csv-files/2001/Dec/Day1.csv
            Following location specifies an entire container in an Azure external
            object store (Azure Blob storage or Azure Data Lake Storage Gen2).
            The container may contain multiple file objects:
            location = "/AZ/YOUR-STORAGE-ACCOUNT.blob.core.windows.net/nos-csv-data"
                connector: AZ
                bucket: YOUR-STORAGE-ACCOUNT
                endpoint: blob.core.windows.net
                key_prefix: nos-csv-data
            This is an example of a Google Cloud Storage location:
            location = "/gs/storage.googleapis.com/YOUR-BUCKET/CSVDATA/RIVERS/rivers.csv"
                connector: GS
                bucket: YOUR-BUCKET
                endpoint: storage.googleapis.com,
                key_prefix: CSVDATA/RIVERS/rivers.csv
        Types: str
    authorization:
        Optional Argument.
        Specifies the authorization for accessing the external storage.
        Following are the options for using "authorization":
        * Passing access id and key as dictionary:
            authorization = {'Access_ID': 'access_id', 'Access_Key': 'secret_key'}
        * Passing access id and key as string in JSON format:
            authorization = '{"Access_ID":"access_id", "Access_Key":"secret_key"}'
        * Using Authorization Object:
            On any platform, authorization object can be specified as
            authorization = "[DatabaseName.]AuthorizationObjectName" 
            EXECUTE privilege on AuthorizationObjectName is needed.
        Below table specifies the USER (identification) and PASSWORD (secret_key) 
        for accessing external storage. The following table shows the supported 
        credentials for USER and PASSWORD (used in the CREATE AUTHORIZATION SQL command):
        +-----------------------+--------------------------+--------------------------+
        |     System/Scheme     |           USER           |         PASSWORD         |
        +-----------------------+--------------------------+--------------------------+
        |          AWS          |      Access Key ID       |    Access Key Secret     |
        |                       |                          |                          |
        |   Azure / Shared Key  |   Storage Account Name   |   Storage Account Key    |
        |                       |                          |                          |
        |  Azure Shared Access  |   Storage Account Name   |    Account SAS Token     |
        |    Signature (SAS)    |                          |                          |
        |                       |                          |                          |
        |      Google Cloud     |      Access Key ID       |    Access Key Secret     |
        |   (S3 interop mode)   |                          |                          |
        |                       |                          |                          |
        | Google Cloud (native) |       Client Email       |       Private Key        |
        |                       |                          |                          |
        |  Public access object |      <empty string>      |      <empty string>      |
        |        stores         | Enclose the empty string | Enclose the empty string |
        |                       |    in single straight    |    in single straight    |
        |                       |      quotes: USER ''     |    quotes: PASSWORD ''   |
        +-----------------------+--------------------------+--------------------------+
        When accessing GCS, Analytics Database uses either the S3-compatible connector or 
        the native Google connector, depending on the user credentials.
        Notes:
            * If using AWS IAM credentials, "authorization" can be omitted.
            * If S3 user account requires the use of physical or virtual security, 
              session token can be used with Access_ID and Access_Key in this syntax:
                  authorization = {"Access_ID":"access_id", 
                                   "Access_Key":"secret_key",
                                   "Session_Token":"session_token"}
              In which, the session token can be obtained using the AWS CLI. 
              For example: 
                  aws sts get-session-token
        Types: str or dict
    return_type:
        Optional Argument.
        Specifies the format in which data is returned.
        Permitted Values:
            * NOSREAD_RECORD:
                Returns one row for each external record along with its metadata.
                Access external records by specifying one of the following:
                    - Input teradataml DataFrame, location, and teradataml DataFrame
                      on an empty table. For CSV, you can include a schema definition.
                    - Input teradataml DataFrame with a row for each external file. For
                      CSV, this method does not support a schema definition.
                For an empty single-column input table, do the following:
                    - Define an input teradataml DataFrame with a single column, Payload,
                      with the appropriate data type:
                        JSON and CSV
                      This column determines the output Payload column return type.
                    - Specify the filepath in the "location" argument.
                For a multiple-column input table, define an input teradataml
                DataFrame with the following columns:
                    +------------------+-------------------------------------+
                    | Column Name      | Data Types                          |
                    +------------------+-------------------------------------+
                    | Location         | VARCHAR(2048) CHARACTER SET UNICODE |
                    +------------------+-------------------------------------+
                    | ObjectVersionID  | VARCHAR(1024) CHARACTER SET UNICODE |
                    +------------------+-------------------------------------+
                    | OffsetIntoObject | BIGINT                              |
                    +------------------+-------------------------------------+
                    | ObjectLength     | BIGINT                              |
                    +------------------+-------------------------------------+
                    | Payload          | JSON                                |
                    +------------------+-------------------------------------+
                    | CSV              | VARCHAR                             |
                    +------------------+-------------------------------------+
                This teradataml DataFrame can be populated using the output of the
                'NOSREAD_KEYS' return type.
            * NOSREAD_KEYS:
                Retrieve the list of files from the path specified in the "location" argument.
                A schema definition is not necessary.
                'NOSREAD_KEYS' returns Location, ObjectVersionID, ObjectTimeStamp,
                and ObjectLength (size of external file).
            * NOSREAD_PARQUET_SCHEMA:
                Returns information about the Parquet data schema.
                For information about the mapping between Parquet data types 
                and Teradata data types, see Parquet External Files in 
                Teradata Vantage - SQL Data Definition Language
                Syntax and Examples.
            * NOSREAD_SCHEMA:
                 Returns the name and data type of each column of the file specified in
                 the "location" argument. Schema format can be JSON, CSV, or Parquet.
        Default Value: "NOSREAD_RECORD"
        Types: str
    sample_perc:
        Optional Argument.
        Specifies the percentage of rows to retrieve from the external
        storage repository when "return_type" is 'NOSREAD_RECORD'. The valid
        range of values is from 0.0 to 1.0, where 1.0 represents 100%
        of the rows.
        Default Value: 1.0
        Types: float
    stored_as:
        Optional Argument.
        Specifies the formatting style of the external data.
        Permitted Values:
            * PARQUET-
                The external data is formatted as Parquet. This is a
                required parameter for Parquet data.
            * TEXTFILE-
                The external data uses a text-based format, such as
                CSV or JSON.
        Default Value: "TEXTFILE"
        Types: str
    scan_pct:
        Optional Argument.
        Specifies the percentage of "data" to be scanned to discover the schema.
        Value must be greater than or equal to 0 and less than or equal to 1.
            * For CSV and JSON files, 
              +-----------------+----------------------------------------------------------------------------+
              | scan_pct        | Scanned                                                                    |
              +-----------------+----------------------------------------------------------------------------+
              | 0 (default)     | First 100 MB of data set (file by file if there are multiple files).       |
              +-----------------+----------------------------------------------------------------------------+
              | Between 0 and 1 | First, first 100 MB of data set (file by file if there are multiple files).|
              |                 | Then, scan_pct * 100% of first 1000 remaining files.                       |
              +-----------------+----------------------------------------------------------------------------+
              | 1               | First 1000 files of data set.                                              |
              +-----------------+----------------------------------------------------------------------------+
            * For Parquet files, when "scan_pct" is set to:
              +-------------+----------------------------------------------------------------------+
              | scan_pct    | Scanned                                                              |
              +-------------+----------------------------------------------------------------------+
              | Less than 1 | First 100 MB of data set (file by file if there are multiple files). |
              +-------------+----------------------------------------------------------------------+
              | 1           | First 16 MB of each file, up to 100 MB of data set.                  |
              +-------------+----------------------------------------------------------------------+
        Types: float
    manifest:
        Optional Argument.
        Specifies whether the location value points to a manifest file (a
        file containing a list of files to read) or object name. The object
        name can include the full path or a partial path. It must identify a
        single file containing the manifest.
        Note:
            Individual entries within the manifest file must show
            complete paths.
        Below is an example of a manifest file that contains a list of locations 
        in JSON format.
        {
          "entries": [
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-8_9_02-10.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-8_9_02-101.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-10-01/data-8_9_02-102.json"},
                {"url":"s3://nos-core-us-east-1/UNICODE/JSON/mln-key/data-10/data-10-01/data-8_9_02-103.json"}
           ]
        }
        Default Value: False
        Types: bool
    table_format:
        Optional Argument.
        Specifies the format of the tables specified in manifest file.
        Note:
            * "manifest" must be set to True.
            * "location": value must include _symlink_format_manifest.
               For example:
                /S3/YOUR-BUCKET.s3.amazonaws.com/testdeltalake/deltalakewp/_symlink_format_manifest
                /S3/YOUR-
BUCKET.s3.amazonaws.com/testdeltalake/deltalakewp/_symlink_format_manifest/zip=95661/manifest
        Types: str
    row_format:
        Optional Argument.
        Specifies the encoding format of the external row.
        For example:
            row_format = {'field_delimiter': ',', 'record_delimiter': '\n', 'character_set': 'LATIN'}
        If string value is used, JSON format must be used to specify the row format.
        For example:
            row_format = '{"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}'
        Format can include only the three keys shown above. Key names and values 
        are case-specific, except for the value for "character_set", which can 
        use any combination of letter cases.
        The character set specified in "row_format" must be compatible with the character 
        set of the Payload column.
        Do not specify "row_format" for Parquet format data.
        For a JSON column, these are the default values:
            UNICODE:
                row_format = {"record_delimiter":"\n", "character_set":"UTF8"}
            LATIN:
                row_format = {"record_delimiter":"\n", "character_set":"LATIN"}
        For a CSV column, these are the default values:
            UNICODE:
                row_format = '{"character_set":"UTF8"}'
            LATIN:
                row_format = '{"character_set":"LATIN"}'
        User can specify the following options:
            field_delimiter: The default is "," (comma). User can also specify a
                             custom field delimiter, such as tab "\t".
            record_delimiter: New line feed character: "\n". A line feed
                              is the only acceptable record delimiter.
            character_set: "UTF8" or "LATIN". If you do not specify a "row_format"
                           or payload column, Vantage assumes UTF8 Unicode.
        Types: str or dict
    header:
        Optional Argument.
        Specifies whether the first row of data in an input CSV file is
        interpreted as column headings for the subsequent rows of data. Use
        this parameter only when a CSV input file is not associated with a
        separate schema object that defines columns for the CSV data.
        Default Value: True
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept.
        Below are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the function in table or not.
                When set to True, results are persisted in table; otherwise, results
                are garbage collected at the end of the session.
                Default Value: False
                Types: boolean
            volatile:
                Optional Argument.
                Specifies whether to put the results of the function in volatile table or not.
                When set to True, results are stored in volatile table, otherwise not.
                Default Value: False
                Types: boolean
        Function allows the user to partition, hash, order or local order the input
        data. These generic arguments are available for each argument that accepts
        teradataml DataFrame as input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if the underlying Analytic Database
            function supports, else an exception is raised.
RETURNS:
    Instance of ReadNOS.
    Output teradataml DataFrames can be accessed using attribute
    references, such as ReadNOS.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function will raise error if not supported on the Vantage
    #        user is connected to.
    # Check the list of available analytic functions.
    display_analytic_functions()
    # Example 1: Reading PARQUET file from AWS S3 location.
    obj =  ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/location/",
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                   stored_as="PARQUET")
    # print the result DataFame.
    print(obj.result)
    # Example 2: Read PARQUET file in external storage with one row for each external 
    #            record along with its metadata.
    obj = ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/location/",
                  authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                  return_type="NOSREAD_KEYS")
    # print the result DataFame.
    print(obj.result)
    # Example 3: Read CSV file from external storage.
    obj = ReadNOS(location="/S3/s3.amazonaws.com/Your-Bucket/csv-location/",
                  authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                  stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
    # Example 4: Read CSV file in external storage using "data" argument.
    # Create a table to store the data.
    con = create_context(host=host, user=user, password=password)
    execute_sql("CREATE TABLE read_nos_support_tab (payload dataset storage format csv) NO PRIMARY INDEX;")
    read_nos_support_tab = DataFrame("read_nos_support_tab")
    # Read the CSV data using "data" argument.
    obj = ReadNOS(data=read_nos_support_tab,
                  location="/S3/s3.amazonaws.com/Your-Bucket/csv-location/",
                  authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                  row_format={"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}
                  stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
    # Note: 
    #   Before proceeding, verify with your database administrator that you have the 
    #   correct privileges, an authorization object, and a function mapping (for READ_NOS 
    #   function).
    # If function mapping for READ_NOS Analytic database function is created 
    # as 'READ_NOS_FM' and location and authorization object are mapped,
    # then set function mapping with teradataml options as below.
    # Example 5: Setting function mapping using configuration options.
    from teradataml.options.configure import configure
    configure.read_nos_function_mapping = "READ_NOS_FM"
    obj =  ReadNOS(data=read_nos_support_tab,
                   row_format={"field_delimiter": ",", "record_delimiter": "\n", "character_set": "LATIN"}
                   stored_as="TEXTFILE")
    # print the result DataFame.
    print(obj.result)
WriteNOS
WriteNOS
Functions
WriteNOS(data=None, location=None, authorization=None, stored_as='PARQUET', naming='RANGE', header=True, row_format=None, manifest_ﬁle=None,
manifest_only=False, overwrite=False, include_ordering=None, include_hashby=None, max_object_size='16MB', compression=None, **generic_arguments)
DESCRIPTION:
    WriteNOS() function enables access to write input teradataml DataFrame to external storage,
    like Amazon S3, Azure Blob storage, or Google Cloud Storage.
    You must have the EXECUTE FUNCTION privilege on TD_SYSFNLIB.WRITE_NOS.
PARAMETERS:
    data:
        Required Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml DataFrame
    location:
        Optional Argument.
        Specifies the location value, which is a Uniform Resource Identifier
        (URI) pointing to location in the external object storage system.
        A URI identifying the external storage system in the format:
            /connector/endpoint/bucket_or_container/prefix
        The "location" string cannot exceed 2048 characters.
        Types: str
    authorization:
        Optional Argument.
        Specifies the authorization for accessing the external storage.
        Following are the options for using "authorization":
        * Passing access id and key as dictionary:
            authorization = {'Access_ID': 'access_id', 'Access_Key': 'secret_key'}
        * Passing access id and key as string in JSON format:
            authorization = '{"Access_ID":"access_id", "Access_Key":"secret_key"}'
        * Using Authorization Object:
            On any platform, authorization object can be specified as
            authorization = "[DatabaseName.]AuthorizationObjectName" 
            EXECUTE privilege on AuthorizationObjectName is needed.
        Below table specifies the USER (identification) and PASSWORD (secret_key) 
        for accessing external storage. The following table shows the supported 
        credentials for USER and PASSWORD (used in the CREATE AUTHORIZATION SQL command):
        +-----------------------+--------------------------+--------------------------+
        |     System/Scheme     |           USER           |         PASSWORD         |
        +-----------------------+--------------------------+--------------------------+
        |          AWS          |      Access Key ID       |    Access Key Secret     |
        |                       |                          |                          |
        |   Azure / Shared Key  |   Storage Account Name   |   Storage Account Key    |
        |                       |                          |                          |
        |  Azure Shared Access  |   Storage Account Name   |    Account SAS Token     |
        |    Signature (SAS)    |                          |                          |
        |                       |                          |                          |
        |      Google Cloud     |      Access Key ID       |    Access Key Secret     |
        |   (S3 interop mode)   |                          |                          |
        |                       |                          |                          |
        | Google Cloud (native) |       Client Email       |       Private Key        |
        |                       |                          |                          |
        |  Public access object |      <empty string>      |      <empty string>      |
        |        stores         | Enclose the empty string | Enclose the empty string |
        |                       |    in single straight    |    in single straight    |
        |                       |      quotes: USER ''     |    quotes: PASSWORD ''   |
        +-----------------------+--------------------------+--------------------------+
        When accessing GCS, Analytics Database uses either the S3-compatible connector or 
        the native Google connector, depending on the user credentials.
        Notes:
            * If using AWS IAM credentials, "authorization" can be omitted.
            * If S3 user account requires the use of physical or virtual security, 
              session token can be used with Access_ID and Access_Key in this syntax:
                  authorization = {"Access_ID":"access_id", 
                                   "Access_Key":"secret_key",
                                   "Session_Token":"session_token"}
              In which, the session token can be obtained using the AWS CLI. 
              For example: 
                  aws sts get-session-token
        Types: str or dict
    stored_as:
        Optional Argument.
        Specifies the formatting style of the external data.
        PARQUET means the external data is formatted as Parquet.
        Permitted Values:
            * PARQUET
            * CSV
        Default Value: "PARQUET"
        Types: str
    naming:
        Optional Argument.
        Specifies how the objects containing the rows of data are named
        in the external storage:
        Permitted Values:
            * DISCRETE-
                Discrete naming uses the ordering column values as part of the object
                names in external storage. For example, if the "data_order_column"
                has ["dateColumn", "intColumn"], the discrete form name of the objects 
                written to external storage would include the values for those columns 
                as part of the object name, which would look similar to this:
                    S3/ceph-s3.teradata.com/xz186000/2019-03-01/13/object_33_0_1.parquet
                Where 2019-03-01 is the value for the first ordering column, "dateColumn", 
                and 13 is the value for the second ordering column, "intColumn".
                All rows stored in this external Parquet-formatted object contain those
                two values.
            * RANGE-
                Range naming includes part of the object name as range of values included 
                in the partition for each ordering column.
                For example, using the same "data_order_column" as above the object
                names would look similar to this:
                    S3/ceph-s3.teradata.com/xz186000/2019-01-01/2019-03-02/9/10000/object_33_0_1.parquet
                In this external Parquet-formatted object:
                    - 2019-01-01 and 2019-03-02 are the minimum and maximum values for 
                      the first ordering column, dateColumn.
                    - 9 and 10000 are the minimum and maximum values for the second 
                      ordering column, intColumn.
        Default Value: "RANGE"
        Types: str
    header:
        Optional Argument.
        Specifies wheather the first record contains the column names.
        Default Value: True
        Types: bool
    row_format:
        Optional Argument.
        Specifies the encoding format for the rows in the file type specified in 
        "stored_as".
        Notes:
            * For CSV data, encoding format is:
                  {"field_delimiter": "fd_value", 
                   "record_delimiter": "\n", 
                   "character_set": "cs_value"}
            * For Parquet data, encoding format is:
                  {"character_set": "cs_value"}
              field_delimiter:
                  Specifies the field delimiter. Default field delimiter is "," (comma). 
                  User can also specify a custom field delimiter, such as tab "\t". 
                  The key name and "fd_value" are case-sensitive.
              record_delimiter: 
                  Specifies the record delimiter, which is the line feed character, "\n". 
                  The key name and "\n" are case-sensitive.
              character_set:
                  Specifies the field character set "UTF8" or "LATIN". 
                  The key name is case-sensitive, but "cs_value" is not.     
        Types: str or dict
    manifest_file:
        Optional Argument.
        Specifies the fully qualified path and file name where the manifest
        file is written. Use the format
            "/connector/end point/bucket_or_container/prefix/manifest_file_name"
        For example:
            "/S3/ceph-s3.teradata.com/xz186000/manifest/manifest.json"
        If "manifest_file" argument is not included, no manifest file is
        written.
        Types: str
    manifest_only:
        Optional Argument.
        Specifies wheather to write only a manifest file in external storage
        or not. No actual data objects are written to external storage when
        "manifest_only" is set to True. One must also use the "manifest_file" 
        option to create a manifest file in external storage. Use this option 
        to create a new manifest file in the event that a WriteNOS()
        fails due to a database abort or restart, or when network connectivity 
        issues interrupt and stop a WriteNOS() before all data has 
        been written to external storage. The manifest is created from the 
        teradataml DataFrame that is input to WriteNOS(). 
        The input must be a DataFrame of storage object names and sizes, with 
        one row per object.
        Note: 
            The input to WriteNOS() with "manifest_only" can itself incorporate
            ReadNOS(), similar to this, which uses function mappings for WriteNOS()
            and ReadNOS():
                read_nos_obj = ReadNOS(return_type="NOSREAD_KEYS")
                WriteNOS(data=read_nos_obj.result.filter(items =['Location', 'ObjectLength']),
                         manifest_file=manifest_file,
                         manifest_only=True)
        Function calls like above can be used if a WriteNOS() fails before
        it can create a manifest file. The new manifest file created using
        ReadNOS() reflects all data objects currently in the external
        storage location, and can aid in determining which data objects
        resulted from the incomplete WriteNOS(). 
        For more information, see Teradata Vantage - Native Object Store 
        Getting Started Guide.
        Default Value: False
        Types: bool
    overwrite:
        Optional Argument.
        Specifies whether an existing manifest file in external storage is
        overwritten with a new manifest file that has the same name.
        When set to False, WriteNOS() returns an error if a manifest file exists in
        external storage that is named identically to the value of
        "manifest_file".
        Note: 
            Overwrite must be used with "manifest_only" set to True.
        Default Value: False
        Types: bool
    include_ordering:
        Optional Argument.
        Specifies whether column(s) specified in argument "data_order_column" and
        their values are written to external storage.
        Types: bool
    include_hashby:
        Optional Argument.
        Specifies whether column(s) specified in argument "data_hash_column" and
        their values are written to external storage.
        Types: bool
    max_object_size:
        Optional Argument.
        Specifies the maximum output object size in megabytes, where
        maximum object size can range between 4 and 16. The default is the
        value of the DefaultRowGroupSize field in DBS Control. 
        For more information on DBS Control, see Teradata Vantage - Database
        Utilities.
        Default Value: "16MB"
        Types: str
    compression:
        Optional Argument.
        Specifies the compression algorithm used to compress the objects
        written to external storage.
        Note:
            For Parquet files the compression occurs inside parts of the parquet
            file instead of for the entire file, so the file extension on
            external objects remains '.parquet'.
        Permitted Values: GZIP, SNAPPY
        Types: str
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept.
        Below are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the function in table or not.
                When set to True, results are persisted in table; otherwise, results
                are garbage collected at the end of the session.
                Default Value: False
                Types: boolean
            volatile:
                Optional Argument.
                Specifies whether to put the results of the function in volatile table or not.
                When set to True, results are stored in volatile table, otherwise not.
                Default Value: False
                Types: boolean
        Function allows the user to partition, hash, order or local order the input
        data. These generic arguments are available for each argument that accepts
        teradataml DataFrame as input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if the underlying
            Analytic Database function supports it, else an exception is raised.
RETURNS:
    Instance of WriteNOS.
    Output teradataml DataFrames can be accessed using attribute
    references, such as WriteNOSObj.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function will raise error if not supported on the Vantage
    #        user is connected to.
    # Check the list of available analytic functions.
    display_analytic_functions()
    # Load the example data.
    load_example_data("teradataml", "titanic")
    # Create teradataml DataFrame object.
    titanic_data = DataFrame.from_table("titanic")
    # Example 1: Writing the DataFrame to an AWS S3 location.
    obj =  WriteNOS(data=titanic_data,
                    location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                    authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                    stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 2: Write DataFrame to external storage with partition and order by
    #            column 'sex'.
    obj = WriteNOS(data=titanic_data,
                   location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"}, 
                   data_partition_columns="sex",
                   data_order_columns="sex",
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 3: Write DataFrame to external storage with hashing and order by
    #            column 'sex'.
    obj = WriteNOS(data=titanic_data,
                   location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                   data_hash_columns="sex",
                   data_order_columns="sex",
                   local_order_data=True,
                   include_hashing=True,
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 4: Write DataFrame to external storage with max object size as 4MB.
    obj = WriteNOS(data=titanic_data,
                   location='/S3/s3.amazonaws.com/Your-Bucket/location/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"}, 
                   include_ordering=True,
                   max_object_size='4MB',
                   compression='GZIP',
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 5: Write DataFrame of manifest table into a manifest file.
    import pandas as pd
    obj_names = ['/S3/s3.amazonaws.com/YOUR-STORAGE-ACCOUNT/20180701/ManifestFile/object_33_0_1.parquet',
                 '/S3/s3.amazonaws.com/YOUR-STORAGE-ACCOUNT/20180701/ManifestFile/object_33_6_1.parquet',
                 '/S3/s3.amazonaws.com/YOUR-STORAGE-ACCOUNT/20180701/ManifestFile/object_33_1_1.parquet']
    obj_size = [2803, 2733, 3009]
    manifest_df = pd.DataFrame({'ObjectName':obj_names, 'ObjectSize': obj_size})
    from teradataml import copy_to_sql
    copy_to_sql(manifest_df, "manifest_df")
    manifest_df = DataFrame("manifest_df")
    obj = WriteNOS(data=manifest_df,
                   location='YOUR-STORAGE-ACCOUNT/20180701/ManifestFile2/',
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                   manifest_file='YOUR-STORAGE-ACCOUNT/20180701/ManifestFile2/manifest2.json',
                   manifest_only=True,
                   stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
    # Example 7: Write teradataml DataFrame to external object store in CSV format with a header
    #            and field delimiter as '\t' (tab).
    obj = WriteNOS(location=location,
                   authorization={"Access_ID": "YOUR-ID", "Access_Key": "YOUR-KEY"},
                   stored_as="CSV",
                   header=True,
                   row_format={"field_delimiter":"\t", "record_delimiter":"\n"},
                   data=titanic_data)
    # Print the result DataFrame.
    print(obj.result)
    # Note: 
    #   Before proceeding, verify with your database administrator that you have the 
    #   correct privileges, an authorization object, and a function mapping (for WRITE_NOS 
    #   function).
    # If function mapping for WRITE_NOS Analytic database function is created 
    # as 'WRITE_NOS_FM' and location and authorization object are mapped,
    # then set function mapping with teradataml options as below.
    # Example 8: Writing the DataFrame using function mapping.
    from teradataml.options.configure import configure
    configure.write_nos_function_mapping = "WRITE_NOS_FM"
    obj =  WriteNOS(data=titanic_data, stored_as='PARQUET')
    # Print the result DataFrame.
    print(obj.result)
Image2Matrix
Image2Matrix
Functions
Image2Matrix(data=None, output='gray', **generic_arguments)
DESCRIPTION:
    Image2Matrix() function converts an image to a matrix.
    It converts JPEG or PNG images to matrixes with payload values being the pixel values.
    Note:
        * The image size cannot be greater than 16 MB.
        * The image should not exceed 4,000,000 pixels.
PARAMETERS:
    data:
        Required Argument.
        Specifies the teradataml DataFrame which has image details.
        Types: Teradataml DataFrame
    output:
        Optional Argument.
        Specifies the type of output matrix.
        Default: 'gray'
        Permitted Values:
            'gray': Converts the image to a grayscale matrix.
            'rgb': Converts the image to a RGB matrix.
        Types: str
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept.
        Below are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the function in table or not.
                When set to True, results are persisted in table; otherwise, results
                are garbage collected at the end of the session.
                Default Value: False
                Types: boolean
            volatile:
                Optional Argument.
                Specifies whether to put the results of the function in volatile table or not.
                When set to True, results are stored in volatile table, otherwise not.
                Default Value: False
                Types: boolean
        Function allows the user to partition, hash, order or local order the input
        data. These generic arguments are available for each argument that accepts
        teradataml DataFrame as input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if the underlying Analytic Database
            function supports, else an exception is raised.
RETURNS:
    Instance of Image2Matrix.
    Output teradataml DataFrames can be accessed using attribute
    references, such as Image2Matrix.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage, before importing the
    #        function in user space.
    #     2. User can import the function, if it is available on
    #        Vantage user is connected to.
    #     3. To check the list of UAF analytic functions available
    #        on Vantage user connected to, use
    #        "display_analytic_functions()".
    # Check the list of available analytic functions.
    display_analytic_functions()
    # Import function Image2Matrix.
    from teradataml import Image2Matrix
    import teradataml
    # Drop the image table if it is present.
    try:
        db_drop_table('imageTable')
    except:
        pass
    # Create a table to store the image data.
    execute_sql('CREATE TABLE imageTable(id INTEGER, image BLOB);')
    # Load the image data into the fileContent variable.