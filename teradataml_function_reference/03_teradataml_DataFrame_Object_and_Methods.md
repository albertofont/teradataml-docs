# teradataml DataFrame Object and Methods

__init__
teradataml.dataframe.dataframe.DataFrame.__init__ = __init__(self, table_name=None, index=True, index_label=None, query=None, materialize=False)
    Constructor for teradataml DataFrame.
    PARAMETERS:
        table_name:
            Optional Argument.
            The table name or view name in Teradata Vantage referenced by this DataFrame.
            Types: str
        index:
            Optional Argument.
            True if using index column for sorting, otherwise False.
            Default Value: True
            Types: bool
        index_label:
            Optional Argument.
            Column/s used for sorting.
            Types: str OR list of Strings (str)
        query:
            Optional Argument.
            SQL query for this Dataframe. Used by class method from_query.
            Types: str
        materialize:
            Optional Argument.
            Whether to materialize DataFrame or not when created.
            Used by class method from_query.
            You should use materialization, when the query  passed to from_query(),
            is expected to produce non-deterministic results, when it is executed multiple
            times. Using this option will help user to have deterministic results in the
            resulting teradataml DataFrame.
            Default Value: False (No materialization)
            Types: bool
    EXAMPLES:
        from teradataml.dataframe.dataframe import DataFrame
        # Example 1: The following example creates a DataFrame from the 'table_name'
        #            or 'view_name'.
        # Created DataFrame using table name.
        df = DataFrame("mytab")
        # Created DataFrame using view name.
        df = DataFrame("myview")
        # Created DataFrame using view name without using index column for sorting.
        df = DataFrame("myview", False)
        # Created DataFrame using table name and sorted using Col1 and Col2
        df = DataFrame("mytab", True, "Col1, Col2")
        # Example 2: The following example creates a DataFrame from the existing Vantage
        #            table "dbcinfo" in the non-default database "dbc" using the
        #            in_schema() function.
        from teradataml.dataframe.dataframe import in_schema
        df = DataFrame(in_schema("dbc", "dbcinfo"))
    RAISES:
        TeradataMlException - TDMLDF_CREATE_FAIL
    __repr__
    teradataml.dataframe.dataframe.DataFrame.__repr__ = __repr__(self)
    Returns the string representation for a teradataml DataFrame instance.
    The string contains:
        1. Column names of the dataframe.
        2. At most the first no_of_rows rows of the dataframe.
        3. A default index for row numbers.
    NOTES:
      - This makes an explicit call to get rows from the database.
      - To change number of rows to be printed set the max_rows option in
        "options.display.display".
      - Default value of max_rows is 10.
    EXAMPLES:
        df = DataFrame.from_table("table1")
        print(df)
        df = DataFrame.from_query("select col1, col2, col3 from table1")
        print(df)
    __getattr__
    teradataml.dataframe.dataframe.DataFrame.__getattr__ = __getattr__(self, name)
    Returns an attribute of the DataFrame.
    PARAMETERS:
      name: the name of the attribute
    RETURNS:
      Return the value of the named attribute of object (if found).
    EXAMPLES:
      df = DataFrame('table')
      # you can access a column from the DataFrame
      df.c1
    RAISES:
      Attribute Error when the named attribute is not found
    __getitem__
    teradataml.dataframe.dataframe.DataFrame.__getitem__ = __getitem__(self, key)
    Return a column from the DataFrame or filter the DataFrame using an expression
    or select columns from a DataFrame using the specified list of columns..
    The following operators are supported:
      comparison: ==, !=, <, <=, >, >=
      boolean: & (and), | (or), ~ (not), ^ (xor)
    Operands can be Python literals and instances of ColumnExpressions from the DataFrame
    PARAMETERS:
        key:
            Required Argument.
            Specifies the column name(s) of filter expression (ColumnExpression).
            Specify key as:
                * a string - to get the column of a teradataml DataFrame, i.e., a ColumnExpression.
                * list of strings - to select columns of a teradataml DataFrame.
                * filter expression - to filter the data from teradataml DataFrame using the specified expression.
            Types: str or list of strings or ColumnExpression
    RETURNS:
        DataFrame or ColumnExpression instance
    RAISES:
        1. KeyError   - If key is not found
        2. ValueError - When columns of different dataframes are given in ColumnExpression.
    EXAMPLES:
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        df = DataFrame("titanic")
        # Filter the DataFrame df.
        df[df.sibsp > df.parch]
        df[df.age >= 50]
        df[df.sex == "female"]
        df[1 != df.parch]
        df[~(1 < df.parch)]
        df[(df.parch > 0) & (df.sibsp > df.parch)]
        # Retrieve column cabin from df.
        df["cabin"]
        # Select columns using list of column names from a teradataml DataFrame..
        df[["embarked", "age", "cabin"]]
    from_query
    teradataml.dataframe.dataframe.DataFrame.from_query = from_query(query, index=True, index_label=None, materialize=False) method of builtins.type instance
    Class method for creating a DataFrame from a query.
    PARAMETERS:
        query:
            Required Argument.
            Specifies the Teradata Vantage SQL query referenced by this DataFrame.
            Only "SELECT" queries are supported. Exception will be
            raised, if any other type of query is passed.
            Unsupported queries include:
                1. DDL Queries like CREATE, DROP, ALTER etc.
                2. DML Queries like INSERT, UPDATE, DELETE etc. except SELECT.
                3. SELECT query with ORDER BY clause.
            Types: str
        index:
            Optional Argument.
            True if using index column for sorting otherwise False.
            Default Value: True
            Types: bool
        index_label:
            Optional Argument.
            Column/s used for sorting.
            Types: str
        materialize:
            Optional Argument.
            Whether to materialize DataFrame or not when created.
            You should use materialization, when the query  passed to from_query(),
            is expected to produce non-deterministic results, when it is executed multiple
            times. Using this option will help user to have deterministic results in the
            resulting teradataml DataFrame.
            Default Value: False (No materialization)
            Types: bool
    EXAMPLES:
        from teradataml.dataframe.dataframe import DataFrame
        load_example_data("dataframe","sales")
        df = DataFrame.from_query("select accounts, Jan, Feb from sales")
        df = DataFrame.from_query("select accounts, Jan, Feb from sales", False)
        df = DataFrame.from_query("select * from sales", True, "accounts")
    RETURNS:
        DataFrame
    RAISES:
        TeradataMlException - TDMLDF_CREATE_FAIL
    from_table
    teradataml.dataframe.dataframe.DataFrame.from_table = from_table(table_name, index=True, index_label=None, schema_name=None, datalake_name=None) method of
    builtins.type instance
    Class method for creating a DataFrame from a table or a view.
    PARAMETERS:
        table_name:
            Required Argument.
            The table name in Teradata Vantage referenced by this DataFrame.
            Types: str
        index:
            Optional Argument.
            True if using index column for sorting otherwise False.
            Default Value: True
            Types: bool
        index_label:
            Optional
            Column/s used for sorting.
            Types: str
        schema_name:
            Optional Argument.
            Specifies the schema where the table resides.
            Types: str
        datalake_name:
            Optional Argument.
            Specifies the datalake name.
            Types: str
    EXAMPLES:
        >>> from teradataml.dataframe.dataframe import DataFrame
        # Example 1: The following example creates a DataFrame from a table or
                     a view.
        # Load the example data.
        >>> load_example_data("dataframe","sales")
        # Create DataFrame from table
        >>> df = DataFrame.from_table('sales')
        # Create DataFrame from table and without index column sorting.
        >>> df = DataFrame.from_table("sales", False)
        # Create DataFrame from table and sorting using the 'accounts'
        # column.
        >>> df = DataFrame.from_table("sales", True, "accounts")
        # Example 2: The following example creates a DataFrame from existing Vantage
        #            table "dbcinfo" in the non-default database "dbc" using the
        #            in_schema() function.
        >>> from teradataml.dataframe.dataframe import in_schema
        >>> df = DataFrame.from_table(in_schema("dbc", "dbcinfo"))
        # Example 3: Create a DataFrame on existing DataLake
        #            table "lake_table" in the "datalake_database" database
        #            in "datalake" datalake.
        >>> datalake_df = DataFrame.from_table(table_name="lake_table",
        ...                                    schema_name="datalake_database",
        ...                                    datalake_name="datalake" )
    RETURNS:
        DataFrame
    RAISES:
        TeradataMlException - TDMLDF_CREATE_FAIL
    alias
    teradataml.dataframe.dataframe.DataFrame.alias = alias(self, alias_name)
    DESCRIPTION:
        Method to create an aliased teradataml DataFrame.
        Note:
            * This method is recommended to be used before performing
              self join using DataFrame's join() API.
    PARAMETERS:
        alias_name:
            Required Argument.
            Speciﬁes the alias name to be assigned to a teradataml DataFrame.
            Types: str
    RETURNS:
        teradataml DataFrame
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        19     yes  1.98  Advanced    Advanced         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        38     yes  2.65  Advanced    Beginner         1
        # Example 1: Create an alias of teradataml DataFrame.
        >>> df2 = df.alias("adm_trn")
        # Print aliased DataFrame.
        >>> df2
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        19     yes  1.98  Advanced    Advanced         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        38     yes  2.65  Advanced    Beginner         1
    apply
    teradataml.dataframe.dataframe.DataFrame.apply = apply(self, user_function, exec_mode='REMOTE', chunk_size=1000, num_rows=1000, **kwargs)
            DESCRIPTION:
                Function to apply a user-defined function(UDF) to each row in the
                teradataml DataFrame, leveraging Apply Table Operator of Open
                Analytics Framework.
                Notes:
                    1. The function requires to use same Python version in both remote environment and local environment.
                    2. Teradata recommends to use "dill" package with same version in both remote environment and
                       local environment.
            PARAMETERS:
                user_function:
                    Required Argument.
                    Specifies the user defined function to apply to each row in
                    the teradataml DataFrame.
                    Types: function or functools.partial
                    Notes:
                        * This can be either a lambda function, a regular python
                          function, or an object of functools.partial.
                        * The first argument (positional) to the UDF must be a row
                          in a Pandas DataFrame corresponding to the teradataml
                          DataFrame to which it is to be applied.
                        * A non-lambda function can be passed only when the UDF
                          does not accept any arguments other than
                          the mandatory input - the input row.
                          A user can also use functools.partial and lambda functions
                          for the same, which are especially handy when:
                              * there is a need to pass positional and/or keyword
                                arguments (lambda).
                              * there is a need to pass keyword arguments only
                                (functool.partial).
                        * The return type of the UDF must be one
                          of the following:
                              * numpy ndarray
                                  * when one-dimensional, having the same number of
                                    values as output columns.
                                  * when two-dimensional, every array contained in
                                    the outer array having the same number of values
                                    as output columns.
                              * pandas Series
                              * pandas DataFrame
                        * To use apply on the UDF, packages dill and pandas must be
                          installed in remote user environment using install_lib function
                          of UserEnv class. Refer to help(UserEnv.install_lib).
                exec_mode:
                    Optional Argument.
                    Specifies the mode of execution for the UDF.
                    Default value: 'REMOTE'
                    Types: str
                chunk_size:
                    Optional Argument.
                    Specifies the number of rows to be read in a chunk in each
                    iteration using an iterator to apply the UDF
                    to each row in the chunk.
                    Varying the value passed to this argument affects the
                    performance and the memory utilized by the function.
                    Default value: 1000
                    Types: int
                num_rows:
                    For future use.
                env_name:
                    Required Argument.
                    Specifies the name of the remote user environment or an object of
                    class UserEnv.
                    Types: str or oject of class UserEnv.
                returns:
                    Optional Argument.
                    Specifies the output column definition corresponding to the
                    output of "user_function".
                    When not specified, the function assumes that the names and
                    types of the output columns are same as that of the input.
                    Types: Dictionary specifying column name to
                           teradatasqlalchemy type mapping.
                delimiter:
                    Optional Argument.
                    Specifies a delimiter to use when reading columns from a row and
                    writing result columns.
                    Default value: ','
                    Types: str with one character
                    Notes:
                        * This argument cannot be same as "quotechar" argument.
                        * This argument cannot be a newline character ('
    ').
                quotechar:
                    Optional Argument.
                    Specifies a character that forces all input and output of the
                    user function to be quoted using this specified character.
                    Using this argument enables the Advanced SQL Engine to
                    distinguish between NULL fields and empty strings.
                    A string with length zero is quoted, while NULL fields are not.
                    If this character is found in the data, it will be escaped by a
                    second quote character.
                    Types: str with one character
                    Notes:
                        * This argument cannot be same as "delimiter" argument.
                        * This argument cannot be a newline character ('
    ').
                data_partition_column:
                    Optional Argument.
                    Specifies the Partition By columns for the teradataml DataFrame.
                    Values to this argument can be provided as a list, if multiple
                    columns are used for partition. If there is no "data_partition_column",
                    then the entire result set delivered by the function, constitutes a single
                    group or partition.
                    Default Value: ANY
                    Types: str OR list of Strings (str)
                    Notes:
                        1) "data_partition_column" can not be specified along with "data_hash_column".
                        2) "data_partition_column" can not be specified along with "is_local_order = True".
                data_hash_column:
                    Optional Argument.
                    Specifies the column to be used for hashing.
                    The rows in the teradataml DataFrame are redistributed to AMPs
                    based on the hash value of the column specified.
                    If there is no data_hash_column, then the entire result
                    set, delivered by the function, constitutes a single group or
                    partition.
                    Types: str
                    Notes:
                        1. "data_hash_column" can not be specified along with "data_partition_column".
                        2. "data_hash_column" can not be specified along with "is_local_order=False" and
                           "data_order_column".
                data_order_column:
                    Optional Argument.
                    Specifies the Order By columns for the teradataml DataFrame.
                    Values to this argument can be provided as a list, if multiple
                    columns are used for ordering.
                    This argument is used with in both cases:
                    "is_local_order = True" and "is_local_order = False".
                    Types: str OR list of Strings (str)
                    Note:
                        "data_order_column" can not be specified along with "data_hash_column".
                is_local_order:
                    Optional Argument.
                    Specifies a boolean value to determine whether the input data is
                    to be ordered locally or not.
                    "data_order_column" with "is_local_order" set to 'False'
                    specifies the order in which the values in a group, or
                    partition, are sorted.
                    When this argument is set to 'True', qualified rows on each AMP
                    are ordered in preparation to be input to a table function.
                    This argument is ignored, if "data_order_column" is None.
                    Default value: False
                    Types: bool
                    Note:
                        When "is_local_order" is set to 'True', "data_order_column" should be
                        specified, and the columns specified in "data_order_column"
                        are used for local ordering.
                nulls_first:
                    Optional Argument.
                    Specifies a boolean value to determine whether NULLS are listed
                    first or last during ordering.
                    This argument is ignored, if "data_order_column" is None.
                    NULLS are listed first when this argument is set to 'True', and
                    last when set to 'False'.
                    Default value: True
                    Types: bool
                sort_ascending:
                    Optional Argument.
                    Specifies a boolean value to determine if the result set is to
                    be sorted on the "data_order_column" column in ascending or
                    descending order.
                    The sorting is ascending when this argument is set to 'True',
                    and descending when set to 'False'.
                    This argument is ignored, if "data_order_column" is None.
                    Default Value: True
                    Types: bool
                style:
                    Optional Argument.
                    Specifies how input is passed to and output is generated by the 'apply_command'
                    respectively.
                    Note:
                        This clause only supports 'csv' value for Apply.
                    Default value: "csv"
                    Types: str
                debug:
                    Optional Argument.
                    Specifies whether to display the script file path generated during function execution or not. This
                    argument helps in debugging when there are any failures during function execution. When set
                    to True, function displays the path of the script and does not remove the file from local file system.
                    Otherwise, file is removed from the local file system.
                    Default Value: False
                    Types: bool
            RETURNS:
                teradataml DataFrame.
            RAISES:
                 TypeError, TeradataMlException.
            EXAMPLES:
                # Create a Python 3.7.9 environment with given name and description in Vantage.
                >>> env = create_env('testenv', 'python_3.7.9', 'Test environment')
                User environment testenv created.
                # Packages dill and pandas must be installed in remote user environment.
                >>> env.install_lib(['pandas','dill'])
                Request to install libraries initiated successfully in the remote user environment demo_env. Check the status using
    1be2-4d4a-9d47-12cd4365a003'.
                # Check the status of installation.
                >>> env.status('ef255030-1be2-4d4a-9d47-12cd4365a003')
                                               Claim Id     File/Libs  Method Name     Stage             Timestamp Additional Detai
                0  ef255030-1be2-4d4a-9d47-12cd4365a003  pandas, dill  install_lib   Started  2022-08-04T04:27:56Z
                1  ef255030-1be2-4d4a-9d47-12cd4365a003  pandas, dill  install_lib  Finished  2022-08-04T04:29:12Z
                >>>
                # This example uses the 'admissions_train' dataset, to increase
                # the 'gpa' by a give percentage.
                # Load the example data.
                >>> load_example_data("dataframe", "admissions_train")
                >>> df = DataFrame('admissions_train')
                >>> print(df)
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
                # Example 1:
                # Create the user defined function to increase the 'gpa' by the
                # percentage provided. Note that the input to and the output
                # from the function is a Pandas Series object.
                >>> def increase_gpa(row, p=20):
                ...     row['gpa'] = row['gpa'] + row['gpa'] * p/100
                ...     return row
                ...
                >>>
                # Apply the user defined function to the DataFrame.
                # Note that since the output of the user defined function
                # expects the same columns with the same types, we can skip
                # passing the 'returns' argument.
                >>> increase_gpa_20 = df.apply(increase_gpa, env_name='testenv')
                >>>
                >>> # Print the result.
                >>> print(increase_gpa_20)
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
                26     yes  4.284  Advanced    Advanced         1
                19     yes  2.376  Advanced    Advanced         0
                # Example 2:
                # Use the same user defined function with a lambda notation to
                # pass the percentage, 'p = 40'.
                >>> increase_gpa_40 = df.apply(lambda row: increase_gpa(row,
                ...                                                     p = 40),
                ...                            env_name='testenv')
                >>>
                >>> print(increase_gpa_40)
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
                # Example 3:
                # Use the same user defined function with functools.partial to
                # pass the percentage, 'p = 50'.
                >>> from functools import partial
                >>> increase_gpa_50 = df.apply(partial(increase_gpa, p = 50),
                ...                            env_name='testenv')
                >>>
                >>> print(increase_gpa_50)
                   masters    gpa     stats programming  admitted
                id
                13      no  6.000  Advanced      Novice         1
                26     yes  5.355  Advanced    Advanced         1
                5       no  5.160    Novice      Novice         0
                19     yes  2.970  Advanced    Advanced         0
                15     yes  6.000  Advanced    Advanced         1
                40     yes  5.925    Novice    Beginner         0
                7      yes  3.495    Novice      Novice         1
                22     yes  5.190    Novice    Beginner         0
                36      no  4.500  Advanced      Novice         0
                38     yes  3.975  Advanced    Beginner         1
                # Example 4:
                # Use a lambda function to double 'gpa', and
                # return numpy ndarray.
                >>> from numpy import asarray
                >>> inc_gpa_lambda = lambda row, p=20: asarray([row['id'],
                ...                                row['masters'],
                ...                                row['gpa'] + row['gpa'] * p/100,
                ...                                row['stats'],
                ...                                row['programming'],
                ...                                row['admitted']])
                >>> increase_gpa_100 = df.apply(lambda row: inc_gpa_lambda(row,
                ...                                                        p=100),
                ...                             env_name='testenv')
                >>>
                >>> print(increase_gpa_100)
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
    assign
    teradataml.dataframe.dataframe.DataFrame.assign = assign(self, drop_columns=False, **kwargs)
    DESCRIPTION:
        Assign new columns to a teradataml DataFrame.
    PARAMETERS:
        drop_columns:
            Optional Argument.
            If True, drop columns that are not specified in assign.
            Notes:
                1. When DataFrame.assign() is run on DataFrame.groupby(), this argument
                   is ignored. In such cases, all columns are dropped and only new columns
                   and grouping columns are returned.
                2. Argument is ignored for UDF functions.
            Default Value: False
            Types: bool
        kwargs:
            Specifies keyword-value pairs.
            - keywords are the column names.
            - values can be:
                * Column arithmetic expressions.
                * int/float/string literals.
                * DataFrameColumn a.k.a. ColumnExpression Functions.
                  (Seee DataFrameColumn Functions in Function reference guide for more
                   details)
                * SQLAlchemy ClauseElements.
                  (See teradataml extension with SQLAlchemy in teradataml User Guide
                   and Function reference guide for more details)
                * Function - udf, call_udf.
    RETURNS:
        teradataml DataFrame
        A new DataFrame is returned with:
            1. New columns in addition to all the existing columns if "drop_columns" is False.
            2. Only new columns if "drop_columns" is True.
            3. New columns in addition to group by columns, i.e., columns used for grouping,
               if assign() is run on DataFrame.groupby().
    NOTES:
         1. The values in kwargs cannot be callable (functions).
         2. The original DataFrame is not modified.
         3. Since ``kwargs`` is a dictionary, the order of your
           arguments may not be preserved. To make things predictable,
           the columns are inserted in alphabetical order, after the existing columns
           in the DataFrame. Assigning multiple columns within the same ``assign`` is
           possible, but you cannot reference other columns created within the same
           ``assign`` call.
         4. The maximum number of columns that a DataFrame can have is 2048.
         5. With DataFrame.groupby(), only aggregate functions and literal values
           are advised to use. Other functions, such as string functions, can also be
           used, but the column used in such function must be a part of group by columns.
           See examples for teradataml extension with SQLAlchemy on using various
           functions with DataFrame.assign().
         6. UDF expressions can run on both Vantage Cloud Lake leveraging Apply Table Operator
           of Open Analytics Framework and Enterprise leveraging Vantage's Script Table Operator.
         7. One can pass both regular expressions and udf expressions to this API.
           However, regular expressions are computed first followed by udf expressions.
           Hence the order of columns also maintained in same order.
           Look at Example 18 to understand more.
         8. While passing multiple udf expressions, one can not pass one column output
           as another column input in the same ``assign`` call.
         9. If user pass multiple udf expressions, delimiter and quotechar specified in 
           last udf expression are considered for processing.
    RAISES:
         1. ValueError - When a callable is passed as a value, or columns from different
                         DataFrames are passed as values in kwargs.
         2. TeradataMlException - When the return DataFrame initialization fails, or
                                  invalid argument types are passed.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> c1 = df.gpa
        >>> c2 = df.id
        >>>
        #
        # Executing assign() with Arithmetic operations on columns.
        # All below examples use columns "gpa" and "id" for
        # arithmetic operations to create a new DataFrame including the new columns.
        #
        # Let's take a look at various operations that can be performed
        # using assign() and arithmetic operations on columns.
        # Example 1: Addition of two columns "gpa" and "id".
        >>> df.assign(new_column = c1 + c2).sort("id")
           masters   gpa     stats programming admitted  new_column
        id
        1      yes  3.95  Beginner    Beginner        0        4.95
        2      yes  3.76  Beginner    Beginner        0        5.76
        3       no  3.70    Novice    Beginner        1        6.70
        4      yes  3.50  Beginner      Novice        1        7.50
        5       no  3.44    Novice      Novice        0        8.44
        6      yes  3.50  Beginner    Advanced        1        9.50
        7      yes  2.33    Novice      Novice        1        9.33
        8       no  3.60  Beginner    Advanced        1       11.60
        9       no  3.82  Advanced    Advanced        1       12.82
        10      no  3.71  Advanced    Advanced        1       13.71
        >>>
        # Example 2: Multiplication of columns "gpa" and "id".
        >>> df.assign(new_column = c1 * c2).sort("id")
           masters   gpa     stats programming admitted  new_column
        id
        1      yes  3.95  Beginner    Beginner        0        3.95
        2      yes  3.76  Beginner    Beginner        0        7.52
        3       no  3.70    Novice    Beginner        1       11.10
        4      yes  3.50  Beginner      Novice        1       14.00
        5       no  3.44    Novice      Novice        0       17.20
        6      yes  3.50  Beginner    Advanced        1       21.00
        7      yes  2.33    Novice      Novice        1       16.31
        8       no  3.60  Beginner    Advanced        1       28.80
        9       no  3.82  Advanced    Advanced        1       34.38
        10      no  3.71  Advanced    Advanced        1       37.10
        >>>
        # Example 3: Division of columns. Divide "id" by "gpa".
        >>> df.assign(new_column = c2 / c1).sort("id")
           masters   gpa     stats programming admitted  new_column
        id
        1      yes  3.95  Beginner    Beginner        0    0.253165
        2      yes  3.76  Beginner    Beginner        0    0.531915
        3       no  3.70    Novice    Beginner        1    0.810811
        4      yes  3.50  Beginner      Novice        1    1.142857
        5       no  3.44    Novice      Novice        0    1.453488
        6      yes  3.50  Beginner    Advanced        1    1.714286
        7      yes  2.33    Novice      Novice        1    3.004292
        8       no  3.60  Beginner    Advanced        1    2.222222
        9       no  3.82  Advanced    Advanced        1    2.356021
        10      no  3.71  Advanced    Advanced        1    2.695418
        >>>
        # Example 4: Subtract values in column "id" from "gpa".
        >>> df.assign(new_column = c1 - c2).sort("id")
           masters   gpa     stats programming admitted  new_column
        id
        1      yes  3.95  Beginner    Beginner        0        2.95
        2      yes  3.76  Beginner    Beginner        0        1.76
        3       no  3.70    Novice    Beginner        1        0.70
        4      yes  3.50  Beginner      Novice        1       -0.50
        5       no  3.44    Novice      Novice        0       -1.56
        6      yes  3.50  Beginner    Advanced        1       -2.50
        7      yes  2.33    Novice      Novice        1       -4.67
        8       no  3.60  Beginner    Advanced        1       -4.40
        9       no  3.82  Advanced    Advanced        1       -5.18
        10      no  3.71  Advanced    Advanced        1       -6.29
        # Example 5: Modulo division of values in column "id" and "gpa".
        >>> df.assign(new_column = c2 % c1).sort("id")
           masters   gpa     stats programming admitted  new_column
        id
        1      yes  3.95  Beginner    Beginner        0        1.00
        2      yes  3.76  Beginner    Beginner        0        2.00
        3       no  3.70    Novice    Beginner        1        3.00
        4      yes  3.50  Beginner      Novice        1        0.50
        5       no  3.44    Novice      Novice        0        1.56
        6      yes  3.50  Beginner    Advanced        1        2.50
        7      yes  2.33    Novice      Novice        1        0.01
        8       no  3.60  Beginner    Advanced        1        0.80
        9       no  3.82  Advanced    Advanced        1        1.36
        10      no  3.71  Advanced    Advanced        1        2.58
        >>>
        #
        # Executing assign() with literal values.
        #
        # Example 6: Adding an integer literal value to the values of columns "gpa" and "id".
        >>> df.assign(c3 = c1 + 1, c4 = c2 + 1).sort("id")
           masters   gpa     stats programming admitted    c3  c4
        id
        1      yes  3.95  Beginner    Beginner        0  4.95   2
        2      yes  3.76  Beginner    Beginner        0  4.76   3
        3       no  3.70    Novice    Beginner        1  4.70   4
        4      yes  3.50  Beginner      Novice        1  4.50   5
        5       no  3.44    Novice      Novice        0  4.44   6
        6      yes  3.50  Beginner    Advanced        1  4.50   7
        7      yes  2.33    Novice      Novice        1  3.33   8
        8       no  3.60  Beginner    Advanced        1  4.60   9
        9       no  3.82  Advanced    Advanced        1  4.82  10
        10      no  3.71  Advanced    Advanced        1  4.71  11
        >>>
        # Example 7: Create a new column with an integer literal value.
        >>> df.assign(c1 = 1).sort("id")
           masters   gpa     stats programming admitted c1
        id
        1      yes  3.95  Beginner    Beginner        0  1
        2      yes  3.76  Beginner    Beginner        0  1
        3       no  3.70    Novice    Beginner        1  1
        4      yes  3.50  Beginner      Novice        1  1
        5       no  3.44    Novice      Novice        0  1
        6      yes  3.50  Beginner    Advanced        1  1
        7      yes  2.33    Novice      Novice        1  1
        8       no  3.60  Beginner    Advanced        1  1
        9       no  3.82  Advanced    Advanced        1  1
        10      no  3.71  Advanced    Advanced        1  1
        >>>
        # Example 8: Create a new column with a string literal value.
        >>> df.assign(c3 = 'string').sort("id")
           masters   gpa     stats programming admitted      c3
        id
        1      yes  3.95  Beginner    Beginner        0  string
        2      yes  3.76  Beginner    Beginner        0  string
        3       no  3.70    Novice    Beginner        1  string
        4      yes  3.50  Beginner      Novice        1  string
        5       no  3.44    Novice      Novice        0  string
        6      yes  3.50  Beginner    Advanced        1  string
        7      yes  2.33    Novice      Novice        1  string
        8       no  3.60  Beginner    Advanced        1  string
        9       no  3.82  Advanced    Advanced        1  string
        10      no  3.71  Advanced    Advanced        1  string
        >>>
        # Example 9: Concatenation of strings, a string literal and value from
        #            "masters" column.
        #            '+' operator is overridden for string columns.
        >>> df.assign(concatenated = "Completed? " + df.masters).sort("id")
           masters   gpa     stats programming admitted    concatenated
        id
        1      yes  3.95  Beginner    Beginner        0  Completed? yes
        2      yes  3.76  Beginner    Beginner        0  Completed? yes
        3       no  3.70    Novice    Beginner        1   Completed? no
        4      yes  3.50  Beginner      Novice        1  Completed? yes
        5       no  3.44    Novice      Novice        0   Completed? no
        6      yes  3.50  Beginner    Advanced        1  Completed? yes
        7      yes  2.33    Novice      Novice        1  Completed? yes
        8       no  3.60  Beginner    Advanced        1   Completed? no
        9       no  3.82  Advanced    Advanced        1   Completed? no
        10      no  3.71  Advanced    Advanced        1   Completed? no
        >>>
        #
        # Significance of "drop_columns" in assign().
        # Setting drop_columns to True will only return assigned expressions.
        #
        # Example 10: Drop all column and return new assigned expressions.
        >>> df.assign(drop_columns = True,
        ...           addc = c1 + c2,
        ...           subc = c1 - c2,
        ...           mulc = c1 * c2,
        ...           divc = c1/c2).sort("addc")
            addc      divc   mulc  subc
        0   4.95  3.950000   3.95  2.95
        1   5.76  1.880000   7.52  1.76
        2   6.70  1.233333  11.10  0.70
        3   7.50  0.875000  14.00 -0.50
        4   8.44  0.688000  17.20 -1.56
        5   9.33  0.332857  16.31 -4.67
        6   9.50  0.583333  21.00 -2.50
        7  11.60  0.450000  28.80 -4.40
        8  12.82  0.424444  34.38 -5.18
        9  13.71  0.371000  37.10 -6.29
        >>>
        # Example 11: Duplicate a column with a new name.
        #             In the example here, we are duplicating:
        #               1. Column "id" to new column "c1".
        #               2. Column "gpa" to new column "c2".
        >>> df.assign(c1 = c2, c2 = c1).sort("id")
           masters   gpa     stats programming admitted  c1    c2
        id
        1      yes  3.95  Beginner    Beginner        0   1  3.95
        2      yes  3.76  Beginner    Beginner        0   2  3.76
        3       no  3.70    Novice    Beginner        1   3  3.70
        4      yes  3.50  Beginner      Novice        1   4  3.50
        5       no  3.44    Novice      Novice        0   5  3.44
        6      yes  3.50  Beginner    Advanced        1   6  3.50
        7      yes  2.33    Novice      Novice        1   7  2.33
        8       no  3.60  Beginner    Advanced        1   8  3.60
        9       no  3.82  Advanced    Advanced        1   9  3.82
        10      no  3.71  Advanced    Advanced        1  10  3.71
        >>>
        # Example 12: Renaming columns.
        #             Example 6 command can be modified a bit to rename columns, rather than
        #             duplicating it.
        #             Use "drop_column=True" in example 6 command to select all the desired columns.
        >>> df.assign(drop_columns=True, c1 = c2, c2 = c1,
        ...           masters=df.masters,
        ...           stats=df.stats,
        ...           programming=df.programming,
        ...           admitted=df.admitted).sort("c1")
          masters     stats programming  admitted  c1    c2
        0     yes  Beginner    Beginner         0   1  3.95
        1     yes  Beginner    Beginner         0   2  3.76
        2      no    Novice    Beginner         1   3  3.70
        3     yes  Beginner      Novice         1   4  3.50
        4      no    Novice      Novice         0   5  3.44
        5     yes  Beginner    Advanced         1   6  3.50
        6     yes    Novice      Novice         1   7  2.33
        7      no  Beginner    Advanced         1   8  3.60
        8      no  Advanced    Advanced         1   9  3.82
        9      no  Advanced    Advanced         1  10  3.71
        >>>
        #
        # Executing Aggregate Functions with assign() and DataFrame.groupby().
        #
        # Here, we will be using 'func' from sqlalchemy to run some aggregate functions.
        >>> from sqlalchemy import func
        >>>
        # Example 13: Calculate average "gpa" for values in the "stats" column.
        >>> df.groupby("stats").assign(res=func.ave(df.gpa.expression))
              stats       res
        0  Beginner  3.662000
        1  Advanced  3.508750
        2    Novice  3.559091
        >>>
        # Example 14: Calculate standard deviation, kurtosis value and sum of values in
        #             the "gpa" column with values grouped by values in the "stats" column.
        #             Alternate approach for DataFrame.agg(). This allows user to name the
        #             result columns.
        >>> df.groupby("stats").assign(gpa_std_=func.ave(df.gpa.expression),
        ...                            gpa_kurtosis_=func.kurtosis(df.gpa.expression),
        ...                            gpa_sum_=func.sum(df.gpa.expression))
              stats  gpa_kurtosis_  gpa_std_  gpa_sum_
        0  Beginner      -0.452859  3.662000     18.31
        1  Advanced       2.886226  3.508750     84.21
        2    Novice       6.377775  3.559091     39.15
        >>>
        #
        # Executing user defined function (UDF) with assign()
        #
        # Example 15: Create two user defined functions to 'to_upper' and 'sum',
        #            'to_upper' to get the values in 'accounts' to upper case and 
        #            'sum' to add length of string values in column 'accounts' 
        #             with column 'Feb' and store the result in Integer type column.
        >>> @udf
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        >>> from teradatasqlalchemy.types import INTEGER
        >>> @udf(returns=INTEGER()) 
        ... def sum(x, y):
        ...     return len(x)+y
        >>>
        # Assign both Column Expressions returned by user defined functions
        # to the DataFrame.
        >>> res = df.assign(upper_stats = to_upper('accounts'), len_sum = sum('accounts', 'Feb'))
        >>> res
                    Feb    Jan    Mar    Apr  datetime upper_stats  len_sum
        accounts                                                             
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC       98
        Red Inc     200.0  150.0  140.0    NaN  17/01/04     RED INC      207
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC      100
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC      209
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  ORANGE INC      220
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04    ALPHA CO      218
        >>>
        # Example 16: Create a user defined function to add 4 to the 'datetime' column
        #             and store the result in DATE type column.
        >>> from teradatasqlalchemy.types import DATE
        >>> import datetime
        >>> @udf(returns=DATE())
        ... def add_date(x, y):
        ...     return (datetime.datetime.strptime(x, "%y/%m/%d")+datetime.timedelta(y)).strftime("%y/%m/%d")
        >>>
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> res = df.assign(new_date = add_date('datetime', 4))
        >>> res
                        Feb    Jan    Mar    Apr  datetime  new_date
        accounts                                                  
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04  17/01/08
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04  17/01/08
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04  17/01/08
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  17/01/08
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  17/01/08
        Red Inc     200.0  150.0  140.0    NaN  17/01/04  17/01/08
        >>>
        # Example 17: Create a user defined functions to 'to_upper' to get
        #             the values in 'accounts' to upper case and create a
        #             new column with a string literal value.
        >>> @udf
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        # Assign both expressions to the DataFrame.
        >>> res = df.assign(upper_stats = to_upper('accounts'), new_col = 'string')
        >>> res
                      Feb    Jan    Mar    Apr  datetime new_col upper_stats
        accounts                                                            
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04  string    ALPHA CO
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04  string    BLUE INC
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  string  YELLOW INC
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04  string   JONES LLC
        Red Inc     200.0  150.0  140.0    NaN  17/01/04  string     RED INC
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  string  ORANGE INC
        >>>
        # Example 18: Create two user defined functions to 'to_upper' and 'sum'
        #             and create new columns with string literal value and
        #             arithmetic operation on column 'Feb'.
        >>> @udf
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        >>> from teradatasqlalchemy.types import INTEGER
        >>> @udf(returns=INTEGER()) 
        ... def sum(x, y):
        ...     return len(x)+y
        >>>
        # Assign all expressions to the DataFrame.
        >>> res = df.assign(upper_stats = to_upper('accounts'),new_col = 'abc', 
        ...                 len_sum = sum('accounts', 'Feb'), col_sum = df.Feb+1)
        >>> res
                      Feb    Jan    Mar    Apr  datetime  col_sum new_col upper_stats  len_sum
        accounts                                                                              
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04     91.0     abc    BLUE INC       98
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04    211.0     abc    ALPHA CO      218
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04    201.0     abc   JONES LLC      209
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04     91.0     abc  YELLOW INC      100
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04    211.0     abc  ORANGE INC      220
        Red Inc     200.0  150.0  140.0    NaN  17/01/04    201.0     abc     RED INC      207
        >>>
        # Example 19: Convert the values is 'accounts' column to upper case using a user 
        #             defined function on Vantage Cloud Lake.
        # Create a Python 3.10.5 environment with given name and description in Vantage.
        >>> env = create_env('test_udf', 'python_3.10.5', 'Test environment for UDF')
        User environment 'test_udf' created.
        >>>
        # Create a user defined functions to 'to_upper' to get the values in upper case 
        # and pass the user env to run it on.
        >>> from teradataml.dataframe.functions import udf
        >>> @udf(env_name = env)
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> df.assign(upper_stats = to_upper('accounts'))
                      Feb    Jan    Mar    Apr  datetime upper_stats
        accounts                                                    
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04    ALPHA CO
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  ORANGE INC
        Red Inc     200.0  150.0  140.0    NaN  17/01/04     RED INC
        >>>
        # Example 20: Register and Call the user defined function to get the values upper case.
        >>> from teradataml.dataframe.functions import udf, register, call_udf
        >>> @udf
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        # Register the created user defined function with name "upper".
        >>> register("upper", to_upper)
        >>>
        # Call the user defined function registered with name "upper" and assign the 
        # ColumnExpression returned to the DataFrame.
        >>> res = df.assign(upper_col = call_udf("upper", ('accounts',)))
        >>> res
                      Feb    Jan    Mar    Apr  datetime   upper_col
        accounts                                                    
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04    ALPHA CO
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  ORANGE INC
        Red Inc     200.0  150.0  140.0    NaN  17/01/04     RED INC
        >>>
    columns
    teradataml.dataframe.dataframe.DataFrame.columns
    RETURNS:
        a list containing the column names
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> df.columns
        ['accounts', 'Feb', 'Jan', 'Mar', 'Apr', 'datetime']
    concat
    teradataml.dataframe.dataframe.DataFrame.concat = concat(self, other, join='OUTER', allow_duplicates=True, sort=False, ignore_index=False)
    DESCRIPTION:
        Concatenates two teradataml DataFrames along the index axis.
    PARAMETERS:
        other:
            Required Argument.
            Specifies the other teradataml DataFrame with which the concatenation is
            to be performed.
            Types: teradataml DataFrame
        join:
            Optional Argument.
            Specifies how to handle indexes on columns axis.
            Supported values are:
                * 'OUTER': It instructs the function to project all columns from both
                           the DataFrames. Columns not present in either DataFrame will
                           have a SQL NULL value.
                * 'INNER': It instructs the function to project only the columns common
                           to both DataFrames.
            Default value: 'OUTER'
            Permitted values: 'INNER', 'OUTER'
            Types: str
        allow_duplicates:
            Optional Argument.
            Specifies if the result of concatenation can have duplicate rows.
            Default value: True
            Types: bool
        sort:
            Optional Argument.
            Specifies a flag to sort the columns axis if it is not already aligned when the join argument is set to 'outer'.
            Default value: False
            Types: bool
        ignore_index:
            Optional argument.
            Specifies whether to ignore the index columns in resulting DataFrame or not.
            If True, then index columns will be ignored in the concat operation.
            Default value: False
            Types: bool
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> from teradataml import load_example_data
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        >>> # Default options
        >>> df = DataFrame('admissions_train')
        >>> df1 = df[df.gpa == 4].select(['id', 'stats', 'masters', 'gpa'])
        >>> df1
               stats masters  gpa
        id
        13  Advanced      no  4.0
        29    Novice     yes  4.0
        15  Advanced     yes  4.0
        >>> df2 = df[df.gpa < 2].select(['id', 'stats', 'programming', 'admitted'])
        >>> df2
               stats programming admitted
        id
        24  Advanced      Novice        1
        19  Advanced    Advanced        0
        >>>
        >>> cdf = df1.concat(df2)
        >>> cdf
               stats masters  gpa programming admitted
        id
        19  Advanced    None  NaN    Advanced        0
        24  Advanced    None  NaN      Novice        1
        13  Advanced      no  4.0        None     None
        29    Novice     yes  4.0        None     None
        15  Advanced     yes  4.0        None     None
        >>>
        >>> # join = 'inner'
        >>> cdf = df1.concat(df2, join='inner')
        >>> cdf
               stats
        id
        19  Advanced
        24  Advanced
        13  Advanced
        29    Novice
        15  Advanced
        >>>
        >>> # allow_duplicates = True (default)
        >>> cdf = df1.concat(df2)
        >>> cdf
               stats masters  gpa programming admitted
        id
        19  Advanced    None  NaN    Advanced        0
        24  Advanced    None  NaN      Novice        1
        13  Advanced      no  4.0        None     None
        29    Novice     yes  4.0        None     None
        15  Advanced     yes  4.0        None     None
        >>> cdf = cdf.concat(df2)
        >>> cdf
               stats masters  gpa programming admitted
        id
        19  Advanced    None  NaN    Advanced        0
        13  Advanced      no  4.0        None     None
        24  Advanced    None  NaN      Novice        1
        24  Advanced    None  NaN      Novice        1
        19  Advanced    None  NaN    Advanced        0
        29    Novice     yes  4.0        None     None
        15  Advanced     yes  4.0        None     None
        >>>
        >>> # allow_duplicates = False
        >>> cdf = cdf.concat(df2, allow_duplicates=False)
        >>> cdf
               stats masters  gpa programming admitted
        id
        19  Advanced    None  NaN    Advanced        0
        29    Novice     yes  4.0        None     None
        24  Advanced    None  NaN      Novice        1
        15  Advanced     yes  4.0        None     None
        13  Advanced      no  4.0        None     None
        >>>
        >>> # sort = True
        >>> cdf = df1.concat(df2, sort=True)
        >>> cdf
           admitted  gpa masters programming     stats
        id
        19        0  NaN    None    Advanced  Advanced
        24        1  NaN    None      Novice  Advanced
        13     None  4.0      no        None  Advanced
        29     None  4.0     yes        None    Novice
        15     None  4.0     yes        None  Advanced
        >>> 
        >>> # ignore_index = True
        >>> cdf = df1.concat(df2, ignore_index=True)
        >>> cdf
              stats masters  gpa programming  admitted
        0  Advanced     yes  4.0        None       NaN
        1  Advanced    None  NaN    Advanced       0.0
        2    Novice     yes  4.0        None       NaN
        3  Advanced    None  NaN      Novice       1.0
        4  Advanced      no  4.0        None       NaN
    cube
    teradataml.dataframe.dataframe.DataFrame.cube = cube(self, columns, include_grouping_columns=False)
    DESCRIPTION:
        cube() function creates a multi-dimensional cube for the DataFrame
        using the specified column(s), and there by running aggregates on
        it to produce the aggregations on different dimensions.
    PARAMETERS:
        columns:
            Required Argument.
            Specifies the name(s) of input teradataml DataFrame column(s).
            Types: str OR list of str(s)
        include_grouping_columns:
            Optional Argument.
            Specifies whether to include aggregations on the grouping column(s) or not.
            When set to True, the resultant DataFrame will have the aggregations on the 
            columns mentioned in "columns". Otherwise, resultant DataFrame will not have 
            aggregations on the columns mentioned in "columns".
            Default Value: False
            Types: bool
    RETURNS:
        teradataml DataFrameGroupBy
    RAISES:
        TeradataMlException
    EXAMPLES :
        # Load the data to run the example.
        >>> load_example_data("dataframe","admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        26     yes  3.57  Advanced    Advanced         1
        17      no  3.83  Advanced    Advanced         1
        # Example 1: Find the sum of all valid columns by grouping the
        #            DataFrame columns with 'masters' and 'stats'.
        >>> df1 = df.cube(["masters", "stats"]).sum()
        >>> df1
          masters     stats  sum_id  sum_gpa  sum_admitted
        0      no  Beginner       8     3.60             1
        1    None  Advanced     555    84.21            16
        2    None  Beginner      21    18.31             3
        3     yes  Beginner      13    14.71             2
        4    None      None     820   141.67            26
        5     yes  Advanced     366    49.26             7
        6      no      None     343    63.96            16
        7    None    Novice     244    39.15             7
        8      no  Advanced     189    34.95             9
        9     yes    Novice      98    13.74             1
        # Example 2: Find the avg of all valid columns by grouping the DataFrame
        #            with columns 'masters' and 'admitted'. Include grouping columns
        #            in aggregate function 'avg'.
        >>> df1 = df.cube(["masters", "admitted"], include_grouping_columns=True).avg()
        >>> df1
          masters  admitted     avg_id   avg_gpa  avg_admitted
        0     yes       NaN  21.681818  3.532273      0.454545
        1    None       1.0  18.846154  3.533462      1.000000
        2      no       NaN  19.055556  3.553333      0.888889
        3     yes       0.0  24.083333  3.613333      0.000000
        4    None       NaN  20.500000  3.541750      0.650000
        5    None       0.0  23.571429  3.557143      0.000000
        6     yes       1.0  18.800000  3.435000      1.000000
        7      no       1.0  18.875000  3.595000      1.000000
        8      no       0.0  20.500000  3.220000      0.000000
        # Example 3: Find the avg of all valid columns by grouping the DataFrame with
        #            columns 'masters' and 'admitted'. Do not include grouping columns
        #            in aggregate function 'avg'. 
        >>> df1 = df.cube(["masters", "admitted"], include_grouping_columns=False).avg()
        >>> df1
          masters  admitted     avg_id   avg_gpa
        0      no       0.0  20.500000  3.220000
        1    None       1.0  18.846154  3.533462
        2      no       NaN  19.055556  3.553333
        3     yes       0.0  24.083333  3.613333
        4    None       NaN  20.500000  3.541750
        5    None       0.0  23.571429  3.557143
        6     yes       1.0  18.800000  3.435000
        7     yes       NaN  21.681818  3.532273
        8      no       1.0  18.875000  3.595000
    db_object_name
    teradataml.dataframe.dataframe.DataFrame.db_object_name
    DESCRIPTION:
        Get the underlying database object name, on which DataFrame is
        created.
    RETURNS:
        str representing object name of DataFrame
    EXAMPLES:
        >>> load_example_data("dataframe", "sales")
        >>> df = DataFrame('sales')
        >>> df.db_object_name
        '"sales"'
    describe
    teradataml.dataframe.dataframe.DataFrame.describe = describe(self, percentiles=[0.25, 0.5, 0.75], verbose=False, distinct=False, statistics=None, columns=None,
    pivot=False)
    DESCRIPTION:
        Generates statistics for numeric columns. This function can be used in two modes:
            1. Regular Aggregate Mode.
                It computes the count, mean, std, min, percentiles, and max for numeric columns.
                Default statistics include:
                    "count", "mean", "std", "min", "percentile", "max"
            2. Time Series Aggregate Mode.
                It computes max, mean, min, std, median, mode, and percentiles for numeric columns.
                Default statistics include:
                    'max', 'mean', 'min', 'std'
        Note:
            Regular Aggregate Mode: If describe() is used on the output of any DataFrame API or groupby(),
                                    then describe() is used as regular aggregation.
            Time Series Aggregate Mode: If describe() is used on the output of groupby_time(), then describe()
                                        is a time series aggregate, where time series aggregates are used
                                        to calculate the statistics.
    PARAMETERS:
        percentiles:
            Optional Argument.
            A list of values between 0 and 1. Applicable for both modes.
            By default, percentiles are calculated for statistics for 'Regular Aggregate Mode', whereas
            for 'Time Series Aggregate Mode', percentiles are calculated when verbose is set to True.
            Default Values: [.25, .5, .75], which returns the 25th, 50th, and 75th percentiles.
            Types: float or List of floats
        verbose:
            Optional Argument.
            Specifies a boolean value to be used for time series aggregation, stating whether to get
            verbose output or not.
            When this argument is set to 'True', function calculates median, mode, and percentile values
            on top of its default statistics.
            Note:
                verbose as 'True' is not applicable for 'Regular Aggregate Mode'.
            Default Values: False
            Types: bool
        distinct:
            Optional Argument.
            Specifies a boolean value to decide whether to consider duplicate rows in statistic
            calculation or not. By default, duplicate values are considered for statistic calculation.
            When this is set to True, only distinct rows are considered for statistic calculation.
            Default Values: False
            Types: bool
        statistics:
            Optional Argument.
            Specifies the aggregate operation to be performed.
            Computes count, mean, std, min, percentiles, and max for numeric columns.
            Computes count and unique for non-numeric columns.
            Notes:
                1. statistics is not applicable for 'Time Series Aggregate Mode'.
            Permitted Values: count, mean, min, max, unique, std, describe, percentile
            Default Values: None
            Types: str or List of str
        columns:
            Optional Argument.
            Specifies the name(s) of the columns we are collecting statistics for.
            Default Values: None
            Types: str or List of str
        pivot:
            Optional Argument.
            Specifies a boolean value to pivot the output.
            Note:
                * "pivot" is not supported for PTI tables.
            Default Values: 'False'
            Types: bool
    RETURNS:
        teradataml DataFrame
    RAISE:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame('sales')
        >>> print(df)
                      Feb   Jan   Mar   Apr    datetime
        accounts
        Blue Inc     90.0    50    95   101  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        Jones LLC   200.0   150   140   180  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        # Computes count, mean, std, min, percentiles, and max for numeric columns.
        >>> df.describe(pivot=True)
                  Apr      Feb     Mar     Jan
        func
        count       4        6       4       4
        mean   195.25  166.667   147.5   137.5
        std    70.971   59.554  49.749  62.915
        min       101       90      95      50
        25%    160.25    117.5  128.75     125
        50%       215      200     140     150
        75%       250    207.5  158.75   162.5
        max       250      210     215     200
        # Computes count, mean, std, min, percentiles, and max for numeric columns with 
        # default arugments.
        >>> df.describe()
        ATTRIBUTE   StatName            StatValue
        Jan         MAXIMUM             200.0
        Jan         STANDARD DEVIATION      62.91528696058958
        Jan         PERCENTILES(25)     125.0
        Jan         PERCENTILES(50)     150.0
        Mar         COUNT               4.0
        Mar         MINIMUM             95.0
        Mar         MAXIMUM             215.0
        Mar         MEAN                147.5
        Mar         STANDARD DEVIATION      49.749371855331
        Mar         PERCENTILES(25)     128.75
        Mar         PERCENTILES(50)     140.0
        Apr         COUNT               4.0
        Apr         MINIMUM             101.0
        Apr         MAXIMUM             250.0
        Apr         MEAN                195.25
        Apr         STANDARD DEVIATION      70.97123830585646
        Apr         PERCENTILES(25)     160.25
        Apr         PERCENTILES(50)     215.0
        Apr         PERCENTILES(75)     250.0
        Feb         COUNT               6.0
        Feb         MINIMUM             90.0
        Feb         MAXIMUM             210.0
        Feb         MEAN                166.66666666666666
        Feb         STANDARD DEVIATION      59.553897157672786
        Feb         PERCENTILES(25)     117.5
        Feb         PERCENTILES(50)     200.0
        Feb         PERCENTILES(75)     207.5
        Mar         PERCENTILES(75)     158.75
        Jan         PERCENTILES(75)     162.5
        Jan         MEAN                137.5
        Jan         MINIMUM             50.0
        Jan         COUNT               4.0
        # Computes count, mean, std, min, percentiles, and max for numeric columns with 30th and 60th percentiles.
        >>> df.describe(percentiles=[.3, .6], pivot=True)
                  Apr      Feb     Mar     Jan
        func
        count       4        6       4       4
        mean   195.25  166.667   147.5   137.5
        std    70.971   59.554  49.749  62.915
        min       101       90      95      50
        30%     172.1      145   135.5     140
        60%       236      200     140     150
        max       250      210     215     200
        # Computes count, mean, std, min, percentiles, and max for numeric columns group by "datetime" and "Feb".
        >>> df1 = df.groupby(["datetime", "Feb"])
        >>> df1.describe(pivot=True)
                                 Jan   Mar   Apr
        datetime   Feb   func
        04/01/2017 90.0  25%      50    95   101
                         50%      50    95   101
                         75%      50    95   101
                         count     1     1     1
                         max      50    95   101
                         mean     50    95   101
                         min      50    95   101
                         std    None  None  None
                   200.0 25%     150   140   180
                         50%     150   140   180
                         75%     150   140   180
                         count     2     2     1
                         max     150   140   180
                         mean    150   140   180
                         min     150   140   180
                         std       0     0  None
                   210.0 25%     200   215   250
                         50%     200   215   250
                         75%     200   215   250
                         count     1     1     2
                         max     200   215   250
                         mean    200   215   250
                         min     200   215   250
                         std    None  None     0
        # Examples for describe() function as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        >>> ocean_buoys_grpby = ocean_buoys.groupby_time(timebucket_duration="2cy", value_expression="buoyid", fill="NULLS")
        >>>
        #
        # Example 1: Get the basic statistics for time series aggregation for all the numeric columns.
        #            This returns max, mean, min and std values.
        #
        >>> ocean_buoys_grpby.describe()
                                                                                                   temperature salinity
        TIMECODE_RANGE                                     GROUP BY TIME(CAL_YEARS(2)) buoyid func
        ('2014-01-01 00:00:00.000000-00:00', '2016-01-0... 2                           0      max          100       55
                                                                                              mean       54.75       55
                                                                                              min           10       55
                                                                                              std       51.674        0
                                                                                       1      max           79       55
                                                                                              mean        74.5       55
                                                                                              min           70       55
                                                                                              std        3.937        0
                                                                                       2      max           82       55
                                                                                              mean          81       55
                                                                                              min           80       55
                                                                                              std            1        0
                                                                                       44     max           56       55
                                                                                              mean      48.077       55
                                                                                              min           43       55
                                                                                              std        5.766        0
        >>>
        #
        # Example 2: Get the verbose statistics for time series aggregation for all the numeric columns.
        #            This returns max, mean, min, std, median, mode, 25th, 50th and 75th percentile.
        #
        >>> ocean_buoys_grpby.describe(verbose=True)
                                                                                                     temperature salinity
        TIMECODE_RANGE                                     GROUP BY TIME(CAL_YEARS(2)) buoyid func
        ('2014-01-01 00:00:00.000000-00:00', '2016-01-0... 2                           0      25%             10       55
                                                                                              50%           54.5       55
                                                                                              75%          99.25       55
                                                                                              max            100       55
                                                                                              mean         54.75       55
                                                                                              median        54.5       55
                                                                                              min             10       55
                                                                                              mode            10       55
                                                                                              std         51.674        0
                                                                                       1      25%          71.25       55
                                                                                              50%           74.5       55
                                                                                              75%          77.75       55
                                                                                              max             79       55
                                                                                              mean          74.5       55
                                                                                              median        74.5       55
                                                                                              min             70       55
                                                                                              mode            71       55
                                                                                              mode            72       55
                                                                                              mode            77       55
                                                                                              mode            78       55
                                                                                              mode            79       55
                                                                                              mode            70       55
                                                                                              std          3.937        0
                                                                                       2      25%           80.5       55
                                                                                              50%             81       55
                                                                                              75%           81.5       55
                                                                                              max             82       55
                                                                                              mean            81       55
                                                                                              median          81       55
                                                                                              min             80       55
                                                                                              mode            80       55
                                                                                              mode            81       55
                                                                                              mode            82       55
                                                                                              std              1        0
                                                                                       44     25%             43       55
                                                                                              50%             43       55
                                                                                              75%             53       55
                                                                                              max             56       55
                                                                                              mean        48.077       55
                                                                                              median          43       55
                                                                                              min             43       55
                                                                                              mode            43       55
                                                                                              std          5.766        0
        >>>
        #
        # Example 3: Get the basic statistics for time series aggregation for all the numeric columns,
        #            consider only unique values.
        #            This returns max, mean, min and std values.
        #
        >>> ocean_buoys_grpby.describe(distinct=True)
                                                                                                   temperature salinity
        TIMECODE_RANGE                                     GROUP BY TIME(CAL_YEARS(2)) buoyid func
        ('2014-01-01 00:00:00.000000-00:00', '2016-01-0... 2                           0      max          100       55
                                                                                              mean      69.667       55
                                                                                              min           10       55
                                                                                              std       51.675     None
                                                                                       1      max           79       55
                                                                                              mean        74.5       55
                                                                                              min           70       55
                                                                                              std        3.937     None
                                                                                       2      max           82       55
                                                                                              mean          81       55
                                                                                              min           80       55
                                                                                              std            1     None
                                                                                       44     max           56       55
                                                                                              mean        52.2       55
                                                                                              min           43       55
                                                                                              std        5.263     None
        >>>
        #
        # Example 4: Get the verbose statistics for time series aggregation for all the numeric columns.
        #            This select non-default percentiles 33rd and 66th.
        #            This returns max, mean, min, std, median, mode, 33rd, and 66th percentile.
        #
        >>> ocean_buoys_grpby.describe(verbose=True, percentiles=[0.33, 0.66])
                                                                                                     temperature salinity
        TIMECODE_RANGE                                     GROUP BY TIME(CAL_YEARS(2)) buoyid func
        ('2014-01-01 00:00:00.000000-00:00', '2016-01-0... 2                           0      33%             10       55
                                                                                              66%          97.22       55
                                                                                              max            100       55
                                                                                              mean         54.75       55
                                                                                              median        54.5       55
                                                                                              min             10       55
                                                                                              mode            10       55
                                                                                              std         51.674        0
                                                                                       1      33%          71.65       55
                                                                                              66%           77.3       55
                                                                                              max             79       55
                                                                                              mean          74.5       55
                                                                                              median        74.5       55
                                                                                              min             70       55
                                                                                              mode            70       55
                                                                                              mode            71       55
                                                                                              mode            77       55
                                                                                              mode            78       55
                                                                                              mode            79       55
                                                                                              mode            72       55
                                                                                              std          3.937        0
                                                                                       2      33%          80.66       55
                                                                                              66%          81.32       55
                                                                                              max             82       55
                                                                                              mean            81       55
                                                                                              median          81       55
                                                                                              min             80       55
                                                                                              mode            80       55
                                                                                              mode            81       55
                                                                                              mode            82       55
                                                                                              std              1        0
                                                                                       44     33%             43       55
                                                                                              66%             53       55
                                                                                              max             56       55
                                                                                              mean        48.077       55
                                                                                              median          43       55
                                                                                              min             43       55
                                                                                              mode            43       55
                                                                                              std          5.766        0
        >>>
    drop
    teradataml.dataframe.dataframe.DataFrame.drop = drop(self, labels=None, axis=0, columns=None)
    DESCRIPTION:
        Drop specified labels from rows or columns.
        Remove rows or columns by specifying label names and corresponding
        axis, or by specifying the index or column names directly.
    PARAMETERS:
        labels:
            Optional Argument. Required when columns is not provided.
            Single label or list-like. Can be Index or column labels to drop depending on axis.
            Types: str OR list of Strings (str)
        axis:
            Optional Argument.
            0 or 'index' for index labels
            1 or 'columns' for column labels
            Default Values: 0
            Permitted Values: 0, 1, 'index', 'columns'
            Types: int OR str
        columns:
            Optional Argument. Required when labels is not provided.
            Single label or list-like. This is an alternative to specifying axis=1 with labels.
            Cannot specify both labels and columns.
            Types: str OR list of Strings (str)
    RETURNS:
        teradataml DataFrame
    RAISE:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame('admissions_train')
        >>> df
           masters   gpa     stats programming admitted
        id
        5       no  3.44    Novice      Novice        0
        7      yes  2.33    Novice      Novice        1
        22     yes  3.46    Novice    Beginner        0
        17      no  3.83  Advanced    Advanced        1
        13      no  4.00  Advanced      Novice        1
        19     yes  1.98  Advanced    Advanced        0
        36      no  3.00  Advanced      Novice        0
        15     yes  4.00  Advanced    Advanced        1
        34     yes  3.85  Advanced    Beginner        0
        40     yes  3.95    Novice    Beginner        0
        # Drop columns
        >>> df.drop(['stats', 'admitted'], axis=1)
           programming masters   gpa
        id
        5       Novice      no  3.44
        34    Beginner     yes  3.85
        13      Novice      no  4.00
        40    Beginner     yes  3.95
        22    Beginner     yes  3.46
        19    Advanced     yes  1.98
        36      Novice      no  3.00
        15    Advanced     yes  4.00
        7       Novice     yes  2.33
        17    Advanced      no  3.83
        >>> df.drop(columns=['stats', 'admitted'])
           programming masters   gpa
        id
        5       Novice      no  3.44
        34    Beginner     yes  3.85
        13      Novice      no  4.00
        19    Advanced     yes  1.98
        15    Advanced     yes  4.00
        40    Beginner     yes  3.95
        7       Novice     yes  2.33
        22    Beginner     yes  3.46
        36      Novice      no  3.00
        17    Advanced      no  3.83
        # Drop a row by index
        >>> df1 = df[df.gpa == 4.00]
        >>> df1
           masters  gpa     stats programming admitted
        id
        13      no  4.0  Advanced      Novice        1
        29     yes  4.0    Novice    Beginner        0
        15     yes  4.0  Advanced    Advanced        1
        >>> df1.drop([13,15], axis=0)
           masters  gpa   stats programming admitted
        id
        29     yes  4.0  Novice    Beginner        0
        >>>
    drop_duplicate
    teradataml.dataframe.dataframe.DataFrame.drop_duplicate = drop_duplicate(self, column_names=None)
    DESCRIPTION:
        Function drops the duplicate rows, i.e., returns the distinct values from teradataml DataFrame.
    PARAMETERS:
        column_names:
            Optional argument.
            Specifies the name(s) of the column(s) to drop the duplicates, i.e., to get the
            distinct values. If not specified, all columns in the DataFrame are considered for
            the operation.
            Types: str OR list of Strings (str)
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Create a teradataml DataFrame.
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame('admissions_train')
        >>> df
            masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        19     yes  1.98  Advanced    Advanced         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        38     yes  2.65  Advanced    Beginner         1
        # Example 1: Get the distinct rows of values for the column programming.
        >>> df.drop_duplicate('programming')
          programming
        0    Beginner
        1    Advanced
        2      Novice
        # Example 2: Get the distinct rows of values for the columns programming and admitted.
        >>> df.drop_duplicate(['programming','admitted'])
          programming  admitted
        0    Advanced         1
        1      Novice         0
        2      Novice         1
        3    Beginner         1
        4    Advanced         0
        5    Beginner         0
    dropna
    teradataml.dataframe.dataframe.DataFrame.dropna = dropna(self, how='any', thresh=None, subset=None)
    DESCRIPTION:
        Removes rows with null values.
    PARAMETERS:
        how:
            Optional Argument.
            Specifies how rows are removed.
            'any' removes rows with at least one null value.
            'all' removes rows with all null values.
            Default Value: 'any'
            Permitted Values: 'any' or 'all'
            Types: str
        thresh:
            Optional Argument.
            Specifies the minimum number of non null values in a row to include.
            Types: int
        subset:
            Optional Argument.
            Specifies list of column names to include, in array-like format.
            Types: str OR list of Strings (str)
    RETURNS:
        teradataml DataFrame
    RAISE:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame('sales')
        >>> df
                      Feb   Jan   Mar   Apr    datetime
        accounts
        Jones LLC   200.0   150   140   180  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        Blue Inc     90.0    50    95   101  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        # Drop the rows where at least one element is null.
        >>> df.dropna()
                     Feb  Jan  Mar  Apr    datetime
        accounts
        Blue Inc    90.0   50   95  101  04/01/2017
        Jones LLC  200.0  150  140  180  04/01/2017
        Alpha Co   210.0  200  215  250  04/01/2017
        # Drop the rows where all elements are nulls for columns 'Jan' and 'Mar'.
        >>> df.dropna(how='all', subset=['Jan','Mar'])
                     Feb  Jan  Mar   Apr    datetime
        accounts
        Alpha Co   210.0  200  215   250  04/01/2017
        Jones LLC  200.0  150  140   180  04/01/2017
        Red Inc    200.0  150  140  None  04/01/2017
        Blue Inc    90.0   50   95   101  04/01/2017
        # Keep only the rows with at least 4 non null values.
        >>> df.dropna(thresh=4)
                      Feb   Jan   Mar   Apr    datetime
        accounts
        Jones LLC   200.0   150   140   180  04/01/2017
        Blue Inc     90.0    50    95   101  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        # Keep only the rows with at least 5 non null values.
        >>> df.dropna(thresh=5)
                     Feb  Jan  Mar   Apr    datetime
        accounts
        Alpha Co   210.0  200  215   250  04/01/2017
        Jones LLC  200.0  150  140   180  04/01/2017
        Blue Inc    90.0   50   95   101  04/01/2017
        Red Inc    200.0  150  140  None  04/01/2017
    dtypes
    teradataml.dataframe.dataframe.DataFrame.dtypes
    Returns a MetaData containing the column names and types.
    PARAMETERS:
    RETURNS:
        MetaData containing the column names and Python types
    RAISES:
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> print(df.dtypes)
        accounts              str
        Feb                 float
        Jan                   int
        Mar                   int
        Apr                   int
        datetime    datetime.date
        >>>
    fillna
    teradataml.dataframe.dataframe.DataFrame.ﬁllna = ﬁllna(self, value=None, columns=None, literal_value=False, partition_column=None)
    DESCRIPTION:
        Method to replace the null values in a column with the value specified.
    PARAMETERS:
        value:
            Required Argument.
            Specifies the value(s) to replace the null values with. If value is a dict
            then "columns" is ignored.
            Note:
                * To use pre-defined strings to replace the null value set "literal_value" to True.
            Permitted Values:
                * Pre-defined strings:
                    * 'MEAN' - Replace null value with the average of the values in the column.
                    * 'MODE' - Replace null value with the mode of the values in the column.
                    * 'MEDIAN' - Replace null value with the median of the values in the column.
                    * 'MIN' - Replace null value with the minimum of the values in the column.
                    * 'MAX' - Replace null value with the maximum of the values in the column.
            Types: int, float, str, dict containing column names and value, list
        columns:
            Optional Argument.
            Specifies the column names to perform the null value replacement. If "columns"
            is None, then all the columns having null value and data type similar to
            the data type of the value specified are considered.
            Default Value: None
            Types: str, tuple or list of str
        literal_value:
            Optional Argument.
            Specifies whether the pre-defined strings passed to "value" should be treated
            as literal or not.
            Default Value: False
            Types: bool
        partition_column:
            Optional Argument.
            Specifies the column name to partition the data.
            Default Value: None
            Types: str
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe", "sales")
        >>> df = DataFrame("sales")
        >>> df
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        # Example 1: Populate null value in column 'Jan' and 'Mar'
        #            with the value specified as dictionary.
        >>> df.fillna({"Jan": 123, "Mar":234})
                 accounts    Feb  Jan  Mar    Apr  datetime
            0    Blue Inc   90.0   50   95  101.0  17/01/04
            1    Alpha Co  210.0  200  215  250.0  17/01/04
            2   Jones LLC  200.0  150  140  180.0  17/01/04
            3  Yellow Inc   90.0  123  234    NaN  17/01/04
            4  Orange Inc  210.0  123  234  250.0  17/01/04
            5     Red Inc  200.0  150  140    NaN  17/01/04
        # Example 2: Populate the null value in 'Jan' column
        #            with minimum value in that column.
        >>> df.fillna("Min", "Jan")
                 accounts    Feb  Jan    Mar    Apr  datetime
            0  Yellow Inc   90.0   50    NaN    NaN  17/01/04
            1   Jones LLC  200.0  150  140.0  180.0  17/01/04
            2     Red Inc  200.0  150  140.0    NaN  17/01/04
            3    Blue Inc   90.0   50   95.0  101.0  17/01/04
            4    Alpha Co  210.0  200  215.0  250.0  17/01/04
            5  Orange Inc  210.0   50    NaN  250.0  17/01/04
        # Example 3: Populate the null value in 'pclass' and
        #            'fare' column with mean value with partition
        #            column as 'sex'.
        # Load the example data.
        >>> load_example_data("teradataml", ["titanic"])
        >>> df = DataFrame.from_table("titanic")
        >>> df.fillna(value="mean", columns=["pclass", "fare"], partition_column="sex")
            passenger  survived  pclass                                         name     sex   age  sibsp  parch            ticket 
        0        284         1       3                   Dorking, Mr. Edward Arthur    male  19.0      0      0        A/5. 10482  
        1        589         0       3                        Gilinski, Mr. Eliezer    male  22.0      0      0             14973  
        2         17         0       3                         Rice, Master. Eugene    male   2.0      4      1            382652  
        3        282         0       3             Olsson, Mr. Nils Johan Goransson    male  28.0      0      0            347464  
        4        608         1       1                  Daniel, Mr. Robert Williams    male  27.0      0      0            113804  
        5        404         0       3               Hakkarainen, Mr. Pekka Pietari    male  28.0      1      0  STON/O2. 3101279  
        6        427         1       2  Clarke, Mrs. Charles V (Ada Maria Winfield)  female  28.0      1      0              2003  
        7        141         0       3                Boulos, Mrs. Joseph (Sultana)  female   NaN      0      2              2678  
        8        610         1       1                    Shutes, Miss. Elizabeth W  female  40.0      0      0          PC 17582  
        9        875         1       2        Abelson, Mrs. Samuel (Hannah Wizosky)  female  28.0      1      0         P/PP 3381  
    filter
    teradataml.dataframe.dataframe.DataFrame.ﬁlter = ﬁlter(self, items=None, like=None, regex=None, axis=1, **kw)
    DESCRIPTION:
        Filter rows or columns of dataframe according to labels in the specified index.
        The filter is applied to the columns of the index when axis is set to 'rows'.
        Must use one of the parameters 'items', 'like', and 'regex' only.
    PARAMETERS:
        axis:
            Optional Argument.
            Specifies the axis to filter on.
            1 denotes column axis (default). Alternatively, 'columns' can be specified.
            0 denotes row axis. Alternatively, 'rows' can be specified.
            Default Values: 1
            Permitted Values: 0, 1, 'rows', 'columns'
            Types: int OR str
        items:
            Optional Argument.
            List of values that the info axis should be restricted to.
            When axis is 1, items are a list of column names.
            When axis is 0, items are a list of literal values.
            Types: list of Strings (str) or literals
        like:
            Optional Argument.
            Specifies the substring pattern.
            When axis is 1, substring pattern for matching column names.
            When axis is 0, substring pattern for checking index values
            with REGEXP_SUBSTR.
            Types: str
        regex:
            Optional Argument.
            Specifies a regular expression pattern.
            When axis is 1, regex pattern for re.search(regex, column_name).
            When axis is 0, regex pattern for checking index values
            with REGEXP_SUBSTR.
            Types: str
        **kw: optional keyword arguments
            varchar_size:
                Specifies an integer to specify the size of varchar-casted index.
                Used when axis=0 or axis='rows' and index must be char-like in
                "like" and "regex" filtering.
                Default Value: configure.default_varchar_size
                Types: int
            match_arg: string
                Specifies argument to pass if axis is 0/'rows' and regex is used.
                Valid values for match_arg are:
                - 'i': case-insensitive matching.
                - 'c': case sensitive matching.
                - 'n': the period character (match any character) can match
                       the newline character.
                - 'm': index value is treated as multiple lines instead of as a single
                       line. With this option, the '^' and '$' characters apply to each
                       line in source_string instead of the entire index value.
                - 'l': if index value exceeds the current maximum allowed size
                       (currently 16 MB), a NULL is returned instead of an error.
                       This is useful for long-running queries where you do not want
                       long strings causing an error that would make the query fail.
                - 'x': ignore whitespace.
        The 'match_arg' argument may contain more than one character.
        If a character in 'match_arg' is not valid, then that character is ignored.
        See Teradata® Database SQL Functions, Operators, Expressions, and Predicates,
        for more information on specifying arguments for REGEXP_SUBSTR.
        NOTES:
            - Using 'regex' or 'like' with axis equal to 0 will attempt to cast the
              values in the index to a VARCHAR.
              Note that conversion between BYTE data and other types is not supported.
              Also, LOBs are not allowed to be compared.
            - When using 'like' or 'regex', datatypes are casted into VARCHAR.
              This may alter the format of the value in the column(s)
              and thus whether there is a match or not. The size of the VARCHAR may also
              play a role since the casted value is truncated if the size is not big enough.
              See varchar_size under **kw: optional keyword arguments.
    RETURNS:
        teradataml DataFrame
    RAISES:
        ValueError if more than one parameter: 'items', 'like', or 'regex' is used.
        TeradataMlException if invalid argument values are given.
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame('admissions_train')
        >>> df
           masters   gpa     stats programming admitted
        id
        22     yes  3.46    Novice    Beginner        0
        37      no  3.52    Novice      Novice        1
        35      no  3.68    Novice    Beginner        1
        12      no  3.65    Novice      Novice        1
        4      yes  3.50  Beginner      Novice        1
        38     yes  2.65  Advanced    Beginner        1
        27     yes  3.96  Advanced    Advanced        0
        39     yes  3.75  Advanced    Beginner        0
        7      yes  2.33    Novice      Novice        1
        40     yes  3.95    Novice    Beginner        0
        >>>
        >>> # retrieve columns masters, gpa, and stats in df
        ... df.filter(items = ['masters', 'gpa', 'stats'])
          masters   gpa     stats
        0     yes  4.00  Advanced
        1     yes  3.45  Advanced
        2     yes  3.50  Advanced
        3     yes  4.00    Novice
        4     yes  3.59  Advanced
        5      no  3.87    Novice
        6     yes  3.50  Beginner
        7     yes  3.79  Advanced
        8      no  3.00  Advanced
        9     yes  1.98  Advanced
        >>>
        >>> # retrieve rows where index matches ‘2’, ‘4’
        ... df.filter(items = ['2', '4'], axis = 0)
           masters   gpa     stats programming admitted
        id
        2      yes  3.76  Beginner    Beginner        0
        4      yes  3.50  Beginner      Novice        1
        >>>
        >>> df = DataFrame('admissions_train', index_label="programming")
        >>> df
                     id masters   gpa     stats admitted
        programming
        Beginner     22     yes  3.46    Novice        0
        Novice       37      no  3.52    Novice        1
        Beginner     35      no  3.68    Novice        1
        Novice       12      no  3.65    Novice        1
        Novice        4     yes  3.50  Beginner        1
        Beginner     38     yes  2.65  Advanced        1
        Advanced     27     yes  3.96  Advanced        0
        Beginner     39     yes  3.75  Advanced        0
        Novice        7     yes  2.33    Novice        1
        Beginner     40     yes  3.95    Novice        0
        >>>
        >>> # retrieve columns with a matching substring
        ... df.filter(like = 'masters')
          masters
        0     yes
        1     yes
        2     yes
        3     yes
        4     yes
        5      no
        6     yes
        7     yes
        8      no
        9     yes
        >>>
        >>> # retrieve rows where index values have ‘vice’ as a subtring
        ... df.filter(like = 'vice', axis = 'rows')
                     id masters   gpa     stats admitted
        programming
        Novice       12      no  3.65    Novice        1
        Novice        5      no  3.44    Novice        0
        Novice       24      no  1.87  Advanced        1
        Novice       36      no  3.00  Advanced        0
        Novice       23     yes  3.59  Advanced        1
        Novice       13      no  4.00  Advanced        1
        Novice       33      no  3.55    Novice        1
        Novice       30     yes  3.79  Advanced        0
        Novice        4     yes  3.50  Beginner        1
        Novice       37      no  3.52    Novice        1
        >>>
        >>> # give a regular expression to match column names
        ... df.filter(regex = '^a.+')
          admitted
        0        0
        1        1
        2        1
        3        1
        4        1
        5        1
        6        0
        7        0
        8        1
        9        0
        >>>
        >>> # give a regular expression to match values in index
        ... df.filter(regex = '^B.+', axis = 0)
                     id masters   gpa     stats admitted
        programming
        Beginner     39     yes  3.75  Advanced        0
        Beginner     38     yes  2.65  Advanced        1
        Beginner      3      no  3.70    Novice        1
        Beginner     31     yes  3.50  Advanced        1
        Beginner     21      no  3.87    Novice        1
        Beginner     34     yes  3.85  Advanced        0
        Beginner     32     yes  3.46  Advanced        0
        Beginner     29     yes  4.00    Novice        0
        Beginner     35      no  3.68    Novice        1
        Beginner     22     yes  3.46    Novice        0
        >>>
        >>> # case-insensitive, ignore white space when matching index values
        ... df.filter(regex = '^A.+', axis = 0, match_args = 'ix')
                     id masters   gpa     stats admitted
        programming
        Advanced     20     yes  3.90  Advanced        1
        Advanced      8      no  3.60  Beginner        1
        Advanced     25      no  3.96  Advanced        1
        Advanced     19     yes  1.98  Advanced        0
        Advanced     14     yes  3.45  Advanced        0
        Advanced      6     yes  3.50  Beginner        1
        Advanced     17      no  3.83  Advanced        1
        Advanced     11      no  3.13  Advanced        1
        Advanced     15     yes  4.00  Advanced        1
        Advanced     18     yes  3.81  Advanced        1
        >>>
        >>> # case-insensitive/ ignore white space/ match up to 32 characters
        ... df.filter(regex = '^A.+', axis = 0, match_args = 'ix', varchar_size = 32)
                     id masters   gpa     stats admitted
        programming
        Advanced     20     yes  3.90  Advanced        1
        Advanced      8      no  3.60  Beginner        1
        Advanced     25      no  3.96  Advanced        1
        Advanced     19     yes  1.98  Advanced        0
        Advanced     14     yes  3.45  Advanced        0
        Advanced      6     yes  3.50  Beginner        1
        Advanced     17      no  3.83  Advanced        1
        Advanced     11      no  3.13  Advanced        1
        Advanced     15     yes  4.00  Advanced        1
        Advanced     18     yes  3.81  Advanced        1
        >>>
    from_dict
    teradataml.dataframe.dataframe.DataFrame.from_dict = from_dict(data, columns=None) method of builtins.type instance
    DESCRIPTION:
        Creates a DataFrame from a dictionary containing values as lists or numpy arrays.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the Python dictionary to create a teradataml DataFrame.
            Notes:
                * Keys of the dictionary are used as column names.
                * Values of the dictionary should be lists or numpy arrays.
                * Nested dictionaries are not supported.
            Types: dict
        columns:
            Optional Argument.
            Specifies the column names for the DataFrame.
            Types: str OR list of str
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> from teradataml import DataFrame
        >>> data_dict = {"name": ["Alice", "Bob", "Charlie"], "age": [25, 30, 28]}
        # Example 1: Create a teradataml DataFrame from a dictionary where 
        #            keys are column names and values are lists of column data.
        >>> df = DataFrame.from_dict(data_dict)
        >>> df
              name  age
        0  Charlie   28
        1      Bob   30
        2    Alice   25
        # Example 2: Create a teradataml DataFrame from a dictionary where
        #            keys are column names and values are numpy arrays.
        >>> import numpy as np
        >>> data_dict = {"col1": np.array([1, 2, 3]), "col2": np.array([4, 5, 6])}
        >>> df = DataFrame.from_dict(data_dict)
        >>> df
           col1  col2
        0     3     6
        1     2     5
        2     1     4
    from_pandas
    teradataml.dataframe.dataframe.DataFrame.from_pandas = from_pandas(pandas_df, index=True, index_label=None, primary_index=None) method of builtins.type instance
    DESCRIPTION:
        Creates a teradataml DataFrame from a pandas DataFrame.
    PARAMETERS:
        pandas_df:
            Required Argument.
            Specifies the pandas DataFrame to be converted to teradataml DataFrame.
            Types: pandas DataFrame
        index:
            Optional Argument.
            Specifies whether to save Pandas DataFrame index as a column or not.
            Default Value: True
            Types: bool
        index_label:
            Optional Argument.
            Specifies the column label(s) for Pandas DataFrame index column(s).
            Note:
                * Refer to the "index_label" parameter of copy_to_sql() for more details.
            Default Value: None
            Types: str OR list of str
        primary_index:
            Optional Argument.
            Specifies which column(s) to use as primary index for the teradataml DataFrame.
            Types: str OR list of str
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> import pandas as pd
        >>> from teradataml import DataFrame
        >>> pdf = pd.DataFrame({"col1": [1, 2, 3], "col2": [4, 5, 6]})
        >>> pdf1 = pd.DataFrame([[1, 2], [3, 4]])
        # Example 1: Create a teradataml DataFrame from a pandas DataFrame.
        >>> df = DataFrame.from_pandas(pdf)
        >>> df
           col1  col2  index_label
        0     3     6            2
        1     2     5            1
        2     1     4            0
        # Example 2: Create a teradataml DataFrame from a pandas DataFrame
        #            and do not save the index as a column.
        >>> df = DataFrame.from_pandas(pdf, index=False)
        >>> df
           col1  col2
        0     3     6
        1     2     5
        2     1     4
        # Example 3: Create a teradataml DataFrame from a pandas DataFrame
        #            with index label as 'id' and set it as primary index.
        >>> df = DataFrame.from_pandas(pdf, index=True, index_label='id', primary_index='id')
        >>> df
            col1  col2
        id
        2      3     6
        1      2     5
        0      1     4
        # Example 4: Create a teradataml DataFrame from a pandas DataFrame where
        #            columns are not explicitly defined in the pandas DataFrame.
        >>> df = DataFrame.from_pandas(pdf1)
        >>> df
           col_0  col_1  index_label
        0      3      4            1
        1      1      2            0
    from_records
    teradataml.dataframe.dataframe.DataFrame.from_records = from_records(data, columns=None, **kwargs) method of builtins.type instance
    DESCRIPTION:
        Create a DataFrame from a list of lists/tuples/dictionaries/numpy arrays.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the iterator of data or the list of lists/tuples/dictionaries/numpy arrays to 
            be converted to teradataml DataFrame.
            Note:
                * Nested lists or tuples or dictionaries are not supported.
            Types: Iterator, list
        columns:
            Optional Argument.
            Specifies the column names for the DataFrame.
            Note:
                * If the data is a list of lists/tuples/numpy arrays and this argument
                  is not specified, column names will be auto-generated as 'col_0', 'col_1', etc.
            Types: str OR list of str
        kwargs:
            exclude:
                Optional Argument.
                Specifies the columns to be excluded from the DataFrame.
                Types: list OR tuple
            coerce_float:
                Optional Argument.
                Specifies whether to convert values of non-string, non-numeric objects (like decimal.Decimal) 
                to floating point, useful for SQL result sets.
                Default Value: True
                Types: bool
            nrows:
                Optional Argument.
                Specifies the number of rows to be read from the data if the data is iterator.
                Types: int
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> from teradataml import DataFrame
        # Example 1: Create a teradataml DataFrame from a list of lists.
        >>> df = DataFrame.from_records([['Alice', 1], ['Bob', 2]], columns=['name', 'age'])
        >>> df
            name  age
        0    Bob    2
        1  Alice    1
        # Example 2: Create a teradataml DataFrame from a list of tuples.
        >>> df = DataFrame.from_records([('Alice', 1), ('Bob', 3)], columns=['name', 'age'])
        >>> df
            name  age 
        0    Bob    3
        1  Alice    1
        # Example 3: Create a teradataml DataFrame from a list of dictionaries.
        >>> df = DataFrame.from_records([{'name': 'Alice', 'age': 4}, {'name': 'Bob', 'age': 2}])
        >>> df
            name  age 
        0    Bob    2
        1  Alice    4
        # Example 4: Create a teradataml DataFrame from a list where columns
        #            are not explicitly defined.
        >>> df = DataFrame.from_records([['Alice', 1], ['Bob', 2]])
        >>> df
           col_0  col_1 
        0    Bob      2
        1  Alice      1
        # Example 5: Create a teradataml DataFrame from a list by excluding 'grade' column.
        >>> df = DataFrame.from_records([['Alice', 1, 'A'], ['Bob', 2, 'B']], 
        ...                               columns=['name', 'age', 'grade'], 
        ...                               exclude=['grade'])
        >>> df
            name  age 
        0    Bob    2
        1  Alice    1
        # Example 6: Create a teradataml DataFrame from a list of lists
        #            with "coerce_float" set to False.
        >>> df = DataFrame.from_records([[1, Decimal('2.5')], [3, Decimal('4.0')]],
        ...                              columns=['col1', 'col2'], coerce_float=False)
        >>> df
            col1  col2 
        0      3   4.0
        1      1   2.5
        >>> df.tdtypes
        col1                                          BIGINT()
        col2           VARCHAR(length=1024, charset='UNICODE')
        # Example 7: Create a teradataml DataFrame from a list of lists
        #            with "coerce_float" set to True.
        >>> from decimal import Decimal
        >>> df = DataFrame.from_records([[1, Decimal('2.5')], [3, Decimal('4.0')]],
        ...                              columns=['col1', 'col2'], coerce_float=True)
        >>> df
           col1  col2 
        0     3   4.0
        1     1   2.5
        >>> df.tdtypes
        col1           BIGINT()
        col2            FLOAT()
        # Example 8: Create a teradataml DataFrame from an iterator with "nrows" set to 2.
        >>> def data_gen():
        ...     yield ['Alice', 1]
        ...     yield ['Bob', 2]
        ...     yield ['Charlie', 3]
        >>> df = DataFrame.from_records(data_gen(), columns=['name', 'age'], nrows=2)
        >>> df
            name  age 
        0    Bob    2
        1  Alice    1
    get
    teradataml.dataframe.dataframe.DataFrame.get = get(self, key)
    DESCRIPTION:
        Retrieve required columns from DataFrame using column name(s) as key.
        Returns a new teradataml DataFrame with requested columns only.
    PARAMETERS:
        key:
            Required Argument.
            Specifies column(s) to retrieve from the teradataml DataFrame.
            Types: str OR List of Strings (str)
        teradataml supports the following formats (only) for the "get" method:
        1] Single Column String: df.get("col1")
        2] Single Column List: df.get(["col1"])
        3] Multi-Column List: df.get(['col1', 'col2', 'col3'])
        4] Multi-Column List of List: df.get([["col1", "col2", "col3"]])
        Note: Multi-Column retrieval of the same column such as df.get(['col1', 'col1']) is not supported.
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df.sort('id')
           masters   gpa     stats programming admitted
        id
        1      yes  3.95  Beginner    Beginner        0
        2      yes  3.76  Beginner    Beginner        0
        3       no  3.70    Novice    Beginner        1
        4      yes  3.50  Beginner      Novice        1
        ...
        1] Single String Column
        >>> df.get("gpa")
            gpa
        0  3.46
        1  3.52
        2  3.68
        3  3.65
        ...
        2] Single Column List
        >>> df.get(["gpa"])
            gpa
        0  3.46
        1  3.52
        2  3.68
        3  3.65
        ...
        3] Multi-Column List
        >>> df.get(["programming", "masters", "gpa"])
          programming masters   gpa
        0    Beginner     yes  3.46
        1      Novice      no  3.52
        2    Beginner      no  3.68
        3      Novice      no  3.65
        ...
        4] Multi-Column List of List
        >>> df.get([['programming', 'masters', 'gpa']])
          programming masters   gpa
        0    Advanced     yes  4.00
        1    Advanced     yes  3.45
        2    Beginner     yes  3.50
        3    Beginner     yes  4.00
        ...
    get_output
    teradataml.dataframe.dataframe.DataFrame.get_output = get_output(self, output_index=0)
    DESCRIPTION:
        Returns the result of analytic function when analytic function is
        run from 'Analyze' tab in EDA UI.
        Note:
            * The function does not return anything if analytic function is
              not run from EDA UI.
    PARAMETERS:
        output_index:
            Optional Argument.
            Specifies the index of the output dataframe to be returned.
            Default Value: 0
            Types: int
    RAISES:
        IndexError
    RETURNS:
        teradataml DataFrame object.
    get_snapshot
    teradataml.dataframe.dataframe.DataFrame.get_snapshot = get_snapshot(self, as_of)
    DESCRIPTION:
        Gets the data from a DataLake table for the given snapshot id or timestamp string.
        Notes:
            * The snapshot id can be obtained from the 'snapshots' property of the DataFrame.
            * The time travel value represented by 'as_of' should be in the format "YYYY-MM-DD HH:MM:SS.FFFFFFF"
              for TIMESTAMP string or "YYYY-MM-DD" for DATE string.
    PARAMETERS:
        as_of:
            Required Argument.
            Specifies the snapshot id or timestamp information for which the snapshot is to be fetched.
            Types: str or int
    RETURNS:
        teradataml DataFrame.
    RAISES:
        TeradataMLException.
    EXAMPLES:
        # DataFrame creation on OTF table.
        >>> from teradataml.dataframe.dataframe import in_schema
        >>> in_schema_tbl = in_schema(schema_name="datalake_db",
        ...                           table_name="datalake_table",
        ...                           datalake_name="datalake")
        >>> datalake_df = DataFrame(in_schema_tbl)
        # List snapshots first.
        >>> datalake_df.snapshots
                    snapshotId        snapshotTimestamp      timestampMSecs                                      manifestList      
           2046682612111137809      2025-06-03 13:26:15       1748957175692  s3://vim-iceberg-
    v1/datalake_db/datalake_table/metadata/snap-204...    {"added-data-files":"Red Inc","added-records"...}
            282293708812257203      2025-06-03 05:53:19       1748929999245  s3://vim-iceberg-
    v1/datalake_db/datalake_table/metadata/snap-282...    {"added-data-files":"Blue Inc","added-records"...}
        # Example 1: Get the snapshot using snapshot id.
        >>> datalake_df.get_snapshot(2046682612111137809)
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        # Example 2: Get the snapshot using snapshot id in string format.
        >>> datalake_df.get_snapshot("2046682612111137809")
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        # Example 3: Get the snapshot using timestamp string.
        >>> datalake_df.get_snapshot("2025-06-03 13:26:16")
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        # Example 4: Get the snapshot using date string.
        >>> datalake_df.get_snapshot("2025-06-04")
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
    get_values
    teradataml.dataframe.dataframe.DataFrame.get_values = get_values(self, num_rows=99999)
    DESCRIPTION:
        Retrieves all values (only) present in a teradataml DataFrame.
        Values are retrieved as per a numpy.ndarray representation of a teradataml DataFrame.
        This format is equivalent to the get_values() representation of a Pandas DataFrame.
    PARAMETERS:
        num_rows:
            Optional Argument.
            Specifies the number of rows to retrieve values for from a teradataml DataFrame.
            The num_rows parameter specified needs to be an integer value.
            Default Value: 99999
            Types: int
    RETURNS:
        Numpy.ndarray representation of a teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df1 = DataFrame.from_table('admissions_train')
        >>> df1
           masters   gpa     stats programming admitted
        id
        15     yes  4.00  Advanced    Advanced        1
        7      yes  2.33    Novice      Novice        1
        22     yes  3.46    Novice    Beginner        0
        17      no  3.83  Advanced    Advanced        1
        13      no  4.00  Advanced      Novice        1
        38     yes  2.65  Advanced    Beginner        1
        26     yes  3.57  Advanced    Advanced        1
        5       no  3.44    Novice      Novice        0
        34     yes  3.85  Advanced    Beginner        0
        40     yes  3.95    Novice    Beginner        0
        # Retrieve all values from the teradataml DataFrame
        >>> vals = df1.get_values()
        >>> vals
        array([['yes', 4.0, 'Advanced', 'Advanced', 1],
               ['yes', 3.45, 'Advanced', 'Advanced', 0],
               ['yes', 3.5, 'Advanced', 'Beginner', 1],
               ['yes', 4.0, 'Novice', 'Beginner', 0],
                             . . .
               ['no', 3.68, 'Novice', 'Beginner', 1],
               ['yes', 3.5, 'Beginner', 'Advanced', 1],
               ['yes', 3.79, 'Advanced', 'Novice', 0],
               ['no', 3.0, 'Advanced', 'Novice', 0],
               ['yes', 1.98, 'Advanced', 'Advanced', 0]], dtype=object)
        # Retrieve values for a given number of rows from the teradataml DataFrame
        >>> vals = df1.get_values(num_rows = 3)
        >>> vals
        array([['yes', 4.0, 'Advanced', 'Advanced', 1],
               ['yes', 3.45, 'Advanced', 'Advanced', 0],
               ['yes', 3.5, 'Advanced', 'Beginner', 1]], dtype=object)
        # Access specific values from the entire set received as per below:
        # Retrieve all values from an entire row (for example, the first row):
        >>> vals[0]
        array(['yes', 4.0, 'Advanced', 'Advanced', 1], dtype=object)
        # Alternatively, specify a range to retrieve values from  a subset of rows (For example, first 3 rows):
        >>> vals[0:3]
        array([['yes', 4.0, 'Advanced', 'Advanced', 1],
        ['yes', 3.45, 'Advanced', 'Advanced', 0],
        ['yes', 3.5, 'Advanced', 'Beginner', 1]], dtype=object)
        # Alternatively, retrieve all values from an entire column (For example, the first column):
        >>> vals[:, 0]
        array(['yes', 'yes', 'yes', 'yes', 'yes', 'no', 'yes', 'yes', 'yes',
               'yes', 'no', 'no', 'yes', 'yes', 'no', 'yes', 'no', 'yes', 'no',
               'no', 'no', 'no', 'no', 'no', 'yes', 'yes', 'no', 'no', 'yes',
               'yes', 'yes', 'no', 'no', 'yes', 'no', 'no', 'yes', 'yes', 'no',
               'yes'], dtype=object)
        # Alternatively, retrieve a single value from a given row and column (For example, 3rd row, and 2nd column):
        >>> vals[2,1]
        3.5
        Note:
        1) Row and column indexing starts from 0, so the first column = index 0, second column = index 1, and so on...
        2) When a Pandas DataFrame is saved to Teradata Vantage & retrieved back as a teradataml DataFrame, the get_values()
           method on a Pandas DataFrame and the corresponding teradataml DataFrames have the following type differences:
               - teradataml DataFrame get_values() retrieves 'bool' type Pandas DataFrame values (True/False) as BYTEINTS (1/0)
               -
     teradataml DataFrame get_values() retrieves 'Timedelta' type Pandas DataFrame values as equivalent values in seconds.
    groupby
    teradataml.dataframe.dataframe.DataFrame.groupby = groupby(self, columns_expr, **kwargs)
    DESCRIPTION:
        Applies GroupBy to one or more columns of a teradataml Dataframe.
        The result will always behaves like calling groupby with as_index=False
        in pandas.
    PARAMETERS:
        columns_expr:
            Required Argument.
            Specifies the column name(s) to group by.
            Types: str OR list of Strings (str)
        kwargs:
            Optional Argument.
            Specifies keyword arguments.
            option:
                Optional Argument.
                Specifies the groupby option.
                Permitted Values: "CUBE", "ROLLUP", None
                Types: str or NoneType
            include_grouping_columns:
                Optional Argument.
                Specifies whether to include aggregations on the grouping column(s) or not.
                When set to True, the resultant DataFrame will have the aggregations on the 
                columns mentioned in "columns_expr". Otherwise, resultant DataFrame will not have 
                aggregations on the columns mentioned in "columns_expr".
                Default Value: False
                Types: bool
    NOTES:
        1. Users can still apply teradataml DataFrame methods (filters/sort/etc) on top of the result.
        2. Consecutive operations of grouping, i.e., groupby_time(), resample() and groupby() are not permitted.
           An exception will be raised. Following are some cases where exception will be raised as
           "Invalid operation applied, check documentation for correct usage."
                a. df.resample().groupby()
                b. df.resample().resample()
                c. df.resample().groupby_time()
    RETURNS:
        teradataml DataFrameGroupBy Object
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe","admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        26     yes  3.57  Advanced    Advanced         1
        17      no  3.83  Advanced    Advanced         1
        # Example 1: Find the minimum value of all valid columns by 
        #            grouping the DataFrame with column 'masters'.
        >>> df1 = df.groupby(["masters"])
        >>> df1.min()
          masters min_id  min_gpa min_stats min_programming min_admitted
        0      no      3     1.87  Advanced        Advanced            0
        1     yes      1     1.98  Advanced        Advanced            0
        # Example 2: Find the sum of all valid columns by grouping the DataFrame
        #            with columns 'masters' and 'admitted'. Include grouping columns
        #            in aggregate function 'sum'.
        >>> df1 = df.groupby(["masters", "admitted"], include_grouping_columns=True)
        >>> df1.sum()
          masters  admitted  sum_id  sum_gpa  sum_admitted
        0     yes         1     188    34.35            10
        1     yes         0     289    43.36             0
        2      no         0      41     6.44             0
        3      no         1     302    57.52            16
        # Example 3: Find the sum of all valid columns by grouping the DataFrame with
        #            columns 'masters' and 'admitted'. Do not include grouping columns
        #            in aggregate function 'sum'. 
        >>> df1 = df.groupby(["masters", "admitted"], include_grouping_columns=False)
        >>> df1.sum()
          masters  admitted  sum_id  sum_gpa
        0     yes         0     289    43.36
        1      no         0      41     6.44
        2      no         1     302    57.52
        3     yes         1     188    34.35
    groupby_time
    teradataml.dataframe.dataframe.DataFrame.groupby_time = groupby_time(self, timebucket_duration, value_expression=None, timecode_column=None,
    sequence_column=None, ﬁll=None)
    DESCRIPTION:
        Apply Group By Time to one or more columns of a teradataml DataFrame.
        The result always behaves like calling group by time. Outcome of this function
        can be used to run Time Series Aggregate functions.
    PARAMETERS:
        timebucket_duration:
            Required Argument.
            Specifies the time unit duration of each timebucket for aggregation and is used to
            assign each potential timebucket a unique number.
            Permitted Values:
                 ===================================================================================================
                | Time Units        | Formal Form       | Shorthand Equivalents for time_units                      |
                 ===================================================================================================
                | Calendar Years    | CAL_YEARS(N)      | Ncy OR Ncyear OR Ncyears                                  |
                 ---------------------------------------------------------------------------------------------------
                | Calendar Months   | CAL_MONTHS(N)     | Ncm OR Ncmonth OR Ncmonths                                |
                 ---------------------------------------------------------------------------------------------------
                | Calendar Days     | CAL_DAYS(N)       | Ncd OR Ncday OR Ncdays                                    |
                 ---------------------------------------------------------------------------------------------------
                | Weeks             | WEEKS(N)          | Nw  OR Nweek OR Nweeks                                    |
                 ---------------------------------------------------------------------------------------------------
                | Days              | DAYS(N)           | Nd  OR Nday OR Ndays                                      |
                 ---------------------------------------------------------------------------------------------------
                | Hours             | HOURS(N)          | Nh  OR Nhr OR Nhrs OR Nhour OR Nhours                     |
                 ---------------------------------------------------------------------------------------------------
                | Minutes           | MINUTES(N)        | Nm  OR Nmins OR Nminute OR Nminutes                       |
                 ---------------------------------------------------------------------------------------------------
                | Seconds           | SECONDS(N)        | Ns  OR Nsec OR Nsecs OR Nsecond OR Nseconds               |
                 ---------------------------------------------------------------------------------------------------
                | Milliseconds      | MILLISECONDS(N)   | Nms OR Nmsec OR Nmsecs OR Nmillisecond OR Nmilliseconds   |
                 ---------------------------------------------------------------------------------------------------
                | Microseconds      | MICROSECONDS(N)   | Nus OR Nusec OR Nusecs OR Nmicrosecond OR Nmicroseconds   |
                 ===================================================================================================
                Where, N is a 16-bit positive integer with a maximum value of 32767.
                Notes:
                    1. When timebucket_duration is Calendar Days, it will group the columns in
                       24 hour periods starting at 00:00:00.000000 and ending at 23:59:59.999999 on
                       the day identified by time zero.
                    2. A DAYS time unit is a 24 hour span relative to any moment in time.
                       For example,
                            If time zero (in teradataml DataFraame created on PTI tables) equal to
                            2016-10-01 12:00:00, the day buckets are:
                                2016-10-01 12:00:00.000000 - 2016-10-02 11:59:59.999999.
                            This spans multiple calendar days, but encompasses one 24 hour period
                            representative of a day.
                    3. The time units do not store values such as the year or the month.
                       For example,
                            CAL_YEARS(2017) does not set the year to 2017. It sets the timebucket_duration
                            to intervals of 2017 years. Similarly, CAL_MONTHS(7) does not set the month to
                            July. It sets the timebucket_duration to intervals of 7 months.
            Types: str
            Example: MINUTES(23) which is equal to 23 Minutes
                     CAL_MONTHS(5) which is equal to 5 calendar months
        value_expression:
            Optional Argument.
            Specifies a column used for grouping purposes not related to time.
            Types: str or List of Strings
            Example: col1 or ["col1", "col2"]
        timecode_column:
            Optional Argument.
            Specifies a column that serves as the timecode for a non-PTI table. This is the column
            used for resampling time series data.
            For teradataml DataFrame created on PTI table:
                TD_TIMECODE is used implicitly for PTI tables, but can also be specified explicitly
                by the user with this parameter.
            For teradataml DataFrame created on non-PTI table:
                You must pass column name to this argument for teradataml DataFrame created on non-PTI table,
                otherwise an exception is raised.
        sequence_column:
            Optional Argument.
            Specifies a column that is the sequence number.
            For teradataml DataFrame created on PTI table:
                It can be TD_SEQNO or any other column that acts as a sequence number.
            For teradataml DataFrame created on non-PTI table:
                sequence_column is a column that plays the role of TD_SEQNO, because non-PTI
                tables do not have TD_SEQNO.
            Types: str
        fill:
            Optional Argument.
            Specifies values for missing timebucket values.
            Permitted values: NULLS, PREV / PREVIOUS, NEXT, and any numeric_constant
                NULLS:
                    The missing timebuckets are returned to the user with a null value for all
                    aggregate results.
                numeric_constant:
                    Any Teradata Database supported Numeric literal. The missing timebuckets
                    are returned to the user with the specified constant value for all
                    aggregate results. If the data type specified in the fill argument is
                    incompatible with the input data type for an aggregate function,
                    an error is reported.
                PREVIOUS/PREV:
                    The missing timebuckets are returned to the user with the aggregate results
                    populated by the value of the closest previous timebucket with a non-missing
                    value. If the immediate predecessor of a missing timebucket is also missing,
                    both buckets, and any other immediate predecessors with missing values,
                    are loaded with the first preceding non-missing value. If a missing
                    timebucket has no predecessor with a result (for example, if the
                    timebucket is the first in the series or all the preceding timebuckets in
                    the entire series are missing), the missing timebuckets are returned to the
                    user with a null value for all aggregate results. The abbreviation PREV may
                    be used instead of PREVIOUS.
                NEXT:
                    The missing timebuckets are returned to the user with the aggregate results populated
                    by the value of the closest succeeding timebucket with a non-missing value. If the
                    immediate successor of a missing timebucket is also missing, both buckets, and any
                    other immediate successors with missing values, are loaded with the first succeeding
                    non-missing value. If a missing timebucket has no successor with a result
                    (for example, if the timebucket is the last in the series or all the succeeding
                    timebuckets in the entire series are missing), the missing timebuckets are returned
                    to the user with a null value for all aggregate results.
            Types: str or int or float
    NOTES:
        1. This API is similar to resample().
        2. Users can still apply teradataml DataFrame methods (filters/sort/etc) on top of the result.
        3. Consecutive operations of grouping, i.e., groupby_time(), resample() and groupby() are not permitted.
           An exception will be raised. Following are some cases where exception will be raised as
           "Invalid operation applied, check documentation for correct usage."
                a. df.groupby_time().groupby()
                b. df.groupby_time().resample()
                c. df.groupby_time().groupby_time()
    RETURNS:
        teradataml DataFrameGroupBy Object
    RAISES:
        TypeError, ValueError, TeradataMLException
    EXAMPLES:
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_nonpti"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        >>> # DataFrame on NON-PTI table
        ... ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_nonpti.columns
        ['buoyid', 'timecode', 'temperature', 'salinity']
        >>> ocean_buoys_nonpti.head()
                                    buoyid  temperature  salinity
        timecode
        2014-01-06 08:09:59.999999       0         99.0        55
        2014-01-06 08:10:00.000000       0         10.0        55
        2014-01-06 09:01:25.122200       1         70.0        55
        2014-01-06 09:01:25.122200       1         77.0        55
        2014-01-06 09:02:25.122200       1         71.0        55
        2014-01-06 09:03:25.122200       1         72.0        55
        2014-01-06 09:02:25.122200       1         78.0        55
        2014-01-06 08:10:00.000000       0        100.0        55
        2014-01-06 08:08:59.999999       0          NaN        55
        2014-01-06 08:00:00.000000       0         10.0        55
        #
        # Example 1: Group by timebucket of 2 calendar years, using formal notation and buoyid column on
        #            DataFrame created on non-sequenced PTI table.
        #            Fill missing values with Nulls.
        #
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="CAL_YEARS(2)",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_grpby1.bottom(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  bottom2temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0                  10
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0                  10
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                  71
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                  70
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2                  80
        5  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2                  81
        6  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44                  43
        7  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44                  43
        >>>
        #
        # Example 2: Group by timebucket of 2 minutes, using shorthand notation to specify timebucket,
        #            on DataFrame created on non-PTI table. Fill missing values with Nulls.
        #            Time column must be specified for non-PTI table.
        #
        >>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.groupby_time(timebucket_duration="2m",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE",
        ...                                                                                    "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961       0                          10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962       0                           NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963       0                           NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964       0                           NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965       0                          99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                         100.0
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                          10.0
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                          70.0
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                          77.0
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992       1                          71.0
        >>>
        #
        # Example 3: Group by timebucket of 2 minutes, using shorthand notation to specify timebucket,
        #            on DataFrame created on non-PTI table. Fill missing values with previous values.
        #            Time column must be specified for non-PTI table.
        #
        >>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.groupby_time(timebucket_duration="2mins",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="prev")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE",
        ...                                                                                    "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961       0                            10
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962       0                            10
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963       0                            10
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964       0                            10
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965       0                            99
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                            10
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                           100
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                            77
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                            70
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992       1                            71
        #
        # Example 4: Group by timebucket of 2 minutes, using shorthand notation to specify timebucket,
        #            on DataFrame created on non-PTI table. Fill missing values with numeric constant 12345.
        #            Time column must be specified for non-PTI table.
        #
        >>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.groupby_time(timebucket_duration="2minute",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill=12345)
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE",
        ...                                                                                    "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961       0                            10
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962       0                         12345
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963       0                         12345
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964       0                         12345
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965       0                            99
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                            10
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                           100
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                            77
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                            70
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992       1                            71
        >>>
    head
    teradataml.dataframe.dataframe.DataFrame.head = head(self, n=10)
    DESCRIPTION:
        Print the first n rows of the sorted teradataml DataFrame.
        Note: The DataFrame is sorted on the index column or the first column if
        there is no index column. The column type must support sorting.
        Unsupported types: ['BLOB', 'CLOB', 'ARRAY', 'VARRAY']
    PARAMETERS:
        n:
            Optional Argument.
            Specifies the number of rows to select.
            Default Value: 10.
            Types: int
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame.from_table('admissions_train')
        >>> df
           masters   gpa     stats programming admitted
        id
        15     yes  4.00  Advanced    Advanced        1
        7      yes  2.33    Novice      Novice        1
        22     yes  3.46    Novice    Beginner        0
        17      no  3.83  Advanced    Advanced        1
        13      no  4.00  Advanced      Novice        1
        38     yes  2.65  Advanced    Beginner        1
        26     yes  3.57  Advanced    Advanced        1
        5       no  3.44    Novice      Novice        0
        34     yes  3.85  Advanced    Beginner        0
        40     yes  3.95    Novice    Beginner        0
        >>> df.head()
           masters   gpa     stats programming admitted
        id
        3       no  3.70    Novice    Beginner        1
        5       no  3.44    Novice      Novice        0
        6      yes  3.50  Beginner    Advanced        1
        7      yes  2.33    Novice      Novice        1
        9       no  3.82  Advanced    Advanced        1
        10      no  3.71  Advanced    Advanced        1
        8       no  3.60  Beginner    Advanced        1
        4      yes  3.50  Beginner      Novice        1
        2      yes  3.76  Beginner    Beginner        0
        1      yes  3.95  Beginner    Beginner        0
        >>> df.head(15)
           masters   gpa     stats programming admitted
        id
        3       no  3.70    Novice    Beginner        1
        5       no  3.44    Novice      Novice        0
        6      yes  3.50  Beginner    Advanced        1
        7      yes  2.33    Novice      Novice        1
        9       no  3.82  Advanced    Advanced        1
        10      no  3.71  Advanced    Advanced        1
        11      no  3.13  Advanced    Advanced        1
        12      no  3.65    Novice      Novice        1
        13      no  4.00  Advanced      Novice        1
        14     yes  3.45  Advanced    Advanced        0
        15     yes  4.00  Advanced    Advanced        1
        8       no  3.60  Beginner    Advanced        1
        4      yes  3.50  Beginner      Novice        1
        2      yes  3.76  Beginner    Beginner        0
        1      yes  3.95  Beginner    Beginner        0
        >>> df.head(5)
           masters   gpa     stats programming admitted
        id
        3       no  3.70    Novice    Beginner        1
        5       no  3.44    Novice      Novice        0
        4      yes  3.50  Beginner      Novice        1
        2      yes  3.76  Beginner    Beginner        0
        1      yes  3.95  Beginner    Beginner        0
    history
    teradataml.dataframe.dataframe.DataFrame.history
    DESCRIPTION:
        Gets the snapshot history related to a DataLake table.
    PARAMETERS:
        None
    RETURNS:
        teradataml DataFrame.
    RAISES:
        TeradataMLException.
    EXAMPLES :
        # Example 1: Get the partition information for datalake table.
        >>> from teradataml.dataframe.dataframe import in_schema
        >>> in_schema_tbl = in_schema(schema_name="datalake_db",
        ...                           table_name="datalake_table",
        ...                           datalake_name="datalake")
        >>> datalake_df = DataFrame(in_schema_tbl)
        >>> datalake_df.history
                             id                   timestamp
        0   8068130797628952520         2025-05-02 11:45:26
    iloc
    teradataml.dataframe.dataframe.DataFrame.iloc
    Access a group of rows and columns by integer values or a boolean array.
    VALID INPUTS:
        - A single integer values, e.g. 5.
        - A list or array of integer values, e.g. ``[1, 2, 3]``.
        - A slice object with integer values, e.g. ``1:6``.
          Note: The stop value is excluded.
        - A boolean array of the same length as the column axis for column access,
        Note: For integer indexing on row access, the integer index values are
        applied to a sorted teradataml DataFrame on the index column or the first column if
        there is no index column.
    RETURNS:
        teradataml DataFrame
    RAISE:
        TeradataMlException
    EXAMPLES
    --------
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame('sales')
        >>> df
                    Feb   Jan   Mar   Apr    datetime
        accounts
        Blue Inc     90.0    50    95   101  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        Jones LLC   200.0   150   140   180  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        # Retrieve row using a single integer.
        >>> df.iloc[1]
                Feb Jan Mar  Apr    datetime
        accounts
        Blue Inc  90.0  50  95  101  04/01/2017
        # List of integers. Note using ``[[]]``
        >>> df.iloc[[1, 2]]
                    Feb  Jan  Mar  Apr    datetime
        accounts
        Blue Inc    90.0   50   95  101  04/01/2017
        Jones LLC  200.0  150  140  180  04/01/2017
        # Single integer for row and column
        >>> df.iloc[5, 0]
        Empty DataFrame
        Columns: []
        Index: [Yellow Inc]
        # Single integer for row and column
        >>> df.iloc[5, 1]
            Feb
        0  90.0
        # Single integer for row and column access using a tuple
        >>> df.iloc[(5, 1)]
            Feb
        0  90.0
        # Slice for row and single integer for column access. As mentioned
        # above, note the stop for the slice is excluded.
        >>> df.iloc[2:5, 0]
        Empty DataFrame
        Columns: []
        Index: [Orange Inc, Jones LLC, Red Inc]
        # Slice for row and a single integer for column access. As mentioned
        # above, note the stop for the slice is excluded.
        >>> df.iloc[2:5, 2]
            Jan
        0  None
        1   150
        2   150
        # Slice for row and column access. As mentioned
        # above, note the stop for the slice is excluded.
        >>> df.iloc[2:5, 0:5]
                    Mar   Jan    Feb   Apr
        accounts
        Orange Inc  None  None  210.0   250
        Red Inc      140   150  200.0  None
        Jones LLC    140   150  200.0   180
        # Empty slice for row and column access.
        >>> df.iloc[:, :]
                      Feb   Jan   Mar   Apr    datetime
        accounts
        Blue Inc     90.0    50    95   101  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        Jones LLC   200.0   150   140   180  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        # List of integers and boolean array for column access
        >>> df.iloc[[0, 2, 3, 4], [True, True, False, False, True, True]]
                      Feb   Apr    datetime
        accounts
        Orange Inc  210.0   250  04/01/2017
        Red Inc     200.0  None  04/01/2017
        Jones LLC   200.0   180  04/01/2017
        Alpha Co    210.0   250  04/01/2017
    index
    teradataml.dataframe.dataframe.DataFrame.index
    DESCRIPTION:
        Returns the index_label of the teradataml DataFrame.
    RETURNS:
        str or List of Strings (str) representing the index_label of the DataFrame.
    RAISES:
        None
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        5       no  3.44    Novice      Novice         0
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        20     yes  3.90  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        25      no  3.96  Advanced    Advanced         1
        18     yes  3.81  Advanced    Advanced         1
        24      no  1.87  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        >>> # Get the index_label
        >>> df.index
        ['id']
        >>> # Set new index_label
        >>> df = df.set_index(['id', 'masters'])
        >>> df
                     gpa     stats programming  admitted
        id masters
        5  no       3.44    Novice      Novice         0
        3  no       3.70    Novice    Beginner         1
        1  yes      3.95  Beginner    Beginner         0
        17 no       3.83  Advanced    Advanced         1
        13 no       4.00  Advanced      Novice         1
        32 yes      3.46  Advanced    Beginner         0
        11 no       3.13  Advanced    Advanced         1
        9  no       3.82  Advanced    Advanced         1
        34 yes      3.85  Advanced    Beginner         0
        24 no       1.87  Advanced      Novice         1
        >>> # Get the index_label
        >>> df.index
        ['id', 'masters']
    info
    teradataml.dataframe.dataframe.DataFrame.info = info(self, verbose=True, buf=None, max_cols=None, null_counts=False)
    DESCRIPTION:
        Print a summary of the DataFrame.
    PARAMETERS:
        verbose:
            Optional Argument.
            Print full summary if True. Print short summary if False.
            Default Value: True
            Types: bool
        buf:
            Optional Argument.
            Speifies the writable buffer to send the output to.
            If argument is not specified, by default, the output is
            sent to sys.stdout.
        max_cols:
            Optional Argument.
            Speifies the maximum number of columns allowed for printing the
            full summary.
            Types: int
        null_counts:
            Optional Argument.
            Speifies whether to show the non-null counts.
            Display the counts if True, otherwise do not display the counts.
            Default Value: False
            Types: bool
    RETURNS:
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> df.info()
        <class 'teradataml.dataframe.dataframe.DataFrame'>
        Data columns (total 6 columns):
        accounts              str
        Feb                 float
        Jan                   int
        Mar                   int
        Apr                   int
        datetime    datetime.date
        dtypes: datetime.date(1), float(1), str(1), int(3)
        >>>
        >>> df.info(null_counts=True)
        <class 'teradataml.dataframe.dataframe.DataFrame'>
        Data columns (total 6 columns):
        accounts    6 non-null str
        Feb         6 non-null float
        Jan         4 non-null int
        Mar         4 non-null int
        Apr         4 non-null int
        datetime    6 non-null datetime.date
        dtypes: datetime.date(1), float(1), str(1), int(3)
        >>>
        >>> df.info(verbose=False)
        <class 'teradataml.dataframe.dataframe.DataFrame'>
        Data columns (total 6 columns):
        dtypes: datetime.date(1), float(1), str(1), int(3)
        >>>
    is_art
    teradataml.dataframe.dataframe.DataFrame.is_art
    RETURNS:
        Getter for self._is_art.
        Returns True if the DataFrame is created on an ART table, else return False.
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> df.is_art
            False
    itertuples
    teradataml.dataframe.dataframe.DataFrame.itertuples = itertuples(self, name='Row', num_rows=None)
    DESCRIPTION:
        Method to Iterate over teradataml DataFrame rows as namedtuples.
    PARAMETERS:
        name:
            Optional Argument.
            Specifies the name of the returned namedtuples. When set to None,
            the method returns the row in a list instead of namedtuple.
            Default Value: Row
            Types: str
        num_rows:
            Optional Argument.
            Specifies the number of rows to retrieve from the DataFrame.
            When set to None, the method retrieves all the records.
            Types: int OR NoneType
    RETURNS:
        iterator, an object to iterate over row in the DataFrame.
    RAISES:
        None
    EXAMPLES:
        # Load the data.
        >>> load_example_data("teradataml", "titanic")
        # Example 1: Retrieve all the records from the teradataml DataFrame.
        >>> df = DataFrame("titanic")
        >>> rows = df.itertuples()
        >>> next(rows)
        Row(passenger=78, survived=0, pclass=3, name='Moutal Mr. Rahamin Haim', sex='male', age=None, sibsp=0, parch=0, ticket='374
        >>> next(rows)
        Row(passenger=93, survived=0, pclass=1, name='Chaffee Mr. Herbert Fuller', sex='male', age=46, sibsp=1, parch=0, ticket='W.
        >>> next(rows)
        Row(passenger=5, survived=0, pclass=3, name='Allen Mr. William Henry', sex='male', age=35, sibsp=0, parch=0, ticket='373450
        >>>
        # Example 2: Retrieve the first 20 records from the teradataml DataFrame in a list,
        # when DataFrame is ordered by column "passenger".
        >>> df = DataFrame("titanic")
        >>> rows = df.sort("passenger").itertuples(name=None, num_rows=20)
        >>> next(rows)
        [1, 0, 3, 'Braund, Mr. Owen Harris', 'male', 22, 1, 0, 'A/5 21171', 7.25, None, 'S']
        >>> next(rows)
        [2, 1, 1, 'Cumings, Mrs. John Bradley (Florence Briggs Thayer)', 'female', 38, 1, 0, 'PC 17599', 71.2833, 'C85', 'C']
        >>> next(rows)
        [3, 1, 3, 'Heikkinen, Miss. Laina', 'female', 26, 0, 0, 'STON/O2. 3101282', 7.925, None, 'S']
    join
    teradataml.dataframe.dataframe.DataFrame.join = join(self, other, on=None, how='left', lsufﬁx=None, rsufﬁx=None, lpreﬁx=None, rpreﬁx=None)
    DESCRIPTION:
        Joins two different teradataml DataFrames together based on column comparisons
        specified in argument 'on' and type of join is specified in the argument 'how'.
        Supported join operations are:
        • Inner join: Returns only matching rows, non matching rows are eliminated.
        • Left outer join: Returns all matching rows plus non matching rows from the left table.
        • Right outer join: Returns all matching rows plus non matching rows from the right table.
        • Full outer join: Returns all rows from both tables, including non matching rows.
        • Cross join: Returns all rows from both tables where each row from the first table
                      is joined with each row from the second table. The result of the join
                      is a cartesian cross product.
                      Note: For a cross join, the 'on' argument is ignored.
        Supported join operators are =, ==, <, <=, >, >=, <> and != (= and <> operators are
        not supported when using DataFrame columns as operands).
        Notes:
            1.  When multiple join conditions are given as a list string/ColumnExpression,
                they are joined using AND operator.
            2.  Two or more on conditions can be combined using & and | operators
                and can be passed as single ColumnExpression.
                You can use (df1.a == df1.b) & (df1.c == df1.d) in place of
                [df1.a == df1.b, df1.c == df1.d].
            3.  Two or more on conditions can not be combined using pythonic 'and'
                and 'or'.
                You can use (df1.a == df1.b) & (df1.c == df1.d) in place of
                [df1.a == df1.b and df1.c == df1.d].
            4.  Performing self join using same DataFrame object in 'other'
                argument is not supported. In order to perform self join,
                first create aliased DataFrame using alias() API and pass it
                for 'other' argument. Refer to Example 10 in EXAMPLES section.
    PARAMETERS:
        other:
            Required Argument.
            Specifies the right teradataml DataFrame on which join is to be performed.
            Types: teradataml DataFrame
        on:
            Optional argument when "how" is "cross", otherwise required.
            If specified when "how" is "cross", it is ignored.
            Specifies list of conditions that indicate the columns to be join keys.
            It can take the following forms:
            • String comparisons, in the form of "col1 <= col2", where col1 is
              the column of left dataframe df1 and col2 is the column of right
              dataframe df2.
              Examples:
                1. ["a","b"] indicates df1.a = df2.a and df1.b = df2.b.
                2. ["a = b", "c == d"] indicates df1.a = df2.b and df1.c = df2.d.
                3. ["a <= b", "c > d"] indicates df1.a <= df2.b and df1.c > df2.d.
                4. ["a < b", "c >= d"] indicates df1.a < df2.b and df1.c >= df2.d.
                5. ["a <> b"] indicates df1.a != df2.b. Same is the case for ["a != b"].
            • Column comparisons, in the form of df1.col1 <= df2.col2, where col1
              is the column of left dataframe df1 and col2 is the column of right
              dataframe df2.
              Examples:
                1. [df1.a == df2.a, df1.b == df2.b] indicates df1.a = df2.a AND df1.b = df2.b.
                2. [df1.a == df2.b, df1.c == df2.d] indicates df1.a = df2.b AND df1.c = df2.d.
                3. [df1.a <= df2.b & df1.c > df2.d] indicates df1.a <= df2.b AND df1.c > df2.d.
                4. [df1.a < df2.b | df1.c >= df2.d] indicates df1.a < df2.b OR df1.c >= df2.d.
                5. df1.a != df2.b indicates df1.a != df2.b.
            • The combination of both string comparisons and comparisons as column expressions.
              Examples:
                1. ["a", df1.b == df2.b] indicates df1.a = df2.a AND df1.b = df2.b.
                2. [df1.a <= df2.b, "c > d"] indicates df1.a <= df2.b AND df1.c > df2.d.
            • ColumnExpressions containing FunctionExpressions which represent SQL functions
              invoked on DataFrame Columns.
              Examples:
                1. (df1.a.round(1) - df2.a.round(1)).mod(2.5) > 2
                2. df1.a.floor() - df2.b.floor() > 2
            Types: str (or) ColumnExpression (or) List of strings(str) or ColumnExpressions
        how:
            Optional Argument.
            Specifies the type of join to perform.
            Default value is "left".
            Permitted Values : "inner", "left", "right", "full" and "cross"
            Types: str
        lsuffix:
            Optional Argument.
            Specifies the suffix to be added to the left table columns.
            Default Value: None.
            Types: str
        rsuffix:
            Optional Argument.
            Specifies the suffix to be added to the right table columns.
            Default Value: None.
            Types: str
        lprefix:
            Optional Argument.
            Specifies the prefix to be added to the left table columns.
            Default Value: None.
            Types: str
        rprefix:
            Optional Argument.
            Specifies the prefix to be added to the right table columns.
            Default Value: None.
            Types: str
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        >>> from teradataml import load_example_data
        >>> load_example_data("dataframe", ["join_table1", "join_table2"])
        >>> load_example_data("glm", "admissions_train") # used in cross join
        >>> df1 = DataFrame("join_table1")
        >>> df2 = DataFrame("join_table2")
        # Print dataframe.
        >>> df1
                   col2  col3 col5
        col1
        2     analytics   2.3    b
        1      teradata   1.3    a
        3      platform   3.3    c
        # Print dataframe.
        >>> df2
                   col4  col3 col7
        col1
        2     analytics   2.3    b
        1      teradata   1.3    a
        3       are you   4.3    d
        # Example 1: Both "on" argument conditions as strings.
        >>> df1.join(other = df2, on = ["col2=col4", "col1"], how = "inner", lprefix = "t1", rprefix = "t2")
          t1_col1 t2_col1       col2  t1_col3  t2_col3 col5       col4 col7
        0       2       2  analytics      2.3      2.3    b  analytics    b
        1       1       1   teradata      1.3      1.3    a   teradata    a
        # Example 2: One "on" argument condition is ColumnExpression and other is string having two
        #            columns with left outer join.
        >>> df1.join(df2, on = [df1.col2 == df2.col4,"col5 = col7"], how = "left", lprefix = "t1", rprefix = "t2")
          t1_col1 t2_col1       col2  t1_col3  t2_col3 col5       col4  col7
        0       3    None   platform      3.3      None    c       None  None
        1       2       2  analytics      2.3      2.3    b  analytics     b
        2       1       1   teradata      1.3      1.3    a   teradata     a
        # Example 3: One "on" argument condition is ColumnExpression and other is string having only one column.
        >>> df1.join(other = df2, on = [df1.col2 == df2.col4,"col3"], how = "inner", lprefix = "t1", rprefix = "t2")
          t1_col1 t2_col1       col2  t1_col3  t2_col3 col5       col4 col7
        0       2       2  analytics      2.3      2.3    b  analytics    b
        1       1       1   teradata      1.3      1.3    a   teradata    a
        # Example 4: One "on" argument condition is ColumnExpression and other is string having two
        #            columns with full join.
        >>> df1.join(other = df2, on = ["col2=col4",df1.col5 == df2.col7], how = "full", lprefix = "t1", rprefix = "t2")
          t1_col1 t2_col1       col2  t1_col3  t2_col3  col5       col4  col7
        0       3    None   platform      3.3      None     c       None  None
        1    None       3       None      None      4.3  None    are you     d
        2       1       1   teradata      1.3      1.3     a   teradata     a
        3       2       2  analytics      2.3      2.3     b  analytics     b
        # Example 5: Using not equal operation in ColumnExpression condition.
        >>> df1.join(other = df2, on = ["col5==col7",df1.col2 != df2.col4], how = "full", lprefix = "t1", rprefix = "t2")
          t1_col1 t2_col1       col2  t1_col3  t2_col3   col5       col4  col7
        0       1    None   teradata      1.3      None     a       None  None
        1       2    None  analytics      2.3      None     b       None  None
        2    None       2       None      None      2.3  None  analytics     b
        3    None       1       None      None      1.3  None   teradata     a
        4       3    None   platform      3.3      None     c       None  None
        5    None       3       None      None      4.3  None    are you     d
        # Example 6: Using only one string expression with <> operation.
        >>> df1.join(other = df2, on = "col2<>col4", how = "left", lprefix = "t1", rprefix = "t2")
          t1_col1 t2_col1       col2  t1_col3  t2_col3 col5       col4 col7
        0       2       3  analytics      2.3      4.3    b    are you    d
        1       2       1  analytics      2.3      1.3    b   teradata    a
        2       3       2   platform      3.3      2.3    c  analytics    b
        3       1       2   teradata      1.3      2.3    a  analytics    b
        4       3       1   platform      3.3      1.3    c   teradata    a
        5       1       3   teradata      1.3      4.3    a    are you    d
        6       3       3   platform      3.3      4.3    c    are you    d
        # Example 7: Using only one ColumnExpression in "on" argument conditions.
        >>> df1.join(other = df2, on = df1.col5 != df2.col7, how = "full", lprefix = "t1", rprefix = "t2")
          t1_col1 t2_col1       col2  t1_col3  t2_col3 col5       col4 col7
        0       1       3   teradata      1.3      4.3    a    are you    d
        1       3       1   platform      3.3      1.3    c   teradata    a
        2       1       2   teradata      1.3      2.3    a  analytics    b
        3       3       2   platform      3.3      2.3    c  analytics    b
        4       2       1  analytics      2.3      1.3    b   teradata    a
        5       3       3   platform      3.3      4.3    c    are you    d
        6       2       3  analytics      2.3      4.3    b    are you    d
        # Example 8: Both "on" argument conditions as ColumnExpressions.
        >>> df1.join(df2, on = [df1.col2 == df2.col4, df1.col5 > df2.col7], how = "right", lprefix = "t1", rprefix ="t2")
          t1_col1 t2_col1  col2 t1_col3  t2_col3  col5       col4 col7
        0    None       2  None    None      2.3  None  analytics    b
        1    None       1  None    None      1.3  None   teradata    a
        2    None       3  None    None      4.3  None    are you    d
        # Example 9: cross join "admissions_train" with "admissions_train".
        >>> df1 = DataFrame("admissions_train").head(3).sort("id")
        >>> print(df1)
           masters   gpa     stats programming admitted
        id
        1      yes  3.95  Beginner    Beginner        0
        2      yes  3.76  Beginner    Beginner        0
        3       no  3.70    Novice    Beginner        1
        >>> df2 = DataFrame("admissions_train").head(3).sort("id")
        >>> print(df2)
           masters   gpa     stats programming admitted
        id
        1      yes  3.95  Beginner    Beginner        0
        2      yes  3.76  Beginner    Beginner        0
        3       no  3.70    Novice    Beginner        1
        >>> df3 = df1.join(other=df2, how="cross", lprefix="l", rprefix="r")
        >>> df3.set_index("l_id").sort("l_id")
             l_programming   r_stats r_id  r_gpa r_programming  l_gpa   l_stats r_admitted l_admitted l_masters r_masters
        l_id
        1         Beginner    Novice    3   3.70      Beginner   3.95  Beginner          1          0       yes        no
        1         Beginner  Beginner    1   3.95      Beginner   3.95  Beginner          0          0       yes       yes
        1         Beginner  Beginner    2   3.76      Beginner   3.95  Beginner          0          0       yes       yes
        2         Beginner  Beginner    1   3.95      Beginner   3.76  Beginner          0          0       yes       yes
        2         Beginner  Beginner    2   3.76      Beginner   3.76  Beginner          0          0       yes       yes
        2         Beginner    Novice    3   3.70      Beginner   3.76  Beginner          1          0       yes        no
        3         Beginner  Beginner    1   3.95      Beginner   3.70    Novice          0          1        no       yes
        3         Beginner  Beginner    2   3.76      Beginner   3.70    Novice          0          1        no       yes
        3         Beginner    Novice    3   3.70      Beginner   3.70    Novice          1          1        no        no
        # Example 10: Perform self join using aliased DataFrame.
        # Create an aliased DataFrame.
        >>> lhs  = DataFrame("admissions_train").head(3).sort("id")
        >>> rhs = lhs.alias("rhs")
        # Use aliased DataFrame for self join.
        >>> joined_df = lhs.join(other=rhs, how="cross", lprefix="l", rprefix="r")
        >>> joined_df
           l_id  r_id l_masters r_masters  l_gpa  r_gpa   l_stats   r_stats l_programming r_programming  l_admitted  r_admitted
        0     1     3       yes        no   3.95   3.70  Beginner    Novice      Beginner      Beginner           0           1
        1     2     2       yes       yes   3.76   3.76  Beginner  Beginner      Beginner      Beginner           0           0
        2     2     3       yes        no   3.76   3.70  Beginner    Novice      Beginner      Beginner           0           1
        3     3     1        no       yes   3.70   3.95    Novice  Beginner      Beginner      Beginner           1           0
        4     3     3        no        no   3.70   3.70    Novice    Novice      Beginner      Beginner           1           1
        5     3     2        no       yes   3.70   3.76    Novice  Beginner      Beginner      Beginner           1           0
        6     2     1       yes       yes   3.76   3.95  Beginner  Beginner      Beginner      Beginner           0           0
        7     1     2       yes       yes   3.95   3.76  Beginner  Beginner      Beginner      Beginner           0           0
        8     1     1       yes       yes   3.95   3.95  Beginner  Beginner      Beginner      Beginner           0           0
        # Example 11: Perform join with compound 'on' condition having
        #             more than one binary operator.
        >>> rhs_2 = lhs.assign(double_gpa=lhs.gpa * 2)
        >>> joined_df_2 = lhs.join(rhs_2, on=rhs_2.double_gpa == lhs.gpa * 2, how="left", lprefix="l", rprefix="r")
        >>> joined_df_2
           l_id  r_id l_masters r_masters  l_gpa  r_gpa   l_stats   r_stats l_programming r_programming  l_admitted  r_admitted  do
        0     3     3        no        no   3.70   3.70    Novice    Novice      Beginner      Beginner           1           1    
        1     2     2       yes       yes   3.76   3.76  Beginner  Beginner      Beginner      Beginner           0           0    
        2     1     1       yes       yes   3.95   3.95  Beginner  Beginner      Beginner      Beginner           0           0    
        # Example 12: Perform join on DataFrames with 'on' condition
        #             having FunctionExpression.
        >>> df = DataFrame("admissions_train")
        >>> df2 = df.alias("rhs_df")
        >>> joined_df_3 = df.join(df2, on=(df.gpa.round(1) - df2.gpa.round(1)).mod(2.5) > 2,
        >>>                       how="inner", lprefix="l")
        >>> joined_df_3.sort(["id", "l_id"])
            l_id    id      l_masters       masters  l_gpa   gpa     l_stats           stats  l_programming programming     l_admit
        0      1    24            yes            no   3.95  1.87    Beginner        Advanced           Beginner          Novice    
        1     13    24             no            no    4.0  1.87    Advanced        Advanced             Novice          Novice    
        2     15    24            yes            no    4.0  1.87    Advanced        Advanced           Advanced          Novice    
        3     25    24             no            no   3.96  1.87    Advanced        Advanced           Advanced          Novice    
        4     27    24            yes            no   3.96  1.87    Advanced        Advanced           Advanced          Novice    
        5     29    24            yes            no    4.0  1.87      Novice        Advanced           Beginner          Novice    
        6     40    24           yes             no   3.95  1.87      Novice        Advanced           Beginner          Novice    
    keys
    teradataml.dataframe.dataframe.DataFrame.keys = keys(self)
    RETURNS:
        a list containing the column names
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> df.keys()
        ['accounts', 'Feb', 'Jan', 'Mar', 'Apr', 'datetime']
    kurtosis
    teradataml.dataframe.dataframe.DataFrame.kurtosis = kurtosis(self, distinct=False)
    DESCRIPTION:
        Returns column-wise kurtosis value of the dataframe.
        Kurtosis is the fourth moment of the distribution of the standardized (z) values.
        It is a measure of the outlier (rare, extreme observation) character of the distribution as
        compared with the normal (or Gaussian) distribution.
            * The normal distribution has a kurtosis of 0.
            * Positive kurtosis indicates that the distribution is more outlier-prone than the
              normal distribution.
            * Negative kurtosis indicates that the distribution is less outlier-prone than the
              normal distribution.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
            3. Following conditions will produce null result:
                a. Fewer than three non-null data points in the data used for the computation.
                b. Standard deviation for a column is equal to 0.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the kurtosis value.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with kurtosis()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If kurtosis() operation fails to
           generate the column-wise kurtosis value of the dataframe.
           Possible error message:
           Failed to perform 'kurtosis'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the kurtosis() operation
           doesn't support all the columns in the dataframe.
           Possible error message:
           No results. Below is/are the error message(s):
           All selected columns [(col2 -  PERIOD_TIME), (col3 -
           BLOB)] is/are unsupported for 'kurtosis' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["admissions_train"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("admissions_train")
        >>> print(df1.sort("id"))
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        2      yes  3.76  Beginner    Beginner         0
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        8       no  3.60  Beginner    Advanced         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        >>>
        # Prints kurtosis value of each column
        >>> df1.kurtosis()
           kurtosis_id  kurtosis_gpa  kurtosis_admitted
        0         -1.2      4.052659            -1.6582
        >>>
        #
        # Using kurtosis() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        #
        # Time Series Aggregate Example 1: Executing kurtosis() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the kurtosis values.
        #
        # To use kurtosis() as Time Series Aggregate we must run groupby_time() first, followed by kurtosis().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.kurtosis().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid kurtosis_salinity  kurtosis_tempe
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0              None             -5.998128
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1              None             -2.758377
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2              None                   NaN
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44              None             -2.195395
        >>>
        #
        # Time Series Aggregate Example 2: Executing kurtosis() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the kurtosis value.
        #
        # To use kurtosis() as Time Series Aggregate we must run groupby_time() first, followed by kurtosis().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.kurtosis(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid kurtosis_salinity  kurtosis_tempe
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0              None                   NaN
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1              None             -2.758377
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2              None                   NaN
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44              None              4.128426
        >>>
    loc
    teradataml.dataframe.dataframe.DataFrame.loc
    Access a group of rows and columns by label(s) or a boolean array.
    VALID INPUTS:
        - A single label, e.g. ``5`` or ``'a'``, (note that ``5`` is
        interpreted as a label of the index, it is not interpreted as an
        integer position along the index).
        - A list or array of column or index labels, e.g. ``['a', 'b', 'c']``.
        - A slice object with labels, e.g. ``'a':'f'``.
        Note that unlike the usual python slices where the stop index is not included, both the
            start and the stop are included
        - A conditional expression for row access.
        - A boolean array of the same length as the column axis for column access.
    RETURNS:
        teradataml DataFrame
    RAISE:
        TeradataMlException
    EXAMPLES
    --------
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame('sales')
        >>> df
                    Feb   Jan   Mar   Apr    datetime
        accounts
        Blue Inc     90.0    50    95   101  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        Jones LLC   200.0   150   140   180  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        # Retrieve row using a single label.
        >>> df.loc['Blue Inc']
                Feb Jan Mar  Apr    datetime
        accounts
        Blue Inc  90.0  50  95  101  04/01/2017
        # List of labels. Note using ``[[]]``
        >>> df.loc[['Blue Inc', 'Jones LLC']]
                    Feb  Jan  Mar  Apr    datetime
        accounts
        Blue Inc    90.0   50   95  101  04/01/2017
        Jones LLC  200.0  150  140  180  04/01/2017
        # Single label for row and column (index)
        >>> df.loc['Yellow Inc', 'accounts']
        Empty DataFrame
        Columns: []
        Index: [Yellow Inc]
        # Single label for row and column
        >>> df.loc['Yellow Inc', 'Feb']
            Feb
        0  90.0
        # Single label for row and column access using a tuple
        >>> df.loc[('Yellow Inc', 'Feb')]
            Feb
        0  90.0
        # Slice with labels for row and single label for column. As mentioned
        # above, note that both the start and stop of the slice are included.
        >>> df.loc['Jones LLC':'Red Inc', 'accounts']
        Empty DataFrame
        Columns: []
        Index: [Orange Inc, Jones LLC, Red Inc]
        # Slice with labels for row and single label for column. As mentioned
        # above, note that both the start and stop of the slice are included.
        >>> df.loc['Jones LLC':'Red Inc', 'Jan']
            Jan
        0  None
        1   150
        2   150
        # Slice with labels for row and labels for column. As mentioned
        # above, note that both the start and stop of the slice are included.
        >>> df.loc['Jones LLC':'Red Inc', 'accounts':'Apr']
                    Mar   Jan    Feb   Apr
        accounts
        Orange Inc  None  None  210.0   250
        Red Inc      140   150  200.0  None
        Jones LLC    140   150  200.0   180
        # Empty slice for row and labels for column.
        >>> df.loc[:, :]
                    Feb   Jan   Mar    datetime   Apr
        accounts
        Jones LLC   200.0   150   140  04/01/2017   180
        Blue Inc     90.0    50    95  04/01/2017   101
        Yellow Inc   90.0  None  None  04/01/2017  None
        Orange Inc  210.0  None  None  04/01/2017   250
        Alpha Co    210.0   200   215  04/01/2017   250
        Red Inc     200.0   150   140  04/01/2017  None
        # Conditional expression
        >>> df.loc[df['Feb'] > 90]
                    Feb   Jan   Mar   Apr    datetime
        accounts
        Jones LLC   200.0   150   140   180  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        # Conditional expression with column labels specified
        >>> df.loc[df['Feb'] > 90, ['accounts', 'Jan']]
                    Jan
        accounts
        Jones LLC    150
        Red Inc      150
        Alpha Co     200
        Orange Inc  None
        # Conditional expression with multiple column labels specified
        >>> df.loc[df['accounts'] == 'Jones LLC', ['accounts', 'Jan', 'Feb']]
                Jan    Feb
        accounts
        Jones LLC  150  200.0
        # Conditional expression and slice with column labels specified
        >>> df.loc[df['accounts'] == 'Jones LLC', 'accounts':'Mar']
                Mar  Jan    Feb
        accounts
        Jones LLC  140  150  200.0
        # Conditional expression and boolean array for column access
        >>> df.loc[df['Feb'] > 90, [True, True, False, False, True, True]]
                  Feb   Apr    datetime
        accounts
        Alpha Co    210.0   250  04/01/2017
        Jones LLC   200.0   180  04/01/2017
        Red Inc     200.0  None  04/01/2017
        Orange Inc  210.0   250  04/01/2017
        >>>
    manifests
    teradataml.dataframe.dataframe.DataFrame.manifests
    DESCRIPTION:
        Gets manifest information for a DataLake table.
    PARAMETERS:
        None
    RETURNS:
        teradataml DataFrame.
    RAISES:
        TeradataMLException.
    EXAMPLES :
        # Example 1: Get the manifest information for datalake table.
        >>> from teradataml.dataframe.dataframe import in_schema
        >>> in_schema_tbl = in_schema(schema_name="datalake_db",
        ...                           table_name="datalake_table",
        ...                           datalake_name="datalake")
        >>> datalake_df = DataFrame(in_schema_tbl)
        >>> datalake_df.manifests
                     snapshotId         snapshotTimestamp                                 manifestList                             
        0   8068130797628952520       2025-05-02 11:45:26   s3://vim-iceberg-v1/otftestdb/nt_sales/...      s3://vim-iceberg-
    v1/otftestdb/nt_sales/...                    7158                  6               6
    map_partition
    teradataml.dataframe.dataframe.DataFrame.map_partition = map_partition(self, user_function, exec_mode='IN-DB', chunk_size=1000, num_rows=1000,
    data_partition_column=None, data_hash_column=None, **kwargs)
            DESCRIPTION:
                Function to apply a user defined function to a group or partition of rows
                in the teradataml DataFrame, leveraging Vantage's Script Table Operator.
                Notes:
                    1. The function requires to use same Python version in both Vantage and local environment.
                    2. Teradata recommends to use "dill" package with same version in both Vantage and
                       local environment.
            PARAMETERS:
                user_function:
                    Required Argument.
                    Specifies the user defined function to apply to each group or partition of
                    rows in the teradataml DataFrame.
                    Types: function or functools.partial
                    Notes:
                        * This can be either a lambda function, a regular python
                          function, or an object of functools.partial.
                        * The first argument (positional) to the user defined
                          function must be an iterator on the partition of rows
                          from the teradataml DataFrame represented as a pandas
                          DataFrame to which it is to be applied.
                        * A non-lambda function can be passed only when the user
                          defined function does not accept any arguments other than
                          the mandatory input - the iterator on the partition of
                          rows.
                          A user can also use functools.partial and lambda functions
                          for the same, which are especially handy when:
                              * there is a need to pass positional and/or keyword
                                arguments (lambda).
                              * there is a need to pass keyword arguments only
                                (functool.partial).
                        * The return type of the user defined function must be one
                          of the following:
                              * numpy ndarray
                                  * For a one-dimensional array, it is expected that
                                    it has as many values as the number of expected
                                    output columns.
                                  * For a two-dimensional array, it is expected that
                                    every array contained in the outer array has as
                                    many values as the number of expected output
                                    columns.
                              * pandas Series
                                    This represents a row in the output, and the
                                    number of values in it must be the same as the
                                    number of expected output columns.
                              * pandas DataFrame
                                    It is expected that a pandas DataFrame returned
                                    by the "user_function" has the same number of
                                    columns as the number of expected output columns.
                        * The return objects will be printed to the standard output
                          as required by Script using the 'quotechar' and 'delimiter'
                          values.
                        * The user function can also print the required output to
                          the standard output in the delimited (and possibly quoted)
                          format instead of returning an object of supported type.
                exec_mode:
                    Optional Argument.
                    Specifies the mode of execution for the user defined function.
                    Permitted values:
                        * IN-DB: Execute the function on data in the teradataml
                                 DataFrame in Vantage.
                        * LOCAL: Execute the function locally on sample data (at
                                 most "num_rows" rows) from the teradataml
                                 DataFrame.
                    Default value: 'IN-DB'
                    Types: str
                chunk_size:
                    Optional Argument.
                    Specifies the number of rows to be read in a chunk in each
                    iteration using the iterator that will be passed to the user
                    defined function.
                    Varying the value passed to this argument affects the
                    performance and the memory utilization.
                    Default value: 1000
                    Types: int
                num_rows:
                    Optional Argument.
                    Specifies the maximum number of sample rows to use from the
                    teradataml DataFrame to apply the user defined function to when
                    "exec_mode" is 'LOCAL'.
                    Default value: 1000
                    Types: int
                data_partition_column:
                    Optional Argument.
                    Specifies the Partition By columns for the teradataml DataFrame.
                    Values to this argument can be provided as a list, if multiple
                    columns are used for partition.
                    Types: str OR list of Strings (str)
                    Note:
                        * "data_partition_column" cannot be specified along with
                          "data_hash_column".
                        * "data_partition_column" cannot be specified along with
                          "is_local_order = True".
                data_hash_column:
                    Optional Argument.
                    Specifies the column to be used for hashing.
                    The rows in the teradataml DataFrame are redistributed to AMPs
                    based on the hash value of the column specified.
                    The "user_function" then runs once on each AMP.
                    If there is no "data_partition_column", then the entire result
                    set, delivered by the function, constitutes a single group or
                    partition.
                    Types: str
                    Note:
                        * "data_hash_column" cannot be specified along with
                          "data_partition_column".
                        * "is_local_order" must be set to 'True' when
                          "data_order_column" is used with "data_hash_column".
                returns:
                    Optional Argument.
                    Specifies the output column definition corresponding to the
                    output of "user_function".
                    When not specified, the function assumes that the names and
                    types of the output columns are same as that of the input.
                    Note:
                        Teradata reserved keywords should not be provided as column names unless
                        the column names of output dataframe are an exact match of input dataframe.
                        User can find the list or check if the string is a reserved keyword or not
                        using list_td_reserved_keywords() function.
                    Types: Dictionary specifying column name to
                           teradatasqlalchemy type mapping.
                delimiter:
                    Optional Argument.
                    Specifies a delimiter to use when reading columns from a row and
                    writing result columns.
                    Default value: '        '
                    Types: str with one character
                    Notes:
                        * This argument cannot be same as "quotechar" argument.
                        * This argument cannot be a newline character i.e., '
    '.
                quotechar:
                    Optional Argument.
                    Specifies a character that forces all input and output of the
                    user function to be quoted using this specified character.
                    Using this argument enables the Advanced SQL Engine to
                    distinguish between NULL fields and empty strings. A string with
                    length zero is quoted, while NULL fields are not.
                    If this character is found in the data, it will be escaped by a
                    second quote character.
                    Types: str with one character
                    Notes:
                        * This argument cannot be same as "delimiter" argument.
                        * This argument cannot be a newline character i.e., '
    '.
                auth:
                    Optional Argument.
                    Specifies an authorization to use when running the
                    "user_function".
                    Types: str
                charset:
                    Optional Argument.
                    Specifies the character encoding for data.
                    Permitted values: 'utf-16', 'latin'
                    Types: str
                data_order_column:
                    Optional Argument.
                    Specifies the Order By columns for the teradataml DataFrame.
                    Values to this argument can be provided as a list, if multiple
                    columns are used for ordering.
                    This argument is used in both cases:
                    "is_local_order = True" and "is_local_order = False".
                    Types: str OR list of Strings (str)
                    Note:
                        "is_local_order" must be set to 'True' when
                        "data_order_column" is used with "data_hash_column".
                is_local_order:
                    Optional Argument.
                    Specifies a boolean value to determine whether the input data
                    is to be ordered locally or not.
                    "data_order_column" with "is_local_order" set to 'False'
                    specifies the order in which the values in a group, or
                    partition, are sorted.
                    When this argument is set to 'True', qualified rows on each AMP
                    are ordered in preparation to be input to a table function.
                    This argument is ignored, if "data_order_column" is None.
                    Default value: False
                    Types: bool
                    Notes:
                        * "is_local_order" cannot be specified along with
                          "data_partition_column".
                        * When "is_local_order" is set to True, "data_order_column"
                          should be specified, and the columns specified in
                          "data_order_column" are used for local ordering.
                nulls_first:
                    Optional Argument.
                    Specifies a boolean value to determine whether NULLS are listed
                    first or last during ordering.
                    This argument is ignored, if "data_order_column" is None.
                    NULLS are listed first when this argument is set to 'True', and
                    last when set to 'False'.
                    Default value: True
                    Types: bool
                sort_ascending:
                    Optional Argument.
                    Specifies a boolean value to determine if the result set is to
                    be sorted on the "data_order_column" column in ascending or
                    descending order.
                    The sorting is ascending when this argument is set to 'True',
                    and descending when set to 'False'.
                    This argument is ignored, if "data_order_column" is None.
                    Default Value: True
                    Types: bool
                debug:
                    Optional Argument.
                    Specifies whether to display the script file path generated during function execution or not. This
                    argument helps in debugging when there are any failures during function execution. When set
                    to True, function displays the path of the script and does not remove the file from local file system.
                    Otherwise, file is removed from the local file system.
                    Default Value: False
                    Types: bool
            RETURNS:
                1. teradataml DataFrame if exec_mode is "IN-DB".
                2. Pandas DataFrame if exec_mode is "LOCAL".
            RAISES:
                 TypeError, TeradataMlException.
            EXAMPLES:
                >>> # This example uses the 'admissions_train' dataset, calculates
                >>> # the average 'gpa' per partition based on the value in
                >>> # 'admitted' column.
                >>> # Load the example data.
                >>> load_example_data("dataframe", "admissions_train")
                >>> df = DataFrame('admissions_train')
                >>> print(df)
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
                >>> # Example 1:
                >>> # Create the user defined function to calculate the average
                >>> # 'gpa', by reading data in chunks.
                >>> # Note that the function accepts a TextFileReader object to
                >>> # iterate on data in chunks.
                >>> # The return type of the function is a numpy ndarray.
                >>>
                >>> from numpy import asarray
                >>> def grouped_gpa_avg(rows):
                ...     admitted = None
                ...     row_count = 0
                ...     gpa = 0
                ...     for chunk in rows:
                ...         for _, row in chunk.iterrows():
                ...             row_count += 1
                ...             gpa += row['gpa']
                ...             if admitted is None:
                ...                 admitted = row['admitted']
                ...     if row_count > 0:
                ...         return asarray([admitted, gpa/row_count])
                ...
                >>>
                >>> # Apply the user defined function to the DataFrame.
                >>> from teradatasqlalchemy.types import INTEGER, FLOAT
                >>> returns = OrderedDict([('admitted', INTEGER()),
                ...                        ('avg_gpa', FLOAT())])
                >>> avg_gpa_1 = df.map_partition(grouped_gpa_avg,
                ...                              returns = returns,
                ...                              data_partition_column = 'admitted')
                >>>
                >>> # Print the result.
                >>> print(avg_gpa_1)
                   admitted   avg_gpa
                0         1  3.533462
                1         0  3.557143
                >>> # Example 2:
                >>> # Create the user defined function to calculate the average
                >>> # 'gpa', by reading data at once into a pandas DataFrame.
                >>> # Note that the function accepts a TextFileReader object to
                >>> # iterate on data in chunks.
                >>> # The return type of the function is a pandas Series.
                >>> def grouped_gpa_avg_2(rows):
                ...     pdf = rows.read()
                ...     if pdf.shape[0] > 0:
                ...         return pdf[['admitted', 'gpa']].mean()
                ...
                >>> avg_gpa_2 = df.map_partition(grouped_gpa_avg_2,
                ...                              returns = returns,
                ...                              data_partition_column = 'admitted')
                >>>
                >>> print(avg_gpa_2)
                   admitted   avg_gpa
                0         0  3.557143
                1         1  3.533462
                >>> # Example 3:
                >>> # The following example demonstrates using a lambda function to
                >>> # achieve the same result.
                >>> # Note that the the function is written to accept an iterator
                >>> # (TextFileReader object), and return the result which is of
                >>> # type pandas Series.
                >>> avg_gpa_3 = df.map_partition(lambda rows: grouped_gpa_avg(rows),
                ...                              returns = returns,
                ...                              data_partition_column = 'admitted')
                >>>
                >>> print(avg_gpa_3)
                   admitted   avg_gpa
                0         0  3.557143
                1         1  3.533462
                >>> # Example 4:
                >>> # The following example demonstrates using a function that
                >>> # returns the input data.
                >>> # Note that the the function is written to accept an iterator
                >>> # (TextFileReader object), and returns the result which is of
                >>> # type pandas DataFrame.
                >>> def echo(rows):
                ...     pdf = rows.read()
                ...     if pdf is not None:
                ...         return pdf
                ...
                >>> echo_out = df.map_partition(echo,
                ...                             data_partition_column = 'admitted')
                >>> print(echo_out)
                   masters   gpa     stats programming  admitted
                id
                5       no  3.44    Novice      Novice         0
                7      yes  2.33    Novice      Novice         1
                22     yes  3.46    Novice    Beginner         0
                19     yes  1.98  Advanced    Advanced         0
                15     yes  4.00  Advanced    Advanced         1
                17      no  3.83  Advanced    Advanced         1
                34     yes  3.85  Advanced    Beginner         0
                13      no  4.00  Advanced      Novice         1
                36      no  3.00  Advanced      Novice         0
                40     yes  3.95    Novice    Beginner         0
    map_row
    teradataml.dataframe.dataframe.DataFrame.map_row = map_row(self, user_function, exec_mode='IN-DB', chunk_size=1000, num_rows=1000, **kwargs)
            DESCRIPTION:
                Function to apply a user defined function to each row in the
                teradataml DataFrame, leveraging Vantage's Script Table Operator.
                Notes:
                    1. The function requires to use same Python version in both Vantage and local environment.
                    2. Teradata recommends to use "dill" package with same version in both Vantage and
                       local environment.
            PARAMETERS:
                user_function:
                    Required Argument.
                    Specifies the user defined function to apply to each row in
                    the teradataml DataFrame.
                    Types: function or functools.partial
                    Notes:
                        * This can be either a lambda function, a regular python
                          function, or an object of functools.partial.
                        * The first argument (positional) to the user defined
                          function must be a row in a pandas DataFrame corresponding
                          to the teradataml DataFrame to which it is to be applied.
                        * A non-lambda function can be passed only when the user
                          defined function does not accept any arguments other than
                          the mandatory input - the input row.
                          A user can also use functools.partial and lambda functions
                          for the same, which are especially handy when:
                              * there is a need to pass positional and/or keyword
                                arguments (lambda).
                              * there is a need to pass keyword arguments only
                                (functool.partial).
                        * The return type of the user defined function must be one
                          of the following:
                              * numpy ndarray
                                  * For a one-dimensional array, it is expected that
                                    it has as many values as the number of expected
                                    output columns.
                                  * For a two-dimensional array, it is expected that
                                    every array contained in the outer array has as
                                    many values as the number of expected output
                                    columns.
                              * pandas Series
                                    This represents a row in the output, and the
                                    number of values in it must be the same as the
                                    number of expected output columns.
                              * pandas DataFrame
                                    It is expected that a pandas DataFrame returned
                                    by the "user_function" has the same number of
                                    columns as the number of expected output columns.
                        * The return objects will be printed to the standard output
                          as required by Script using the 'quotechar' and 'delimiter'
                          values.
                        * The user function can also print the required output to
                          the standard output in the delimited (and possibly quoted)
                          format instead of returning an object of supported type.
                exec_mode:
                    Optional Argument.
                    Specifies the mode of execution for the user defined function.
                    Permitted values:
                        * IN-DB: Execute the function on data in the teradataml
                                 DataFrame in Vantage.
                        * LOCAL: Execute the function locally on sample data (at
                                 most "num_rows" rows) from the teradataml
                                 DataFrame.
                    Default value: 'IN-DB'
                    Types: str
                chunk_size:
                    Optional Argument.
                    Specifies the number of rows to be read in a chunk in each
                    iteration using an iterator to apply the user defined function
                    to each row in the chunk.
                    Varying the value passed to this argument affects the
                    performance and the memory utilization.
                    Default value: 1000
                    Types: int
                num_rows:
                    Optional Argument.
                    Specifies the maximum number of sample rows to use from the
                    teradataml DataFrame to apply the user defined function to when
                    "exec_mode" is 'LOCAL'.
                    Default value: 1000
                    Types: int
                returns:
                    Optional Argument.
                    Specifies the output column definition corresponding to the
                    output of "user_function".
                    When not specified, the function assumes that the names and
                    types of the output columns are same as that of the input.
                    Note:
                        Teradata reserved keywords should not be provided as column names unless
                        the column names of output dataframe are an exact match of input dataframe.
                        User can find the list or check if the string is a reserved keyword or not
                        using list_td_reserved_keywords() function.
                    Types: Dictionary specifying column name to
                           teradatasqlalchemy type mapping.
                delimiter:
                    Optional Argument.
                    Specifies a delimiter to use when reading columns from a row and
                    writing result columns.
                    Default value: '        '
                    Types: str with one character
                    Notes:
                        * This argument cannot be same as "quotechar" argument.
                        * This argument cannot be a newline character i.e., '
    '.
                quotechar:
                    Optional Argument.
                    Specifies a character that forces all input and output of the
                    user function to be quoted using this specified character.
                    Using this argument enables the Advanced SQL Engine to
                    distinguish between NULL fields and empty strings.
                    A string with length zero is quoted, while NULL fields are not.
                    If this character is found in the data, it will be escaped by a
                    second quote character.
                    Types: str with one character
                    Notes:
                        * This argument cannot be same as "delimiter" argument.
                        * This argument cannot be a newline character i.e., '
    '.
                auth:
                    Optional Argument.
                    Specifies an authorization to use when running the
                    "user_function".
                    Types: str
                charset:
                    Optional Argument.
                    Specifies the character encoding for data.
                    Permitted values: 'utf-16', 'latin'
                    Types: str
                data_order_column:
                    Optional Argument.
                    Specifies the Order By columns for the teradataml DataFrame.
                    Values to this argument can be provided as a list, if multiple
                    columns are used for ordering.
                    This argument is used in both cases:
                    "is_local_order = True" and "is_local_order = False".
                    Types: str OR list of Strings (str)
                    Note:
                        "is_local_order" must be set to 'True' when
                        "data_order_column" is used with "data_hash_column".
                is_local_order:
                    Optional Argument.
                    Specifies a boolean value to determine whether the input data is
                    to be ordered locally or not.
                    "data_order_column" with "is_local_order" set to 'False'
                    specifies the order in which the values in a group, or
                    partition, are sorted.
                    When this argument is set to 'True', qualified rows on each AMP
                    are ordered in preparation to be input to a table function.
                    This argument is ignored, if "data_order_column" is None.
                    Default value: False
                    Types: bool
                    Notes:
                        * "is_local_order" cannot be specified along with
                          "data_partition_column".
                        * When "is_local_order" is set to True, "data_order_column"
                          should be specified, and the columns specified in
                          "data_order_column" are used for local ordering.
                nulls_first:
                    Optional Argument.
                    Specifies a boolean value to determine whether NULLS are listed
                    first or last during ordering.
                    This argument is ignored, if "data_order_column" is None.
                    NULLS are listed first when this argument is set to 'True', and
                    last when set to 'False'.
                    Default value: True
                    Types: bool
                sort_ascending:
                    Optional Argument.
                    Specifies a boolean value to determine if the result set is to
                    be sorted on the "data_order_column" column in ascending or
                    descending order.
                    The sorting is ascending when this argument is set to 'True',
                    and descending when set to 'False'.
                    This argument is ignored, if "data_order_column" is None.
                    Default Value: True
                    Types: bool
                debug:
                    Optional Argument.
                    Specifies whether to display the script file path generated during function execution or not. This
                    argument helps in debugging when there are any failures during function execution. When set
                    to True, function displays the path of the script and does not remove the file from local file system.
                    Otherwise, file is removed from the local file system.
                    Default Value: False
                    Types: bool
            RETURNS:
                1. teradataml DataFrame if exec_mode is "IN-DB".
                2. Pandas DataFrame if exec_mode is "LOCAL".
            RAISES:
                 TypeError, TeradataMlException.
            EXAMPLES:
                >>> # This example uses the 'admissions_train' dataset, to increase
                >>> # the 'gpa' by a give percentage.
                >>> # Load the example data.
                >>> load_example_data("dataframe", "admissions_train")
                >>> df = DataFrame('admissions_train')
                >>> print(df)
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
                >>> # Example 1:
                >>> # Create the user defined function to increase the 'gpa' by the
                >>> # percentage provided. Note that the input to and the output
                >>> # from the function is a pandas Series object.
                >>> def increase_gpa(row, p=20):
                ...     row['gpa'] = row['gpa'] + row['gpa'] * p/100
                ...     return row
                ...
                >>>
                >>> # Apply the user defined function to the DataFrame.
                >>> # Note that since the output of the user defined function
                >>> # expects the same columns with the same types, we can skip
                >>> # passing the 'returns' argument.
                >>> increase_gpa_20 = df.map_row(increase_gpa)
                >>>
                >>> # Print the result.
                >>> print(increase_gpa_20)
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
                26     yes  4.284  Advanced    Advanced         1
                19     yes  2.376  Advanced    Advanced         0
                >>> # Example 2:
                >>> # Use the same user defined function with a lambda notation to
                >>> # pass the percentage, 'p = 40'.
                >>> increase_gpa_40 = df.map_row(lambda row: increase_gpa(row,
                ...                                                       p = 40))
                >>>
                >>> print(increase_gpa_40)
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
                >>> # Example 3:
                >>> # Use the same user defined function with functools.partial to
                >>> # pass the percentage, 'p = 50'.
                >>> from functools import partial
                >>> increase_gpa_50 = df.map_row(partial(increase_gpa, p = 50))
                >>>
                >>> print(increase_gpa_50)
                   masters    gpa     stats programming  admitted
                id
                13      no  6.000  Advanced      Novice         1
                26     yes  5.355  Advanced    Advanced         1
                5       no  5.160    Novice      Novice         0
                19     yes  2.970  Advanced    Advanced         0
                15     yes  6.000  Advanced    Advanced         1
                40     yes  5.925    Novice    Beginner         0
                7      yes  3.495    Novice      Novice         1
                22     yes  5.190    Novice    Beginner         0
                36      no  4.500  Advanced      Novice         0
                38     yes  3.975  Advanced    Beginner         1
                >>> # Example 4:
                >>> # Use a lambda function to increase the 'gpa' by 50 percent, and
                >>> # return numpy ndarray.
                >>> from numpy import asarray
                >>> inc_gpa_lambda = lambda row, p=20: asarray([row['id'],
                ...                                row['masters'],
                ...                                row['gpa'] + row['gpa'] * p/100,
                ...                                row['stats'],
                ...                                row['programming'],
                ...                                row['admitted']])
                >>> increase_gpa_100 = df.map_row(lambda row: inc_gpa_lambda(row,
                ...                                                          p=100))
                >>>
                >>> print(increase_gpa_100)
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
    materialize
    teradataml.dataframe.dataframe.DataFrame.materialize = materialize(self)
    DESCRIPTION:
        Method to materialize teradataml DataFrame into a database object.
        Notes:
            * DataFrames are materialized in either view/table/volatile table,
              which is decided and taken care by teradataml.
            * If user wants to materialize object into speciﬁc database object
              such as table/volatile table, use 'to_sql()' or 'copy_to_sql()' or
              'fastload()' functions.
            * Materialized object is garbage collected at the end of the session.
    PARAMETERS:
        None
    RETURNS:
        DataFrame
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        19     yes  1.98  Advanced    Advanced         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        38     yes  2.65  Advanced    Beginner         1
        # Example 1: Perform operations on teradataml DataFrame
        #            and materializeit in a database object.
        >>> df2 = df.get([["id", "masters", "gpa"]])
        # Initially table_name will be None.
        >>> df2._table_name
        >>> df2.materialize()
           masters   gpa
        id
        15     yes  4.00
        7      yes  2.33
        22     yes  3.46
        17      no  3.83
        13      no  4.00
        38     yes  2.65
        26     yes  3.57
        5       no  3.44
        34     yes  3.85
        40     yes  3.95
        # After materialize(), view name will be assigned.
        >>> df2._table_name
        '"ALICE"."ml__select__172077355985236"'
        >>>
    max
    teradataml.dataframe.dataframe.DataFrame.max = max(self, distinct=False)
    DESCRIPTION:
        Returns column-wise maximum value of the dataframe.
        Note:
            Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the maximum value.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with max()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If max() operation fails to
            generate the column-wise maximum value of the dataframe.
            Possible error message:
            Failed to perform 'max'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the max() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'max' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints maximum value of each column(with supported data types).
        >>> df1.max()
          max_employee_no max_first_name max_marks max_dob max_joined_date
        0             112          abcde      None    None        18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df3 = df1.select(['employee_no', 'first_name', 'joined_date'])
        # Prints maximum value of each column(with supported data types).
        >>> df3.max()
          max_employee_no max_first_name max_joined_date
        0             112          abcde        18/12/05
        >>>
        #
        # Using max() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        #
        # Time Series Aggregate Example 1: Executing max() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the maximum values.
        #
        # To use max() as Time Series Aggregate we must run groupby_time() first, followed by max().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.max().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             max_TD_TIMECODE  max_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:10:00.000000            55              100
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:03:25.122200            55               79
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:03:25.122200            55               82
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:52:00.000009            55               56
        >>>
        #
        # Time Series Aggregate Example 2: Executing max() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the maximum value.
        #
        # To use max() as Time Series Aggregate we must run groupby_time() first, followed by max().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.max(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             max_TD_TIMECODE  max_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:10:00.000000            55              100
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:03:25.122200            55               79
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:03:25.122200            55               82
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:52:00.000009            55               56
        >>>
    median
    teradataml.dataframe.dataframe.DataFrame.median = median(self, distinct=False)
    DESCRIPTION:
        Returns column-wise median value of the dataframe.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the median.
            Note:
                This is allowed only when median() is used as Time Series Aggregate function, i.e.,
                this can be set to True, only when median() is operated on DataFrameGroupByTime object.
                Otherwise, an exception will be raised.
            Default Values: False
    RETURNS:
        teradataml DataFrame object with median() operation
        performed.
    RAISES:
        1. TDMLDF_AGGREGATE_FAILED - If median() operation fails to
            generate the column-wise median value of the dataframe.
            Possible error message:
            Unable to perform 'median()' on the dataframe.
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the median() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'median' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints median value of each column(with supported data types).
        >>> df1.median()
          median_employee_no median_marks
        0                101         None
        >>>
        #
        # Using median() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing median() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the median value.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        # To use median() as Time Series Aggregate we must run groupby_time() first, followed by median().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.median().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  median_temperature  median_salin
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                54.5             55.0
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                74.5             55.0
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                81.0             55.0
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                43.0             55.0
        >>>
        #
        # Time Series Aggregate Example 2: Executing median() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT rows for the
        #                                  columns while calculating the median value.
        #
        # To use median() as Time Series Aggregate we must run groupby_time() first, followed by median().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.median(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  median_temperature  median_salin
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                99.0             55.0
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                74.5             55.0
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                81.0             55.0
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                54.0             55.0
        >>>
    merge
    teradataml.dataframe.dataframe.DataFrame.merge = merge(self, right, on=None, how='inner', left_on=None, right_on=None, use_index=False, lsufﬁx=None, rsufﬁx=None)
    DESCRIPTION:
        Merges two teradataml DataFrames together.
        Supported merge operations are:
            - cross: Returns cartesian product between the two dataframes.
            - inner: Returns only matching rows, non-matching rows are eliminated.
            - left: Returns all matching rows plus non-matching rows from the left teradataml DataFrame.
            - right: Returns all matching rows plus non-matching rows from the right teradataml DataFrame.
            - full: Returns all rows from both teradataml DataFrames, including non matching rows.
    PARAMETERS:
        right:
            Required Argument.
            Specifies right teradataml DataFrame on which merge is to be performed.
            Types: teradataml DataFrame
        on:
            Optional Argument.
            Specifies list of conditions that indicate the columns used for the merge.
            When no arguments are provided for this condition, the merge is performed using the indexes
            of the teradataml DataFrames. Both teradataml DataFrames are required to have index labels to
            perform a merge operation when no arguments are provided for this condition.
            When either teradataml DataFrame does not have a valid index label in the above case,
            an exception is thrown.
            • String comparisons, in the form of "col1 <= col2", where col1 is
              the column of left DataFrame df1 and col2 is the column of right
              DataFrame df2.
              Examples:
                1. ["a","b"] indicates df1.a = df2.a and df1.b = df2.b.
                2. ["a = b", "c = d"] indicates df1.a = df2.b and df1.c = df2.d
                3. ["a <= b", "c > d"] indicates df1.a <= df2.b and df1.c > df2.d.
                4. ["a < b", "c >= d"] indicates df1.a < df2.b and df1.c >= df2.d.
                5. ["a <> b"] indicates df1.a != df2.b. Same is the case for ["a != b"].
            • Column comparisons, in the form of df1.col1 <= df2.col2, where col1
              is the column of left DataFrame df1 and col2 is the column of right
              DataFrame df2.
              Examples:
                1. [df1.a == df2.a, df1.b == df2.b] indicates df1.a = df2.a and df1.b = df2.b.
                2. [df1.a == df2.b, df1.c == df2.d] indicates df1.a = df2.b and df1.c = df2.d.
                3. [df1.a <= df2.b and df1.c > df2.d] indicates df1.a <= df2.b and df1.c > df2.d.
                4. [df1.a < df2.b and df1.c >= df2.d] indicates df1.a < df2.b and df1.c >= df2.d.
                5. df1.a != df2.b indicates df1.a != df2.b.
            • The combination of both string comparisons and comparisons as column expressions.
              Examples:
                1. ["a", df1.b == df2.b] indicates df1.a = df2.a and df1.b = df2.b.
                2. [df1.a <= df2.b, "c > d"] indicates df1.a <= df2.b and df1.c > df2.d.
            Default Value: None
            Types: str or ColumnExpression or List of strings(str) or ColumnExpressions
        how:
            Optional Argument.
            Specifies the type of merge to perform. Supports inner, left, right, full and cross merge operations.
            When how is "cross", the arguments on, left_on, right_on and use_index are ignored.
            Default Value: "inner".
            Types: str
        left_on:
            Optional Argument.
            Specifies column to merge on, in the left teradataml DataFrame.
            When both the 'on' and 'left_on' parameters are unspecified, the index columns
            of the teradataml DataFrames are used to perform the merge operation.
            Default Value: None.
            Types: str or ColumnExpression or List of strings(str) or ColumnExpressions
        right_on:
            Optional Argument.
            Specifies column to merge on, in the right teradataml DataFrame.
            When both the 'on' and 'right_on' parameters are unspecified, the index columns
            of the teradataml DataFrames are used to perform the merge operation.
            Default Value: None.
            Types: str or ColumnExpression or List of strings(str) or ColumnExpressions
        use_index:
            Optional Argument.
            Specifies whether (or not) to use index from the teradataml DataFrames as the merge key(s).
            When False, and 'on', 'left_on', and 'right_on' are all unspecified, the index columns
            of the teradataml DataFrames are used to perform the merge operation.
            Default value: False.
            Types: bool
        lsuffix:
            Optional Argument.
            Specifies suffix to be added to the left table columns.
            Default Value: None.
            Types: str
            Note: A suffix is required if teradataml DataFrames being merged have columns
                  with the same name.
        rsuffix:
            Optional Argument.
            Specifies suffix to be added to the right table columns.
            Default Value: None.
            Types: str
            Note: A suffix is required if teradataml DataFrames being merged have columns
                  with the same name.
    RAISES:
        TeradataMlException
    RETURNS:
        teradataml DataFrame
    EXAMPLES:
        # Example set-up teradataml DataFrames for merge
        >>> from datetime import datetime, timedelta
        >>> dob = datetime.strptime('31101991', '%d%m%Y').date()
        >>> df1 = pd.DataFrame(data={'col1': [1, 2,3],
                       'col2': ['teradata','analytics','platform'],
                       'col3': [1.3, 2.3, 3.3],
                       'col5': ['a','b','c'],
                        'col 6': [dob, dob + timedelta(1), dob + timedelta(2)],
                        "'col8'":[3,4,5]})
        >>> df2 = pd.DataFrame(data={'col1': [1, 2, 3],
                            'col4': ['teradata', 'analytics', 'are you'],
                            'col3': [1.3, 2.3, 4.3],
                             'col7':['a','b','d'],
                             'col 6': [dob, dob + timedelta(1), dob + timedelta(3)],
                             "'col8'": [3, 4, 5]})
        >>> # Persist the Pandas DataFrames in Vantage.
        >>> copy_to_sql(df1, "table1", primary_index="col1")
        >>> copy_to_sql(df2, "table2", primary_index="col1")
        >>> df1 = DataFrame("table1")
        >>> df2 = DataFrame("table2")
        >>> df1
             'col8'       col 6       col2  col3 col5
        col1                                         
        2         4  1991-11-01  analytics   2.3    b
        1         3  1991-10-31   teradata   1.3    a
        3         5  1991-11-02   platform   3.3    c
        >>> df2
             'col8'       col 6  col3       col4 col7
        col1                                         
        2         4  1991-11-01   2.3  analytics    b
        1         3  1991-10-31   1.3   teradata    a
        3         5  1991-11-03   4.3    are you    d            
        >>> # 1) Specify both types of 'on' conditions and DataFrame indexes as merge keys:
        >>> df1.merge(right = df2, how = "left", on = ["col3","col2=col4"], use_index = True, lsuffix = "t1", rsuffix = "t2")
          t2_col1 col5    t2_col 6 t1_col1 t2_'col8'  t1_col3       col4  t2_col3  col7       col2    t1_col 6 t1_'col8'
        0       2    b  1991-11-01       2         4      2.3  analytics      2.3     b  analytics  1991-11-01         4
        1       1    a  1991-10-31       1         3      1.3   teradata      1.3     a   teradata  1991-10-31         3
        2    None    c        None       3      None      3.3       None      NaN  None   platform  1991-11-02         5
        >>> # 2) Specify 'on' conditions as ColumnExpression and DataFrame indexes as merge keys:
        >>> df1.merge(right = df2, how = "left", on = [df1.col1, df1.col3], use_index = True, lsuffix = "t1", rsuffix = "t2")
          t1_col1  t2_col1       col2  t1_col3  t2_col3 col5    t1_col 6    t2_col 6  t1_'col8'  t2_'col8'       col4  col7
        0        2      2.0  analytics      2.3      2.3    b  1991-06-23  1991-06-23          4        4.0  analytics     b
        1        1      1.0   teradata      1.3      1.3    a  1991-06-22  1991-06-22          3        3.0   teradata     a
        2        3      NaN   platform      3.3      NaN    c  1991-06-24        None          5        NaN       None  None
        >>> # 3) Specify left_on, right_on conditions along with DataFrame indexes as merge keys:
        >>> df1.merge(right = df2, how = "right", left_on = "col2", right_on = "col4", use_index = True, lsuffix = "t1", rsuffix = 
          t1_col1 t2_col1       col2  t1_col3  t2_col3  col5    t1_col 6    t2_col 6 t1_'col8' t2_'col8'       col4 col7
        0       2       2  analytics      2.3      2.3     b  1991-11-01  1991-11-01         4         4  analytics    b
        1       1       1   teradata      1.3      1.3     a  1991-10-31  1991-10-31         3         3   teradata    a
        2    None       3       None      NaN      4.3  None        None  1991-11-03      None         5    are you    d
        >>> # 4) If teradataml DataFrames to be merged do not contain common columns, lsuffix and
            #  rsuffix are not required:
        >>> new_df1 = df1.select(['col2', 'col5'])
        >>> new_df2 = df2.select(['col4', 'col7'])
        >>> new_df1
          col5       col2
        0    b  analytics
        1    a   teradata
        2    c   platform
        >>> new_df2
          col7       col4
        0    b  analytics
        1    a   teradata
        2    d    are you
        >>> new_df1.merge(right = new_df2, how = "inner", on = "col5=col7")
          col5       col4       col2 col7
        0    b  analytics  analytics    b
        1    a   teradata   teradata    a
        >>> # 5) When no merge conditions are specified, teradataml DataFrame
            # indexes are used as merge keys.
        >>> df1.merge(right = df2, how = "full", lsuffix = "t1", rsuffix = "t2")
          t2_col1 col5    t2_col 6 t1_col1 t2_'col8'  t1_col3       col4  t2_col3 col7       col2    t1_col 6 t1_'col8'
        0       2    b  1991-11-01       2         4      2.3  analytics      2.3    b  analytics  1991-11-01         4
        1       1    a  1991-10-31       1         3      1.3   teradata      1.3    a   teradata  1991-10-31         3
        2       3    c  1991-11-03       3         5      3.3    are you      4.3    d   platform  1991-11-02         5
    partitions
    teradataml.dataframe.dataframe.DataFrame.partitions
    DESCRIPTION:
        Gets partition information for a DataLake table.
    PARAMETERS:
        None
    RETURNS:
        teradataml DataFrame.
    RAISES:
        TeradataMLException.
    EXAMPLES :
        # Example 1: Get the partition information for datalake table.
        >>> from teradataml.dataframe.dataframe import in_schema
        >>> in_schema_tbl = in_schema(schema_name="datalake_db",
        ...                           table_name="datalake_table",
        ...                           datalake_name="datalake")
        >>> datalake_df = DataFrame(in_schema_tbl)
        >>> datalake_df.partitions
              id    name
        0   1000      c2
        1   1001      c3
    pivot
    teradataml.dataframe.dataframe.DataFrame.pivot = pivot(self, columns=None, aggfuncs=None, limit_combinations=False, margins=None, returns=None, all_columns=False,
    **kwargs)
    DESCRIPTION:
        Rotate data from rows into columns to create easy-to-read DataFrames. 
        The function is useful for reporting purposes, as it allows to 
        aggregate and rotate the data.  
    PARAMETERS:
        columns:
            Required when keyword arguments are not specified, optional otherwise.
            Specifies the column(s) or dictionary of column(s) with distinct column 
            value(s) used for pivoting the data.
            * When specified as a column(s), function automatically extracts 
              the distinct values for the column.
              For example:
                columns = df.qtr # or columns = [df.qtr, dr.yr]
              Note that, distinct values 'Q1', 'Q2', 'Q3', ... are 
              automatically extracted from column 'qtr' for pivoting.
            * When specified as dictionary:
                - key is a column expression representing a column
                - value is a literal or list of literals, i.e., values in a column 
                  or a teradataml DataFrame with only one column.
                  Note:
                      When value is a teradataml DataFrame, the order of output pivoted columns
                      are not guarenteed. Hence value is a teradataml DataFrame, teradata recommonds
                      to not specify "returns" argument so column names are generated properly
                      according to the order of records.
              When dictionary value contains literals then pivot is done based on the each 
              combination for the values specified for each key, if "limit_combinations" is 
              set to False, otherwise combinations are restricted based on the index values 
              of the list.
              Notes:
                   * All the values in dictionary should have same length when "limit_combinations"
                      is set to True.
                      Take a look at the examples below to understand:
                        - columns={df.qtr: ['Q1', 'Q2'], df.year: [2001, 2002]}
                          Pivot is based on all the combinations of values as specified
                          below for columns 'qtr' and 'yr'.
                            If "limit_combinations" is set to False.
                                (Q1, 2001), (Q1, 2002), (Q2, 2001), and (Q2, 2002).
                            If "limit_combinations" is set to True.
                                (Q1, 2001) and (Q2, 2002).
                        - columns={df.qtr: quarter_df, df.year: [2001, 2002]}
                          Note that, value passed to df.qtr is a teradataml DataFrame
                          with one column. In such cases, function extracts the distinct
                          values from the dataframe and Pivot is based on all the combinations
                          of values or limited combination of values based on argument
                          "limit_combinations".
                  * String type values in dictionary are not case sensitive. Refer to example 2
                    for more details.
            Types: ColumnExpression OR list of ColumnExpression or dict
        aggfuncs:
            Required Argument.
            Specifies the column aggregate function(s) to be used for pivoting the data.
            For example:
                To pivot total of 'sales' column and average of 'cogs' + 'sales' columns
                in DataFrame (df), specify "aggfuncs" as:
                    aggfuncs=[df.sales.sum(), (df.cogs+df.sales).avg()]
            Types: ColumnExpression OR list of ColumnExpression
        limit_combinations:
            Optional Argument. 
            Specifies whether to limit the number of combinations when "columns" argument
            is passed as a dictionary.
            When set to True, function limits the combinations one-to-one
            based on index of the values in list, hence all dictionary values 
            should be a list of same length or a single literal.
            For example:
                df.pivot(columns={df.qtr: ['Q1', 'Q2'], df.year: [2001, 2002]}, 
                         limit_combinations=True,....)
            Pivot will be based on columns 'qtr' and 'yr' for values 
            (Q1, 2001) and (Q2, 2002) only.
            Note:
                Argument is ignored when "columns" is a ColumnExpression or a list of ColumnExpressions.
            Default Value: False
            Types: bool
        margins:
            Optional Argument.
            Specifies the aggregate operation to be performed on output columns.
            Aggregate operation to be performed should be passed as dictionary where:
                * Key is a string specifying aggregate operation to be performed.
                * Value is a tuple or list of tuples specifying the output column names as 
                  string. Columns specified in the tuple are used in aggregate operation 
                  performed.
            For example, if for the year 2001 following three aggregate columns are needed
            in the output:
                1. Sum of sales in Q1 and Q2
                2. Sum of sales in Q2 and Q3
                3. Average of cogs for first three quarters
            "margins" can be sepcified as:
                margins={"SUM": [("Q12001_sales", "Q22001_sales"), 
                                 ("Q22001_sales", "Q32001_sales")],
                         "AVG": ("Q12001_cogs", "Q22001_cogs", "Q32001_cogs")}
            Notes:
                 * Supported aggregate functions are SUM, AVG, MIN and MAX.
                 * If "returns" is specified, column names for margins are considered
                   from returns clause.
            Types: dict
        returns:
            Optional Argument.
            Specifies the custom column name(s) to be returned in the output DataFrame.
            Notes:
                * If not specified, function internally generate the output column name(s).
                * Number of column names should be same as columns generated by pivot.
                  For example: 
                    If columns={df.qtr: ['Q1', 'Q2'], df.year: [2001, 2002]},
                       aggfuncs=[df.sales.sum(), df.cogs.sum()] and
                       limit_combination=False, then
                    number of new columns in output will be:
                        len(aggfuncs) * len(columns[df.year]) * len(columns[df.qtr])
                        or 2 * 2 * 2 = 8
                    Hence, "returns" should be list of string with 8 elements.
                    If limit_combination is set to 'True' in above example,
                    number of new columns is output will be:
                        len(aggfuncs) * (len(columns[df.year]) OR len(columns[df.qtr]))
                        or or 2 * 2 = 4
                    Hence, "returns" should be list of string with 4 elements.
                * All columns including columns in DataFrame which do not participate in pivot
                  must be specified if "all_columns" is set to True.
            Types: str OR list of Strings (str)
        all_columns:
            Optional Argument.
            Specifies whether "returns" argument should include only the names of pivot 
            columns or all columns.
            When set to True, all columns including columns in DataFrame which do not
            participate in pivot must be specified.
            When set to False, only output columns excluding columns in DataFrame which do not
            participate in pivot must be specified.
            Default Value: False
            Types: bool
        **kwargs:
            Specifies the keyword argument to accept column name and column value(s)
            as named arguments.
            For example:
                col1=df.year, col1_value=2001, col2=df.qtr, col2_value=['Q1', 'Q2', 'Q3']
            Notes:
                * Either use "columns" argument or keyword arguments.
                * Format for column name arguments should be col1, col2,.. colN with
                  values of type ColumnExpression.
                * Format for column value argument should be col1_value, col2_value,.. colN_value.
    RETURNS:
            teradataml DataFrame
            Notes:
                * The columns which are not participating in pivoting are always aligned to 
                  left most in the output DataFrame. 
                * Order of pivot columns in output DataFrame follows the same order as the 
                  values specified in argument "columns" or the order in keyword arguments. 
                * The name of the output columns are according to the name specified in 
                  "returns" argument.
                  If not specified, column names are generated based on aggregate functions
                  and values in "columns" or keyword arguments for column values.
                  For Example:
                    When columns={df.qtr:["Q1", "Q2"]} and aggfuncs=[df.col1.sum(), df.col2.max()]
                    Column names are:
                        'sum_col1_q1', 'max_col2_q1', 'sum_col1_Q2', and 'max_col2_Q2'.
    RAISES:
        TypeError, ValueError, TeradataMLException
    EXAMPLES:
        # Create a teradataml DataFrame.
        >>> load_example_data("teradataml", "star1")
        >>> df = DataFrame("star1")
        >>> print(df)
                state    yr qtr  sales  cogs
        country
        USA        NY  2001  Q1   45.0  25.0
        CANADA     ON  2001  Q2   10.0   0.0
        CANADA     BC  2001  Q3   10.0   0.0
        USA        CA  2002  Q2   50.0  20.0
        USA        CA  2002  Q1   30.0  15.0
        # Example 1: Pivot data using all values in column 'qtr' and aggregate using 
        #            sum of column 'sales'.
        >>> pivot_df = df.pivot(columns=df.qtr,
        ...                     aggfuncs=df.sales.sum())
        >>> print(pivot_df)
          country state    yr  cogs  sum_sales_q2  sum_sales_q1  sum_sales_q3
        0     USA    NY  2001  25.0           NaN          45.0           NaN
        1  CANADA    ON  2001   0.0          10.0           NaN           NaN
        2  CANADA    BC  2001   0.0           NaN           NaN          10.0
        3     USA    CA  2002  20.0          50.0           NaN           NaN
        4     USA    CA  2002  15.0           NaN          30.0           NaN
        # Example 2: Pivot data using columns 'yr' and 'qtr', aggregate using sum of
        #            'sales' and median of 'cogs' column. Limit combination of 'Q1' with '2001' and
        #            'Q2' with '2002'.
        >>> pivot_df = df.pivot(columns={df.yr: [2001, 2002], df.qtr: ['Q1', 'Q2']},
        ...                     aggfuncs=[df.sales.sum(), df.cogs.median()],
        ...                     limit_combinations=True)
        >>> print(pivot_df)
          country state  sum_sales_2001_q1  median_cogs_2001_q1  sum_sales_2002_q2  median_cogs_2002_q2
        0  CANADA    ON                NaN                  NaN                NaN                  NaN
        1     USA    CA                NaN                  NaN               50.0                 20.0
        2     USA    NY               45.0                 25.0                NaN                  NaN
        3  CANADA    BC                NaN                  NaN                NaN                  NaN
        # Example 3: Get all possible column combinations when pivoting using 'yr'and 'qtr',
        #            aggreagation using 'sales' and 'cogs' column.
        >>> pivot_df = df.pivot(columns={df.yr: [2001, 2002], df.qtr: ['q1', 'q2']},
        ...                     aggfuncs=[df.sales.sum(), df.cogs.max()])
        >>> print(pivot_df)
          country state  sum_sales_2001_q1  max_cogs_2001_q1  sum_sales_2001_q2  max_cogs_2001_q2  sum_sales_2002_q1  max_cogs_2002
        0  CANADA    BC                NaN               NaN                NaN               NaN                NaN               
        1  CANADA    ON                NaN               NaN               10.0               0.0                NaN               
        2     USA    NY               45.0              25.0                NaN               NaN                NaN               
        3     USA    CA                NaN               NaN                NaN               NaN               30.0              1
        # Example 4: Custom name the returned columns using "returns" argument.
        >>> pivot_df = df.pivot(columns={df.yr:2001, df.qtr:['Q1', 'Q2']},
        ...                     aggfuncs=[df.sales.sum(), (2*df.sales-1).max()],
        ...                     returns=["Q1_2001_total_sales", "Q1_2001_total_cogs",
        ...                              "Q2_2001_total_sales", "Q2_2001_total_cogs"])
        >>> print(pivot_df)
          country state  cogs  Q1_2001_total_sales  Q1_2001_total_cogs  Q2_2001_total_sales  Q2_2001_total_cogs
        0     USA    NY  25.0                 45.0                89.0                  NaN                 NaN
        1  CANADA    ON   0.0                  NaN                 NaN                 10.0                19.0
        2  CANADA    BC   0.0                  NaN                 NaN                  NaN                 NaN
        3     USA    CA  20.0                  NaN                 NaN                  NaN                 NaN
        4     USA    CA  15.0                  NaN                 NaN                  NaN                 NaN
        # Example 5: Custom name all output columns using "returns" and "all_columns" argument.
        >>> pivot_df = df.pivot(columns={df.yr:2001, df.qtr:['Q1', 'Q2']},
        ...                     aggfuncs=[df.sales.avg(), df.cogs.median()],
        ...                     returns=["con", "st", "Q1_2001_total_sales", "Q1_2001_total_cogs",
        ...                              "Q2_2001_total_sales", "Q2_2001_total_cogs"],
        ...                     all_columns=True)
        >>> print(pivot_df)
              con  st  Q1_2001_total_sales  Q1_2001_total_cogs  Q2_2001_total_sales  Q2_2001_total_cogs
        0  CANADA  ON                  NaN                 NaN                 10.0                 0.0
        1     USA  CA                  NaN                 NaN                  NaN                 NaN
        2     USA  NY                 45.0                25.0                  NaN                 NaN
        3  CANADA  BC                  NaN                 NaN                  NaN                 NaN
        # Example 6: Use keyword arguments to specify columns and values instead of "columns" argument.
        #            Note since "returns" is not specifies, column names are function generated.
        >>> pivot_df = df.pivot(aggfuncs=[df.sales.avg(), ((2*df.sales)+(df.cogs/2)).min()],
        ...                     col1=df.yr,
        ...                     col1_values=2001,
        ...                     col2=df.qtr,
        ...                     col2_values=['Q1', 'Q2'])
        >>> print(pivot_df)
          country state  avg_sales_2001_q1  min_sales_2_cogs_2_2001_q1  avg_sales_2001_q2  min_sales_2_cogs_2_2001_q2
        0  CANADA    BC                NaN                         NaN                NaN                         NaN
        1  CANADA    ON                NaN                         NaN               10.0                        20.0
        2     USA    NY               45.0                       102.5                NaN                         NaN
        3     USA    CA                NaN                         NaN                NaN                         NaN
        # Example 7: Find the median sales and mean cogs for first three quarters of
        #            year 2001 using "margins".
        >>> pivot_df = df.pivot(columns={df.qtr: ['q1', 'q2', 'q3'], df.yr: [2001]},
        ...                     aggfuncs=df.sales.median(),
        ...                     margins={"SUM": [("median_sales_q1_2001", "median_sales_q2_2001"),
        ...                                      ("median_sales_q3_2001", "median_sales_q2_2001")],
        ...                              "AVG": [("median_sales_q1_2001", "median_sales_q2_2001"),
        ...                                      ("median_sales_q2_2001", "median_sales_q1_2001")]})
        >>> print(pivot_df)
          country state  cogs  median_sales_q1_2001  median_sales_q2_2001  median_sales_q3_2001  sum_median_sales_q1_2001_median_sa
        0  CANADA    BC   0.0                   NaN                   NaN                  10.0                                    
        1     USA    NY  25.0                  45.0                   NaN                   NaN                                    
        2  CANADA    ON   0.0                   NaN                  10.0                   NaN                                    
        3     USA    CA  20.0                   NaN                   NaN                   NaN                                    
        4     USA    CA  15.0                   NaN                   NaN                   NaN                                    
        # Example 8: Find the min of sales and average of cogs for year '2001' in
        #            quater 'Q1', 'Q2'. Name the aggregated column as 'margins'.
        >>> pivot_df = df.pivot(columns={df.qtr: ['q1', 'q2'], df.yr: [2001]},
        ...                      aggfuncs=[df.cogs.min(), df.sales.avg()],
        ...                      margins={"MIN": ("cogs_min_q12001", "sales_var_samp_q22001")},
        ...                      all_columns=True,
        ...                      returns=["con",
        ...                               "st",
        ...                               "cogs_min_q12001",
        ...                               "sales_var_samp_q12001",
        ...                               "cogs_min_q22001",
        ...                               "sales_var_samp_q22001",
        ...                               "margins"])
        >>> print(pivot_df)
          con  st  cogs_min_q12001  sales_var_samp_q12001  cogs_min_q22001  sales_var_samp_q22001  margins
        0  CANADA  BC              NaN                    NaN              NaN                    NaN      NaN
        1  CANADA  ON              NaN                    NaN              0.0                   10.0     10.0
        2     USA  NY             25.0                   45.0              NaN                    NaN     25.0
        3     USA  CA              NaN                    NaN              NaN                    NaN      NaN
        # Example 9: Specify teradataml DataFrame with one column for values of 'qtr' column and pivot
        #            the data. Note this allows user to implicitly use all the distinct values for
        #            the pivot operation and not specify those explicitly.
        >>> quarters_df = df.drop(columns=['country', 'state', 'yr', 'sales', 'cogs'])
        >>> print(quarters_df)
          qtr
        0  Q1
        1  Q2
        2  Q3
        3  Q2
        4  Q1
        >>>
        >>> pivot_df = df.pivot(columns={df.qtr: quarters_df},
        ...                     aggfuncs=[df.sales.sum(), df.cogs.avg()])
        >>> print(pivot_df)
          country state    yr  sum_sales_q2  avg_cogs_q2  sum_sales_q1  avg_cogs_q1  sum_sales_q3  avg_cogs_q3
        0  CANADA    ON  2001          10.0          0.0           NaN          NaN           NaN          NaN
        1  CANADA    BC  2001           NaN          NaN           NaN          NaN          10.0          0.0
        2     USA    NY  2001           NaN          NaN          45.0         25.0           NaN          NaN
        3     USA    CA  2002          50.0         20.0          30.0         15.0           NaN          NaN
    plot
    teradataml.dataframe.dataframe.DataFrame.plot = plot(self, x, y, scale=None, kind='line', **kwargs)
    DESCRIPTION:
        Generate plots on teradataml DataFrame. Following type of plots
        are supported, which can be specified using argument "kind":
            * bar plot
            * corr plot
            * line plot
            * mesh plot
            * scatter plot
            * wiggle plot
    PARAMETERS:
        x:
            Required Argument.
            Specifies a DataFrame column to use for the x-axis data.
            Types: teradataml DataFrame Column
        y:
            Required Argument.
            Specifies DataFrame column(s) to use for the y-axis data.
            Types: teradataml DataFrame Column OR list of teradataml DataFrame Columns.
        scale:
            Optional Argument.
            Specifies DataFrame column to use for scale data to
            wiggle and mesh plots.
            Note:
                "scale" is significant for wiggle and mesh plots. Ignored for other
                type of plots.
            Types: teradataml DataFrame Column.
        kind:
            Optional Argument.
            Specifies the kind of plot.
            Permitted Values:
                * 'line'
                * 'bar'
                * 'scatter'
                * 'corr'
                * 'wiggle'
                * 'mesh'
            Default Value: line
            Types: str
        ax:
            Optional Argument.
            Specifies the axis for the plot.
            Types: Axis
        cmap:
            Optional Argument.
            Specifies the name of the colormap to be used for plotting.
            Notes:
                 * Significant only when corresponding type of plot is mesh or geometry.
                 * Ignored for other type of plots.
            Permitted Values:
                * All the colormaps mentioned in below URLs are supported.
                    * https://matplotlib.org/stable/tutorials/colors/colormaps.html
                    * https://matplotlib.org/cmocean/
            Types: str
        color:
            Optional Argument.
            Specifies the color for the plot.
            Note:
                Hexadecimal color codes are not supported.
            Permitted Values:
                * 'blue'
                * 'orange'
                * 'green'
                * 'red'
                * 'purple'
                * 'brown'
                * 'pink'
                * 'gray'
                * 'olive'
                * 'cyan'
                * Apart from above mentioned colors, the colors mentioned in
                  https://xkcd.com/color/rgb are also supported.
            Default Value: blue
            Types: str OR list of str
        figure:
            Optional Argument.
            Specifies the figure for the plot.
            Types: Figure
        figsize:
            Optional Argument.
            Specifies the size of the figure in a tuple of 2 elements. First
            element represents width of plot image in pixels and second
            element represents height of plot image in pixels.
            Default Value: (640, 480)
            Types: tuple
        figtype:
            Optional Argument.
            Specifies the type of the image to generate.
            Permitted Values:
                * 'png'
                * 'jpg'
                * 'svg'
            Default Value: png
            Types: str
        figdpi:
            Optional Argument.
            Specifies the dots per inch for the plot image.
            Note:
                * Valid range for "dpi" is: 72 <= width <= 300.
            Default Value: 100 for PNG and JPG Type image.
            Types: int
        grid_color:
            Optional Argument.
            Specifies the color of the grid.
            Note:
                Hexadecimal color codes are not supported.
            Permitted Values:
                * 'blue'
                * 'orange'
                * 'green'
                * 'red'
                * 'purple'
                * 'brown'
                * 'pink'
                * 'gray'
                * 'olive'
                * 'cyan'
                * Apart from above mentioned colors, the colors mentioned in
                  https://xkcd.com/color/rgb are also supported.
            Default Value: gray
            Types: str
        grid_format:
            Optional Argument.
            Specifies the format for the grid.
            Types: str
        grid_linestyle:
            Optional Argument.
            Specifies the line style of the grid.
            Permitted Values:
                * -
                * --
                * -.
            Default Value: -
            Types: str
        grid_linewidth:
            Optional Argument.
            Specifies the line width of the grid.
            Note:
                Valid range for "grid_linewidth" is: 0.5 <= grid_linewidth <= 10.
            Default Value: 0.8
            Types: int OR float
        heading:
            Optional Argument.
            Specifies the heading for the plot.
            Types: str
        legend:
            Optional Argument.
            Specifies the legend(s) for the Plot.
            Types: str OR list of str
        legend_style:
            Optional Argument.
            Specifies the location for legend to display on Plot image. By default,
            legend is displayed at upper right corner.
            Permitted Values:
                * 'upper right'
                * 'upper left'
                * 'lower right'
                * 'lower left'
                * 'right'
                * 'center left'
                * 'center right'
                * 'lower center'
                * 'upper center'
                * 'center'
            Default Value: 'upper right'
            Types: str
        linestyle:
            Optional Argument.
            Specifies the line style for the plot.
            Permitted Values:
                * -
                * --
                * -.
                * :
            Default Value: -
            Types: str OR list of str
        linewidth:
            Optional Argument.
            Specifies the line width for the plot.
            Note:
                Valid range for "linewidth" is: 0.5 <= linewidth <= 10.
            Default Value: 0.8
            Types: int OR float OR list of int OR list of float
        marker:
            Optional Argument.
            Specifies the type of the marker to be used.
            Permitted Values:
                All the markers mentioned in https://matplotlib.org/stable/api/markers_api.html
                are supported.
            Types: str OR list of str
        markersize:
            Optional Argument.
            Specifies the size of the marker.
            Note:
                Valid range for "markersize" is: 1 <= markersize <= 20.
            Default Value: 6
            Types: int OR float OR list of int OR list of float
        position:
            Optional Argument.
            Specifies the position of the axis in the figure. Accepts a tuple
            of two elements where first element represents the row and second
            element represents column.
            Default Value: (1, 1)
            Types: tuple
        span:
            Optional Argument.
            Specifies the span of the axis in the figure. Accepts a tuple
            of two elements where first element represents the row and second
            element represents column.
            For Example,
                Span of (2, 1) specifies the Axis occupies 2 rows and 1 column
                in Figure.
            Default Value: (1, 1)
            Types: tuple
        reverse_xaxis:
            Optional Argument.
            Specifies whether to reverse tick values on x-axis or not.
            Default Value: False
            Types: bool
        reverse_yaxis:
            Optional Argument.
            Specifies whether to reverse tick values on y-axis or not.
            Default Value: False
            Types: bool
        series_identifier:
            Optional Argument.
            Specifies the teradataml DataFrame Column which represents the
            identifier for the data. As many plots as distinct "series_identifier"
            are generated in a single Axis.
            For example:
                consider the below data in teradataml DataFrame.
                       ID   x   y
                    0  1    1   1
                    1  1    2   2
                    2  2   10  10
                    3  2   20  20
                If "series_identifier" is not specified, simple plot is
                generated where every 'y' is plotted against 'x' in a
                single plot. However, specifying "series_identifier" as 'ID'
                generates two plots in a single axis. One plot is for ID 1
                and another plot is for ID 2.
            Types: teradataml DataFrame Column.
        style:
            Optional Argument.
            Specifies the color for the plot.
            Note:
                Hexadecimal color codes are not supported.
            Permitted Values:
                * 'blue'
                * 'orange'
                * 'green'
                * 'red'
                * 'purple'
                * 'brown'
                * 'pink'
                * 'gray'
                * 'olive'
                * 'cyan'
                * Apart from above mentioned colors, the colors mentioned in
                  https://xkcd.com/color/rgb are also supported.
            Default Value: blue
            Types: str
        title:
            Optional Argument.
            Specifies the title for the Axis.
            Types: str
        xlabel:
            Optional Argument.
            Specifies the label for x-axis.
            Notes:
                 * When set to empty string, label is not displayed for x-axis.
                 * When set to None, name of the x-axis column is displayed as
                   label.
            Types: str
        xlim:
            Optional Argument.
            Specifies the range for xtick values.
            Types: tuple
        xtick_format:
            Optional Argument.
            Specifies whether to format tick values for x-axis or not.
            Types: str
        ylabel:
            Optional Argument.
            Specifies the label for y-axis.
            Notes:
                 * When set to empty string, label is not displayed for y-axis.
                 * When set to None, name of the y-axis column(s) is displayed as
                   label.
            Types: str
        ylim:
            Optional Argument.
            Specifies the range for ytick values.
            Types: tuple
        ytick_format:
            Optional Argument.
            Specifies whether to format tick values for y-axis or not.
            Types: str
        vmin:
            Optional Argument.
            Specifies the lower range of the color map. By default, the range
            is derived from data and color codes are assigned accordingly.
            Note:
                "vmin" significant only for Geometry Plot.
            Types: int OR float
        vmax:
            Optional Argument.
            Specifies the upper range of the color map. By default, the range is
            derived from data and color codes are assigned accordingly.
            Note:
                "vmax" Significant only for Geometry Plot.
            For example:
                Assuming user wants to use colormap 'matter' and derive the colors for
                values which are in between 1 and 100.
                Note:
                    Colormap 'matter' starts with Pale Yellow and ends with Violet.
                * If "colormap_range" is not specified, then range is derived from
                  existing values. Thus, colors are represented as below in the whole range:
                  * 1 as Pale Yellow.
                  * 100 as Violet.
                * If "colormap_range" is specified as -100 and 100, the value 1 is at middle of
                  the specified range. Thus, colors are represented as below in the whole range:
                  * -100 as Pale Yellow.
                  * 1 as Orange.
                  * 100 as Violet.
            Types: int OR float
        wiggle_fill:
            Optional Argument.
            Specifies whether to fill the wiggle area or not. By default, the right
            positive half of the wiggle is not filled. If specified as True, wiggle
            area is filled.
            Note:
                Applicable only for the wiggle plot.
            Default Value: False
            Types: bool
        wiggle_scale:
            Optional Argument.
            Specifies the scale of the wiggle. By default, the amplitude of wiggle is scaled
            relative to RMS of the first payload.  In certain cases, it can lead to excessively
            large wiggles. Use "wiggle_scale" to adjust the relative size of the wiggle.
            Note:
                Applicable only for the wiggle and mesh plots.
            Types: int OR float
        ignore_nulls:
            Optional Argument.
            Specifies whether to delete rows with null values or not present in 'x', 'y' and
            'scale' params.
            Default Value: False
            Types: bool
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load example data.
        >>> load_example_data("movavg", "ibm_stock")
        >>> load_example_data("uaf", ["waveletTable", "us_air_pass"])
        >>> load_example_data("teradataml", "iris_input")
        # Create teradataml DataFrame objects.
        >>> ibm_stock = DataFrame("ibm_stock")
        >>> mesh = DataFrame("waveletTable")
        >>> us_air_pass = DataFrame("us_air_pass")
        >>> iris_input = DataFrame("iris_input")
        # Example 1: Line Plot - This example creates simple line plot
        # and composite line plot with 2 columns in y-axis.
        # Print the DataFrame.
        >>> print(ibm_stock)
            name    period  stockprice
        id
        244  ibm  62/05/04       475.0
        101  ibm  61/10/10       557.0
        284  ibm  62/07/02       350.0
        141  ibm  61/12/07       577.0
        120  ibm  61/11/06       592.0
        303  ibm  62/07/30       376.0
        263  ibm  62/06/01       364.0
        305  ibm  62/08/01       385.0
        122  ibm  61/11/08       596.0
        265  ibm  62/06/05       370.0
        # Simple Line Plot.
        >>> plot =  ibm_stock.plot(x=ibm_stock.period, y=ibm_stock.stockprice,
                                   title="period vs stockprice",
                                   heading="Simple Line Plot")
        # Display the plot.
        >>> plot.show()
        # Composite Line Plot.
        >>> figure = Figure(width=800, height=900, image_type="jpg",
                            heading="Composite Line Plot")
        # Print the DataFrame.
        >>> print(ibm_stock)
            name    period  stockprice
        id
        244  ibm  62/05/04       475.0
        101  ibm  61/10/10       557.0
        284  ibm  62/07/02       350.0
        141  ibm  61/12/07       577.0
        120  ibm  61/11/06       592.0
        303  ibm  62/07/30       376.0
        263  ibm  62/06/01       364.0
        305  ibm  62/08/01       385.0
        122  ibm  61/11/08       596.0
        265  ibm  62/06/05       370.0
        >>> n1 = ibm_stock.assign(n=ibm_stock.stockprice * 2)
        >>> plot =  n1.plot(x=n1.period, y=[n1.stockprice, n1.n],
                            color=['dark orange', 'sand'], figure=figure)
        # Display the plot.
        >>> plot.show()
        # Example 2: Bar Plot - This example creates simple bar plot
        # and composite bar plot with 2 columns in y-axis.
        # Print the DataFrame.
        >>> print(ibm_stock)
            name    period  stockprice
        id
        244  ibm  62/05/04       475.0
        101  ibm  61/10/10       557.0
        284  ibm  62/07/02       350.0
        141  ibm  61/12/07       577.0
        120  ibm  61/11/06       592.0
        303  ibm  62/07/30       376.0
        263  ibm  62/06/01       364.0
        305  ibm  62/08/01       385.0
        122  ibm  61/11/08       596.0
        265  ibm  62/06/05       370.0
        # Simple Bar Plot.
        >>> plot =  ibm_stock.plot(x=ibm_stock.period, y=ibm_stock.stockprice,
                               kind='bar', xtick_format='MMM', ytick_format='9,99.99',
                               xlabel='xlabel', ylabel='ylabel', color="orange")
        # Display the plot.
        >>> plot.show()
        # Composite Bar Plot.
        # Import Figure.
        >>> from teradataml import Figure
        >>> figure = Figure(width=800, height=900, image_type="jpg", heading="Heading")
        # Print the DataFrame.
        >>> print(us_air_pass)
          TD_TIMECODE  id  idx  international  domestic
        0    17/11/01   0   10           7.72     61.91
        1    18/01/01   0   12           8.60     55.83
        2    18/02/01   0   13           7.64     54.08
        3    17/01/01   0    0           8.51     54.11
        4    17/03/01   0    2           9.00     63.96
        5    17/04/01   0    3           9.16     61.10
        6    17/05/01   0    4           9.24     64.44
        7    17/06/01   0    5          10.26     66.75
        8    17/02/01   0    1           7.30     51.08
        9    17/12/01   0   11           8.96     61.37
        >>> plot =  us_air_pass.plot(x=us_air_pass.idx, y=[us_air_pass.international, us_air_pass.domestic],
                                     kind='bar', color=['blue', 'red'], figure=figure,
                                     heading="Composite Bar Plot", figsize=(1200, 1100))
        # Display the plot.
        >>> plot.show()
        # Example 3: Scatter Plot - This example creates simple scatter plot
        # and composite scatter plot with 2 columns in y-axis.
        # Print the DataFrame.
        >>> print(iris_input)
             sepal_length  sepal_width  petal_length  petal_width  species
        id
        141           6.7          3.1           5.6          2.4        3
        99            5.1          2.5           3.0          1.1        2
        17            5.4          3.9           1.3          0.4        1
        139           6.0          3.0           4.8          1.8        3
        15            5.8          4.0           1.2          0.2        1
        137           6.3          3.4           5.6          2.4        3
        118           7.7          3.8           6.7          2.2        3
        120           6.0          2.2           5.0          1.5        3
        101           6.3          3.3           6.0          2.5        3
        122           5.6          2.8           4.9          2.0        3
        # Simple Scatter Plot.
        >>> plot =  plot = iris_input.plot(x=iris_input.sepal_length, y=iris_input.petal_length,
                                           kind='scatter', xlabel='sepal_length',
                                           ylabel='petal_length',
                                           color="red", grid_color='black',
                                           grid_linewidth=1, grid_linestyle="-",
                                           marker="p", marker_size=1,
                                           title="Scatter plot of sepal_length vs petal_length")
        # Display the plot.
        >>> plot.show()
        # Composite Scatter Plot.
        # Create DataFrame objects for species 1, 2, 3.
        >>> iris_sp1 = iris_input[iris_input.species == 1]
        >>> iris_sp2 = iris_input[iris_input.species == 2]
        >>> iris_sp3 = iris_input[iris_input.species == 3]
        # Print the DataFrames.
        >>> print(iris_sp1)
            sepal_length  sepal_width  petal_length  petal_width  species
        id
        38           4.9          3.6           1.4          0.1        1
        7            4.6          3.4           1.4          0.3        1
        26           5.0          3.0           1.6          0.2        1
        17           5.4          3.9           1.3          0.4        1
        34           5.5          4.2           1.4          0.2        1
        13           4.8          3.0           1.4          0.1        1
        32           5.4          3.4           1.5          0.4        1
        11           5.4          3.7           1.5          0.2        1
        15           5.8          4.0           1.2          0.2        1
        36           5.0          3.2           1.2          0.2        1
        >>> print(iris_sp2)
            sepal_length  sepal_width  petal_length  petal_width  species
        id
        59           6.6          2.9           4.6          1.3        2
        57           6.3          3.3           4.7          1.6        2
        97           5.7          2.9           4.2          1.3        2
        99           5.1          2.5           3.0          1.1        2
        51           7.0          3.2           4.7          1.4        2
        70           5.6          2.5           3.9          1.1        2
        89           5.6          3.0           4.1          1.3        2
        68           5.8          2.7           4.1          1.0        2
        53           6.9          3.1           4.9          1.5        2
        78           6.7          3.0           5.0          1.7        2
        >>> print(iris_sp3)
             sepal_length  sepal_width  petal_length  petal_width  species
        id
        133           6.4          2.8           5.6          2.2        3
        131           7.4          2.8           6.1          1.9        3
        110           7.2          3.6           6.1          2.5        3
        122           5.6          2.8           4.9          2.0        3
        141           6.7          3.1           5.6          2.4        3
        120           6.0          2.2           5.0          1.5        3
        139           6.0          3.0           4.8          1.8        3
        118           7.7          3.8           6.7          2.2        3
        101           6.3          3.3           6.0          2.5        3
        112           6.4          2.7           5.3          1.9        3
        # Import subplots.
        >>> from teradataml import subplots
        # Function to create a figure and a set of subplots.
        # The function makes it convenient to create common layouts of subplots,
        # including the enclosing figure object.
        # This will help to create a figure with 3 subplots in 1 row.
        # fig and axes is passed to plot().
        >>> fig, axes = subplots(nrows=1, ncols=3)
        >>> p = iris_sp1.plot(x=iris_sp1.sepal_length, y=iris_sp1.petal_length,
                              ax=axes[0], xlim=(0,10), ylim=(0,8),
                              figure=fig, kind="scatter",
                              title="Scatter plot of species 1: Sepal Length v/s Petal Length", color="blue", marker="*")
        >>> p = iris_sp2.plot(x=iris_sp2.sepal_length, y=iris_sp2.petal_length,
                              ax=axes[1], xlim=(0,10), ylim=(0,8),
                              figure=fig, kind="scatter",
                              title="Scatter plot of species 2: Sepal Length v/s Petal Length", color="red", marker="p")
        >>> p = iris_sp3.plot(x=iris_sp3.sepal_length, y=iris_sp3.petal_length,
                              ax=axes[2], xlim=(0,10), ylim=(0,8),
                              figure=fig, kind="scatter",
                              title="Scatter plot of species 3: Sepal Length v/s Petal Length", color="orange", marker="1")
        # Display the plot.
        >>> plot.show()
        # Example 4: Mesh Plot - This example creates simple mesh plot.
        # Print the DataFrame.
        >>> print(mesh)
               x      t      y             c
        ID
        a   94.0  800.0  701.0 -2.034400e-22
        a   94.0  800.0  702.0 -4.217551e-22
        a   94.0  800.0  702.5 -5.192715e-22
        a   94.0  800.0  703.0 -5.182389e-22
        a   94.0  800.0  704.0  5.473949e-22
        a   94.0  800.0  704.5  2.389177e-21
        a   94.0  800.0  703.5 -2.592409e-22
        a   94.0  800.0  701.5 -3.051780e-22
        a   94.0  800.0  700.5 -1.266145e-22
        a   94.0  800.0  700.0 -7.378603e-23
        # Simple Mesh Plot.
        >>> plot =  mesh.plot(x=mesh.x, y=mesh.y, scale=mesh.c,
                              kind='mesh', cmap='ice', vmin=-0.5,
                              vmax=0.5)
        # Display the plot.
        >>> plot.show()
        # Example 5: Wiggle Plot - This example creates simple wiggle plot.
        # Simple Wiggle Plot.
        # Create teradataml DataFrame object.
        >>> wiggle = DataFrame("waveletTable")
        # Print the DataFrame.
        >>> print(wiggle)
               x      t      y             c
        ID
        a   94.0  800.0  701.0 -2.034400e-22
        a   94.0  800.0  702.0 -4.217551e-22
        a   94.0  800.0  702.5 -5.192715e-22
        a   94.0  800.0  703.0 -5.182389e-22
        a   94.0  800.0  704.0  5.473949e-22
        a   94.0  800.0  704.5  2.389177e-21
        a   94.0  800.0  703.5 -2.592409e-22
        a   94.0  800.0  701.5 -3.051780e-22
        a   94.0  800.0  700.5 -1.266145e-22
        a   94.0  800.0  700.0 -7.378603e-23
        >>> plot =  wiggle.plot(x=wiggle.x, y=wiggle.y, scale=wiggle.c, kind='wiggle')
        # Display the plot.
        >>> plot.show()
        # Examples for subplot.
        # Example 1: This example creates a figure with subplot with scatter plots.
        # Load example data.
        >>> load_example_data("uaf", "house_values")
        # Create teradataml DataFrame objects.
        >>> house_values = DataFrame("house_values")
        # Print the DataFrame.
        >>> print(house_values)
                               TD_TIMECODE  house_val    salary  mortgage
        cityid
        33      2020-07-01 08:00:00.000000    66000.0   29000.0     0.039
        33      2020-04-01 08:00:00.000000    80000.0   22000.0     0.029
        33      2020-05-01 08:00:00.000000   184000.0   49000.0     0.030
        33      2020-06-01 08:00:00.000000   320000.0  112000.0     0.017
        33      2020-09-01 08:00:00.000000   195000.0   72000.0     0.049
        33      2020-10-01 08:00:00.000000   134000.0   89000.0     0.045
        33      2020-11-01 08:00:00.000000   198000.0   49000.0     0.052
        33      2020-08-01 08:00:00.000000   144000.0   74000.0     0.034
        33      2020-03-01 08:00:00.000000   220000.0   76000.0     0.035
        33      2020-02-01 08:00:00.000000   144000.0   50000.0     0.040
        # Import subplots.
        >>> from teradataml import subplots
        # This will help to create a figure with 2 subplots in 1 row.
        # fig and axes is passed to plot().
        >>> fig, axes = subplots(nrows=1, ncols=2)
        # Create plot with house_val, salary and salary and mortgage.
        >>> plot =  house_values.plot(x=house_values.house_val, y=house_values.salary,
                                      ax=axes[0], figure=fig, kind="scatter",
                                      xlim=(100000,250000), ylim=(25000, 100000),
                                      title="Scatter plot of House Val v/s Salary",
                                      color="green")
        >>> plot =  house_values.plot(x=house_values.salary, y=house_values.mortgage,
                                      ax=axes[1], figure=fig, kind="scatter",
                                      title="Scatter plot of House Val v/s Mortgage",
                                      color="red")
        # Show the plot.
        >>> plot.show()
        Example 2:
        # Subplot with grid. This will generate a figure with 2 subplots in first row
        # first column and second column respectively and 1 subplot in second row.
        >>> fig, axes = subplots(grid = {(1, 1): (1, 1), (1, 2): (1, 1),
                                         (2, 1): (1, 2)})
        # Print the DataFrame.
        >>> print(house_values)
                               TD_TIMECODE  house_val    salary  mortgage
        cityid
        33      2020-07-01 08:00:00.000000    66000.0   29000.0     0.039
        33      2020-04-01 08:00:00.000000    80000.0   22000.0     0.029
        33      2020-05-01 08:00:00.000000   184000.0   49000.0     0.030
        33      2020-06-01 08:00:00.000000   320000.0  112000.0     0.017
        33      2020-09-01 08:00:00.000000   195000.0   72000.0     0.049
        33      2020-10-01 08:00:00.000000   134000.0   89000.0     0.045
        33      2020-11-01 08:00:00.000000   198000.0   49000.0     0.052
        33      2020-08-01 08:00:00.000000   144000.0   74000.0     0.034
        33      2020-03-01 08:00:00.000000   220000.0   76000.0     0.035
        33      2020-02-01 08:00:00.000000   144000.0   50000.0     0.040
        # Create plot with house_val, salary and salary and mortgage.
        >>> plot =  house_values.plot(x=house_values.house_val, y=house_values.salary,
                                      ax=axes[0], figure=fig, kind="scatter",
                                      title="Scatter plot of House Val v/s Salary",
                                      color="green")
        >>> plot =  house_values.plot(x=house_values.salary, y=house_values.mortgage,
                                      ax=axes[1], figure=fig, kind="scatter",
                                      title="Scatter plot of Salary v/s Mortgage",
                                      color="red")
        >>> plot =  house_values.plot(x=house_values.salary, y=house_values.mortgage,
                                      ax=axes[2], figure=fig, kind="scatter",
                                      title="Scatter plot of House Val v/s Mortgage",
                                      color="blue")
        # Show the plot.
        >>> plot.show()
    replace
    teradataml.dataframe.dataframe.DataFrame.replace = replace(self, to_replace, value=None, subset=None)
    DESCRIPTION:
        Function replaces every occurrence of "to_replace" with the "value"
        in the columns mentioned in "subset". When "subset" is not provided,
        function replaces in all columns.
    PARAMETERS:
        to_replace:
            Required Argument.
            Specifies a ColumnExpression or a literal that the function
            searches for values in the Column. Use ColumnExpression when
            you want to match the condition based on a DataFrameColumn
            function, else use literal.
            Note:
                Only ColumnExpressions generated from DataFrameColumn
                functions are supported. BinaryExpressions are not supported.
                Example: Consider teradataml DataFrame has two columns COL1, COL2.
                         df.COL1.abs() is supported but df.COL1 == df.COL2 is not
                         supported.
            Supported column types: CHAR, VARCHAR, FLOAT, INTEGER, DECIMAL
            Types: ColumnExpression OR int OR float OR str OR dict
        value:
            Required argument when "to_replace" is not a dictionary. Optional otherwise.
            Specifies a ColumnExpression or a literal that replaces
            the "to_replace" in the column. Use ColumnExpression when
            you want to replace based on a DataFrameColumn function, else
            use literal.
            Notes:
                 * Argument is ignored if "to_replace" is a dictionary.
                 * Only ColumnExpressions generated from DataFrameColumn
                   functions are supported. BinaryExpressions are not supported.
                   Example: Consider teradataml DataFrame has two columns COL1, COL2.
                            df.COL1.abs() is supported but df.COL1 == df.COL2 is not
                            supported.
            Supported column types: CHAR, VARCHAR, FLOAT, INTEGER, DECIMAL
            Types: ColumnExpression OR int OR float OR str
        subset:
            Optional Argument.
            Specifies column(s) to consider for replacing the values.
            Types: ColumnExpression OR str OR list
    RAISES:
        TeradataMlException
    RETURNS:
        teradataml DataFrame
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train")
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        26     yes  3.57  Advanced    Advanced         1
        17      no  3.83  Advanced    Advanced         1
        # Example 1: Replace the string 'Advanced' with 'Good' in columns 'stats'
        #            and 'programming'.
        >>> res = df.replace("Advanced", "Good", subset=["stats", "programming"])
        >>> print(res)
           masters   gpa   stats programming  admitted
        id
        13      no  4.00    Good      Novice         1
        36      no  3.00    Good      Novice         0
        15     yes  4.00    Good        Good         1
        40     yes  3.95  Novice    Beginner         0
        22     yes  3.46  Novice    Beginner         0
        38     yes  2.65    Good    Beginner         1
        26     yes  3.57    Good        Good         1
        5       no  3.44  Novice      Novice         0
        7      yes  2.33  Novice      Novice         1
        19     yes  1.98    Good        Good         0
        # Example 2: Replace the string 'Advanced' with 'Good' and 'Beginner' with 'starter'
        #            in columns 'stats' and 'programming'.
        >>> res = df.replace({"Advanced": "Good", "Beginner": "starter"}, subset=["stats", "programming"])
        >>> print(res)
           masters   gpa   stats programming  admitted
        id
        15     yes  4.00    Good        Good         1
        7      yes  2.33  Novice      Novice         1
        22     yes  3.46  Novice     starter         0
        17      no  3.83    Good        Good         1
        13      no  4.00    Good      Novice         1
        38     yes  2.65    Good     starter         1
        26     yes  3.57    Good        Good         1
        5       no  3.44  Novice      Novice         0
        34     yes  3.85    Good     starter         0
        40     yes  3.95  Novice     starter         0
        # Example 3: Append the string '_New' to 'stats' column when values in
        #           'programming' and 'stats' are same.
        >>> res = df.replace({df.programming: df.stats+"_New"}, subset=["stats"])
        >>> print(res)
           masters   gpa         stats programming  admitted
        id
        15     yes  4.00  Advanced_New    Advanced         1
        34     yes  3.85      Advanced    Beginner         0
        13      no  4.00      Advanced      Novice         1
        38     yes  2.65      Advanced    Beginner         1
        5       no  3.44    Novice_New      Novice         0
        40     yes  3.95        Novice    Beginner         0
        7      yes  2.33    Novice_New      Novice         1
        22     yes  3.46        Novice    Beginner         0
        26     yes  3.57  Advanced_New    Advanced         1
        17      no  3.83  Advanced_New    Advanced         1
        # Example 4: Round the values of gpa to it's nearest integer.
        >>> res = df.replace({df.gpa: df.gpa.round(0)}, subset=["gpa"])
        >>> print(res)
           masters  gpa     stats programming  admitted
        id
        15     yes  4.0  Advanced    Advanced         1
        7      yes  2.0    Novice      Novice         1
        22     yes  3.0    Novice    Beginner         0
        17      no  4.0  Advanced    Advanced         1
        13      no  4.0  Advanced      Novice         1
        38     yes  3.0  Advanced    Beginner         1
        26     yes  4.0  Advanced    Advanced         1
        5       no  3.0    Novice      Novice         0
        34     yes  4.0  Advanced    Beginner         0
        40     yes  4.0    Novice    Beginner         0
        # Example 5: Replace the value of masters with '1' if value is 'yes'
        #            and with '0' if value is no.
        >>> res = df.replace({'yes': 1, 'no': 0}, subset=["masters"])
        >>> print(res)
           masters   gpa     stats programming  admitted
        id
        15       1  4.00  Advanced    Advanced         1
        7        1  2.33    Novice      Novice         1
        22       1  3.46    Novice    Beginner         0
        17       0  3.83  Advanced    Advanced         1
        13       0  4.00  Advanced      Novice         1
        38       1  2.65  Advanced    Beginner         1
        26       1  3.57  Advanced    Advanced         1
        5        0  3.44    Novice      Novice         0
        34       1  3.85  Advanced    Beginner         0
        40       1  3.95    Novice    Beginner         0
    resample
    teradataml.dataframe.dataframe.DataFrame.resample = resample(self, rule, value_expression=None, on=None, sequence_column=None, ﬁll_method=None)
    DESCRIPTION:
        Resample time series data. This function allows grouping done by time on
        a datetime column of a teradataml DataFrame. Outcome of this function
        can be used to run Time Series Aggregate functions.
    PARAMETERS:
        rule:
            Required Argument.
            Specifies the time unit duration/interval of each timebucket for resampling and is used to
            assign each potential timebucket a unique number.
            Permitted Values:
                 ===================================================================================================
                | Time Units        | Formal Form       | Shorthand Equivalents for time_units                      |
                 ===================================================================================================
                | Calendar Years    | CAL_YEARS(N)      | Ncy OR Ncyear OR Ncyears                                  |
                 ---------------------------------------------------------------------------------------------------
                | Calendar Months   | CAL_MONTHS(N)     | Ncm OR Ncmonth OR Ncmonths                                |
                 ---------------------------------------------------------------------------------------------------
                | Calendar Days     | CAL_DAYS(N)       | Ncd OR Ncday OR Ncdays                                    |
                 ---------------------------------------------------------------------------------------------------
                | Weeks             | WEEKS(N)          | Nw  OR Nweek OR Nweeks                                    |
                 ---------------------------------------------------------------------------------------------------
                | Days              | DAYS(N)           | Nd  OR Nday OR Ndays                                      |
                 ---------------------------------------------------------------------------------------------------
                | Hours             | HOURS(N)          | Nh  OR Nhr OR Nhrs OR Nhour OR Nhours                     |
                 ---------------------------------------------------------------------------------------------------
                | Minutes           | MINUTES(N)        | Nm  OR Nmins OR Nminute OR Nminutes                       |
                 ---------------------------------------------------------------------------------------------------
                | Seconds           | SECONDS(N)        | Ns  OR Nsec OR Nsecs OR Nsecond OR Nseconds               |
                 ---------------------------------------------------------------------------------------------------
                | Milliseconds      | MILLISECONDS(N)   | Nms OR Nmsec OR Nmsecs OR Nmillisecond OR Nmilliseconds   |
                 ---------------------------------------------------------------------------------------------------
                | Microseconds      | MICROSECONDS(N)   | Nus OR Nusec OR Nusecs OR Nmicrosecond OR Nmicroseconds   |
                 ===================================================================================================
                Where, N is a 16-bit positive integer with a maximum value of 32767.
                Notes:
                    1. When timebucket_duration is Calendar Days, it will group the columns in
                       24 hour periods starting at 00:00:00.000000 and ending at 23:59:59.999999 on
                       the day identified by time zero.
                    2. A DAYS time unit is a 24 hour span relative to any moment in time.
                       For example,
                            If time zero (in teradataml DataFraame created on PTI tables) equal to
                            2016-10-01 12:00:00, the day buckets are:
                                2016-10-01 12:00:00.000000 - 2016-10-02 11:59:59.999999.
                            This spans multiple calendar days, but encompasses one 24 hour period
                            representative of a day.
                    3. The time units do not store values such as the year or the month.
                       For example,
                            CAL_YEARS(2017) does not set the year to 2017. It sets the timebucket_duration
                            to intervals of 2017 years. Similarly, CAL_MONTHS(7) does not set the month to
                            July. It sets the timebucket_duration to intervals of 7 months.
            Types: str
            Example: MINUTES(23) which is equal to 23 Minutes
                     CAL_MONTHS(5) which is equal to 5 calendar months
        value_expression:
            Optional Argument.
            Specifies a column used for grouping purposes not related to time.
            Types: str or List of Strings
            Example: col1 or ["col1", "col2"]
        on:
            Optional Argument.
            Specifies a column that serves as the timecode for a non-PTI table. This is the column
            used for resampling time series data.
            For teradataml DataFrame created on PTI table:
                TD_TIMECODE is used implicitly for PTI tables, but can also be specified explicitly
                by the user with this parameter.
            For teradataml DataFrame created on non-PTI table:
                Column must be specified to this argument if DataFrame is created on non-PTI table,
                otherwise, an exception is raised.
            Types: str
        sequence_column:
            Optional Argument.
            Specifies a column that is the sequence number.
            For teradataml DataFrame created on PTI table:
                It can be TD_SEQNO or any other column that acts as a sequence number.
            For teradataml DataFrame created on non-PTI table:
                sequence_column is a column that plays the role of TD_SEQNO, because non-PTI
                tables do not have TD_SEQNO.
            Types: str
        fill_method:
            Optional Argument.
            Specifies values for missing timebucket values.
            Permitted values: NULLS, PREV / PREVIOUS, NEXT, and any numeric_constant
                NULLS:
                    The missing timebuckets are returned to the user with a null value for all
                    aggregate results.
                numeric_constant:
                    Any Teradata Database supported Numeric literal. The missing timebuckets
                    are returned to the user with the specified constant value for all
                    aggregate results. If the data type specified in the fill_method argument is
                    incompatible with the input data type for an aggregate function,
                    an error is reported.
                PREVIOUS/PREV:
                    The missing timebuckets are returned to the user with the aggregate results
                    populated by the value of the closest previous timebucket with a non-missing
                    value. If the immediate predecessor of a missing timebucket is also missing,
                    both buckets, and any other immediate predecessors with missing values,
                    are loaded with the first preceding non-missing value. If a missing
                    timebucket has no predecessor with a result (for example, if the
                    timebucket is the first in the series or all the preceding timebuckets in
                    the entire series are missing), the missing timebuckets are returned to the
                    user with a null value for all aggregate results. The abbreviation PREV may
                    be used instead of PREVIOUS.
                NEXT:
                    The missing timebuckets are returned to the user with the aggregate results populated
                    by the value of the closest succeeding timebucket with a non-missing value. If the
                    immediate successor of a missing timebucket is also missing, both buckets, and any
                    other immediate successors with missing values, are loaded with the first succeeding
                    non-missing value. If a missing timebucket has no successor with a result
                    (for example, if the timebucket is the last in the series or all the succeeding
                    timebuckets in the entire series are missing), the missing timebuckets are returned
                    to the user with a null value for all aggregate results.
            Types: str or int or float
    NOTES:
        1. This API is similar to groupby_time().
        2. Users can still apply teradataml DataFrame methods (filters/sort/etc) on top of the result.
        3. Consecutive operations of grouping, i.e., groupby_time(), resample() and groupby() are not permitted.
           An exception will be raised. Following are some cases where exception will be raised as
           "Invalid operation applied, check documentation for correct usage."
                a. df.resample().groupby()
                b. df.resample().resample()
                c. df.resample().groupby_time()
    RETURNS:
        teradataml DataFrameGroupBy Object
    RAISES:
        TypeError, ValueError, TeradataMLException
    EXAMPLES:
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_nonpti"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        >>> # DataFrame on NON-PTI table
        ... ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_nonpti.columns
        ['buoyid', 'timecode', 'temperature', 'salinity']
        >>> ocean_buoys_nonpti.head()
                                    buoyid  temperature  salinity
        timecode
        2014-01-06 08:09:59.999999       0         99.0        55
        2014-01-06 08:10:00.000000       0         10.0        55
        2014-01-06 09:01:25.122200       1         70.0        55
        2014-01-06 09:01:25.122200       1         77.0        55
        2014-01-06 09:02:25.122200       1         71.0        55
        2014-01-06 09:03:25.122200       1         72.0        55
        2014-01-06 09:02:25.122200       1         78.0        55
        2014-01-06 08:10:00.000000       0        100.0        55
        2014-01-06 08:08:59.999999       0          NaN        55
        2014-01-06 08:00:00.000000       0         10.0        55
        #
        # Example 1: Resample by timebucket of 2 calendar years, using formal notation and buoyid column on
        #            DataFrame created on non-sequenced PTI table.
        #            Fill missing values with Nulls.
        #
        >>> ocean_buoys_grpby1 = ocean_buoys.resample(rule="CAL_YEARS(2)", value_expression="buoyid", fill_method="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_grpby1.bottom(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  bottom2temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0                  10
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0                  10
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                  71
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                  70
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2                  80
        5  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2                  81
        6  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44                  43
        7  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44                  43
        >>>
        #
        # Example 2: Resample data by timebucket of 2 minutes, using shorthand notation to specify timebucket,
        #            on DataFrame created on non-PTI table. Fill missing values with Nulls.
        #            Time column must be specified for non-PTI table using 'on' argument.
        #
        >>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.resample(rule="2m", value_expression="buoyid",
        ...                                                         on="timecode", fill_method="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE",
        ...                                                                                    "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961       0                          10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962       0                           NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963       0                           NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964       0                           NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965       0                          99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                         100.0
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                          10.0
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                          70.0
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                          77.0
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992       1                          71.0
        >>>
        #
        # Example 3: Resample time series data by timebucket of 2 minutes, using shorthand notation to specify
        #            timebucket, on teradataml DataFrame created on non-PTI table. Fill missing values with
        #            previous values. Time column must be specified for non-PTI table using 'on' argument.
        #
        >>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.resample(rule="2mins", value_expression="buoyid",
        ...                                                         on="timecode", fill_method="prev")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE",
        ...                                                                                    "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961       0                            10
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962       0                            10
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963       0                            10
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964       0                            10
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965       0                            99
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                            10
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                           100
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                            77
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                            70
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992       1                            71
        #
        # Example 4: Resample time series data by timebucket of 2 minutes, using shorthand notation to specify
        #            timebucket, on teradataml DataFrame created on non-PTI table. Fill missing values with
        #            numeric 12345. Time column must be specified for non-PTI table using 'on' argument.
        #
        >>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.resample(rule="2minute", value_expression="buoyid",
        ...                                                         on="timecode", fill_method=12345)
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE",
        ...                                                                                    "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961       0                            10
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962       0                         12345
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963       0                         12345
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964       0                         12345
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965       0                            99
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                            10
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                           100
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                            77
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                            70
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992       1                            71
        >>>
    rollup
    teradataml.dataframe.dataframe.DataFrame.rollup = rollup(self, columns, include_grouping_columns=False)
    DESCRIPTION:
        rollup() function creates a multi-dimensional rollup for the DataFrame
        using the specified column(s), and there by running aggregates on
        it to produce the aggregations on different dimensions.
    PARAMETERS:
        columns:
            Required Argument.
            Specifies the name(s) of input teradataml DataFrame column(s).
            Types: str OR list of str(s)
        include_grouping_columns:
            Optional Argument.
            Specifies whether to include aggregations on the grouping column(s) or not.
            When set to True, the resultant DataFrame will have the aggregations on the 
            columns mentioned in "columns". Otherwise, resultant DataFrame will not have 
            aggregations on the columns mentioned in "columns".
            Default Value: False
            Types: bool
    RETURNS:
        teradataml DataFrameGroupBy
    RAISES:
        TeradataMlException
    EXAMPLES :
        # Load the data to run the example.
        >>> load_example_data("dataframe","admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        26     yes  3.57  Advanced    Advanced         1
        17      no  3.83  Advanced    Advanced         1
        # Example 1: Find the sum of all valid columns by grouping the
        #            DataFrame columns with 'masters' and 'stats'.
        >>> df1 = df.rollup(["masters", "stats"]).sum()
        >>> df1
          masters     stats  sum_id  sum_gpa  sum_admitted
        0      no      None     343    63.96            16
        1     yes      None     477    77.71            10
        2    None      None     820   141.67            26
        3      no    Novice     146    25.41             6
        4      no  Beginner       8     3.60             1
        5     yes    Novice      98    13.74             1
        6     yes  Beginner      13    14.71             2
        7     yes  Advanced     366    49.26             7
        8      no  Advanced     189    34.95             9
        # Example 2: Find the avg of all valid columns by grouping the DataFrame
        #            with columns 'masters' and 'admitted'. Include grouping columns
        #            in aggregate function 'avg'.
        >>> df1 = df.rollup(["masters", "admitted"], include_grouping_columns=True).avg()
        >>> df1
          masters  admitted     avg_id   avg_gpa  avg_admitted
        0      no       NaN  19.055556  3.553333      0.888889
        1     yes       NaN  21.681818  3.532273      0.454545
        2    None       NaN  20.500000  3.541750      0.650000
        3     yes       0.0  24.083333  3.613333      0.000000
        4      no       1.0  18.875000  3.595000      1.000000
        5     yes       1.0  18.800000  3.435000      1.000000
        6      no       0.0  20.500000  3.220000      0.000000
        # Example 3: Find the avg of all valid columns by grouping the DataFrame with
        #            columns 'masters' and 'admitted'. Do not include grouping columns
        #            in aggregate function 'avg'.
        >>> df1 = df.rollup(["masters", "admitted"], include_grouping_columns=False).avg()
        >>> df1
          masters  admitted     avg_id   avg_gpa
        0      no       NaN  19.055556  3.553333
        1     yes       NaN  21.681818  3.532273
        2      no       0.0  20.500000  3.220000
        3     yes       0.0  24.083333  3.613333
        4      no       1.0  18.875000  3.595000
        5     yes       1.0  18.800000  3.435000
        6    None       NaN  20.500000  3.541750
    sample
    teradataml.dataframe.dataframe.DataFrame.sample = sample(self, n=None, frac=None, replace=False, randomize=False, case_when_then=None, case_else=None,
    stratify_column=None, seed=None, id_column=None)
    DESCRIPTION:
        Allows to sample few rows from dataframe directly or based on conditions.
        Creates a new column 'sampleid' which has a unique id for each sample
        sampled, it helps to uniquely identify each sample.
    PARAMETERS:
        n:
            Required Argument, if neither of 'frac' and 'case_when_then' are specified.
            Specifies a set of positive integer constants that specifies the number of 
            rows to be sampled from the teradataml DataFrame.
            Example:
                n = 10 or n = [10] or n = [10, 20, 30, 40]
            Default Value: None
            Types: int or list of ints.
            Note:
                1. You should use only one of the following arguments: 'n', 'frac' and 'case_when_then'.
                2. No more than 16 samples can be requested per count description.
        frac:
            Required Argument, if neither of 'n' and 'case_when_then' are specified.
            Specifies any set of unsigned floating point constant numbers in the half
            opened interval (0,1] that means greater than 0 and less than or equal to 1.
            It specifies the percentage of rows to be sampled from the teradataml DataFrame.
            Example:
                frac = 0.4 or frac = [0.4] or frac = [0.2, 0.5]
            Default Value: None
            Types: float or list of floats.
            Note:
                1. You should use only one of the following arguments: 'n', 'frac' and 'case_when_then'.
                2. No more than 16 samples can be requested per count description.
                3. Sum of elements in list should not be greater than 1 as total percentage cannot be
                   more than 100% and should not be less than or equal to 0.
                4. Stratifying data sample is supported only when "stratify_column" 
                   is used with "frac" argument.
                5. List sizes must include a minimum of one float element and a maximum of two elements
                   when data sampled with stratification. The train data sample percentage 
                   corresponds to the first element, whereas the test data sample percentage is
                   associated with the second element.
                6. The remaining fraction is considered for sampling the data when "frac" has 
                   only one fraction for data sampling with stratification.
        replace:
            Optional Argument.
            Specifies if sampling should be done with replacement or not.
            Default Value: False
            Types: bool
        randomize:
            Optional Argument.
            Specifies if sampling should be done across AMPs in Teradata or per AMP.
            Default Value: False
            Types: bool
        case_when_then :
            Required Argument, if neither of 'frac' and 'n' are specified.
            Specifies condition and number of samples to be sampled as key value pairs.
            Keys should be of type ColumnExpressions.
            Values should be either of type int, float, list of ints or list of floats.
            The following usage of key is not allowed:
                case_when_then = {"gpa" > 2 : 2}
            The following operators are supported:
                  comparison: ==, !=, <, <=, >, >=
                  boolean: & (and), | (or), ~ (not), ^ (xor)
            Example :
                  case_when_then = {df.gpa > 2 : 2}
                  case_when_then = {df.gpa > 2 & df.stats == 'Novice' : [0.2, 0.3],
                                   df.programming == 'Advanced' : [10,20,30]}
            Default Value: None
            Types: dictionary
            Note:
                1. You should use only one of the following arguments: 'n', 'frac' and 'case_when_then'.
                2. No more than 16 samples can be requested per fraction description or count description.
                3. If any value in dictionary is specified as list of floats then
                   sum of elements in list should not be greater than 1 as total percentage cannot be
                   more than 100% and should not be less than or equal to 0.
        case_else :
            Optional Argument.
            Specifies number of samples to be sampled from rows where none of the conditions in
            'case_when_then' are met.
            Example :
                case_else = 10
                case_else = [10,20]
                case_else = [0.5]
                case_else = [0.2,0.4]
            Default Value: None
            Types: int or float or list of ints or list of floats
            Note:
                1. This argument can only be used with 'case_when_then'. 
                   If used otherwise, below error will raised.
                       'case_else' can only be used when 'case_when_then' is specified.
                2. No more than 16 samples can be requested per fraction description 
                   or count description.
                3. If case_else is list of floats then sum of elements in list should not be 
                   greater than 1 as total percentage cannot be more than 100% and should not 
                   be less than or equal to 0.
        stratify_column:
            Optional Argument.
            Specifies column name that contains the labels indicating
            which data needs to be stratified.
            Notes:
                1. Must be used with "frac" argument for stratifying data.
                2. seed is supported for stratify column.
                3. Arguments "stratify_column", "seed", "id_column" are supported only 
                   for stratifying the data.
            Types: str OR Feature
        seed:
            Optional Argument.
            Specifies the seed value which controls the data sample. The sample remains 
            same as long as the seed remains same. Use this argument to get the 
            deterministic samples. "seed" must be greater than or equal to 0 and 
            less than or equal to 2147483647.
            Notes:
                1. Random seed is generated internally when argument 
                   is not specified.
                2. Seed is supported only when only when "stratify_column" is used. 
                   Ignored otherwise. 
                3. Arguments "stratify_column", "seed", "id_column" are supported only 
                   for stratifying the data.
            Types: int
        id_column:
            Required when "seed" is used. Optional otherwise.
            Specifies the input data column name that has the
            unique identifier for each row in the input.
            Notes:
                1. Arguments "stratify_column", "seed", "id_column" are supported only 
                   for stratifying the data.
                2. "id_column" is supported only when "stratify_column" is used. 
                   Ignored otherwise.
            Types: str OR Feature
    RETURNS:
        teradataml DataFrame
    RAISES:
        1. ValueError - When columns of different dataframes are given in ColumnExpression.
                         or
                        When columns are given in string format and not ColumnExpression.
        2. TeradataMlException - If types of input parameters are mismatched.
        3. TypeError
    Examples:
        >>> from teradataml import *
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        # Print dataframe.
        >>> df
              masters   gpa     stats programming admitted
           id
           13      no  4.00  Advanced      Novice        1
           26     yes  3.57  Advanced    Advanced        1
           5       no  3.44    Novice      Novice        0
           19     yes  1.98  Advanced    Advanced        0
           15     yes  4.00  Advanced    Advanced        1
           40     yes  3.95    Novice    Beginner        0
           7      yes  2.33    Novice      Novice        1
           22     yes  3.46    Novice    Beginner        0
           36      no  3.00  Advanced      Novice        0
           38     yes  2.65  Advanced    Beginner        1
        # Sample with only n argument.
        # Randomly samples 2 rows from the teradataml DataFrame.
        # As there is only 1 sample 'sampleid' is 1.
        >>> df.sample(n = 2)
              masters   gpa     stats programming admitted SampleId
           id
           18     yes  3.81  Advanced    Advanced        1        1
           19     yes  1.98  Advanced    Advanced        0        1
        # Sample with multiple sample values for n.
        # Creates 2 samples with 2 and 1 rows each respectively.
        # There are 2 values(1,2) for 'sampleid' each for one sample.
        >>> df.sample(n = [2, 1])
              masters   gpa     stats programming admitted SampleId
           id
           1      yes  3.95  Beginner    Beginner        0        1
           10      no  3.71  Advanced    Advanced        1        1
           11      no  3.13  Advanced    Advanced        1        2
        # Sample with only frac parameter.
        # Randomly samples 20% of total rows present in teradataml DataFrame.
        >>> df.sample(frac = 0.2)
              masters   gpa     stats programming admitted SampleId
           id
           18     yes  3.81  Advanced    Advanced        1        1
           15     yes  4.00  Advanced    Advanced        1        1
           14     yes  3.45  Advanced    Advanced        0        1
           35      no  3.68    Novice    Beginner        1        1
           27     yes  3.96  Advanced    Advanced        0        1
           25      no  3.96  Advanced    Advanced        1        1
           10      no  3.71  Advanced    Advanced        1        1
           9       no  3.82  Advanced    Advanced        1        1
        # Sample with multiple sample values for frac.
        # Creates 2 samples each with 4% and 2% of total rows in teradataml DataFrame.
        >>> df.sample(frac = [0.04, 0.02])
              masters   gpa     stats programming admitted SampleId
           id
           29     yes  4.00    Novice    Beginner        0        1
           19     yes  1.98  Advanced    Advanced        0        2
           11      no  3.13  Advanced    Advanced        1        1
        # Sample with n and replace and randomization.
        # Creates 2 samples with 2 and 1 rows respectively with possible redundant
        # sampling as replace is True and also selects rows from different AMPS as
        # randomize is True.
        >>> df.sample(n = [2, 1], replace = True, randomize = True)
              masters   gpa     stats programming admitted SampleId
           id
           12      no  3.65    Novice      Novice        1        1
           39     yes  3.75  Advanced    Beginner        0        1
           20     yes  3.90  Advanced    Advanced        1        2
        # Sample with frac and replace and randomization.
        # Creates 2 samples with 4% and 2% of total rows in teradataml DataFrame
        # respectively with possible redundant sampling and also selects rows from different AMPS.
        >>> df.sample(frac = [0.04, 0.02], replace = True, randomize = True)
              masters   gpa     stats programming admitted SampleId
           id
           7      yes  2.33    Novice      Novice        1        2
           30     yes  3.79  Advanced      Novice        0        1
           33      no  3.55    Novice      Novice        1        1
        # Sample with case_when_then.
        # Creates 2 samples with 1, 2 rows respectively from rows which satisfy df.gpa < 2
        # and 2.5% of rows from rows which satisfy df.stats == 'Advanced'.
        >>> df.sample(case_when_then={df.gpa < 2 : [1, 2], df.stats == 'Advanced' : 0.025})
              masters   gpa     stats programming admitted SampleId
           id
           19     yes  1.98  Advanced    Advanced        0        1
           24      no  1.87  Advanced      Novice        1        1
           11      no  3.13  Advanced    Advanced        1        3
        # Sample with case_when_then and replace, randomize.
        # Creates 2 samples with 1, 2 rows respectively from rows which satisfy df.gpa < 2
        # and 2.5% of rows from rows which satisfy df.stats == 'Advanced' and selects rows
        # from different AMPs with replacement.
        >>> df.sample(replace = True, randomize = True, case_when_then={df.gpa < 2 : [1, 2],
                                                                       df.stats == 'Advanced' : 0.025})
              masters   gpa     stats programming admitted SampleId
           id
           24      no  1.87  Advanced      Novice        1        1
           24      no  1.87  Advanced      Novice        1        2
           24      no  1.87  Advanced      Novice        1        2
           24      no  1.87  Advanced      Novice        1        2
           24      no  1.87  Advanced      Novice        1        2
           24      no  1.87  Advanced      Novice        1        1
           31     yes  3.50  Advanced    Beginner        1        3
        # Sample with case_when_then and case_else.
        # Creates 7 samples 2 with 1, 3 rows from rows which satisfy df.gpa > 2.
        # 1 sample with 5 rows from rows which satisify df.programming == 'Novice'.
        # 1 sample with 5 rows from rows which satisify df.masters == 'no'.
        # 1 sample with 1 row from rows which does not meet all above conditions.
        >>> df.sample(case_when_then = {df.gpa > 2 : [1, 3], df.stats == 'Novice' : [1, 2],
                                       df.programming == 'Novice' : 5, df.masters == 'no': 5}, case_else = 1)
              masters   gpa     stats programming admitted SampleId
           id
           24      no  1.87  Advanced      Novice        1        5
           2      yes  3.76  Beginner    Beginner        0        1
           12      no  3.65    Novice      Novice        1        2
           38     yes  2.65  Advanced    Beginner        1        2
           36      no  3.00  Advanced      Novice        0        2
           19     yes  1.98  Advanced    Advanced        0        7
        # Sample with case_when_then and case_else
        # Creates 4 samples 2 with 1, 3 rows from rows which satisfy df.gpa > 2.
        # 2 samples with 2.5%, 5% of rows from all the rows which does not
        # meet condition df.gpa < 2.
        >>> df.sample(case_when_then = {df.gpa < 2 : [1, 3]}, case_else = [0.025, 0.05])
              masters   gpa     stats programming admitted SampleId
           id
           9       no  3.82  Advanced    Advanced        1        4
           24      no  1.87  Advanced      Novice        1        1
           26     yes  3.57  Advanced    Advanced        1        4
           13      no  4.00  Advanced      Novice        1        3
           19     yes  1.98  Advanced    Advanced        0        1
        # Sample with case_when_then, case_else, replace, randomize
        # Creates 4 samples 2 with 1, 3 rows from rows which satisfy df.gpa > 2 and
        # 2 samples with 2.5%, 5% of rows from all the rows which does not
        # meet condition df.gpa < 2  with possible redundant replacement
        # and also selects rows from different AMPs
        >>> df.sample(case_when_then = {df.gpa < 2 : [1, 3]}, replace = True,
                    randomize = True, case_else = [0.025, 0.05])
              masters   gpa     stats programming admitted SampleId
           id
           19     yes  1.98  Advanced    Advanced        0        1
           19     yes  1.98  Advanced    Advanced        0        2
           19     yes  1.98  Advanced    Advanced        0        2
           19     yes  1.98  Advanced    Advanced        0        2
           19     yes  1.98  Advanced    Advanced        0        2
           40     yes  3.95    Novice    Beginner        0        3
           3       no  3.70    Novice    Beginner        1        4
           19     yes  1.98  Advanced    Advanced        0        2
           19     yes  1.98  Advanced    Advanced        0        2
           19     yes  1.98  Advanced    Advanced        0        1
    select
    teradataml.dataframe.dataframe.DataFrame.select = select(self, select_expression)
    DESCRIPTION:
        Select required columns from DataFrame using an expression.
        Returns a new teradataml DataFrame with selected columns only.
    PARAMETERS:
        select_expression:
            Required Argument.
            String or List representing columns to select.
            Types: str OR List of Strings (str)
            The following formats (only) are supported for select_expression:
            A] Single Column String: df.select("col1")
            B] Single Column List: df.select(["col1"])
            C] Multi-Column List: df.select(['col1', 'col2', 'col3'])
            D] Multi-Column List of List: df.select([["col1", "col2", "col3"]])
            Column Names ("col1", "col2"..) are Strings representing Teradata Vantage table Columns.
            All Standard Teradata data types for columns supported: INTEGER, VARCHAR(5), FLOAT.
            Note: Multi-Column selection of the same column such as df.select(['col1', 'col1']) is not supported.
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException (TDMLDF_SELECT_INVALID_COLUMN, TDMLDF_SELECT_INVALID_FORMAT,
                             TDMLDF_SELECT_DF_FAIL, TDMLDF_SELECT_EXPR_UNSPECIFIED,
                             TDMLDF_SELECT_NONE_OR_EMPTY)
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame('admissions_train')
        >>> df
           masters   gpa     stats programming admitted
        id
        5       no  3.44    Novice      Novice        0
        7      yes  2.33    Novice      Novice        1
        22     yes  3.46    Novice    Beginner        0
        17      no  3.83  Advanced    Advanced        1
        13      no  4.00  Advanced      Novice        1
        19     yes  1.98  Advanced    Advanced        0
        36      no  3.00  Advanced      Novice        0
        15     yes  4.00  Advanced    Advanced        1
        34     yes  3.85  Advanced    Beginner        0
        40     yes  3.95    Novice    Beginner        0
        A] Single String Column
        >>> df.select("id")
        Empty DataFrame
        Columns: []
        Index: [22, 34, 13, 19, 15, 38, 26, 5, 36, 17]
        B] Single Column List
        >>> df.select(["id"])
        Empty DataFrame
        Columns: []
        Index: [15, 26, 5, 40, 22, 17, 34, 13, 7, 38]
        C] Multi-Column List
        >>> df.select(["id", "masters", "gpa"])
           masters   gpa
        id
        5       no  3.44
        36      no  3.00
        15     yes  4.00
        17      no  3.83
        13      no  4.00
        40     yes  3.95
        7      yes  2.33
        22     yes  3.46
        34     yes  3.85
        19     yes  1.98
        D] Multi-Column List of List
        >>> df.select([['id', 'masters', 'gpa']])
           masters   gpa
        id
        5       no  3.44
        34     yes  3.85
        13      no  4.00
        40     yes  3.95
        22     yes  3.46
        19     yes  1.98
        36      no  3.00
        15     yes  4.00
        7      yes  2.33
        17      no  3.83
    set_index
    teradataml.dataframe.dataframe.DataFrame.set_index = set_index(self, keys, drop=True, append=False)
        DESCRIPTION:
            Assigns one or more existing columns as the new index to a teradataml DataFrame.
        PARAMETERS:
            keys:
                Required Argument.
                Specifies the column name or a list of column names to use as the DataFrame index.
                Types: str OR list of Strings (str)
            drop:
                Optional Argument.
                Specifies whether or not to display the column(s) being set as index as
                teradataml DataFrame columns anymore.
                When drop is True, columns are set as index and not displayed as columns.
                When drop is False, columns are set as index; but also displayed as columns.
                Note: When the drop argument is set to True, the column being set as index does not cease to
                      be a part of the underlying table upon which the teradataml DataFrame is based off.
                      A column that is dropped while being set as an index is merely not used for display
                      purposes anymore as a column of the teradataml DataFrame.
                Default Value: True
                Types: bool
            append:
                Optional Argument.
                Specifies whether or not to append requested columns to the existing index.
    `           When append is False, replaces existing index.
                When append is True, retains both existing & currently appended index.
                Default Value: False
                Types: bool
        RETURNS:
            teradataml DataFrame
        RAISES:
            TeradataMlException
        EXAMPLES:
            >>> load_example_data("dataframe","admissions_train")
            >>> df = DataFrame("admissions_train")
            >>> df.sort('id')
               masters   gpa     stats programming admitted
            id
            1      yes  3.95  Beginner    Beginner        0
            2      yes  3.76  Beginner    Beginner        0
            3       no  3.70    Novice    Beginner        1
            4      yes  3.50  Beginner      Novice        1
            5       no  3.44    Novice      Novice        0
            6      yes  3.50  Beginner    Advanced        1
            7      yes  2.33    Novice      Novice        1
            8       no  3.60  Beginner    Advanced        1
            9       no  3.82  Advanced    Advanced        1
            10      no  3.71  Advanced    Advanced        1
            >>> # Set new index.
            >>> df.set_index('masters').sort('id')
                     id   gpa     stats programming admitted
            masters
            yes       1  3.95  Beginner    Beginner        0
            yes       2  3.76  Beginner    Beginner        0
            no        3  3.70    Novice    Beginner        1
            yes       4  3.50  Beginner      Novice        1
            no        5  3.44    Novice      Novice        0
            yes       6  3.50  Beginner    Advanced        1
            yes       7  2.33    Novice      Novice        1
            no        8  3.60  Beginner    Advanced        1
            no        9  3.82  Advanced    Advanced        1
            no       10  3.71  Advanced    Advanced        1
            >>> # Set multiple indexes using list of columns
            >>> df.set_index(['masters', 'id']).sort('id')
                         gpa     stats programming admitted
            id masters
            1  yes      3.95  Beginner    Beginner        0
            2  yes      3.76  Beginner    Beginner        0
            3  no       3.70    Novice    Beginner        1
            4  yes      3.50  Beginner      Novice        1
            5  no       3.44    Novice      Novice        0
            6  yes      3.50  Beginner    Advanced        1
            7  yes      2.33    Novice      Novice        1
            8  no       3.60  Beginner    Advanced        1
            9  no       3.82  Advanced    Advanced        1
            10 no       3.71  Advanced    Advanced        1
            >>> # Append to new index to the existing set of index.
            >>> df.set_index(['masters', 'id']).set_index('gpa', drop = False, append = True).sort('id')
                                stats programming admitted
            gpa  masters id
            3.95 yes     1   Beginner    Beginner        0
            3.76 yes     2   Beginner    Beginner        0
            3.70 no      3     Novice    Beginner        1
            3.50 yes     4   Beginner      Novice        1
            3.44 no      5     Novice      Novice        0
            3.50 yes     6   Beginner    Advanced        1
            2.33 yes     7     Novice      Novice        1
            3.60 no      8   Beginner    Advanced        1
            3.82 no      9   Advanced    Advanced        1
            3.71 no      10  Advanced    Advanced        1
            >>>
    shape
    teradataml.dataframe.dataframe.DataFrame.shape
    Returns a tuple representing the dimensionality of the DataFrame.
    PARAMETERS:
        None
    RETURNS:
        Tuple representing the dimensionality of this DataFrame.
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> df
                      Feb   Jan   Mar   Apr  datetime
        accounts
        Orange Inc  210.0  None  None   250  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        Blue Inc     90.0    50    95   101  04/01/2017
        Jones LLC   200.0   150   140   180  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        >>> df.shape
        (6, 6)
        >>>
    RAISES:
        TeradataMlException (TDMLDF_INFO_ERROR)
    show_query
    teradataml.dataframe.dataframe.DataFrame.show_query = show_query(self, full_query=False)
    DESCRIPTION:
        Function returns underlying SQL for the teradataml DataFrame. It is the same 
        SQL that is used to view the data for a teradataml DataFrame.
    PARAMETERS:
        full_query:
            Optional Argument.
            Specifies if the complete query for the dataframe should be returned.
            When this parameter is set to True, query for the dataframe is returned
            with respect to the base dataframe's table (from_table() or from_query()) or from the
            output tables of analytical functions (if there are any in the workflow).
            This query may or may not be directly used to retrieve data for the dataframe upon
            which the function is called.
            When this parameter is not used, string returned is the query already used
            or will be used to retrieve data for the teradataml DataFrame.
            Default Value: False
            Types: bool
    RETURNS:
        String representing the underlying SQL query for the teradataml DataFrame.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> load_example_data("NaiveBayes", "nb_iris_input_train")
        >>> df = DataFrame.from_table("admissions_train")
        # Example 1: Show query on base (from_table) dataframe, with default option
        >>> df.show_query()
        'select * from "admissions_train"'
        # Example 2: Show query on base (from_query) dataframe, with default option
        >>> df_from_query = DataFrame.from_query("select masters, gpa from admissions_train")
        >>> df_from_query.show_query()
        'select masters, gpa from admissions_train'
        # Example 3: Show query on base (from_table) dataframe, with full_query option
        #            This will return same query as with default option because workflow
        #            only has one dataframe.
        >>> df.show_query(full_query = True)
        'select * from "admissions_train"'
        # Example 4: Show query on base (from_query) dataframe, with full_query option
        #            This will return same query as with default option because workflow
        #            only has one dataframe.
        >>> df_from_query = DataFrame.from_query("select masters, gpa from admissions_train")
        >>> df_from_query.show_query(full_query = True)
        'select masters, gpa from admissions_train'
        # Example 5: Show query used in a workflow demonstrating default and full_query options.
        # Workflow Step-1: Assign operation on base dataframe
        >>> df1 = df.assign(temp_column=admissions_train_df.gpa + admissions_train_df.admitted)
        # Workflow Step-2: Selecting columns from assign's result
        >>> df2 = df1.select(["masters", "gpa", "programming", "admitted"])
        # Workflow Step-3: Filtering on top of select's result
        >>> df3 = df2[df2.admitted > 0]
        # Workflow Step-4: Sampling 90% rows from filter's result
        >>> df4 = df3.sample(frac=0.9)
        # Show query with full_query option on df4. 
        # This will give full query upto base dataframe(df)
        >>> df4.show_query(full_query = True)
        'select masters,gpa,stats,programming,admitted,sampleid as "sampleid" from (
         select * from (select masters,gpa,stats,programming,admitted from (select id AS 
         id, masters AS masters, gpa AS gpa, stats AS stats, programming AS programming, 
         admitted AS admitted, gpa + admitted AS temp_column from "admissions_train") as
         temp_table) as temp_table where admitted > 0) as temp_table SAMPLE 0.9'
        # Show query with default option on df4. This will give same query as give in above case.
        >>> df4.show_query()
        'select masters,gpa,stats,programming,admitted,sampleid as "sampleid" from (select * 
         from (select masters,gpa,stats,programming,admitted from (select id AS id, masters 
         AS masters, gpa AS gpa, stats AS stats, programming AS programming, admitted AS admitted, 
         gpa + admitted AS temp_column from "admissions_train") as temp_table) as temp_table 
         where admitted > 0) as temp_table SAMPLE 0.9'
        # Executing intermediate dataframe df3
        >>> df2
          masters   gpa programming  admitted
        0      no  4.00      Novice         1
        1     yes  3.57    Advanced         1
        2      no  3.44      Novice         0
        3     yes  1.98    Advanced         0
        4     yes  4.00    Advanced         1
        5     yes  3.95    Beginner         0
        6     yes  2.33      Novice         1
        7     yes  3.46    Beginner         0
        8      no  3.00      Novice         0
        9     yes  2.65    Beginner         1
        # Show query with default option on df4. This will give query with respect 
        # to view/table created by the latest executed dataframe in the workflow (df2 in this scenario).
        # This is the query teradataml internally uses to retrieve data for dataframe df4, if executed
        # at this point.
        >>> df4.show_query()
        'select masters,gpa,stats,programming,admitted,sampleid as "sampleid" from (select * from
        "ALICE"."ml__select__1585722211621282" where admitted > 0) as temp_table SAMPLE 0.9'
        # Show query with full_query option on df4. This will still give the same full query upto base dataframe(df)
        >>> df4.show_query(full_query = True)
        'select masters,gpa,stats,programming,admitted,sampleid as "sampleid" from (select *
         from (select masters,gpa,stats,programming,admitted from (select id AS id, masters
         AS masters, gpa AS gpa, stats AS stats, programming AS programming, admitted AS admitted,
         gpa + admitted AS temp_column from "admissions_train") as temp_table) as temp_table
         where admitted > 0) as temp_table SAMPLE 0.9'
    size
    teradataml.dataframe.dataframe.DataFrame.size
    Returns a value representing the number of elements in the DataFrame.
    PARAMETERS:
        None
    RETURNS:
        Value representing the number of elements in the DataFrame.
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> df
                      Feb   Jan   Mar   Apr  datetime
        accounts
        Orange Inc  210.0  None  None   250  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        Blue Inc     90.0    50    95   101  04/01/2017
        Jones LLC   200.0   150   140   180  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        >>> df.size
        36
    RAISES:
        None
    skew
    teradataml.dataframe.dataframe.DataFrame.skew = skew(self, distinct=False)
    DESCRIPTION:
        Returns column-wise skewness of the distribution of the dataframe.
        Skewness is the third moment of a distribution. It is a measure of the asymmetry of the
        distribution about its mean compared with the normal (or Gaussian) distribution.
            * The normal distribution has a skewness of 0.
            * Positive skewness indicates a distribution having an asymmetric tail
              extending toward more positive values.
            * Negative skewness indicates an asymmetric tail extending toward more negative values.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Nulls are not included in the result computation.
            3. Following conditions will produce null result:
                a. Fewer than three non-null data points in the data used for the computation.
                b. Standard deviation for a column is equal to 0.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the skewness of the distribution.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with skew()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If the skew() operation fails to
            generate the column-wise skew value of the dataframe.
            Possible error message:
            Failed to perform 'skew'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the skew() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'skew' operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["admissions_train"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("admissions_train")
        >>> print(df1.sort('id'))
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        2      yes  3.76  Beginner    Beginner         0
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        8       no  3.60  Beginner    Advanced         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        >>>
        # Prints skew value of each column(with supported data types).
        >>> df1.skew()
           skew_id  skew_gpa  skew_admitted
        0      0.0 -2.058969      -0.653746
        >>>
        #
        # Using skew() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        #
        # Time Series Aggregate Example 1: Executing skew() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the skew values.
        #
        # To use skew() as Time Series Aggregate we must run groupby_time() first, followed by skew().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.skew().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid skew_salinity  skew_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0          None          0.000324
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1          None          0.000000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2          None          0.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44          None          0.246084
        >>>
        #
        # Time Series Aggregate Example 2: Executing skew() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the skew value.
        #
        # To use skew() as Time Series Aggregate we must run groupby_time() first, followed by skew().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.skew(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid skew_salinity  skew_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0          None         -1.731321
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1          None          0.000000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2          None          0.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44          None         -1.987828
        >>>
    snapshots
    teradataml.dataframe.dataframe.DataFrame.snapshots
    DESCRIPTION:
        Gets snapshot information for a DataLake table.
    PARAMETERS:
        None
    RETURNS:
        teradataml DataFrame.
    RAISES:
        TeradataMLException.
    EXAMPLES :
        # Example 1: Get the snapshot information for datalake table.
        >>> from teradataml.dataframe.dataframe import in_schema
        >>> in_schema_tbl = in_schema(schema_name="datalake_db",
        ...                           table_name="datalake_table",
        ...                           datalake_name="datalake")
        >>> datalake_df = DataFrame(in_schema_tbl)
        >>> datalake_df.snapshots
                     snapshotId       snapshotTimestamp     timestampMSecs                                       manifestList      
        0   6373759902296319074     2023-06-15 00:07:47     1686787667420   s3://vim-iceberg-v1/glue/metadata/snap-
    6373759...       {"added-data-files":"1","added-records":"5","a...}
        1   4768076782814510171     2023-06-15 00:09:01     1686787741964   s3://vim-iceberg-v1/glue/metadata/snap-
    4768076...       {"added-data-files":"1","added-records":"2","a...}
        2   7771482207931850214     2024-05-29 04:59:09     1716958749946   s3://vim-iceberg-v1/glue/metadata/snap-
    7771482...       {"deleted-data-files":"2","deleted-records":"7...}
        3   1545363077953282623     2024-05-29 05:13:39     1716959619455   s3://vim-iceberg-v1/glue/metadata/snap-
    1545363...       {"changed-partition-count":"0","total-records"...}
        4   2166707884289108360     2024-05-29 05:17:49     1716959869075   s3://vim-iceberg-v1/glue/metadata/snap-
    2166707...       {"changed-partition-count":"0","total-records"...}
        5   8934190131471882700     2024-05-29 05:21:32     1716960092422   s3://vim-iceberg-v1/glue/metadata/snap-
    8934190...       {"changed-partition-count":"0","total-records"...}
        6   3086605171258231948     2024-05-29 05:34:43     1716960883786   s3://vim-iceberg-v1/glue/metadata/snap-
    3086605...       {"changed-partition-count":"0","total-records"...}
        7   7592503716012384122     2024-05-29 06:04:48     1716962688047   s3://vim-iceberg-v1/glue/metadata/snap-
    7592503...       {"changed-partition-count":"0","total-records"...}
        8   2831061717890032890     2024-06-04 17:21:01     1717521661689   s3://vim-iceberg-v1/glue/metadata/snap-
    2831061...       {"added-data-files":"2","added-records":"7","a...}
        9   8810491341502972715     2024-10-22 23:47:22     1729640842067   s3://vim-iceberg-v1/glue/metadata/snap-
    8810491...       {"added-data-files":"1","added-records":"1","a...}
        10  3953136136558551163     2024-12-03 04:40:48     1733200848733   s3://vim-iceberg-v1/glue/metadata/snap-
    3953136...       {"added-data-files":"1","added-records":"4","a...}
        11  6034775168901969481     2024-12-03 04:40:49     1733200849966   s3://vim-iceberg-v1/glue/metadata/snap-
    6034775...       {"deleted-data-files":"1","deleted-records":"5...}
    sort
    teradataml.dataframe.dataframe.DataFrame.sort = sort(self, columns, ascending=True)
    DESCRIPTION:
        Get sorted data by one or more columns in either ascending or descending order
        for a Dataframe.
        Unsupported column types for sorting: ['BLOB', 'CLOB', 'ARRAY', 'VARRAY']
    PARAMETERS:
        columns:
            Required Argument.
            Specifies the name(s) of the columns or ColumnExpression(s) to sort on.
            Types: str OR ColumnExpression OR list of Strings (str) OR list of ColumnExpressions.
        ascending:
            Optional Argument.
            Specifies whether to order in ascending or descending order for each column.
            When set to True, sort in ascending order. Otherwise, sort in descending order.
            Notes:
                 * If a list is specified, length of the 'ascending' must equal
                   length of the "columns".
                 * If a list is specified, element is ignored if the corresponding
                   element in "columns" is a ColumnExpression.
                 * The argument is ignored if "columns" is a ColumnExpression.
            Default value: True
            Types: bool or list of bool
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe", "sales")
        >>> df = DataFrame("sales")
        >>> df
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        # Example 1: Sort the data based on the column Feb in
        #            ascending order by passing the name of the column.
        >>> df.sort("Feb")
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        >>>
        # Example 2: Sort the data based on the column Feb in
        #            descending order by passing the ColumnExpression.
        >>> df.sort([df.Feb.desc()])
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        >>>
        # Example 3: Sort the data based on the columns Jan and accounts in
        #            ascending order and descending order with NULLS at first
        #            respectively.
        #            Note:
        #                * Since the second element in "columns" is a ColumnExpression,
        #                  the data is sorted in descending order even though the
        #                  second element in "ascending" is True.
        >>> df.sort(["Jan", df.accounts.desc().nulls_first()], [True, True])
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        >>>
        # Example 4: Sort the data based on columns Jan and Apr in ascending order
        #            with NULLS at first and descending order with NULLS at first
        #            respectively.
        >>> df.sort([df.Jan.nulls_first(), df.Apr.desc().nulls_first()])
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        >>>
        # Example 5: Sort the data based on columns Jan and Apr in ascending order
        #            with NULLS at first and descending order with NULLS at last
        #            respectively.
        >>> df.sort([df.Jan.nulls_first(), df.Apr.desc().nulls_last()])
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        >>>
    sort_index
    teradataml.dataframe.dataframe.DataFrame.sort_index = sort_index(self, axis=0, ascending=True, kind='quicksort')
    DESCRIPTION:
        Gets sorted object by labels (along an axis) in either ascending or
        descending order for a teradataml DataFrame.
    PARAMETERS:
        axis:
            Optional Argument.
            Specifies the value to direct sorting on index or columns. 
            Values can be either 0 ('rows') OR 1 ('columns'), value as 0 will sort on index (if no index is present then parent Dat
            and value as 1 will sort on columns names (if no index is present then parent DataFrame will be returned with sorted co
            Default value: 0
            Types: int
        ascending:
            Optional Argument.
            Specifies a flag to sort columns in either ascending (True) or descending (False).
            Default value: True
            Types: bool
        kind:
            Optional Argument.
            Specifies a desired algorithm to be used.
            Permitted values: 'quicksort', 'mergesort' or 'heapsort'
            Default value: 'quicksort'
            Types: str
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","scale_housing_test")
        >>> df = DataFrame.from_table('scale_housing_test')
        >>> df
                  id    price  lotsize  bedrooms  bathrms  stories
        types                                                     
        classic   14  36000.0   2880.0       3.0      1.0      1.0
        bungalow  11  90000.0   7200.0       3.0      2.0      1.0
        classic   15  37000.0   3600.0       2.0      1.0      1.0
        classic   13  27000.0   1700.0       3.0      1.0      2.0
        classic   12  30500.0   3000.0       2.0      1.0      1.0
        >>> df.sort_index()
                  id    price  lotsize  bedrooms  bathrms  stories
        types                                                     
        bungalow  11  90000.0   7200.0       3.0      2.0      1.0
        classic   13  27000.0   1700.0       3.0      1.0      2.0
        classic   12  30500.0   3000.0       2.0      1.0      1.0
        classic   14  36000.0   2880.0       3.0      1.0      1.0
        classic   15  37000.0   3600.0       2.0      1.0      1.0
        >>> df.sort_index(0)
                  id    price  lotsize  bedrooms  bathrms  stories
        types                                                     
        bungalow  11  90000.0   7200.0       3.0      2.0      1.0
        classic   13  27000.0   1700.0       3.0      1.0      2.0
        classic   12  30500.0   3000.0       2.0      1.0      1.0
        classic   14  36000.0   2880.0       3.0      1.0      1.0
        classic   15  37000.0   3600.0       2.0      1.0      1.0
        >>> df.sort_index(1, False) # here 'False' means DESCENDING for respective axis
                  stories    price  lotsize  id  bedrooms  bathrms
        types                                                     
        classic       1.0  36000.0   2880.0  14       3.0      1.0
        bungalow      1.0  90000.0   7200.0  11       3.0      2.0
        classic       1.0  37000.0   3600.0  15       2.0      1.0
        classic       2.0  27000.0   1700.0  13       3.0      1.0
        classic       1.0  30500.0   3000.0  12       2.0      1.0
        >>> df.sort_index(1, True, 'mergesort')
                  bathrms  bedrooms  id  lotsize    price  stories
        types                                                     
        classic       1.0       3.0  14   2880.0  36000.0      1.0
        bungalow      2.0       3.0  11   7200.0  90000.0      1.0
        classic       1.0       2.0  15   3600.0  37000.0      1.0
        classic       1.0       3.0  13   1700.0  27000.0      2.0
        classic       1.0       2.0  12   3000.0  30500.0      1.0
    squeeze
    teradataml.dataframe.dataframe.DataFrame.squeeze = squeeze(self, axis=None)
    DESCRIPTION:
        Squeeze one-dimensional axis objects into scalars.
        teradataml DataFrames with a single element are squeezed to a scalar.
        teradataml DataFrames with a single column are squeezed to a Series.
        Otherwise the object is unchanged.
        Note: Currently only '1' and 'None' are supported for axis.
              For now with axis = 0, the teradataml DataFrame is returned.
    PARAMETERS:
        axis:
            Optional Argument.
            A specific axis to squeeze. By default, all axes with
            length equals one are squeezed.
            Permitted Values: 0 or 'index', 1 or 'columns', None
            Default: None
    RETURNS:
        teradataml DataFrame, teradataml Series, or scalar,
        the projection after squeezing 'axis' or all the axes.
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming admitted
        id
        22     yes  3.46    Novice    Beginner        0
        36      no  3.00  Advanced      Novice        0
        15     yes  4.00  Advanced    Advanced        1
        38     yes  2.65  Advanced    Beginner        1
        5       no  3.44    Novice      Novice        0
        17      no  3.83  Advanced    Advanced        1
        34     yes  3.85  Advanced    Beginner        0
        13      no  4.00  Advanced      Novice        1
        26     yes  3.57  Advanced    Advanced        1
        19     yes  1.98  Advanced    Advanced        0
        >>> gpa = df.select(["gpa"])
        >>> gpa.squeeze()
        0    4.00
        1    2.33
        2    3.46
        3    3.83
        4    4.00
        5    2.65
        6    3.57
        7    3.44
        8    3.85
        9    3.95
        Name: gpa, dtype: float64
        >>> gpa.squeeze(axis = 1)
        0    3.46
        1    3.00
        2    4.00
        3    2.65
        4    3.44
        5    3.83
        6    3.85
        7    4.00
        8    3.57
        9    1.98
        Name: gpa, dtype: float64
        >>> gpa.squeeze(axis = 0)
            gpa
        0  3.46
        1  3.00
        2  4.00
        3  2.65
        4  3.44
        5  3.83
        6  3.85
        7  4.00
        8  3.57
        9  1.98
        >>> df = DataFrame.from_query('select gpa, stats from admissions_train where gpa=2.33')
        >>> s = df.squeeze()
        >>> s
            gpa   stats
        0  2.33  Novice
        >>> single_gpa = DataFrame.from_query('select gpa from admissions_train where gpa=2.33')
        >>> single_gpa
            gpa
        0  2.33
        >>> single_gpa.squeeze()
        2.33
        >>> single_gpa.squeeze(axis = 1)
        0    2.33
        Name: gpa, dtype: float64
        >>> single_gpa.squeeze(axis = 0)
            gpa
        0  2.33
    sum
    teradataml.dataframe.dataframe.DataFrame.sum = sum(self, distinct=False)
    DESCRIPTION:
        Returns column-wise sum value of the dataframe.
        Notes:
            1. teradataml doesn't support sum operation on columns of str, datetime types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the sum.
            Default Value: False 
            Types: bool
    RETURNS:
        teradataml DataFrame object with sum()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If sum() operation fails to
            generate the column-wise summation value of the dataframe.
            Possible error message:
            Failed to perform 'sum'. (Followed by error message).
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the sum() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'sum' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints sum of the values of each column(with supported data types).
        >>> df1.sum()
          sum_employee_no sum_marks
        0             313      None
        >>>
        #
        # Using sum() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing sum() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the sum value.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        # To use sum() as Time Series Aggregate we must run groupby_time() first, followed by sum().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.sum().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  sum_salinity  sum_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           275              219
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           330              447
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           165              243
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           715              625
        >>>
        #
        # Time Series Aggregate Example 2: Executing sum() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the sum value.
        #
        # To use sum() as Time Series Aggregate we must run groupby_time() first, followed by sum().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.sum(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  sum_salinity  sum_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0            55              209
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1            55              447
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2            55              243
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44            55              261
        >>>
    tail
    teradataml.dataframe.dataframe.DataFrame.tail = tail(self, n=10)
    DESCRIPTION:
        Print the last n rows of the sorted teradataml DataFrame.
        Note: The Dataframe is sorted on the index column or the first column if
        there is no index column. The column type must support sorting.
        Unsupported types: ['BLOB', 'CLOB', 'ARRAY', 'VARRAY']
    PARAMETERS:
        n:
            Optional Argument.
            Specifies the number of rows to select.
            Default Value: 10.
            Types: int
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame.from_table('admissions_train')
        >>> df
           masters   gpa     stats programming admitted
        id
        15     yes  4.00  Advanced    Advanced        1
        7      yes  2.33    Novice      Novice        1
        22     yes  3.46    Novice    Beginner        0
        17      no  3.83  Advanced    Advanced        1
        13      no  4.00  Advanced      Novice        1
        38     yes  2.65  Advanced    Beginner        1
        26     yes  3.57  Advanced    Advanced        1
        5       no  3.44    Novice      Novice        0
        34     yes  3.85  Advanced    Beginner        0
        40     yes  3.95    Novice    Beginner        0
        >>> df.tail()
           masters   gpa     stats programming admitted
        id
        38     yes  2.65  Advanced    Beginner        1
        36      no  3.00  Advanced      Novice        0
        35      no  3.68    Novice    Beginner        1
        34     yes  3.85  Advanced    Beginner        0
        32     yes  3.46  Advanced    Beginner        0
        31     yes  3.50  Advanced    Beginner        1
        33      no  3.55    Novice      Novice        1
        37      no  3.52    Novice      Novice        1
        39     yes  3.75  Advanced    Beginner        0
        40     yes  3.95    Novice    Beginner        0
        >>> df.tail(3)
           masters   gpa     stats programming admitted
        id
        38     yes  2.65  Advanced    Beginner        1
        39     yes  3.75  Advanced    Beginner        0
        40     yes  3.95    Novice    Beginner        0
        >>> df.tail(15)
           masters   gpa     stats programming admitted
        id
        38     yes  2.65  Advanced    Beginner        1
        36      no  3.00  Advanced      Novice        0
        35      no  3.68    Novice    Beginner        1
        34     yes  3.85  Advanced    Beginner        0
        32     yes  3.46  Advanced    Beginner        0
        31     yes  3.50  Advanced    Beginner        1
        30     yes  3.79  Advanced      Novice        0
        29     yes  4.00    Novice    Beginner        0
        28      no  3.93  Advanced    Advanced        1
        27     yes  3.96  Advanced    Advanced        0
        26     yes  3.57  Advanced    Advanced        1
        33      no  3.55    Novice      Novice        1
        37      no  3.52    Novice      Novice        1
        39     yes  3.75  Advanced    Beginner        0
        40     yes  3.95    Novice    Beginner        0
    tdtypes
    teradataml.dataframe.dataframe.DataFrame.tdtypes
    DESCRIPTION:
        Get the teradataml DataFrame metadata containing column names and
        corresponding teradatasqlalchemy types.
    RETURNS:
        MetaData containing the column names and Teradata types
    RAISES:
        None.
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> print(df.tdtypes)
        accounts    VARCHAR(length=20, charset='LATIN')
        Feb                                     FLOAT()
        Jan                                    BIGINT()
        Mar                                    BIGINT()
        Apr                                    BIGINT()
        datetime                                 DATE()
        >>>
    to_csv
    teradataml.dataframe.dataframe.DataFrame.to_csv = to_csv(self, csv_ﬁle, num_rows=99999, all_rows=False, fastexport=False, sep=',', quotechar='"',
    catch_errors_warnings=False, **kwargs)
    DESCRIPTION:
        Export data to CSV from teradataml DataFrame with or
        without FastExport protocol.
    PARAMETERS:
        csv_file:
            Required Argument.
            Specifies the name of CSV file to export the data into.
            Types: str
        num_rows:
            Optional Argument.
            Specifies the number of rows to export.
            Note:
                This argument is ignored if "all_rows" is set to True.
            Default Value: 99999
            Types: int
        all_rows:
            Optional Argument.
            Specifies whether all rows should be exported to CSV or not.
            Default Value: False
            Types: bool
        fastexport:
            Optional Argument.
            Specifies whether FastExport protocol should be used or not while
            exporting data. When set to True, data is exported using FastExport
            protocol, otherwise FastExport protocol is not used, which is default.
            When set to None, the approach is decided based on the number of rows
            to be exported.
            Notes:
                1. Teradata recommends to use FastExport when number of rows
                   in teradataml DataFrame are atleast 100,000. To extract
                   lesser rows ignore this option and go with regular
                   approach. FastExport opens multiple data transfer connections
                   to the database.
                2. FastExport does not support all Teradata Database data types.
                   For example, tables with BLOB and CLOB type columns cannot
                   be extracted.
                3. FastExport cannot be used to extract data from a
                   volatile or temporary table.
                4. For best efficiency, do not use DataFrame.groupby() and
                   DataFrame.sort() with FastExport.
            For additional information about FastExport protocol through
            teradatasql driver, please refer to FASTEXPORT section of
            https://pypi.org/project/teradatasql/#FastExport driver documentation.
            Default Value: False
            Types: bool
        sep:
            Optional Argument.
            Specifies a single character string used to separate fields in a CSV file.
            Default Value: ","
            Notes:
                1. "sep" cannot be line feed ('\n') or carriage return ('\r').
                2. "sep" should not be same as "quotechar".
                3. Length of "sep" argument should be 1.
            Types: String
        quotechar:
            Optional Argument.
            Specifies a single character string used to quote fields in a CSV file.
            Default Value: """
            Notes:
                1. "quotechar" cannot be line feed ('\n') or carriage return ('\r').
                2. "quotechar" should not be same as "sep".
                3. Length of "quotechar" argument should be 1.
            Types: String
        catch_errors_warnings:
            Optional Argument.
            Specifies whether to catch errors/warnings (if any) raised by
            FastExport protocol while exporting data.
            Note:
                This argument is ignored if "fastexport" is set to False.
            Default Value: False
            Types: bool
        kwargs:
            Optional Argument.
            Specifies keyword arguments. Argument "open_sessions" can be
            passed as keyword arguments.
                * "open_sessions" specifies the number of Teradata data transfer
                  sessions to be opened for fastexport. This argument is only
                  applicable in fastexport mode.
            Note:
                If "open_sessions" argument is not provided, the default value
                   is the smaller of 8 or the number of AMPs avaialble.
                   For additional information about number of Teradata data-transfer
                   sessions opened during fastexport, please refer to:
                   https://pypi.org/project/teradatasql/#FastExport
    RETURNS:
        When FastExport protocol is used and "catch_errors_warnings" is set to True,
        then the function returns a tuple containing:
            a. Errors, if any, thrown by fastexport in a list of strings.
            b. Warnings, if any, thrown by fastexport in a list of strings.
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Create a teradataml DataFrame.
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming admitted
        id
        22     yes  3.46    Novice    Beginner        0
        37      no  3.52    Novice      Novice        1
        35      no  3.68    Novice    Beginner        1
        12      no  3.65    Novice      Novice        1
        4      yes  3.50  Beginner      Novice        1
        38     yes  2.65  Advanced    Beginner        1
        27     yes  3.96  Advanced    Advanced        0
        39     yes  3.75  Advanced    Beginner        0
        7      yes  2.33    Novice      Novice        1
        40     yes  3.95    Novice    Beginner        0
        ...
        # Example 1: Export data from teradataml DataFrame into CSV,
        #            with only required argument.
        >>> df.to_csv("export_to_csv_1.csv")
           Data is successfully exported into export_to_csv_1.csv
        # Example 2: Export all rows from teradataml DataFrame into CSV
        #            using FastExport protocol.
        >>> df.to_csv("export_to_csv_2.csv", all_rows=True, fastexport=True)
           Data is successfully exported into export_to_csv_2.csv
        # Example 3: Export 20 rows from teradataml DataFrame into CSV.
        >>> df.to_csv("export_to_csv_3.csv", num_rows=20)
           Data is successfully exported into export_to_csv_3.csv
        # Example 4: Export data from teradataml DataFrame into CSV using
        #            FastExport protocol by opening one Teradata data
        #            transfer session. Save errors and warnings
        #            thrown by fastexport.
        >>> err, warn = df.to_csv("export_to_csv_4.csv", fastexport=True,
                                  catch_errors_warnings=True, open_sessions=1 )
           Data is successfully exported into export_to_csv_4.csv
        >>>err
           []
        >>>warn
           []
        # Example 5: Export data from teradataml DataFrame into CSV
        #            file with '|' as field separator and single quote(')
        #            as field quote character.
        >>> df.to_csv("export_to_csv_5.csv", sep="|", quotechar="'" )
           Data is successfully exported into export_to_csv_5.csv
    to_pandas
    teradataml.dataframe.dataframe.DataFrame.to_pandas = to_pandas(self, index_column=None, num_rows=99999, all_rows=False, fastexport=False,
    catch_errors_warnings=False, **kwargs)
    DESCRIPTION:
        Returns a Pandas DataFrame for the corresponding teradataml
        DataFrame Object.
    PARAMETERS:
        index_column:
            Optional Argument.
            Specifies column(s) to be used as Pandas index.
            When the argument is provided, the specified column is used as
            the Pandas index. Otherwise, the teradataml DataFrame's index
            (if exists) is used as the Pandas index or the primary index of
            the table on Vantage is used as the Pandas index. The default
            integer index is used if none of the above indexes exists.
            Default Value: Integer index
            Types: str OR list of Strings (str)
        num_rows:
            Optional Argument.
            Specifies the number of rows to retrieve randomly from the DataFrame
            while creating Pandas Dataframe.
            Default Value: 99999
            Types: int
            Note:
                This argument is ignored if "all_rows" is set to True.
        all_rows:
            Optional Argument.
            Specifies whether all rows from teradataml DataFrame should be
            retrieved while creating Pandas DataFrame.
            Default Value: False
            Types: bool
        fastexport:
            Optional Argument.
            Specifies whether fastexport protocol should be used while
            converting teradataml DataFrame to a Pandas DataFrame. If the
            argument is set to True, fastexport wire protocol is used
            internally for data transfer. By default, fastexport protocol will not be
            used while converting teradataml DataFrame to a Pandas DataFrame.
            When set to None, the approach is decided based on the number of rows
            requested by the user for extraction.
            If requested number of rows are greater than or equal to 100000,
            then fastexport is used, otherwise regular mode is used for data
            extraction.
            Note:
                1. Teradata recommends to use FastExport when number of rows
                   in teradataml DataFrame are atleast 100,000. To extract
                   lesser rows ignore this option and go with regular
                   approach. FastExport opens multiple data transfer connections
                   to the database.
                2. FastExport does not support all Teradata Database data types.
                   For example, tables with BLOB and CLOB type columns cannot
                   be extracted.
                3. FastExport cannot be used to extract data from a
                   volatile or temporary table.
                4. For best efficiency, do not use DataFrame.groupby() and
                   DataFrame.sort() with FastExport.
            For additional information about FastExport protocol through
            teradatasql driver, please refer to FASTEXPORT section of
            https://pypi.org/project/teradatasql/#FastExport driver documentation.
            Default Value: False
            Types: bool
        catch_errors_warnings:
            Optional Argument.
            Specifies whether to catch errors/warnings(if any) raised by
            fastexport protocol while converting teradataml DataFrame to
            Pandas DataFrame. When this is set to True and fastexport is used,
            to_pandas() returns a tuple containing:
                a. Pandas DataFrame.
                b. Errors(if any) in a list thrown by fastexport.
                c. Warnings(if any) in a list thrown by fastexport.
            When set to False and fastexport is used, prints the fastexport
            errors/warnings to the standard output, if there are any.
            Note:
                This argument is ignored if "fastexport" is set to False.
            Default Value: False
            Types: bool
        kwargs:
            Optional Argument.
            Specifies keyword arguments. Arguments "coerce_float"
            "parse_dates" and "open_sessions" can be passed as keyword arguments.
                * "coerce_float" specifies a boolean to for attempting to
                  convert non-string, non-numeric objects to floating point.
                * "parse_dates" specifies columns to parse as dates.
                * "open_sessions" specifies the number of Teradata data transfer
                  sessions to be opened for fastexport. This argument is only applicable
                  in fastexport mode.
                * Function returns the pandas dataframe with Decimal columns types as float instead of object.
                  If user want datatype to be object, set argument "coerce_float" to False.
            Notes:
                1. For additional information about "coerce_float" and
                   "parse_date" arguments please refer to:
                   https://pandas.pydata.org/docs/reference/api/pandas.read_sql.html
                2. If "open_sessions" argument is not provided, the default value
                   is the smaller of 8 or the number of AMPs avaialble.
                   For additional information about number of Teradata data-transfer
                   sessions opened during fastexport, please refer to:
                   https://pypi.org/project/teradatasql/#FastExport
    RETURNS:
        When "catch_errors_warnings" is set to True and if protocol used for
        data transfer is fastexport, then the function returns a tuple
        containing:
            a. Pandas DataFrame.
            b. Errors, if any, thrown by fastexport in a list of strings.
            c. Warnings, if any, thrown by fastexport in a list of strings.
        Only Pandas DataFrame otherwise.
        Note:
            Column types of the resulting Pandas DataFrame depends on
            pandas.from_records().
    RAISES:
        TeradataMlException
    EXAMPLES:
        Teradata supports the following formats:
        A] No parameter(s): df.to_pandas()
        B] Single index_column parameter: df.to_pandas(index_column = "col1")
        C] Multiple index_column (list) parameters:
        df.to_pandas(index_column = ['col1', 'col2'])
        D] Only num_rows parameter specified:  df.to_pandas(num_rows = 100)
        E] Both index_column & num_rows specified:
        df.to_pandas(index_column = 'col1', num_rows = 100)
        F] Only all_rows parameter specified:  df.to_pandas(all_rows = True)
        Column names ("col1", "col2"..) are strings representing Teradata
        Vantage table Columns. It supports all standard Teradata data types
        for columns: INTEGER, VARCHAR(5), FLOAT etc.
        df is a Teradata DataFrame object: df = DataFrame.from_table('admissions_train')
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming admitted
        id
        22     yes  3.46    Novice    Beginner        0
        37      no  3.52    Novice      Novice        1
        35      no  3.68    Novice    Beginner        1
        12      no  3.65    Novice      Novice        1
        4      yes  3.50  Beginner      Novice        1
        38     yes  2.65  Advanced    Beginner        1
        27     yes  3.96  Advanced    Advanced        0
        39     yes  3.75  Advanced    Beginner        0
        7      yes  2.33    Novice      Novice        1
        40     yes  3.95    Novice    Beginner        0
        >>> pandas_df = df.to_pandas()
        >>> pandas_df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        14     yes  3.45  Advanced    Advanced         0
        31     yes  3.50  Advanced    Beginner         1
        29     yes  4.00    Novice    Beginner         0
        23     yes  3.59  Advanced      Novice         1
        21      no  3.87    Novice    Beginner         1
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        32     yes  3.46  Advanced    Beginner         0
        11      no  3.13  Advanced    Advanced         1
        ...
        >>> pandas_df = df.to_pandas(index_column = 'id')
        >>> pandas_df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        14     yes  3.45  Advanced    Advanced         0
        31     yes  3.50  Advanced    Beginner         1
        29     yes  4.00    Novice    Beginner         0
        23     yes  3.59  Advanced      Novice         1
        21      no  3.87    Novice    Beginner         1
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        32     yes  3.46  Advanced    Beginner         0
        11      no  3.13  Advanced    Advanced         1
        28      no  3.93  Advanced    Advanced         1
        ...
        >>> pandas_df = df.to_pandas(index_column = 'gpa')
        >>> pandas_df
              id masters     stats programming  admitted
        gpa
        4.00  15     yes  Advanced    Advanced         1
        3.45  14     yes  Advanced    Advanced         0
        3.50  31     yes  Advanced    Beginner         1
        4.00  29     yes    Novice    Beginner         0
        3.59  23     yes  Advanced      Novice         1
        3.87  21      no    Novice    Beginner         1
        3.83  17      no  Advanced    Advanced         1
        3.85  34     yes  Advanced    Beginner         0
        4.00  13      no  Advanced      Novice         1
        3.46  32     yes  Advanced    Beginner         0
        3.13  11      no  Advanced    Advanced         1
        3.93  28      no  Advanced    Advanced         1
        ...
        >>> pandas_df = df.to_pandas(index_column = ['masters', 'gpa'])
        >>> pandas_df
                      id     stats programming  admitted
        masters gpa
        yes     4.00  15  Advanced    Advanced         1
                3.45  14  Advanced    Advanced         0
                3.50  31  Advanced    Beginner         1
                4.00  29    Novice    Beginner         0
                3.59  23  Advanced      Novice         1
        no      3.87  21    Novice    Beginner         1
                3.83  17  Advanced    Advanced         1
        yes     3.85  34  Advanced    Beginner         0
        no      4.00  13  Advanced      Novice         1
        yes     3.46  32  Advanced    Beginner         0
        no      3.13  11  Advanced    Advanced         1
                3.93  28  Advanced    Advanced         1
        ...
        >>> pandas_df = df.to_pandas(index_column = 'gpa', num_rows = 3)
        >>> pandas_df
              id masters   stats programming  admitted
        gpa
        3.46  22     yes  Novice    Beginner         0
        2.33   7     yes  Novice      Novice         1
        3.95  40     yes  Novice    Beginner         0
        >>> pandas_df = df.to_pandas(all_rows = True)
        >>> pandas_df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        14     yes  3.45  Advanced    Advanced         0
        31     yes  3.50  Advanced    Beginner         1
        29     yes  4.00    Novice    Beginner         0
        23     yes  3.59  Advanced      Novice         1
        21      no  3.87    Novice    Beginner         1
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        32     yes  3.46  Advanced    Beginner         0
        11      no  3.13  Advanced    Advanced         1
        ...
        # Convert teradataml DataFrame to pandas DataFrame using fastexport.
        # Prints errors/warnings if any on to the screen as catch_errors_warnings
        # argument is not set.
        >>> pandas_df = df.to_pandas(fastexport = True)
        Errors: []
        Warnings: []
        >>> pandas_df
           masters   gpa     stats programming  admitted
        id
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        20     yes  3.90  Advanced    Advanced         1
        18     yes  3.81  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        25      no  3.96  Advanced    Advanced         1
        2      yes  3.76  Beginner    Beginner         0
        ...
        # Convert teradataml DataFrame to pandas DataFrame using fastexport
        # also catch warnings/errors if any raised by fastexport. Returns
        # a tuple.
        >>> pandas_df, err, warn = df.to_pandas(fastexport = True,
                                                catch_errors_warnings = True)
        # Print pandas df.
        >>> pandas_df
           masters   gpa     stats programming  admitted
        id
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        20     yes  3.90  Advanced    Advanced         1
        18     yes  3.81  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        25      no  3.96  Advanced    Advanced         1
        2      yes  3.76  Beginner    Beginner         0
        17      no  3.83  Advanced    Advanced         1
        ...
        # Print errors list.
        >>> err
        []
        # Print warnings list.
        >>> warn
        []
        # Convert teradataml DataFrame to pandas DataFrame without
        # fastexport.
        >>> pandas_df = df.to_pandas(fastexport = False)
        >>> pandas_df
           masters   gpa     stats programming  admitted
        id
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        20     yes  3.90  Advanced    Advanced         1
        18     yes  3.81  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        25      no  3.96  Advanced    Advanced         1
        2      yes  3.76  Beginner    Beginner         0
        ...
        # Convert teradataml DataFrame to pandas DataFrame using fastexport
        # by opening 2 Teradata data transfer sessiosns.
        >>> pandas_df = df.to_pandas(fastexport = True, open_sessions = 2)
        Errors: []
        Warnings: []
        >>> pandas_df
           masters   gpa     stats programming  admitted
        id
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        20     yes  3.90  Advanced    Advanced         1
        18     yes  3.81  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        25      no  3.96  Advanced    Advanced         1
        2      yes  3.76  Beginner    Beginner         0
        ...
    to_sql
    teradataml.dataframe.dataframe.DataFrame.to_sql = to_sql(self, table_name, if_exists='fail', primary_index=None, temporary=False, schema_name=None, types=None,
    primary_time_index_name=None, timecode_column=None, timebucket_duration=None, timezero_date=None, columns_list=None, sequence_column=None, seq_max=None,
    set_table=False)
    DESCRIPTION:
        Writes records stored in a teradataml DataFrame to Teradata Vantage.
    PARAMETERS:
        table_name:
            Required Argument.
            Specifies the name of the table to be created in Teradata Vantage.
            Types: str
        schema_name:
            Optional Argument.
            Specifies the name of the SQL schema in Teradata Vantage to write to.
            Default Value: None (Use default Teradata Vantage schema).
            Types: str
            Note: schema_name will be ignored when temporary=True.
        if_exists:
            Optional Argument.
            Specifies the action to take when table already exists in Teradata Vantage.
            Default Value: 'fail'
            Permitted Values: 'fail', 'replace', 'append'
                - fail: If table exists, do nothing.
                - replace: If table exists, drop it, recreate it, and insert data.
                - append: If table exists, insert data. Create table, if does not exist.
            Types: str
            Note: Replacing a table with the contents of a teradataml DataFrame based on
                  the same underlying table is not supported.
        primary_index:
            Optional Argument.
            Creates Teradata table(s) with primary index column(s) when specified.
            When None, No primary index Teradata tables are created.
            Default Value: None
            Types: str or List of Strings (str)
                Example:
                    primary_index = 'my_primary_index'
                    primary_index = ['my_primary_index1', 'my_primary_index2', 'my_primary_index3']
        temporary:
            Optional Argument.
            Creates Teradata SQL tables as permanent or volatile.
            When True,
                1. volatile tables are created, and
                2. schema_name is ignored.
            When False, permanent tables are created.
            Default Value: False
            Types: boolean
        types:
            Optional Argument.
            Specifies required data types for requested columns to be saved in Vantage.
            Types: Python dictionary ({column_name1: type_value1, ... column_nameN: type_valueN})
            Default: None
            Note:
                1. This argument accepts a dictionary of columns names and their required teradatasqlalchemy types
                   as key-value pairs, allowing to specify a subset of the columns of a specific type.
                   When only a subset of all columns are provided, the column types for the rest are retained.
                   When types argument is not provided, the column types are retained.
                2. This argument does not have any effect when the table specified using table_name and schema_name
                   exists and if_exists = 'append'.
        primary_time_index_name:
            Optional Argument.
            Specifies a name for the Primary Time Index (PTI) when the table
            to be created must be a PTI table.
            Types: String
            Note: This argument is not required or used when the table to be created
                  is not a PTI table. It will be ignored if specified without the timecode_column.
        timecode_column:
            Optional Argument.
            Required when the DataFrame must be saved as a PTI table.
            Specifies the column in the DataFrame that reflects the form
            of the timestamp data in the time series.
            This column will be the TD_TIMECODE column in the table created.
            It should be of SQL type TIMESTAMP(n), TIMESTAMP(n) WITH TIMEZONE, or DATE,
            corresponding to Python types datetime.datetime or datetime.date.
            Types: String
            Note: When you specify this parameter, an attempt to create a PTI table
                  will be made. This argument is not required when the table to be created
                  is not a PTI table. If this argument is specified, primary_index will be ignored.
        timezero_date:
            Optional Argument.
            Used when the DataFrame must be saved as a PTI table.
            Specifies the earliest time series data that the PTI table will accept;
            a date that precedes the earliest date in the time series data.
            Value specified must be of the following format: DATE 'YYYY-MM-DD'
            Default Value: DATE '1970-01-01'.
            Types: String
            Note: This argument is not required or used when the table to be created
                  is not a PTI table. It will be ignored if specified without the timecode_column.
        timebucket_duration:
            Optional Argument.
            Required if columns_list is not specified or is None.
            Used when the DataFrame must be saved as a PTI table.
            Specifies a duration that serves to break up the time continuum in
            the time series data into discrete groups or buckets.
            Specified using the formal form time_unit(n), where n is a positive
            integer, and time_unit can be any of the following:
            CAL_YEARS, CAL_MONTHS, CAL_DAYS, WEEKS, DAYS, HOURS, MINUTES,
            SECONDS, MILLISECONDS, or MICROSECONDS.
            Types:  String
            Note: This argument is not required or used when the table to be created
                  is not a PTI table. It will be ignored if specified without the timecode_column.
        columns_list:
            Optional Argument.
            Required if timebucket_duration is not specified.
            Used when the DataFrame must be saved as a PTI table.
            Specifies a list of one or more PTI table column names.
            Types: String or list of Strings
            Note: This argument is not required or used when the table to be created
                  is not a PTI table. It will be ignored if specified without the timecode_column.
        sequence_column:
            Optional Argument.
            Used when the DataFrame must be saved as a PTI table.
            Specifies the column of type Integer containing the unique identifier for
            time series data readings when they are not unique in time.
            * When specified, implies SEQUENCED, meaning more than one reading from the same
              sensor may have the same timestamp.
              This column will be the TD_SEQNO column in the table created.
            * When not specified, implies NONSEQUENCED, meaning there is only one sensor reading
              per timestamp.
              This is the default.
            Types: str
            Note: This argument is not required or used when the table to be created
                  is not a PTI table. It will be ignored if specified without the timecode_column.
        seq_max:
            Optional Argument.
            Used when the DataFrame must be saved as a PTI table.
            Specifies the maximum number of sensor data rows that can have the
            same timestamp. Can be used when 'sequenced' is True.
            Accepted range:  1 - 2147483647.
            Default Value: 20000.
            Types: int
            Note: This argument is not required or used when the table to be created
                  is not a PTI table. It will be ignored if specified without the timecode_column.
        set_table:
            Optional Argument.
            Specifies a flag to determine whether to create a SET or a MULTISET table.
            When True, a SET table is created.
            When False, a MULTISET table is created.
            Default value: False
            Types: boolean
            Note: 1. Specifying set_table=True also requires specifying primary_index or timecode_column.
                  2. Creating SET table (set_table=True) may result in loss of duplicate rows.
                  3. This argument has no effect if the table already exists and if_exists='append'.
    RETURNS:
        None
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df2 = df[(df.gpa == 4.00)]
        >>> df2.to_sql('to_sql_example', primary_index='id')
        >>> df3 = DataFrame('to_sql_example')
        >>> df3
           masters  gpa     stats programming admitted
        id
        13      no  4.0  Advanced      Novice        1
        29     yes  4.0    Novice    Beginner        0
        15     yes  4.0  Advanced    Advanced        1
        >>>
        >>> # Save as PTI table making sure it is a SET table
        >>> load_example_data("sessionize", "sessionize_table")
        >>> df4 = DataFrame('sessionize_table')
        >>> df4.to_sql("test_copyto_pti",
                       timecode_column='clicktime',
                       columns_list='event',
                       set_table=True
                      )
        >>> df5 = DataFrame('test_copyto_pti')
        >>> df5
                                TD_TIMECODE partition_id adid productid
        event
        click    2009-07-04 09:18:17.000000         1231    1      1001
        click    2009-07-24 04:18:10.000000         1167    2      1001
        click    2009-08-08 02:18:12.000000         9231    3      1001
        click    2009-08-11 00:01:24.000000         9231    3      1001
        page_02  2009-08-22 04:20:05.000000         1039    5      1001
        page_02  2009-08-27 23:03:05.000000         1039    5      1001
        view     2009-02-09 15:17:59.000000         1263    4      1001
        view     2009-03-09 21:17:59.000000         1199    2      1001
        view     2009-03-13 17:17:59.000000         1071    4      1001
        view     2009-03-19 01:17:59.000000         1199    1      1001
    unpivot
    teradataml.dataframe.dataframe.DataFrame.unpivot = unpivot(self, columns=None, transpose_column=None, measure_columns=None, exclude_nulls=True, returns=None,
    all_columns=False, **kwargs)
    DESCRIPTION:
        Rotate data from columns into rows to create easy-to-read DataFrames.  
    PARAMETERS:
        columns:
            Required when keyword arguments are not specified, optional otherwise.
            Specifies the dictonary of column(s) with distinct column 
            value(s) used for unpivoting the data.
            Notes:
                * key is a column expression or tuple of column expressions.
                * value is a literal to be taken by column when transposed 
                  into "transpose_column". If not specified, value is generated
                  by the function based on column names.
                * Number of columns specified in each key should be equal.
            For example:
                columns={(df.Q101Sales, df.Q101Cogs): "2001_Q1",
                         (df.Q201Sales, df.Q201Cogs): None,
                         (df.Q301Sales, df.Q301Cogs): "2001_Q3"},
                transpose_column="yr_qtr",
                This example transposes columns 'Q101Sales' and 'Q101Cogs' 
                into a row where 'yr_qtr' column value would be '2001_Q1'.
                Similarly, 'Q301Sales' and 'Q301Cogs' into row 
                with '2001_Q3' value in 'yr_qtr' column.
                For 'Q201Sales' and 'Q201Cogs' value of 'yr_qtr' is
                function generated.
            Types: dict
        transpose_column:
            Required Argument.
            Specifies the name of the column in the output DataFrame, which holds 
            the data for columns specified in keys of "columns" argument.
            Types: str
        measure_columns:
            Required Argument. 
            Specifies the name(s) of output column(s) to unpivot the data in the columns
            specified in "columns" argument.
            Notes:
                * Number of columns specified in "measure_columns" should be equal 
                  to number of columns in specified in each key of "columns".
                * One exception for above is when all the columns is to be unpivoted
                  into a single column. In such case, "measure_columns" should be
                  specified as a string or list of string with one value.
            Types: str or list of str (string)
        exclude_nulls:
            Optional Argument.
            Specifies whether to exclude NULL(None) values while unpivoting the data.
            When set to True, excludes NULL(None), otherwise includes NULL.
            Default Value: True
            Types: bool
        returns:
            Optional Argument.
            Specifies the custom column name(s) to be returned in the output DataFrame.
            Notes:
                * Number of column names should be equal to one greater than number in
                  "measure_column". 
                * All columns including columns in DataFrame which do not participate 
                  in unpivot must be specified if "all_columns" is set to True.
            Types: str OR list of Strings (str)
        all_columns:
            Optional Argument.
            Specifies whether "returns" argument should include the names of only unpivot 
            columns or all columns.
            When set to True, all columns including columns in DataFrame which do not
            participate in unpivot must be specified.
            When set to False, only output columns excluding columns in DataFrame which do not
            participate in unpivot must be specified.
            Default Value: False
            Types: bool
        **kwargs:
            Specifies the keyword argument to accept column name and column value(s)
            as named arguments.
            For example:
                col1=df.year, col1_value=2001, col2=df.qtr, col2_value='Q1'
                or 
                col1=(df.Q101Sales, df.Q101Cogs), col1_value="2001_Q1",
                col2=(df.Q201Sales, df.Q201Cogs), col2_value=None,
                col3=(df.Q301Sales, df.Q301Cogs), col3_value="2001_Q3"
            Notes:
                * Either use "columns" argument or keyword arguments.
                * Format for column name arguments should be col1, col2,.. colN with
                  values of type ColumnExpression.
                * Format for column value argument should be col1_value, col2_value,.. colN_value.
    RETURNS:
            teradataml DataFrame
            Notes:
                * The columns which are not participating in unpivoting always aligned to 
                  left most in the output DataFrame. 
                * Order of unpivot columns in output DataFrame follows the same order as the 
                  values specified in argument "columns" or the order in keyword arguments. 
                * The output columns are named according to the values specified in 
                  "transpose_column" and "measure_columns" or "returns" argument.
    RAISES:
        TypeError, ValueError, TeradataMLException
    EXAMPLES:
        # Create a teradataml DataFrame.
        >>> load_example_data("teradataml", "star1")
        >>> df = DataFrame("star1")
        >>> print(df)
                state    yr qtr  sales  cogs
        country
        USA        NY  2001  Q1   45.0  25.0
        CANADA     ON  2001  Q2   10.0   0.0
        CANADA     BC  2001  Q3   10.0   0.0
        USA        CA  2002  Q2   50.0  20.0
        USA        CA  2002  Q1   30.0  15.0
        # Create a pivot DataFrame. 
        >>> df = df.pivot(columns={df.qtr: ["Q1", "Q2", "Q3"], df.yr: ["2001"]},
        ...               aggfuncs=[df.sales.sum(), df.cogs.sum()],
        ...               returns=["Q101Sales", "Q201Sales", "Q301Sales",
        ...                        "Q101Cogs", "Q201Cogs", "Q301Cogs"])
        >>> print(df)
          country state  Q101Sales  Q201Sales  Q301Sales  Q101Cogs  Q201Cogs  Q301Cogs
        0  CANADA    BC        NaN        NaN        NaN       NaN      10.0       0.0
        1  CANADA    ON        NaN        NaN       10.0       0.0       NaN       NaN
        2     USA    NY       45.0       25.0        NaN       NaN       NaN       NaN
        3     USA    CA        NaN        NaN        NaN       NaN       NaN       NaN
        # Example 1: Unpivot quarterly sales data to 'sales' column and quarterly 
        #            cogs data to 'cogs' columns using "columns" argument.
        >>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs): "2001_Q1",
        ...                                  (df.Q201Sales, df.Q201Cogs): None,
        ...                                  (df.Q301Sales, df.Q301Cogs): "2001_Q3"},
        ...                         transpose_column="yr_qtr",
        ...                         measure_columns=["sales", "cogs"])
        >>> print(unpivot_df)
          country state              yr_qtr  sales  cogs
        0  CANADA    BC  Q201Sales_Q201Cogs    NaN  10.0
        1  CANADA    ON             2001_Q1    NaN   0.0
        2  CANADA    ON             2001_Q3   10.0   NaN
        3  CANADA    BC             2001_Q3    NaN   0.0
        4     USA    NY  Q201Sales_Q201Cogs   25.0   NaN
        5     USA    NY             2001_Q1   45.0   NaN
        # Example 2: Unpivot 'sales' and 'cogs' in to a single column 'sales_cogs'.
        >>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs,
        ...                                  df.Q201Sales, df.Q201Cogs,
        ...                                  df.Q301Sales, df.Q301Cogs): None},
        ...                         transpose_column="yr_qtr",
        ...                         measure_columns="sales_cogs")
        >>> print(unpivot_df)
          country state     yr_qtr  sales_cogs
        0  CANADA    BC   Q201Cogs        10.0
        1  CANADA    ON   Q101Cogs         0.0
        2  CANADA    ON  Q301Sales        10.0
        3  CANADA    BC   Q301Cogs         0.0
        4     USA    NY  Q201Sales        25.0
        5     USA    NY  Q101Sales        45.0
        # Example 3: Unpivot quarterly sales data to 'sales' column and quarterly 
        #            cogs data to 'cogs' columns using keyword arguments.    
        >>> unpivot_df = df.unpivot(transpose_column="yr_qtr",
        ...                         measure_columns=["sales", "cogs"],
        ...                         col1 = (df.Q101Sales, df.Q101Cogs),
        ...                         col1_value = "Q101",
        ...                         col2 = (df.Q201Sales, df.Q201Cogs),
        ...                         col2_value = None,
        ...                         col3 = (df.Q301Sales, df.Q301Cogs),
        ...                         col3_value = "Q103")
        >>> print(unpivot_df)
          country state              yr_qtr  sales  cogs
        0  CANADA    BC  Q201Sales_Q201Cogs    NaN  10.0
        1  CANADA    ON                Q101    NaN   0.0
        2  CANADA    ON                Q103   10.0   NaN
        3  CANADA    BC                Q103    NaN   0.0
        4     USA    NY  Q201Sales_Q201Cogs   25.0   NaN
        5     USA    NY                Q101   45.0   NaN
        # Example 4: Unpivot quarterly sales data to 'sales' column and quarterly
        #            cogs data to 'cogs' columns and rename column of the
        #            output using "returns" argument.
        >>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs): "Q101",
        ...                                  (df.Q201Sales, df.Q201Cogs): None,
        ...                                  (df.Q301Sales, df.Q301Cogs): "Q301"},
        ...                         transpose_column="yr_qtr",
        ...                         measure_columns=["sales", "cogs"],
        ...                         returns=["year_quarter", "sales_data", "cogs_data"])
        >>> print(unpivot_df)
          country state        year_quarter  sales_data  cogs_data
        0  CANADA    BC  Q201Sales_Q201Cogs         NaN       10.0
        1  CANADA    ON                Q101         NaN        0.0
        2  CANADA    ON                Q301        10.0        NaN
        3  CANADA    BC                Q301         NaN        0.0
        4     USA    NY  Q201Sales_Q201Cogs        25.0        NaN
        5     USA    NY                Q101        45.0        NaN
        # Example 5: Unpivot quarterly sales data to 'sales' column and quarterly
        #            cogs data to 'cogs' columns and rename each column of the
        #            output using "returns" and "all_columns" argument.
        >>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs): "Q101",
        ...                                  (df.Q201Sales, df.Q201Cogs): None,
        ...                                  (df.Q301Sales, df.Q301Cogs): "Q301"},
        ...                         transpose_column="yr_qtr",
        ...                         measure_columns=["sales", "cogs"],
        ...                         returns=["con", "st", "year_quarter",
        ...                                  "sales_data", "cogs_data"],
        ...                         all_columns=True)
        >>> print(unpivot_df)
              con  st        year_quarter  sales_data  cogs_data
        0  CANADA  BC  Q201Sales_Q201Cogs         NaN       10.0
        1  CANADA  ON                Q101         NaN        0.0
        2  CANADA  ON                Q301        10.0        NaN
        3  CANADA  BC                Q301         NaN        0.0
        4     USA  NY  Q201Sales_Q201Cogs        25.0        NaN
        5     USA  NY                Q101        45.0        NaN
        # Example 6: Unpivot quarterly sales data to 'sales' column and quarterly
        #            cogs data to 'cogs' columns by including NULLs.
        >>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs): "2001_Q1",
        ...                                  (df.Q201Sales, df.Q201Cogs): None,
        ...                                  (df.Q301Sales, df.Q301Cogs): "2001_Q3"},
        ...                         transpose_column="yr_qtr",
        ...                         measure_columns=["sales", "cogs"],
        ...                         exclude_nulls=False)
        >>> print(unpivot_df)
          country state              yr_qtr  sales  cogs
        0     USA    CA             2001_Q3    NaN   NaN
        1     USA    NY  Q201Sales_Q201Cogs   25.0   NaN
        2     USA    NY             2001_Q3    NaN   NaN
        3  CANADA    BC             2001_Q1    NaN   NaN
        4  CANADA    BC             2001_Q3    NaN   0.0
        5  CANADA    ON             2001_Q1    NaN   0.0
        6  CANADA    BC  Q201Sales_Q201Cogs    NaN  10.0
        7     USA    NY             2001_Q1   45.0   NaN
        8     USA    CA  Q201Sales_Q201Cogs    NaN   NaN
        9     USA    CA             2001_Q1    NaN   NaN
    window
    teradataml.dataframe.dataframe.DataFrame.window = window(self, partition_columns=None, order_columns=None, sort_ascending=True, nulls_ﬁrst=None,
    window_start_point=None, window_end_point=None, ignore_window=False)
    DESCRIPTION:
        This function generates Window object on a teradataml DataFrame to run
        window aggregate functions.
        Function allows user to specify window for different types of
        computations:
            * Cumulative
            * Group
            * Moving
            * Remaining
        By default, window with Unbounded Preceding and Unbounded Following
        is considered for calculation.
        Note:
            If both "partition_columns" and "order_columns" are None and
            original DataFrame has BLOB and CLOB type of columns, then
                * Window aggregate operation on CLOB and BLOB type of
                  columns is omitted.
                * Resultant DataFrame does not contain the BLOB and CLOB
                  type of columns from original DataFrame.
    PARAMETERS:
        partition_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) over which the ordered
            aggregate function executes by partitioning the rows. Such a
            grouping is static.
            Notes:
                 1. If this argument is not specified, then the entire data
                    from teradataml DataFrame, constitutes a single
                    partition, over which the ordered aggregate function
                    executes.
                 2. "partition_columns" does not support CLOB and BLOB type
                    of columns.
                    Refer 'DataFrame.tdtypes' to get the types of the
                    columns of a teradataml DataFrame.
                 3. "partition_columns" supports only columns specified in
                    groupby function, if window is initiated on DataFrameGroupBy.
            Types: str OR list of Strings (str) OR ColumnExpression OR list of ColumnExpressions
        order_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to order the rows in a
            partition, which determines the sort order of the rows over
            which the function is applied.
            Notes:
                1. "order_columns" does not support CLOB and BLOB type
                   of columns.
                   Refer 'DataFrame.tdtypes' to get the types of the
                   columns of a teradataml DataFrame.
                2. "order_columns" supports only columns specified in
                    groupby, if window is initiated on DataFrameGroupBy.
                3. When ColumnExpression(s) is(are) passed to "order_columns", then the
                       corresponding expression takes precedence over arguments
                       "sort_ascending" and "nulls_first". Say, ColumnExpression is col1, then
                       1. col1.asc() or col.desc() is effective irrespective of "sort_ascending".
                       2. col1.nulls_first() or col.nulls_last() is effective irrespective of "nulls_first".
                       3. Any combination of above two take precedence over "sort_ascending" and "nulls_first".
            Types: str OR list of Strings (str) OR ColumnExpression OR list of ColumnExpressions
        sort_ascending:
            Optional Argument.
            Specifies whether column ordering should be in ascending or
            descending order.
            Default Value: True (ascending)
            Notes:
                 * When "order_columns" argument is not specified, this argument
                   is ignored.
                 * When ColumnExpression(s) is(are) passed to "order_columns", then the
                   argument is ignored.
            Types: bool
        nulls_first:
            Optional Argument.
            Specifies whether null results are to be listed first or last
            or scattered.
            Default Value: None
            Notes:
                 * When "order_columns" argument is not specified, this argument
                   is ignored.
                 * When "order_columns" is a ColumnExpression(s), this argument
                   is ignored.
            Types: bool
        window_start_point:
            Optional Argument.
            Specifies a starting point for a window. Based on the integer
            value, n, starting point of the window is decided.
                * If 'n' is negative, window start point is n rows
                  preceding the current row/data point.
                * If 'n' is positive, window start point is n rows
                  following the current row/data point.
                * If 'n' is 0, window start at current row itself.
                * If 'n' is None, window start as Unbounded preceding,
                  i.e., all rows before current row/data point are
                  considered.
            Notes:
                 1. Value passed to this should always satisfy following condition:
                    window_start_point <= window_end_point
                 2. Following functions does not require any window to
                    perform window aggregation. So, "window_start_point" is
                    insignificant for below functions:
                    * cume_dist
                    * rank
                    * dense_rank
                    * percent_rank
                    * row_number
                    * lead
                    * lag
            Default Value: None
            Types: int
        window_end_point:
            Optional Argument.
            Specifies an end point for a window. Based on the integer value,
            n, starting point of the window is decided.
                * If 'n' is negative, window end point is n rows preceding
                  the current row/data point.
                * If 'n' is positive, window end point is n rows following
                  the current row/data point.
                * If 'n' is 0, window end's at current row itself.
                * If 'n' is None, window end's at Unbounded Following,
                  i.e., all rows before current row/data point are
                  considered.
            Notes:
                 1. Value passed to this should always satisfy following condition:
                    window_start_point <= window_end_point
                 2. Following functions does not require any window to
                    perform window aggregation. So, "window_end_point" is
                    insignificant for below functions:
                    * cume_dist
                    * rank
                    * dense_rank
                    * percent_rank
                    * row_number
                    * lead
                    * lag
            Default Value: None
            Types: int
        ignore_window:
            Optional Argument.
            Specifies a flag to ignore parameters related to creating
            window ("window_start_point", "window_end_point") and use other
            arguments, if specified.
            When set to True, window is ignored, i.e., ROWS clause is not
            included.
            When set to False, window will be created, which is specified
            by "window_start_point" and "window_end_point" parameters.
            Default Value: False
            Types: bool
    RAISES:
        TypeError, ValueError
    RETURNS:
        An object of type Window.
    EXAMPLES:
        # Example 1: Create a window on a teradataml DataFrame.
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> window = df.window()
        >>>
        # Example 2: Create a cumulative (expanding) window with rows
        #            between unbounded preceding and 3 preceding with
        #            "partition_columns" and "order_columns" argument with
        #            default sorting.
        >>> window = df.window(partition_columns=df.Feb,
        ...                    order_columns=[df.Feb, "datetime"],
        ...                    window_start_point=None,
        ...                    window_end_point=-3)
        >>>
        # Example 3: Create a moving (rolling) window with rows between
        #            current row and 3 following with sorting done on 'Feb'
        #            in ascending order, datetime' columns in descending order
        #            and "partition_columns" argument.
        >>> window = df.window(partition_columns=df.Feb,
        ...                    order_columns=[df.Feb.asc(), df.datetime.desc()],
        ...                    window_start_point=0,
        ...                    window_end_point=3)
        >>>
        # Example 4: Create a remaining (contracting) window with rows
        #            between current row and unbounded following with
        #            sorting done on 'Feb', 'datetime' columns in ascending
        #            order and NULL values in 'Feb', 'datetime'
        #            columns appears at last.
        >>> window = df.window(partition_columns=df.Feb,
        ...                    order_columns=[df.Feb.nulls_first(), df.datetime.nulls_first()],
        ...                    window_start_point=0,
        ...                    window_end_point=None)
        >>>
        # Example 5: Create a grouping window, with sorting done on 'Feb',
        #            'datetime' columns in ascending order with NULL values
        #            in 'Feb' column appears at first and 'datetime' column
        #            appears at last.
        >>> window = df.window(partition_columns="Feb",
        ...                    order_columns=[df.Feb.nulls_first(), df.datetime.nulls_last()],
        ...                    window_start_point=None,
        ...                    window_end_point=None)
        >>>
        # Example 6: Create a window on a teradataml DataFrame, which
        #            ignores all the parameters while creating window.
        >>> window = df.window(partition_columns=df.Feb,
        ...                    order_columns=[df.Feb.desc().nulls_last(), df.datetime.desc().nulls_last()]
        ...                    ignore_window=True)
        >>>
        # Example 7: Perform sum on every valid column in DataFrame.
        >>> window = df.window(partition_columns="Feb",
        ...                    order_columns=["Feb", "datetime"],
        ...                    sort_ascending=False,
        ...                    nulls_first=False,
        ...                    ignore_window=True)
        >>> window.sum()
                      Feb    Jan    Mar    Apr    datetime  Apr_sum  Feb_sum  Jan_sum  Mar_sum
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017      781   1000.0      550      590
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017      781   1000.0      550      590
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017      781   1000.0      550      590
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017      781   1000.0      550      590
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017      781   1000.0      550      590
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017      781   1000.0      550      590
        # Example 8: Perform count on every valid column in DataFrame.
        >>> window = df.window()
        >>> window.count()
                      Feb    Jan    Mar    Apr    datetime  Apr_count  Feb_count  Jan_count  Mar_count  accounts_count  datetime_co
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017          4          6          4          4               6             
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017          4          6          4          4               6             
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017          4          6          4          4               6             
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017          4          6          4          4               6             
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017          4          6          4          4               6             
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017          4          6          4          4               6             
        >>>
        # Example 9: Perform count of all the valid columns in teradataml
        # DataFrame, which is grouped by 'accounts'.
        >>> window = df.groupby("accounts").window()
        >>> window.count()
             accounts  accounts_count
        0   Jones LLC               6
        1     Red Inc               6
        2  Yellow Inc               6
        3  Orange Inc               6
        4    Blue Inc               6
        5    Alpha Co               6
        >>>
    DataFrame Aggregate Functions
    agg
    teradataml.dataframe.dataframe.DataFrame.agg = agg(self, func=None)
    DESCRIPTION:
        Perform aggregates using one or more operations.
    PARAMETERS:
        func:
            Required Argument.
            Specifies the function(s) to apply on DataFrame columns.
            Valid values for func are:
                * 'count', 'sum', 'min', 'max', 'mean', 'std', 'percentile', 'percentile_<floatvalue>', 'unique',
                  'median', 'var'
                * Note: In 'percentile_<floatvalue>', <floatvalue> specifies the desired percentile value to
                        calculate aggregate. It should be in the range of 0.0 to 1.0 (both inclusive).
            Acceptable formats for function(s) are
                string, dictionary, list of strings/functions/ColumnExpression or ColumnExpression.
            Accepted combinations are:
                1. String function name
                2. List of string functions
                3. Dictionary containing column name as key and
                   aggregate function name (string or list of
                   strings) as value
                4. ColumnExpression built using the aggregate functions. 
                5. List of ColumnExpression built using the aggregate functions.
            Note:
            * The name of the output columns are generated based on aggregate functions and column names.
                For Example, 
                1. "func" passed as a string. 
                    >>> df.agg('mean')
                    Assume that the column names of the dataframe are employee_no, first_name, marks, dob, joined_date.
                    After the above operation, the output column names are: 
                      mean_employee_no, mean_marks, mean_dob, mean_joined_date 
                2. "func" passed as a list of string functions.
                    >>> df.agg(['min', 'sum'])
                    Assume that the column names of the dataframe are employee_no, first_name, marks, dob, joined_date.
                    After the above operation, the output column names are:
                      min_employee_no, sum_employee_no, min_first_name, min_marks, sum_marks, min_dob, min_joined_date
                3. "func" passed as a dictionary containing column name as key and aggregate function name as value. 
                    >>> df.agg({'employee_no' : ['min', 'sum', 'var'], 'first_name' : ['min']})
                    Output column names after the above operation are:
                      min_employee_no, sum_employee_no, var_employee_no, min_first_name
                4. "percentile_<floatvalue>" passed to agg.
                    >>> df.agg({'employee_no' : ['percentile_0.25', 'percentile_0.75', 'min']})
                    >>> df.agg(['percentile_0.25', 'percentile_0.75', 'sum'])
                    >>> df.agg('percentile_0.25')
                5. "func" passed as a ColumnExpression built using the aggregate functions.
                    >>> df.agg(df.first_name.count())
                    Output column name after the above operation is:
                      count(first_name)
                6. "func" passed as a list of ColumnExpression built using the aggregate functions.
                    >>> df.agg([df.employee_no.min(), df.first_name.count()])
                    Output column names after the above operation are:
                      min(employee_no), count(first_name)
            * On ColumnExpression or list of ColumnExpression alias() can be used to 
              return the output columns with aliased name.
                For Example,
                >>> df.agg(df.first_name.count().alias("total_names"))
                Output column name after the above operation is:
                  total_names
                >>> df.agg([df.joined_date.min().alias("min_date"), df.first_name.count().alias("total_names")])
                Output column names after the above operation are:
                  min_date, total_names
    RETURNS:
        teradataml DataFrame object with operations
        mentioned in parameter 'func' performed on specified
        columns.
    RAISES:
        TeradataMLException
        1. TDMLDF_AGGREGATE_FAILED - If operations on given columns
            fail to generate aggregate dataframe.
            Possible error message:
            Unable to perform 'agg()' on the dataframe.
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the provided
            aggregate operations do not support specified columns.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col1 - VARCHAR)] is/are
            unsupported for 'sum' operation.
        3. TDMLDF_INVALID_AGGREGATE_OPERATION - If the aggregate
            operation(s) received in parameter 'func' is/are
            invalid.
            Possible error message:
            Invalid aggregate operation(s): minimum, counter.
            Valid aggregate operation(s): count, max, mean, min,
            std, sum.
        4. TDMLDF_AGGREGATE_INVALID_COLUMN - If any of the columns
            specified in 'func' is not present in the dataframe.
            Possible error message:
            Invalid column(s) given in parameter func: col1.
            Valid column(s) : A, B, C, D.
        5. MISSING_ARGS - If the argument 'func' is missing.
            Possible error message:
            Following required arguments are missing: func.
        6. UNSUPPORTED_DATATYPE - If the argument 'func' is not of
            valid datatype.
            Possible error message:
            Invalid type(s) passed to argument 'func', should be:
            ['str, dict, ColumnExpression or list of values of type(s): str, ColumnExpression'].
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info", "sales"])
        # Create teradataml dataframe.
        >>> df = DataFrame("employee_info")
        >>> print(df)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Get the minimum, sum and variance of employee number and minimum and mean of name,
        # by passing dictionary of column names to string function/list of string functions as parameter.
        >>> df.agg({'employee_no' : ['min', 'sum', 'var'], 'first_name' : ['min', 'mean']})
          min_employee_no sum_employee_no  var_employee_no min_first_name
        0             100             313        44.333333           abcd
        # Get the minimum, 25 percentile value and variance of employee number, by passing dictionary of
        # column names to string function/list of string functions as parameter.
        >>> df.agg({'employee_no' : ['min', 'percentile_0.25', 'var']})
          min_employee_no  percentile_0.25_employee_no  var_employee_no
        0              100                          100        44.333333
        # Get the minimum and sum of all the columns in the dataframe,
        # by passing list of string functions as parameter.
        >>> df.agg(['min', 'sum'])
          min_employee_no sum_employee_no min_first_name min_marks sum_marks min_dob min_joined_date
        0             100             313           abcd      None      None    None      1902-05-12
        # Get the mean of all the columns in the dataframe, by passing string function as parameter.
        >>> df.agg('mean')
           mean_employee_no mean_marks mean_dob mean_joined_date
        0        104.333333       None     None         60/12/04
        # Get the total names in the dataframe, by running count() on the "first_name"
        # and passing ColumnExpression as parameter.
        >>> df.agg(df.first_name.count())
           count(first_name)
        0                  2
        # Get the minimum of joining date and total of names in the dataframe, 
        # by running min() on joined_date and count() on the "first_name"
        # and passing list of ColumnExpression as parameter.
        >>> df.agg([df.employee_no.min(), df.first_name.count()])
           min(employee_no)  count(first_name)
        0               100                  2
        # Get the total names in the dataframe, by running count() on the "first_name" and
        # use alias() to have the output column named as "total_names".
        >>> df.agg(df.first_name.count().alias("total_names"))
           total_names
        0            2
        # Get the minimum of joining date and total names in the dataframe, 
        # by running min() on joined_date and count() on the "first_name" and 
        # use alias() to have the output column named as "min_date" and "total_names".
        >>> df.agg([df.joined_date.min().alias("min_date"), df.first_name.count().alias("total_names")])
           min_date  total_names
        0  02/12/05            2
        # Select only subset of columns from the DataFrame.
        >>> df1 = df.select(['employee_no', 'first_name', 'joined_date'])
        # List of string functions as parameter.
        >>> df1.agg(['mean', 'unique'])
           mean_employee_no unique_employee_no unique_first_name mean_joined_date unique_joined_date
        0        104.333333                  3                 2         60/12/04                  2
        # Get the percentile of each column in the dataframe with default value 0.5.
        >>> df.agg('percentile')
            percentile_employee_no percentile_marks
        0                    101             None
        # Get 80 percentile of each column in the datafame.
        >>> df.agg('percentile_0.8')
           percentile_0.8_employee_no percentile_0.8_marks
        0                         107                 None
        # Using another table 'sales' (having repeated values) to demonstrate operations
        # 'unique' and 'percentile'.
        # Create teradataml dataframe.
        >>> df = DataFrame('sales')
        >>> df
                          Feb   Jan   Mar   Apr    datetime
            accounts
            Yellow Inc   90.0  None  None  None  2017-04-01
            Alpha Co    210.0   200   215   250  2017-04-01
            Jones LLC   200.0   150   140   180  2017-04-01
            Orange Inc  210.0  None  None   250  2017-04-01
            Blue Inc     90.0    50    95   101  2017-04-01
            Red Inc     200.0   150   140  None  2017-04-01
        # Get 80 and 40 percentile values of each column in the dataframe.
        >>> df1 = df.select(['Feb', 'Jan', 'Mar', 'Apr'])
        >>> df1.agg(['percentile_0.8', 'percentile_0.4'])
            percentile_0.8_Feb  percentile_0.4_Feb  percentile_0.8_Jan  percentile_0.4_Jan  percentile_0.8_Mar  percentile_0.4_Mar 
        0               210.0               200.0                 170                 150                 170                 140  
        >>> df.agg('unique')
              unique_accounts unique_Feb unique_Jan unique_Mar unique_Apr unique_datetime
            0               6          3          3          3          3               1
    corr
    teradataml.dataframe.dataframe.DataFrame.corr = corr(expression)
    DESCRIPTION:
        Function returns the column-wise Sample Pearson product moment correlation
        coefficient of its arguments for all non-null data point pairs.
        The Sample Pearson product moment correlation coefficient is a measure of
        the linear association between variables. The boundary on the computed
        coefficient ranges from -1.00 to +1.00.
        Note that high correlation does not imply a causal relationship between
        the variables.
        The coefficient of correlation between two variables has the following four extreme values:
            1. -1.00 : Association between the variables is perfectly linear, but inverse.
                       As the value in ColumnExpression varies, the value for
                       "expression" varies identically in the opposite direction.
            2. 0     : Association between the variables does not exist and they are considered
                       to be uncorrelated.
            3. +1.00 : Association between the variables is perfectly linear.
                       As the value in ColumnExpression varies, the value for
                       "expression" varies identically in the same direction.
            4. NULL  : Association between the variables cannot be measured because there
                       are no non-null data point pairs in the data used for the computation.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or name of the column or
            a numeric literal to be correlated with ColumnExpression.
            Types: ColumnExpression OR str OR int OR float
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the correlation between all the valid columns
        #            and 'gpa' in teradataml DataFrame.
        >>> df = admissions_train.corr(admissions_train.gpa)
        >>> df
            corr_id  corr_gpa  corr_admitted
        0 -0.013896       1.0      -0.022265
        >>>
        # Example 2: Calculate the correlation between all the valid columns
        #            and 'gpa' in teradataml DataFrame, for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").corr(admissions_train.gpa)
        >>> df
          programming   corr_id  corr_gpa  corr_admitted
        0    Advanced  0.166604       1.0       0.487737
        1      Novice  0.022877       1.0      -0.114656
        2    Beginner -0.277338       1.0      -0.417565
    count
    teradataml.dataframe.dataframe.DataFrame.count = count(self, distinct=False)
    DESCRIPTION:
        Returns column-wise count of the dataframe.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the count.
            Default Value: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with count() operation
        performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If count() operation fails to
            generate the column-wise count of the dataframe.
            Possible error message:
            Failed to perform 'count'. (Followed by error message).
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the count() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'count' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df2 = df1.select(['employee_no', 'first_name', 'marks'])
        # Prints count of the values in all the selected columns
        # (excluding None types).
        >>> df2.count()
          count_employee_no count_first_name count_marks
        0                 3                2           0
        >>>
        #
        # Using count() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys_seq"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing count() function on DataFrame created on
        #                                  sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the count.
        #
        >>> ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        44      2014-01-06 10:00:25.122200         6        55           43  2014-06-06
        44      2014-01-06 10:01:25.122200         8        55           53  2014-08-08
        44      2014-01-06 10:01:25.122200        20        55           54  2015-08-20
        1       2014-01-06 09:01:25.122200        11        55           70  2014-11-11
        1       2014-01-06 09:02:25.122200        12        55           71  2014-12-12
        1       2014-01-06 09:02:25.122200        24        55           78  2015-12-24
        1       2014-01-06 09:03:25.122200        13        55           72  2015-01-13
        1       2014-01-06 09:03:25.122200        25        55           79  2016-01-25
        1       2014-01-06 09:01:25.122200        23        55           77  2015-11-23
        44      2014-01-06 10:00:26.122200         7        55           43  2014-07-07
        >>>
        # To use count() as Time Series Aggregate we must run groupby_time() first, followed by count().
        >>> ocean_buoys_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="2cy", value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.count().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  count_TD_TIMECODE  count_TD_SEQN
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                  5               5               5                  4            5
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                  6               6               6                  6            6
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                  3               3               3                  3            3
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      22                  1               1               1                  1            1
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                 13              13              13                 13           13
        >>>
        #
        # Time Series Aggregate Example 2: Executing count() function on DataFrame created on
        #                                  sequenced PTI table. We will consider DISTINCT rows for the
        #                                  columns while calculating the count.
        #
        # To use count() as Time Series Aggregate we must run groupby_time() first, followed by count().
        >>> ocean_buoys_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="2cy",value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.count(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  count_TD_TIMECODE  count_TD_SEQN
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                  4               5               1                  3            5
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                  3               6               1                  6            6
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                  3               3               1                  3            3
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      22                  1               1               1                  1            1
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                 10              13               1                  5           13
        >>>
    covar_pop
    teradataml.dataframe.dataframe.DataFrame.covar_pop = covar_pop(expression)
    DESCRIPTION:
        Function returns the column-wise population covariance of its arguments
        for all non-null data point pairs. Covariance measures whether or not
        two random variables vary in the same way. It is the average of the
        products of deviations for each non-null data point pair.
        Notes:
            1. When there are no non-null data point pairs in the data used for
               the computation, the function returns None.
            2. High covariance does not imply a causal relationship between
               the variables.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or name of the column
            or a numeric literal to be paired with another variable to determine
            their population covariance.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate population covariance between 'admitted' and all the
        #            valid columns in teradataml DataFrame.
        >>> df = admissions_train.covar_pop(admissions_train.admitted)
        >>> df
           covar_pop_id  covar_pop_gpa  covar_pop_admitted
        0        -1.075      -0.005387              0.2275
        >>>
        # Example 2: Calculate population covariance between 'admitted' and all the
        #            valid columns in teradataml DataFrame, for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").covar_pop(admissions_train.admitted)
        >>> df
          programming  covar_pop_id  covar_pop_gpa  covar_pop_admitted
        0    Beginner      0.171598      -0.069231            0.236686
        1    Advanced     -0.597656       0.091055            0.152344
        2      Novice     -0.900826      -0.031488            0.198347
        >>>
    covar_samp
    teradataml.dataframe.dataframe.DataFrame.covar_samp = covar_samp(expression)
    DESCRIPTION:
        Function returns the column-wise sample covariance of its arguments
        for all non-null data point pairs. Covariance measures whether or not
        two random variables vary in the same way. It is the average of the
        products of deviations for each non-null data point pair.
        Notes:
            1. When there are no non-null data point pairs in the data used for
               the computation, the function returns None.
            2. High covariance does not imply a causal relationship between
               the variables.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or name of the column
            or a numeric literal to be paired with another variable to determine
            their sample covariance.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate sample covariance between 'admitted'
        #            and all the valid columns in teradataml DataFrame.
        >>> df = admissions_train.covar_pop(admissions_train.admitted)
        >>> df
           covar_pop_id  covar_pop_gpa  covar_pop_admitted
        0        -1.075      -0.005387              0.2275
        >>>
        # Example 2: Calculate sample covariance between 'admitted'
        #            and all the valid columns in teradataml DataFrame, for
        #            each level of 'programming'.
        >>> df = admissions_train.groupby("programming").covar_samp(admissions_train.admitted)
        >>> df
          programming  covar_samp_id  covar_samp_gpa  covar_samp_admitted
        0    Advanced      -0.597665        0.091056             0.152346
        1      Novice      -0.900846       -0.031488             0.198352
        2    Beginner       0.171601       -0.069232             0.236691
        >>>
    csum
    teradataml.dataframe.dataframe.DataFrame.csum = csum(self, sort_columns, drop_columns=False)
    DESCRIPTION:
        Returns column-wise cumulative sum value for rows in the partition
        of the dataframe.
        Note:
            csum does not support below type of columns.
                * BLOB
                * BYTE
                * CHAR
                * CLOB
                * DATE
                * PERIOD_DATE
                * PERIOD_TIME
                * PERIOD_TIMESTAMP
                * TIME
                * TIMESTAMP
                * VARBYTE
                * VARCHAR            
    PARAMETERS:
        sort_columns:
            Required Argument.
            Specifies the columns to use for sorting.
            Note:
                "sort_columns" does not support CLOB and BLOB type of
                columns.
            Types: str (or) ColumnExpression (or) List of strings(str)
                   or ColumnExpressions
        drop_columns:
            Optional Argument.
            Specifies whether to retain all the input DataFrame columns
            in the output or not. When set to False, columns from input
            DataFrame are retained, dropped otherwise.
            Default Value: False
            Types: bool
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        teradataml DataFrame.
    EXAMPLES:
        # Load the data to run the example.
        >>> from teradataml import load_example_data
        >>> load_example_data("dataframe","sales")
        # Create teradataml dataframe.
        >>> df = DataFrame.from_table('sales')
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        >>>
        # Sorts the Data on column accounts in ascending order and
        # calculates cumulative sum.
        >>> df.csum(df.accounts)
                      Feb    Jan    Mar    Apr    datetime  csum_Feb  csum_Jan  csum_Mar  csum_Apr
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     500.0       400       450       531
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     910.0       550       590       781
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017    1000.0       550       590       781
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     710.0       400       450       781
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     300.0       250       310       351
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0       200       215       250
        >>>
        # Sorts the Data on column accounts in ascending order and column
        # Feb in descending order, then calculates cumulative sum by dropping
        # the input DataFrame columns.
        >>> df.csum(sort_columns=[df.accounts, df.Feb.desc()], drop_columns=True)
           csum_Feb  csum_Jan  csum_Mar  csum_Apr
        0     500.0       400       450       531
        1     910.0       550       590       781
        2    1000.0       550       590       781
        3     710.0       400       450       781
        4     300.0       250       310       351
        5     210.0       200       215       250
        >>>
    kurtosis
    teradataml.dataframe.dataframe.DataFrame.kurtosis = kurtosis(self, distinct=False)
    DESCRIPTION:
        Returns column-wise kurtosis value of the dataframe.
        Kurtosis is the fourth moment of the distribution of the standardized (z) values.
        It is a measure of the outlier (rare, extreme observation) character of the distribution as
        compared with the normal (or Gaussian) distribution.
            * The normal distribution has a kurtosis of 0.
            * Positive kurtosis indicates that the distribution is more outlier-prone than the
              normal distribution.
            * Negative kurtosis indicates that the distribution is less outlier-prone than the
              normal distribution.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
            3. Following conditions will produce null result:
                a. Fewer than three non-null data points in the data used for the computation.
                b. Standard deviation for a column is equal to 0.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the kurtosis value.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with kurtosis()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If kurtosis() operation fails to
           generate the column-wise kurtosis value of the dataframe.
           Possible error message:
           Failed to perform 'kurtosis'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the kurtosis() operation
           doesn't support all the columns in the dataframe.
           Possible error message:
           No results. Below is/are the error message(s):
           All selected columns [(col2 -  PERIOD_TIME), (col3 -
           BLOB)] is/are unsupported for 'kurtosis' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["admissions_train"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("admissions_train")
        >>> print(df1.sort("id"))
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        2      yes  3.76  Beginner    Beginner         0
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        8       no  3.60  Beginner    Advanced         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        >>>
        # Prints kurtosis value of each column
        >>> df1.kurtosis()
           kurtosis_id  kurtosis_gpa  kurtosis_admitted
        0         -1.2      4.052659            -1.6582
        >>>
        #
        # Using kurtosis() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        #
        # Time Series Aggregate Example 1: Executing kurtosis() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the kurtosis values.
        #
        # To use kurtosis() as Time Series Aggregate we must run groupby_time() first, followed by kurtosis().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.kurtosis().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid kurtosis_salinity  kurtosis_tempe
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0              None             -5.998128
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1              None             -2.758377
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2              None                   NaN
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44              None             -2.195395
        >>>
        #
        # Time Series Aggregate Example 2: Executing kurtosis() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the kurtosis value.
        #
        # To use kurtosis() as Time Series Aggregate we must run groupby_time() first, followed by kurtosis().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.kurtosis(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid kurtosis_salinity  kurtosis_tempe
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0              None                   NaN
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1              None             -2.758377
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2              None                   NaN
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44              None              4.128426
        >>>
    mavg
    teradataml.dataframe.dataframe.DataFrame.mavg = mavg(self, width, sort_columns, drop_columns=False)
    DESCRIPTION:
        Computes the moving average for the current row and the preceding
        "width"-1 rows in a partition, by sorting the rows according to
        "sort_columns".
        Note:
            mavg does not support below type of columns.
                * BLOB
                * BYTE
                * CHAR
                * CLOB
                * DATE
                * PERIOD_DATE
                * PERIOD_TIME
                * PERIOD_TIMESTAMP
                * TIME
                * TIMESTAMP
                * VARBYTE
                * VARCHAR            
    PARAMETERS:
        width:
            Required Argument.
            Specifies the width of the partition. "width" must be
            greater than 0 and less than or equal to 4096.
            Types: int
        sort_columns:
            Required Argument.
            Specifies the columns to use for sorting.
            Note:
                "sort_columns" does not support CLOB and BLOB type of
                columns.
            Types: str (or) ColumnExpression (or) List of strings(str)
                   or ColumnExpressions
        drop_columns:
            Optional Argument.
            Specifies whether to retain all the input DataFrame columns
            in the output or not. When set to False, columns from input
            DataFrame are retained, dropped otherwise.
            Default Value: False
            Types: bool
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        teradataml DataFrame.
    EXAMPLES:
        # Load the data to run the example.
        >>> from teradataml import load_example_data
        >>> load_example_data("dataframe","sales")
        # Create teradataml dataframe.
        >>> df = DataFrame.from_table('sales')
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        >>>
        # Sorts the Data on column accounts in ascending order and
        # calculates moving avergae on the window of size 2.
        >>> df.mavg(width=2, sort_columns=df.accounts)
                      Feb    Jan    Mar    Apr    datetime  mavg_Feb  mavg_Jan  mavg_Mar  mavg_Apr mavg_datetime
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     145.0     100.0     117.5     140.5    04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     205.0     150.0     140.0     250.0    04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     145.0     150.0     140.0       NaN    04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     205.0     150.0     140.0     215.0    04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     150.0     125.0     155.0     175.5    04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0     200.0     215.0     250.0    04/01/2017
        >>>
        # Sorts the Data on column accounts in ascending order and column
        # Feb in descending order, then calculates moving average by dropping
        # the input DataFrame columns on the window of size 2.
        >>> df.mavg(width=2, sort_columns=[df.accounts, df.Feb.desc()], drop_columns=True)
           mavg_Feb  mavg_Jan  mavg_Mar  mavg_Apr mavg_datetime
        0     145.0     100.0     117.5     140.5    04/01/2017
        1     205.0     150.0     140.0     250.0    04/01/2017
        2     145.0     150.0     140.0       NaN    04/01/2017
        3     205.0     150.0     140.0     215.0    04/01/2017
        4     150.0     125.0     155.0     175.5    04/01/2017
        5     210.0     200.0     215.0     250.0    04/01/2017
        >>>
    max
    teradataml.dataframe.dataframe.DataFrame.max = max(self, distinct=False)
    DESCRIPTION:
        Returns column-wise maximum value of the dataframe.
        Note:
            Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the maximum value.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with max()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If max() operation fails to
            generate the column-wise maximum value of the dataframe.
            Possible error message:
            Failed to perform 'max'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the max() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'max' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints maximum value of each column(with supported data types).
        >>> df1.max()
          max_employee_no max_first_name max_marks max_dob max_joined_date
        0             112          abcde      None    None        18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df3 = df1.select(['employee_no', 'first_name', 'joined_date'])
        # Prints maximum value of each column(with supported data types).
        >>> df3.max()
          max_employee_no max_first_name max_joined_date
        0             112          abcde        18/12/05
        >>>
        #
        # Using max() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        #
        # Time Series Aggregate Example 1: Executing max() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the maximum values.
        #
        # To use max() as Time Series Aggregate we must run groupby_time() first, followed by max().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.max().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             max_TD_TIMECODE  max_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:10:00.000000            55              100
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:03:25.122200            55               79
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:03:25.122200            55               82
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:52:00.000009            55               56
        >>>
        #
        # Time Series Aggregate Example 2: Executing max() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the maximum value.
        #
        # To use max() as Time Series Aggregate we must run groupby_time() first, followed by max().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.max(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             max_TD_TIMECODE  max_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:10:00.000000            55              100
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:03:25.122200            55               79
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:03:25.122200            55               82
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:52:00.000009            55               56
        >>>
    mdiff
    teradataml.dataframe.dataframe.DataFrame.mdiff = mdiff(self, width, sort_columns, drop_columns=False)
    DESCRIPTION:
        Computes the moving difference for the current row and the preceding
        "width" rows in a partition, by sorting the rows according to
        "sort_columns".
        Note:
            mdiff does not support below type of columns.
                * BLOB
                * BYTE
                * CHAR
                * CLOB
                * DATE
                * PERIOD_DATE
                * PERIOD_TIME
                * PERIOD_TIMESTAMP
                * TIME
                * TIMESTAMP
                * VARBYTE
                * VARCHAR
    PARAMETERS:
        width:
            Required Argument.
            Specifies the width of the partition. "width" must be
            greater than 0 and less than or equal to 4096.
            Types: int
        sort_columns:
            Required Argument.
            Specifies the columns to use for sorting.
            Note:
                "sort_columns" does not support CLOB and BLOB type of
                columns.
            Types: str (or) ColumnExpression (or) List of strings(str)
                   or ColumnExpressions
        drop_columns:
            Optional Argument.
            Specifies whether to retain all the input DataFrame columns
            in the output or not. When set to False, columns from input
            DataFrame are retained, dropped otherwise.
            Default Value: False
            Types: bool
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        teradataml DataFrame.
    EXAMPLES:
        # Load the data to run the example.
        >>> from teradataml import load_example_data
        >>> load_example_data("dataframe","sales")
        # Create teradataml dataframe.
        >>> df = DataFrame.from_table('sales')
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        >>>
        # Sorts the Data on column accounts in ascending order and
        # calculates moving difference on the window of size 2.
        >>> df.mdiff(width=2, sort_columns=df.accounts)
                      Feb    Jan    Mar    Apr    datetime  mdiff_Feb  mdiff_Jan  mdiff_Mar  mdiff_Apr  mdiff_datetime
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017      -10.0      -50.0      -75.0      -70.0             0.0
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017        0.0        0.0        0.0     -180.0             0.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     -120.0        NaN        NaN     -250.0             0.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017      120.0      -50.0      -95.0      149.0             0.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017        NaN        NaN        NaN        NaN             NaN
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017        NaN        NaN        NaN        NaN             NaN
        >>>
        # Sorts the Data on column accounts in ascending order and column
        # Feb in descending order, then calculates moving difference by
        # dropping the input DataFrame columns on the window of size 2.
        >>> df.mdiff(width=2, sort_columns=[df.accounts, df.Feb.desc()], drop_columns=True)
           mdiff_Feb  mdiff_Jan  mdiff_Mar  mdiff_Apr
        0      -10.0      -50.0      -75.0      -70.0
        1        0.0        0.0        0.0     -180.0
        2     -120.0        NaN        NaN     -250.0
        3      120.0      -50.0      -95.0      149.0
        4        NaN        NaN        NaN        NaN
        5        NaN        NaN        NaN        NaN
        >>>
    mean
    teradataml.dataframe.dataframe.DataFrame.mean = mean(self, distinct=False)
    DESCRIPTION:
        Returns column-wise mean value of the dataframe.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the mean.
            Default Values: False
    RETURNS:
        teradataml DataFrame object with mean()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If mean() operation fails to
            generate the column-wise mean value of the dataframe.
            Possible error message:
            Failed to perform 'mean'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the mean() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'mean' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df2 = df1.select(['employee_no', 'marks', 'first_name'])
        # Prints mean value of each column(with supported data types).
        >>> df2.mean()
           mean_employee_no mean_marks
        0        104.333333       None
        >>>
        #
        # Using mean() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        #
        # Time Series Aggregate Example 1: Executing mean() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the mean values.
        #
        # To use mean() as Time Series Aggregate we must run groupby_time() first, followed by mean().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.mean().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  mean_salinity  mean_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0           55.0         54.750000
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1           55.0         74.500000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2           55.0         81.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44           55.0         48.076923
        >>>
        #
        # Time Series Aggregate Example 2: Executing mean() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the mean value.
        #
        # To use mean() as Time Series Aggregate we must run groupby_time() first, followed by mean().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.mean(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  mean_salinity  mean_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0           55.0         69.666667
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1           55.0         74.500000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2           55.0         81.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44           55.0         52.200000
        >>>
    median
    teradataml.dataframe.dataframe.DataFrame.median = median(self, distinct=False)
    DESCRIPTION:
        Returns column-wise median value of the dataframe.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the median.
            Note:
                This is allowed only when median() is used as Time Series Aggregate function, i.e.,
                this can be set to True, only when median() is operated on DataFrameGroupByTime object.
                Otherwise, an exception will be raised.
            Default Values: False
    RETURNS:
        teradataml DataFrame object with median() operation
        performed.
    RAISES:
        1. TDMLDF_AGGREGATE_FAILED - If median() operation fails to
            generate the column-wise median value of the dataframe.
            Possible error message:
            Unable to perform 'median()' on the dataframe.
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the median() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'median' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints median value of each column(with supported data types).
        >>> df1.median()
          median_employee_no median_marks
        0                101         None
        >>>
        #
        # Using median() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing median() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the median value.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        # To use median() as Time Series Aggregate we must run groupby_time() first, followed by median().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.median().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  median_temperature  median_salin
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                54.5             55.0
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                74.5             55.0
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                81.0             55.0
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                43.0             55.0
        >>>
        #
        # Time Series Aggregate Example 2: Executing median() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT rows for the
        #                                  columns while calculating the median value.
        #
        # To use median() as Time Series Aggregate we must run groupby_time() first, followed by median().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.median(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  median_temperature  median_salin
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                99.0             55.0
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                74.5             55.0
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                81.0             55.0
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                54.0             55.0
        >>>
    min
    teradataml.dataframe.dataframe.DataFrame.min = min(self, distinct=False)
    DESCRIPTION:
        Returns column-wise minimum value of the dataframe.
        Note:
            Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the minimum value.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with min()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If min() operation fails to
            generate the column-wise minimum value of the dataframe.
            Possible error message:
            Failed to perform 'min'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the min() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'min' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints minimum value of each column(with supported data types).
        >>> df1.min()
          min_employee_no min_first_name min_marks min_dob min_joined_date
        0             100           abcd      None    None        02/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        # Prints minimum value of each column(with supported data types).
        >>> df3 = df1.select(['employee_no', 'first_name', 'joined_date'])
        >>> df3.min()
          min_employee_no min_first_name min_joined_date
        0             100           abcd        02/12/05
        >>>
        #
        # Using min() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        #
        # Time Series Aggregate Example 1: Executing min() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the minimum values.
        #
        # To use min() as Time Series Aggregate we must run groupby_time() first, followed by min().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.min().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             min_TD_TIMECODE  min_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:00:00.000000            55               10
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:01:25.122200            55               70
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:01:25.122200            55               80
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:00:24.000000            55               43
        >>>
        #
        # Time Series Aggregate Example 2: Executing min() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the minimum value.
        #
        # To use min() as Time Series Aggregate we must run groupby_time() first, followed by min().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.min(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             min_TD_TIMECODE  min_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:00:00.000000            55               10
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:01:25.122200            55               70
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:01:25.122200            55               80
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:00:24.000000            55               43
        >>>
    mlinreg
    teradataml.dataframe.dataframe.DataFrame.mlinreg = mlinreg(self, width, sort_column, drop_columns=False)
    DESCRIPTION:
        Computes the moving linear regression for the current row and the
        preceding "width"-1 rows in a partition, by sorting the rows
        according to "sort_columns".
        Note:
            mlinreg does not support below type of columns.
                * BLOB
                * BYTE
                * CHAR
                * CLOB
                * DATE
                * PERIOD_DATE
                * PERIOD_TIME
                * PERIOD_TIMESTAMP
                * TIME
                * TIMESTAMP
                * VARBYTE
                * VARCHAR
    PARAMETERS:
        width:
            Required Argument.
            Specifies the width of the partition. "width" must be
            greater than 2 and less than or equal to 4096.
            Types: int
        sort_column:
            Required Argument.
            Specifies the column to use for sorting.
            Note:
                "sort_column" does not support CLOB and BLOB type of
                columns.
            Types: str (or) ColumnExpression
        drop_columns:
            Optional Argument.
            Specifies whether to retain all the input DataFrame columns
            in the output or not. When set to False, columns from input
            DataFrame are retained, dropped otherwise.
            Default Value: False
            Types: bool
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        teradataml DataFrame.
    EXAMPLES:
        # Load the data to run the example.
        >>> from teradataml import load_example_data
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame('admissions_train')
        >>> print(df)
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
        >>>
        # Sorts the Data on column id in descending order and
        # calculates moving linear regression on the window of size 3.
        >>> df.mlinreg(width=3, sort_column=df.id.desc())
           masters   gpa     stats programming  admitted  mlinreg_id  mlinreg_gpa  mlinreg_admitted
        id
        6      yes  3.50  Beginner    Advanced         1         6.0         1.06               1.0
        17      no  3.83  Advanced    Advanced         1        17.0         5.64               2.0
        16      no  3.70  Advanced    Advanced         1        16.0         3.85               1.0
        28      no  3.93  Advanced    Advanced         1        28.0         4.21               0.0
        26     yes  3.57  Advanced    Advanced         1        26.0         3.99              -1.0
        40     yes  3.95    Novice    Beginner         0         NaN          NaN               NaN
        39     yes  3.75  Advanced    Beginner         0         NaN          NaN               NaN
        38     yes  2.65  Advanced    Beginner         1        38.0         3.55               0.0
        27     yes  3.96  Advanced    Advanced         0        27.0         3.86               2.0
        18     yes  3.81  Advanced    Advanced         1        18.0         0.06              -1.0
        >>>
        # Sorts the Data on column id in ascending order and
        # calculates moving linear regression by dropping the
        # input DataFrame columns on the window of size 3.
        >>> df.mlinreg(width=3, sort_column=df.id.asc(), drop_columns=True)
           mlinreg_id  mlinreg_gpa  mlinreg_admitted
        0        35.0         4.15              -1.0
        1        24.0         3.72               2.0
        2        25.0         0.15               1.0
        3        13.0         4.17               1.0
        4        15.0         2.90              -1.0
        5         NaN          NaN               NaN
        6         NaN          NaN               NaN
        7         3.0         3.57               0.0
        8        14.0         4.35               1.0
        9        23.0         3.05              -1.0
        >>>
    msum
    teradataml.dataframe.dataframe.DataFrame.msum = msum(self, width, sort_columns, drop_columns=False)
    DESCRIPTION:
        Computes the moving sum for the current row and the preceding
        "width"-1 rows in a partition, by sorting the rows according to
        "sort_columns".
        Note:
            msum does not support below type of columns.
                * BLOB
                * BYTE
                * CHAR
                * CLOB
                * DATE
                * PERIOD_DATE
                * PERIOD_TIME
                * PERIOD_TIMESTAMP
                * TIME
                * TIMESTAMP
                * VARBYTE
                * VARCHAR            
    PARAMETERS:
        width:
            Required Argument.
            Specifies the width of the partition. "width" must be
            greater than 0 and less than or equal to 4096.
            Types: int
        sort_columns:
            Required Argument.
            Specifies the columns to use for sorting.
            Note:
                "sort_columns" does not support CLOB and BLOB type of
                columns.
            Types: str (or) ColumnExpression (or) List of strings(str)
                   or ColumnExpressions
        drop_columns:
            Optional Argument.
            Specifies whether to retain all the input DataFrame columns
            in the output or not. When set to False, columns from input
            DataFrame are retained, dropped otherwise.
            Default Value: False
            Types: bool
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        teradataml DataFrame.
    EXAMPLES:
        # Load the data to run the example.
        >>> from teradataml import load_example_data
        >>> load_example_data("dataframe","sales")
        # Create teradataml dataframe.
        >>> df = DataFrame.from_table('sales')
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        >>>
        # Sorts the Data on column accounts in ascending order and
        # calculates moving sum on the window of size 2.
        >>> df.msum(width=2, sort_columns=df.accounts)
                      Feb    Jan    Mar    Apr    datetime  msum_Feb  msum_Jan  msum_Mar  msum_Apr
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     290.0       200       235       281
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     410.0       150       140       250
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     290.0       150       140         0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     410.0       150       140       430
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     300.0       250       310       351
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0       200       215       250
        >>>
        # Sorts the Data on column accounts in ascending order and column
        # Feb in descending order, then calculates moving sum by dropping the
        # input DataFrame columns on the window of size 2.
        >>> df.msum(width=2, sort_columns=[df.accounts, df.Feb.desc()], drop_columns=True)
           msum_Feb  msum_Jan  msum_Mar  msum_Apr
        0     290.0       200       235       281
        1     410.0       150       140       250
        2     290.0       150       140         0
        3     410.0       150       140       430
        4     300.0       250       310       351
        5     210.0       200       215       250
        >>>
    percentile
    teradataml.dataframe.dataframe.DataFrame.percentile = percentile(percentile, interpolation='LINEAR')
    DESCRIPTION:
        Function returns the value which represents the desired percentile from each group.
        The result value is determined by the desired index (di) in an ordered list of values.
        The following equation is for the di:
            di = (number of values in group - 1) * percentile/100
        When di is a whole number, that value is the returned result.
        The di can also be between two data points, i and j, where i<j. In that case, the result
        is interpolated according to the value specified in interpolation argument.
        Notes:
             1. This function is valid only on columns with numeric types.
             2. Null values are not included in the result computation.
             3. This function works with only DataFrame.groupby().
    PARAMETERS:
        percentile:
            Required Argument.
            Specifies the desired percentile value to calculate.
            It should be between 0 and 1, both inclusive.
            Types: int or float
        interpolation:
            Optional Argument.
            Specifies the interpolation type to use to interpolate the result value when the
            desired result lies between two data points.
            The desired result lies between two data points, i and j, where i<j. In this case,
            the result is interpolated according to the permitted values.
            Permitted Values: "LINEAR", None
                * LINEAR: Linear interpolation.
                    The result value is computed using the following equation:
                        result = i + (j - i) * (di/100)MOD 1
                    Specify by passing "LINEAR" as string to this parameter.
                * None: No interpolation.
                    The result value is computed simply by returning a value from the set
                    of values.
                    Specify by passing None to this parameter.
            Default Values: "LINEAR"
            Types: str, NoneType
    RETURNS:
        teradataml DataFrame.
    RAISES:
        TypeError - If incorrect type of values passed to input argument.
        ValueError - If invalid value passed to the the argument.
        TeradataMLException - TDMLDF_AGGREGATE_FAILED - If percentile() operation fails to
                              generate the column-wise percentile values in the columns.
    EXAMPLES:
        >>> # Load the example datasets.
        ... load_example_data("dataframe", "admissions_train")
        >>>
        # Example 1: Executing percentile() function on DataFrame created on a regular table.
        #            Calculate the 25th percentile value for all numeric columns using default
        #            values, i.e., consider all rows (duplicate rows as well) and linear
        #            interpolation while computing the percentile value.
        #
        >>> # Create the required DataFrame.
        ... admissions_train = DataFrame("admissions_train")
        >>> # Check DataFrame columns and let's peek at the data.
        ... admissions_train.columns
        ['id', 'masters', 'gpa', 'stats', 'programming', 'admitted']
        >>> admissions_train.head()
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        >>> df = admissions_train.groupby("admitted").percentile(0.25)
        >>> df.show_query()
        'select "admitted", percentile_cont(0.25) WITHIN GROUP (ORDER BY id) AS percentile_id, percentile_cont(0.25) WITHIN GROUP (
        >>> df
           admitted  percentile_id  percentile_gpa
        0         0             15          3.4525
        1         1             10          3.5050
        >>>
        #
        # Example 2: Executing percentile() function on DataFrame created on a regular table.
        #            Calculate the 35th percentile value for all numeric columns using default
        #            values, i.e., consider all rows (duplicate rows as well) and no
        #            interpolation while computing the percentile value.
        #
        >>> # Create the required DataFrame.
        ... admissions_train = DataFrame("admissions_train")
        >>> # Check DataFrame columns and let's peek at the data.
        ... admissions_train.columns
        ['id', 'masters', 'gpa', 'stats', 'programming', 'admitted']
        >>> admissions_train.head()
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        >>> df = admissions_train.groupby("admitted").percentile(0.25, interpolation=None)
        'select "admitted", percentile_disc(0.25) WITHIN GROUP (ORDER BY id) AS percentile_id, percentile_disc(0.25) WITHIN GROUP (
        >>> df.show_query()
        >>> df
           admitted  percentile_id  percentile_gpa
        0         0             19            3.46
        1         1             13            3.57
        >>>
    regr_avgx
    teradataml.dataframe.dataframe.DataFrame.regr_avgx = regr_avgx(expression)
    DESCRIPTION:
        Function returns the column-wise mean of the independent variable for all
        non-null data pairs of the dependent and an independent variable arguments.
        The function considers all the valid columns in teradataml DataFrame as
        dependent variable and "expression" as an independent variable.
        Note:
            When there are fewer than two non-null data point pairs in the
            data used for the computation, the function returns None.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under
            your control to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the mean of the column 'gpa' for all non-null
        #          data pairs with dependent variable as all other valid columns.
        >>> df = admissions_train.regr_avgx(admissions_train.gpa)
        >>> df
           regr_avgx_id  regr_avgx_gpa  regr_avgx_admitted
        0       3.54175        3.54175             3.54175
        >>>
        # Example 2: Calculate the mean of the column 'gpa' for all non-null
        #            data pairs with independent variable as all other valid columns,
        #            for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").regr_avgx(admissions_train.gpa)
        >>> df
          programming  regr_avgx_id  regr_avgx_gpa  regr_avgx_admitted
        0    Advanced      3.615625       3.615625            3.615625
        1      Novice      3.294545       3.294545            3.294545
        2    Beginner      3.660000       3.660000            3.660000
        >>>
    regr_avgy
    teradataml.dataframe.dataframe.DataFrame.regr_avgy = regr_avgy(expression)
    DESCRIPTION:
        Function returns the column-wise mean of the dependent variable for all
        non-null data pairs of the dependent and independent variable arguments.
        The function considers all the valid columns in teradataml DataFrame as
        dependent variable and "expression" as a independent variable.
        Note:
            When there are fewer than two non-null data point pairs in the data used
            for the computation, the function returns None.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the mean of the "gpa" column for all non-null
        #            data pairs with dependent variable as all other valid columns.
        >>> df = admissions_train.regr_avgy(admissions_train.gpa)
        >>> df
           regr_avgy_id  regr_avgy_gpa  regr_avgy_admitted
        0          20.5        3.54175                0.65
        >>>
        # Example 2: Calculate the mean of the "gpa" column for all non-null
        #            data pairs with dependent variable as all other valid columns,
        #            for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").regr_avgy(admissions_train.gpa)
        >>> df
          programming  regr_avgy_id  regr_avgy_gpa  regr_avgy_admitted
        0    Advanced     16.812500       3.615625            0.812500
        1      Novice     20.363636       3.294545            0.727273
        2    Beginner     25.153846       3.660000            0.384615
        >>>
    regr_count
    teradataml.dataframe.dataframe.DataFrame.regr_count = regr_count(expression)
    DESCRIPTION:
        Function returns the column-wise count of all non-null data pairs of the
        dependent and independent variable arguments. The function considers all
        the valid columns in teradataml DataFrame as dependent variable and
        "expression" as an independent variable.
        Note:
            When there are fewer than two non-null data point pairs in the
            data used for the computation, the function returns None.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the count of the column 'gpa' for all non-null
        #            data pairs with dependent variable as all other columns.
        >>> df = admissions_train.regr_count(admissions_train.gpa)
        >>> df
           regr_count_id  regr_count_gpa  regr_count_admitted
        0             40              40                   40
        >>>
        # Example 2: Calculate the count of the column 'gpa' for all non-null
        #            data pairs with dependent variable as all other columns,
        #            for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").regr_count(admissions_train.gpa)
        >>> df
          programming  regr_count_id  regr_count_gpa  regr_count_admitted
        0    Advanced             16              16                   16
        1      Novice             11              11                   11
        2    Beginner             13              13                   13
        >>>
    regr_intercept
    teradataml.dataframe.dataframe.DataFrame.regr_intercept = regr_intercept(expression)
    DESCRIPTION:
        Function returns the column-wise intercept of the univariate linear regression line
        through all non-null data pairs of the dependent and independent variable
        arguments. The intercept is the point at which the regression line through
        the non-null data pairs in the sample intersects the ordinate, or y-axis,
        of the graph. The plot of the linear regression on the variables is used to
        predict the behavior of the dependent variable from the change in the
        independent variable. There can be a strong nonlinear relationship between
        independent and dependent variables, and the computation of the simple linear
        regression between such variable pairs does not reflect such a relationship.
        The function considers all the valid columns in teradataml DataFrame as
        dependent variable and "expression" as an independent variable.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the intercept of the univariate linear regression
        #            line through all non-null data pairs for all the applicable
        #            columns and independent variable as 'gpa'.
        >>> df = admissions_train.regr_intercept(admissions_train.gpa)
        >>> df
           regr_intercept_id  regr_intercept_gpa  regr_intercept_admitted
        0          21.619895                 0.0                 0.724144
        >>>
        # Example 2: Calculate the intercept of the univariate linear regression
        #            line through all non-null data pairs for all the applicable
        #            columns and independent variable as 'gpa', for each level
        #            of 'programming'
        >>> df = admissions_train.groupby("programming").regr_intercept(admissions_train.gpa)
        >>> df
          programming  regr_intercept_id  regr_intercept_gpa  regr_intercept_admitted
        0    Advanced           8.221993                 0.0                -0.626557
        1      Novice          18.889270                 0.0                 1.000091
        2    Beginner          66.340361                 0.0                 2.566361
        >>>
    regr_r2
    teradataml.dataframe.dataframe.DataFrame.regr_r2 = regr_r2(expression)
    DESCRIPTION:
        Function returns the column-wise coefficient of determination for all
        non-null data pairs of the dependent and independent variable arguments.
        The function considers ColumnExpression as a dependent variable and
        "expression" as an independent variable.
        Note:
            When there are fewer than two non-null data point pairs in the data
            used for the computation, the function returns NULL.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the coefficient of determination of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns.
        >>> df = admissions_train.regr_r2(admissions_train.gpa)
        >>> df
           regr_r2_id  regr_r2_gpa  regr_r2_admitted
        0    0.000193          1.0          0.000496
        >>>
        # Example 2: Calculate the coefficient of determination of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns, for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").regr_r2(admissions_train.gpa)
        >>> df
          programming  regr_r2_id  regr_r2_gpa  regr_r2_admitted
        0    Advanced    0.027757          1.0          0.237888
        1      Novice    0.000523          1.0          0.013146
        2    Beginner    0.076917          1.0          0.174361
        >>>
    regr_slope
    teradataml.dataframe.dataframe.DataFrame.regr_slope = regr_slope(expression)
    DESCRIPTION:
        Function returns the column-wise coefficient slope of the univariate linear
        regression line through all non-null data pairs of the dependent and an independent
        variable arguments. The function considers all the valid columns in teradataml
        DataFrame as dependent variable and "expression" as an independent variable.
        Note:
            When there are fewer than two non-null data point pairs in the
            data used for the computation, the function returns None.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the coefficient slope of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns.
        >>> df = admissions_train.regr_slope(admissions_train.gpa)
        >>> df
           regr_slope_id  regr_slope_gpa  regr_slope_admitted
        0      -0.316198             1.0            -0.020934
        >>>
        # Example 2: Calculate the coefficient slope of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns, for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").regr_slope(admissions_train.gpa)
        >>> df
          programming  regr_slope_id  regr_slope_gpa  regr_slope_admitted
        0    Advanced       2.375940             1.0             0.398010
        1      Novice       0.447517             1.0            -0.082809
        2    Beginner     -11.253146             1.0            -0.596105
        >>>
    regr_sxx
    teradataml.dataframe.dataframe.DataFrame.regr_slope = regr_slope(expression)
    DESCRIPTION:
        Function returns the column-wise coefficient slope of the univariate linear
        regression line through all non-null data pairs of the dependent and an independent
        variable arguments. The function considers all the valid columns in teradataml
        DataFrame as dependent variable and "expression" as an independent variable.
        Note:
            When there are fewer than two non-null data point pairs in the
            data used for the computation, the function returns None.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the coefficient slope of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns.
        >>> df = admissions_train.regr_slope(admissions_train.gpa)
        >>> df
           regr_slope_id  regr_slope_gpa  regr_slope_admitted
        0      -0.316198             1.0            -0.020934
        >>>
        # Example 2: Calculate the coefficient slope of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns, for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").regr_slope(admissions_train.gpa)
        >>> df
          programming  regr_slope_id  regr_slope_gpa  regr_slope_admitted
        0    Advanced       2.375940             1.0             0.398010
        1      Novice       0.447517             1.0            -0.082809
        2    Beginner     -11.253146             1.0            -0.596105
        >>>
    regr_sxy
    teradataml.dataframe.dataframe.DataFrame.regr_sxy = regr_sxy(expression)
    DESCRIPTION:
        Function returns the column-wise sum of the products of the independent variable
        and the dependent variable for all non‑null data pairs of the dependent and
        independent variable arguments. The function considers all the valid columns
        in teradataml DataFrame as dependent variable and "expression" as an independent
        variable.
        Note:
            When there are fewer than two non-null data point pairs in the
            data used for the computation, the function returns None.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the sum of the products of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns.
        >>> df = admissions_train.regr_sxy(admissions_train.gpa)
        >>> df
           regr_sxy_id  regr_sxy_gpa  regr_sxy_admitted
        0       -3.255     10.294177            -0.2155
        >>>
        # Example 2: Calculate the sum of the products of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns, for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").regr_sxy(admissions_train.gpa)
        >>> df
          programming  regr_sxy_id  regr_sxy_gpa  regr_sxy_admitted
        0    Advanced     8.696875      3.660394           1.456875
        1      Novice     1.871818      4.182673          -0.346364
        2    Beginner   -16.990000      1.509800          -0.900000
        >>>
    regr_syy
    teradataml.dataframe.dataframe.DataFrame.regr_syy = regr_syy(expression)
    DESCRIPTION:
        Function returns the column-wise sum of the squares of the dependent variable
        expression for all non-null data pairs of dependent and an independent
        variable arguments. The function considers all the valid columns in teradataml
        DataFrame as dependent variable and "expression" as an independent variable.
        Note:
            When there are fewer than two non-null data point pairs in the
            data used for the computation, the function returns None.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under
            your control to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        teradataml DataFrame
    RAISES:
        RuntimeError - If none of the columns support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Calculate the sum of the squares of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns.
        >>> df = admissions_train.regr_syy(admissions_train.gpa)
        >>> df
           regr_syy_id  regr_syy_gpa  regr_syy_admitted
        0       5330.0     10.294177                9.1
        >>>
        # Example 2: Calculate the sum of the squares of the column 'gpa'
        #            for all non-null data pairs with dependent variable as all other
        #            valid columns, for each level of 'programming'.
        >>> df = admissions_train.groupby("programming").regr_syy(admissions_train.gpa)
        >>> df
          programming  regr_syy_id  regr_syy_gpa  regr_syy_admitted
        0    Advanced   744.437500      3.660394           2.437500
        1      Novice  1600.545455      4.182673           2.181818
        2    Beginner  2485.692308      1.509800           3.076923
        >>>
    skew
    teradataml.dataframe.dataframe.DataFrame.skew = skew(self, distinct=False)
    DESCRIPTION:
        Returns column-wise skewness of the distribution of the dataframe.
        Skewness is the third moment of a distribution. It is a measure of the asymmetry of the
        distribution about its mean compared with the normal (or Gaussian) distribution.
            * The normal distribution has a skewness of 0.
            * Positive skewness indicates a distribution having an asymmetric tail
              extending toward more positive values.
            * Negative skewness indicates an asymmetric tail extending toward more negative values.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Nulls are not included in the result computation.
            3. Following conditions will produce null result:
                a. Fewer than three non-null data points in the data used for the computation.
                b. Standard deviation for a column is equal to 0.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the skewness of the distribution.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with skew()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If the skew() operation fails to
            generate the column-wise skew value of the dataframe.
            Possible error message:
            Failed to perform 'skew'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the skew() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'skew' operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["admissions_train"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("admissions_train")
        >>> print(df1.sort('id'))
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        2      yes  3.76  Beginner    Beginner         0
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        8       no  3.60  Beginner    Advanced         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        >>>
        # Prints skew value of each column(with supported data types).
        >>> df1.skew()
           skew_id  skew_gpa  skew_admitted
        0      0.0 -2.058969      -0.653746
        >>>
        #
        # Using skew() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        #
        # Time Series Aggregate Example 1: Executing skew() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the skew values.
        #
        # To use skew() as Time Series Aggregate we must run groupby_time() first, followed by skew().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.skew().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid skew_salinity  skew_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0          None          0.000324
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1          None          0.000000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2          None          0.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44          None          0.246084
        >>>
        #
        # Time Series Aggregate Example 2: Executing skew() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the skew value.
        #
        # To use skew() as Time Series Aggregate we must run groupby_time() first, followed by skew().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.skew(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid skew_salinity  skew_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0          None         -1.731321
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1          None          0.000000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2          None          0.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44          None         -1.987828
        >>>
    std
    teradataml.dataframe.dataframe.DataFrame.std = std(self, distinct=False, population=False)
    DESCRIPTION:
        Returns column-wise sample or population standard deviation value of the
        dataframe. The standard deviation is the second moment of a distribution.
            * For a sample, it is a measure of dispersion from the mean of that sample.
            * For a population, it is a measure of dispersion from the mean of that population.
        The computation is more conservative for the population standard deviation
        to minimize the effect of outliers on the computed value.
        Note:
            1. When there are fewer than two non-null data points in the sample used
               for the computation, then std returns None.
            2. Null values are not included in the result computation.
            3. If data represents only a sample of the entire population for the
               columns, Teradata recommends to calculate sample standard deviation,
               otherwise calculate population standard deviation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating
            the standard deviation.
            Default Value: False
            Types: bool
        population:
            Optional Argument.
            Specifies whether to calculate standard deviation on entire population or not.
            Set this argument to True only when the data points represent the complete
            population. If your data represents only a sample of the entire population for the
            columns, then set this variable to False, which will compute the sample standard
            deviation. As the sample size increases, even though the values for sample
            standard deviation and population standard deviation approach the same number,
            you should always use the more conservative sample standard deviation calculation,
            unless you are absolutely certain that your data constitutes the entire population
            for the columns.
            Default Value: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with std() operation performed.
    RAISES:
        1. EXECUTION_FAILED - If std() operation fails to
            generate the column-wise standard deviation of the
            dataframe.
            Possible error message:
            Failed to perform 'std'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the std() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'std' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df2 = df1.select(['employee_no', 'first_name', 'marks', 'joined_date'])
        # Prints sample standard deviation of each column(with supported data types).
        >>> df2.std()
           std_employee_no std_marks std_joined_date
        0         6.658328      None        82/03/09
        >>>
        # Prints population standard deviation of each column(with supported data types).
        >>> df2.std(population=True)
           std_employee_no std_marks std_joined_date
        0         5.436502      None        58/02/28
        >>>
        #
        # Using std() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing std() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the standard deviation.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        # To use std() as Time Series Aggregate we must run groupby_time() first, followed by std().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.std().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  std_salinity  std_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           0.0        51.674462
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           0.0         3.937004
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           0.0         1.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           0.0         5.765725
        >>>
        #
        # Time Series Aggregate Example 2: Executing std() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT rows for the
        #                                  columns while calculating the standard deviation.
        #
        # To use std() as Time Series Aggregate we must run groupby_time() first, followed by std().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.std(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid std_salinity  std_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0         None        51.675268
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1         None         3.937004
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2         None         1.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44         None         5.263079
        >>>
        #
        # Time Series Aggregate Example 3: Executing std() function on DataFrame created on
        #                                  non-sequenced PTI table. We shall calculate the
        #                                  standard deviation on entire population, with
        #                                  all non-null data points considered for calculations.
        #
        # To use std() as Time Series Aggregate we must run groupby_time() first, followed by std().
        # To calculate population standard deviation we must set population=True.
        #
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.std(population=True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  std_salinity  std_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           0.0        44.751397
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           0.0         3.593976
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           0.0         0.816497
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           0.0         5.539530
        >>>
    sum
    teradataml.dataframe.dataframe.DataFrame.sum = sum(self, distinct=False)
    DESCRIPTION:
        Returns column-wise sum value of the dataframe.
        Notes:
            1. teradataml doesn't support sum operation on columns of str, datetime types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the sum.
            Default Value: False 
            Types: bool
    RETURNS:
        teradataml DataFrame object with sum()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If sum() operation fails to
            generate the column-wise summation value of the dataframe.
            Possible error message:
            Failed to perform 'sum'. (Followed by error message).
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the sum() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'sum' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints sum of the values of each column(with supported data types).
        >>> df1.sum()
          sum_employee_no sum_marks
        0             313      None
        >>>
        #
        # Using sum() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing sum() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the sum value.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        # To use sum() as Time Series Aggregate we must run groupby_time() first, followed by sum().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.sum().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  sum_salinity  sum_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           275              219
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           330              447
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           165              243
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           715              625
        >>>
        #
        # Time Series Aggregate Example 2: Executing sum() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the sum value.
        #
        # To use sum() as Time Series Aggregate we must run groupby_time() first, followed by sum().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.sum(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  sum_salinity  sum_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0            55              209
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1            55              447
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2            55              243
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44            55              261
        >>>
    var
    teradataml.dataframe.dataframe.DataFrame.var = var(self, distinct=False, population=False)
    DESCRIPTION:
        Returns column-wise sample or population variance of the columns in a
        dataframe.
            * The variance of a population is a measure of dispersion from the
              mean of that population.
            * The variance of a sample is a measure of dispersion from the mean
              of that sample. It is the square of the sample standard deviation.
        Note:
            1. When there are fewer than two non-null data points in the sample used
               for the computation, then var returns None.
            2. Null values are not included in the result computation.
            3. If data represents only a sample of the entire population for the
               columns, Teradata recommends to calculate sample variance,
               otherwise calculate population variance.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate column values while calculating the
            variance value.
            Default Values: False
            Types: bool
        population:
            Optional Argument.
            Specifies whether to calculate variance on entire population or not.
            Set this argument to True only when the data points represent the complete
            population. If your data represents only a sample of the entire population
            for the columns, then set this variable to False, which will compute the
            sample variance. As the sample size increases, even though the values for
            sample variance and population variance approach the same number, but you
            should always use the more conservative sample standard deviation calculation,
            unless you are absolutely certain that your data constitutes the entire
            population for the columns.
            Default Value: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with var() operation performed.
    RAISES:
        1. TDMLDF_AGGREGATE_FAILED - If var() operation fails to
            generate the column-wise variance of the dataframe.
            Possible error message:
            Unable to perform 'var()' on the dataframe.
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the var() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'var' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info", "sales"])
        # Example 1 - Applying var on table 'employee_info' that has all
        #             NULL values in marks and dob columns which are
        #             captured as None in variance dataframe.
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df3 = df1.select(["employee_no", "first_name", "dob", "marks"])
        # Prints unbiased variance of each column(with supported data types).
        >>> df3.var()
               var_employee_no var_dob var_marks
            0        44.333333    None      None
        # Example 2 - Applying var on table 'sales' that has different
        #             types of data like floats, integers, strings
        #             some of which having NULL values which are ignored.
        # Create teradataml dataframe.
        >>> df1 = DataFrame("sales")
        >>> print(df1)
                          Feb   Jan   Mar   Apr    datetime
        accounts
        Blue Inc     90.0    50    95   101  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Jones LLC   200.0   150   140   180  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        # Prints unbiased sample variance of each column(with supported data types).
        >>> df3 = df1.select(["accounts","Feb","Jan","Mar","Apr"])
        >>> df3.var()
               var_Feb      var_Jan  var_Mar      var_Apr
        0  3546.666667  3958.333333   2475.0  5036.916667
        >>>
        # Prints population variance of each column(with supported data types).
        >>> df3.var(population=True)
               var_Feb  var_Jan  var_Mar    var_Apr
        0  2955.555556  2968.75  1856.25  3777.6875
        >>>
        #
        # Using var() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing var() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the variance value.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
        >>> ocean_buoys.head()
                               TD_TIMECODE  temperature  salinity
        buoyid
        0       2014-01-06 08:10:00.000000        100.0        55
        0       2014-01-06 08:08:59.999999          NaN        55
        1       2014-01-06 09:01:25.122200         77.0        55
        1       2014-01-06 09:03:25.122200         79.0        55
        1       2014-01-06 09:01:25.122200         70.0        55
        1       2014-01-06 09:02:25.122200         71.0        55
        1       2014-01-06 09:03:25.122200         72.0        55
        0       2014-01-06 08:09:59.999999         99.0        55
        0       2014-01-06 08:00:00.000000         10.0        55
        0       2014-01-06 08:10:00.000000         10.0        55
        # To use var() as Time Series Aggregate we must run groupby_time() first, followed by var().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.var().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  var_salinity  var_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           0.0       2670.25000
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           0.0         15.50000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           0.0          1.00000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           0.0         33.24359
        >>>
        #
        # Time Series Aggregate Example 2: Executing var() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT rows for the
        #                                  columns while calculating the variance value.
        #
        # To use var() as Time Series Aggregate we must run groupby_time() first, followed by var().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.var(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid var_salinity  var_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0         None      2670.333333
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1         None        15.500000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2         None         1.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44         None        27.700000
        >>>
        #
        # Time Series Aggregate Example 3: Executing var() function on DataFrame created on
        #                                  non-sequenced PTI table. We shall calculate the
        #                                  variance on entire population, with all non-null
        #                                  data points considered for calculations.
        #
        # To use var() as Time Series Aggregate we must run groupby_time() first, followed by var().
        # To calculate population variance we must set population=True.
        #
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.var(population=True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  var_salinity  var_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           0.0      2002.687500
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           0.0        12.916667
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           0.0         0.666667
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           0.0        30.686391
        >>>
    teradataml: DataFrame Functions
    call_udf
    teradataml.dataframe.functions.call_udf = call_udf(udf_name, func_args=(), **kwargs)
    DESCRIPTION:
        Call a registered user defined function (UDF).
    PARAMETERS:
        udf_name:
            Required Argument.
            Specifies the name of the registered user defined.
            Types: str
        func_args:
            Optional Argument.
            Specifies the arguments to pass to the registered UDF.
            Default Value: ()
            Types: tuple
        delimiter:
            Optional Argument.
            Specifies a delimiter to use when reading columns from a row and
            writing result columns.
            Notes:
                * This argument cannot be same as "quotechar" argument.
                * This argument cannot be a newline character.
                * Use a different delimiter if categorial columns in the data contains
                  a character same as the delimiter.
            Default Value: ','
            Types: one character string  
        quotechar:
            Optional Argument.
            Specifies a character that forces input of the user function
            to be quoted using this specified character.
            Using this argument enables the Analytics Database to
            distinguish between NULL fields and empty strings.
            A string with length zero is quoted, while NULL fields are not.
            Notes:
                * This argument cannot be same as "delimiter" argument.
                * This argument cannot be a newline character.
            Default Value: None
            Types: one character string
    RETURNS:
        ColumnExpression
    RAISES:
        TeradataMLException
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "sales")
        # Create a DataFrame on 'sales' table.
        >>> import random
        >>> dfsales = DataFrame("sales")
        >>> df = dfsales.assign(id = case([(df.accounts == 'Alpha Co', random.randrange(1, 9)),
        ...                           (df.accounts == 'Blue Inc', random.randrange(1, 9)),
        ...                           (df.accounts == 'Jones LLC', random.randrange(1, 9)),
        ...                           (df.accounts == 'Orange Inc', random.randrange(1, 9)),
        ...                           (df.accounts == 'Yellow Inc', random.randrange(1, 9)),
        ...                           (df.accounts == 'Red Inc', random.randrange(1, 9))]))
        # Example 1: Register and Call the user defined function to get the values upper case.
        >>> from teradataml.dataframe.functions import udf, register, call_udf
        >>> @udf
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        # Register the created user defined function with name "upper".
        >>> register("upper", to_upper)
        >>>
        # Call the user defined function registered with name "upper" and assign the 
        # ColumnExpression returned to the DataFrame.
        >>> res = df.assign(upper_col = call_udf("upper", ('accounts',)))
        >>> res
                      Feb    Jan    Mar    Apr  datetime  id   upper_col
        accounts                                                        
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04   4  YELLOW INC
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04   2    ALPHA CO
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   5   JONES LLC
        Red Inc     200.0  150.0  140.0    NaN  17/01/04   3     RED INC
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04   1    BLUE INC
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04   4  ORANGE INC
        >>>
        # Example 2: Register and Call user defined function to get factorial of a number  
        #            and store the result in Integer type column.
        >>> from teradataml.dataframe.functions import udf, register
        >>> @udf(returns = INTEGER())
        ... def factorial(n):
        ...    import math
        ...    return math.factorial(n)
        >>>
        # Register the created user defined function with name "fact".
        >>> from teradatasqlalchemy.types import INTEGER
        >>> register("fact", factorial)
        >>>
        # Call the user defined function registered with name "fact" and assign the
        # ColumnExpression returned to the DataFrame.
        >>> res = df.assign(fact_col = call_udf("fact", ('id',)))
        >>> res
                      Feb    Jan    Mar    Apr  datetime  id  fact_col
        accounts                                                      
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   5       120
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04   4        24
        Red Inc     200.0  150.0  140.0    NaN  17/01/04   3         6
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04   1         1
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04   2         2
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04   4        24
        >>>
        # Example 3: Register and Call the Python function to get the values upper case.
        >>> from teradataml.dataframe.functions import register, call_udf
        >>> def to_upper(s):
        ...     return s.upper()
        >>>
        # Register the created Python function with name "upper".
        >>> register("upper", to_upper, returns = VARCHAR(1024))
        >>>
        # Call the Python function registered with name "upper" and assign the 
        # ColumnExpression returned to the DataFrame.
        >>> res = df.assign(upper_col = call_udf("upper", ('accounts',)))
        >>> res
                      Feb    Jan    Mar    Apr  datetime  id   upper_col
        accounts                                                        
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04   4  YELLOW INC
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04   2    ALPHA CO
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   5   JONES LLC
        Red Inc     200.0  150.0  140.0    NaN  17/01/04   3     RED INC
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04   1    BLUE INC
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04   4  ORANGE INC
        >>>
    deregister
    teradataml.dataframe.functions.deregister = deregister(name, returns=None)
    DESCRIPTION:
        Deregisters a user defined function (UDF).
    PARAMETERS:
        name:
            Required Argument.
            Specifies the name of the user defined function to deregister.
            Types: str
        returns:
            Optional Argument.
            Specifies the type used to deregister the user defined function.
            Types: teradatasqlalchemy types object
    RETURNS:
        None
    RAISES:
        TeradataMLException.
    EXAMPLES:
        # Example 1: Register the user defined function to get the values in lower case,
        #            then deregister it.
        >>> @udf
        ... def to_lower(s):
        ...   if s is not None:
        ...        return s.lower()
        # Register the created user defined function.
        >>> register("lower", to_lower)
        # List all the UDFs registered
        >>> list_udfs(True)
        id      name  return_type                                          file_name
         0     lower  VARCHAR1024  tdml_udf_name_lower_udf_type_VARCHAR1024_register.py
         1     upper  VARCHAR1024  tdml_udf_name_upper_udf_type_VARCHAR1024_register.py
         2  add_date         DATE   tdml_udf_name_add_date_udf_type_DATE_register.py
         3  sum_cols      INTEGER  tdml_udf_name_sum_cols_udf_type_INTEGER_register.py
        >>>
        # Deregister the created user defined function.
        >>> deregister("lower")
        # List all the UDFs registered
        >>> list_udfs(True)
        id      name  return_type                                          file_name
         0     upper  VARCHAR1024  tdml_udf_name_upper_udf_type_VARCHAR1024_register.py
         1  add_date         DATE   tdml_udf_name_add_date_udf_type_DATE_register.py
         2  sum_cols      INTEGER  tdml_udf_name_sum_cols_udf_type_INTEGER_register.py
        >>>
        # Example 2: Deregister only specified udf function with it return type.
        >>> @udf(returns=FLOAT())
        ... def sum(x, y):
        ...    return len(x) + y
        # Deregister the created user defined function.
        >>> register("sum", sum)
        # List all the UDFs registered
        >>> list_udfs(True)
        id name return_type                                       file_name
         0  sum       FLOAT    tdml_udf_name_sum_udf_type_FLOAT_register.py
         1  sum     INTEGER  tdml_udf_name_sum_udf_type_INTEGER_register.py
         >>>
        # Deregister the created user defined function.
        >>> from teradatasqlalchemy import FLOAT
        >>> deregister("sum", FLOAT())
        # List all the UDFs registered
        >>> list_udfs(True)
        id name return_type                                       file_name
         0  sum     INTEGER  tdml_udf_name_sum_udf_type_INTEGER_register.py
         >>>
    get_formatters
    teradataml.dataframe.functions.get_formatters = get_formatters(formatter_type=None)
    DESCRIPTION:
        Function to get the formatters for NUMERIC, DATE and CHAR types.
    PARAMETERS:
        formatter_type:
            Optional Argument.
            Specifies the category of formatter to format data.
            Default Value: None
            Permitted values:
                "NUMERIC" - Formatters to convert given data to a numeric type.
                "DATE" - Formatters to convert given data to a date type.
                "CHAR" - Formatters to convert given data to a character type.
            Types: str
    RAISES:
        ValueError
    RETURNS:    
        None
    EXAMPLES:
        # Example 1: Get the formatters for the NUMERIC type.
        >>> from teradataml.dataframe.functions import get_formatters
        >>> get_formatters("NUMERIC")
    list_udfs
    teradataml.dataframe.functions.list_udfs = list_udfs(show_ﬁles=False)
    DESCRIPTION:
        List all the UDFs registered using 'register()' function.
    PARAMETERS:
        show_files:
            Optional Argument.
            Specifies whether to show file names or not.
            Default Value: False
            Types: bool
    RETURNS:
        Pandas DataFrame containing files and it's details or
        None if DataFrame is empty.
    RAISES:
        TeradataMLException.
    EXAMPLES:
        # Example 1: Register the user defined function to get the values in lower case,
                     then list all the UDFs registered.
        >>> @udf
        ... def to_lower(s):
        ...   if s is not None:
        ...        return s.lower()
        # Register the created user defined function.
        >>> register("lower", to_lower)
        # List all the UDFs registered
        >>> list_udfs(True)
        id      name  return_type                                          file_name
         0     lower  VARCHAR1024  tdml_udf_name_lower_udf_type_VARCHAR1024_register.py
         1     upper  VARCHAR1024  tdml_udf_name_upper_udf_type_VARCHAR1024_register.py
         2  add_date         DATE   tdml_udf_name_add_date_udf_type_DATE_register.py
         3  sum_cols      INTEGER  tdml_udf_name_sum_cols_udf_type_INTEGER_register.py
        >>>
    register
    teradataml.dataframe.functions.register = register(name, user_function, returns=VARCHAR(length=1024))
    DESCRIPTION:
        Registers a user defined function (UDF).
    PARAMETERS:
        name:
            Required Argument.
            Specifies the name of the user defined function to register.
            Types: str
        user_function:
            Required Argument.
            Specifies the user defined function to create a column for
            teradataml DataFrame.
            Types: function, udf
        returns:
            Optional Argument.
            Specifies the output column type used to register the user defined function.
            Note:
                * If 'user_function' is a udf, then return type of the udf is used as return type
                  of the registered user defined function.
            Default Value: VARCHAR(1024)
            Types: teradatasqlalchemy types object
    RETURNS:
        None
    RAISES:
        TeradataMLException, TypeError
    EXAMPLES:
        # Example 1: Register the user defined function to get the values upper case.
        >>> from teradataml.dataframe.functions import udf, register
        >>> @udf
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        # Register the created user defined function.
        >>> register("upper_val", to_upper)
        >>>
        # Example 2: Register a user defined function to get factorial of a number and  
        #            store the result in Integer type column.
        >>> from teradataml.dataframe.functions import udf, register
        >>> from teradatasqlalchemy.types import INTEGER
        >>> @udf 
        ... def factorial(n):
        ...    import math
        ...    return math.factorial(n)
        >>>
        # Register the created user defined function.
        >>> register("fact", factorial, INTEGER())
        >>>
        # Example 3: Register a Python function to get the values upper case.
        >>> from teradataml.dataframe.functions import register
        >>> def to_upper(s):
        ...     return s.upper()       
        >>>
        # Register the created Python function.
        >>> register("upper_val", to_upper)
        >>>
    td_range
    teradataml.dataframe.functions.td_range = td_range(start, end=None, step=1)
    DESCRIPTION:
        Creates a DataFrame with a specified range of numbers.
        Notes: 
            1. The range is inclusive of the start and exclusive of the end.
            2. If only start is provided, then end is set to start and start is set to 0.
    PARAMETERS:
        start:
            Required Argument.
            Specifies the starting number of the range.
            Types: int
        end:
            Optional Argument.
            Specifies the end number of the range(exclusive).
            Default Value: None
            Types: int
        step:
            Optional Argument.
            Specifies the step size of the range.
            Default Value: 1
            Types: int
    RETURNS:
        teradataml DataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
            # Example 1: Create a DataFrame with a range of numbers from 0 to 5.
            >>> from teradataml.dataframe.functions import td_range
            >>> df = td_range(5)
            >>> df.sort('id')
               id
            0   0
            1   1
            2   2
            3   3
            4   4
            # Example 2: Create a DataFrame with a range of numbers from 5 to 1 with step size of -2.
            >>> from teradataml.dataframe.functions import td_range
            >>> td_range(5, 1, -2)
               id
            0   3
            1   5
            >>> Example 3: Create a DataFrame with a range of numbers from 1 to 5 with default step size of 1.
            >>> from teradataml.dataframe.functions import td_range
            >>> td_range(1, 5)
               id
            0   3
            1   4
            2   2
            3   1
    udf
    teradataml.dataframe.functions.udf = udf(user_function=None, returns=VARCHAR(length=1024), env_name=None, delimiter=',', quotechar=None, debug=False)
    DESCRIPTION:
        Creates a user defined function (UDF).
        Notes: 
            1. Date and time data types must be formatted to supported formats.
               (See Prerequisite Input and Output Structures in Open Analytics Framework for more details.)
            2. Packages required to run the user defined function must be installed in remote user 
               environment using install_lib method of UserEnv class. Import statements of these
               packages should be inside the user defined function itself.
            3. Do not call a regular function defined outside the udf() from the user defined function.
               The function definition and call must be inside the udf(). Look at Example 9 to understand more.
    PARAMETERS:
        user_function:
            Required Argument.
            Specifies the user defined function to create a column for
            teradataml DataFrame.
            Types: function
            Note:
                Lambda functions are not supported. Re-write the lambda function as regular Python function to use with UDF.
        returns:
            Optional Argument.
            Specifies the output column type.
            Types: teradatasqlalchemy types object
            Default: VARCHAR(1024)
        env_name:
            Optional Argument.
            Specifies the name of the remote user environment or an object of
            class UserEnv for VantageCloud Lake.
            Types: str or oject of class UserEnv.
            Note:
                * One can set up a user environment with required packages using teradataml
                  Open Analytics APIs. If no ``env_name`` is provided, udf use the default 
                  ``openml_env`` user environment. This default environment has latest Python
                  and scikit-learn versions that are supported by Open Analytics Framework
                  at the time of creating environment.
        delimiter:
            Optional Argument.
            Specifies a delimiter to use when reading columns from a row and
            writing result columns.
            Default value: ','
            Types: str with one character
            Notes:
                * This argument cannot be same as "quotechar" argument.
                * This argument cannot be a newline character.
                * Use a different delimiter if categorial columns in the data contains
                  a character same as the delimiter.
        quotechar:
            Optional Argument.
            Specifies a character that forces input of the user function
            to be quoted using this specified character.
            Using this argument enables the Advanced SQL Engine to
            distinguish between NULL fields and empty strings.
            A string with length zero is quoted, while NULL fields are not.
            Default value: None
            Types: str with one character
            Notes:
                * This argument cannot be same as "delimiter" argument.
                * This argument cannot be a newline character.
        debug:
            Optional Argument.
            Specifies whether to display the script file path generated during function execution or not. This
            argument helps in debugging when there are any failures during function execution. When set
            to True, function displays the path of the script and does not remove the file from local file system.
            Otherwise, file is removed from the local file system.
            Default Value: False
            Types: bool
    RETURNS:
        ColumnExpression
    RAISES:
        TeradataMLException
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "sales")
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame("sales")
        >>> df
                    Feb    Jan    Mar    Apr    datetime
        accounts                                          
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        # Example 1: Create the user defined function to get the values in 'accounts'
        #            to upper case without passing returns argument.
        >>> from teradataml.dataframe.functions import udf
        >>> @udf
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> res = df.assign(upper_stats = to_upper('accounts'))
        >>> res
                    Feb    Jan    Mar    Apr  datetime upper_stats
        accounts                                                    
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04    ALPHA CO
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  ORANGE INC
        Red Inc     200.0  150.0  140.0    NaN  17/01/04     RED INC
        >>>
        # Example 2: Create a user defined function to add length of string values in column  
        #           'accounts' with column 'Feb' and store the result in Integer type column.
        >>> from teradatasqlalchemy.types import INTEGER
        >>> @udf(returns=INTEGER()) 
        ... def sum(x, y):
        ...     return len(x)+y
        >>>
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> res = df.assign(len_sum = sum('accounts', 'Feb'))
        >>> res
                    Feb    Jan    Mar    Apr  datetime  len_sum
        accounts                                                 
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04      218
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04       98
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04      100
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04      209
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04      220
        Red Inc     200.0  150.0  140.0    NaN  17/01/04      207
        >>>
        # Example 3: Create a function to get the values in 'accounts' to upper case
        #            and pass it to udf as parameter to create a user defined function.
        >>> from teradataml.dataframe.functions import udf
        >>> def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>> upper_case = udf(to_upper)
        >>>
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> res = df.assign(upper_stats = upper_case('accounts'))
        >>> res
                    Feb    Jan    Mar    Apr  datetime upper_stats
        accounts                                                    
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04    ALPHA CO
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  ORANGE INC
        Red Inc     200.0  150.0  140.0    NaN  17/01/04     RED INC
        >>>
        # Example 4: Create a user defined function to add 4 to the 'datetime' column
        #            and store the result in DATE type column.
        >>> from teradatasqlalchemy.types import DATE
        >>> import datetime
        >>> @udf(returns=DATE())
        ... def add_date(x, y):
        ...     return (datetime.datetime.strptime(x, "%y/%m/%d")+datetime.timedelta(y)).strftime("%y/%m/%d")
        >>>
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> res = df.assign(new_date = add_date('datetime', 4))
        >>> res
                      Feb    Jan    Mar    Apr  datetime  new_date
        accounts                                                  
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04  17/01/08
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04  17/01/08
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04  17/01/08
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  17/01/08
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  17/01/08
        Red Inc     200.0  150.0  140.0    NaN  17/01/04  17/01/08
        # Example 5: Create a user defined function to add 4 to the 'datetime' column
        #            without passing returns argument.
        >>> from teradatasqlalchemy.types import DATE
        >>> import datetime
        >>> @udf
        ... def add_date(x, y):
        ...     return (datetime.datetime.strptime(x, "%y/%m/%d")+datetime.timedelta(y))
        >>>
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> res = df.assign(new_date = add_date('datetime', 4))
        >>> res
                      Feb    Jan    Mar    Apr  datetime             new_date
        accounts                                                             
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04  2017-01-08 00:00:00
        Red Inc     200.0  150.0  140.0    NaN  17/01/04  2017-01-08 00:00:00
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  2017-01-08 00:00:00
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04  2017-01-08 00:00:00
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  2017-01-08 00:00:00
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04  2017-01-08 00:00:00
        # Example 6: Create a two user defined function to 'to_upper' and 'sum',
        #            'to_upper' to get the values in 'accounts' to upper case and 
        #            'sum' to add length of string values in column 'accounts' 
        #            with column 'Feb' and store the result in Integer type column.
        >>> @udf
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        >>> from teradatasqlalchemy.types import INTEGER
        >>> @udf(returns=INTEGER()) 
        ... def sum(x, y):
        ...     return len(x)+y
        >>>
        # Assign the both Column Expression returned by user defined functions
        # to the DataFrame.
        >>> res = df.assign(upper_stats = to_upper('accounts'), len_sum = sum('accounts', 'Feb'))
        >>> res
                      Feb    Jan    Mar    Apr  datetime upper_stats  len_sum
        accounts                                                             
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC       98
        Red Inc     200.0  150.0  140.0    NaN  17/01/04     RED INC      207
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC      100
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC      209
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  ORANGE INC      220
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04    ALPHA CO      218
        >>>
        # Example 7: Convert the values is 'accounts' column to upper case using a user 
        #            defined function on Vantage Cloud Lake.
        # Create a Python 3.10.5 environment with given name and description in Vantage.
        >>> env = create_env('test_udf', 'python_3.10.5', 'Test environment for UDF')
        User environment 'test_udf' created.
        >>>
        # Create a user defined functions to 'to_upper' to get the values in upper case 
        # and pass the user env to run it on.
        >>> from teradataml.dataframe.functions import udf
        >>> @udf(env_name = env)
        ... def to_upper(s):
        ...     if s is not None:
        ...         return s.upper()
        >>>
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> df.assign(upper_stats = to_upper('accounts'))
                    Feb    Jan    Mar    Apr  datetime upper_stats
        accounts                                                    
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04    ALPHA CO
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04    BLUE INC
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  YELLOW INC
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04   JONES LLC
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  ORANGE INC
        Red Inc     200.0  150.0  140.0    NaN  17/01/04     RED INC
        # Example 8: Create a user defined function to add 4 to the 'datetime' column
        #            and store the result in DATE type column on Vantage Cloud Lake.
        >>> from teradatasqlalchemy.types import DATE
        >>> import datetime
        >>> @udf(returns=DATE())
        ... def add_date(x, y):
        ...     return (datetime.datetime.strptime(x, "%Y-%m-%d")+datetime.timedelta(y)).strftime("%Y-%m-%d")
        >>>
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> res = df.assign(new_date = add_date('datetime', 4))
        >>> res
                      Feb    Jan    Mar    Apr  datetime  new_date
        accounts                                                  
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04  17/01/08
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04  17/01/08
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04  17/01/08
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  17/01/08
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  17/01/08
        Red Inc     200.0  150.0  140.0    NaN  17/01/04  17/01/08
        >>>
        # Example 9: Define a function 'inner_add_date' inside the udf to create a 
        #            date object by passing year, month, and day and add 1 to that date.
        #            Call this function inside the user defined function.
        >>> @udf
        ... def add_date(y,m,d):
        ... import datetime
        ... def inner_add_date(y,m,d):
        ...     return datetime.date(y,m,d) + datetime.timedelta(1)
        ... return inner_add_date(y,m,d)
        # Assign the Column Expression returned by user defined function
        # to the DataFrame.
        >>> res = df.assign(new_date = add_date(2021, 10, 5))
        >>> res
                    Feb    Jan    Mar    Apr  datetime    new_date
        accounts                                                    
        Jones LLC   200.0  150.0  140.0  180.0  17/01/04  2021-10-06
        Blue Inc     90.0   50.0   95.0  101.0  17/01/04  2021-10-06
        Yellow Inc   90.0    NaN    NaN    NaN  17/01/04  2021-10-06
        Orange Inc  210.0    NaN    NaN  250.0  17/01/04  2021-10-06
        Alpha Co    210.0  200.0  215.0  250.0  17/01/04  2021-10-06
        Red Inc     200.0  150.0  140.0    NaN  17/01/04  2021-10-06
        >>>