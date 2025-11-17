# teradataml DataFrameColumn Expressions

teradataml: DataFrameColumn Functions
asc
teradataml.dataframe.sql.DataFrameColumn.asc = asc(self)
    DESCRIPTION:
        Generates a new _SQLColumnExpression which sorts the actual
        expression in Ascending Order.
        Note:
            This function is supported only while sorting the Data. This
            function is neither supported in projection nor supported in
            filtering the Data.
    RAISES:
        None
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
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
    cast
    teradataml.dataframe.sql.DataFrameColumn.cast = cast(self, type_=None, format=None, timezone=None)
    DESCRIPTION:
        Apply the CAST SQL function to the column with the type specified.
        NOTE: This method can currently be used only with 'filter' and
              'assign' methods of teradataml DataFrame.
    PARAMETERS:
        type_:
            Required Argument.
            Specifies a teradatasqlalchemy type or an object of a teradatasqlalchemy type
            that the column needs to be cast to.
            Default value: None
            Types: teradatasqlalchemy type or object of teradatasqlalchemy type
        format:
            Optional Argument.
            Specifies a variable length string containing formatting characters
            that define the display format for the data type.
            Formats can be specified for columns that have character, numeric, byte,
            DateTime, Period or UDT data types.
            Note:
                * Teradata supports different formats. Look at 'Formats' section in
                  "SQL-Data-Types-and-Literals" in Vantage documentation for additional
                  details.
            Default value: None
            Types: str
        timezone:
            Optional Argument.
            Specifies the timezone string.
            Check "SQL-Date-and-Time-Functions-and-Expressions" in
            Vantage documentation for supported timezones.
            Type: ColumnExpression or str.
    RETURNS:
        ColumnExpression
    RAISES:
        TeradataMlException
    EXAMPLES:
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
        >>> df.dtypes
        id               int
        masters          str
        gpa            float
        stats            str
        programming      str
        admitted         int
        >>> dataframe_dict = {"id": [100, 200,300],
        >>> "timestamp_col": ['1000-01-10 23:00:12-02:00', '2015-01-08 13:00:00+12:00', '2014-12-10 10:00:35-08:00'],
        >>> "timezone_col": ["GMT", "America Pacific", "GMT+10"]}
        >>> pandas_df = pd.DataFrame(dataframe_dict)
        >>> copy_to_sql(pandas_df, table_name = 'new_table', if_exists = 'replace')
        >>> df1 = DataFrame("new_table")
        >>> df1
        id              timestamp_col     timezone_col
        300  2014-12-10 10:00:35-08:00           GMT+10
        200  2015-01-08 13:00:00+12:00  America Pacific
        100  1000-01-10 23:00:12-02:00              GMT
        >>> df1.dtypes
        id               int
        timestamp_col    str
        timezone_col     str
        # Example 1: Let's try creating a new DataFrame casting 'id' column (of type INTEGER) to VARCHAR(5),
        #            an object of a teradatasqlalchemy type.
        >>> from teradatasqlalchemy import VARCHAR
        >>> new_df = df.assign(char_id = df.id.cast(type_=VARCHAR(5)))
        >>> new_df
           masters   gpa     stats programming  admitted char_id
        id
        5       no  3.44    Novice      Novice         0       5
        34     yes  3.85  Advanced    Beginner         0      34
        13      no  4.00  Advanced      Novice         1      13
        40     yes  3.95    Novice    Beginner         0      40
        22     yes  3.46    Novice    Beginner         0      22
        19     yes  1.98  Advanced    Advanced         0      19
        36      no  3.00  Advanced      Novice         0      36
        15     yes  4.00  Advanced    Advanced         1      15
        7      yes  2.33    Novice      Novice         1       7
        17      no  3.83  Advanced    Advanced         1      17
        >>> new_df.dtypes
        id               int
        masters          str
        gpa            float
        stats            str
        programming      str
        admitted         int
        char_id          str
        # Example 2:  Now let's try creating a new DataFrame casting 'id' column (of type INTEGER) to VARCHAR,
        #             a teradatasqlalchemy type.
        >>> new_df_2 = df.assign(char_id = df.id.cast(type_=VARCHAR))
        >>> new_df_2
           masters   gpa     stats programming  admitted char_id
        id
        5       no  3.44    Novice      Novice         0       5
        34     yes  3.85  Advanced    Beginner         0      34
        13      no  4.00  Advanced      Novice         1      13
        40     yes  3.95    Novice    Beginner         0      40
        22     yes  3.46    Novice    Beginner         0      22
        19     yes  1.98  Advanced    Advanced         0      19
        36      no  3.00  Advanced      Novice         0      36
        15     yes  4.00  Advanced    Advanced         1      15
        7      yes  2.33    Novice      Novice         1       7
        17      no  3.83  Advanced    Advanced         1      17
        >>> new_df_2.dtypes
        id               int
        masters          str
        gpa            float
        stats            str
        programming      str
        admitted         int
        char_id          str
        # Example 3: Let's try filtering some data with a match on a column cast to another type,
        #            an object of a teradatasqlalchemy type.
        >>> df[df.id.cast(VARCHAR(5)) == '1']
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        # Example 4: Now let's try the same, this time using a teradatasqlalchemy type.
        >>> df[df.id.cast(VARCHAR) == '1']
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        # Example 5: Let's try creating a new DataFrame casting 'timestamp_col' column (of type VARCHAR) to TIMESTAMP,
        #            using format.
        >>> new_df1 = df1.assign(new_col = df1.timestamp_col.cast(TIMESTAMP, format='Y4-MM-DDBHH:MI:SSBZ'))
        id              timestamp_col     timezone_col              new_col
        300  2014-12-10 10:00:35-08:00           GMT+10  2014-12-10 18:00:35
        200  2015-01-08 13:00:00+12:00  America Pacific  2015-01-08 01:00:00
        100  1000-01-10 23:00:12-02:00              GMT  1000-01-11 01:00:12
        >>> new_df1.tdtypes
        id                             int
        timestamp_col                  str
        timezone_col                   str
        new_col          datetime.datetime
        # Example 6: Let's try creating a new DataFrame casting 'id' column (of type INTEGER) to VARCHAR,
        #            using format.
        >>> new_df2 = df1.assign(new_col = df1.id.cast(VARCHAR, format='zzz.zz'))
        id              timestamp_col     timezone_col new_col
        300  2014-12-10 10:00:35-08:00           GMT+10  300.00
        200  2015-01-08 13:00:00+12:00  America Pacific  200.00
        100  1000-01-10 23:00:12-02:00              GMT  100.00
        >>> new_df2.dtypes
        id               int
        timestamp_col    str
        timezone_col     str
        new_col          str
        # Example 7: Let's try creating a new DataFrame casting 'timestamp_with_timezone' column (of type TIMESTAMP) to
        #            TIMESTAMP WITH TIMEZONE, with offset 'GMT+10'.
        >>> new_df3 = new_df1.assign(timestamp_with_timezone = new_df1.new_col.cast(TIMESTAMP(timezone=True), timezone='GMT+10'))
        id              timestamp_col     timezone_col              new_col         timestamp_with_timezone
        300  2014-12-10 10:00:35-08:00           GMT+10  2014-12-10 18:00:35  2014-12-11 04:00:35.000000+10:00
        200  2015-01-08 13:00:00+12:00  America Pacific  2015-01-08 01:00:00  2015-01-08 11:00:00.000000+10:00
        100  1000-01-10 23:00:12-02:00              GMT  1000-01-11 01:00:12  1000-01-11 11:00:12.000000+10:00
        >>> new_df3.dtypes
        id                                       int
        timestamp_col                            str
        timezone_col                             str
        new_col                    datetime.datetime
        timestamp_with_timezone    datetime.datetime
    desc
    teradataml.dataframe.sql.DataFrameColumn.desc = desc(self)
    DESCRIPTION:
        Generates a new _SQLColumnExpression which sorts the actual
        expression in Descending Order.
        Note:
            This function is supported only while sorting the Data. This
            function is neither supported in projection nor supported in
            filtering the Data.
    RAISES:
        None
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
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
    distinct
    teradataml.dataframe.sql.DataFrameColumn.distinct = distinct(self)
    DESCRIPTION:
        Generates a new _SQLColumnExpression which removes the duplicate
        rows while processing the function.
        Note:
            This function is supported only in Projection. It is neither
            supported in sorting the records nor supported in filtering
            the records.
    RAISES:
        None
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> from teradataml import *
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> df
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        >>>
        >>> df.assign(drop_columns=True, distinct_feb=df.Feb.distinct())
           distinct_feb
        0         210.0
        1          90.0
        2         200.0
        >>>
    isin
    teradataml.dataframe.sql.DataFrameColumn.isin = isin(self, values=None)
    Function to check for the presence of values in a column.
    PARAMETERS:
        values:
            Required Argument.
            Specifies the list of values to check for their presence in the column.
            in the provided set of values.
            Types: list
    RETURNS:
        _SQLColumnExpression
    RAISES:
        TypeError - If invalid type of values are passed to argument 'values'.
        ValueError - If None is passed to argument 'values'.
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame('admissions_train')
        >>> df
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
        # Example 1: Filter results where gpa values are in any of these following values:
        #            4.0, 3.0, 2.0, 1.0, 3.5, 2.5, 1.5
        >>> df[df.gpa.isin([4.0, 3.0, 2.0, 1.0, 3.5, 2.5, 1.5])]
           masters  gpa     stats programming  admitted
        id
        31     yes  3.5  Advanced    Beginner         1
        6      yes  3.5  Beginner    Advanced         1
        13      no  4.0  Advanced      Novice         1
        4      yes  3.5  Beginner      Novice         1
        29     yes  4.0    Novice    Beginner         0
        15     yes  4.0  Advanced    Advanced         1
        36      no  3.0  Advanced      Novice         0
        >>>
        # Example 2: Filter results where stats values are neither 'Novice' nor 'Advanced'
        >>> df[~df.stats.isin(['Novice', 'Advanced'])]
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        2      yes  3.76  Beginner    Beginner         0
        8       no  3.60  Beginner    Advanced         1
        4      yes  3.50  Beginner      Novice         1
        6      yes  3.50  Beginner    Advanced         1
        >>>
    isna
    teradataml.dataframe.sql.DataFrameColumn.isna = isna(self)
    Test for NA values
    PARAMETERS:
        None
    RETURNS:
        When used with assign() function, newly assigned column contains
        A boolean Series of numeric values:
          - 1 if value is NA (None)
          - 0 if values is not NA
        Otherwise returns ColumnExpression, also known as, teradataml DataFrameColumn.
    EXAMPLES:
        >>> load_example_data("dataframe", "sales")
        >>> df = DataFrame("sales")
        # Example 1: Filter out the NA values from 'Mar' column.
        >>> df[df.Mar.isna() == 1]
                      Feb   Jan   Mar    Apr    datetime
        accounts
        Orange Inc  210.0  None  None  250.0  04/01/2017
        Yellow Inc   90.0  None  None    NaN  04/01/2017
        # Filter out the non-NA values from 'Mar' column.
        >>> df[df.Mar.isna() == 0]
                     Feb  Jan  Mar    Apr    datetime
        accounts
        Blue Inc    90.0   50   95  101.0  04/01/2017
        Red Inc    200.0  150  140    NaN  04/01/2017
        Jones LLC  200.0  150  140  180.0  04/01/2017
        Alpha Co   210.0  200  215  250.0  04/01/2017
        # Example 2: Filter out the NA values from 'Mar' column using boolean True.
        >>> df[df.Mar.isna() == True]
                      Feb   Jan   Mar    Apr    datetime
        accounts
        Orange Inc  210.0  None  None  250.0  04/01/2017
        Yellow Inc   90.0  None  None    NaN  04/01/2017
        # Filter out the non-NA values from 'Mar' column using boolean False.
        >>> df[df.Mar.isna() == False]
                     Feb  Jan  Mar    Apr    datetime
        accounts
        Blue Inc    90.0   50   95  101.0  04/01/2017
        Red Inc    200.0  150  140    NaN  04/01/2017
        Jones LLC  200.0  150  140  180.0  04/01/2017
        Alpha Co   210.0  200  215  250.0  04/01/2017
        # Example 3: Assign the tested values to dataframe as a column.
        >>> df.assign(isna_=df.Mar.isna())
                      Feb    Jan    Mar    Apr    datetime isna_
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     1
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     1
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     0
    isnull
    teradataml.dataframe.sql.DataFrameColumn.isnull = isnull(self)
    Test for NA values. Alias for isna()
    PARAMETERS:
        None
    RETURNS:
        When used with assign() function, newly assigned column contains
        A boolean Series of numeric values:
          - 1 if value is NA (None)
          - 0 if values is not NA
        Otherwise returns ColumnExpression, also known as, teradataml DataFrameColumn.
    EXAMPLES:
        >>> load_example_data("dataframe", "sales")
        >>> df = DataFrame("sales")
        # Example 1: Filter out the NA values from 'Mar' column.
        >>> df[df.Mar.isnull() == 1]
                      Feb   Jan   Mar    Apr    datetime
        accounts
        Orange Inc  210.0  None  None  250.0  04/01/2017
        Yellow Inc   90.0  None  None    NaN  04/01/2017
        # Filter out the non-NA values from 'Mar' column.
        >>> df[df.Mar.isnull() == 0]
                     Feb  Jan  Mar    Apr    datetime
        accounts
        Blue Inc    90.0   50   95  101.0  04/01/2017
        Red Inc    200.0  150  140    NaN  04/01/2017
        Jones LLC  200.0  150  140  180.0  04/01/2017
        Alpha Co   210.0  200  215  250.0  04/01/2017
        # Example 2: Filter out the NA values from 'Mar' column using boolean True.
        >>> df[df.Mar.isnull() == True]
                      Feb   Jan   Mar    Apr    datetime
        accounts
        Orange Inc  210.0  None  None  250.0  04/01/2017
        Yellow Inc   90.0  None  None    NaN  04/01/2017
        # Filter out the non-NA values from 'Mar' column using boolean False.
        >>> df[df.Mar.isnull() == False]
                     Feb  Jan  Mar    Apr    datetime
        accounts
        Blue Inc    90.0   50   95  101.0  04/01/2017
        Red Inc    200.0  150  140    NaN  04/01/2017
        Jones LLC  200.0  150  140  180.0  04/01/2017
        Alpha Co   210.0  200  215  250.0  04/01/2017
        # Example 3: Assign the tested values to dataframe as a column.
        >>> df.assign(isnull_=df.Mar.isnull())
                      Feb    Jan    Mar    Apr    datetime isnull_
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     1
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     1
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     0
    notna
    teradataml.dataframe.sql.DataFrameColumn.notna = notna(self)
    Test for non NA values
    The boolean complement of isna()
    PARAMETERS:
        None
    RETURNS:
        When used with assign() function, newly assigned column contains
        A boolean Series of numeric values:
          - 1 if value is NA (None)
          - 0 if values is not NA
        Otherwise returns ColumnExpression, also known as, teradataml DataFrameColumn.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        # Test for NA values on dataframe by using 0 and 1.
        >>> df[df.gpa.notna() == 1]
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
        >>> df[df.gpa.notna() == 0]
        Empty DataFrame
        Columns: [masters, gpa, stats, programming, admitted]
        Index: []
        # Test for NA values on dataframe by using False and True.
        >>> df[df.gpa.notna() == True]
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
        >>> df[df.gpa.notna() == False]
        Empty DataFrame
        Columns: [masters, gpa, stats, programming, admitted]
        Index: []
        # Assign the tested values to dataframe as a column.
        >>> df.assign(notna_=df.gpa.notna())
           masters   gpa     stats programming  admitted notna_
        id
        22     yes  3.46    Novice    Beginner         0      1
        36      no  3.00  Advanced      Novice         0      1
        15     yes  4.00  Advanced    Advanced         1      1
        38     yes  2.65  Advanced    Beginner         1      1
        5       no  3.44    Novice      Novice         0      1
        17      no  3.83  Advanced    Advanced         1      1
        34     yes  3.85  Advanced    Beginner         0      1
        13      no  4.00  Advanced      Novice         1      1
        26     yes  3.57  Advanced    Advanced         1      1
        19     yes  1.98  Advanced    Advanced         0      1
    notnull
    teradataml.dataframe.sql.DataFrameColumn.notnull = notnull(self)
    Alias for notna().Test for non NA values
    The boolean complement of isna()
    PARAMETERS:
        None
    RETURNS:
        When used with assign() function, newly assigned column contains
        A boolean Series of numeric values:
          - 1 if value is NA (None)
          - 0 if values is not NA
        Otherwise returns ColumnExpression, also known as, teradataml DataFrameColumn.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df[df.gpa.notnull() == 1]
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
        >>> df[df.gpa.notnull() == 0]
        Empty DataFrame
        Columns: [masters, gpa, stats, programming, admitted]
        Index: []
        # alternatively, True and False can be used
        >>> df[df.gpa.notnull() == True]
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        13      no  4.00  Advanced      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        >>> df[df.gpa.notnull() == False]
        Empty DataFrame
        Columns: [masters, gpa, stats, programming, admitted]
        Index: []
        # Assign the tested values to dataframe as a column.
        >>> df.assign(notnull_=df.gpa.notnull())
           masters   gpa     stats programming  admitted notnull_
        id
        22     yes  3.46    Novice    Beginner         0       1
        26     yes  3.57  Advanced    Advanced         1       1
        5       no  3.44    Novice      Novice         0       1
        17      no  3.83  Advanced    Advanced         1       1
        13      no  4.00  Advanced      Novice         1       1
        19     yes  1.98  Advanced    Advanced         0       1
        36      no  3.00  Advanced      Novice         0       1
        15     yes  4.00  Advanced    Advanced         1       1
        34     yes  3.85  Advanced    Beginner         0       1
        38     yes  2.65  Advanced    Beginner         1       1
    nulls_first
    teradataml.dataframe.sql.DataFrameColumn.nulls_ﬁrst = nulls_ﬁrst(self)
    DESCRIPTION:
        Generates a new _SQLColumnExpression which displays NULL values first.
        Note:
            The function can be applied only in conjunction with "asc" or "desc".
    RAISES:
        None
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        # Sorts the Data on column accounts in ascending order and column
        # Feb in descending order, then calculates moving average by dropping
        # the input DataFrame columns on the window of size 2.
        >>> df.mavg(width=2, sort_columns=[df.accounts.desc().nulls_first()], drop_columns=True)
           mavg_Feb  mavg_Jan  mavg_Mar  mavg_Apr mavg_datetime
        0     145.0     100.0     117.5     140.5    04/01/2017
        1     205.0     150.0     140.0     250.0    04/01/2017
        2     145.0     150.0     140.0       NaN    04/01/2017
        3     205.0     150.0     140.0     215.0    04/01/2017
        4     150.0     125.0     155.0     175.5    04/01/2017
        5     210.0     200.0     215.0     250.0    04/01/2017
    nulls_last
    teradataml.dataframe.sql.DataFrameColumn.nulls_last = nulls_last(self)
    DESCRIPTION:
        Generates a new _SQLColumnExpression which displays NULL values last.
        Note:
            The function can be applied only in conjunction with "asc" or "desc".
    RAISES:
        None
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        # Sorts the Data on column accounts in ascending order and column
        # Feb in descending order, then calculates moving average by dropping
        # the input DataFrame columns on the window of size 2.
        >>> df.mavg(width=2, sort_columns=[df.accounts.asc().nulls_last(), df.Feb.desc().nulls_last()], drop_columns=True)
           mavg_Feb  mavg_Jan  mavg_Mar  mavg_Apr mavg_datetime
        0     145.0     100.0     117.5     140.5    04/01/2017
        1     205.0     150.0     140.0     250.0    04/01/2017
        2     145.0     150.0     140.0       NaN    04/01/2017
        3     205.0     150.0     140.0     215.0    04/01/2017
        4     150.0     125.0     155.0     175.5    04/01/2017
        5     210.0     200.0     215.0     250.0    04/01/2017
    replace
    teradataml.dataframe.sql.DataFrameColumn.replace = replace(self, to_replace, value=None)
    DESCRIPTION:
        Function replaces every occurrence of "to_replace" in the column
        with the "value". Use this function to replace a value from string.
        Note:
            The function replaces value in a string column only when it
            matches completely. If you want to replace a value in a string
            column with only a portion of the string, then use function
            oreplace.
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
    RAISES:
        TeradataMlException
    RETURNS:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml", "chi_sq")
        # Create a DataFrame on 'chi_sq' table.
        >>> df = DataFrame("chi_sq")
        >>> print(df)
                dem  rep
        gender
        female    6    9
        male      8    5
        # Example 1: Create a new column 'new_column' by replacing all the
        #            occurances of 'male' with 'man' in Column 'gender'.
        >>> res = df.assign(new_column = df.gender.replace("male", "man"))
        >>> print(res)
                dem  rep new_column
        gender
        male      8    5        man
        female    6    9     female
        # Example 2: Create a new Column 'new_column' by replacing all the
        #            occurances of 5 with square root of 5 in Column 'rep'.
        >>> print(df.assign(new_column = df.rep.replace(5, df.rep.sqrt())))
                dem  rep  new_column
        gender
        male      8    5    2.236068
        female    6    9    9.000000
        # Example 3: Create a new Column 'new_column' by replacing all the
        #            occurances of 5 with square root of 5 and 9 with
        #            the values of Column 'dem' in Column 'rep'.
        >>> print(df.assign(new_column = df.rep.replace({5: df.rep.sqrt(), 9:df.dem})))
                dem  rep  new_column
        gender
        male      8    5    2.236068
        female    6    9    6.000000
        # Example 4: Create a new Column 'new_column' by replacing all the
        #            the values of Column 'rep' with it's square root.
        >>> print(df.assign(new_column = df.rep.replace({df.rep: df.rep.sqrt()})))
                dem  rep  new_column
        gender
        female    6    9    3.000000
        male      8    5    2.236068
    to_date
    teradataml.dataframe.sql.DataFrameColumn.to_date = to_date(self, formatter=None)
    DESCRIPTION:
        Convert a string-like representation of a DATE or PERIOD type to Date type.
    PARAMETERS:
        formatter:
            Optional Argument.
            Specifies a variable length string containing formatting characters
            that define the format of column.
            Type: str
            Note:
                * If "formatter" is omitted, the following default date format is used: 'YYYY-MM-DD'
            * formatter for date type:
                +--------------------------------------------------------------------------------------------------+
                |    FORMATTER                             DESCRIPTION                                             |
                +--------------------------------------------------------------------------------------------------+
                |    -                                                                                             |
                |    /                                                                                             |
                |    ,                                     Punctuation characters are ignored and text enclosed in |
                |    .                                     quotation marks is ignored.                             |
                |    ;                                                                                             |
                |    :                                                                                             |
                |    "text"                                                                                        |
                |                                         Example: Date with value '2003-12-10'                    |
                |                                              +-------------------------------------------------+ |
                |                                              | data             formatter          value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | '2003-12-10'     YYYY-MM-DD         03/12/10    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    D                                     Day of week (1-7).                                      |
                |                                          Example: day of week with value '2'                     |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2               D                   24/01/01    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DAY                                   Name of day.                                            |
                |                                          Example: Date with value '2024-TUESDAY-01-30'           |
                |                                              +-------------------------------------------------+ |
                |                                              | data                formatter         value     | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2024-TUESDAY-01-30   YYYY-DAY-MM-DD   24/01/30  | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DD                                    Day of month (1-31).                                    |
                |                                          Example: Date with value '2003-10-25'                   |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2003-10-25       YYYY-MM-DD         03/10/25    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DDD                                   Day of year (1-366).                                    |
                |                                          Example: Date with value '2024-366'                     |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2024-366       YYYY-DDD             24/12/31    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DY                                    abbreviated name of day.                                |
                |                                          Example: Date with value '2024-Mon-01-29'               |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2024-Mon-01-29   YYYY-DY-MM-DD   24/01/29       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    HH                                                                                            |
                |    HH12                                 Hour of day (1-12).                                      |
                |                                          Example: Date with value '2016-01-06 09:08:01'          |
                |                                              +-------------------------------------------------+ |
                |                                              | data                formatter            value  | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01 YYYY-MM-DD HH:MI:SS  6/01/06| |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    HH24                                 Hour of the day (0-23).                                  |
                |                                          Example:  Date with value '2016-01-06 23:08:01'         |
                |                                           +----------------------------------------------------+ |
                |                                           | data                formatter              value   | |
                |                                           +----------------------------------------------------+ |
                |                                           | 2016-01-06 23:08:01 YYYY-MM-DD HH24:MI:SS  6/01/06 | |
                |                                           +----------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    J                                    Julian day, the number of days since January 1, 4713 BC. |
                |                                         Number specified with J must be integers.                |
                |                                         Teradata uses the Gregorian calendar in calculations to  |
                |                                         and from Julian Days.                                    |
                |                                          Example: Number of julian days with value '2457394'     |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2457394         J                   16/01/06    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MI                                   Minute (0-59).                                           |
                |                                          Example: Date with value '2016-01-06 23:08:01'          |
                |                                           +----------------------------------------------------+ |
                |                                           | data                formatter              value  | |
                |                                           +----------------------------------------------------+ |
                |                                           | 2016-01-06 23:08:01 YYYY-MM-DD HH24:MI:SS  6/01/06 | |
                |                                           +----------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MM                                   Month (01-12).                                           |
                |                                          Example: Date with value '2016-01-06 23:08:01'          |
                |                                           +----------------------------------------------------+ |
                |                                           | data                formatter              value  | |
                |                                           +----------------------------------------------------+ |
                |                                           | 2016-01-06 23:08:01 YYYY-MM-DD HH24:MI:SS  6/01/06 | |
                |                                           +----------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MON                                  Abbreviated name of month.                               |
                |                                          Example: Date with value '2016-JAN-06'                  |
                |                                           +----------------------------------------------------+ |
                |                                           | data                formatter              value   | |
                |                                           +----------------------------------------------------+ |
                |                                           | 2016-JAN-06         YYYY-MON-DD           16/01/06 | |
                |                                           +----------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MONTH                                Name of month.                                           |
                |                                          Example: Date with value '2016-JANUARY-06'              |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter              value    | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-JANUARY-06  YYYY-MONTH-DD         16/01/06 | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    PM                                                                                            |
                |    P.M.                                 Meridian indicator.                                      |
                |                                          Example:  Date with value '2016-01-06 23:08:01 PM'      |
                |                                      +---------------------------------------------------------+ |
                |                                      | data                   formatter                value   | |
                |                                      +---------------------------------------------------------+ |
                |                                      | 2016-01-06 23:08:01 PM YYYY-MM-DD HH24:MI:SS PM 16/01/06| |
                |                                      +---------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    RM                                   Roman numeral month (I - XII).                           |
                |                                          Example: Date with value '2024-XII'                     |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2024-XII       YYYY-RM             24/12/01     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    RR                                   Stores 20th century dates in the 21st century using only |
                |                                         2 digits. If the current year and the specified year are |
                |                                         both in the range of 0-49, the date is in the current    |
                |                                         century.                                                 |
                |                                         Example: Date with value '2024-365, 21'                  |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2024-365, 21      YYYY-DDD, RR      21/12/31    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    RRRR                                 Round year. Accepts either 4-digit or 2-digit input.     |
                |                                         2-digit input provides the same return as RR.            |
                |                                          Example: Date with value '2024-365, 21'                  |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2024-365, 21       YYYY-DDD, RRRR   24/12/31    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    SS                                   Second (0-59).                                           |
                |                                          Example: Date with value '2016-01-06 23:08:01'          |
                |                                           +----------------------------------------------------+ |
                |                                           | data                formatter              value   | |
                |                                           +----------------------------------------------------+ |
                |                                           | 2016-01-06 23:08:01 YYYY-MM-DD HH24:MI:SS  6/01/06 | |
                |                                           +----------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    SSSSS                                Seconds past midnight (0-86399).                         |
                +--------------------------------------------------------------------------------------------------+
                |    TZH                                  Time zone hour.                                          |
                +--------------------------------------------------------------------------------------------------+
                |    TZM                                  Time zone minute.                                        |
                +--------------------------------------------------------------------------------------------------+
                |    X                                    Local radix character.                                   |
                |                                         Example: Date with value '2024.366'                      |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2024.366       YYYYXDDD             24/12/31    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    Y,YYY                                Year with comma in this position.                        |
                |                                          Example: Date with value '2,024-366'                    |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2,024-366       Y,YYY-DDD           24/12/31    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    YYYY                                                                                          |
                |    SYYYY                                4-digit year. S prefixes BC dates with a minus sign.     |
                |                                          Example: Date with value '2024-366'                     |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter           value       | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2024-366       YYYY-DDD             24/12/31    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    YYY                                  Last 3, 2, or 1 digit of year.                           |
                |    YY                                   If the current year and the specified year are both in   |
                |    Y                                    the range of 0-49, the date is in the current century.   |
                |                                          Example: Date with value '24-366'                       |
                |                                              +-------------------------------------------------+ |
                |                                              | data            formatter       value           | |
                |                                              +-------------------------------------------------+ |
                |                                              | 24-366       YY-DDD             24/12/31        | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
    RAISES:
        TypeError, ValueError, TeradataMlException
    Returns:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("uaf", "stock_data")
        # Create a DataFrame on 'stock_data' table.
        >>> df = DataFrame("stock_data")
        >>> df
                    seq_no timevalue  magnitude
        data_set_id
        556               3  19/01/16     61.080
        556               5  19/01/30     63.810
        556               6  19/02/06     63.354
        556               7  19/02/13     63.871
        556               9  19/02/27     61.490
        556              10  19/03/06     61.524
        556               8  19/02/20     61.886
        556               4  19/01/23     63.900
        556               2  19/01/09     61.617
        556               1  19/01/02     60.900
        # create new_column "timevalue_char" using to_char().
        >>> new_df = df.assign(timevalue_char=df.timevalue.to_char('DD-MON-YYYY'))
        >>> new_df
                        seq_no timevalue  magnitude timevalue_char
        data_set_id
        556               3  19/01/16     61.080    16-JAN-2019
        556               5  19/01/30     63.810    30-JAN-2019
        556               6  19/02/06     63.354    06-FEB-2019
        556               7  19/02/13     63.871    13-FEB-2019
        556               9  19/02/27     61.490    27-FEB-2019
        556              10  19/03/06     61.524    06-MAR-2019
        556               8  19/02/20     61.886    20-FEB-2019
        556               4  19/01/23     63.900    23-JAN-2019
        556               2  19/01/09     61.617    09-JAN-2019
        556               1  19/01/02     60.900    02-JAN-2019
        # Example 1: convert "timevalue_char" column to DATE type.
        >>> res = new_df.assign(timevalue_char=new_df.timevalue_char.to_date('DD-MON-YYYY'))
        >>> res
                    seq_no timevalue  magnitude timevalue_char
        data_set_id
        556               3  19/01/16     61.080       19/01/16
        556               5  19/01/30     63.810       19/01/30
        556               6  19/02/06     63.354       19/02/06
        556               7  19/02/13     63.871       19/02/13
        556               9  19/02/27     61.490       19/02/27
        556              10  19/03/06     61.524       19/03/06
        556               8  19/02/20     61.886       19/02/20
        556               4  19/01/23     63.900       19/01/23
        556               2  19/01/09     61.617       19/01/09
        556               1  19/01/02     60.900       19/01/02
        >>> res.tdtypes
        column            type
        data_set_id       INTEGER()
        seq_no            INTEGER()
        timevalue            DATE()
        magnitude           FLOAT()
        timevalue_char       DATE()
    to_number
    teradataml.dataframe.sql.DataFrameColumn.to_number = to_number(self, formatter=None)
    DESCRIPTION:
        Converts a string-like representation of a number to NUMBER type.
    PARAMETERS:
        formatter:
            Optional Argument.
            Specifies a variable length string containing formatting characters
            that define the format of the columns.
            Type: str OR ColumnExpression
            Note:
                * If 'formatter' is omitted, numeric values is converted to a string exactly
                  long enough to hold its significant digits.
            * Formatters:
                +--------------------------------------------------------------------------------------------------+
                |    FORMATTER                             DESCRIPTION                                             |
                +--------------------------------------------------------------------------------------------------+
                |    , (comma)                             A comma in the specified position.                      |
                |                                          A comma cannot begin a number format.                   |
                |                                          A comma cannot appear to the right of a decimal         |
                |                                          character or period in a number format.                 |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "1,234"            "9,999"           1234     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    . (period)                            A decimal point. Only one allowed in a format.          |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "12.34"            "99.99"          12.34     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    $                                     A value with a leading dollar sign.                     |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "$1234"            "$9999"           1234     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    0                                     Leading or trailing zeros.                              |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "0123"             "0999"            123      | |
                |                                              |   "1230"             "9990"            1230     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    9                                     Specified number of digits.                             |
                |                                          Leading space if positive, minus if negative.           |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    "1234"            "9999"            1234     | |
                |                                              |   "-1234"            "9999"           -1234     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    B                                     Blanks if integer part is zero.                         |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    "0"               "B9999"             0      | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    C                                     ISO currency symbol (from SDF ISOCurrency).             |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "USD123"            "C999"             123    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    D                                     Radix separator for non-monetary values.                |
                |                                          From SDF RadixSeparator.                                |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "12.34"            "99D99"           12.34    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    EEEE                                  Scientific notation.                                    |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "1.2E+04"         "9.9EEEE"          12000    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    G                                     Group separator for non-monetary values.                |
                |                                          From SDF GroupSeparator.                                |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "1,234,567"       "9G999G999"       1234567   | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    L                                     Local currency (from SDF Currency element).             |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "$123"             "L999"              123    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MI                                    Trailing minus sign if value is negative.               |
                |                                          Can only appear in the last position.                   |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |   "1234-"            "9999MI"          -1234    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    PR                                    Negative value in angle brackets.                       |
                |                                          Positive value with leading/trailing blank.             |
                |                                          Only in the last position.                              |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    " 123 "          "9999PR"            123     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    S                                     Sign indicator: + / - at beginning or end.              |
                |                                          Can only appear in first or last position.              |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    "-1234"           "S9999"           -1234    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    U                                     Dual currency (from SDF DualCurrency).                  |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    "$123"             "U999"             123    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    X                                     Hexadecimal format.                                     |
                |                                          Accepts only non-negative values.                       |
                |                                          Must be preceded by 0 or FM.                            |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    "FF"                "XX"             255     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml", "to_num_data")
        # Create a DataFrame on 'to_num_data' table.
        >>> df = DataFrame("to_num_data")
        >>> df
        price col_format
        $1234      $9999
        USD123      C999
        78.12      99.99
        #  Example 1: Convert 'price' column to number type without passing any formatter.
        >>> res = df.assign(new_col=df.price.to_number())
        >>> res
        price col_format  new_col
        $1234      $9999      NaN
        USD123      C999      NaN
        78.12      99.99    78.12
        #  Example 2: Convert 'price' column to number type by passing formatter as string.
        >>> res = df.assign(new_col=df.price.to_number('99.99'))
        >>> res
        price col_format  new_col
        $1234      $9999      NaN
        USD123      C999      NaN
        78.12      99.99    78.12
        #  Example 3: Convert 'price' column to number type by passing formatter as ColumnExpression.
        >>> res = df.assign(new_col=df.price.to_number(df.col_format))
        >>> res
        price col_format  new_col
         $1234     $9999     1234
        USD123      C999      123
         78.12     99.99    78.12
        >>> df.tdtypes
        price           VARCHAR(length=20, charset='LATIN')
        col_format  VARCHAR(length=20, charset='LATIN')
        new_col                                    NUMBER()
    window
    teradataml.dataframe.sql.DataFrameColumn.window = window(self, partition_columns=None, order_columns=None, sort_ascending=True, nulls_ﬁrst=None,
    window_start_point=None, window_end_point=None, ignore_window=False)
    DESCRIPTION:
        This function generates Window object on a teradataml DataFrame Column to run
        window aggregate functions.
        Function allows user to specify window for different types of
        computations:
            * Cumulative
            * Group
            * Moving
            * Remaining
        By default, window with Unbounded Preceding and Unbounded following
        is considered for calculation.
        Note:
            If both "partition_columns" and "order_columns" are None, then
            Window cannot be created on CLOB and BLOB type of columns.
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
                    groupby function, if Column is from DataFrameGroupBy.
            Types: str OR list of Strings (str)
        order_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to order the rows in a
            partition, which determines the sort order of the rows over
            which the function is applied.
            Notes:
                1. "order_columns" does not support CLOB and BLOB type of
                   columns.
                   Refer 'DataFrame.tdtypes' to get the types of the columns
                   of a teradataml DataFrame.
                2. "order_columns" supports only columns specified in
                    groupby function, if Column is from DataFrameGroupBy.
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
                 * When "order_columns" argument is not specified, argument
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
                 * When "order_columns" argument is not specified, argument
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
                * If 'n' is None, window end's at Unbounded following,
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
        # Example 1: Create a window on a teradataml DataFrame column.
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame.from_table('sales')
        >>> window = df.Feb.window()
        >>>
        # Example 2: Create a cumulative (expanding) window with rows
        #            between unbounded preceding and 3 preceding with
        #            "partition_columns" and "order_columns" argument with
        #            default sorting.
        >>> window = df.Feb.window(partition_columns=df.Feb,
        ...                        order_columns=[df.Feb, df.datetime],
        ...                        window_start_point=None,
        ...                        window_end_point=-3)
        >>>
        # Example 3: Create a moving (rolling) window with rows between
        #            current row and 3 following with sorting done on 'Feb',
        #            'datetime' columns in descending order and
        #            "partition_columns" argument.
        >>> window = df.Feb.window(partition_columns=df.Feb,
        ...                        order_columns=[df.Feb.desc(), df.datetime.desc()],
        ...                        window_start_point=0,
        ...                        window_end_point=3)
        >>>
        # Example 4: Create a remaining (contracting) window with rows
        #            between current row and unbounded following with
        #            sorting done on 'Feb', 'datetime' columns in ascending
        #            order and NULL values in 'Feb', 'datetime'
        #            columns appears at last.
        >>> window = df.Feb.window(partition_columns="Feb",
        ...                        order_columns=[df.Feb.nulls_first(), df.datetime.nulls_first()],
        ...                        window_start_point=0,
        ...                        window_end_point=None
        ...                        )
        >>>
        # Example 5: Create a grouping window, with sorting done on 'Feb',
        #            'datetime' columns in ascending order and NULL values
        #            in 'Feb', 'datetime' columns appears at last.
        >>> window = df.Feb.window(partition_columns=df.Feb,
        ...                        order_columns=[df.Feb.desc().nulls_last(), df.datetime.desc().nulls_last()],
        ...                        window_start_point=None,
        ...                        window_end_point=None
        ...                        )
        >>>
        # Example 6: Create a window on a teradataml DataFrame column, which
        #            ignores all the parameters while creating window.
        >>> window = df.Feb.window(partition_columns=df.Feb,
        ...                        order_columns=[df.Feb.desc().nulls_last(), df.datetime.desc().nulls_last()],
        ...                        ignore_window=True
        ...                        )
        >>>
        # Example 7: Perform sum of Feb and attach new column to the
        # DataFrame.
        >>> window = df.Feb.window()
        >>> df.assign(feb_sum=window.sum())
                      Feb    Jan    Mar    Apr    datetime  feb_sum
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017   1000.0
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017   1000.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017   1000.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017   1000.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017   1000.0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017   1000.0
        >>>
        # Example 8: Perform min and max operations on column Apr and
        # attach both the columns to the DataFrame.
        >>> window = df.Apr.window()
        >>> df.assign(apr_min=window.min(), apr_max=window.max())
                      Feb    Jan    Mar    Apr    datetime  apr_max  apr_min
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017      250      101
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017      250      101
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017      250      101
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017      250      101
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017      250      101
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017      250      101
        >>>
        # Example 9: Perform count and max operations on column accounts in
        # teradataml DataFrame, which is grouped by 'accounts', and attach
        # column to DataFrame.
        >>> df = df.groupby("accounts")
        >>> window = df.accounts.window()
        >>> df.assign(accounts_max=window.max(), accounts_count=window.count())
             accounts  accounts_count accounts_max
        0   Jones LLC               6   Yellow Inc
        1     Red Inc               6   Yellow Inc
        2  Yellow Inc               6   Yellow Inc
        3  Orange Inc               6   Yellow Inc
        4    Blue Inc               6   Yellow Inc
        5    Alpha Co               6   Yellow Inc
        >>>
    DataFrameColumn Aggregate Functions
    corr
    teradataml.dataframe.sql.DataFrameColumn.corr = corr(expression)
    DESCRIPTION:
        Function returns the Sample Pearson product moment correlation
        coefficient for all non-null data point pairs of ColumnExpression.
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
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Get the Sample Pearson product moment correlation coefficient
        #            between 'gpa' and 'admitted' columns.
        # Execute corr() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> corr_column = admissions_train.admitted.corr(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, corr_=corr_column)
        >>> df
              corr_
        0 -0.022265
        # Example 2: Get the Sample Pearson product moment correlation coefficient
        #            between 'gpa' and 'admitted' columns for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute corr() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> corr_column = admissions_train.admitted.corr(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(corr_=corr_column)
        >>> df
          programming     corr_
        0    Advanced  0.487737
        1      Novice -0.114656
        2    Beginner -0.417565
        >>>
    count
    teradataml.dataframe.sql.DataFrameColumn.count = count(self, distinct=False, skipna=False, **kwargs)
    DESCRIPTION:
        Function to get the number of values in a column.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Default Values: False
            Types: bool
        skipna:
            Optional Argument.
            Specifies a flag that decides whether to skip null values or not.
            Default Values: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Get the count of the values in 'gpa' column.
        # Execute count() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> count_column = admissions_train.gpa.count()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, count_=count_column)
        >>> df
           count_
        0      40
        >>>
        # Example 2: Get the count of the distinct values in 'gpa' column
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute count() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> count_column = admissions_train.gpa.count(distinct=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(count_=count_column)
        >>> df
          programming  count_
        0    Advanced      15
        1      Novice      11
        2    Beginner      11
        >>>
    covar_pop
    teradataml.dataframe.sql.DataFrameColumn.covar_pop = covar_pop(expression)
    DESCRIPTION:
        Function returns the population covariance of its arguments for all non-null
        data point pairs. Covariance measures whether or not two random variables
        vary in the same way. It is the average of the products of deviations for
        each non-null data point pair.
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
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
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
        # Example 1: Calculate population covariance between 'admitted' and 'gpa'
        #            columns in teradataml DataFrame.
        # Execute covar_pop() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> covar_pop_column = admissions_train.admitted.covar_pop(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, covar_pop_=covar_pop_column)
        >>> df
           covar_pop_
        0   -0.005387
        # Example 2:Calculate population covariance between 'admitted' and 'gpa'
        #           columns in teradataml DataFrame, for each level of programming.
        # Note:
        #     When assign() is run after DataFrame.groupby(), the function ignores
        #     the "drop_columns" argument.
        # Execute covar_pop() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> covar_pop_column = admissions_train.admitted.covar_pop(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(covar_pop_=covar_pop_column)
        >>> df
          programming  covar_pop_
        0    Advanced    0.091055
        1      Novice   -0.031488
        2    Beginner   -0.069231
        >>>
    covar_samp
    teradataml.dataframe.sql.DataFrameColumn.covar_samp = covar_samp(expression)
    DESCRIPTION:
        Function returns the sample covariance of its arguments for all non-null
        data point pairs. Covariance measures whether or not two random variables
        vary in the same way. It is the average of the products of deviations for
        each non-null data point pair.
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
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
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
        # Example 1: Calculate sample covariance between 'admitted' and 'gpa'
        #            columns in teradataml DataFrame.
        # Execute covar_samp() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> covar_samp_column = admissions_train.admitted.covar_samp(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, covar_samp_=covar_samp_column)
        >>> df
           covar_samp_
        0    -0.005526
        # Example 2:Calculate sample covariance between 'admitted' and 'gpa'
        #           columns in teradataml DataFrame, for each level of programming.
        # Note:
        #     When assign() is run after DataFrame.groupby(), the function ignores
        #     the "drop_columns" argument.
        # Execute covar_samp() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> covar_samp_column = admissions_train.admitted.covar_samp(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(covar_samp_=covar_samp_column)
        >>> df
          programming  covar_samp_
        0    Advanced     0.097125
        1      Novice    -0.034636
        2    Beginner    -0.075000
        >>>
    csum
    teradataml.dataframe.sql.DataFrameColumn.csum = csum(sort_columns)
    DESCRIPTION:
        Returns cumulative sum value for rows in ColumnExpression, over the
        partition specified by "sort_columns".
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
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
        * One must use DataFrame.assign() when using the aggregate functions on
          ColumnExpression, also known as, teradataml DataFrameColumn.
        * ColumnExpression specified in "sort_columns" should be from the same
          teradataml DataFrame as that of the ColumnExpression invoking the function.
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
        # Example 1: Calculate the cumulative sum of 'Feb' by sorting
        # data in column 'accounts' in ascending order.
        >>> df.assign(csum_feb=df.Feb.csum(df.accounts))
                      Feb    Jan    Mar    Apr    datetime  csum_feb
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     500.0
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     910.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017    1000.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     710.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     300.0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0
        >>>
        # Example 2: Calculate the cumulative sum of column 'Feb' by sorting
        # data in column 'Mar' in descending order and column 'accounts' in
        # ascending order.
        >>> df.assign(csum_feb=df.Feb.csum(sort_columns=[df.Mar.desc(), df.accounts]))
                      Feb    Jan    Mar    Apr    datetime  csum_feb
        accounts
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     610.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     910.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017    1000.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     700.0
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     410.0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0
    kurtosis
    teradataml.dataframe.sql.DataFrameColumn.kurtosis = kurtosis(self, distinct=False, **kwargs)
    DESCRIPTION:
        Function returns kurtosis value for a column.
        Kurtosis is the fourth moment of the distribution of the standardized
        (z) values. It is a measure of the outlier (rare, extreme observation)
        character of the distribution as compared with the normal, Gaussian
        distribution.
            * The normal distribution has a kurtosis of 0.
            * Positive kurtosis indicates that the distribution is more
              outlier-prone than the normal distribution.
            * Negative kurtosis indicates that the distribution is less
              outlier-prone than the normal distribution.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Default Values: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Get the kurtosis of the values in 'gpa' column.
        # Execute kurtosis() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> kurtosis_column = admissions_train.gpa.kurtosis()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, kurtosis_=kurtosis_column)
        >>> df
           kurtosis_
        0   4.052659
        >>>
        # Example 2: Get the kurtosis of the distinct values in 'gpa' column
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute kurtosis() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> kurtosis_column = admissions_train.gpa.kurtosis(distinct=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(kurtosis_=kurtosis_column)
        >>> df
          programming  kurtosis_
        0    Advanced   8.106762
        1      Novice   1.420745
        2    Beginner   5.733691
        >>>
    mavg
    teradataml.dataframe.sql.DataFrameColumn.mavg = mavg(width, sort_columns)
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
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
        * One must use DataFrame.assign() when using the aggregate functions on
          ColumnExpression, also known as, teradataml DataFrameColumn.
        * ColumnExpression specified in "sort_columns" should be from the same
          teradataml DataFrame as that of the ColumnExpression invoking the function.
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
        # Example 1: Calculate the moving average for column 'Feb' by sorting the data
        # on column accounts in ascending order, with window of size 2.
        >>> df.assign(mavg_feb=df.Feb.mavg(2, df.accounts))
                      Feb    Jan    Mar    Apr    datetime  mavg_feb
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     145.0
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     205.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     145.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     205.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     150.0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0
        >>>
        # Example 2: Calculate the moving average for column 'Feb' by sorting the data
        # on column 'Mar' in descending order and column 'accounts' in ascending order,
        # with window of size 2.
        >>> df.assign(mavg_feb=df.Feb.mavg(2, sort_columns=[df.Mar.desc(), df.accounts]))
                      Feb    Jan    Mar    Apr    datetime  mavg_feb
        accounts
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     200.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     150.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     150.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     145.0
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     205.0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0
    max
    teradataml.dataframe.sql.DataFrameColumn.max = max(self, distinct=False, **kwargs)
    DESCRIPTION:
        Function to get the maximum value for a column.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Default Values: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Get the maximum value in 'gpa' column.
        # Execute max() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> max_column = admissions_train.gpa.max()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, max_=max_column)
        >>> df
           max_
        0   4.0
        >>>
        # Example 2: Get the maximum value from the distinct values in 'gpa' column
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute max() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> max_column = admissions_train.gpa.max(distinct=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(max_=max_column)
        >>> df
          programming  max_
        0    Beginner   4.0
        1    Advanced   4.0
        2      Novice   4.0
        >>>
    mdiff
    teradataml.dataframe.sql.DataFrameColumn.mdiff = mdiff(width, sort_columns)
    DESCRIPTION:
        Computes the moving difference for the current row and the preceding
        "width"-1 rows in a partition, by sorting the rows according to
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
        sort_columns:
            Required Argument.
            Specifies the columns to use for sorting.
            Note:
                "sort_columns" does not support CLOB and BLOB type of
                columns.
            Types: str (or) ColumnExpression (or) List of strings(str)
                   or ColumnExpressions
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
        * One must use DataFrame.assign() when using the aggregate functions on
          ColumnExpression, also known as, teradataml DataFrameColumn.
        * ColumnExpression specified in "sort_columns" should be from the same
          teradataml DataFrame as that of the ColumnExpression invoking the function.
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
        # Example 1: Calculate the moving sum for column 'Feb' by sorting the data
        # on column accounts in ascending order, with window of size 2.
        >>> df.assign(msum_feb=df.Feb.msum(2, df.accounts))
                      Feb    Jan    Mar    Apr    datetime  msum_feb
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     290.0
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     410.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     290.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     410.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     300.0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0
        >>>
        # Example 2: Calculate the moving sum for column 'Feb' by sorting the data
        # on column 'Mar' in descending order and column 'accounts' in ascending order,
        # with window of size 2.
        >>> df.assign(msum_feb=df.Feb.msum(2, sort_columns=[df.Mar.desc(), df.accounts]))
                      Feb    Jan    Mar    Apr    datetime  msum_feb
        accounts
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     400.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     300.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     300.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     290.0
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     410.0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0
    mean
    teradataml.dataframe.sql.DataFrameColumn.mean = mean(self, distinct=False, **kwargs)
    DESCRIPTION:
        Function to get the average value for a column.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Default Values: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Get the mean value of 'gpa' column.
        # Execute mean() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> mean_column = admissions_train.gpa.mean()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, mean_=mean_column)
        >>> df
            mean_
        0  3.54175
        >>>
        # Example 2: Get the mean of the distinct values in 'gpa' column
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute mean() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> mean_column = admissions_train.gpa.mean(distinct=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(mean_=mean_column)
        >>> df
          programming     mean_
        0    Beginner  3.651818
        1    Advanced  3.592667
        2      Novice  3.294545
        >>>
    median
    teradataml.dataframe.sql.DataFrameColumn.median = median(self, distinct=False, **kwargs)
    DESCRIPTION:
        Function to get the median value for a column.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Default Values: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Get the median value of 'gpa' column.
        # Execute median() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> median_column = admissions_train.gpa.median()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, median_=median_column)
        >>> df
           min_
        0  3.69
        # Example 2: Get the median of the distinct values in 'gpa' column.
        # Execute median() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> median_column = admissions_train.gpa.median(distict=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, median_=median_column)
        >>> df
           median_
        0     3.69
        # Example 3: Get the median value in 'gpa' column for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute median() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> median_column = admissions_train.gpa.median()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(median_=median_column)
        >>> df
          programming  median_
        0    Advanced     3.76
        1      Novice     3.52
        2    Beginner     3.75
        >>>
    min
    teradataml.dataframe.sql.DataFrameColumn.min = min(self, distinct=False, **kwargs)
    DESCRIPTION:
        Function to get the minimum value for a column.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Default Values: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Get the minimum value in 'gpa' column.
        # Execute min() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> min_column = admissions_train.gpa.min()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, min_=min_column)
        >>> df
           min_
        0  1.87
        # Example 2: Get the minimum value from the distinct values in 'gpa' column
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute min() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> min_column = admissions_train.gpa.min(distinct=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(min_=min_column)
        >>> df
          programming  min_
        0    Advanced  1.98
        1      Novice  1.87
        2    Beginner  2.65
        >>>
    mlinreg
    teradataml.dataframe.sql.DataFrameColumn.mlinreg = mlinreg(width, sort_column)
    DESCRIPTION:
        Computes the moving linear regression for the current row and the preceding
        "width"-1 rows in a partition, by sorting the rows according to "sort_column".
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
            greater than 0 and less than or equal to 4096.
            Types: int
        sort_column:
            Required Argument.
            Specifies the column to use for sorting.
            Note:
                "sort_column" does not support CLOB and BLOB type of
                columns.
            Types: str (or) ColumnExpression
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
        * One must use DataFrame.assign() when using the aggregate functions on
          ColumnExpression, also known as, teradataml DataFrameColumn.
        * ColumnExpression specified in "sort_columns" should be from the same
          teradataml DataFrame as that of the ColumnExpression invoking the function.
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
        # Example 1: Calculate the moving linear regression for column 'id' by
        # sorting the data on column accounts in descending order, with window of size 3.
        >>> df.assign(mlinreg_id=df.id.mlinreg(3, df.id))
           masters   gpa     stats programming  admitted  mlinreg_id
        id
        3       no  3.70    Novice    Beginner         1         3.0
        5       no  3.44    Novice      Novice         0         5.0
        6      yes  3.50  Beginner    Advanced         1         6.0
        7      yes  2.33    Novice      Novice         1         7.0
        9       no  3.82  Advanced    Advanced         1         9.0
        10      no  3.71  Advanced    Advanced         1        10.0
        8       no  3.60  Beginner    Advanced         1         8.0
        4      yes  3.50  Beginner      Novice         1         4.0
        2      yes  3.76  Beginner    Beginner         0         NaN
        1      yes  3.95  Beginner    Beginner         0         NaN
        >>>
    msum
    teradataml.dataframe.sql.DataFrameColumn.msum = msum(width, sort_columns)
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
    RAISES:
        TeradataMlException, TypeError
    RETURNS:
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
        * One must use DataFrame.assign() when using the aggregate functions on
          ColumnExpression, also known as, teradataml DataFrameColumn.
        * ColumnExpression specified in "sort_columns" should be from the same
          teradataml DataFrame as that of the ColumnExpression invoking the function.
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
        # Example 1: Calculate the moving sum for column 'Feb' by sorting the data
        # on column accounts in ascending order, with window of size 2.
        >>> df.assign(msum_feb=df.Feb.msum(2, df.accounts))
                      Feb    Jan    Mar    Apr    datetime  msum_feb
        accounts
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     290.0
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     410.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     290.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     410.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     300.0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0
        >>>
        # Example 2: Calculate the moving sum for column 'Feb' by sorting the data
        # on column 'Mar' in descending order and column 'accounts' in ascending order,
        # with window of size 2.
        >>> df.assign(msum_feb=df.Feb.msum(2, sort_columns=[df.Mar.desc(), df.accounts]))
                      Feb    Jan    Mar    Apr    datetime  msum_feb
        accounts
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017     400.0
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017     300.0
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017     300.0
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017     290.0
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017     410.0
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017     210.0
    percentile
    teradataml.dataframe.sql.DataFrameColumn.percentile = percentile(self, percentile, distinct=False, interpolation='LINEAR', as_time_series_aggregate=False, **kwargs)
    DESCRIPTION:
        Function to get the percentile values for a column.
    PARAMETERS:
        percentile:
            Required Argument.
            Specifies the desired percentile value to calculate.
            It should be between 0 and 1, both inclusive.
            Types: float
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Note: "distinct" is insignificant if percentile is calculated
                  as regular aggregate i.e., "as_time_series_aggregate" is
                  set to False.
            Default Values: False
            Types: bool
        interpolation:
            Optional Argument.
            Specifies the interpolation type to use to interpolate the result value when the
            desired result lies between two data points.
            The desired result lies between two data points, i and j, where i<j. In this case,
            the result is interpolated according to the permitted values.
            Permitted Values for time series aggregate:
                * LINEAR: Linear interpolation.
                    The result value is computed using the following equation:
                        result = i + (j - i) * (di/100)MOD 1
                    Specify by passing "LINEAR" as string to this parameter.
                * LOW: Low value interpolation.
                    The result value is equal to i.
                    Specify by passing "LOW" as string to this parameter.
                * HIGH: High value interpolation.
                    The result value is equal to j.
                    Specify by passing "HIGH" as string to this parameter.
                * NEAREST: Nearest value interpolation.
                    The result value is i if (di/100 )MOD 1 <= .5; otherwise, it is j.
                    Specify by passing "NEAREST" as string to this parameter.
                * MIDPOINT: Midpoint interpolation.
                     The result value is equal to (i+j)/2.
                     Specify by passing "MIDPOINT" as string to this parameter.
            Permitted Values for regular aggregate:
                * LINEAR: Linear interpolation.
                    Percentile is calculated after doing linear interpolation.
                * None:
                    Percentile is calculated with no interpolation.
            Default Values: "LINEAR"
            Types: str
        as_time_series_aggregate:
            Optional Argument.
            Specifies a flag that decides whether percentiles are being calculated
            as regular aggregate or time series aggregate. When it is set to False, it'll
            be executed as regular aggregate, if set to True; then it is used as time series
            aggregate.
            Default Values: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["admissions_train", "ocean_buoys"])
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
        # Create a DataFrame on 'ocean_buoys' table.
        >>> ocean_buoys = DataFrame("ocean_buoys")
        >>> ocean_buoys
                               TD_TIMECODE  salinity  temperature
        buoyid
        1       2014-01-06 09:02:25.122200        55         78.0
        44      2014-01-06 10:00:24.333300        55         43.0
        44      2014-01-06 10:00:25.122200        55         43.0
        2       2014-01-06 21:01:25.122200        55         80.0
        2       2014-01-06 21:03:25.122200        55         82.0
        0       2014-01-06 08:00:00.000000        55         10.0
        0       2014-01-06 08:08:59.999999        55          NaN
        0       2014-01-06 08:09:59.999999        55         99.0
        2       2014-01-06 21:02:25.122200        55         81.0
        44      2014-01-06 10:00:24.000000        55         43.0
        >>>
        # Example 1: Calculate the 25th percentile of temperature in ocean_buoys table,
        #            with LINEAR interpolation.
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="10m", value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.assign(True, temperature_percentile_=ocean_buoys_grpby1.temperature.percentile(0.25))
           temperature_percentile_
        0                       43
        >>>
        # Example 2: Calculate the 35th percentile of gpa in admissions_train table,
        #            with LINEAR interpolation.
        >>> admissions_train_grpby = admissions_train.groupby("admitted")
        >>> admissions_train_grpby.assign(True, percentile_cont_=admissions_train_grpby.gpa.percentile(0.35))
           admitted  percentile_cont_
        0         0             3.460
        1         1             3.565
        >>>
        # Example 3: Calculate the 45th percentile of gpa in admissions_train table,
        #            with no interpolation.
        >>> admissions_train_grpby = admissions_train.groupby("admitted")
        >>> admissions_train_grpby.assign(True, percentile_disc_=admissions_train_grpby.gpa.percentile(0.35, interpolation=None))
           admitted  percentile_disc_
        0         0              3.46
        1         1              3.57
        >>>
    regr_avgx
    teradataml.dataframe.sql.DataFrameColumn.regr_avgx = regr_avgx(expression)
    DESCRIPTION:
        Function returns the mean of the independent variable for all
        non-null data pairs of the dependent and an independent variable arguments.
        The function considers ColumnExpression as a dependent variable and
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
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Calculate the mean of the "gpa" column (independent variable) with
        #            respect to values in "admitted" column (dependent variable).
        # Execute regr_avgx() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_avgx_column = admissions_train.admitted.regr_avgx(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, regr_avgx_=regr_avgx_column)
        >>> df
           regr_avgx_
        0     3.54175
        >>>
        # Example 2: Calculate the mean of the "gpa" column (independent variable) with
        #            respect to values in "admitted" column (dependent variable) for each 
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute regr_avgx() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_avgx_column = admissions_train.admitted.regr_avgx(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(regr_avgx_=regr_avgx_column)
        >>> df
          programming  regr_avgx_
        0    Advanced    3.615625
        1      Novice    3.294545
        2    Beginner    3.660000
        >>>
    regr_avgy
    teradataml.dataframe.sql.DataFrameColumn.regr_avgy = regr_avgy(expression)
    DESCRIPTION:
        Function returns the mean of the dependent variable for all non-null data
        pairs of the dependent and independent variable arguments. The function
        considers ColumnExpression as an independent variable and "expression" as
        a dependent variable.
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
        ColumnExpression, also known as, teradataml DataFrameColumn
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Calculate the mean of the "admitted" column (dependent variable) with 
        #            respect to the values in "gpa" column (independent variable).
        # Execute regr_avgy() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_avgy_column = admissions_train.admitted.regr_avgy(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, regr_avgy_=regr_avgy_column)
        >>> df
           regr_avgy_
        0        0.65
        >>>
        # Example 2: Calculate the mean of the "admitted" column (dependent variable) with
        #            respect to the values in "gpa" column (independent variable) for each
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute regr_avgy() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_avgy_column = admissions_train.admitted.regr_avgy(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(regr_avgy_=regr_avgy_column)
        >>> df
          programming  regr_avgy_
        0    Advanced    0.812500
        1      Novice    0.727273
        2    Beginner    0.384615
        >>>
    regr_count
    teradataml.dataframe.sql.DataFrameColumn.regr_count = regr_count(expression)
    DESCRIPTION:
        Function returns the count of all non-null data pairs of the dependent and
        independent variable arguments. The function considers ColumnExpression as
        a dependent variable and "expression" as an independent variable.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Calculate the total number of non-null pairs between "gpa" column (independent variable)
        #            and "admitted" column (dependent variable).
        # Execute regr_count() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_count_column = admissions_train.admitted.regr_count(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, regr_count_=regr_count_column)
        >>> df
           regr_count_
        0           40
        >>>
        # Example 2: Calculate the total number of non-null pairs between "gpa" column (independent variable)
        #            and "admitted" column (dependent variable) for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute regr_count() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_count_column = admissions_train.admitted.regr_count(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.groupby("programming").assign(regr_count_=regr_count_column)
        >>> df
          programming  regr_count_
        0    Advanced           16
        1      Novice           11
        2    Beginner           13
        >>>
    regr_intercept
    teradataml.dataframe.sql.DataFrameColumn.regr_intercept = regr_intercept(expression)
    DESCRIPTION:
        Function returns the intercept of the univariate linear regression line
        through all non-null data pairs of the dependent and independent variable
        arguments. The intercept is the point at which the regression line through
        the non-null data pairs in the sample intersects the ordinate, or y-axis,
        of the graph. The plot of the linear regression on the variables is used to
        predict the behavior of the dependent variable from the change in the
        independent variable. There can be a strong nonlinear relationship between
        independent and dependent variables, and the computation of the simple linear
        regression between such variable pairs does not reflect such a relationship.
        The function considers ColumnExpression as a dependent variable and "expression"
        as an independent variable.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable is something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        ColumnExpression, also known as, teradataml DataFrameColumn
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Calculate the intercept of the "gpa" column (independent variable) with
        #            "admitted" column (dependent variable).
        # Execute regr_intercept() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_intercept_column = admissions_train.admitted.regr_intercept(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, regr_intercept_=regr_intercept_column)
        >>> df
           regr_intercept_
        0         0.724144
        >>>
        # Example 2: Calculate the intercept of the "gpa" column (independent variable) with
        #            "admitted" column (dependent variable) for each
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute regr_intercept() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_intercept_column = admissions_train.admitted.regr_intercept(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.groupby("programming").assign(regr_intercept_=regr_intercept_column)
        >>> df
          programming  regr_intercept_
        0    Advanced        -0.626557
        1      Novice         1.000091
        2    Beginner         2.566361
        >>>
    regr_r2
    teradataml.dataframe.sql.DataFrameColumn.regr_r2 = regr_r2(expression)
    DESCRIPTION:
        Function returns the coefficient of determination for all non-null data
        pairs of the dependent and independent variable arguments. The function
        considers ColumnExpression as a dependent variable and "expression" as an
        independent variable.
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
        ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Calculate the coefficient of determination for the values forming a pair between
        #            "gpa" column (independent variable) and "admitted" column (dependent variable).
        # Execute regr_r2() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_r2_column = admissions_train.admitted.regr_r2(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, regr_r2_=regr_r2_column)
        >>> df
           regr_r2_
        0  0.000496
        >>>
        # Example 2: Calculate the coefficient of determination for the values forming a pair between
        #            "gpa" column (independent variable) and "admitted" column (dependent variable)
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute regr_r2() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_r2_column = admissions_train.admitted.regr_r2(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.groupby("programming").assign(regr_r2_=regr_r2_column)
        >>> df
          programming  regr_r2_
        0    Advanced  0.237888
        1      Novice  0.013146
        2    Beginner  0.174361
        >>>
    regr_slope
    teradataml.dataframe.sql.DataFrameColumn.regr_slope = regr_slope(expression)
    DESCRIPTION:
        Function returns the slope of the univariate linear regression line through
        all non-null data pairs of the dependent and an independent variable arguments.
        When function is executed, "expression" is treated as an independent variable
        and dependent variable is ColumnExpression.
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
        ColumnExpression, also known as, teradataml DataFrameColumn
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Calculate the slope of the univariate linear regression line through
        #            the data pairs of values in "gpa" column (independent variable)
        #            and "admitted" column (dependent variable).
        # Execute regr_slope() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_slope_column = admissions_train.admitted.regr_slope(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, regr_slope_=regr_slope_column)
        >>> df
           regr_slope_
        0    -0.020934
        >>>
        # Example 2: Calculate the slope of the univariate linear regression line through
        #            the data pairs of values in "gpa" column (independent variable)
        #            and "admitted" column (dependent variable) for each
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute regr_slope() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_slope_column = admissions_train.admitted.regr_slope(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.groupby("programming").assign(regr_slope_=regr_slope_column)
        >>> df
          programming  regr_slope_
        0    Advanced     0.398010
        1      Novice    -0.082809
        2    Beginner    -0.596105
        >>>
    regr_sxx
    teradataml.dataframe.sql.DataFrameColumn.regr_sxx = regr_sxx(expression)
    DESCRIPTION:
        Function returns the sum of the squares of the independent variable
        expression for all non-null data pairs of dependent and an independent
        variable arguments. When function is executed, "expression" is treated as
        an independent variable and dependent variable is ColumnExpression
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
        ColumnExpression, also known as, teradataml DataFrameColumn.
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
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
        # Example 1: Calculate the sum of the squares of the values in "gpa" column
        #            (independent variable) with respect to values in "admitted"
        #            column (dependent variable).
        # Execute regr_sxx() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_sxx_column = admissions_train.admitted.regr_sxx(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, regr_sxx_=regr_sxx_column)
        >>> df
           regr_sxx_
        0    10.294177
        >>>
        # Example 2: Calculate the sum of the squares of the values in "gpa" column
        #            (independent variable) with respect to values in "admitted"
        #            column (dependent variable) for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute regr_sxx() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_sxx_column = admissions_train.admitted.regr_sxx(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.groupby("programming").assign(regr_sxx_=regr_sxx_column)
        >>> df
          programming  regr_sxx_
        0    Advanced   3.660394
        1      Novice   4.182673
        2    Beginner   1.509800
        >>>
    regr_sxy
    teradataml.dataframe.sql.DataFrameColumn.regr_sxy = regr_sxy(expression)
    DESCRIPTION:
        Function returns the sum of the products of the independent variable and the
        dependent variable for all non‑null data pairs of the dependent and independent
        variable arguments. When function is executed, "expression" is treated as an
        independent variable and dependent variable is ColumnExpression.
        Note:
            When there are fewer than two non-null data point pairs in the
            data used for the computation, the function returns None.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a column or name of the column or a
            literal representing an independent variable for the regression.
            An independent variable something that is varied under your control
            to test the behavior of another variable.
            Types: ColumnExpression OR int OR float OR str
    RETURNS:
        ColumnExpression, also known as, teradataml DataFrameColumn.
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
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
        # Example 1: Calculate the sum of the products of the "gpa" column (independent variable)
        #            with respect to values in "admitted" column (dependent variable).
        # Execute regr_sxy() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_sxy_column = admissions_train.admitted.regr_sxy(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, regr_sxy_=regr_sxy_column)
        >>> df
           regr_sxy_
        0    -0.2155
        >>>
        # Example 2: Calculate the sum of the products of the "gpa" column (independent variable)
        #            with respect to values in "admitted" column (dependent variable) for each
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute regr_sxy() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_sxy_column = admissions_train.admitted.regr_sxy(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.groupby("programming").assign(regr_sxy_=regr_sxy_column)
        >>> df
          programming  regr_sxy_
        0    Advanced   1.456875
        1      Novice  -0.346364
        2    Beginner  -0.900000
        >>>
    regr_syy
    teradataml.dataframe.sql.DataFrameColumn.regr_syy = regr_syy(expression)
    DESCRIPTION:
        Function returns the sum of the squares of the dependent variable
        expression for all non-null data pairs of dependent and an independent
        variable arguments. When function is executed, "expression" is treated
        as an independent variable and dependent variable is ColumnExpression.
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
        ColumnExpression, also known as, teradataml DataFrameColumn
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Calculate the sum of the squares of the values in "admitted" column
        #            (dependent variable) with respect to values in "gpa"
        #            column (independent variable).
        # Execute regr_syy() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_syy_column = admissions_train.admitted.regr_syy(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, regr_syy_=regr_syy_column)
        >>> df
           regr_syy_
        0        9.1
        >>>
        # Example 2: Calculate the sum of the squares of the values in "admitted" column
        #            (dependent variable) with respect to values in "gpa"
        #            column (independent variable) for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute regr_syy() using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> regr_syy_column = admissions_train.admitted.regr_syy(admissions_train.gpa)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df
          programming  regr_syy_
        0    Advanced   2.437500
        1      Novice   2.181818
        2    Beginner   3.076923
        >>>
    skew
    teradataml.dataframe.sql.DataFrameColumn.skew = skew(self, distinct=False, **kwargs)
    DESCRIPTION:
        Function to get the skewness of the distribution for a column.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Default Values: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Calculate the skewness of the distribution for values in 'gpa' column.
        # Execute skew() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> skew_column = admissions_train.gpa.skew()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, skew_=skew_column)
        >>> df
              skew_
        0 -2.058969
        >>>
        # Example 2: Calculate the skewness of the distribution for distinct values in
        #            'gpa' column for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute skew() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> skew_column = admissions_train.gpa.skew(distinct=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(skew_=skew_column)
        >>> df
          programming     skew_
        0    Beginner -2.197710
        1    Advanced -2.647604
        2      Novice -1.459620
        >>>
    std
    teradataml.dataframe.sql.DataFrameColumn.std = std(self, distinct=False, population=False, **kwargs)
    DESCRIPTION:
        Function to get the sample or population standard deviation for values in a column.
        The standard deviation is the second moment of a distribution.
            * For a sample, it is a measure of dispersion from the mean of that sample.
            * For a population, it is a measure of dispersion from the mean of that population.
        The computation is more conservative for the population standard deviation
        to minimize the effect of outliers on the computed value.
        Note:
            1. When there are fewer than two non-null data points in the sample used
               for the computation, then std returns None.
            2. Null values are not included in the result computation.
            3. If data represents only a sample of the entire population for the
               column, Teradata recommends to calculate sample standard deviation,
               otherwise calculate population standard deviation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Default Values: False
            Types: bool
        population:
            Optional Argument.
            Specifies whether to calculate standard deviation on entire population or not.
            Set this argument to True only when the data points represent the complete
            population. If your data represents only a sample of the entire population for the
            column, then set this variable to False, which will compute the sample standard
            deviation. As the sample size increases, even though the values for sample
            standard deviation and population standard deviation approach the same number,
            you should always use the more conservative sample standard deviation calculation,
            unless you are absolutely certain that your data constitutes the entire population
            for the column.
            Default Value: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Get the sample standard deviation for values in 'gpa' column.
        # Execute std() function on teradataml DataFrameColumn to generate the ColumnExpression.
        >>> std_column = admissions_train.gpa.std()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, std_=std_column)
        >>> df
               std_
        0  0.513764
        >>>
        # Example 2: Get the population standard deviation for values in 'gpa' column.
        # Execute std() function on teradataml DataFrameColumn to generate the ColumnExpression.
        # To calculate population standard deviation we must set population=True.
        >>> std_column = admissions_train.gpa.std(population=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, std_=std_column)
        >>> df
               std_
        0  0.507301
        >>>
        # Example 3: Get the sample standard deviation for distinct values in 'gpa' column
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute std() function on teradataml DataFrameColumn to generate the ColumnExpression.
        # We will consider DISTINCT values for the columns while calculating the standard deviation value.
        >>> std_column = admissions_train.gpa.std(distinct=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(std_=std_column)
        >>> df
          programming      std_
        0    Beginner  0.372151
        1    Advanced  0.502415
        2      Novice  0.646736
        >>>
    sum
    teradataml.dataframe.sql.DataFrameColumn.sum = sum(self, distinct=False, **kwargs)
    DESCRIPTION:
        Function to get the sum of values in a column.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
            Default Values: False
            Types: bool
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Calculate the sum of the values in 'gpa' column.
        # Execute sum() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> sum_column = admissions_train.gpa.sum()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, sum_=sum_column)
        >>> df
            sum_
        0  141.67
        >>>
        # Example 2: Calculate the sum of the distinct values in'gpa' column
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute sum() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> sum_column = admissions_train.gpa.sum(distinct=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(sum_=sum_column)
        >>> df
          programming   sum_
        0    Beginner  40.17
        1    Advanced  53.89
        2      Novice  36.24
        >>>
    var
    teradataml.dataframe.sql.DataFrameColumn.var = var(self, distinct=False, population=False, **kwargs)
    DESCRIPTION:
        Returns sample or population variance for values in a column.
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
            Specifies a flag that decides whether to consider duplicate values in
            a column or not.
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
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression, also known as, teradataml DataFrameColumn.
    NOTES:
         * One must use DataFrame.assign() when using the aggregate functions on
           ColumnExpression, also known as, teradataml DataFrameColumn.
         * One should always use "drop_columns=True" in DataFrame.assign(), while
           running the aggregate operation on teradataml DataFrame.
         * "drop_columns" argument in DataFrame.assign() is ignored, when aggregate
           function is operated on DataFrame.groupby().
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
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
        # Example 1: Get the sample variance for values in 'gpa' column.
        # Execute var() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> var_column = admissions_train.gpa.var()
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, var_=var_column)
        >>> df
               var_
        0  0.263953
        # Example 2: Get the population variance for values in 'gpa' column.
        # Execute var() function on teradataml DataFrameColumn to generate the ColumnExpression.
        # To calculate population variance we must set population=True.
        >>> var_column = admissions_train.gpa.var(population=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df = admissions_train.assign(True, var_=var_column)
        >>> df
               var_
        0  0.257354
        >>>
        # Example 3: Get the sample variance for distinct values in 'gpa' column.
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        # Execute var() function using teradataml DataFrameColumn to generate the ColumnExpression.
        >>> var_column = admissions_train.gpa.var(distinct=True)
        # Pass the generated ColumnExpression to DataFrame.assign(), to run and produce the result.
        >>> df=admissions_train.groupby("programming").assign(var_=var_column)
        >>> df
          programming      var_
        0    Advanced  0.252421
        1      Novice  0.418267
        2    Beginner  0.138496
        >>>
    DataFrameColumn Arithmetic Functions
    abs
    teradataml.dataframe.sql.DataFrameColumn.abs = abs()
    DESCRIPTION:
        Function computes the absolute value of the column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported. For more information on implicit type conversion,
           see Teradata Vantage™ Data Types and Literals.
        2. Unsupported column types:
            a. BYTE or VARBYTE
            b. LOBs (BLOB or CLOB)
            c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Compute the absolute value for the "gpa" column and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.abs())
        >>> print(res)
           masters   gpa     stats programming  admitted   col
        id
        3       no  3.70    Novice    Beginner         1  3.70
        4      yes  3.50  Beginner      Novice         1  3.50
        2      yes  3.76  Beginner    Beginner         0  3.76
        1      yes  3.95  Beginner    Beginner         0  3.95
        # Example 2: Executed abs() function on "gpa" column and filtered computed values
        #            which are equal to 3.95.
        >>> print(df[df.gpa.abs() == 3.95])
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
    add
    teradataml.dataframe.sql.DataFrameColumn.add = add(self, other)
    Compute the addition between two ColumnExpressions.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Add 100 to the expenditure amount and assign the final amount
        #            to new column 'total_expenditure'.
        >>> df.assign(total_expenditure=df.expenditure.add(100))
           start_time_column end_time_column  expenditure  income  investment  total_expenditure
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0              534.0
        2           67/06/30        07/07/10        421.0   465.0       179.0              521.0
        1           67/06/30        07/07/10        415.0   451.0       180.0              515.0
        5           67/06/30        07/07/10        459.0   509.0       211.0              559.0
        4           67/06/30        07/07/10        448.0   493.0       192.0              548.0
        # Example 2: Filter the rows where the income left after the investment is more than 300.
        >>> df[df.income.sub(df.investment) > 300]
           start_time_column end_time_column  expenditure  income  investment
        id
        4           67/06/30        07/07/10        448.0   493.0       192.0
    cbrt
    teradataml.dataframe.sql.DataFrameColumn.cbrt = cbrt(self)
    DESCRIPTION:
        Function to compute cube root of the column.
        Note:
            Function computes cuberoot for column only when it's values are positive.
            Else, the function fails.
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml","titanic")
        # Create a DataFrame on 'titanic' table.
        >>> titanic = DataFrame.from_table('titanic')
        >>> df = titanic.select(["passenger", "age", "fare"])
        >>> print(df)
                    age      fare
        passenger
        326        36.0  135.6333
        183         9.0   31.3875
        652        18.0   23.0000
        265         NaN    7.7500
        530        23.0   11.5000
        122         NaN    8.0500
        591        35.0    7.1250
        387         1.0   46.9000
        734        23.0   13.0000
        795        25.0    7.8958
        >>>
        # Example 1: Compute cuberoot values in "fare" and pass it as input to
        #           DataFrame.assign().
        >>> cbrt_df = df.assign(fare_cbrt=df.fare.cbrt())
        >>> print(cbrt_df)
                    age      fare  fare_cbrt
        passenger
        326        36.0  135.6333   5.137937
        183         9.0   31.3875   3.154416
        652        18.0   23.0000   2.843867
        40         14.0   11.2417   2.240151
        774         NaN    7.2250   1.933211
        366        30.0    7.2500   1.935438
        509        28.0   22.5250   2.824153
        795        25.0    7.8958   1.991279
        61         22.0    7.2292   1.933586
        469         NaN    7.7250   1.976816
        >>>
    ceil
    teradataml.dataframe.sql.DataFrameColumn.ceil = ceil()
    DESCRIPTION:
        Function returns the smallest integer value that is not less than the value in the column.
    ALTERNATE NAME:
        ceiling
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Compute the ceil value for the "gpa" column and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.ceil())
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  4.0
        4      yes  3.50  Beginner      Novice         1  4.0
        2      yes  3.76  Beginner    Beginner         0  4.0
        1      yes  3.95  Beginner    Beginner         0  4.0
        # Example 2: Executed ceil() function on "gpa" column and filtered computed
        #            values which are equal to 4.0.
        >>> print(df[df.gpa.ceil() == 4.0])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    ceiling
    teradataml.dataframe.sql.DataFrameColumn.ceiling = ceiling()
    DESCRIPTION:
        Function returns the smallest integer value that is not less than the value in the column.
    ALTERNATE NAME:
        ceil
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Compute the ceiling value for the "gpa" column and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.ceiling())
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  4.0
        4      yes  3.50  Beginner      Novice         1  4.0
        2      yes  3.76  Beginner    Beginner         0  4.0
        1      yes  3.95  Beginner    Beginner         0  4.0
        # Example 2: Executed ceiling() function on "gpa" column and filtered computed
        #            values which are equal to 4.0.
        >>> print(df[df.gpa.ceiling() == 4.0])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    degrees
    teradataml.dataframe.sql.DataFrameColumn.degrees = degrees()
    DESCRIPTION:
        Converts the radians value from the column to degrees.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported. For more information on implicit type conversion,
           see Teradata Vantage™ Data Types and Literals.
        2. Unsupported column types:
            a. BYTE or VARBYTE
            b. LOBs (BLOB or CLOB)
            c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculate degrees for values in "admitted" column and pass as
        #            input to DataFrame.assign().
        >>> res = df.assign(col = df.admitted.degrees())
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  57.29578
        4      yes  3.50  Beginner      Novice         1  57.29578
        2      yes  3.76  Beginner    Beginner         0   0.00000
        1      yes  3.95  Beginner    Beginner         0   0.00000
        # Example 2: Executed degrees() on "admitted" column and filtered computed values
        #            which are greater than 57.
        >>> print(df[df.admitted.degrees() > 57])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
        3       no  3.7    Novice    Beginner         1
    div
    teradataml.dataframe.sql.DataFrameColumn.div = div(self, other)
    Compute the division between two ColumnExpressions.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Calculate the percent of investment of income and assign the
        #            divided amount to new column 'percentage_investment'.
        >>> df.assign(percentage_investment=(df.investment.mul(100)).truediv(df.income))
           start_time_column end_time_column  expenditure  income  investment  percentage_investment
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0              38.144330
        2           67/06/30        07/07/10        421.0   465.0       179.0              38.494624
        1           67/06/30        07/07/10        415.0   451.0       180.0              39.911308
        5           67/06/30        07/07/10        459.0   509.0       211.0              41.453831
        4           67/06/30        07/07/10        448.0   493.0       192.0              38.945233
        # Example 2: Filter out the rows after diving income amount by 2 is less than 240.
        >>> df[(df.income.div(2)) < 240]
           start_time_column end_time_column  expenditure  income  investment
        id
        2           67/06/30        07/07/10        421.0   465.0       179.0
        1           67/06/30        07/07/10        415.0   451.0       180.0
    exp
    teradataml.dataframe.sql.DataFrameColumn.exp = exp(column_expression)
    DESCRIPTION:
        Function raises e (the base of natural logarithms) to the power of the value in the column, where e = 2.71828182845905.
    NOTES:
       1. If the type of the column is not FLOAT, column values are converted to FLOAT
          based on implicit type conversion rules. If the value cannot be converted, an
          error is reported. For more information on implicit type conversion,
          see Teradata Vantage™ Data Types and Literals.
       2. Unsupported column types:
          a. BYTE or VARBYTE
          b. LOBs (BLOB or CLOB)
          c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Execute radians() function and pass as input to DataFrame.assign().
        >>> res = df.assign(col = df.admitted.exp())
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  2.718282
        4      yes  3.50  Beginner      Novice         1  2.718282
        2      yes  3.76  Beginner    Beginner         0  1.000000
        1      yes  3.95  Beginner    Beginner         0  1.000000
        # Example 2: Executed exp() function on "admitted" column and filtered computed values
        #            which are greater than 2.7.
        >>> print(df[df.admitted.exp() > 2.7])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
        3       no  3.7    Novice    Beginner         1
    ﬂoor
    teradataml.dataframe.sql.DataFrameColumn.ﬂoor = ﬂoor()
    DESCRIPTION:
        Function returns the largest integer equal to or less than the value in the column of DataFrame.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates floor() value for the "gpa" column and pass as input to
        #            DataFrame.assign().
        >>> res = df.assign(col = df.gpa.floor())
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  3.0
        4      yes  3.50  Beginner      Novice         1  3.0
        2      yes  3.76  Beginner    Beginner         0  3.0
        1      yes  3.95  Beginner    Beginner         0  3.0
        # Example 2: Executed floor() function on "gpa" column and filtered computed values
        #            which are equal to 3.0.
        >>> print(df[df.gpa.floor() == 3.0])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    ﬂoordiv
    teradataml.dataframe.sql.DataFrameColumn.ﬂoordiv = ﬂoordiv(self, other)
    Compute the floor-division between two ColumnExpressions.
    PARAMETRS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Calculate the percent of investment of income and assign the
        #            final amount to new column 'percentage_investment'.
        >>> df.assign(percentage_investment=(df.investment.mul(100)).floordiv(df.income))
           start_time_column end_time_column  expenditure  income  investment  percentage_investment
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0              38.144330
        2           67/06/30        07/07/10        421.0   465.0       179.0              38.494624
        1           67/06/30        07/07/10        415.0   451.0       180.0              39.911308
        5           67/06/30        07/07/10        459.0   509.0       211.0              41.453831
        4           67/06/30        07/07/10        448.0   493.0       192.0              38.945233
        # Example 2: Filter out the rows after diving income amount by 2 is less than 240.
        >>> df[(df.income.floordiv(2)) < 240]
           start_time_column end_time_column  expenditure  income  investment
        id
        2           67/06/30        07/07/10        421.0   465.0       179.0
        1           67/06/30        07/07/10        415.0   451.0       180.0
    hex
    teradataml.dataframe.sql.DataFrameColumn.hex = hex(self)
    DESCRIPTION:
        Function to compute the Hexadecimal from decimal for the column.
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml","titanic")
        # Create a DataFrame on 'titanic' table.
        >>> titanic = DataFrame.from_table('titanic')
        >>> df = titanic.select(["passenger", "age", "fare"])
        >>> print(df)
                    age      fare
        passenger
        326        36.0  135.6333
        183         9.0   31.3875
        652        18.0   23.0000
        265         NaN    7.7500
        530        23.0   11.5000
        122         NaN    8.0500
        591        35.0    7.1250
        387         1.0   46.9000
        734        23.0   13.0000
        795        25.0    7.8958
        >>>
        # Example 1: Converts values in "age" decimal to hexadecimal and pass it as input to
        #            DataFrame.assign().
        >>> hex_df = df.assign(age_in_hex=df.age.hex())
        >>> print(hex_df)
                    age    fare age_in_hex
        passenger
        530        23.0  11.500         17
        591        35.0   7.125         23
        387         1.0  46.900          1
        856        18.0   9.350         12
        244        22.0   7.125         16
        713        48.0  52.000         30
        448        34.0  26.550         22
        122         NaN   8.050       None
        734        23.0  13.000         17
        265         NaN   7.750       None
        >>>
    hypot
    teradataml.dataframe.sql.DataFrameColumn.hypot = hypot(self, other)
    DESCRIPTION:
        Function to compute the hypotenuse.
    PARAMETERS:
        other:
            Required Argument.
            Specifies DataFrame column for calculation of hypotenuse.
            Types: int or float or str or ColumnExpression
    Returns:
        ColumnExpression
    Examples:
        # Load the data to run the example.
        >>> load_example_data("teradataml","titanic")
        # Create a DataFrame on 'titanic' table.
        >>> titanic = DataFrame.from_table('titanic')
        >>> df = titanic.select(["passenger", "age", "fare"])
        >>> print(df)
                    age      fare
        passenger
        326        36.0  135.6333
        183         9.0   31.3875
        652        18.0   23.0000
        265         NaN    7.7500
        530        23.0   11.5000
        122         NaN    8.0500
        591        35.0    7.1250
        387         1.0   46.9000
        734        23.0   13.0000
        795        25.0    7.8958
        >>>
        # Example 1: compute hypotenuse of two columns fare and age.
        >>> hypot_df = df.assign(hypot_column=df.fare.hypot(titanic.age))
        >>> print(hypot_df)
                    age      fare  hypot_column
        passenger
        326        36.0  135.6333    140.329584
        183         9.0   31.3875     32.652338
        652        18.0   23.0000     29.206164
        40         14.0   11.2417     17.954827
        774         NaN    7.2250           NaN
        366        30.0    7.2500     30.863611
        509        28.0   22.5250     35.935715
        795        25.0    7.8958     26.217240
        61         22.0    7.2292     23.157317
        469         NaN    7.7250           NaN
        >>>
    isﬁnite
    teradataml.dataframe.sql.DataFrameColumn.isﬁnite = isﬁnite(self)
    DESCRIPTION:
        Function evaluates a variable or expression to determine if
        it is a finite floating value. A finite floating value is not
        a NaN (Not a Number) value and is not an infinity value.
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml","titanic")
        # Create a DataFrame on 'titanic' table.
        >>> titanic = DataFrame.from_table('titanic')
        >>> df = titanic.select(["passenger", "age", "fare"])
        >>> print(df)
                    age      fare
        passenger
        326        36.0  135.6333
        183         9.0   31.3875
        652        18.0   23.0000
        40         14.0   11.2417
        774         NaN    7.2250
        366        30.0    7.2500
        509        28.0   22.5250
        795        25.0    7.8958
        61         22.0    7.2292
        469         NaN    7.7250
        >>>
        # Example 1: Find whether 'fare' column contains finite values or not.
        >>> finite_df = df.assign(finiteornot = df.fare.isfinite())
        >>> print(finite_df)
                    age    fare finiteornot
        passenger
        530        23.0  11.500           1
        591        35.0   7.125           1
        387         1.0  46.900           1
        856        18.0   9.350           1
        244        22.0   7.125           1
        713        48.0  52.000           1
        448        34.0  26.550           1
        122         NaN   8.050           1
        734        23.0  13.000           1
        265         NaN   7.750           1
        >>>
    isinf
    teradataml.dataframe.sql.DataFrameColumn.isinf = isinf(self)
    DESCRIPTION:
        Function evaluates a variable or expression to determine if the
        floating-point argument is an infinite number. This function determines
        if a database table contains positive or negative infinite values.
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml","titanic")
        # Create a DataFrame on 'titanic' table.
        >>> titanic = DataFrame.from_table('titanic')
        >>> df = titanic.select(["passenger", "age", "fare"])
        >>> print(df)
                    age      fare
        passenger
        326        36.0  135.6333
        183         9.0   31.3875
        652        18.0   23.0000
        40         14.0   11.2417
        774         NaN    7.2250
        366        30.0    7.2500
        509        28.0   22.5250
        795        25.0    7.8958
        61         22.0    7.2292
        469         NaN    7.7250
        >>>
        # Example 1: Find whether 'fare' column contains infinity values or not.
        >>> inf_df = df.assign(infornot = df.fare.isinf())
        >>> print(inf_df)
                    age      fare infornot
        passenger
        326        36.0  135.6333        0
        183         9.0   31.3875        0
        652        18.0   23.0000        0
        40         14.0   11.2417        0
        774         NaN    7.2250        0
        366        30.0    7.2500        0
        509        28.0   22.5250        0
        795        25.0    7.8958        0
        61         22.0    7.2292        0
        469         NaN    7.7250        0
        >>>
    isnan
    teradataml.dataframe.sql.DataFrameColumn.isnan = isnan(self)
    DESCRIPTION:
        Function evaluates a variable or expression to determine if the
        floating-point argument is a NaN (Not-a-Number) value. When a database
        table contains a NaN value, the data is undefined and unrepresentable
        in floating-point arithmetic. For example, division by 0, or the square root
        of a negative number would return a NaN result.
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml","titanic")
        # Create a DataFrame on 'titanic' table.
        >>> titanic = DataFrame.from_table('titanic')
        >>> df = titanic.select(["passenger", "age", "fare"])
        >>> print(df)
                    age      fare
        passenger
        326        36.0  135.6333
        183         9.0   31.3875
        652        18.0   23.0000
        40         14.0   11.2417
        774         NaN    7.2250
        366        30.0    7.2500
        509        28.0   22.5250
        795        25.0    7.8958
        61         22.0    7.2292
        469         NaN    7.7250
        >>>
        # Example 1: Find whether 'fare' column contains NaN values or not.
        >>> nan_df = df.assign(nanornot = df.fare.isnan())
        >>> print(nan_df)
                    age      fare nanornot
        passenger
        326        36.0  135.6333        0
        183         9.0   31.3875        0
        652        18.0   23.0000        0
        40         14.0   11.2417        0
        774         NaN    7.2250        0
        366        30.0    7.2500        0
        509        28.0   22.5250        0
        795        25.0    7.8958        0
        61         22.0    7.2292        0
        469         NaN    7.7250        0
        >>>
    ln
    teradataml.dataframe.sql.DataFrameColumn.ln = ln()
    DESCRIPTION:
        Function returns the natural logarithm of the value in column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported. For more information on implicit type conversion,
           see Teradata Vantage™ Data Types and Literals.
        2. Unsupported column types:
            a. BYTE or VARBYTE
            b. LOBs (BLOB or CLOB)
            c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Computes natural logarithm of values in "gpa" column and pass it as
        #            input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.ln())
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  1.308333
        4      yes  3.50  Beginner      Novice         1  1.252763
        2      yes  3.76  Beginner    Beginner         0  1.324419
        1      yes  3.95  Beginner    Beginner         0  1.373716
        # Example 2: Executed ln() function on "gpa" column and filtered computed
        #            values which are less than 1.3.
        >>> print(df[df.gpa.ln() > 1.3])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    log
    teradataml.dataframe.sql.DataFrameColumn.log = log(self, base)
    DESCRIPTION:
        Returns the logarithm value of the column with respect to 'base'.
    PARAMETERS:
        base:
            Required Argument.
            Specifies base of logarithm.
            Type: int or float or ColumnExpression
    Returns:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml", "titanic")
        # Create a DataFrame on 'titanic' table.
        >>> titanic = DataFrame.from_table('titanic')
        >>> df = titanic.select(["passenger", "age", "fare"])
        >>> print(df)
                    age      fare
        passenger
        326        36.0  135.6333
        183         9.0   31.3875
        652        18.0   23.0000
        265         NaN    7.7500
        530        23.0   11.5000
        122         NaN    8.0500
        591        35.0    7.1250
        387         1.0   46.9000
        734        23.0   13.0000
        795        25.0    7.8958
        >>>
        # Example 1: Compute log values for column 'fare' using base as column 'age'.
        >>> log_df = df.assign(fare_log=df.fare.log(df.age))
        >>> print(log_df)
                    age      fare  fare_log
        passenger
        326        36.0  135.6333  1.370149
        183         9.0   31.3875  1.568529
        652        18.0   23.0000  1.084807
        40         14.0   11.2417  0.916854
        774         NaN    7.2250       NaN
        366        30.0    7.2500  0.582442
        509        28.0   22.5250  0.934704
        795        25.0    7.8958  0.641942
        61         22.0    7.2292  0.639955
        469         NaN    7.7250       NaN
        >>>
    log10
    teradataml.dataframe.sql.DataFrameColumn.log10 = log10()
    DESCRIPTION:
        Function returns the base 10 logarithm of the values in column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported. For more information on implicit type conversion,
           see Teradata Vantage™ Data Types and Literals.
        2. Unsupported column types:
            a. BYTE or VARBYTE
            b. LOBs (BLOB or CLOB)
            c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Computes logarithm of values in "gpa" and "gpa" * 10 and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.log10(),
        ...                col_mul = (df.gpa*10).log10())
        >>> print(res)
           masters   gpa     stats programming  admitted       col   col_mul
        id
        3       no  3.70    Novice    Beginner         1  0.568202  1.568202
        4      yes  3.50  Beginner      Novice         1  0.544068  1.544068
        2      yes  3.76  Beginner    Beginner         0  0.575188  1.575188
        1      yes  3.95  Beginner    Beginner         0  0.596597  1.596597
        # Example 2: Executed log10() function on "gpa" column and filtered computed
        #            values which are greater than 0.57.
        >>> print(df[df.gpa.log10() > 0.57])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    mod
    teradataml.dataframe.sql.DataFrameColumn.mod = mod(expression)
    DESCRIPTION:
        Function returns the remainder (modulus) of the value in column
        divided by divisor (expression or constant).
    ALTERNATE NAME:
        pmod
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value that is the divisor.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Compute the modulus of "gpa" column with constant and "gpa" column
        #            with "id" column, and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.mod(2),
        ...                 col_id =  df.gpa.mod(df.id))
        >>> print(res)
           masters   gpa     stats programming  admitted   col  col_id
        id
        3       no  3.70    Novice    Beginner         1  1.70    0.70
        4      yes  3.50  Beginner      Novice         1  1.50    3.50
        2      yes  3.76  Beginner    Beginner         0  1.76    1.76
        1      yes  3.95  Beginner    Beginner         0  1.95    0.95
        # Example 2: Executed mod() function on "gpa" column and filtered computed
        #            values which are equal to 1.5.
        >>> print(df[df.gpa.mod(2) == 1.5])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
    mul
    teradataml.dataframe.sql.DataFrameColumn.mul = mul(self, other)
    Compute the multiplication between two ColumnExpressions.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        7      yes  2.33    Novice      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        # Example 1: Increase the GPA for each student by 10 % and assign
        #            increased income to new column 'increased_gpa'.
        >>> df.assign(increased_gpa=df.gpa + df.gpa.mul(0.1))
           masters   gpa     stats programming  admitted  increased_gpa
        id
        22     yes  3.46    Novice    Beginner         0          3.806
        26     yes  3.57  Advanced    Advanced         1          3.927
        5       no  3.44    Novice      Novice         0          3.784
        17      no  3.83  Advanced    Advanced         1          4.213
        13      no  4.00  Advanced      Novice         1          4.400
        19     yes  1.98  Advanced    Advanced         0          2.178
        36      no  3.00  Advanced      Novice         0          3.300
        15     yes  4.00  Advanced    Advanced         1          4.400
        34     yes  3.85  Advanced    Beginner         0          4.235
        38     yes  2.65  Advanced    Beginner         1          2.915
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Calculate the percent of investment done of total income and assign the
        #            final amount to new column 'percentage_investment'.
        >>> df.assign(percentage_investment=(df.investment.mul(100)).div(df.income))
           start_time_column end_time_column  expenditure  income  investment  percentage_investment
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0              38.144330
        2           67/06/30        07/07/10        421.0   465.0       179.0              38.494624
        1           67/06/30        07/07/10        415.0   451.0       180.0              39.911308
        5           67/06/30        07/07/10        459.0   509.0       211.0              41.453831
        4           67/06/30        07/07/10        448.0   493.0       192.0              38.945233
        # Example 3: Filter out the rows after doubling income is greater than 1000.
        >>> df[(df.income * 2) > 1000]
           start_time_column end_time_column  expenditure  income  investment  double_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0          970.0
        2           67/06/30        07/07/10        421.0   465.0       179.0          930.0
        1           67/06/30        07/07/10        415.0   451.0       180.0          902.0
        5           67/06/30        07/07/10        459.0   509.0       211.0         1018.0
        4           67/06/30        07/07/10        448.0   493.0       192.0          986.0
    nullifzero
    teradataml.dataframe.sql.DataFrameColumn.nullifzero = nullifzero()
    DESCRIPTION:
        Function converts the values in column from zero to null to avoid problems with division by zero.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported. For more information on implicit type conversion,
           see Teradata Vantage™ Data Types and Literals.
        2. Unsupported column types:
            a. BYTE or VARBYTE
            b. LOBs (BLOB or CLOB)
            c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Convert 0 values in "admitted" column to null using
        #            nullifzero() function.
        >>> res = df.assign(col = df.admitted.nullifzero())
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  1.0
        4      yes  3.50  Beginner      Novice         1  1.0
        2      yes  3.76  Beginner    Beginner         0  NaN
        1      yes  3.95  Beginner    Beginner         0  NaN
        # Example 2: Executed nullifzero() function on "admitted" column and filtered
        #            computed values which are None.
        >>> print(df[df.admitted.nullifzero() == None])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    pmod
    teradataml.dataframe.sql.DataFrameColumn.pmod = pmod(expression)
    DESCRIPTION:
        Function returns the remainder (modulus) of the value in column
        divided by divisor (expression or constant).
    ALTERNATE NAME:
        mod
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value that is the divisor.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Compute the modulus of "gpa" column with constant and "gpa"
        #            column with "id" column, and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.pmod(2),
        ...                col_id =  df.gpa.pmod(df.id))
        >>> print(res)
           masters   gpa     stats programming  admitted   col  col_id
        id
        3       no  3.70    Novice    Beginner         1  1.70    0.70
        4      yes  3.50  Beginner      Novice         1  1.50    3.50
        2      yes  3.76  Beginner    Beginner         0  1.76    1.76
        1      yes  3.95  Beginner    Beginner         0  1.95    0.95
        # Example 2: Executed mod() function on "gpa" column and filtered computed values
        #            which are greater than 1.7.
        >>> print(df[df.gpa.mod(2) > 1.7])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    pow
    teradataml.dataframe.sql.DataFrameColumn.pow = pow(expression)
    DESCRIPTION:
        Function returns the value in the column raised to the power of the exponent value (expression).
    ALTERNATE NAME:
        power
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value that is the exponent value.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Compute the power and pass it as input to DataFrame.assign().
        >>> res = df.assign(col_2 = df.gpa.pow(2),
        ...                    col = df.gpa.pow(df.admitted))
        >>> print(res)
           masters   gpa     stats programming  admitted  col    col_2
        id
        3       no  3.70    Novice    Beginner         1  3.7  13.6900
        4      yes  3.50  Beginner      Novice         1  3.5  12.2500
        2      yes  3.76  Beginner    Beginner         0  1.0  14.1376
        1      yes  3.95  Beginner    Beginner         0  1.0  15.6025
        Example 2: Executed pow() function on "admitted" column and filtered computed
        #          values which are equal to 1.0.
        >>> print(df[df.gpa.pow(df.admitted) == 1.0])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    power
    teradataml.dataframe.sql.DataFrameColumn.power = power(expression)
    DESCRIPTION:
        Function returns the value in the column raised to the power of the exponent value (expression).
    ALTERNATE NAME:
        pow
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value that is the exponent value.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Compute the power and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.power(2),
        ...                    col_gpa = df.gpa.power(df.admitted))
        >>> print(res)
           masters   gpa     stats programming  admitted  col    col_2
        id
        3       no  3.70    Novice    Beginner         1  3.7  13.6900
        4      yes  3.50  Beginner      Novice         1  3.5  12.2500
        2      yes  3.76  Beginner    Beginner         0  1.0  14.1376
        1      yes  3.95  Beginner    Beginner         0  1.0  15.6025
        # Example 2: Executed power() function on "admitted" column and filtered computed
        #            values which are equal to 1.0.
        >>> print(df[df.gpa.power(df.admitted) == 1.0])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    radians
    teradataml.dataframe.sql.DataFrameColumn.radians = radians()
    DESCRIPTION:
        Converts the degrees value from the column to radians.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported. For more information on implicit type conversion,
           see Teradata Vantage™ Data Types and Literals.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Converts values in the "admitted" column to radians and pass
        #            it as input to DataFrame.assign().
        >>> res = df.assign(col = df.admitted.radians())
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  0.017453
        4      yes  3.50  Beginner      Novice         1  0.017453
        2      yes  3.76  Beginner    Beginner         0  0.000000
        1      yes  3.95  Beginner    Beginner         0  0.000000
        # Example 2: Executed radians() function on "admitted" column and filtered computed
        #            values which are greater than 0.01.
        >>> print(df[df.admitted.radians() > 0.01])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
        3       no  3.7    Novice    Beginner         1
    round
    teradataml.dataframe.sql.DataFrameColumn.round = round(expression)
    DESCRIPTION:
        Function returns the value in the column rounded by the places_value (expression) number
        of places to the right or left of the decimal point.
        Notes:
            * It rounds places_value places to the right of the decimal point if places_value
              is positive.
            * It rounds places_value places to the left of the decimal point if places_value
              is negative.
            * It rounds to 0 places if places_value is zero or is omitted.
            * If numeric_value or places_value is NULL, the function returns NULL.
            * It rounds the value away from zero, and it only rounds when the next digit is
              a value of 5 or greater.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant value that
            defines the number of places to round.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Execute the round() function and pass it as input to
        #            DataFrame.assign().
        >>> res = df.assign(col_1 = df.gpa.round(1),
        ...                 col = df.gpa.round(df.admitted))
        >>> print(res)
           masters   gpa     stats programming  admitted  col  col_1
        id
        3       no  3.70    Novice    Beginner         1  3.7    3.7
        4      yes  3.50  Beginner      Novice         1  3.5    3.5
        2      yes  3.76  Beginner    Beginner         0  4.0    3.8
        1      yes  3.95  Beginner    Beginner         0  4.0    4.0
        # Example 2: Executed round() function on "gpa" column and filtered computed values
        #            which are greater than 3.8.
        >>> print(df[df.gpa.round(1) > 3.8])
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
    sign
    teradataml.dataframe.sql.DataFrameColumn.sign = sign()
    DESCRIPTION:
        Function returns the sign of numeric values in column.
        Notes:
            * If numeric value is < 0, -1 is returned.
            * If numeric value is = 0, 0 is returned.
            * If numeric value is > 0, 1 is returned.
    ALTERNATE NAME:
        signum
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
             masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Execute the sign() function and pass it as input to
        #            DataFrame.assign().
        >>> res = df.assign(col = df.gpa.sign())
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  1.0
        4      yes  3.50  Beginner      Novice         1  1.0
        2      yes  3.76  Beginner    Beginner         0  1.0
        1      yes  3.95  Beginner    Beginner         0  1.0
        # Example 2: Executed sign() function on "gpa" column and filtered
        #            computed values which are equal to 1.0.
        >>> print(df[df.gpa.sign() == 1.0])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    signum
    teradataml.dataframe.sql.DataFrameColumn.signum = signum()
    DESCRIPTION:
        Function returns the sign of numeric values in column.
        Notes:
            * If numeric value is < 0, -1 is returned.
            * If numeric value is = 0, 0 is returned.
            * If numeric value is > 0, 1 is returned.
    ALTERNATE NAME:
        sign
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
             masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Execute the signum() function and pass it as input
        #            to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.signum())
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  1.0
        4      yes  3.50  Beginner      Novice         1  1.0
        2      yes  3.76  Beginner    Beginner         0  1.0
        1      yes  3.95  Beginner    Beginner         0  1.0
        # Example 2: Executed signum() function on "gpa" column and filtered
        #            computed values which are equal to 1.0.
        >>> print(df[df.gpa.signum() == 1.0])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    sqrt
    teradataml.dataframe.sql.DataFrameColumn.sqrt = sqrt()
    DESCRIPTION:
        Function computes the square root of the values in a column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported. For more information on implicit type conversion,
           see Teradata Vantage™ Data Types and Literals.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Execute sqrt() function and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.sqrt())
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  1.923538
        4      yes  3.50  Beginner      Novice         1  1.870829
        2      yes  3.76  Beginner    Beginner         0  1.939072
        1      yes  3.95  Beginner    Beginner         0  1.987461
        # Example 2: Executed sqrt() function on "gpa" column and filtered computed
        #            values which are greater than 1.93.
        >>> print(df[df.gpa.sqrt() > 1.93])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    sub
    teradataml.dataframe.sql.DataFrameColumn.sub = sub(self, other)
    Compute the subtraction between two ColumnExpressions.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Subtract 100 from the income amount and assign the final amount
        #            to new column 'remaining_income'.
        >>> df.assign(remaining_income=df.income - 100)
           start_time_column end_time_column  expenditure  income  investment  remaining_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0             385.0
        2           67/06/30        07/07/10        421.0   465.0       179.0             365.0
        1           67/06/30        07/07/10        415.0   451.0       180.0             351.0
        5           67/06/30        07/07/10        459.0   509.0       211.0             409.0
        4           67/06/30        07/07/10        448.0   493.0       192.0             393.0
        # Example 2: Filter the rows where the income left after the investment is more than 300.
        >>> df[df.income.sub(df.investment) > 300]
           start_time_column end_time_column  expenditure  income  investment
        id
        4           67/06/30        07/07/10        448.0   493.0       192.0
    truediv
    teradataml.dataframe.sql.DataFrameColumn.truediv = truediv(self, other)
    Compute the true-division between two ColumnExpressions.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Calculate the percent of investment of income and assign the
        #            final amount to new column 'percentage_investment'.
        >>> df.assign(percentage_investment=(df.investment.mul(100)).truediv(df.income))
           start_time_column end_time_column  expenditure  income  investment  percentage_investment
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0              38.144330
        2           67/06/30        07/07/10        421.0   465.0       179.0              38.494624
        1           67/06/30        07/07/10        415.0   451.0       180.0              39.911308
        5           67/06/30        07/07/10        459.0   509.0       211.0              41.453831
        4           67/06/30        07/07/10        448.0   493.0       192.0              38.945233
        # Example 2: Filter out the rows after diving income amount by 2 is less than 240.
        >>> df[(df.income.truediv(2)) < 240]
           start_time_column end_time_column  expenditure  income  investment
        id
        2           67/06/30        07/07/10        421.0   465.0       179.0
        1           67/06/30        07/07/10        415.0   451.0       180.0
    trunc
    teradataml.dataframe.sql.DataFrameColumn.trunc = trunc(self, expression=0, formatter=None)
    DESCRIPTION:
        Function to truncate the values inside the column based on formatter.
        Numeric type column:
            Function returns the values in column truncated places_value (expression) places to the right or left
            of the decimal point.
            trunc() functions as follows:
                * It truncates places_value places to the right of the decimal point if
                  places_value is positive.
                * It truncates (makes 0) places_value places to the left of the decimal
                  point if places_value is negative.
                * It truncates to 0 places if places_value is zero or is omitted.
                * If numeric_value or places_value is NULL, the function returns NULL.
        Date type column:
           Function truncates date type based on the format specified by formatter.
           Example:
               First example truncates data to first day of the week.
               Second example truncates data to beginning of the month.
               +------------------------------------------+
               |    data            formatter       result|
               +------------------------------------------+
               |    19/01/16        'D'           19/01/13|
               |    19/02/27        'MON'         19/02/01|
               +------------------------------------------+
    PARAMETERS:
        expression:
            Optional Argument.
            Specifies to truncate the "expression" number of digits.
            Note:
                This argument applicable only for Numeric columns.
            Default Value: 0
            Types: ColumnExpression OR int
        formatter:
            Optional Argument.
            Specifies a literal string to truncate the values of column.
            If 'formatter' is omitted, date_value is truncated to the nearest day.
            Type: str
            Note:
                * This argument applicable only for Date type columns.
            * Various formatter given below:
                +--------------------------------------------------------------------------------------------------+
                |    FORMATTER                        DESCRIPTION                                                  |
                +--------------------------------------------------------------------------------------------------+
                |    CC                                                                                            |
                |    SCC                              One year greater than the first two digits of a 4-digit year.|
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/01/16        CC                  01/01/01    |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    SYYY                                                                                          |
                |    YYYY                                                                                          |
                |    YEAR                                                                                          |
                |    SYEAR                            Year. Returns a value of 1, the first day of the year.       |
                |    YYY                                                                                           |
                |    YY                                                                                            |
                |    Y                                                                                             |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/01/16        CC                  19/01/01    |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    IYYY                                                                                          |
                |    IYY                              ISO year                                                     |
                |    IY                                                                                            |
                |    I                                                                                             |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/01/16        CC                  18/12/31    |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    MONTH                                                                                         |
                |    MON                              Month. Returns a value of 1, the first day of the month.     |
                |    MM                                                                                            |
                |    RM                                                                                            |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/02/16        RM                  19/02/01    |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    Q                                Quarter. Returns a value of 1, the first day of the quarter. |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/02/16        Q                  19/01/01     |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    WW                               Same day of the week as the 1st day of the year.             |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/02/16        WW                  19/02/15    |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    IW                               Same day of the week as the first day of the ISO year.       |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/02/16        RM                  19/02/14    |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    W                                Same day of the week as the first day of the month.          |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/02/16        W                  19/02/15     |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    DDD                                                                                           |
                |    DD                               Day.                                                         |
                |    J                                                                                             |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/02/16        DDD                 19/02/16    |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    DAY                                                                                           |
                |    DY                               Starting day of the week.                                    |
                |    D                                                                                             |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/01/16        DAY                  19/02/13   |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    HH                                                                                            |
                |    HH12                             Hour.                                                        |
                |    HH24                                                                                          |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data            formatter           result      |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 19/02/16        HH                  19/02/16    |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |    MI                               Minute.                                                      |
                |                                     Example:                                                     |
                |                                         +-------------------------------------------------+      |
                |                                         | data                       formatter  result    |      |
                |                                         +-------------------------------------------------+      |
                |                                         | 2016-01-06 09:08:01.000000    MI      16/01/06  |      |
                |                                         +-------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        ColumnExpression
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>> load_example_data("uaf", "stock_data")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> df
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Create a DataFrame on 'stock_data' table.
        >>> df1 = DataFrame("stock_data")
        >>> df1
                    seq_no timevalue  magnitude
        data_set_id
        556               3  19/01/16     61.080
        556               5  19/01/30     63.810
        556               6  19/02/06     63.354
        556               7  19/02/13     63.871
        556               9  19/02/27     61.490
        556              10  19/03/06     61.524
        556               8  19/02/20     61.886
        556               4  19/01/23     63.900
        556               2  19/01/09     61.617
        556               1  19/01/02     60.900
        # Example 1: Truncate the value of 'gpa' to 0 decimal place and 1 decimal place.
        >>> res = df.assign(col = df.gpa.trunc(),
                               col_1 = df.gpa.trunc(1))
        >>> res
           masters   gpa     stats programming  admitted  col  col_1
        id
        3       no  3.70    Novice    Beginner         1  3.0    3.7
        4      yes  3.50  Beginner      Novice         1  3.0    3.5
        2      yes  3.76  Beginner    Beginner         0  3.0    3.7
        1      yes  3.95  Beginner    Beginner         0  3.0    3.9
        # Example 2: Get the records with gpa > 3.7 by rounding the gpa to single decimal point.
        >>> df[df.gpa.trunc(1) > 3.7]
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        # Example 3: Truncate the value of 'timevalue' to beginning of the month.
        >>> res=df1.assign(timevalue=df1.timevalue.trunc(formatter="MON"))
        >>> res
                        seq_no timevalue  magnitude time_value
        data_set_id
        556               3  19/01/16     61.080   19/01/01
        556               5  19/01/30     63.810   19/01/01
        556               6  19/02/06     63.354   19/02/01
        556               7  19/02/13     63.871   19/02/01
        556               9  19/02/27     61.490   19/02/01
        556              10  19/03/06     61.524   19/03/01
        556               8  19/02/20     61.886   19/02/01
        556               4  19/01/23     63.900   19/01/01
        556               2  19/01/09     61.617   19/01/01
        556               1  19/01/02     60.900   19/01/01
    unhex
    teradataml.dataframe.sql.DataFrameColumn.unhex = unhex(self)
    DESCRIPTION:
        Function to compute the decimal from Hexadecimal for the column.
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml","titanic")
        # Create a DataFrame on 'titanic' table.
        >>> titanic = DataFrame.from_table('titanic')
        >>> df = titanic.select(["passenger", "age", "fare"])
        >>> print(df)
                    age      fare
        passenger
        326        36.0  135.6333
        183         9.0   31.3875
        652        18.0   23.0000
        265         NaN    7.7500
        530        23.0   11.5000
        122         NaN    8.0500
        591        35.0    7.1250
        387         1.0   46.9000
        734        23.0   13.0000
        795        25.0    7.8958
        >>>
        # create a "age_in_hex" column which contains hexadecimal values
        >>> hex_df = df.assign(age_in_hex=df.age.hex())
        >>> print(hex_df)
                    age    fare age_in_hex
        passenger
        530        23.0  11.500         17
        591        35.0   7.125         23
        387         1.0  46.900          1
        856        18.0   9.350         12
        244        22.0   7.125         16
        713        48.0  52.000         30
        448        34.0  26.550         22
        122         NaN   8.050       None
        734        23.0  13.000         17
        265         NaN   7.750       None
        >>>
        # Example 1: Converts values in "age_in_hex" hexadecimal to decimal and pass it as input to
        #            DataFrame.assign().
        >>> unhex_df = hex_df.assign(age_in_decimal=hex_df.age_in_hex.unhex())
        >>> print(unhex_df)
                    age      fare age_in_hex age_in_decimal
        passenger
        326        36.0  135.6333         24             36
        183         9.0   31.3875          9              9
        652        18.0   23.0000         12             18
        40         14.0   11.2417          E             14
        774         NaN    7.2250       None           None
        366        30.0    7.2500         1E             30
        509        28.0   22.5250         1C             28
        795        25.0    7.8958         19             25
        61         22.0    7.2292         16             22
        469         NaN    7.7250       None           None
        >>>
    width_bucket
    teradataml.dataframe.sql.DataFrameColumn.width_bucket = width_bucket(min, max, numBucket)
    DESCRIPTION:
        Function returns the number of the partition to which values in a column
        is assigned.
        Following rules apply to width_bucket:
            * If any value is null, then the result is also null.
            * If "min" < "max", then following rules apply:
                * If value in column < "min", then 0 returned.
                * If value in column >= "max", the "numBucket" + 1 is returned.
                  If the result cannot be represented by the data type specified for the
                  result, then an error is returned.
                * Else the greatest exact numeric value with scale 0 that is less than or
                  equal to the following expression is returned.
                  (("numBucket")(value in column - "min")/("max" - "min")) + 1
            * If "min" > "max", then following rules apply:
                * If column_expression > "min", then 0 returned.
                * If column_expression <= "max", the "numBucket" + 1 is returned.
                  If the result cannot be represented by the data type specified for the
                  result, then an error is returned.
                * Else the least exact numeric value with scale 0 that is less than or equal
                  to the following expression is returned.
                  (("numBucket")("min" - column_expression)/("min" - "max")) + 1
            * Error is reported in following cases:
                * If "numBucket" <= 0 or if "numBucket" > 2147483646
                * If "min" = "max"
    PARAMETERS:
        min:
            Required Argument.
            Specifies the lower boundary for the range of values to be partitioned equally.
            Types: float, int
        max:
            Required Argument.
            Specifies the upper boundary for the range of values to be partitioned equally.
            Types: float, int
        numBucket:
            Required Argument.
            Specified the number of partitions to be created. This value also specifies
            the width of the partitions by default. The number of partitions created is
            "numBucket" + 2. Partition 0 and partition "numBucket" + 1 account
            for values of column_expression that are outside the lower and upper boundaries.
            Types: float, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Execute the function and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.gpa.width_bucket(2.5, 3.5, 3))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1    4
        4      yes  3.50  Beginner      Novice         1    4
        2      yes  3.76  Beginner    Beginner         0    4
        1      yes  3.95  Beginner    Beginner         0    4
        # Example 2: Executed width_bucket() function on "gpa" column and filtered computed
        #            values which are equal to 4.
        >>> print(df[df.gpa.width_bucket(2.5, 3.5, 3) == 4])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    zeroifnull
    teradataml.dataframe.sql.DataFrameColumn.zeroifnull = zeroifnull()
    DESCRIPTION:
        Function converts the values in column from null to zero to avoid cases where a null result creates an error.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported. For more information on implicit type conversion,
           see Teradata Vantage™ Data Types and Literals.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe","admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        # We will process the data to introduce some null values in the dataframe, which we will convert to 0
        # using "zeroifnull()" function.
        >>> df = DataFrame('admissions_train')
        >>> df1 = df[df.gpa == 4].select(['id', 'stats', 'masters', 'gpa'])
        >>> df2 = df[df.gpa < 2].select(['id', 'stats', 'programming', 'admitted'])
        # Let's concat both df1 and df2.
        >>> cdf = df1.concat(df2)
        >>> cdf
               stats masters  gpa programming  admitted
        id
        29    Novice     yes  4.0        None       NaN
        24  Advanced    None  NaN      Novice       1.0
        19  Advanced    None  NaN    Advanced       0.0
        15  Advanced     yes  4.0        None       NaN
        13  Advanced      no  4.0        None       NaN
        # Let's persist the cdf to Vantage.
        >>> copy_to_sql(cdf, "zeroifnull_test_table")
        >>> newdf = DataFrame("zeroifnull_test_table")
        >>> newdf
           id     stats masters  gpa programming  admitted
        0  19  Advanced    None  NaN    Advanced       0.0
        1  24  Advanced    None  NaN      Novice       1.0
        2  13  Advanced      no  4.0        None       NaN
        3  29    Novice     yes  4.0        None       NaN
        4  15  Advanced     yes  4.0        None       NaN
        # Converting null values in 'admitted' column to 0 using 'zeroifnull()' function.
        >>> rdf = newdf.assign(admitted_zero_if_null = newdf.admitted.zeroifnull())
        >>> print(rdf)
           id     stats masters  gpa programming  admitted  admitted_zero_if_null
        0  29    Novice     yes  4.0        None       NaN                      0
        1  24  Advanced    None  NaN      Novice       1.0                      1
        2  19  Advanced    None  NaN    Advanced       0.0                      0
        3  13  Advanced      no  4.0        None       NaN                      0
        4  15  Advanced     yes  4.0        None       NaN                      0
    DataFrameColumn Hyperbolic Functions
    acosh
    teradataml.dataframe.sql.DataFrameColumn.acosh = acosh()
    DESCRIPTION:
        Function computes the inverse hyperbolic cosine value of the values in column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculate inverse hyperbolic cosine value for the "gpa" column and pass
        #            it as input to DataFrame.assign().
        >>> df = df.assign(col=df.gpa.acosh())
        >>> print(df)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  1.982697
        4      yes  3.50  Beginner      Novice         1  1.924847
        2      yes  3.76  Beginner    Beginner         0  1.999394
        1      yes  3.95  Beginner    Beginner         0  2.050440
        # Example 2: Filter the rows where inverse hyperbolic cosine of values in "gpa" column are
        #            greater than 2.
        >>> print(df[df.gpa.acosh() > 2])
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
    asinh
    teradataml.dataframe.sql.DataFrameColumn.asinh = asinh()
    DESCRIPTION:
        Function computes the inverse hyperbolic sine value of the values in column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculate inverse hyperbolic sin value for the "gpa" column and pass it
        #            as input to DataFrame.assign().
        >>> df = df.assign(col = df.gpa.asinh())
        >>> print(df)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  2.019261
        4      yes  3.50  Beginner      Novice         1  1.965720
        2      yes  3.76  Beginner    Beginner         0  2.034798
        1      yes  3.95  Beginner    Beginner         0  2.082514
        # Example 2: Filter the rows where inverse hyperbolic sine of values in "gpa" column are
        #            greater than 2.
        >>> print(df[df.gpa.asinh() > 2])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    atanh
    teradataml.dataframe.sql.DataFrameColumn.atanh = atanh()
    DESCRIPTION:
        Function computes the inverse hyperbolic tangent value of the values in column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculate inverse hyperbolic tan value for the 'gpa column values/10'
        #            and pass it as input to DataFrame.assign().
        >>> result = df.assign(col = (df.gpa / 10).atanh())
        >>> print(result)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  0.388423
        4      yes  3.50  Beginner      Novice         1  0.365444
        2      yes  3.76  Beginner    Beginner         0  0.395393
        1      yes  3.95  Beginner    Beginner         0  0.417711
        # Example 2: Filter the rows where inverse hyperbolic tangent of values in 'gpa column values/10'
        #            column are greater than 0.4.
        >>> print(df[(df.gpa/10).atanh() > 0.4])
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
    cosh
    teradataml.dataframe.sql.DataFrameColumn.cosh = cosh(column_expression)
    DESCRIPTION:
        Function computes the hyperbolic cosine value of the values in column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates hyperbolic cos value for the "gpa" column and pass it as
        #            input to DataFrame.assign().
        >>> result = df.assign(col=df.gpa.cosh())
        >>> print(result)
           masters   gpa     stats programming  admitted        col
        id
        3       no  3.70    Novice    Beginner         1  20.236014
        4      yes  3.50  Beginner      Novice         1  16.572825
        2      yes  3.76  Beginner    Beginner         0  21.485855
        1      yes  3.95  Beginner    Beginner         0  25.977311
        # Example 2: Filter the rows where hyperbolic cosine of values in "gpa" column
        #            are greater than 21.
        >>> print(df[df.gpa.cosh() > 21])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    sinh
    teradataml.dataframe.sql.DataFrameColumn.sinh = sinh(column_expression)
    DESCRIPTION:
        Function computes the hyperbolic sine value of the values in column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates hyperbolic sine value for the "gpa" column and pass
        #            it as input to DataFrame.assign().
        >>> result = df.assign(col = df.gpa.sinh())
        >>> print(result)
           masters   gpa     stats programming  admitted        col
        id
        3       no  3.70    Novice    Beginner         1  20.211290
        4      yes  3.50  Beginner      Novice         1  16.542627
        2      yes  3.76  Beginner    Beginner         0  21.462571
        1      yes  3.95  Beginner    Beginner         0  25.958056
        # Example 2: Filter the rows where hyperbolic sine of values in "gpa" column
        #            are greater than 21.
        >>> print(df[df.gpa.sinh() > 21])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    tanh
    teradataml.dataframe.sql.DataFrameColumn.tanh = tanh()
    DESCRIPTION:
        Function computes the hyperbolic tangent value of the values in column.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates hyperbolic tan value for the "gpa" column and pass it as
        # input to DataFrame.assign().
        >>> result = df.assign(col = df.gpa.tanh())
        >>> print(result)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  0.998778
        4      yes  3.50  Beginner      Novice         1  0.998178
        2      yes  3.76  Beginner    Beginner         0  0.998916
        1      yes  3.95  Beginner    Beginner         0  0.999259
        # Example 2: Filter the rows where hyperbolic tangent of values in "gpa" column
        #            are greater than 0.9989.
        >>> print(df[df.gpa.tanh() > 0.9989])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    DataFrameColumn Trigonometric Functions
    acos
    teradataml.dataframe.sql.DataFrameColumn.acos = acos()
    DESCRIPTION:
        Function computes the arccosine value of values in column.
        The arccosine is the angle whose cosine is the values in column.
        Result values return an angle in the range 0 to π radians, inclusive.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the values cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates arccosine value for the 'gpa column values / 10' and
        #            pass it as input to DataFrame.assign().
        >>> result = df.assign(col=(df.gpa/10).acos())
        >>> print(result)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  1.191787
        4      yes  3.50  Beginner      Novice         1  1.213225
        2      yes  3.76  Beginner    Beginner         0  1.185321
        1      yes  3.95  Beginner    Beginner         0  1.164728
        # Example 2: Filter the rows where arccosine of values in 'gpa column values / 10' column
        #            are greater than 1.2.
        >>> print(df[(df.gpa/10).acos() > 1.2])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
    asin
    teradataml.dataframe.sql.DataFrameColumn.asin = asin()
    DESCRIPTION:
        Function computes the arcsine value of values in column.
        The arcsine is the angle whose sine is the values in column.
        Result values return an angle in the range -π /2 to π /2 radians, inclusive.
    NOTES:
        1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If an argument cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates arcsine value for the 'gpa column values / 10' and
        #            pass it as input to DataFrame.assign().
        >>> result = df.assign(col = (df.gpa / 10).asin())
        >>> print(result)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  0.379009
        4      yes  3.50  Beginner      Novice         1  0.357571
        2      yes  3.76  Beginner    Beginner         0  0.385476
        1      yes  3.95  Beginner    Beginner         0  0.406068
        # Example 2: Filter the rows where arcsine of values in 'gpa column values / 10' column
        #            are greater than 0.38.
        >>> print(df[(df.gpa/10).asin() > 0.38])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    atan
    teradataml.dataframe.sql.DataFrameColumn.atan = atan()
    DESCRIPTION:
        Function computes the arctangent value of values in column.
        The arctangent is the angle whose tangent is the values in column.
        Result values return an angle in the range -π /2 to π /2 radians, inclusive.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the values cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates arctangent value for the "gpa" column and pass
        #            it as input to DataFrame.assign().
        >>> result = df.assign(col=df.gpa.atan())
        >>> print(result)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  1.306833
        4      yes  3.50  Beginner      Novice         1  1.292497
        2      yes  3.76  Beginner    Beginner         0  1.310856
        1      yes  3.95  Beginner    Beginner         0  1.322841
        # Example 2: Filter the rows where arctangent of values in "gpa" column
        #            are greater than 1.31.
        >>> print(df[df.gpa.atan() > 1.31])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    atan2
    teradataml.dataframe.sql.DataFrameColumn.atan2 = atan2(expression)
    DESCRIPTION:
        Function computes the arctangent value based on the values in column and argument
        coordinate provided as input. The arctangent is the angle whose tangent is the value in the column.
    NOTES:
        1. Result values return an angle between -π and π radians, excluding -π.
        2. A positive result represents a counterclockwise angle from the x-axis.
        3. A negative result represents a clockwise angle from the x-axis.
        4. If both the values from column and argument are 0, an error is returned.
        5. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        6. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    PARAMETERS:
        expression:
            Required Argument.
            Specifies the y-coordinate of a point to use in the arctangent calculation.
            Accepts a ColumnExpression of a numeric column or a numeric constant.
            Format for the argument: '<dataframe>.<dataframe_column>'.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculate arctangent value for the "gpa" column values as x-coordinate
        #            and pass it as input to DataFrame.assign().
        >>> result = df.assign(col=df.gpa.atan2(1),
        ...                    col_gpa=df.gpa.atan2(df.admitted))
        >>> print(result)
           masters   gpa     stats programming  admitted       col   col_gpa
        id
        3       no  3.70    Novice    Beginner         1  0.263964  0.263964
        4      yes  3.50  Beginner      Novice         1  0.278300  0.278300
        2      yes  3.76  Beginner    Beginner         0  0.259940  0.000000
        1      yes  3.95  Beginner    Beginner         0  0.247955  0.000000
        # Example 2: Filter the rows where arctangent of values in "gpa" column
        #            and argument are greater than 0.4.
        >>> print(df[df.gpa.atan2(1) > 0.26])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
        3       no  3.7    Novice    Beginner         1
    cos
    teradataml.dataframe.sql.DataFrameColumn.cos = cos()
    DESCRIPTION:
        Function computes the cosine value of the values in column.
        The cosine of an angle is the ratio of two sides of a right triangle.
        The ratio is the length of the side adjacent to the angle divided by
        the length of the hypotenuse.
        The cosine of the values in a column returns values in radians in
        the range -1 to 1, inclusive.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates cosine value for the "gpa" column and pass it
        #            as input to DataFrame.assign().
        >>> result = df.assign(col=df.gpa.cos())
        >>> print(result)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1 -0.848100
        4      yes  3.50  Beginner      Novice         1 -0.936457
        2      yes  3.76  Beginner    Beginner         0 -0.814803
        1      yes  3.95  Beginner    Beginner         0 -0.690651
        # Example 2: Filter the rows where cosine of values in "gpa" column
        #            are less than -0.70.
        >>> print(df[df.gpa.cos() < -0.70])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        3       no  3.70    Novice    Beginner         1
        2      yes  3.76  Beginner    Beginner         0
    sin
    teradataml.dataframe.sql.DataFrameColumn.sin = sin()
    DESCRIPTION:
        Function computes the sine value of the values in column.
        The sine of an angle is the ratio of two sides of a right triangle.
        The ratio is the length of the side opposite to the angle divided
        by the length of the hypotenuse.
        The sine of argument returns values in radians in the range -1 to 1, inclusive.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates sine value for the "gpa" column and pass it
        #            as input to DataFrame.assign().
        >>> result = df.assign(col=df.gpa.sin())
        >>> print(result)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1 -0.529836
        4      yes  3.50  Beginner      Novice         1 -0.350783
        2      yes  3.76  Beginner    Beginner         0 -0.579738
        1      yes  3.95  Beginner    Beginner         0 -0.723188
        # Example 2: Filter the rows where sine of values in "gpa" column
        #            are less than -0.70.
        >>> print(df[df.gpa.sin() < -0.70])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        3       no  3.70    Novice    Beginner         1
        2      yes  3.76  Beginner    Beginner         0
    tan
    teradataml.dataframe.sql.DataFrameColumn.tan = tan()
    DESCRIPTION:
        Function computes the tangent value of the values in column.
        The tangent of an angle is the ratio of two sides of a right triangle.
        The ratio is the length of the side opposite to the angle divided
        by the length of the side adjacent to the angle.
        The tangent of argument returns values in radians.
    NOTES:
        1. If the type of the column is not FLOAT, column values are converted to FLOAT
           based on implicit type conversion rules. If the value cannot be converted, an
           error is reported.
        2. Unsupported column types:
           a. BYTE or VARBYTE
           b. LOBs (BLOB or CLOB)
           c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculates tangent value for the "gpa" column and pass
        #            it as input to DataFrame.assign().
        >>> result = df.assign(tan_gpa_=df.gpa.tan())
        >>> print(result)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1  0.624733
        4      yes  3.50  Beginner      Novice         1  0.374586
        2      yes  3.76  Beginner    Beginner         0  0.711507
        1      yes  3.95  Beginner    Beginner         0  1.047111
        # Example 2: Filter the rows where tangent of values in "gpa" column
        #            are greater than 0.62.
        >>> print(df[df.gpa.tan() > 0.62])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    DataFrameColumn Comparison Functions
    greatest
    teradataml.dataframe.sql.DataFrameColumn.greatest = greatest(self, *columns)
    DESCRIPTION:
        Get the greatest values from the given columns.
        Note:
            * All of the input columns type must be of same data
              type or else the types must be compatible.
    PARAMETERS:
        *columns:
            Specifies the name(s) of the columns or ColumnExpression(s)
            as positional arguments.
            At least one positional argument is required.
            Types: str OR int OR float OR ColumnExpression OR ColumnExpressions
    RETURNS:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("glmpredict", "housing_test")
        >>>
        # Create a DataFrame on 'housing_test' table.
        >>> df = DataFrame("housing_test")
        >>> df = df.select(["sn", "price", "lotsize", "bedrooms", "bathrms", "stories"])
        >>> df
               price  lotsize  bedrooms  bathrms  stories
        sn
        364  72000.0  10700.0         3        1        2
        13   27000.0   1700.0         3        1        2
        459  44555.0   2398.0         3        1        1
        463  49000.0   2610.0         3        1        2
        260  41000.0   6000.0         2        1        1
        177  70000.0   5400.0         4        1        2
        53   68000.0   9166.0         2        1        1
        440  69000.0   6862.0         3        1        2
        255  61000.0   4360.0         4        1        2
        301  55000.0   4080.0         2        1        1
        >>>
        # Example 1: Find the greatest values in the columns "price" and "lotsize".
        >>> gt_df = df.assign(gt_col=df.price.greatest(df.lotsize))
        >>> gt_df
               price  lotsize  bedrooms  bathrms  stories   gt_col
        sn
        364  72000.0  10700.0         3        1        2  72000.0
        13   27000.0   1700.0         3        1        2  27000.0
        459  44555.0   2398.0         3        1        1  44555.0
        463  49000.0   2610.0         3        1        2  49000.0
        260  41000.0   6000.0         2        1        1  41000.0
        177  70000.0   5400.0         4        1        2  70000.0
        53   68000.0   9166.0         2        1        1  68000.0
        440  69000.0   6862.0         3        1        2  69000.0
        255  61000.0   4360.0         4        1        2  61000.0
        301  55000.0   4080.0         2        1        1  55000.0
        >>>
        # Example 2: Find the greatest values in the columns "price", "lotsize" and 70000.0.
        >>> gt_df = df.assign(gt_col=df.price.greatest(df.lotsize, 70000))
        >>> gt_df
               price  lotsize  bedrooms  bathrms  stories   gt_col
        sn
        364  72000.0  10700.0         3        1        2  72000.0
        13   27000.0   1700.0         3        1        2  70000.0
        459  44555.0   2398.0         3        1        1  70000.0
        463  49000.0   2610.0         3        1        2  70000.0
        260  41000.0   6000.0         2        1        1  70000.0
        177  70000.0   5400.0         4        1        2  70000.0
        53   68000.0   9166.0         2        1        1  70000.0
        440  69000.0   6862.0         3        1        2  70000.0
        255  61000.0   4360.0         4        1        2  70000.0
        301  55000.0   4080.0         2        1        1  70000.0
        >>>
    least
    teradataml.dataframe.sql.DataFrameColumn.least = least(self, *columns)
    DESCRIPTION:
        Get the least values from the given columns.
        Note:
            * All of the input columns type must be of same data
              type or else the types must be compatible.
    PARAMETERS:
        *columns:
            Specifies the name(s) of the columns or ColumnExpression(s)
            as positional arguments.
            At least one positional argument is required.
            Types: str or int or float or ColumnExpression OR ColumnExpressions
    RETURNS:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("glmpredict", "housing_test")
        >>>
        # Create a DataFrame on 'housing_test' table.
        >>> df = DataFrame("housing_test")
        >>> df = df.select(["sn", "price", "lotsize", "bedrooms", "bathrms", "stories"])
        >>> df
               price  lotsize  bedrooms  bathrms  stories
        sn
        364  72000.0  10700.0         3        1        2
        13   27000.0   1700.0         3        1        2
        459  44555.0   2398.0         3        1        1
        463  49000.0   2610.0         3        1        2
        260  41000.0   6000.0         2        1        1
        177  70000.0   5400.0         4        1        2
        53   68000.0   9166.0         2        1        1
        440  69000.0   6862.0         3        1        2
        255  61000.0   4360.0         4        1        2
        301  55000.0   4080.0         2        1        1
        >>>
        # Example 1: Find the least values in the columns "price" and "lotsize".
        >>> lt_df = df.assign(lt_col=df.price.least(df.lotsize))
        >>> lt_df
               price  lotsize  bedrooms  bathrms  stories   lt_col
        sn
        364  72000.0  10700.0         3        1        2  10700.0
        13   27000.0   1700.0         3        1        2   1700.0
        459  44555.0   2398.0         3        1        1   2398.0
        463  49000.0   2610.0         3        1        2   2610.0
        260  41000.0   6000.0         2        1        1   6000.0
        177  70000.0   5400.0         4        1        2   5400.0
        53   68000.0   9166.0         2        1        1   9166.0
        440  69000.0   6862.0         3        1        2   6862.0
        255  61000.0   4360.0         4        1        2   4360.0
        301  55000.0   4080.0         2        1        1   4080.0
        >>>
        # Example 2: Find the least values in the columns "price", "lotsize" and 70000.0.
        >>> lt_df = df.assign(lt_col=df.price.least(df.lotsize, 70000))
        >>> lt_df
               price  lotsize  bedrooms  bathrms  stories   lt_col
        sn
        260  41000.0   6000.0         2        1        1   6000.0
        38   67000.0   5170.0         3        1        4   5170.0
        364  72000.0  10700.0         3        1        2  10700.0
        301  55000.0   4080.0         2        1        1   4080.0
        459  44555.0   2398.0         3        1        1   2398.0
        177  70000.0   5400.0         4        1        2   5400.0
        53   68000.0   9166.0         2        1        1   9166.0
        440  69000.0   6862.0         3        1        2   6862.0
        13   27000.0   1700.0         3        1        2   1700.0
        469  55000.0   2176.0         2        1        2   2176.0
        >>>
    DataFrameColumn Logical Functions
    eq
    teradataml.dataframe.sql.DataFrameColumn.eq = eq(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values equal to other or not.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        7      yes  2.33    Novice      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        # Example 1: Get all the students with gpa equal to 3.
        >>> df[df.gpa.eq(3)]
           masters  gpa     stats programming  admitted
        id
        36      no  3.0  Advanced      Novice         0
        # Example 2: Get all the students with gpa equal to 3 and
        #            admitted values equal to 0.
        >>> df[c1.eq(3) & c2.eq(0)]
           masters  gpa     stats programming  admitted
        id
        36      no  3.0  Advanced      Novice         0
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure equal to 415 or income equal to 509.
        >>> df[(df.expenditure.eq(415)) | (df.income.eq(509))]
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
    ge
    teradataml.dataframe.sql.DataFrameColumn.ge = ge(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values greater than or equal to the other or not.
    PARAMETERS:
        other :
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        7      yes  2.33    Novice      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        # Example 1: Get all the students with gpa greater than
        #            or equal to 3.
        >>> df[df.gpa.ge(3)]
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        30     yes  3.79  Advanced      Novice         0
        14     yes  3.45  Advanced    Advanced         0
        37      no  3.52    Novice      Novice         1
        1      yes  3.95  Beginner    Beginner         0
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure greater than or equal to 450 and
        #            investment is greater than or equal to 200.
        >>> df[(df.expenditure.ge(450)) & (df.investment.ge(200))]
           start_time_column end_time_column  expenditure  income  investment
        id
        5           67/06/30        07/07/10        459.0   509.0       211.0
    gt
    teradataml.dataframe.sql.DataFrameColumn.gt = gt(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values greater than the other or not.
    PARAMETERS:
        other :
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        7      yes  2.33    Novice      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        # Example 1: Get all the students with gpa greater than 3.
        >>> df[df.gpa.gt(3)]
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        15     yes  4.00  Advanced    Advanced         1
        30     yes  3.79  Advanced      Novice         0
        14     yes  3.45  Advanced    Advanced         0
        31     yes  3.50  Advanced    Beginner         1
        37      no  3.52    Novice      Novice         1
        1      yes  3.95  Beginner    Beginner         0
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure greater than 400 and investment
        #            greater than 170.
        >>> df[(df.expenditure.gt(400)) & (df.investment.gt(170))]
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
    le
    teradataml.dataframe.sql.DataFrameColumn.le = le(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values less than or equal to other or not.
    PARAMETERS:
    other:
        Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        7      yes  2.33    Novice      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        # Example 1: Get all the students with gpa less than
        #              or equal to 3.
        >>> df[df.gpa.le(3)]
           masters   gpa     stats programming  admitted
        id
        36      no  3.00  Advanced      Novice         0
        24      no  1.87  Advanced      Novice         1
        38     yes  2.65  Advanced    Beginner         1
        19     yes  1.98  Advanced    Advanced         0
        7      yes  2.33    Novice      Novice         1
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure less than or equal to
        #            500 and income less than or equal to 480.
        >>> df[(df.expenditure.le(500)) & (df.income.le(480))]
           start_time_column end_time_column  expenditure  income  investment
        id
        2           67/06/30        07/07/10        421.0   465.0       179.0
        1           67/06/30        07/07/10        415.0   451.0       180.0
    lt
    teradataml.dataframe.sql.DataFrameColumn.lt = lt(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values less than the other or not.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        7      yes  2.33    Novice      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        # Example 1: Get all the students with gpa less than 4.
        >>> df[df.gpa.lt(4)]
           masters   gpa     stats programming  admitted
        id
        5       no  3.44    Novice      Novice         0
        34     yes  3.85  Advanced    Beginner         0
        32     yes  3.46  Advanced    Beginner         0
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        19     yes  1.98  Advanced    Advanced         0
        36      no  3.00  Advanced      Novice         0
        30     yes  3.79  Advanced      Novice         0
        7      yes  2.33    Novice      Novice         1
        17      no  3.83  Advanced    Advanced         1
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure less than 440 and
        #            income greater than 180.
        >>> df[(df.expenditure.lt(440)) & (df.income.lt(180))]
           start_time_column end_time_column  expenditure  income  investment
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0
    ne
    teradataml.dataframe.sql.DataFrameColumn.ne = ne(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values not equal to other.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        7      yes  2.33    Novice      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        # Example 1: Get all the students with gpa values not equal to 3.44.
        >>> df[df.gpa.ne(3.44)]
           masters   gpa     stats programming  admitted
        id
        24      no  1.87  Advanced      Novice         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        19     yes  1.98  Advanced    Advanced         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        7      yes  2.33    Novice      Novice         1
        17      no  3.83  Advanced    Advanced         1
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure not equal to 415 or income
        #            not equal to 400.
        >>> df[(df.expenditure.ne(415)) | (df.income.ne(400))]
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
    DataFrameColumn String Functions
    ascii
    teradataml.dataframe.sql.DataFrameColumn.ascii = ascii()
    DESCRIPTION:
        Function returns the decimal representation of the first character in
        the values of the column as a NUMBER value. The decimal representation will reflect
        the character set of the input string.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns the ascii representation for character string in "stats" column and
        #            pass it as input to DataFrame.assign().
        >>> res = df.assign(col=df.stats.ascii())
        >>> print(res)
           masters   gpa     stats programming  admitted   col
        id
        3       no  3.70    Novice    Beginner         1  78.0
        4      yes  3.50  Beginner      Novice         1  66.0
        2      yes  3.76  Beginner    Beginner         0  66.0
        1      yes  3.95  Beginner    Beginner         0  66.0
        # Example 2: Executed ascii() function on "stats" column and filtered computed
        #            values which are equal to 66.0.
        >>> print(df[df.stats.ascii() == 66.0])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    char
    teradataml.dataframe.sql.DataFrameColumn.char = char()
    DESCRIPTION:
        Function returns the Latin ASCII character of a given numeric code value.
    ALTERNATE NAME:
        chr
    NOTES:
        1. If the value in column is null, then result is null.
        2. Numeric value must be zero or greater.  If value in the column is greater
           than 255, an operation of value mod 256 is executed to return a value
           between 0 and 255.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "employee_info")
        # Create a DataFrame on 'employee_info' table.
        >>> df = DataFrame("employee_info")
        >>> print(df)
                    first_name marks   dob joined_date
        employee_no
        100               abcd  None  None        None
        101              abcde  None  None    02/12/05
        112               None  None  None    18/12/05
        # Example 1: Returns the Latin ASCII character for values in "employee_no" column and pass
        #            it as input to DataFrame.assign().
        >>> df = df.assign(col = df.employee_no.char())
        >>> print(df)
                    first_name marks   dob joined_date col
        employee_no
        100               abcd  None  None        None  d
        101              abcde  None  None    02/12/05  e
        112               None  None  None    18/12/05  p
        # Example 2: Executed char() function on "employee_no" column and filtered computed
        #            values which are equal to 'p'.
        >>> print(df[df.employee_no.char() == 'p'])
                    first_name marks   dob joined_date col
        employee_no
        112               None  None  None    18/12/05  p
    char_length
    teradataml.dataframe.sql.DataFrameColumn.char_length = char_length()
    DESCRIPTION:
        Function returns the number of characters in the values of column.
    ALTERNATE_NAMES:
        character_length
        length
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns length of character string in "programming" column and pass
        #            it as input to DataFrame.assign().
        >>> result = df.assign(col = df.programming.char_length())
        >>> print(result)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  8.0
        4      yes  3.50  Beginner      Novice         1  6.0
        2      yes  3.76  Beginner    Beginner         0  8.0
        1      yes  3.95  Beginner    Beginner         0  8.0
        # Example 2: Executed char_length() function on "programming" column and filtered computed
        #            values which are equal to '6.0'.
        >>> print(df[df.programming.char_length() == 6.0])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
    char2hexint
    teradataml.dataframe.sql.DataFrameColumn.char2hexint = char2hexint()
    DESCRIPTION:
        Function returns the hexadecimal representation for a character string in
        the values of column.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns the hexadecimal representation for character string in "stats" column and pass
        #            it as input to DataFrame.assign().
        >>> result = df.assign(col = df.stats.char2hexint())
        >>> print(result)
           masters   gpa     stats programming  admitted              col
        id
        3       no  3.70    Novice    Beginner         1  4E6F76696365
        4      yes  3.50  Beginner      Novice         1  426567696E6E6572
        2      yes  3.76  Beginner    Beginner         0  426567696E6E6572
        1      yes  3.95  Beginner    Beginner         0  426567696E6E6572
        # Example 2: Executed char2hexint() function on "stats" column and filtered computed
        #            values which are equal to '4E6F76696365'.
        >>> print(df[df.stats.char2hexint() == '4E6F76696365'])
           masters  gpa   stats programming  admitted
        id
        3       no  3.7  Novice    Beginner         1
    character_length
    teradataml.dataframe.sql.DataFrameColumn.character_length = character_length()
    DESCRIPTION:
        Function returns the number of characters in the values of column.
    ALTERNATE_NAMES:
        char_length
        length
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns length of character string in "programming" column and pass
        #            it as input to DataFrame.assign().
        >>> result = df.assign(col = df.programming.character_length())
        >>> print(result)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  8.0
        4      yes  3.50  Beginner      Novice         1  6.0
        2      yes  3.76  Beginner    Beginner         0  8.0
        1      yes  3.95  Beginner    Beginner         0  8.0
        # Example 2: Executed character_length() function on "programming" column and filtered computed
        #            values which are equal to '6.0'.
        >>> print(df[df.programming.character_length() == 6.0])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
    char_length
    teradataml.dataframe.sql.DataFrameColumn.char_length = char_length()
    DESCRIPTION:
        Function returns the number of characters in the values of column.
    ALTERNATE_NAMES:
        character_length
        length
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns length of character string in "programming" column and pass
        #            it as input to DataFrame.assign().
        >>> result = df.assign(col = df.programming.char_length())
        >>> print(result)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  8.0
        4      yes  3.50  Beginner      Novice         1  6.0
        2      yes  3.76  Beginner    Beginner         0  8.0
        1      yes  3.95  Beginner    Beginner         0  8.0
        # Example 2: Executed char_length() function on "programming" column and filtered computed
        #            values which are equal to '6.0'.
        >>> print(df[df.programming.char_length() == 6.0])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
    chr
    teradataml.dataframe.sql.DataFrameColumn.chr = chr()
    DESCRIPTION:
        Function returns the Latin ASCII character of a given numeric code value.
    ALTERNATE NAME:
        char
    NOTES:
        1. If the value in column is null, then result is null.
        2. Numeric value must be zero or greater.  If value in the column is greater
           than 255, an operation of value mod 256 is executed to return a value
           between 0 and 255.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "employee_info")
        # Create a DataFrame on 'employee_info' table.
        >>> df = DataFrame("employee_info")
        >>> print(df)
                    first_name marks   dob joined_date
        employee_no
        100               abcd  None  None        None
        101              abcde  None  None    02/12/05
        112               None  None  None    18/12/05
        # Example 1: Returns the Latin ASCII character for values in "employee_no" column and pass
        #            it as input to DataFrame.assign().
        >>> df = df.assign(col = df.employee_no.chr())
        >>> print(df)
                    first_name marks   dob joined_date col
        employee_no
        100               abcd  None  None        None  d
        101              abcde  None  None    02/12/05  e
        112               None  None  None    18/12/05  p
        # Example 2: Executed chr() function on "employee_no" column and filtered computed
        #            values which are equal to 'p'.
        >>> print(df[df.employee_no.chr() == 'p'])
                    first_name marks   dob joined_date col
        employee_no
        112               None  None  None    18/12/05  p
    concat
    teradataml.dataframe.sql.DataFrameColumn.concat = concat(self, separator, *columns)
    DESCRIPTION:
        Function to concatenate the columns with a separator.
    PARAMETERS:
        separator:
            Required Argument.
            Specifies the string to be used as a separator between two concatenated columns.
            Note:
                This argument is ignored when no column is specified.
            Types: str
        columns:
            Optional Argument.
            Specifies the name(s) of the columns or ColumnExpression(s) to concat on.
            Types: str OR ColumnExpression OR ColumnExpressions
    Returns:
        ColumnExpression
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
        # Example 1: Concatenate the columns "stats" and "programming" with out any seperator.
        >>> df = admissions_train.assign(concat_gpa_=admissions_train.stats.concat("", admissions_train.programming))
        >>> print(df)
            masters   gpa     stats programming  admitted        new_column
        id
        34     yes  3.85  Advanced    Beginner         0  AdvancedBeginner
        32     yes  3.46  Advanced    Beginner         0  AdvancedBeginner
        11      no  3.13  Advanced    Advanced         1  AdvancedAdvanced
        40     yes  3.95    Novice    Beginner         0    NoviceBeginner
        38     yes  2.65  Advanced    Beginner         1  AdvancedBeginner
        36      no  3.00  Advanced      Novice         0    AdvancedNovice
        7      yes  2.33    Novice      Novice         1      NoviceNovice
        26     yes  3.57  Advanced    Advanced         1  AdvancedAdvanced
        19     yes  1.98  Advanced    Advanced         0  AdvancedAdvanced
        13      no  4.00  Advanced      Novice         1    AdvancedNovice
        >>>
        # Example 2: Concatenate the columns "programming", "gpa" and "masters" with '_'.
        >>> df = admissions_train.assign(new_column=admissions_train.programming.concat("_", admissions_train.gpa, "masters"))
        >>> print(df)
           masters   gpa     stats programming  admitted                           new_column
        id
        34     yes  3.85  Advanced    Beginner         0  Beginner_ 3.85000000000000E 000_yes
        32     yes  3.46  Advanced    Beginner         0  Beginner_ 3.46000000000000E 000_yes
        11      no  3.13  Advanced    Advanced         1   Advanced_ 3.13000000000000E 000_no
        40     yes  3.95    Novice    Beginner         0  Beginner_ 3.95000000000000E 000_yes
        38     yes  2.65  Advanced    Beginner         1  Beginner_ 2.65000000000000E 000_yes
        36      no  3.00  Advanced      Novice         0     Novice_ 3.00000000000000E 000_no
        7      yes  2.33    Novice      Novice         1    Novice_ 2.33000000000000E 000_yes
        26     yes  3.57  Advanced    Advanced         1  Advanced_ 3.57000000000000E 000_yes
        19     yes  1.98  Advanced    Advanced         0  Advanced_ 1.98000000000000E 000_yes
        13      no  4.00  Advanced      Novice         1     Novice_ 4.00000000000000E 000_no
    contains
    teradataml.dataframe.sql.DataFrameColumn.contains = contains(self, pattern, case=True, na=None, **kw)
    Search the pattern or substring in the column.
    PARAMETERS:
        pattern:
            Required Argument.
            Specifies a literal value or ColumnExpression. Use ColumnExpression
            when comparison is done based on values inside ColumnExpression or
            based on a ColumnExpression function. Else, use literal value.
            Note:
                 Argument supports regular expressions too.
            Types: str OR ColumnExpression
        case:
            Optional Argument.
            Specifies the case-sentivity match.
            When True, case-sensitive matches, otherwise case-sensitive does not matches.
            Default value: True
            Types: bool
        na:
            Optional Argument.
            Specifies an optional fill value for NULL values in the column
            Types: bool, str, or numeric python literal.
        **kw:
            Optional Argument.
            Specifies optional parameters to pass to regexp_substr
            match_arg:
                A string of characters to use for the match_arg parameter for REGEXP_SUBSTR
                See the Reference for more information about the match_arg parameter.
            Note:
                 Specifying match_arg overrides the case parameter
    RETURNS:
        A numeric Series of values where:
            - Nulls are replaced by the fill parameter
            - A 1 if the value matches the pattern or else 0
        The type of the series is upcasted to support the fill value, if specified.
    EXAMPLES:
        >>> load_example_data("sentimentextractor", "additional_table")
        >>> df = DataFrame("additional_table")
        >>> df
                        polarity_strength
        sentiment_word
        'integral'                      1
        'eagerness'                     1
        'fearfully'                    -1
        irregular'                     -1
        'upgradable'                    1
        'rupture'                      -1
        'imperfect'                    -1
        'rejoicing'                     1
        'comforting'                    1
        'obstinate'                    -1
        >>> sentiment_word = df["sentiment_word"]
        # Example 1: Check if 'in' string is present or not in values in
        #            column 'sentiment_word'.
        >>> df.assign(drop_columns = True,
                     Name = sentiment_word,
                     has_in = sentiment_word.str.contains('in'))
                   Name has_in
        0    'integral'      1
        1   'eagerness'      0
        2   'fearfully'      0
        3    irregular'      0
        4  'upgradable'      0
        5     'rupture'      0
        6   'imperfect'      0
        7   'rejoicing'      1
        8  'comforting'      1
        9   'obstinate'      1
         # Example 2: Check if accounts column contains 'Er' string by ignoring
         #            case sensitivity and specifying a literal for null values.
         >>> df.assign(drop_columns = True,
                       Name = sentiment_word,
                       has_er = sentiment_word.str.contains('ER', case=False, na = 'no value'))
                    Name has_er
         0    'integral'      0
         1   'eagerness'      1
         2   'fearfully'      0
         3    irregular'      0
         4  'upgradable'      0
         5     'rupture'      0
         6   'imperfect'      1
         7   'rejoicing'      0
         8  'comforting'      0
         9   'obstinate'      0
        >>> load_example_data("dataframe", "sales")
        >>> df = DataFrame("sales")
        >>> df
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        # Example 3: Get the all the accounts where accounts has 'Inc' string.
        >>> df[accounts.str.contains('Inc') == True]
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        # Example 4: Get all the accounts where accounts does not
        #            have 'Inc' string.
        >>> df[accounts.str.contains('Inc') == False]
                     Feb  Jan  Mar  Apr    datetime
        accounts
        Jones LLC  200.0  150  140  180  04/01/2017
        Alpha Co   210.0  200  215  250  04/01/2017
        # Example 5: Get all the accounts where accounts has 'Inc' by
        #            specifying numeric literals for True (1).
        >>> df[accounts.str.contains('Inc') == 1]
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        #Example 6: Get all the accounts where accounts has 'Inc' by
        #           specifying numeric literals for False (0).
        >>> df[accounts.str.contains('Inc') == 0]
                     Feb  Jan  Mar  Apr    datetime
        accounts
        Jones LLC  200.0  150  140  180  04/01/2017
        Alpha Co   210.0  200  215  250  04/01/2017
        >>> load_example_data("ntree", "employee_table")
        >>> df = DataFrame("employee_table")
        >>> df
               emp_name  mgr_id mgr_name
        emp_id
        200         Pat   100.0      Don
        300       Donna   100.0      Don
        400         Kim   200.0      Pat
        500        Fred   400.0      Kim
        100         Don     NaN       NA
        # Example 7: Get all the employees whose name has managers name.
        >>> df[df.emp_name.str.contains(df.mgr_name) == True]
        >>> df
               emp_name  mgr_id mgr_name
        emp_id
        300       Donna     100      Don
    count_delimiters
    teradataml.dataframe.sql.DataFrameColumn.count_delimiters = count_delimiters(self, delimiter)
    DESCRIPTION:
        Function to count the total number of occurrences of a specified delimiter.
    PARAMETERS:
        delimiter:
            Required Argument.
            Specifies the delimiter to count in the column values.
            Types: str
    RETURNS:
        ColumnExpression.
    EXAMPLES:
    # Load sample data
    >>> load_example_data("dataframe", "admissions_train")
    >>> df = DataFrame("admissions_train")
    # Create a DataFrame with a column containing delimiters.
    >>> df1 = df.assign(delim_col = 'ab.c.def.g')
    >>> df1
       masters   gpa     stats programming  admitted   delim_col
    id                                                          
    38     yes  2.65  Advanced    Beginner         1  ab.c.def.g
    7      yes  2.33    Novice      Novice         1  ab.c.def.g
    26     yes  3.57  Advanced    Advanced         1  ab.c.def.g
    # Example 1: Count the number of periods in column 'delim_col'.
    >>> res = df1.assign(dot_count = df1.delim_col.count_delimiters('.'))
    >>> res
       masters   gpa     stats programming  admitted   delim_col  dot_count
    id                                                                     
    38     yes  2.65  Advanced    Beginner         1  ab.c.def.g          3
    7      yes  2.33    Novice      Novice         1  ab.c.def.g          3
    26     yes  3.57  Advanced    Advanced         1  ab.c.def.g          3
    # Example 2: Count multiple delimiters in a string.
    >>> df2 = df.assign(delim_col = 'a,b;c;d-e')
    >>> res = df2.assign(
    ...     comma_count = df2.delim_col.count_delimiters(','),
    ...     semicolon_count = df2.delim_col.count_delimiters(';'),
    ...     colon_count = df2.delim_col.count_delimiters(':'),
    ...     dash_count = df2.delim_col.count_delimiters('-')
    ... )
    >>> res
       masters   gpa     stats programming  admitted  delim_col colon_count comma_count dash_count  semicolon_count
    id                                                                                                                
    38     yes  2.65  Advanced    Beginner         1  a,b;c;d-e           0           1          1                2
    7      yes  2.33    Novice      Novice         1  a,b;c;d-e           0           1          1                2
    26     yes  3.57  Advanced    Advanced         1  a,b;c;d-e           0           1          1                2
    5       no  3.44    Novice      Novice         0  a,b;c;d-e           0           1          1                2
    edit_distance
    teradataml.dataframe.sql.DataFrameColumn.edit_distance = edit_distance(expression, ci=1, cd=1, cs=1, ct=1)
    DESCRIPTION:
        Function returns the minimum number of edit operations (insertions, deletions,
        substitutions and transpositions) required to transform string1 (values in the column)
        into "expression" (value passed in argument).
        Edit distance measures the similarity between two strings. A low number of deletions,
        insertions, substitutions or transpositions implies a high similarity. The insertions,
        deletions, substitutions, and transpositions are based on the Damerau-Levenshtein
        Distance algorithm with modifications for costed operations.
    ALTERNATE_NAME:
        levenshtein
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Support column types are: CHARACTER, VARCHAR, or CLOB.
            Types: ColumnExpression, str
        ci:
            Optional Argument.
            Specifies the relative cost of an insert operation.
            The value specified must be a non-negative integer.
            Default Value: 1
            Types: int
        cd:
            Optional Argument.
            Specifies the relative cost of a delete operation.
            The value specified must be a non-negative integer.
            Default Value: 1
            Types: int
        cs:
            Optional Argument.
            Specifies the relative cost of a substitute operation.
            The value specified must be a non-negative integer.
            Default Value: 1
            Types: int
        ct:
            Optional Argument.
            Specifies the relative cost of a transpose operation.
            The value specified must be a non-negative integer.
            Default Value: 1
            Types: int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculate the edit distance between values in "stats" and "programming"
        #            columns and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.edit_distance(df.programming))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1    6
        4      yes  3.50  Beginner      Novice         1    6
        2      yes  3.76  Beginner    Beginner         0    0
        1      yes  3.95  Beginner    Beginner         0    0
        # Example 2: Calculate the edit distance between values in "stats" and "programming"
        #            columns with cost associated with the edit operations passed and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.edit_distance(df.programming, 2, 1, 1, 2))
        >>> print(res)
           masters   gpa     stats programming  admitted  editdistance_func  col
        id
        3       no  3.70    Novice    Beginner         1                  8    8
        4      yes  3.50  Beginner      Novice         1                  6    6
        2      yes  3.76  Beginner    Beginner         0                  0    0
        1      yes  3.95  Beginner    Beginner         0                  0    0
        # Example 3: Executed edit_distance() function on "stats" column and filtered computed
        #            values which are equal to 8.
        >>> print(df[df.stats.edit_distance(df.programming, 2, 1, 1, 2) == 8])
           masters  gpa   stats programming  admitted
        id
        3       no  3.7  Novice    Beginner         1
    endswith
    teradataml.dataframe.sql.DataFrameColumn.endswith = endswith(self, other)
    DESCRIPTION:
        Function to check whether the column value ends with the specified value or not.
    PARAMETERS:
        other:
            Required Argument.
            Specifies a string literal or ColumnExpression to match.
            Types: str OR ColumnExpression
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        >>> load_example_data("ntree", "employee_table")
        >>> df = DataFrame("employee_table")
        >>> df = df.assign(new_col = 'on')
               emp_name  mgr_id mgr_name new_col
        emp_id
        300       Donna   100.0      Don      on
        500        Fred   400.0      Kim      on
        100         Don     NaN       NA      on
        400         Kim   200.0      Pat      on
        200         Pat   100.0      Don      on
        # Example 1: Find out the employees whose manager name ends
        #            with values in column 'new_col'.
        >>> df[df.mgr_name.endswith(df.new_col)]
               emp_name  mgr_id mgr_name new_col
        emp_id
        300       Donna     100      Don      on
        200         Pat     100      Don      on
        # Example 2: Find out the employees whose name starts with
        #            'D' and ends with 'n'.
        >>> df[df.emp_name.startswith('D') & df.emp_name.endswith('n')]
               emp_name mgr_id mgr_name new_col
        emp_id
        100         Don   None       NA      on
        # Example 3: Create a new column with values as
        #            1, if employees manager name ends with 'im'.
        #            0, else.
        >>> df.assign(new_col=case_when((df.mgr_name.endswith('im').expression, 1), else_=0))
               emp_name  mgr_id mgr_name new_col
        emp_id
        300       Donna   100.0      Don       0
        500        Fred   400.0      Kim       1
        100         Don     NaN       NA       0
        400         Kim   200.0      Pat       0
        200         Pat   100.0      Don       0
    format
    teradataml.dataframe.sql.DataFrameColumn.format = format(self, formatter)
    DESCRIPTION:
        Function to format the values in column based on formatter.
    PARAMETERS:
        formatter:
            Required Argument.
            Specifies a variable length string containing formatting characters
            that define the display format for the data type.
            Formats can be specified for columns that have character, numeric, byte,
            DateTime, Period or UDT data types.
            Note:
                If the "formatter" does not include a sign character or a signed zoned decimal character,
                then the sign for a negative value is discarded and the output is displayed as a positive
                number.
            Types: str
            Following tables provide information about different formatters on numeric and
            date/time type columns:
                  The formatting characters determine whether the output of numeric data is considered
                  to be monetary or non-monetary. Numeric information is considered to be monetary if the
                  "formatter" contains a currency symbol.
                  Note:
                      Formatting characters are case insensitive.
                  The result of a formatted numeric string is right-justified.
                +--------------------------------------------------------------------------------------------------+
                |    formatter                                   description                                       |
                +--------------------------------------------------------------------------------------------------+
                |    G              Invokes the currency grouping rule defined by CurrencyGroupingRule in the SDF. |
                |                   The value of CurrencyGroupSeparator in the current SDF is copied to the output |
                |                   string to separate groups of digits to the left of the currency radix separator|
                |                   ,according to the currency grouping rule defined by CurrencyGroupingRule.      |
                |                   Grouping applies to only the integer portion of floating numbers.              |
                |                   The G must appear as the first character in a "formatter".                     |
                |                   A currency character, such as L or C, must also appear in the "formatter".     |
                |                   If the "formatter" does not contain the letter G, no grouping is               |
                |                   done on the output string.                                                     |
                |                   The G cannot appear in a "formatter" that contains any of the following        |
                |                   characters:                                                                    |
                |                       * ,                                                                        |
                |                       * .                                                                        |
                |                       * /                                                                        |
                |                       * :                                                                        |
                |                       * S                                                                        |
                |                   Examples:                                                                      |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    -12345678.90       'G--(8)D9(2)'         -12,345,678.90    |          |
                |                       |    1234567890         'G-(10)9'             1.234.567.890     |          |
                |                       |    9988.77            'GLLZ(I)D9(F)'        $9,988.77         |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    /              Insertion characters.                                                          |
                |                                                                                                  |
                |    :              Copied to output string where they appear in the "formatter".                  |
                |                                                                                                  |
                |    %              The % insertion character cannot appear in a "formatter" that contains S,      |
                |                   and cannot appear between digits in a "formatter" that contains G, D, or E.    |
                |                   For example, GL9999D99% is valid, but L9(9)D99%E999 is not.                    |
                |                                                                                                  |
                |                   The / and : insertion characters cannot appear in a "formatter" that           |
                |                   contains any of the following characters:                                      |
                |                        * G                                                                       |
                |                        * D                                                                       |
                |                        * S                                                                       |
                |                   Examples:                                                                      |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    1234567891         '9(8)/9(2)'           12345678/91       |          |
                |                       |    1234567891         '9(8):9(2)'           12345678:91       |          |
                |                       |    1234567891         '9(8)%9(2)'           12345678%91       |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    B             Insertion character.                                                            |
                |                  A blank space is copied to the output string wherever a B appears in the FORMAT |
                |                  phrase.                                                                         |
                |                  B cannot appear between digits in a "formatter" that contains G, D, or E.       |
                |                  For example, GNB99D99 is valid, but G9(9)BD99 is not.                           |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    998877.66         '-Z(I)BN'              998878 US Dollars |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    +             Sign characters.                                                                |
                |                                                                                                  |
                |    -             These characters can appear at the beginning or end of a format string,         |
                |                  but cannot appear between Z or 9 characters, or between repeated currency       |
                |                  characters. One sign character places the edit character in a fixed position    |
                |                  for the output string.                                                          |
                |                  If two or more of these characters are present, the sign floats (moves to the   |
                |                  position just to the left of the number as determined by the stated structure). |
                |                  Repeated sign characters must appear to the left of formatting characters       |
                |                  consisting of a combination of the radix and any 9 formatting characters.       |
                |                                                                                                  |
                |                  If a group of repeated sign characters appears in a "formatter" with a group    |
                |                  of repeated Z characters or a group of repeated currency characters or both, the|
                |                  groups must be contiguous. For example, +++$$$ZZZ.                              |
                |                  One trailing sign character can occur to the right of any digits, and can       |
                |                  combine with B and one currency character or currency sign. For example,        |
                |                  G9(I)B+L. The trailing sign character for a mantissa cannot appear to the right |
                |                  of the exponent.                                                                |
                |                  For example, 999D999E+999+ is invalid.                                          |
                |                                                                                                  |
                |                  The + translates to + or - as appropriate; the - translates to - or blank.      |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    0034567890         '--(8)D9(2)'           34567890.00      |          |
                |                       |    -0034567890        '++(8)D9(2)'          -34567890.00      |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |    $             Currency signs:                                                                 |
                |                  * $ means Dollar sign.                                                          |
                |    £             * £ means Pound sterling.                                                       |
                |                  * ¥ means Yen sign.                                                             |
                |    ¥             * ¤ means general currency sign.                                                |
                |                                                                                                  |
                |    ¤             A currency sign cannot appear between Z or 9 formatting characters, or between  |
                |                  repeated sign characters.                                                       |
                |                  One currency sign places the edit character in a fixed position for the output  |
                |                  string.                                                                         |
                |                  If a result is formatted using a single currency sign with Zs for               |
                |                  zero-suppressed decimal digits (for example, £ZZ9.99), blanks can occur between |
                |                  the currency sign and the leftmost nonzero digit of the number.                 |
                |                                                                                                  |
                |                  If the same currency sign appears more than once, the sign floats to the right, |
                |                  leaving no blanks between it and the leftmost digit.                            |
                |                                                                                                  |
                |                  A currency sign cannot appear in the same phrase with a currency character,     |
                |                  such as L.                                                                      |
                |                  If + or - is present, the currency character cannot precede it.                 |
                |                                                                                                  |
                |                  If a group of repeated currency signs appears in a "formatter" with a group of  |
                |                  repeated sign characters or a group of repeated Z characters or both, the groups|
                |                  must be contiguous. For example, +++$$$ZZZ.                                     |
                |                                                                                                  |
                |                  One currency sign can occur to the right of any digits, and can combine with B  |
                |                  and one trailing sign character. For example, G9(I)B+$.                         |
                |                                                                                                  |
                |                  A currency sign cannot appear to the right of an exponent. For example,         |
                |                  999D999E+999B+$ is invalid.                                                     |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    1                 '$(5).9(2)'            $1.00             |          |
                |                       |    .069              '$$9.99'               $0.07             |          |
                |                       |    1095              '$$9.99'               ******            |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    L             Currency characters.                                                            |
                |                  The value of the corresponding currency string in the current SDF is copied to  |
                |    C             the output string whenever the character appears in the "formatter".            |
                |                      * L in a "formatter" is interpreted as the currency symbol and the value    |
                |    N                   of the Currency string in the SDF is copied to the output string.         |
                |                      * C in a "formatter" is interpreted as the ISO currency symbol and the      |
                |    O                   value of the ISOCurrency string in the SDF is copied to the output        |
                |                        string.                                                                   |
                |                      * N in a "formatter" is interpreted as the full currency name, such as Yen  |
                |    U                   or Kroner, and the value of the CurrencyName string in the SDF is copied  |
                |                        to the output string.                                                     |
                |    A                 * O in a "formatter" is interpreted as the dual currency symbol and the     |
                |                        value of the DualCurrency string in the SDF is copied to the output       |
                |                        string.                                                                   |
                |                      * U in a "formatter" is interpreted as the dual ISO currency symbol and     |
                |                        the value of the DualISOCurrency string in the SDF is copied to the       |
                |                        output string.                                                            |
                |                      * A in a "formatter" is interpreted as the full dual currency name,         |
                |                        such as Euro, and the value of the DualCurrencyName string in the SDF     |
                |                        is copied to the output string.                                           |
                |                                                                                                  |
                |                  A currency character cannot appear between Z or 9 formatting characters, or     |
                |                  between repeated sign characters.                                               |
                |                  If the same currency character appears more than once, the value that is        |
                |                  copied to the output string floats to the right, leaving no blanks between it   |
                |                  and the leftmost digit. Repeated characters must be contiguous, and must appear |
                |                  to the left formatting characters consisting of a combination of the radix and  |
                |                  any 9 formatting characters.                                                    |
                |                                                                                                  |
                |                  If a group of repeated currency characters appears in a "formatter" with a      |
                |                  group of repeated sign characters or a group of repeated Z characters or both,  |
                |                  the groups must be contiguous. For example, +++LLLZZZ.                          |
                |                  A currency character cannot appear in the same phrase with any of the following |
                |                  characters:                                                                     |
                |                      * other currency characters                                                 |
                |                      * a currency sign, such as $ or £                                           |
                |                      * ,                                                                         |
                |                      * .                                                                         |
                |                  One currency character can occur to the right of any digits, and can combine    |
                |                  with B and one trailing sign character. For example, G9(I)B+L.                  |
                |                                                                                                  |
                |                  A currency character cannot appear to the right of an exponent. For example,    |
                |                  999D999E+999B+L is invalid.                                                     |
                |                  Examples:                                                                       |
                |                       +----------------------------------------------------------------+         |
                |                       |    data               formatter             result             |         |
                |                       +----------------------------------------------------------------+         |
                |                       |    9988.77            'GLLZ(I)D9(F)'        $9,988.77          |         |
                |                       |    9988.77            'GCBZ(4)D9(F)'        USD 9,988.77       |         |
                |                       |    9988.77            'GNBZ(4)D9(F)'        US Dollars 9,988.77|         |
                |                       +----------------------------------------------------------------+         |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    V             Implied decimal point position.                                                 |
                |                  Internally, the V is recognized as a decimal point to align the numeric value   |
                |                  properly for calculation.                                                       |
                |                  Because the decimal point is implied, it does not occupy any space in storage   |
                |                  and is not included in the output.                                              |
                |                  V cannot appear in a "formatter" that contains the 'D' radix symbol or          |
                |                  the '.' radix character.                                                        |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    128.457            '999V99'              12846             |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    Z             Zero-suppressed decimal digit.                                                  |
                |                  Translates to blank if the digit is zero and preceding digits are also zero.    |
                |                                                                                                  |
                |                  A Z cannot follow a 9.                                                          |
                |                                                                                                  |
                |                  Repeated Z characters must appear to the left of any combination of the radix   |
                |                  and any 9 formatting characters.                                                |
                |                                                                                                  |
                |                  The characters to the right of the radix cannot be a combination of 9 and Z     |
                |                  characters; they must be all 9s or all Zs. If they are all Zs, then the         |
                |                  characters to the left of the radix must also be all Zs.                        |
                |                                                                                                  |
                |                  If a group of repeated Z characters appears in a "formatter" with a group of    |
                |                  repeated sign characters, the group of Z characters must immediately follow     |
                |                  the group of sign characters. For example, --ZZZ.                               |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    1.3451             'zz.z'                1.3               |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    9             Decimal digit (no zero suppress).                                               |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    0034567890         '9999999999'           0034567890       |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    E             For exponential notation.                                                       |
                |                                                                                                  |
                |                  Defines the end of the mantissa and the start of the exponent.                  |
                |                                                                                                  |
                |                  The exponent consists of one optional + or - sign character followed by one or  |
                |                  more 9 formatting characters.                                                   |
                |                  Examples:                                                                       |
                |                       +-------------------------------------------------------------------+      |
                |                       |    data                     formatter             result          |      |
                |                       +-------------------------------------------------------------------+      |
                |                       |1095                     '9.99E99'             1.10E03             |      |
                |                       |1.74524064372835e-2 '-9D99999999999999E-999'  1.74524064372835E-002|      |
                |                       +-------------------------------------------------------------------+      |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    CHAR(n)       For more than one occurrence of a character, where CHAR can be one of the       |
                |                  following:                                                                      |
                |                      * - (sign character)                                                        |
                |                      * +                                                                         |
                |                      * Z                                                                         |
                |                      * 9                                                                         |
                |                      * $                                                                         |
                |                      * ¤                                                                         |
                |                      * ¥                                                                         |
                |                      * £                                                                         |
                |                  and n can be:                                                                   |
                |                      * an integer constant                                                       |
                |                      * I                                                                         |
                |                      * F                                                                         |
                |                  If n is F, CHAR can only be Z or 9.                                             |
                |                                                                                                  |
                |                  If n is an integer constant, the (n) notation means that the character repeats n|
                |                  number of times. For the meanings of I and F, see the definitions later in this |
                |                  table.                                                                          |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    0034567890         'z(8)D9(2)'           34567890.00       |          |
                |                       |    -0034567890        '+z(8)D9(2)'          -34567890.00      |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    ,             Currency grouping character.                                                    |
                |                  The comma is inserted only if a digit has already appeared.                     |
                |                                                                                                  |
                |                  The comma is interpreted as the currency grouping character regardless of the   |
                |                  value of the CurrencyGroupSeparator in the SDF.                                 |
                |                                                                                                  |
                |                  The comma cannot appear in a "formatter" that contains any of the following     |
                |                  characters:                                                                     |
                |                      * G                                                                         |
                |                      * D                                                                         |
                |                      * L                                                                         |
                |                      * C                                                                         |
                |                      * O                                                                         |
                |                      * U                                                                         |
                |                      * N                                                                         |
                |                      * A                                                                         |
                |                      * S                                                                         |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    0034567890         'z(7),z'              3456789,0         |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    .             Currency radix character.                                                       |
                |                  The period is interpreted as the currency radix character, regardless of the    |
                |                  value of the CurrencyRadixSeparator in the SDF, and is copied to the output     |
                |                  string.                                                                         |
                |                                                                                                  |
                |                  The period cannot appear in a "formatter" that contains any of the following    |
                |                  characters:                                                                     |
                |                      * G                                                                         |
                |                      * D                                                                         |
                |                      * L                                                                         |
                |                      * V                                                                         |
                |                      * C                                                                         |
                |                      * O                                                                         |
                |                      * U                                                                         |
                |                      * N                                                                         |
                |                      * A                                                                         |
                |                      * S                                                                         |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    0034567890         'z(8).z'              34567890.0        |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    D             Radix symbol.                                                                   |
                |                  The value of CurrencyRadixSeparator in the current SDF is copied to the output  |
                |                  string whenever a D appears in the "formatter".                                 |
                |                  A currency symbol, such as a dollar sign or yen sign, must also appear in the   |
                |                  "formatter".                                                                    |
                |                  The D cannot appear in a "formatter" that contains any of the following         |
                |                  characters:                                                                     |
                |                      * ,                                                                         |
                |                      * .                                                                         |
                |                      * /                                                                         |
                |                      * :                                                                         |
                |                      * S                                                                         |
                |                      * V                                                                         |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    0034567890         '--(8)D9(2)'           34567890.00      |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    I             The number of characters needed to display the integer portion of numeric and   |
                |                  integer data.                                                                   |
                |                                                                                                  |
                |                  I can only appear as n in the CHAR(n) character sequence (see the definition of |
                |                  CHAR(n) earlier in this table), where CHAR can be:                              |
                |                      * - (sign character)                                                        |
                |                      * +                                                                         |
                |                      * Z                                                                         |
                |                      * 9                                                                         |
                |                      * $                                                                         |
                |                      * ¤                                                                         |
                |                      * ¥                                                                         |
                |                      * £                                                                         |
                |                  CHAR(I) can only appear once, and is valid for the following types:             |
                |                      * DECIMAL/NUMERIC                                                           |
                |                      * BYTEINT                                                                   |
                |                      * SMALLINT                                                                  |
                |                      * INTEGER                                                                   |
                |                      * BIGINT                                                                    |
                |                 The value of I is resolved during the formatting of the monetary numeric data.   |
                |                 The value is obtained from the declaration of the data type. For example, I is   |
                |                 eight for the DECIMAL(10,2) type.                                                |
                |                                                                                                  |
                |                 If CHAR(F) also appears in the "formatter", CHAR(F) must appear to the right of  |
                |                 CHAR(I), and one of the following characters must appear between CHAR(I) and     |
                |                 CHAR(F):                                                                         |
                |                     * D                                                                          |
                |                     * .                                                                          |
                |                     * V                                                                          |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    000000.42         'Z(I)D9(F)'            .42               |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |    F             The number of characters needed to display the fractional portion of            |
                |                  numeric data.                                                                   |
                |                  F can only appear as n in the CHAR(n) character sequence                        |
                |                  (see the definition of CHAR(n) earlier in this table),                          |
                |                  where CHAR can be:                                                              |
                |                      * Z                                                                         |
                |                      * 9                                                                         |
                |                  CHAR(F) is valid for the DECIMAL/NUMERIC data type.                             |
                |                                                                                                  |
                |                  The value of F is resolved during the formatting of the monetary numeric data.  |
                |                  The value is obtained from the declaration of the data type. For example,       |
                |                  F is two for the DECIMAL(10,2) type.                                            |
                |                                                                                                  |
                |                  A value of zero for F displays no fractional precision for the data; however,   |
                |                  the value of CurrencyRadixSeparator in the current SDF is copied to the output  |
                |                  string if a D appears in the "formatter".                                       |
                |                                                                                                  |
                |                  CHAR(F) can appear only once. If CHAR(I) also appears in the "formatter",       |
                |                  CHAR(F) must appear to the right of CHAR(I), and one of the following characters|
                |                  must appear between CHAR(I) and CHAR(F):                                        |
                |                      * D                                                                         |
                |                      * .                                                                         |
                |                      * V                                                                         |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    000000.42         'Z(I)D9(F)'            .42               |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    -             Dash character.                                                                 |
                |                  Used when storing numbers such as telephone numbers, social security numbers,   |
                |                  and account numbers.                                                            |
                |                                                                                                  |
                |                  A dash appears after the first digit and before the last digit, and is treated  |
                |                  as an embedded dash rather than a sign character. A dash cannot follow any of   |
                |                  these characters:                                                               |
                |                      * .                                                                         |
                |                      * ,                                                                         |
                |                      * +                                                                         |
                |                      * G                                                                         |
                |                      * N                                                                         |
                |                      * A                                                                         |
                |                      * C                                                                         |
                |                      * L                                                                         |
                |                      * O                                                                         |
                |                      * U                                                                         |
                |                      * D                                                                         |
                |                      * V                                                                         |
                |                      * S                                                                         |
                |                      * E                                                                         |
                |                      * $                                                                         |
                |                      * ¤                                                                         |
                |                      * £                                                                         |
                |                      * ¥                                                                         |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    0034567890         '--(8)D9(2)'           34567890.00      |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    S             Signed Zoned Decimal character.                                                 |
                |                  Defines signed zoned decimal input as a numeric data type and displays numeric  |
                |                  output as signed zone decimal character strings.                                |
                |                                                                                                  |
                |                  When converting signed zone decimal input to a numeric data type, the final     |
                |                  character is converted as follows:                                              |
                |                  * Last character = { or 0, then the numeric conversion is n … 0                 |
                |                  * Last character = A or 1, then the numeric conversion is n … 1                 |
                |                  * Last character = B or 2, then the numeric conversion is n … 2                 |
                |                  * Last character = C or 3, then the numeric conversion is n … 3                 |
                |                  * Last character = D or 4, then the numeric conversion is n … 4                 |
                |                  * Last character = E or 5, then the numeric conversion is n … 5                 |
                |                  * Last character = F or 6, then the numeric conversion is n … 6                 |
                |                  * Last character = G or 7, then the numeric conversion is n … 7                 |
                |                  * Last character = H or 8, then the numeric conversion is n … 8                 |
                |                  * Last character = I or 9, then the numeric conversion is n … 9                 |
                |                  * Last character = }, then the numeric conversion is -n … 0                     |
                |                  * Last character = J, then the numeric conversion is -n … 1                     |
                |                  * Last character = K, then the numeric conversion is -n … 2                     |
                |                  * Last character = L, then the numeric conversion is -n … 3                     |
                |                  * Last character = M, then the numeric conversion is -n … 4                     |
                |                  * Last character = N, then the numeric conversion is -n … 5                     |
                |                  * Last character = O, then the numeric conversion is -n … 6                     |
                |                  * Last character = P, then the numeric conversion is -n … 7                     |
                |                  * Last character = Q, then the numeric conversion is -n … 8                     |
                |                  * Last character = R, then the numeric conversion is -n … 9                     |
                |                                                                                                  |
                |                  When displaying numeric output as signed zone decimal character strings,        |
                |                  the final character indicates the sign, as follows:                             |
                |                                                                                                  |
                |                  If the final data digit is 0, then the final result digit is displayed as:      |
                |                  * { if the result is a positive number                                          |
                |                  * } if the result is a negative number                                          |
                |                  If the final data digit is 1, then the final result digit is displayed as:      |
                |                  * A if the result is a positive number                                          |
                |                  * J if the result is a negative number                                          |
                |                  If the final data digit is 2, then the final result digit is displayed as:      |
                |                  * B if the result is a positive number                                          |
                |                  * K if the result is a negative number                                          |
                |                  If the final data digit is 3, then the final result digit is displayed as:      |
                |                  * C if the result is a positive number                                          |
                |                  * L if the result is a negative number                                          |
                |                  If the final data digit is 4, then the final result digit is displayed as:      |
                |                  * D if the result is a positive number                                          |
                |                  * M if the result is a negative number                                          |
                |                  If the final data digit is 5, then the final result digit is displayed as:      |
                |                  * E if the result is a positive number                                          |
                |                  * N if the result is a negative number                                          |
                |                  If the final data digit is 6, then the final result digit is displayed as:      |
                |                  * F if the result is a positive number                                          |
                |                  * O if the result is a negative number                                          |
                |                  If the final data digit is 7, then the final result digit is displayed as:      |
                |                  * G if the result is a positive number                                          |
                |                  * P if the result is a negative number                                          |
                |                  If the final data digit is 8, then the final result digit is displayed as:      |
                |                  * H if the result is a positive number                                          |
                |                  * Q if the result is a negative number                                          |
                |                  If the final data digit is 9, then the final result digit is displayed as:      |
                |                  * I if the result is a positive number                                          |
                |                  * R if the result is a negative number                                          |
                |                                                                                                  |
                |                  The S must follow the last decimal digit in the "formatter". It cannot appear   |
                |                  in the same phrase with the following characters.                               |
                |                    * %                                                                           |
                |                    * +                                                                           |
                |                    * :                                                                           |
                |                    * /                                                                           |
                |                    * -                                                                           |
                |                    * ,                                                                           |
                |                    * .                                                                           |
                |                    * Z                                                                           |
                |                    * E                                                                           |
                |                    * D                                                                           |
                |                    * G                                                                           |
                |                    * F                                                                           |
                |                    * N                                                                           |
                |                    * A                                                                           |
                |                    * C                                                                           |
                |                    * L                                                                           |
                |                    * U                                                                           |
                |                    * O                                                                           |
                |                    * $                                                                           |
                |                    * ¤                                                                           |
                |                    * £                                                                           |
                |                    * ¥                                                                           |
                |                  Examples:                                                                       |
                |                       +---------------------------------------------------------------+          |
                |                       |    data               formatter             result            |          |
                |                       +---------------------------------------------------------------+          |
                |                       |    -1095              '99999S'              0109N             |          |
                |                       |    1095               '99999S'              0109E             |          |
                |                       +---------------------------------------------------------------+          |
                +--------------------------------------------------------------------------------------------------+
                A "formatter" that defines fewer positions than are required by numeric values causes
                the data to be returned as follows:
                    * Asterisks appear when the integer portion cannot be accommodated.
                    * When only the integer portion can be accommodated, any digits to the right of the least
                    significant digit are either truncated (for an integer value) or rounded (for a floating,
                    number, or decimal value).
                Rounding is based on “Round to the Nearest” mode, as illustrated by the following process.
                    * Let B represent the actual result.
                    * Let A and C represent the nearest bracketing values that can be represented, such that
                      A < B < C.
                    * The determination as to whether A or C is the represented result is made as follows:
                        * When possible, the result is the value nearest to B.
                        * If A and C are equidistant (for example, the fractional part is exactly .5),
                          the result is the even number.
            * Date type FORMAT to SPECIFIC FORMAT
              The date and time formatting characters in a "formatter" determine the output of
              DATE, TIME, and TIMESTAMP teradatasqlalchemy types information.
            Following tables provide information about different formatters on
            date type columns:
                +--------------------------------------------------------------------------------------------------+
                |    formatter                                          description                                |
                +--------------------------------------------------------------------------------------------------+
                |    MMMM                                Represent the month as a full month name, such as         |
                |                                        November.                                                 |
                |    M4                                  Valid names are specified by LongMonths in the            |
                |                                        current SDF.                                              |
                |                                        M4 is equivalent to MMMM, and is preferable to allow for a|
                |                                        shorter, unambiguous format string.                       |
                |                                                                                                  |
                |                                        You cannot specify M4 in a format that also has M3 or MM. |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'M4'           JANUARY              | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    MMM                                 Represent the month as an abbreviated month name, such as |
                |                                        'Apr' for April.                                          |
                |    M3                                  Valid names are specified by ShortMonths in the current   |
                |                                        SDF.                                                      |
                |                                                                                                  |
                |                                        M3 is equivalent to MMM, and is preferable to allow for a |
                |                                        shorter, unambiguous format string.                       |
                |                                                                                                  |
                |                                        You cannot specify MMM in a format that also has MM.      |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'M4'           Jan                  | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    MM                                  Represent the month as two numeric digits.                |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'MM'           01                   | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    DDD                                 Represent the date as the sequential day in the year,     |
                |                                        using three numeric digits, such as '032' as February 1.  |
                |                                                                                                  |
                |    D3                                  D3 is equivalent to DDD, and allows for a shorter format  |
                |                                        string.                                                   |
                |                                        You cannot specify DDD or D3 in a format that also has DD.|
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/02/13       'D3'           044                  | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    DD                                  Represent the day of the month as two numeric digits.     |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'DD'           16                   | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    YYYY                                Represent the year as four numeric digits.                |
                |                                                                                                  |
                |    Y4                                  Y4 is equivalent to YYYY, and allows for a shorter format |
                |                                        string.                                                   |
                |                                                                                                  |
                |                                        You cannot specify YYYY or Y4 in a format that also has   |
                |                                        YY.                                                       |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'Y4'           2019                 | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    YY                                  Represent the year as two numeric digits.                 |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'YY'           19                   | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    EEEE                                Represent the day of the week using the full name,        |
                |                                        such as Thursday.                                         |
                |                                                                                                  |
                |    E4                                  Valid names are specified by LongDays in the current SDF. |
                |                                                                                                  |
                |                                        E4 is equivalent to EEEE, and allows for a shorter format |
                |                                        string.                                                   |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'E4'           Wednesday            | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    EEE                                 Represent the day of the week as an abbreviated name, such|
                |                                        as 'Mon' for Monday.                                      |
                |    E3                                  Valid abbreviations are specified by ShortDays in the     |
                |                                        current SDF.                                              |
                |                                        E3 is equivalent to EEE, and allows for a shorter format  |
                |                                        string.                                                   |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'E3'            Wed                 | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    /                                   Slash separator.                                          |
                |                                        Copied to output string where it appears in the FORMAT    |
                |                                        phrase.                                                   |
                |                                        This is the default separator for Teradata dates.         |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'M4/E3'        January/Wed          | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    B                                   Blank representation separator.                           |
                |                                                                                                  |
                |    b                                   Use this instead of a space to represent a blank.         |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'M4BE3'        JANUARY Wed          | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    ,                                   Comma separator.                                          |
                |                                        Copied to output string where it appears in the FORMAT    |
                |                                        phrase.                                                   |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'M4,E3'        January,Wed          | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    :                                   Colon separator.                                          |
                |                                        Copied to output string where it appears in the FORMAT    |
                |                                        phrase.                                                   |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'M4:E3'        January:Wed          | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    .                                   Period separator.                                         |
                |                                        Copied to output string where it appears in the FORMAT    |
                |                                        phrase.                                                   |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'M4.E3'        January.Wed          | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    -                                   Dash separator.                                           |
                |                                        Copied to output string where it appears in the FORMAT    |
                |                                        phrase.                                                   |
                |                                        This is the default separator for ANSI dates.             |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'M4-E3'        January-Wed          | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    9                                   Decimal digit.                                            |
                |                                        This formatting character can only be used with separators|
                |                                        less than 0x009F.                                         |
                |                                                                                                  |
                |                                        The 9(n) notation can be used for more than one occurrence|
                |                                        of this character, where n is an integer constant and     |
                |                                        means that the '9' repeats n number of times.             |
                |                                                                                                  |
                |                                        This formatting character is for DATE and PERIOD(DATE)    |
                |                                        types only and cannot appear as a date formatting         |
                |                                        character for PERIOD(TIMESTAMP) and TIMESTAMP types.      |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       '999999'       190116               | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    Z                                   Zero-suppressed decimal digit.                            |
                |                                        This formatting character can only be used with separators|
                |                                        less than 0x009F.                                         |
                |                                                                                                  |
                |                                        The Z(n) notation can be used for more than one occurrence|
                |                                        of this character, where n is an integer constant and     |
                |                                        means that the 'Z' repeats n number of times.             |
                |                                                                                                  |
                |                                        This formatting character is for DATE and PERIOD(DATE)    |
                |                                        types only and cannot appear as a date formatting         |
                |                                        character for PERIOD(TIMESTAMP) and TIMESTAMP types.      |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data           formatter      result               | |
                |                                        +-------------------------------------------------------+ |
                |                                        |    19/01/16       'ZZZZZZ'       190116               | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
            Following tables provide information about different formatters on
            time type columns:
                +--------------------------------------------------------------------------------------------------+
                |    formatter                                    description                                      |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    HH                                  Represent the hour as two numeric digits.                 |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                      formatter      result    | |
                |                                        +-------------------------------------------------------+ |
                |                                        | 2020-07-01 08:00:00.000000   'HH'           08        | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    MI                                  Represent the minute as two numeric digits.               |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter      result   | |
                |                                        +-------------------------------------------------------+ |
                |                                        | 2020-07-01 13:20:53.64+03:00: 'HH:MI'        13:20    | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    SS                                  Represent the second as two numeric digits.               |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter   result      | |
                |                                        +-------------------------------------------------------+ |
                |                                        | 2020-07-01 13:20:53.64+03:00: 'HH.MI.SS'  13.20.53    | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    S(n)                                Number of fractional seconds.                             |
                |                                                                                                  |
                |    S(F)                                Replace n with a number between 0 and 6, or use F for the |
                |                                        number of characters needed to display the fractional     |
                |                                        seconds precision.                                        |
                |                                                                                                  |
                |                                        The value of F is resolved during the formatting of the   |
                |                                        TIME or TIMESTAMP data. The value is obtained from the    |
                |                                        fractional seconds precision in the declaration of the    |
                |                                        data type. For example, F is two for the TIME(2) type.    |
                |                                                                                                  |
                |                                        A value of zero for F displays no radix symbol and no     |
                |                                        fractional precision for the data.                        |
                |                                        The S(F) formatting characters must follow a D formatting |
                |                                        character or a . separator character.                     |
                |                                                                                                  |
                |                                        A value of n that is less than the PERIOD(TIME),          |
                |                                        PERIOD(TIMESTAMP), TIME or TIMESTAMP fractional second    |
                |                                        precision produces an error.                              |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter      result   | |
                |                                        +-------------------------------------------------------+ |
                |                                        |2020-07-01 13:20:53.64+03:00:'HH:MI:SSDS(F)'13:20:53.64| |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    D                                   Radix symbol.                                             |
                |                                        The value of RadixSeparator in the current SDF is copied  |
                |                                        to the output string whenever a D appears in the FORMAT   |
                |                                        phrase.                                                   |
                |                                                                                                  |
                |                                        Separator characters, such as . or :, can also appear in  |
                |                                        the "formatter", but only if they do not match the value  |
                |                                        of RadixSeparator.                                        |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter      result   | |
                |                                        +-------------------------------------------------------+ |
                |                                        |2020-07-01 13:20:53.64+03:00:'HH:MI:SSDS(F)'13:20:53.64| |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    T                                   Represent time in 12-hour format instead of 24-hour       |
                |                                        format. The appropriate time of day, as specified by AMPM |
                |                                        in the current SDF is copied to the output string         |
                |                                        where a T appears in the "formatter".                     |
                |                                                                                                  |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter      result   | |
                |                                        +-------------------------------------------------------+ |
                |                                        |2020-07-01 13:20:53.64+03:00:'HH:MI:SSBT'01:20:53 Nachm| |
                |                                        |                              (Nachm is German for PM.)| |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    Z                                   Time zone.                                                |
                |                                        The Z controls the placement of the time zone in the      |
                |                                        output of PERIOD(TIME), PERIOD(TIMESTAMP), TIME and       |
                |                                        TIMESTAMP data, and can only appear at the beginning or   |
                |                                        end of the time formatting characters.                    |
                |                                                                                                  |
                |                                        For example, the following statement uses a "formatter"   |
                |                                        that includes a Z before the time formatting characters:  |
                |                                                                                                  |
                |                                            SELECT CURRENT_TIMESTAMP                              |
                |                                            (FORMAT 'YYYY-MM-DDBZBHH:MI:SS.S(6)');                |
                |                                            If the PERIOD(TIME), PERIOD(TIMESTAMP), TIME or       |
                |                                            TIMESTAMP teradatasqlalchemy types contains time zone |
                |                                            data, the time zone is copied to the output string.   |
                |                                            The time zone format  is +HH:MI or -HH:MI, depending  |
                |                                            on the time zone hour displacement.                   |
                |                                        Examples:                                                 |
                |                                    +------------------------------------------------------------+|
                |                                    |    data                       formatter         result     ||
                |                                    +------------------------------------------------------------+|
                |                                    |2020-07-01 13:20:53.64+03:00:  'HH:MI:SSDS(F)Z'  13:20:53.64||
                |                                    |                                                      +03:00||
                |                                    +------------------------------------------------------------+|
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    :                                   Colon separator.                                          |
                |                                        Copied to output string where it appears in the FORMAT    |
                |                                        phrase. This is the default separator for ANSI time.      |
                |                                                                                                  |
                |                                        This character cannot appear in the "formatter" if the    |
                |                                        value of RadixSeparator in the current SDF is a colon.    |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter      result   | |
                |                                        +-------------------------------------------------------+ |
                |                                        | 2020-07-01 13:20:53.64+03:00: 'HH:MI'        13:20    | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    .                                   Period separator.                                         |
                |                                        This can also be used to indicate the fractional seconds. |
                |                                                                                                  |
                |                                        Copied to output string where it appears in the FORMAT    |
                |                                        phrase.                                                   |
                |                                                                                                  |
                |                                        This character cannot appear in the "formatter" if the    |
                |                                        value of RadixSeparator in the current SDF is a period.   |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter   result      | |
                |                                        +-------------------------------------------------------+ |
                |                                        | 2020-07-01 13:20:53.64+03:00: 'HH.MI.SS'  13.20.53    | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    -                                   Dash separator.                                           |
                |                                        Copied to output string where it appears in the FORMAT    |
                |                                        phrase.                                                   |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter      result   | |
                |                                        +-------------------------------------------------------+ |
                |                                        | 2020-07-01 13:20:53.64+03:00: 'HH-MI'        13-20    | |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    h                                   Hour separator.                                           |
                |                                        A lowercase h character is copied to the output string.   |
                |                                                                                                  |
                |                                        The h formatting character must follow the HH formatting  |
                |                                        characters.                                               |
                |                                                                                                  |
                |                                        This character cannot appear in the "formatter" if the    |
                |                                        value of RadixSeparator in the current SDF is a lowercase |
                |                                         h character.                                             |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter      result   | |
                |                                        +-------------------------------------------------------+ |
                |                                        | 2020-07-01 13:20:53.64+03:00: 'HHhMImSSs'    13h20m53s| |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    m                                   Minute separator.                                         |
                |                                        A lowercase m character is copied to the output string.   |
                |                                                                                                  |
                |                                        The m formatting character must follow the MI formatting  |
                |                                        characters.                                               |
                |                                                                                                  |
                |                                        This character cannot appear in the "formatter" if the    |
                |                                        value of RadixSeparator in the current SDF is a lowercase |
                |                                        m character.                                              |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter      result   | |
                |                                        +-------------------------------------------------------+ |
                |                                        | 2020-07-01 13:20:53.64+03:00: 'HHhMImSSs'    13h20m53s| |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    s                                   Second separator.                                         |
                |                                        A lowercase s character is copied to the output string.   |
                |                                                                                                  |
                |                                        The s formatting character must follow SS or SSDS(F)      |
                |                                        formatting characters.                                    |
                |                                                                                                  |
                |                                        This character cannot appear in the "formatter" if the    |
                |                                        value of RadixSeparator in the current SDF is a lowercase |
                |                                        s character.                                              |
                |                                        Examples:                                                 |
                |                                        +-------------------------------------------------------+ |
                |                                        |    data                       formatter      result   | |
                |                                        +-------------------------------------------------------+ |
                |                                        | 2020-07-01 13:20:53.64+03:00: 'HHhMImSSs'    13h20m53s| |
                |                                        +-------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |                                                                                                  |
                |    B                                  Blank representation separator.                            |
                |                                                                                                  |
                |    b                                  Use this instead of a space to represent a blank.          |
                |                                                                                                  |
                |                                       This character cannot appear in the "formatter" if the     |
                |                                       value of RadixSeparator in the current SDF is a blank.     |
                |                                        Examples:                                                 |
                |                                 +--------------------------------------------------------------+ |
                |                                 | data                           formatter           result    | |
                |                                 +--------------------------------------------------------------+ |
                |                                 | 2020-07-01 13:20:53.64+03:00:  'MM/DD/YYBHH:MIBT'  07/01/20  | |
                |                                 |                                                    01:20 PM  | |
                |                                 +--------------------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>> load_example_data("uaf", "stock_data")
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train=DataFrame("admissions_train")
        >>> admissions_train
            masters   gpa     stats programming  admitted
        id
        34     yes  3.85  Advanced    Beginner         0
        32     yes  3.46  Advanced    Beginner         0
        11      no  3.13  Advanced    Advanced         1
        30     yes  3.79  Advanced      Novice         0
        28      no  3.93  Advanced    Advanced         1
        16      no  3.70  Advanced    Advanced         1
        9       no  3.82  Advanced    Advanced         1
        13      no  4.00  Advanced      Novice         1
        15     yes  4.00  Advanced    Advanced         1
        17      no  3.83  Advanced    Advanced         1
        >>>
        # Example 1: Round the 'age' column upto 1 decimal values.
        >>> format_df = admissions_train.assign(format_column=admissions_train.gpa.format("zz.z"))
        >>> format_df
            masters   gpa     stats programming  admitted format_column
        id
        38     yes  2.65  Advanced    Beginner         1           2.6
        7      yes  2.33    Novice      Novice         1           2.3
        26     yes  3.57  Advanced    Advanced         1           3.6
        5       no  3.44    Novice      Novice         0           3.4
        3       no  3.70    Novice    Beginner         1           3.7
        22     yes  3.46    Novice    Beginner         0           3.5
        24      no  1.87  Advanced      Novice         1           1.9
        36      no  3.00  Advanced      Novice         0           3.0
        19     yes  1.98  Advanced    Advanced         0           2.0
        40     yes  3.95    Novice    Beginner         0           4.0
        >>>
        # Create a DataFrame on 'stock_data' table.
        >>> stock_data=DataFrame("stock_data")
        >>> stock_data
                     seq_no timevalue  magnitude
        data_set_id
        556               3  19/01/16     61.080
        556               5  19/01/30     63.810
        556               6  19/02/06     63.354
        556               7  19/02/13     63.871
        556               9  19/02/27     61.490
        556              10  19/03/06     61.524
        556               8  19/02/20     61.886
        556               4  19/01/23     63.900
        556               2  19/01/09     61.617
        556               1  19/01/02     60.900
        >>>
        # Example 2: change the format of 'timevalue' column.
        >>> format_df = stock_data.assign(format_column=stock_data.timevalue.format('MMMBDD,BYYYY'))
        >>> format_df
                    seq_no timevalue  magnitude format_column
        data_set_id
        556               3  19/01/16     61.080  Jan 16, 2019
        556               5  19/01/30     63.810  Jan 30, 2019
        556               6  19/02/06     63.354  Feb 06, 2019
        556               7  19/02/13     63.871  Feb 13, 2019
        556               9  19/02/27     61.490  Feb 27, 2019
        556              10  19/03/06     61.524  Mar 06, 2019
        556               8  19/02/20     61.886  Feb 20, 2019
        556               4  19/01/23     63.900  Jan 23, 2019
        556               2  19/01/09     61.617  Jan 09, 2019
        556               1  19/01/02     60.900  Jan 02, 2019
        >>>
    ilike
    teradataml.dataframe.sql.DataFrameColumn.ilike = ilike(self, other, escape_char=None)
    DESCRIPTION:
        Function which is used to match the pattern.
    PARAMETERS:
        other:
            Required Argument.
            Specifies a string to match. String match is case insensitive.
            Types: str OR ColumnExpression
        escape_char:
            Optional Argument.
            Specifies the escape character to be used in the pattern.
            Types: str with one character
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load example data.
        >>> load_example_data("teradataml", "pattern_matching_data")
        >>> df = DataFrame('pattern_matching_data')
                   data     pattern     level
        id                                   
        5       prod_01    prod_01%  Beginner
        8      log%2024        l_g%  Beginner
        2     user%2025     user!%%  Beginner
        6       prod%v2     prod!_%    Novice
        4   data%backup     data@%%  Advanced
        10     backup_9  restore!_9  Beginner
        7      log_file   log^_file  Advanced
        1    user_Alpha     user!_%  Advanced
        3     data_2024          d%    Novice
        9     temp_file    temp!__%    Novice
        # Example 1: Find out the records which starts with 'A' in the column 'level'.
        >>> df = df[df.level.ilike('A%')]
        >>> df
                   data    pattern     level
        id                                  
        4   data%backup    data@%%  Advanced
        7      log_file  log^_file  Advanced
        1    user_Alpha    user!_%  Advanced
        >>>
        # Example 2: Create a new Column with values as -
        #            1 if value of column 'level' starts with 'n' and third letter is 'v',
        #            0 otherwise. Ignore case.
        >>> from sqlalchemy.sql.expression import case as case_when
        >>> df.assign(new_col = case_when((df.level.ilike('n_v%').expression, 1), else_=0))
                   data     pattern     level new_col
        id                                           
        3     data_2024          d%    Novice       1
        1    user_Alpha     user!_%  Advanced       0
        8      log%2024        l_g%  Beginner       0
        2     user%2025     user!%%  Beginner       0
        10     backup_9  restore!_9  Beginner       0
        9     temp_file    temp!__%    Novice       1
        6       prod%v2     prod!_%    Novice       1
        5       prod_01    prod_01%  Beginner       0
        4   data%backup     data@%%  Advanced       0
        7      log_file   log^_file  Advanced       0
        >>>
        # Example 3: Find out the records where the value in the 'data' column 
        #            matches the pattern specified in the 'pattern' column.
        >>> df = df[df.data.ilike(df.pattern)]
        >>> df
                 data   pattern     level
        id                               
        3   data_2024        d%    Novice
        8    log%2024      l_g%  Beginner
        5     prod_01  prod_01%  Beginner
        >>>
        # Example 4: Find out the records where the value in the 'data' column
        #            matches the pattern specified in the 'pattern' column considering the
        #            escape character as '!'.
        >>> df = df[df.data.ilike(df.pattern, escape_char='!')]
        >>> df
                  data   pattern     level
        id                                
        8     log%2024      l_g%  Beginner
        9    temp_file  temp!__%    Novice
        3    data_2024        d%    Novice
        2    user%2025   user!%%  Beginner
        1   user_Alpha   user!_%  Advanced
        5      prod_01  prod_01%  Beginner
        >>>
    index
    teradataml.dataframe.sql.DataFrameColumn.index = index(expression)
    DESCRIPTION:
        Function returns the position in string values in column where string specified
        in the argument starts.
    NOTES:
        a. If string in argument is not found in string in column, then the result is zero.
        b. If string in argument is null, then the result is null.
        c. If the arguments are character types, index returns a logical character position,
           not a byte position, except when the server character set of the arguments is KANJI1
           and the session client character set is KanjiEBCDIC.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            which is used as a substring to be searched for its position within the
            full string in column.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types are:
                1. Character
                2. Byte - If value of column is of type BYTE, then "expression" must be of type BYTE.
                3. Numeric - If value of column is numeric, then it is converted implicitly to CHARACTER type.
            Types: ColumnExpression, str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns the start position of "ce" in "stats" column and pass it as input
        #            to DataFrame.assign().
        >>> res = df.assign(col = df.stats.index("ner"))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1    0
        4      yes  3.50  Beginner      Novice         1    6
        2      yes  3.76  Beginner    Beginner         0    6
        1      yes  3.95  Beginner    Beginner         0    6
        # Example 2: Executed index() function on "stats" column and filtered computed
        #            values which are equal to 0.
        >>> print(df[df.stats.index("ner") != 0])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    initcap
    teradataml.dataframe.sql.DataFrameColumn.initcap = initcap()
    DESCRIPTION:
        Function modifies a string value in column and returns the string with the first character
        in each word in uppercase and all other characters in lowercase. Words are delimited
        by white space or characters that are not alphanumeric.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Convert the first character to uppercase for strings in "masters" column and
        #            pass it as input to DataFrame.assign().
        >>> result = df.assign(col = df.masters.initcap())
        >>> print(result)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1   No
        4      yes  3.50  Beginner      Novice         1  Yes
        2      yes  3.76  Beginner    Beginner         0  Yes
        1      yes  3.95  Beginner    Beginner         0  Yes
        # Example 2: Executed initcap() function on "masters" column and filtered computed
        #            values which are equal to 'Yes'.
        >>> print(df[df.masters.initcap() == "Yes"])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    instr
    teradataml.dataframe.sql.DataFrameColumn.instr = instr(expression, position=1, occurrence=1)
    DESCRIPTION:
        Function searches the string values in column for occurrences of "expression".
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that the function searches for in source_string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, str
        position:
            Optional Argument.
            Specifies the position as an integer to begin searching at in the string values in column.
            If "position" is not specified, the search starts at the beginning of source_string.
            If "position" is negative, the function counts and searches backwards from the end of
            source_string.
            It cannot have a value of zero.
            Default Value: 1
            Types: int
        occurrence:
            Optional Argument.
            Specifies an integer which decides occurrence of search_string to find in source_string.
            If "occurrence" is not specified, the function searches for the first occurrence.
            If "occurrence" is greater than 1, the function searches for additional occurrences
            beginning with the second character in the previous occurrence.
            It cannot be zero or a negative value.
            Default Value: 1
            Types: int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Searches for a string 'e' in "programming" column values and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.programming.instr("e"))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  2.0
        4      yes  3.50  Beginner      Novice         1  6.0
        2      yes  3.76  Beginner    Beginner         0  2.0
        1      yes  3.95  Beginner    Beginner         0  2.0
        # Example 2: Executed instr() function on "programming" column and filtered computed
        #            values which are equal to 6.0.
        >>> print(df[df.programming.instr("e") == 6.0])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
    lcase
    teradataml.dataframe.sql.DataFrameColumn.lcase = lcase()
    DESCRIPTION:
        Function returns a character string identical to the string value in column,
        except that all uppercase letters are replaced with their lowercase equivalents.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Convert strings in "stats" column to lowercase and pass
        #            it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.lcase())
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1    novice
        4      yes  3.50  Beginner      Novice         1  beginner
        2      yes  3.76  Beginner    Beginner         0  beginner
        1      yes  3.95  Beginner    Beginner         0  beginner
        # Example 2: Executed lcase() function on "stats" column and filtered computed
        #            values which are equal to 'novice'.
        >>> print(df[df.stats.lcase() == "novice"])
           masters  gpa   stats programming  admitted
        id
        3       no  3.7  Novice    Beginner         1
    left
    teradataml.dataframe.sql.DataFrameColumn.left = left(length)
    DESCRIPTION:
        Function truncates string value in column to a specified number of characters desired from
        the left side of the string.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    PARAMETERS:
        length:
            Required Argument.
            Specifies a positive integer specifying the number of characters desired from
            the left side of the string. If the number of character exceeds the number of
            characters in the original string, the original string is returned.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Truncates values in "programming" column to a length of 3 and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.programming.left(3))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  Beg
        4      yes  3.50  Beginner      Novice         1  Nov
        2      yes  3.76  Beginner    Beginner         0  Beg
        1      yes  3.95  Beginner    Beginner         0  Beg
        # Example 2: Executed left() function on "programming" column and filtered computed
        #            values which are equal to 'Nov'.
        >>> print(df[df.programming.left(3) == "Nov"])
           masters  gpa     stats programming  admitted  col
        id
        4      yes  3.5  Beginner      Novice         1  Nov
    length
    teradataml.dataframe.sql.DataFrameColumn.length = length()
    DESCRIPTION:
        Function returns the number of characters in the values of column.
    ALTERNATE_NAMES:
        character_length
        char_length
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns length of character string in "programming" column and pass
        #            it as input to DataFrame.assign().
        >>> result = df.assign(col = df.programming.length())
        >>> print(result)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  8.0
        4      yes  3.50  Beginner      Novice         1  6.0
        2      yes  3.76  Beginner    Beginner         0  8.0
        1      yes  3.95  Beginner    Beginner         0  8.0
        # Example 2: Executed length() function on "programming" column and filtered computed
        #            values which are equal to 6.0.
        >>> print(df[df.programming.length() == 6.0])
           masters  gpa     stats programming  admitted
        id
        4      yes  3.5  Beginner      Novice         1
    levenshtein
    teradataml.dataframe.sql.DataFrameColumn.levenshtein = levenshtein(expression, ci=1, cd=1, cs=1, ct=1)
    DESCRIPTION:
        Function returns the minimum number of edit operations (insertions, deletions,
        substitutions and transpositions) required to transform string1 (values in the column)
        into "expression" (value passed in argument).
        Edit distance measures the similarity between two strings. A low number of deletions,
        insertions, substitutions or transpositions implies a high similarity. The insertions,
        deletions, substitutions, and transpositions are based on the Damerau-Levenshtein
        Distance algorithm with modifications for costed operations.
    ALTERNATE_NAME:
        edit_distance
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Support column types are: CHARACTER, VARCHAR, or CLOB.
            Types: ColumnExpression, str
        ci:
            Optional Argument.
            Specifies the relative cost of an insert operation.
            The value specified must be a non-negative integer.
            Default Value: 1
            Types: int
        cd:
            Optional Argument.
            Specifies the relative cost of a delete operation.
            The value specified must be a non-negative integer.
            Default Value: 1
            Types: int
        cs:
            Optional Argument.
            Specifies the relative cost of a substitute operation.
            The value specified must be a non-negative integer.
            Default Value: 1
            Types: int
        ct:
            Optional Argument.
            Specifies the relative cost of a transpose operation.
            The value specified must be a non-negative integer.
            Default Value: 1
            Types: int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to execute the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Calculate the edit distance between values in "stats" and "programming"
        #            columns and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.levenshtein(df.programming))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1    6
        4      yes  3.50  Beginner      Novice         1    6
        2      yes  3.76  Beginner    Beginner         0    0
        1      yes  3.95  Beginner    Beginner         0    0
        # Example 2: Calculate the edit distance between values in "stats" and "programming"
        #            columns with cost associated with the edit operations passed and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.levenshtein(df.programming, 2, 1, 1, 2))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1    8
        4      yes  3.50  Beginner      Novice         1    6
        2      yes  3.76  Beginner    Beginner         0    0
        1      yes  3.95  Beginner    Beginner         0    0
        # Example 3: Executed levenshtein() function on "stats" column and filtered computed
        #            values which are equal to 8.
        >>> print(df[df.stats.levenshtein(df.programming, 2, 1, 1, 2) == 8])
           masters  gpa   stats programming  admitted
        id
        3       no  3.7  Novice    Beginner         1
    like
    teradataml.dataframe.sql.DataFrameColumn.like = like(self, other, escape_char=None)
    DESCRIPTION:
        Function which is used to match the pattern.
    PARAMETERS:
        other:
            Required Argument.
            Specifies a string to match. String match is case sensitive.
            Types: str OR ColumnExpression
        escape_char:
            Optional Argument.
            Specifies the escape character to be used in the pattern.
            Types: str with one character
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load example data.
        >>> load_example_data("teradataml", "pattern_matching_data")
        >>> df = DataFrame('pattern_matching_data')
                   data     pattern     level
        id                                   
        5       prod_01    prod_01%  Beginner
        8      log%2024        l_g%  Beginner
        2     user%2025     user!%%  Beginner
        6       prod%v2     prod!_%    Novice
        4   data%backup     data@%%  Advanced
        10     backup_9  restore!_9  Beginner
        7      log_file   log^_file  Advanced
        1    user_Alpha     user!_%  Advanced
        3     data_2024          d%    Novice
        9     temp_file    temp!__%    Novice
        # Example 1: Find out the records which starts with 'A' in the column 'level'.
        >>> df = df[df.level.like('A%')]
        >>> df
                   data    pattern     level
        id                                  
        4   data%backup    data@%%  Advanced
        7      log_file  log^_file  Advanced
        1    user_Alpha    user!_%  Advanced
        >>>
        # Example 2: Create a new Column with values as -
        #            1 if value of column 'stats' starts with 'N' and third letter is 'v',
        #            0 otherwise. Do not ignore case.
        >>> from sqlalchemy.sql.expression import case as case_when
        >>> df.assign(new_col = case_when((df.level.like('N_v%').expression, 1), else_=0))
                   data     pattern     level new_col
        id                                           
        3     data_2024          d%    Novice       1
        1    user_Alpha     user!_%  Advanced       0
        8      log%2024        l_g%  Beginner       0
        2     user%2025     user!%%  Beginner       0
        10     backup_9  restore!_9  Beginner       0
        9     temp_file    temp!__%    Novice       1
        6       prod%v2     prod!_%    Novice       1
        5       prod_01    prod_01%  Beginner       0
        4   data%backup     data@%%  Advanced       0
        7      log_file   log^_file  Advanced       0
        >>>
        # Example 3: Find out the records where the value in the 'data' column 
        #            matches the pattern specified in the 'pattern' column.
        >>> df = df[df.data.like(df.pattern)]
        >>> df
                 data   pattern     level
        id                               
        3   data_2024        d%    Novice
        8    log%2024      l_g%  Beginner
        5     prod_01  prod_01%  Beginner
        >>>
        # Example 4: Find out the records where the value in the 'data' column
        #            matches the pattern specified in the 'pattern' column considering the
        #            escape character as '!'.
        >>> df = df[df.data.like(df.pattern, escape_char='!')]
        >>> df
                  data   pattern     level
        id                                
        8     log%2024      l_g%  Beginner
        9    temp_file  temp!__%    Novice
        3    data_2024        d%    Novice
        2    user%2025   user!%%  Beginner
        1   user_Alpha   user!_%  Advanced
        5      prod_01  prod_01%  Beginner
        >>>
    locate
    teradataml.dataframe.sql.DataFrameColumn.locate = locate(expression, n1=1)
    DESCRIPTION:
        Function returns the position of the first occurrence of string values in column within
        value specified in argument.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal for
            the full string to be searched in values in column.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Support types of arguments:
                1. Character, except for CLOB
                2. Byte, except for BLOB
                    If value of column is of type BYTE, then "expression" must be of type BYTE.
                3. Numeric - This is converted implicitly to CHARACTER type.
            Types: ColumnExpression, str
        n1:
            Optional Argument.
            Specifies where the search begins with the character position indicated by the
            value of n1. Accepts a positive integer value.
            Default Value: 1
            Types: int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns the position of string in "stats" column in a string
        #            'novicebeginner' and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.locate("novicebeginner"))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1    1
        4      yes  3.50  Beginner      Novice         1    7
        2      yes  3.76  Beginner    Beginner         0    7
        1      yes  3.95  Beginner    Beginner         0    7
        # Example 2: Executed locate() function on "stats" column and filtered computed
        #            values which are equal to 1.
        >>> print(df[df.stats.locate("novicebeginner") == 1])
           masters  gpa   stats programming  admitted
        id
        3       no  3.7  Novice    Beginner         1
    lower
    teradataml.dataframe.sql.DataFrameColumn.lower = lower()
    DESCRIPTION:
        Function returns a character string identical to the string value in column,
        except that all uppercase letters are replaced with their lowercase equivalents.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Convert strings in "stats" column to lowercase and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.lower())
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1    novice
        4      yes  3.50  Beginner      Novice         1  beginner
        2      yes  3.76  Beginner    Beginner         0  beginner
        1      yes  3.95  Beginner    Beginner         0  beginner
        # Example 2: Executed lower() function on "stats" column and filtered computed
        #            values which are equal to 'novice'.
        >>> print(df[df.stats.lower() == "novice"])
           masters  gpa   stats programming  admitted
        id
        3       no  3.7  Novice    Beginner         1
    lpad
    teradataml.dataframe.sql.DataFrameColumn.lpad = lpad(length, ﬁll_string=' ')
    DESCRIPTION:
        Function returns the string value in column padded to the left with the characters
        in "fill_string" so that the resulting string has "length" characters.
    PARAMETERS:
        length:
            Required Argument.
            Specifies a ColumnExpression of an int column or an integer literal specifying
            the number of characters in the resulting string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: INTEGER, BIGINT, or NUMBER
            Types: ColumnExpression, int
        fill_string:
            Optional Argument.
            Specifies a ColumnExpression of a string column or a string literal
            used to pad the source_string.
            The sequence of characters in fill_string is replicated as necessary.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR, or CLOB
            Default Value: ' '
            Types: ColumnExpression, str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Pad string in "stats" column with 0 and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.lpad(10, "0"))
        >>> print(res)
           masters   gpa     stats programming  admitted         col
        id
        3       no  3.70    Novice    Beginner         1  0000Novice
        4      yes  3.50  Beginner      Novice         1  00Beginner
        2      yes  3.76  Beginner    Beginner         0  00Beginner
        1      yes  3.95  Beginner    Beginner         0  00Beginner
        # Example 2: Pad string in "stats" column with strings from "masters" column and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.lpad(20, df.masters))
        >>> print(res)
           masters   gpa     stats programming  admitted                   col
        id
        3       no  3.70    Novice    Beginner         1  nononononononoNovice
        4      yes  3.50  Beginner      Novice         1  yesyesyesyesBeginner
        2      yes  3.76  Beginner    Beginner         0  yesyesyesyesBeginner
        1      yes  3.95  Beginner    Beginner         0  yesyesyesyesBeginner
    ltrim
    teradataml.dataframe.sql.DataFrameColumn.ltrim = ltrim(expression=' ')
    DESCRIPTION:
        Function returns the string values in column, with its left-most characters removed up
        to the first character that is not in the string value in argument.
    PARAMETERS:
        expression:
            Optional Argument.
            Specifies a ColumnExpression of a string column or a string literal that
            will be removed from string values in column. If "expression" is specified, it must
            be the same data type as string values in column.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Default Value: ' '
    NOTES:
        Column expression of both arguments can be of following type:
            a. Character/String types: CHAR, VARCHAR, or CLOB
            b. Integer types: BYTEINT, SMALLINT, INTEGER, or BIGINT
            c. Numeric types: FLOAT, REAL, DOUBLE PRECISION, DECIMAL, NUMERIC, or NUMBER
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Left trim string in "stats" column if it has 'Begi' and pass it as
        #            input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.ltrim("Begi"))
        >>> print(res)
           masters   gpa     stats programming  admitted     col
        id
        3       no  3.70    Novice    Beginner         1  Novice
        4      yes  3.50  Beginner      Novice         1    nner
        2      yes  3.76  Beginner    Beginner         0    nner
        1      yes  3.95  Beginner    Beginner         0    nner
        # Example 2: Executed ltrim() function on "stats" column and filtered computed
        #            values which are equal to 'nner'.
        >>> print(df[df.stats.ltrim("Begi") == "nner"])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    ngram
    teradataml.dataframe.sql.DataFrameColumn.ngram = ngram(expression, length, position=1)
    DESCRIPTION:
        Function returns the number of n-gram matches between values in column
        and "expression". A high number of matching n-gram patterns implies
        a high similarity between the two strings.
        For positional n-gram matching, the position as well as the pattern must match
        when measuring similarity. The position value indicates how far away positionally
        the match may be between the 2 strings as follows:
            * If position is set to a value of zero, the match must be at the same position
              in the 2 strings.
            * If position is set to a value of x , the match must be within x positions in
              the 2 strings.
              For example, if position = 2, then the match must be within 2 positions in the
              2 strings.
        The function returns zero in the following cases:
            * If the length argument is greater than the length of either values in column
              or expression.
            * If the length argument is less than or equal to 0 or if either values in column or
              expression is an empty string.
        Patterns beyond the length of 255 are ignored.
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            If the argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR, or CLOB
            Types: ColumnExpression, str
        length:
            Required Argument.
            Specifies an integer value ninn-gram, which is the comparison length.
            Types: int
        position:
            Optional Argument.
            Specifies an integer value that the n-gram is a positional n-gram match.
            Default Value: 1
            Types: int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns n-gram value for character string in "stats" and "programming" column and
        #            pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.ngram(df.programming, 3))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1    0
        4      yes  3.50  Beginner      Novice         1    0
        2      yes  3.76  Beginner    Beginner         0    6
        1      yes  3.95  Beginner    Beginner         0    6
        # Example 2: Executed ngram() function on "stats" column and filtered computed
        #            values which are equal to 6.
        >>> print(df[df.stats.ngram(df.programming, 3) == 6])
           masters   gpa     stats programming  admitted
        id
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    nvp
    teradataml.dataframe.sql.DataFrameColumn.nvp = nvp(name_to_search, name_delimiters='&', value_delimiters='=', occurrence=1)
    DESCRIPTION:
        Function extracts the value of a name-value pair where the name in the pair matches
        the name and the number of the "occurrence" specified.
    PARAMETERS:
        name_to_search:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            whose instring value NVP returns.
            Types: ColumnExpression, str
        name_delimiters:
            Optional Argument.
            Specifies the multi-byte delimiters used to separate name-value pairs.
            Delimiters can contain any characters. They are separated from each
            other in the string by spaces. If a space is used as part of a delimiter,
            it must be escaped using a backslash (\). The maximum length of any
            delimiter is 10, and the maximum size of this parameter is 32.
            Default value: '&' (ampersand).
            Types: str
        value_delimiters:
            Optional Argument.
            Specifies the multi-byte delimiters used to associate a name to its value in a
            name-value pair.
            Delimiters can contain any characters. They are separated from each other in the
            string by spaces. If a space is used as part of a delimiter, it must be escaped
            using a backslash (\). The maximum length of any delimiter is 10, and the
            maximum size of this parameter is 32.
            Default value: '=' (equal sign).
            Types: str
        occurrence:
            Optional Argument.
            Specifies the number of occurrences of name_to_search that NVP searches for.
            Default value: 1.
            Types: int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "employee_info")
        # Create a DataFrame on 'employee_info' table.
        >>> df = DataFrame("employee_info")
        >>> print(df)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        # Create a column 'nvp_col' for specifying string literal 'entree:orange chicken#entree2:honey salmon'
        >>> df = df.assign(nvp_col = 'entree:orange chicken#entree2:honey salmon')
        >>> print(df)
                    first_name marks   dob joined_date    nvp_literal_                                     nvp_col
        employee_no
        112               None  None  None    18/12/05  orange chicken  entree:orange chicken#entree2:honey salmon
        100               abcd  None  None        None  orange chicken  entree:orange chicken#entree2:honey salmon
        101              abcde  None  None    02/12/05  orange chicken  entree:orange chicken#entree2:honey salmon
        # Example 1: Retrieve nvp value for "nvp_col" and pass it as input to DataFrame.assign().
        >>> res = df.assign(col= df.nvp_col.nvp('entree','#', ':', 1))
        >>> print(res)
                    first_name marks   dob joined_date                                     nvp_col             col
        employee_no
        112               None  None  None    18/12/05  entree:orange chicken#entree2:honey salmon  orange chicken
        100               abcd  None  None        None  entree:orange chicken#entree2:honey salmon  orange chicken
        101              abcde  None  None    02/12/05  entree:orange chicken#entree2:honey salmon  orange chicken
        # Example 2: Executed nvp() function on "nvp_col" column and filtered computed
        #            values which are equal to 'orange chicken'.
        >>> print(df[df.nvp_col.nvp('entree','#', ':', 1) == "orange chicken"])
                    first_name marks   dob joined_date                                     nvp_col
        employee_no
        100               abcd  None  None        None  entree:orange chicken#entree2:honey salmon
        101              abcde  None  None    02/12/05  entree:orange chicken#entree2:honey salmon
        112               None  None  None    18/12/05  entree:orange chicken#entree2:honey salmon
    oreplace
    teradataml.dataframe.sql.DataFrameColumn.oreplace = oreplace(search_string, replace_string)
    DESCRIPTION:
        Function replaces every occurrence of "search_string" in the string value in column
        with the "replace_string". Use this function either to replace or remove
        portions of a string.
    ALTERNATE NAME:
        replace
    PARAMETERS:
        search_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that the function searches for string values in column.
            If argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR, or CLOB
            Types: ColumnExpression, str
        replace_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that replaces the characters specified by "search_string".
            If argument is NULL or is an empty string, or is omitted, all
            occurrences of "search_string" are removed from the string values in column.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR, or CLOB
            Types: ColumnExpression, str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Removes occurrence of 'ner' in "stats" column and pass it as
        #            input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.oreplace("ner"))
        >>> print(res)
           masters   gpa     stats programming  admitted     col
        id
        3       no  3.70    Novice    Beginner         1  Novice
        4      yes  3.50  Beginner      Novice         1  Begin
        2      yes  3.76  Beginner    Beginner         0  Begin
        1      yes  3.95  Beginner    Beginner         0  Begin
        # Example 2: Executed oreplace() function on "stats" column and filtered computed
        #            values which are equal to 'Begin'.
        >>> print(df[df.stats.oreplace("ner") == "Begin"])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    otranslate
    teradataml.dataframe.sql.DataFrameColumn.otranslate = otranslate(expression, replace_string)
    DESCRIPTION:
        Function returns values in column with every occurrence of each character in
        "expression" replaced with the corresponding character in "replace_string".
        If the first character in "expression" occurs in the values in column, all
        occurrences of it are replaced by the first character in "replace_string". This
        repeats for all characters in "expression" and for all characters in
        "replace_string". The replacement is performed character-by-character, that is,
        the replacement of the second character is done on the string resulting
        from the replacement of the first character.
        If "expression" contains more characters than "replace_string", the extra characters
        are removed from the string values in column.
        If "expression" contains fewer characters than "replace_string", the extra characters
        in "replace_string" have no effect.
        If the same character occurs more than once in "expression", only the replacement
        character from the "replace_string" corresponding to the first occurrence is used.
    ALTERNATE NAME:
        translate
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that will be replaced in string values in column.
            If argument is NULL, the function returns string values in column.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR
            Types: ColumnExpression, str
        replace_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that replaces the characters specified by expression.
            If argument is NULL or empty, the function removes the characters
            specified in expression.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR
            Types: ColumnExpression, str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Replaces the occurrence of 'i' in the string in "stats" column by the
        # character in to_string ('X') and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.otranslate("i", "X"))
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1    NovXce
        4      yes  3.50  Beginner      Novice         1  BegXnner
        2      yes  3.76  Beginner    Beginner         0  BegXnner
        1      yes  3.95  Beginner    Beginner         0  BegXnner
        # Example 2: Executed otranslate() function on "stats" column and filtered computed
        # values which are equal to 'BegXnner'.
        >>> print(df[df.stats.otranslate("i", "X") == "BegXnner"])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    parse_url
    teradataml.dataframe.sql.DataFrameColumn.parse_url = parse_url(self, url_part, key=None)
    DESCRIPTION:
        Extracts a specific part from the URL.
    PARAMETERS:
        url_part:
            Required Argument.
            Specifies which part to be extracted.
            Permitted Values: HOST, PATH, QUERY, REF, PROTOCOL, FILE, AUTHORITY, USERINFO
            Type: str or ColumnExpression
        key:
            Optional Argument.
            Specifies the key to be used for extracting the value from the query string.
            Note:
                * Applicable only when url_part is set to 'QUERY'.
            Type: str or ColumnExpression
    Returns:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml", "url_data")
        # Create a DataFrame on 'url_data' table.
        >>> df = DataFrame("url_data")
        >>> df
                                                                    urls       part     query_key
        id                                                                                       
        3                                       https://www.facebook.com       HOST  facebook.com
        8             http://example.com/api?query1=value1&query2=value2      QUERY        query1
        6              smtp://user:password@smtp.example.com:21/file.txt   USERINFO      password
        4   https://teracloud-pod-services-pod-account-service.dummyvalu      QUERY          None
        0                                   http://example.com:8080/path       FILE          path
        2   https://example.net/path4/path5/path6?query4=value4#fragment        REF     fragment3
        1                                      ftp://example.net:21/path       PATH          path
        5                        http://pg.example.ml/path150#fragment90  AUTHORITY    fragment90
        7                                         https://www.google.com   PROTOCOL    google.com
        # Example 1: Extract components from column 'urls' using column 'part'.
        >>> df.assign(col = df.urls.parse_url(df.part))
                                                                    urls       part     query_key                          col
        id                                                                                                                    
        3                                       https://www.facebook.com       HOST  facebook.com             www.facebook.com
        8             http://example.com/api?query1=value1&query2=value2      QUERY        query1  query1=value1&query2=value2
        6              smtp://user:password@smtp.example.com:21/file.txt   USERINFO      password                user:password
        4   https://teracloud-pod-services-pod-account-service.dummyvalu      QUERY          None                         None
        0                                   http://example.com:8080/path       FILE          path                        /path
        2   https://example.net/path4/path5/path6?query4=value4#fragment        REF     fragment3                     fragment
        1                                      ftp://example.net:21/path       PATH          path                        /path
        5                        http://pg.example.ml/path150#fragment90  AUTHORITY    fragment90                pg.example.ml
        7                                         https://www.google.com   PROTOCOL    google.com                        https
        # Example 2: Extract components from column 'urls' using 'part' and
        #            'query_key' column.
        >>> df.assign(col = df.urls.parse_url(df.part, df.query_key))
                                                                    urls       part     query_key     col
        id                                                                                               
        3                                       https://www.facebook.com       HOST  facebook.com    None
        8             http://example.com/api?query1=value1&query2=value2      QUERY        query1  value1
        6              smtp://user:password@smtp.example.com:21/file.txt   USERINFO      password    None
        4   https://teracloud-pod-services-pod-account-service.dummyvalu      QUERY          None    None
        0                                   http://example.com:8080/path       FILE          path    None
        2   https://example.net/path4/path5/path6?query4=value4#fragment        REF     fragment3    None
        1                                      ftp://example.net:21/path       PATH          path    None
        5                        http://pg.example.ml/path150#fragment90  AUTHORITY    fragment90    None
        7                                         https://www.google.com   PROTOCOL    google.com    None
        # Extract components from column 'urls' using 'part' and 'query_key' str.
        >>> df.assign(col = df.urls.parse_url('QUERY', 'query2'))
                                                                    urls       part     query_key     col
        id                                                                                               
        3                                       https://www.facebook.com       HOST  facebook.com    None
        8             http://example.com/api?query1=value1&query2=value2      QUERY        query1  value2
        6              smtp://user:password@smtp.example.com:21/file.txt   USERINFO      password    None
        4   https://teracloud-pod-services-pod-account-service.dummyvalu      QUERY          None    None
        0                                   http://example.com:8080/path       FILE          path    None
        2   https://example.net/path4/path5/path6?query4=value4#fragment        REF     fragment3    None
        1                                      ftp://example.net:21/path       PATH          path    None
        5                        http://pg.example.ml/path150#fragment90  AUTHORITY    fragment90    None
        7                                         https://www.google.com   PROTOCOL    google.com    None
    replace
    teradataml.dataframe.sql.DataFrameColumn.replace = replace(search_string, replace_string)
    DESCRIPTION:
        Function replaces every occurrence of "search_string" in the string value in column
        with the "replace_string". Use this function either to replace or remove
        portions of a string.
    ALTERNATE NAME:
        oreplace
    PARAMETERS:
        search_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that the function searches for string values in column.
            If argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR, or CLOB
            Types: ColumnExpression, str
        replace_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that replaces the characters specified by "search_string".
            If argument is NULL or is an empty string, or is omitted, all
            occurrences of "search_string" are removed from the string values in column.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR, or CLOB
            Types: ColumnExpression, str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Removes occurrence of 'ner' in "stats" column and pass it as input
        #            to DataFrame.assign().
        >>> res = df.assign(col = df.stats.replace("ner"))
        >>> print(res)
           masters   gpa     stats programming  admitted     col
        id
        3       no  3.70    Novice    Beginner         1  Novice
        4      yes  3.50  Beginner      Novice         1  Begin
        2      yes  3.76  Beginner    Beginner         0  Begin
        1      yes  3.95  Beginner    Beginner         0  Begin
        # Example 2: Executed replace() function on "stats" column and filtered computed
        #            values which are equal to 'Begin'.
        >>> print(df[df.stats.replace("ner") == "Begin"])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    reverse
    teradataml.dataframe.sql.DataFrameColumn.reverse = reverse()
    DESCRIPTION:
        Function reverses the string value in column.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Reverses strings in "stats" column and pass it as input to
        #            DataFrame.assign().
        >>> res = df.assign(col= df.stats.reverse())
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1    ecivoN
        4      yes  3.50  Beginner      Novice         1  rennigeB
        2      yes  3.76  Beginner    Beginner         0  rennigeB
        1      yes  3.95  Beginner    Beginner         0  rennigeB
        # Example 2: Executed reverse() function on "stats" column and filtered computed
        #            values which are equal to 'rennigeB'.
        >>> print(df[df.stats.reverse() == "rennigeB"])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    right
    teradataml.dataframe.sql.DataFrameColumn.right = right(length)
    DESCRIPTION:
        Function truncates string value in column to a specified number of characters desired from
        the right side of the string.
    PARAMETERS:
        length:
            Required Argument.
            Specifies a positive integer specifying the number of characters desired from
            the left side of the string. If the number of character exceeds the number of
            characters in the original string, the original string is returned.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Truncates values in "programming" column to a length of 3 and pass it
        #            as input to DataFrame.assign().
        >>> res = df.assign(col = df.programming.right(3))
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1  ner
        4      yes  3.50  Beginner      Novice         1  ice
        2      yes  3.76  Beginner    Beginner         0  ner
        1      yes  3.95  Beginner    Beginner         0  ner
        # Example 2: Executed right() function on "programming" column and filtered computed
        #            values which are equal to 'ner'.
        >>> print(df[df.programming.right(3) == "ner"])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    rlike
    teradataml.dataframe.sql.DataFrameColumn.rlike = rlike(self, pattern, case_sensitive=True)
    DESCRIPTION:
        Function to match a string against a regular expression pattern.
    PARAMETERS:
        pattern:
            Required Argument.
            Specifies a regular expression pattern to match against the column values.
            Note:
                The pattern follows POSIX regular expression syntax.
            Type: str OR ColumnExpression
        case_sensitive:
            Optional Argument.
            Specifies whether the pattern matching is case-sensitive.
            When set to False, the function ignores case sensitivity and matches 
            the regex. Otherwise, function considers case sensitivity while matching regex.
            Default: True
            Type: bool
    RAISES:
        TeradataMlException
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
            masters    gpa      stats programming  admitted
        id
        13      no     4.00  Advanced      Novice         1
        26     yes     3.57  Advanced    Advanced         1
        5       no     3.44    Novice      Novice         0
        19     yes     1.98  Advanced    Advanced         0
        15     yes     4.00  Advanced    Advanced         1
        40     yes     3.95    Novice    Beginner         0
        7      yes     2.33    Novice      Novice         1
        22     yes     3.46    Novice    Beginner         0
        36      no     3.00  Advanced      Novice         0
        38     yes     2.65  Advanced    Beginner         1
        # Example 1: Find records whose 'stats' column contains 'van'.
        >>> result = df[df.stats.rlike('.*van.*')]
        >>> result
            masters     gpa     stats  programming  admitted
        id
        13      no     4.00  Advanced      Novice         1
        26     yes     3.57  Advanced    Advanced         1
        34     yes     3.85  Advanced    Beginner         0
        19     yes     1.98  Advanced    Advanced         0
        15     yes     4.00  Advanced    Advanced         1
        36      no     3.00  Advanced      Novice         0
        38     yes     2.65  Advanced    Beginner         1
        # Example 2: Find records whose 'stats' column ends with 'ced'.
        >>> result = df[df.stats.rlike('.*ced$')]
        >>> result
           masters   gpa     stats programming  admitted
        id                                              
        34     yes  3.85  Advanced    Beginner         0
        32     yes  3.46  Advanced    Beginner         0
        11      no  3.13  Advanced    Advanced         1
        30     yes  3.79  Advanced      Novice         0
        28      no  3.93  Advanced    Advanced         1
        16      no  3.70  Advanced    Advanced         1
        14     yes  3.45  Advanced    Advanced         0
        # Example 3: Case-insensitive search for records containing 'NOVICE'.
        >>> result = df[df.stats.rlike('NOVICE', case_sensitive=False)]
        >>> result
           masters   gpa   stats programming  admitted
        id                                            
        12      no  3.65  Novice      Novice         1
        40     yes  3.95  Novice    Beginner         0
        7      yes  2.33  Novice      Novice         1
        5       no  3.44  Novice      Novice         0
        22     yes  3.46  Novice    Beginner         0
        37      no  3.52  Novice      Novice         1
    rpad
    teradataml.dataframe.sql.DataFrameColumn.rpad = rpad(length, ﬁll_string=' ')
    DESCRIPTION:
        Function returns the string value in column padded to the right with the characters
        in "fill_string" so that the resulting string has "length" characters.
    PARAMETERS:
        length:
            Required Argument.
            Specifies a ColumnExpression of an int column or an integer literal specifying
            the number of characters in the resulting string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: INTEGER, BIGINT, or NUMBER
            Types: ColumnExpression, int
        fill_string:
            Optional Argument.
            Specifies a ColumnExpression of a string column or a string literal
            used to pad the source_string.
            The sequence of characters in fill_string is replicated as necessary.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR, or CLOB
            Default Value: ' '
            Types: ColumnExpression, str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Pad string in "stats" column with 0 and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.rpad(10, "0"))
        >>> print(res)
           masters   gpa     stats programming  admitted         col
        id
        3       no  3.70    Novice    Beginner         1  Novice0000
        4      yes  3.50  Beginner      Novice         1  Beginner00
        2      yes  3.76  Beginner    Beginner         0  Beginner00
        1      yes  3.95  Beginner    Beginner         0  Beginner00
        # Example 2: Pad string in "stats" column with strings from "masters" column and pass it
        #            as input to DataFrame.assign().
        >>> df = df.assign(lpad_gpa_ = df.stats.rpad(20, df.masters))
        >>> print(df)
           masters   gpa     stats programming  admitted                   col
        id
        3       no  3.70    Novice    Beginner         1  Novicenonononononono
        4      yes  3.50  Beginner      Novice         1  Beginneryesyesyesyes
        2      yes  3.76  Beginner    Beginner         0  Beginneryesyesyesyes
        1      yes  3.95  Beginner    Beginner         0  Beginneryesyesyesyes
    rtrim
    teradataml.dataframe.sql.DataFrameColumn.rtrim = rtrim(expression=' ')
    DESCRIPTION:
        Function returns the string values in column, with its right-most characters removed up
        to the first character that is not in the string value in argument.
    PARAMETERS:
        expression:
            Optional Argument.
            Specifies a ColumnExpression of a string column or a string literal that
            will be removed from string values in column. If expression is specified, it must
            be the same data type as string values in column.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Default Value: ' '
            Types: ColumnExpression, str
    NOTES:
        Column expression of both arguments can be of following type:
            a. Character/String types: CHAR, VARCHAR, or CLOB
            b. Integer types: BYTEINT, SMALLINT, INTEGER, or BIGINT
            c. Numeric types: FLOAT, REAL, DOUBLE PRECISION, DECIMAL, NUMERIC, or NUMBER
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Right trim string in "stats" column if it has 'Begi' and pass it as
        #            input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.rtrim("ner"))
        >>> print(res)
           masters   gpa     stats programming  admitted    col
        id
        3       no  3.70    Novice    Beginner         1  Novic
        4      yes  3.50  Beginner      Novice         1   Begi
        2      yes  3.76  Beginner    Beginner         0   Begi
        1      yes  3.95  Beginner    Beginner         0   Begi
        # Example 2: Executed rtrim() function on "stats" column and filtered computed
        #            values which are equal to 'Begi'.
        >>> print(df[df.stats.rtrim("ner") == "Begi"])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    soundex
    teradataml.dataframe.sql.DataFrameColumn.soundex = soundex()
    DESCRIPTION:
        Function returns a character string that represents the Soundex code for
        string_expression.
        Soundex is a system that codes surnames having the same or similar sounds,
        but variant spellings. The Soundex system was first used by the National
        Archives in 1880 to index the United States census.
        Soundex codes begin with the first letter of the surname followed by a three-digit
        code. Zeros are added to names that do not have enough letters.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns the Soundex code for character string in "programming" column and pass
        #            the Function object as input to DataFrame.assign().
        >>> res = df.assign(col = df.programming.soundex())
        >>> print(res)
           masters   gpa     stats programming  admitted   col
        id
        3       no  3.70    Novice    Beginner         1  B256
        4      yes  3.50  Beginner      Novice         1  N120
        2      yes  3.76  Beginner    Beginner         0  B256
        1      yes  3.95  Beginner    Beginner         0  B256
        # Example 2: Executed soundex() function on "programming" column and filtered computed
        #            values which are equal to 'B256'.
        >>> print(df[df.programming.soundex() == "B256"])
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    startswith
    teradataml.dataframe.sql.DataFrameColumn.startswith = startswith(self, other)
    DESCRIPTION:
        Function to check whether the column value starts with the specified value or not.
    PARAMETERS:
        other:
            Required Argument.
            Specifies a string literal or ColumnExpression to match.
            Types: str OR ColumnExpression
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        >>> load_example_data("ntree", "employee_table")
        >>> df = DataFrame("employee_table")
        >>> df
               emp_name  mgr_id mgr_name
        emp_id
        200         Pat   100.0      Don
        300       Donna   100.0      Don
        400         Kim   200.0      Pat
        500        Fred   400.0      Kim
        100         Don     NaN       NA
        # Example 1: Find out the employees whose name starts with their managers name.
        >>> df[df.emp_name.startswith(df.mgr_name)]
               emp_name  mgr_id mgr_name
        emp_id
        300       Donna     100      Don
        # Example 2: Find out the employees whose manager name starts with Don.
        >>> df[df.mgr_name.startswith('Don')]
               emp_name  mgr_id mgr_name
        emp_id
        300       Donna     100      Don
        200         Pat     100      Don
        # Example 3: Create a new column with values as
        #            1, if employees manager name starts with 'Don'.
        #            0, else.
        >>> df.assign(new_col=case_when((df.mgr_name.startswith('Don').expression, 1), else_=0))
               emp_name  mgr_id mgr_name new_col
        emp_id
        300       Donna   100.0      Don       1
        500        Fred   400.0      Kim       0
        100         Don     NaN       NA       0
        400         Kim   200.0      Pat       0
        200         Pat   100.0      Don       1
    string_cs
    teradataml.dataframe.sql.DataFrameColumn.string_cs = string_cs()
    DESCRIPTION:
        Function returns a heuristically derived integer value that you can use to help determine
        which KANJI1-compatible client character set was used to encode string_expression.
        The result value can also help determine which client character set to use to interpret
        the character data.
        ===========================================================================================
        | IF the result value is …  | THEN the heuristic found that string_expression  …            |
         ===========================================================================================
        |   -1                          |   most likely uses a single-byte client character set         |
        |                               |   encoding, but it may also contain a mix of encodings.       |
         -------------------------------------------------------------------------------------------
        |   0                           |   does not contain anything distinguishable from any          |
        |                               |   particular character set, so any character set that you use |
        |                               |   to interpret string_expression provides the same result.    |
        |                               |   Not all translations use the same interpretation for the    |
        |                               |   characters represented by 0x5C and 0x7E, however.           |
        |                               |   If string_expression contains:                              |
        |                               |       * 0x5C and you want it to be interpreted as             |
        |                               |         REVERSE SOLIDUS, use a single-byte character set.     |
        |                               |       * 0x7E and you want it to be interpreted as TILDE, use  |
        |                               |         a single-byte character set.                          |
        |                               |       * 0x5C and you want it to be interpreted as YEN SIGN,   |
        |                               |       * 0x7E and you want it to be interpreted as OVERLINE,   |
        |                               |         use any of the following:                             |
        |                               |           * KANJISJIS_0S                                      |
        |                               |           * KANJIEBCDIC5026_0I                                |
        |                               |           * KANJIEBCDIC5035_0I                                |
        |                               |           * KATAKANAEBCDIC                                    |
        |                               |           * KANJIEUC_0U                                       |
         -------------------------------------------------------------------------------------------
        |   1                           |   uses the encoding of one of the following:                  |
        |                               |       * KANJIEBCDIC5026_0I                                    |
        |                               |       * KANJIEBCDIC5035_0I                                    |
        |                               |       * KATAKANAEBCDIC                                        |
         -------------------------------------------------------------------------------------------
        |   2                           |   uses the encoding of KANJIEUC_0U.                           |
         -------------------------------------------------------------------------------------------
        |   3                           |   uses the encoding of KANJISJIS_0S.                          |
         -------------------------------------------------------------------------------------------
        Function helps determine which encoding to use when using the TRANSLATE function to
        translate a string from the KANJI1 server character set to the UNICODE server character set.
         ===========================================================================================
        | IF the result value is …  | THEN substitute the following value for source_TO_target in   |
        |                           | TRANSLATE(string_expression USING source_to_target ) …        |
         ===========================================================================================
        |   -1                          |   KANJI1_SBC_TO_UNICODE.                                      |
         -------------------------------------------------------------------------------------------
        |   0                           |   KANJI1_SBC_TO_UNICODE.                                      |
         -------------------------------------------------------------------------------------------
        |   1                           |   KANJI1_KANJIEBCDIC_TO_UNICODE.                              |
         -------------------------------------------------------------------------------------------
        |   2                           |   KANJI1_KANJIEUC_TO_UNICODE.                                 |
         -------------------------------------------------------------------------------------------
        |   3                           |   KANJI1_KANJISJIS_TO_UNICODE.                                |
         -------------------------------------------------------------------------------------------
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Returns the heuristically derived integer value for character string in "stats"
        #            column and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.string_cs())
        >>> print(res)
           masters   gpa     stats programming  admitted  col
        id
        3       no  3.70    Novice    Beginner         1    0
        4      yes  3.50  Beginner      Novice         1    0
        2      yes  3.76  Beginner    Beginner         0    0
        1      yes  3.95  Beginner    Beginner         0    0
    strip
    teradataml.dataframe.sql.DataFrameColumn.strip = strip(self)
           Remove leading and trailing whitespace.
           REFERENCE:
               SQL Functions, Operators, Expressions, and Predicates
               Chapter 26 String Operators and Functions
           PARAMETERS:
               None
           RETURNS:
               A str Series with leading and trailing whitespace removed
           EXAMPLES:
               >>> load_example_data("dataframe", "sales")
               >>> df = DataFrame("sales")
               >>> df
                             Feb    Jan    Mar    Apr    datetime
               accounts
               Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
               Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
               Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
               Red Inc     200.0  150.0  140.0    NaN  04/01/2017
               Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
               Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
               >>> accounts = df['accounts']
               # create a column with some whitespace
               >>> wdf = df.assign(drop_columns = True,
                                 accounts = accounts,
                                 w_spaces = '
    ' + accounts + '     ')
               >>> wdf
                                     w_spaces
               accounts
               Blue Inc      
    Blue Inc
               Orange Inc  
    Orange Inc
               Red Inc        
    Red Inc
               Yellow Inc  
    Yellow Inc
               Jones LLC    
    Jones LLC
               Alpha Co      
    Alpha Co
               # Example 1: Strip the leading and trailing whitespace.
               >>> wdf.assign(drop_columns = True,
                            wo_wspaces = wdf.w_spaces.str.strip())
                  wo_wspaces
               0    Blue Inc
               1  Orange Inc
               2     Red Inc
               3  Yellow Inc
               4   Jones LLC
               5    Alpha Co
    substr
    teradataml.dataframe.sql.DataFrameColumn.substr = substr(self, start_pos, length)
    DESCRIPTION:
        Function to get substring from column.
    PARAMETERS:
        start_pos:
            Required Argument.
            Specifies starting position to extract string from column.
            Note:
                Index position starts with 1 instead of 0.
            Types: int OR ColumnExpression
        length:
            Required Argument.
            Specifies the length of the string to extract from column.
            Types: int OR ColumnExpression
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        >>> load_example_data("ntree", "employee_table")
        >>> df = DataFrame("employee_table")
               emp_name  mgr_id mgr_name
        emp_id
        200         Pat   100.0      Don
        300       Donna   100.0      Don
        400         Kim   200.0      Pat
        500        Fred   400.0      Kim
        100         Don     NaN       NA
        # Example 1: Create a new column by extracting the first 3 letters
        #            from column emp_name.
        >>> df.assign(new_column = df.emp_name.substr(1,3))
               emp_name  mgr_id mgr_name new_column
        emp_id
        200         Pat   100.0      Don        Pat
        300       Donna   100.0      Don        Don
        400         Kim   200.0      Pat        Kim
        500        Fred   400.0      Kim        Fre
        100         Don     NaN       NA        Don
        # Example 2: Find out the employees whose first three letters
        #            in their name is 'Fre'.
        >>> df[df.emp_name.substr(1,3) == 'Fre']
               emp_name  mgr_id mgr_name new_col
        emp_id
        500        Fred     400      Kim      on
        # Example 3: Create a new column by passing ColumnExpression as
        #            start_pos and length.
        >>> df.assign(new_column = df.emp_name.substr(df.emp_id, df.mgr_id))
                 emp_name  mgr_id mgr_name new_column
        emp_id
        1         Pat   2      Don        Pa
    substring_index
    teradataml.dataframe.sql.DataFrameColumn.substring_index = substring_index(self, delimiter, count)
    DESCRIPTION:
        Function to return the substring from a column before a specified 
        delimiter, up to a given occurrence count.
    PARAMETERS:
        delimiter:
            Required Argument.
            Specifies the delimiter string to split the column values.
            Types: str
        count:
            Required Argument.
            Specifies the number of occurrences of the delimiter to consider.
            If positive, the substring is extracted from the start of the string.
            If negative, the substring is extracted from the end of the string.
            If zero, an empty string is returned.
            Types: int
    RAISES:
        TeradataMlException
    RETURNS:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe","admissions_train")
        >>> df = DataFrame('admissions_train')
        # Create a new column 'delim_col' with string.
        >>> df1 = df.assign(delim_col = 'ab.c.def.g')
        >>> df1
           masters   gpa     stats programming  admitted   delim_col
        id                                                          
        38     yes  2.65  Advanced    Beginner         1  ab.c.def.g
        7      yes  2.33    Novice      Novice         1  ab.c.def.g
        26     yes  3.57  Advanced    Advanced         1  ab.c.def.g
        5       no  3.44    Novice      Novice         0  ab.c.def.g
        3       no  3.70    Novice    Beginner         1  ab.c.def.g
        22     yes  3.46    Novice    Beginner         0  ab.c.def.g
        1      yes  3.95  Beginner    Beginner         0  ab.c.def.g
        17      no  3.83  Advanced    Advanced         1  ab.c.def.g
        15     yes  4.00  Advanced    Advanced         1  ab.c.def.g
        34     yes  3.85  Advanced    Beginner         0  ab.c.def.g
        # Example 1: Create a new column 'new_column' by extracting the substring
                     based on positive count.
        >>> res = df1.assign(new_column = df1.delim_col.substring_index('.', 2))
        >>> res
           masters   gpa     stats programming  admitted   delim_col new_column
        id                                                                     
        34     yes  3.85  Advanced    Beginner         0  ab.c.def.g       ab.c
        32     yes  3.46  Advanced    Beginner         0  ab.c.def.g       ab.c
        11      no  3.13  Advanced    Advanced         1  ab.c.def.g       ab.c
        30     yes  3.79  Advanced      Novice         0  ab.c.def.g       ab.c
        28      no  3.93  Advanced    Advanced         1  ab.c.def.g       ab.c
        16      no  3.70  Advanced    Advanced         1  ab.c.def.g       ab.c
        35      no  3.68    Novice    Beginner         1  ab.c.def.g       ab.c
        40     yes  3.95    Novice    Beginner         0  ab.c.def.g       ab.c
        19     yes  1.98  Advanced    Advanced         0  ab.c.def.g       ab.c
        # Example 2: Create a new column 'new_column' by extracting the substring
                     based on negative count.
        >>> res = df1.assign(new_column = df1.delim_col.substring_index('.', -3))
        >>> res
           masters   gpa     stats programming  admitted   delim_col new_column
        id                                                                     
        34     yes  3.85  Advanced    Beginner         0  ab.c.def.g    c.def.g
        32     yes  3.46  Advanced    Beginner         0  ab.c.def.g    c.def.g
        11      no  3.13  Advanced    Advanced         1  ab.c.def.g    c.def.g
        30     yes  3.79  Advanced      Novice         0  ab.c.def.g    c.def.g
        28      no  3.93  Advanced    Advanced         1  ab.c.def.g    c.def.g
        16      no  3.70  Advanced    Advanced         1  ab.c.def.g    c.def.g
        35      no  3.68    Novice    Beginner         1  ab.c.def.g    c.def.g
        40     yes  3.95    Novice    Beginner         0  ab.c.def.g    c.def.g
        19     yes  1.98  Advanced    Advanced         0  ab.c.def.g    c.def.g
        # Example 3: Create a new column 'new_column' by extracting the substring
                     with 2-character delimiter based on positive count.
        >>> res = df1.assign(new_column = df1.delim_col.substring_index('c.d', 1))
        >>> res
           masters   gpa     stats programming  admitted   delim_col new_column
        id                                                                     
        34     yes  3.85  Advanced    Beginner         0  ab.c.def.g        ab.
        32     yes  3.46  Advanced    Beginner         0  ab.c.def.g        ab.
        11      no  3.13  Advanced    Advanced         1  ab.c.def.g        ab.
        30     yes  3.79  Advanced      Novice         0  ab.c.def.g        ab.
        28      no  3.93  Advanced    Advanced         1  ab.c.def.g        ab.
        16      no  3.70  Advanced    Advanced         1  ab.c.def.g        ab.
        35      no  3.68    Novice    Beginner         1  ab.c.def.g        ab.
        40     yes  3.95    Novice    Beginner         0  ab.c.def.g        ab.
    to_char
    teradataml.dataframe.sql.DataFrameColumn.to_char = to_char(self, formatter=None)
    DESCRIPTION:
        Converts numeric type or datetype to character type.
    PARAMETERS:
        formatter:
            Optional Argument.
            Specifies the format for formatting the values of the column.
            Type: str OR ColumnExpression 
            Note:
               * If 'formatter' is omitted, numeric values is converted to a string exactly
                 long enough to hold its significant digits.
            * Formatter for Numeric types:
                +--------------------------------------------------------------------------------------------------+
                |    FORMATTER                             DESCRIPTION                                             |
                +--------------------------------------------------------------------------------------------------+
                |    , (comma)                             A comma in the specified position.                      |
                |                                          A comma cannot begin a number format.                   |
                |                                          A comma cannot appear to the right of a decimal         |
                |                                          character or period in a number format.                 |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    1234             9,999             1,234     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |   . (period)                             A decimal point.                                        |
                |                                          User can only specify one period in a number format.    |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    123.46             9999.9             123.5  | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    $                                     A value with a leading dollar sign.                     |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    1234             $9999              $1234    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    0                                     Leading zeros.                                          |
                |                                          Trailing zeros.                                         |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    1234             09999              01234    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    9                                     A value with the specified number of digits with a      |
                |                                          leading space if positive or with a leading minus       |
                |                                          if negative.                                            |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    1234             9999              1234      | |
                |                                              |    1234             999               ####      | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    B                                     Blank space for the integer part of a fixed point number|
                |                                          when the integer part is zero.                          |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    0.1234             B.999          Blank space| |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    C                                     The ISO currency symbol as specified in the ISOCurrency |
                |                                          element in the SDF file.                                |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    234              C999              USD234    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    D                                     The character that separates the integer and fractional |
                |                                          part of non-monetary values.                            |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    234.56           999D9             234.6     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    EEEE                                  A value in scientific notation.                         |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    234.56           9.9EEEE             2.3E+02 | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    G                                     The character that separates groups of digits in the    |
                |                                          integer part of non-monetary values.                    |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    123456           9G99G99           1,234,56  | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    L                                     The string representing the local currency as specified |
                |                                          in the Currency element according to system settings.   |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    234              L999              $234      | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MI                                    A trailing minus sign if the value is negative.         |
                |                                          The MI format element can appear only in the last       |
                |                                          position of a number format.                            |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    -1234            9999MI            1234-     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    PR                                    A negative value in <angle brackets>, or                |
                |                                          a positive value with a leading and trailing blank.     |
                |                                          The PR format element can appear only in the last       |
                |                                          position of a number format.                            |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    -1234            9999PR            <1234>    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    S                                     A negative value with a leading or trailing minus sign. |
                |                                          a positive value with a leading or trailing plus sign.  |
                |                                          The S format element can appear only in the first or    |
                |                                          last position of a number format.                       |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    +1234            S9999             +1234     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    TM                                    (text minimum format) Returns the smallest number of    |
                |                                          characters possible. This element is case insensitive.  |
                |                                          TM or TM9 return the number in fixed notation unless    |
                |                                          the output exceeds 64 characters. If the output exceeds |
                |                                          64 characters, the number is returned in scientific     |
                |                                          notation.                                               |
                |                                          TME returns the number in scientific notation with the  |
                |                                          smallest number of characters.                          |
                |                                          You cannot precede this element with an other element.  |
                |                                          You can follow this element only with one 9 or one E    |
                |                                          (or e), but not with any combination of these.          |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    1234             TM                1234      | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    U                                    (dual currency) The string that represents the dual     |
                |                                          currency as specified in the DualCurrency element       |
                |                                          according to system settings.                           |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    1234             U9999             $1234     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    V                                     A value multiplied by 10 to the n (and, if necessary,   |
                |                                          rounded up), where n is the number of 9's after the V.  |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    1234             9999V99           123400    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    X                                     The hexadecimal value of the specified number of digits.|
                |                                          If the specified number is not an integer, the function |
                |                                          will round it to an integer.                            |
                |                                          This element accepts only positive values or zero.      |
                |                                          Negative values return an error. You can precede this   |
                |                                          element only with zero (which returns leading zeros) or |
                |                                          FM. Any other elements return an error. If you do not   |
                |                                          specify zero or FM, the return always has one leading   |
                |                                          blank.                                                  |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    1234             XXXX              4D2       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
            * Formatter for Date types:
                +--------------------------------------------------------------------------------------------------+
                |    FORMATTER                             DESCRIPTION                                             |
                +--------------------------------------------------------------------------------------------------+
                |    -                                                                                             |
                |    /                                                                                             |
                |    ,                                     Punctuation characters are ignored and text enclosed in |
                |    .                                     quotation marks is ignored.                             |
                |    ;                                                                                             |
                |    :                                                                                             |
                |    "text"                                                                                        |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         MM-DD             09-17     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    AD                                    AD indicator.                                           |
                |    A.D.                                                                                          |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         CCAD              21AD      | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    AM                                    Meridian indicator.                                     |
                |    A.M.                                                                                          |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         CCAM              21AM     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    BC                                                                                            |
                |    B.C.                                  BC indicator.                                           |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         CCBC              21BC      | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    CC                                    Century.                                                |
                |    SCC                                   If the last 2 digits of a 4-digit year are between 01   |
                |                                          and 99 inclusive, the century is 1 greater than the     |
                |                                          first 2 digits of that year.                            |
                |                                          If the last 2 digits of a 4-digit year are 00, the      |
                |                                          century is the same as the first 2 digits of that year. |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         CCBC              21BC      | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    D                                     Day of week (1-7).                                      |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         D                 4         | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DAY                                   Name of day.                                            |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         DAY               WEDNESDAY | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DD                                    Day of month (1-31).                                    |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         DD                17        | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DDD                                   Day of year (1-366).                                    |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         DDD               260       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DL                                    Date Long. Equivalent to the format string ‘FMDay,      |
                |                                          Month FMDD, YYYY’.                                      |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              | data       formatter         result             | |
                |                                              +-------------------------------------------------+ |
                |                                              | 03/09/17   DL      Wednesday, September 17, 2003| |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DS                                    Date Short. Equivalent to the format string             |
                |                                          ‘FMMM/DD/YYYYFM’.                                       |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         DS                9/17/2003 | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    DY                                    abbreviated name of day.                                |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              |    data             formatter         result    | |
                |                                              +-------------------------------------------------+ |
                |                                              |    03/09/17         DY                WED       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    FF                                    [1..9]   Fractional seconds.                            |
                |                                          Use [1..9] to specify the number of fractional digits.  |
                |                                          FF without any number following it prints a decimal     |
                |                                          followed by digits equal to the number of fractional    |
                |                                          seconds in the input data type. If the data type has no |
                |                                          fractional digits, FF prints nothing.                   |
                |                                          Any fractional digits beyond 6 digits are truncated.    |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  FF         000000   | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    HH                                                                                            |
                |    HH12                                 Hour of day (1-12).                                      |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  HH         09       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    HH24                                 Hour of the day (0-23).                                  |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  HH24         09     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    IW                                   Week of year (1-52 or 1-53) based on ISO model.          |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  IW         01       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    IYY                                                                                           |
                |    IY                                   Last 3, 2, or 1 digits of ISO year.                      |
                |    I                                                                                             |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  IY         16       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    IYYY                                 4-digit year based on the ISO standard.                  |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  IYYY         2016   | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    J                                    Julian day, the number of days since January 1, 4713 BC. |
                |                                         Number specified with J must be integers.                |
                |                                         Teradata uses the Gregorian calendar in calculations to  |
                |                                         and from Julian Days.                                    |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  J          2457394  | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MI                                   Minute (0-59).                                           |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  MI         08       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MM                                   Month (01-12).                                           |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  MM         01       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MON                                  Abbreviated name of month.                               |
                |                                          Example:                                                |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  MON        JAN      | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    MONTH                                Name of month.                                           |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  MONTH      JANUARY  | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    PM                                                                                            |
                |    P.M.                                 Meridian indicator.                                      |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  HHPM       09PM     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    Q                                    Quarter of year (1, 2, 3, 4).                            |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  Q       1           | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    RM                                   Roman numeral month (I - XII).                           |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  RM         I        | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    SP                                   Spelled. Any numeric element followed by SP is spelled in|
                |                                         English words. The words are capitalized according to how|
                |                                         the element is capitalized.                              |
                |                                         For example: 'DDDSP' specifies all uppercase, 'DddSP'    |
                |                                         specifies that the first letter is capitalized, and      |
                |                                         'dddSP' specifies all lowercase.                         |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  HHSP       NINE     | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    SS                                   Second (0-59).                                           |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  SS         03       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    SSSSS                                Seconds past midnight (0-86399).                         |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  SSSSS      32883    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    TS                                   Time Short. Equivalent to the format string              |
                |                                         'HH:MI:SS AM'.                                           |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  TS     09:08:01 AM  | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    TZH                                  Time zone hour.                                          |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  TZH        +00      | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    TZM                                  Time zone minute.                                        |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  TZM        00       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    TZR                                  Time zone region. Equivalent to the format string        |
                |                                         'TZH:TZM'.                                               |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  TZR        +00:00   | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    WW                                   Week of year (1-53) where week 1 starts on the first day |
                |                                         of the year and continues to the 7th day of the year.    |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  WW         01       | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    W                                    Week of month (1-5) where week 1 starts on the first day |
                |                                         of the month and ends on the seventh.                    |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  W          1        | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    X                                    Local radix character.                                   |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  MMXYY      01.16    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    Y,YYY                                Year with comma in this position.                        |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                        formatter  result   | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  Y,YYY      2,016    | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    YEAR                                 Year, spelled out. S prefixes BC dates with a minus sign.|
                |    SYEAR                                                                                         |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                     formatter  result      | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  YEAR  TWENTY SIXTEEN| |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    YYYY                                                                                          |
                |    SYYYY                                4-digit year. S prefixes BC dates with a minus sign.     |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                     formatter  result      | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  YYYY    2016        | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
                |    YYY                                  Last 3, 2, or 1 digit of year.                           |
                |    YY                                   If the current year and the specified year are both in   |
                |    Y                                    the range of 0-49, the date is in the current century.   |
                |                                         Example:                                                 |
                |                                              +-------------------------------------------------+ |
                |                                              | data                     formatter  result      | |
                |                                              +-------------------------------------------------+ |
                |                                              | 2016-01-06 09:08:01.000000  YY      16          | |
                |                                              +-------------------------------------------------+ |
                +--------------------------------------------------------------------------------------------------+
    RAISES:
        TypeError, ValueError, TeradataMlException
    Returns:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml", "tochar_data")
        # Create a DataFrame on 'tochar_data' table.
        >>> df = DataFrame("tochar_data")
        >>> df
            int_col  float_col  date_col int_format float_format date_format
        id                                                                  
        3      1314     123.46  03/09/17       XXXX          TM9          DY
        0      1234     234.56  03/09/17      9,999        999D9       MM-DD
        2       789     123.46  03/09/17       0999       9999.9         DAY
        1       456     234.56  03/09/17       $999      9.9EEEE        CCAD
        >>> df.tdtypes
        COLUMN NAME                                    TYPE
        id                                        INTEGER()
        int_col                                   INTEGER()
        float_col                                   FLOAT()
        date_col                                     DATE()
        int_format      VARCHAR(length=20, charset='LATIN')
        float_format    VARCHAR(length=20, charset='LATIN')
        date_format     VARCHAR(length=20, charset='LATIN')
        # Example 1: Convert 'int_col' column to character type.
        >>> res = df.assign(int_col = df.int_col.to_char())
        >>> res
            int_col  float_col  date_col int_format float_format date_format
        id                                                                 
        0     1234     234.56  03/09/17      9,999        999D9       MM-DD
        3     1314     123.46  03/09/17       XXXX          TM9          DY
        2      789     123.46  03/09/17       0999       9999.9         DAY
        1      456     234.56  03/09/17       $999      9.9EEEE        CCAD
        >>> res.tdtypes
        COLUMN NAME                                    TYPE
        id                                        INTEGER()
        int_col                                   VARCHAR()
        float_col                                   FLOAT()
        date_col                                     DATE()
        int_format      VARCHAR(length=20, charset='LATIN')
        float_format    VARCHAR(length=20, charset='LATIN')
        date_format     VARCHAR(length=20, charset='LATIN')
        # Example 2: Convert 'float_col' column to character type in '$999.9' format.
        >>> res = df.assign(char_col = df.float_col.to_char('$999.9'))
        >>> res
            int_col  float_col  date_col int_format float_format date_format char_col
        id                                                                           
        0      1234     234.56  03/09/17      9,999        999D9       MM-DD   $234.6
        3      1314     123.46  03/09/17       XXXX          TM9          DY   $123.5
        2       789     123.46  03/09/17       0999       9999.9         DAY   $123.5
        1       456     234.56  03/09/17       $999      9.9EEEE        CCAD   $234.6
        # Example 3: Convert 'date_col' column to character type in 'YYYY-DAY-MONTH' format
        >>> res = df.assign(char_col = df.date_col.to_char('YYYY-DAY-MONTH'))
        >>> res
            int_col  float_col  date_col int_format float_format date_format                  char_col
        id                                                                                            
        3      1314   123.4600  03/09/17       XXXX          TM9          DY  1903-THURSDAY -SEPTEMBER
        0      1234   234.5600  03/09/17      9,999        999D9       MM-DD  1903-THURSDAY -SEPTEMBER
        2       789   123.4600  03/09/17       0999       9999.9         DAY  1903-THURSDAY -SEPTEMBER
        1       456   234.5600  03/09/17       $999      9.9EEEE        CCAD  1903-THURSDAY -SEPTEMBER
        # Example 4: Convert 'int_col' column to character type in 'int_format' column format.
        >>> res = df.assign(char_col = df.int_col.to_char(df.int_format))
        >>> res
            int_col  float_col  date_col int_format float_format date_format char_col
        id                                                                           
        0      1234     234.56  03/09/17      9,999        999D9       MM-DD    1,234
        3      1314     123.46  03/09/17       XXXX          TM9          DY      522
        2       789     123.46  03/09/17       0999       9999.9         DAY     0789
        1       456     234.56  03/09/17       $999      9.9EEEE        CCAD     $456
        # Example 5: Convert 'float_col' column to character type in 'float_format' column format.
        >>> res = df.assign(char_col = df.float_col.to_char(df.float_format))
        >>> res
            int_col  float_col  date_col int_format float_format date_format   char_col
        id                                                                             
        2       789     123.46  03/09/17       0999       9999.9         DAY      123.5
        3      1314     123.46  03/09/17       XXXX          TM9          DY     123.46
        1       456     234.56  03/09/17       $999      9.9EEEE        CCAD    2.3E+02
        0      1234     234.56  03/09/17      9,999        999D9       MM-DD      234.6
        # Example 4: Convert 'date_col' column to character type in 'date_format' column format.
        >>> res = df.assign(char_col = df.date_col.to_char(df.date_format))
        >>> res
            int_col  float_col  date_col int_format float_format date_format   char_col
        id                                                                             
        0      1234     234.56  03/09/17      9,999        999D9       MM-DD      09-17
        3      1314     123.46  03/09/17       XXXX          TM9          DY        THU
        2       789     123.46  03/09/17       0999       9999.9         DAY   THURSDAY 
        1       456     234.56  03/09/17       $999      9.9EEEE        CCAD       20AD
    translate
    teradataml.dataframe.sql.DataFrameColumn.translate = translate(expression, replace_string)
    DESCRIPTION:
        Function returns values in column with every occurrence of each character in
        "expression" replaced with the corresponding character in "replace_string".
        If the first character in "expression" occurs in the values in column, all
        occurrences of it are replaced by the first character in "replace_string". This
        repeats for all characters in "expression" and for all characters in
        "replace_string". The replacement is performed character-by-character, that is,
        the replacement of the second character is done on the string resulting
        from the replacement of the first character.
        If "expression" contains more characters than "replace_string", the extra characters
        are removed from the string values in column.
        If "expression" contains fewer characters than "replace_string", the extra characters
        in "replace_string" have no effect.
        If the same character occurs more than once in "expression", only the replacement
        character from the "replace_string" corresponding to the first occurrence is used.
    ALTERNATE NAME:
        otranslate
    PARAMETERS:
        expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that will be replaced in string values in column.
            If argument is NULL, the function returns string values in column.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR
            Types: ColumnExpression, str
        replace_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that replaces the characters specified by expression.
            If argument is NULL or empty, the function removes the characters
            specified in expression.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR
            Types: ColumnExpression, str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Replaces the occurrence of 'i' in the string in "stats" column by the
        #            character in to_string ('X') and pass it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.translate("i", "X"))
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1    NovXce
        4      yes  3.50  Beginner      Novice         1  BegXnner
        2      yes  3.76  Beginner    Beginner         0  BegXnner
        1      yes  3.95  Beginner    Beginner         0  BegXnner
        # Example 2: Executed translate() function on "stats" column and filtered computed
        #            values which are equal to 'BegXnner'.
        >>> print(df[df.stats.translate("i", "X") == "BegXnner"])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    trim
    teradataml.dataframe.sql.DataFrameColumn.trim = trim(self, expression=' ')
    DESCRIPTION:
        Function trims the string values in the column.
    PARAMETERS:
        expression:
            Optional Argument.
            Specifies a ColumnExpression of a string column or a string literal to
            be trimmed. If "expression" is specified, it must be the same data type
            as string values in column.
            Default Value: ' '
            Type: str or ColumnExpression
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train")
        >>> df
            masters   gpa     stats programming  admitted
        id
        38     yes  2.65  Advanced    Beginner         1
        7      yes  2.33    Novice      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        3       no  3.70    Novice    Beginner         1
        22     yes  3.46    Novice    Beginner         0
        24      no  1.87  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        19     yes  1.98  Advanced    Advanced         0
        40     yes  3.95    Novice    Beginner         0
        # Example 1: Trim "Begi" from the strings in column "programing".
        >>> res = df.assign(trim_col = df.programming.trim("Begi"))
        >>> res
            masters   gpa     stats programming  admitted  trim_col
        id
        38     yes  2.65  Advanced    Beginner         1      nner
        7      yes  2.33    Novice      Novice         1     Novic
        26     yes  3.57  Advanced    Advanced         1  Advanced
        5       no  3.44    Novice      Novice         0     Novic
        3       no  3.70    Novice    Beginner         1      nner
        22     yes  3.46    Novice    Beginner         0      nner
        24      no  1.87  Advanced      Novice         1     Novic
        36      no  3.00  Advanced      Novice         0     Novic
        19     yes  1.98  Advanced    Advanced         0  Advanced
        40     yes  3.95    Novice    Beginner         0      nner
        # Example 2: Filter the rows where values in the column "programming" result
        #            in "nner" after it is trimmed with 'Begi'.
        >>> df[df.programming.trim("Begi") == "nner"]
            masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        34     yes  3.85  Advanced    Beginner         0
        35      no  3.68    Novice    Beginner         1
        31     yes  3.50  Advanced    Beginner         1
        29     yes  4.00    Novice    Beginner         0
        32     yes  3.46  Advanced    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        # Example 3: Trim string in "programing" column using "masters" column as argument.
        >>> res = df.assign(trim_col = df.programming.trim(df.masters))
        >>> res
            masters   gpa     stats programming  admitted  trim_col
        id
        38     yes  2.65  Advanced    Beginner         1  Beginner
        7      yes  2.33    Novice      Novice         1     Novic
        26     yes  3.57  Advanced    Advanced         1  Advanced
        17      no  3.83  Advanced    Advanced         1  Advanced
        34     yes  3.85  Advanced    Beginner         0  Beginner
        13      no  4.00  Advanced      Novice         1    Novice
        32     yes  3.46  Advanced    Beginner         0  Beginner
        11      no  3.13  Advanced    Advanced         1  Advanced
        15     yes  4.00  Advanced    Advanced         1  Advanced
        36      no  3.00  Advanced      Novice         0    Novice
    upper
    teradataml.dataframe.sql.DataFrameColumn.upper = upper()
    DESCRIPTION:
        Function returns a character string identical to string values in column,
        except that all lowercase letters are replaced with their uppercase equivalents.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train").iloc[:4]
        >>> print(df)
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        # Example 1: Converts strings in "stats" column to uppercase and pass
        #            it as input to DataFrame.assign().
        >>> res = df.assign(col = df.stats.upper())
        >>> print(res)
           masters   gpa     stats programming  admitted       col
        id
        3       no  3.70    Novice    Beginner         1    NOVICE
        4      yes  3.50  Beginner      Novice         1  BEGINNER
        2      yes  3.76  Beginner    Beginner         0  BEGINNER
        1      yes  3.95  Beginner    Beginner         0  BEGINNER
        # Example 2: Executed upper() function on "stats" column and filtered computed
        #            values which are equal to 'BEGINNER'.
        >>> print(df[df.stats.upper() == "BEGINNER"])
           masters   gpa     stats programming  admitted
        id
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
    DataFrameColumn Time Series Functions
    ﬁrst
    teradataml.dataframe.sql.DataFrameColumn.ﬁrst = ﬁrst(self, **kwargs)
    DESCRIPTION:
        Function returns oldest value, determined by the timecode, for each group
        in a column.
        Note:
            This can only be used as Time Series Aggregate function.
    PARAMETERS:
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
    EXAMPLES:
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cd",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.temperature.first()
    last
    teradataml.dataframe.sql.DataFrameColumn.last = last(self, **kwargs)
    DESCRIPTION:
        Function returns newest value, determined by the timecode, for each group
        in a column.
        Note:
            This can only be used as Time Series Aggregate function.
    PARAMETERS:
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
    EXAMPLES:
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cd",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.temperature.last()
    mad
    teradataml.dataframe.sql.DataFrameColumn.mad = mad(self, constant_multiplier=None, **kwargs)
    DESCRIPTION:
        Function returns the median of the set of values defined as the
        absolute value of the difference between each value and the median
        of all values in each group.
        Formula for computing MAD is as follows:
            MAD = b * Mi(|Xi - Mj(Xj)|)
            Where,
                b       = Some numeric constant. Default value is 1.4826.
                Mj(Xj)  = Median of the original set of values.
                Xi      = The original set of values.
                Mi      = Median of absolute value of the difference between
                          each value in Xi and the Median calculated in Mj(Xj).
        Note:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
            3. This can only be used as Time Series Aggregate function.
    PARAMETERS:
        constant_multiplier:
            Optional Argument.
            Specifies a numeric values to be used as constant multiplier
            (b in the above formula). It should be any numeric value
            greater than or equal to 0.
            Note:
                When this argument is not used, Vantage uses 1.4826 as
                constant multiplier.
            Default Values: None
            Types: int or float
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
    EXAMPLES:
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti"])
        # Example 1: Calculate Median Absolute Deviation for all columns over 1 calendar day of
        #            timebucket duration. Use default constant multiplier.
        #            No need to pass any arguments.
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="1cd",value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.temperature.mad()
        # Example 2: Calculate MAD values using 2 as constant multiplier for all the columns
        #            in ocean_buoys_seq DataFrame on sequenced PTI table.
        >>> # DataFrame on sequenced PTI table
        ... ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> ocean_buoys_seq_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="CAL_DAYS(2)", value_expression="buoyid", fil
        >>> constant_multiplier_columns = {2: "*"}
        >>> ocean_buoys_seq_grpby1.temperature.mad(constant_multiplier_columns)
    mode
    teradataml.dataframe.sql.DataFrameColumn.mode = mode(self, **kwargs)
    DESCRIPTION:
        Function to get the mode value for a column.
        Note:
            This can only be used as Time Series Aggregate function.
    PARAMETERS:
        kwargs:
            Specifies optional keyword arguments.
    RETURNS:
         ColumnExpression
    RAISES:
        RuntimeError - If column does not support the aggregate operation.
    EXAMPLES:
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti"])
        # Example 1: Executing mode function on DataFrame column created on non-sequenced PTI table.
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="10m",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.temperature.mode()
    DataFrameColumn Bit Byte Functions
    bit_and
    teradataml.dataframe.sql.DataFrameColumn.bit_and = bit_and(bit_mask)
    DESCRIPTION:
        Function performs the logical AND operation on the binary representation of
        the integer or byte value in column and corresponding bits from input argument.
        Function takes two bit patterns of equal length and performs the logical AND
        operation on each pair of corresponding bits as follows:
            * The result is 0 if either of the two bits are 0 or both the bits are 0.
            * The result is 1 if both the bits at same position is 1.
        If the binary representation of the integer or byte value in column and "bit_mask"
        argument differ in length, the arguments are processed as follows:
            * The value in column and "bit_mask" arguments are aligned on their least
              significant byte/bit.
            * The smaller argument is padded with zeros to the left until it becomes
              the same size as the larger argument.
    ALTERNATE NAME:
        bitand
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of a numeric or byte column or a numeric or byte
            literal value.
            Note:
                1. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types:
                ColumnExpression, int
                The data type of the "bit_mask" parameter varies depending upon the data type
                of the bits in column. The following table shows the supported column/literal
                value types for bits in column and "bit_mask" parameters and their permitted
                combinations:
                 ===================================================
                | bits in column type | "bit_mask" type             |
                 ===================================================
                | BYTEINT             | BYTE(1) or BYTEINT          |
                | SMALLINT            | BYTE(2) or SMALLINT         |
                | INTEGER             | BYTE(4) or INTEGER          |
                | BIGINT              | BYTE(8) or BIGINT           |
                | NUMBER(38,0)        | VARBYTE(16) or NUMBER(38,0) |
                | VARBYTE(n)          | VARBYTE(n)                  |
                 ===================================================
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Performs logical AND operation operation on "id" and "byte_col" columns
        #           and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.id_col.bit_and(df.byte_col))
        >>> print(res_df)
               byte_col      varbyte_col                blob_col  col
        id_col
        2         b'61'      b'627A7863'     b'6162636431323233'    0
        1         b'62'  b'616263643132'     b'3331363136323633'    0
        0         b'63'      b'62717765'     b'3330363136323633'    0
        # Example2: Performs logical AND operation operation on "varbyte_col" and "byte_col"
        #           columns and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.varbyte_col.bit_and(df.byte_col))
        >>> print(res_df)
               byte_col      varbyte_col                blob_col    col
        id_col
        2         b'61'      b'627A7863'     b'6162636431323233'  b'61'
        1         b'62'  b'616263643132'     b'3331363136323633'  b'22'
        0         b'63'      b'62717765'     b'3330363136323633'  b'61'
    bit_get
    teradataml.dataframe.sql.DataFrameColumn.bit_get = bit_get(bit_mask)
    DESCRIPTION:
        Function returns the bit specified by "bit_mask" from the integer or byte value
        in column and returns either 0 or 1 to indicate the value of that bit.
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal.
            Notes:
                1. The range of input values can vary from 0 (bit 0 is the least significant
                   bit) to the (sizeof(integer or byte value in column) - 1).
                2. If "bit_mask" is negative or out-of-range (meaning that it exceeds the size
                   of integer or byte value in column), an error is returned.
                3. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Retrieves the fourth bit from the values in "varbyte_col" column
        #           and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col= df.varbyte_col.bit_get(3))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col  col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'    0
        1         b'62'      b'62717765'  b'3331363136323633'    0
        0         b'63'      b'627A7863'  b'3330363136323633'    0
    bit_or
    teradataml.dataframe.sql.DataFrameColumn.bit_or = bit_or(bit_mask)
    DESCRIPTION:
        Function performs the logical OR operation on the binary representation of the
        integer or byte value in column and corresponding bits from input argument.
        Function takes two bit patterns of equal length and performs the logical OR
        operation on each pair of corresponding bits as follows:
            * The result is 1 if either of the two bits are 1.
            * The result is 0 if both the bits are 0.
        If the binary representation of the integer or byte value in column and "bit_mask"
        argument differ in length, the arguments are processed as follows:
            * The value in column and "bit_mask" arguments are aligned on their least
              significant byte/bit.
            * The smaller argument is padded with zeros to the left until it becomes
              the same size as the larger argument.
    ALTERNATE NAME:
        bitor
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of a numeric or byte column or a numeric or byte
            literal value.
            Note:
                1. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types:
                ColumnExpression, int
                The data type of the "bit_mask" parameter varies depending upon the data type
                of the bits in column. The following table shows the supported column/literal
                value types for bits in column and "bit_mask" parameters and their permitted
                combinations:
                 ===================================================
                | bits in column type | "bit_mask" type             |
                 ===================================================
                | BYTEINT             | BYTE(1) or BYTEINT          |
                | SMALLINT            | BYTE(2) or SMALLINT         |
                | INTEGER             | BYTE(4) or INTEGER          |
                | BIGINT              | BYTE(8) or BIGINT           |
                | NUMBER(38,0)        | VARBYTE(16) or NUMBER(38,0) |
                | VARBYTE(n)          | VARBYTE(n)                  |
                 ===================================================
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Performs bit_or operation on "id" and "byte_col" columns and
        #           and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col=df.id_col.bit_or(df.byte_col))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col          col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'   1627389954
        1         b'62'      b'62717765'  b'3331363136323633'   1644167169
        0         b'63'      b'627A7863'  b'3330363136323633'   1660944384
    bit_xor
    teradataml.dataframe.sql.DataFrameColumn.bit_xor = bit_xor(bit_mask)
    DESCRIPTION:
        Function performs a bitwise XOR operation on the binary representation of the
        integer or byte column and corresponding bits from input argument.
        Function takes two bit patterns of equal length and performs the exclusive OR
        operation on each pair of corresponding bits as follows:
            * The result in each position is 1 if the two bits are different.
            * The result in each position is 0 if the two bits are same.
        If the binary representation of the integer or byte value in column and "bit_mask"
        argument differ in length, the arguments are processed as follows:
            * The value in column and "bit_mask" arguments are aligned on their least
              significant byte/bit.
            * The smaller argument is padded with zeros to the left until it becomes
              the same size as the larger argument.
    ALTERNATE NAME:
        bitxor
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            Note:
                1. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types:
                ColumnExpression, int
                The data type of the "bit_mask" parameter varies depending upon the data type
                of the integer or byte column. The following table shows the supported column/literal
                value types for integer or byte column and "bit_mask" parameters and their permitted
                combinations:
                 ===============================================
                | value of column type | bit_mask type          |
                 ===============================================
                | BYTEINT              | BYTE(1) or BYTEINT     |
                | SMALLINT             | BYTE(2) or SMALLINT    |
                | INTEGER              | BYTE(4) or INTEGER     |
                | BIGINT               | BYTE(8) or BIGINT      |
                | VARBYTE(n)           | VARBYTE(n)             |
                 ===============================================
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Performs bitxor operation on "id" and "byte_col" columns and pass
        #           it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.id_col.bit_xor(df.byte_col))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col         col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  1627389954
        1         b'62'      b'62717765'  b'3331363136323633'  1644167169
        0         b'63'      b'627A7863'  b'3330363136323633'  1660944384
    bitand
    teradataml.dataframe.sql.DataFrameColumn.bitand = bitand(bit_mask)
    DESCRIPTION:
        Function performs the logical AND operation on the binary representation of
        the integer or byte value in column and corresponding bits from input argument.
        Function takes two bit patterns of equal length and performs the logical AND
        operation on each pair of corresponding bits as follows:
            * The result is 0 if either of the two bits are 0 or both the bits are 0.
            * The result is 1 if both the bits at same position is 1.
        If the binary representation of the integer or byte value in column and "bit_mask"
        argument differ in length, the arguments are processed as follows:
            * The value in column and "bit_mask" arguments are aligned on their least
              significant byte/bit.
            * The smaller argument is padded with zeros to the left until it becomes
              the same size as the larger argument.
    ALTERNATE NAME:
        bit_and
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of a numeric or byte column or a numeric or byte
            literal value.
            Note:
                1. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types:
                The data type of the "bit_mask" parameter varies depending upon the data type
                of the bits in column. The following table shows the supported column/literal
                value types for bits in column and "bit_mask" parameters and their permitted
                combinations:
                 ==================================================
                | bits in column type | "bit_mask" type             |
                 ===================================================
                | BYTEINT             | BYTE(1) or BYTEINT          |
                | SMALLINT            | BYTE(2) or SMALLINT         |
                | INTEGER             | BYTE(4) or INTEGER          |
                | BIGINT              | BYTE(8) or BIGINT           |
                | NUMBER(38,0)        | VARBYTE(16) or NUMBER(38,0) |
                | VARBYTE(n)          | VARBYTE(n)                  |
                 ===================================================
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Performs logical AND operation operation on "id" and "byte_col" columns
        #           and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.id_col.bitand(df.byte_col))
        >>> print(res_df)
               byte_col      varbyte_col                blob_col  col
        id_col
        2         b'61'      b'627A7863'     b'6162636431323233'    0
        1         b'62'  b'616263643132'     b'3331363136323633'    0
        0         b'63'      b'62717765'     b'3330363136323633'    0
        # Example2: Performs logical AND operation operation on "varbyte_col" and "byte_col"
        #           columns and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.varbyte_col.bitand(df.byte_col))
        >>> print(res_df)
               byte_col      varbyte_col                blob_col    col
        id_col
        2         b'61'      b'627A7863'     b'6162636431323233'  b'61'
        1         b'62'  b'616263643132'     b'3331363136323633'  b'22'
        0         b'63'      b'62717765'     b'3330363136323633'  b'61'
    bitnot
    teradataml.dataframe.sql.DataFrameColumn.bitnot = bitnot()
    DESCRIPTION:
        Function performs a bitwise complement on the binary representation of integer or byte
        value in column. The bitwise NOT, or complement, is a unary operation which performs
        logical negation on each bit, forming the ones' complement of the specified binary value.
        The digits in the argument which were 0 become 1, and vice versa.
    ALTERNATE NAMES:
        * bitwiseNOT
        * bitwise_not
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Performs a bitwise complement operation on "varbyte_col" column and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.varbyte_col.bitnot())
        >>> print(res_df)
               byte_col      varbyte_col             blob_col              col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  b'-616263643133'
        1         b'62'      b'62717765'  b'3331363136323633'      b'-62717766'
        0         b'63'      b'627A7863'  b'3330363136323633'      b'-627A7864'
    bitor
    teradataml.dataframe.sql.DataFrameColumn.bitor = bitor(bit_mask)
    DESCRIPTION:
        Function performs the logical OR operation on the binary representation of the
        integer or byte value in column and corresponding bits from input argument.
        Function takes two bit patterns of equal length and performs the logical OR
        operation on each pair of corresponding bits as follows:
            * The result is 1 if either of the two bits are 1.
            * The result is 0 if both the bits are 0.
        If the binary representation of the integer or byte value in column and "bit_mask"
        argument differ in length, the arguments are processed as follows:
            * The value in column and "bit_mask" arguments are aligned on their least
              significant byte/bit.
            * The smaller argument is padded with zeros to the left until it becomes
              the same size as the larger argument.
    ALTERNATE NAME:
        bit_or
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of a numeric or byte column or a numeric or byte
            literal value.
            Note:
                1. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types:
                ColumnExpression, int
                The data type of the "bit_mask" parameter varies depending upon the data type
                of the bits in column. The following table shows the supported column/literal
                value types for bits in column and "bit_mask" parameters and their permitted
                combinations:
                 ===================================================
                | bits in column type | "bit_mask" type             |
                 ===================================================
                | BYTEINT             | BYTE(1) or BYTEINT          |
                | SMALLINT            | BYTE(2) or SMALLINT         |
                | INTEGER             | BYTE(4) or INTEGER          |
                | BIGINT              | BYTE(8) or BIGINT           |
                | NUMBER(38,0)        | VARBYTE(16) or NUMBER(38,0) |
                | VARBYTE(n)          | VARBYTE(n)                  |
                 ===================================================
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Performs bitor operation on "id" and "byte_col" columns and
        #           and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col=df.id_col.bitor(df.byte_col))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col         col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  1627389954
        1         b'62'      b'62717765'  b'3331363136323633'  1644167169
        0         b'63'      b'627A7863'  b'3330363136323633'  1660944384
    bitwise_not
    teradataml.dataframe.sql.DataFrameColumn.bitwise_not = bitwise_not()
    DESCRIPTION:
        Function performs a bitwise complement on the binary representation of integer or byte
        value in column. The bitwise NOT, or complement, is a unary operation which performs
        logical negation on each bit, forming the ones' complement of the specified binary value.
        The digits in the argument which were 0 become 1, and vice versa.
    ALTERNATE NAMES:
        * bitnot
        * bitwise_not
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Performs a bitwise complement operation on "varbyte_col" column and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.varbyte_col.bitwise_not())
        >>> print(res_df)
               byte_col      varbyte_col             blob_col               col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  b'-616263643133'
        1         b'62'      b'62717765'  b'3331363136323633'      b'-62717766'
        0         b'63'      b'627A7863'  b'3330363136323633'      b'-627A7864'
    bitwiseNOT
    teradataml.dataframe.sql.DataFrameColumn.bitwiseNOT = bitwiseNOT()
    DESCRIPTION:
        Function performs a bitwise complement on the binary representation of integer or byte
        value in column. The bitwise NOT, or complement, is a unary operation which performs
        logical negation on each bit, forming the ones' complement of the specified binary value.
        The digits in the argument which were 0 become 1, and vice versa.
    ALTERNATE NAMES:
        * bitnot
        * bitwise_not
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Performs a bitwise complement operation on "varbyte_col" column and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.varbyte_col.bitwiseNOT())
        >>> print(res_df)
               byte_col      varbyte_col             blob_col               col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  b'-616263643133'
        1         b'62'      b'62717765'  b'3331363136323633'      b'-62717766'
        0         b'63'      b'627A7863'  b'3330363136323633'      b'-627A7864'
    bitxor
    teradataml.dataframe.sql.DataFrameColumn.bitxor = bitxor(bit_mask)
    DESCRIPTION:
        Function performs a bitwise XOR operation on the binary representation of the
        integer or byte column and corresponding bits from input argument.
        Function takes two bit patterns of equal length and performs the exclusive OR
        operation on each pair of corresponding bits as follows:
            * The result in each position is 1 if the two bits are different.
            * The result in each position is 0 if the two bits are same.
        If the binary representation of the integer or byte value in column and "bit_mask"
        argument differ in length, the arguments are processed as follows:
            * The value in column and "bit_mask" arguments are aligned on their least
              significant byte/bit.
            * The smaller argument is padded with zeros to the left until it becomes
              the same size as the larger argument.
    ALTERNATE NAME:
        bit_xor
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            Note:
                1. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types:
                ColumnExpression, int
                The data type of the "bit_mask" parameter varies depending upon the data type
                of the integer or byte column. The following table shows the supported column/literal
                value types for integer or byte column and "bit_mask" parameters and their permitted
                combinations:
                 ===============================================
                | value of column type | bit_mask type          |
                 ===============================================
                | BYTEINT              | BYTE(1) or BYTEINT     |
                | SMALLINT             | BYTE(2) or SMALLINT    |
                | INTEGER              | BYTE(4) or INTEGER     |
                | BIGINT               | BYTE(8) or BIGINT      |
                | VARBYTE(n)           | VARBYTE(n)             |
                 ===============================================
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Performs bitxor operation on "id" and "byte_col" columns and pass
        #           it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.id_col.bitxor(df.byte_col))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col         col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  1627389954
        1         b'62'      b'62717765'  b'3331363136323633'  1644167169
        0         b'63'      b'627A7863'  b'3330363136323633'  1660944384
    countset
    teradataml.dataframe.sql.DataFrameColumn.countset = countset(target_value)
    DESCRIPTION:
        Function returns the count of the binary bits within the integer or byte value column
        that are either set to 1 or set to 0, depending on the "target_value" value.
    PARAMETERS:
        target_value:
            Optional Argument.
            Specifies a ColumnExpression of an integer column or an integer literal representing
            a binary bit value, 0 or 1, to be counted within the integer or byte value column.
            Notes:
                1. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Default Value: 1
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Counts number of 0's within values of "varbyte_col" column.
        #           and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col= df.varbyte_col.countset(0))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col  col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'   29
        1         b'62'      b'62717765'  b'3331363136323633'   15
        0         b'63'      b'627A7863'  b'3330363136323633'   16
    from_byte
    teradataml.dataframe.sql.DataFrameColumn.from_byte = from_byte(self, encoding='base10')
    DESCRIPTION:
        The function encodes a sequence of bits into a sequence of characters.
        Note:
            * By default it converts a sequence of bits to 'base10', which is decimal.
    PARAMETERS:
        encoding:
            Optional Argument.
            Specifies encoding "from_byte" uses to encode the sequence of characters
            specified by column.
            The following encodings are supported:
                * BaseX
                * BaseY
                * Base64M (MIME)
                * ASCII
                where X is a power of 2 (for example, 2, 8, 16) and
                Y is not a power of 2 (for example, 10 and 36).
            Default Value: 'base10'
            Types: str
    Returns:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example 1: Converts values in "byte_col" to decimal and pass it as input to
        #           DataFrame.assign().
        >>> decimal_df = df.assign(decimal_col = df.byte_col.from_byte())
        >>> print(decimal_df)
                byte_col      varbyte_col             blob_col decimal_col
        id_col
        2         b'61'      b'627A7863'  b'6162636431323233...'  97
        1         b'62'  b'616263643132'  b'3331363136323633...'  98
        0         b'63'      b'62717765'  b'3330363136323633...'  99
        >>>
    getbit
    teradataml.dataframe.sql.DataFrameColumn.getbit = getbit(bit_mask)
    DESCRIPTION:
        Function returns the bit specified by "bit_mask" from the integer or byte value
        in column and returns either 0 or 1 to indicate the value of that bit.
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal.
            Notes:
                1. The range of input values can vary from 0 (bit 0 is the least significant
                   bit) to the (sizeof(integer or byte value in column) - 1).
                2. If "bit_mask" is negative or out-of-range (meaning that it exceeds the size
                   of integer or byte value in column), an error is returned.
                3. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Retrieves the fourth bit from the values in "varbyte_col" column
        #           and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col= df.varbyte_col.getbit(3))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col  col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'    0
        1         b'62'      b'62717765'  b'3331363136323633'    0
        0         b'63'      b'627A7863'  b'3330363136323633'    0
    rotateleft
    teradataml.dataframe.sql.DataFrameColumn.rotateleft = rotateleft(bit_mask)
    DESCRIPTION:
        Function returns an expression rotated to the left by the specified number of bits,
        with the most significant bits wrapping around to the right.
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the number of bit positions to rotate.
            Notes:
            1. If "bit_mask" is equal to zero, then the function returns value in column
               unchanged.
            2. If "bit_mask" is negative, then the function rotates the bits to the right
               instead of the left.
            3. If value in column and/or "bit_mask" are NULL, then the function returns NULL.
            4. If "bit_mask" is larger than the size of value in column, then the function rotates
               ("bit_mask" MOD sizeof(value in column)) bits. The scope of the rotation operation
               is bounded by the size of the bit value in column.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Rotates values in "varbyte_col" column to left by 3 bits and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col= df.varbyte_col.rotateleft(3))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col             col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  b'B131B218993'
        1         b'62'      b'62717765'  b'3331363136323633'     b'138BBB2B'
        0         b'63'      b'627A7863'  b'3330363136323633'     b'13D3C31B'
    rotateright
    teradataml.dataframe.sql.DataFrameColumn.rotateright = rotateright(bit_mask)
    DESCRIPTION:
        Function returns an expression rotated to the right by the specified number of bits,
        with the most significant bits wrapping around to the right.
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the number of bit positions to rotate.
            Notes:
            1. If "bit_mask" is equal to zero, then the function returns value in column
               unchanged.
            2. If "bit_mask" is negative, then the function rotates the bits to the left
               instead of the right.
            3. If value in column and/or "bit_mask" are NULL, then the function returns NULL.
            4. If "bit_mask" is larger than the size of value in column, then the function rotates
               ("bit_mask" MOD sizeof(value in column)) bits. The scope of the rotation operation
               is bounded by the size of the bit value in column.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Rotates values in "varbyte_col" column to right by 3 bits and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col= df.varbyte_col.rotateright(3))
        >>> print(res_df)
               byte_col      varbyte_col                blob_col              col
        id_col
        2         b'61'      b'627A7863'     b'6162636431323233'      b'6C4F4F0C'
        1         b'62'  b'616263643132'     b'3331363136323633'  b'4C2C4C6C8626'
        0         b'63'      b'62717765'     b'3330363136323633'     b'-53B1D114'
    setbit
    teradataml.dataframe.sql.DataFrameColumn.setbit = setbit(target_bit, target_value)
    DESCRIPTION:
        Function sets the value of the bit specified by "target_bit" to the value
        of "target_value" in the integer or byte value of column.
    PARAMETERS:
        target_bit:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal.
            Notes:
                1. The range of input values can vary from 0 (bit 0 is the least significant
                   bit) to the (sizeof(byte value of column) - 1).
                2. If "target_bit" is negative or out-of-range (meaning that it exceeds the size
                   of byte value of column), an error is returned.
                3. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
        target_value:
            Optional Argument.
            Specifies a ColumnExpression of an integer column or an integer literal representing
            a binary bit value, 0 or 1, to be set within the byte value of column.
            Note:
                1. If the argument is NULL, the function returns NULL.
            Default Value: 1
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Set the fourth bit from the values in "varbyte_col" column with "1" and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col= df.varbyte_col.setbit(3))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col              col
        id_col
        0         b'63'      b'627A7863'  b'3330363136323633'      b'627A786B'
        2         b'61'  b'616263643132'  b'6162636431323233'  b'61626364313A'
        1         b'62'      b'62717765'  b'3331363136323633'      b'6271776D'
    shiftleft
    teradataml.dataframe.sql.DataFrameColumn.shiftleft = shiftleft(bit_mask)
    DESCRIPTION:
        Function returns the integer or byte value of column shifted by the specified
        number of bits ("bit_mask") to the left. The bits in the most significant
        positions are lost, and the bits in the least significant positions are
        filled with zeros.
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the number of bit positions to shift.
            Notes:
            1. If "bit_mask" is equal to zero, then the function returns integer or byte
               value of column unchanged.
            2. If "bit_mask" is negative, then the function shifts the bits to the right
               instead of the left.
            3. If integer or byte value of column and/or "bit_mask" are NULL, then the
               function returns NULL.
            4. The scope of the shift operation is bounded by the size of the integer or byte
               value of column. Specifying a shift that is outside the range of integer or byte
               value of column results in an error.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Shifts values in "varbyte_col" column to left by 3 bits and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col= df.varbyte_col.shiftleft(3))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col             col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  b'B131B218990'
        1         b'62'      b'62717765'  b'3331363136323633'     b'138BBB28'
        0         b'63'      b'627A7863'  b'3330363136323633'     b'13D3C318'
    shiftright
    teradataml.dataframe.sql.DataFrameColumn.shiftright = shiftright(bit_mask)
    DESCRIPTION:
        Function returns the integer or byte value of column shifted by the specified
        number of bits ("bit_mask") to the right. The bits in the most significant
        positions are lost, and the bits in the least significant positions are
        filled with zeros.
    PARAMETERS:
        bit_mask:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the number of bit positions to shift.
            Notes:
            1. If "bit_mask" is equal to zero, then the function returns integer or byte
               value of column unchanged.
            2. If "bit_mask" is negative, then the function shifts the bits to the right
               instead of the right.
            3. If integer or byte value of column and/or "bit_mask" are NULL, then the
               function returns NULL.
            4. The scope of the shift operation is bounded by the size of the integer or byte
               value of column. Specifying a shift that is outside the range of integer or byte
               value of column results in an error.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Shifts values in "varbyte_col" column to right by 3 bits and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col= df.varbyte_col.shiftright(3))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col             col
        id_col
        0         b'63'      b'627A7863'  b'3330363136323633'      b'C4F4F0C'
        2         b'61'  b'616263643132'  b'6162636431323233'  b'C2C4C6C8626'
        1         b'62'      b'62717765'  b'3331363136323633'      b'C4E2EEC'
    subbitstr
    teradataml.dataframe.sql.DataFrameColumn.subbitstr = subbitstr(position, num_bits)
    DESCRIPTION:
        Function extracts a bit substring from the integer or byte column
        based on the specified bit position. Function returns a VARBYTE string,
        thus resulting number of bits returned are rounded to the byte boundary
        greater than the number of bits requested.
        The bits returned are right-justified, and the excess bits (those exceeding
        the requested number of bits) are filled with zeros.
    PARAMETERS:
        position:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the starting position of the bit substring to be extracted.
            Notes:
                1. If "position" is negative or out-of-range (meaning that it exceeds
                   the size of integer or byte column), an error is returned.
                2. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
        num_bits:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the length of the bit substring to be extracted. This
            specifies the number of bits for the function to return.
            Notes:
                1. If "num_bits" is negative, or is greater than the number of bits
                   remaining after the starting position is taken into account, an error
                   is returned.
                2. If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>'.
            Types: ColumnExpression, int
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example1: Requests that 3 bits be returned starting at the third bit
        #           from values in "varbyte_col" column and pass it as input to
        #           DataFrame.assign().
        >>> res_df = df.assign(col= df.varbyte_col.subbitstr(2, 3))
        >>> print(res_df)
               byte_col      varbyte_col             blob_col  col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  b'4'
        1         b'62'      b'62717765'  b'3331363136323633'  b'1'
        0         b'63'      b'627A7863'  b'3330363136323633'  b'0'
    to_byte
    teradataml.dataframe.sql.DataFrameColumn.to_byte = to_byte(self, encoding='base10')
    DESCRIPTION:
        The function decodes a sequence of characters in a given
        encoding into a sequence of bits.
        Note:
            * By default, consider DataFrame column as 'base10' and encodes
              into a sequence of bits.
    PARAMETERS:
        encoding:
            Optional Argument.
            Specifies encoding "to_byte" uses to return the sequence of characters
            specified by column.
            The following encodings are supported:
                * BaseX
                * BaseY
                * Base64M (MIME)
                * ASCII
                where X is a power of 2 (for example, 2, 8, 16) and
                Y is not a power of 2 (for example, 10 and 36).
            Default Value: 'base10'
            Types: str
    Returns:
        ColumnExpression.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        # Create a DataFrame on 'bytes_table' table.
        >>> df = DataFrame("bytes_table")
        >>> print(df)
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        # Example 1: Converts values in "id_col" to bytes and pass it as input to
        #           DataFrame.assign().
        >>> byte_df = df.assign(byte_col = df.id_col.to_byte())
        >>> print(byte_df)
                byte_col      varbyte_col             blob_col   byte_col
        id_col
        2         b'61'      b'627A7863'  b'6162636431323233...'  b'2'
        1         b'62'  b'616263643132'  b'3331363136323633...'  b'1'
        0         b'63'      b'62717765'  b'3330363136323633...'   b''
        >>>
    DataFrameColumn Regular Expression Functions
    regexp_instr
    teradataml.dataframe.sql.DataFrameColumn.regexp_instr = regexp_instr(regexp_string, position, occurence, return_opt, match)
    DESCRIPTION:
        Function searches for the string value in column for a match to "regexp_string".
    PARAMETERS:
        regexp_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal which
            is to be used as regex.
            Note:
                1. If regexp_string is NULL, NULL is returned.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR
            Types: ColumnExpression, str
        position:
            Optional Argument.
            Specifies the position in string value in column from which to start searching.
            Notes:
                1. If the value is greater than the input string length, 0 is returned.
                2. If the value is NULL, the value NULL is returned.
            Types: ColumnExpression, int
            Default Value: 1
        occurence:
            Optional Argument.
            Specifies the number of the occurrence to be returned.
            Notes:
                1. If the value is greater than the number of matches found, 0 is returned.
                2. If the value is NULL, a NULL result is returned.
            Types: ColumnExpression, int
            Default Value: 1
        return_opt:
            Optional Argument.
            Specifies a numeric value 0 or 1 that decides return value for the match.
            Note:
                1. If the value is NULL, NULL is returned.
            Permitted Values:
                * 0 = function returns the beginning position of the match (default).
                * 1 = function returns the end position (character following the occurrence) of the match.
            Types: ColumnExpression, int
            Default Value: 0
        match:
            Optional Argument.
            Specifies a character which decides the handling of regex matching.
            Notes:
                1. If a character in the argument is not valid, then that error is reported.
                2. If match_arg is not specified, is NULL, or is empty:
                    a. The match is case-sensitive.
                    b. A period does not match the newline character.
                    c. string value in column is treated as a single line.
                3. The argument can contain more than one character.
            Permitted Values:
                * 'i' - case-insensitive matching.
                * 'c' - case sensitive matching.
                * 'n' - the period character (match any character) can match the newline character.
                * 'm' - string value in column is treated as multiple lines instead of as a single line.
                        With this option, the '^' and '$' characters apply to each line in string value in column
                        instead of the entire string value in column.
                * 'l' - if string value in column exceeds the current maximum allowed string value in column size
                        (currently 16 MB), a NULL is returned instead of an error. This is useful for
                        long-running queries where you do not want long strings causing an error that
                        would make the query fail.
                * 'x' - ignore whitespace.
            Types: str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train")
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
        # Example1: Searches for "d" substring in "stats" column and returns the
        #           position of the matched string based on integer value in
        #           "admitted" column and pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col = df.stats.regexp_instr("d", 1, 1, df.admitted, 'c'))
        >>> print(res_df)
           masters   gpa     stats programming  admitted  col
        id
        13      no  4.00  Advanced      Novice         1    3
        26     yes  3.57  Advanced    Advanced         1    3
        5       no  3.44    Novice      Novice         0    0
        19     yes  1.98  Advanced    Advanced         0    2
        15     yes  4.00  Advanced    Advanced         1    3
        40     yes  3.95    Novice    Beginner         0    0
        7      yes  2.33    Novice      Novice         1    0
        22     yes  3.46    Novice    Beginner         0    0
        36      no  3.00  Advanced      Novice         0    2
        38     yes  2.65  Advanced    Beginner         1    3
    regexp_replace
    teradataml.dataframe.sql.DataFrameColumn.regexp_replace = regexp_replace(regexp_string, replace_string, position, occurrence, match)
    DESCRIPTION:
        Function replaces portions of string value in column, that match "regexp_string" with
        the "replace_string".
    PARAMETERS:
        regexp_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to use for regex matching.
            Note:
                1. If regexp_string is NULL, NULL is returned.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR
            Types: ColumnExpression, str
        replace_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to use as replacement.
            Note:
                1. If a replace_string is not specified, is NULL or is an empty string,
                   the matches are removed from the result.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR
            Types: ColumnExpression, str
        position:
            Optional Argument.
            Specifies the position in string value in column from which to start searching.
            Note:
                1. If the value greater than the input string length, NULL is returned.
                2. If the value is NULL, the value NULL is returned.
            Types: ColumnExpression, int
            Default Value: 1
        occurrence:
            Optional Argument.
            Specifies the occurrence to replace the match with replace_string.
            Notes:
                1. If a value 0 is specified, all occurrences are replaced.
                2. If the value is greater than 1, the search begins for the second
                   occurrence beginning with the first character following the first
                   occurrence of the regexp_string, and so on.
                3. If occurrence_arg is greater than the number of matches found, nothing
                   is replaced and string value in column is returned.
                4. If occurrence_arg is NULL, a NULL result is returned.
            Types: ColumnExpression, int
            Default Value: 0
        match:
            Optional Argument.
            Specifies a character which decides the handling of regex matching.
            Notes:
                1. If a character in the argument is not valid, then that character is ignored.
                2. If match_arg is not specified, is NULL, or is empty:
                    a. The match is case-sensitive.
                    b. A period does not match the newline character.
                    c. string value in column is treated as a single line.
                3. The argument can contain more than one character.
            Permitted values:
                * 'i' - case-insensitive matching.
                * 'c' - case sensitive matching.
                * 'n' - the period character (match any character) can match the newline character.
                * 'm' - string value in column is treated as multiple lines instead of as a single line.
                        With this option, the '^' and '$' characters apply to each line in string value in column
                        instead of the entire string value in column.
                * 'l' - if string value in column exceeds the current maximum allowed size
                        (currently 16 MB), a NULL is returned instead of an error. This is useful for
                        long-running queries where you do not want long strings causing an error that
                        would make the query fail.
                * 'x' - ignore whitespace.
            Types: str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train")
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
        # Example1: Searches for "vice" substring from "stats" column and replaces
        #           the same with "w You See Me" and pass it as input to
        #           DataFrame.assign().
        >>> res_df = df.assign(col=df.stats.regexp_replace("vice", "w You See Me", 1, 1, 'c'))
        >>> print(res_df)
           masters   gpa     stats programming  admitted             col
        id
        5       no  3.44    Novice      Novice         0  Now You See Me
        34     yes  3.85  Advanced    Beginner         0        Advanced
        13      no  4.00  Advanced      Novice         1        Advanced
        40     yes  3.95    Novice    Beginner         0  Now You See Me
        22     yes  3.46    Novice    Beginner         0  Now You See Me
        19     yes  1.98  Advanced    Advanced         0        Advanced
        36      no  3.00  Advanced      Novice         0        Advanced
        15     yes  4.00  Advanced    Advanced         1        Advanced
        7      yes  2.33    Novice      Novice         1  Now You See Me
        17      no  3.83  Advanced    Advanced         1        Advanced
    regexp_similar
    teradataml.dataframe.sql.DataFrameColumn.regexp_similar = regexp_similar(regexp_string, match)
    DESCRIPTION:
        Function compares string value in column to "regexp_string" and returns integer value.
    PARAMETERS:
        regexp_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            which is to be used as regex.
            Note:
                1. If regexp_string is NULL, NULL is returned.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR
            Types: ColumnExpression, str
        match:
            Optional Argument.
            Specifies a character which decides the handling of regex matching.
            Notes:
                1. If a character in the argument is not valid, then that character is ignored.
                2. If match_arg is not specified, is NULL, or is empty:
                    a. The match is case-sensitive.
                    b. A period does not match the newline character.
                    c. string value in column is treated as a single line.
                3. The argument can contain more than one character.
            Permitted values:
                * 'i' - case-insensitive matching.
                * 'c' - case sensitive matching.
                * 'n' - the period character (match any character) can match the newline character.
                * 'm' - string value in column is treated as multiple lines instead of as a single line.
                        With this option, the '^' and '$' characters apply to each line in string value in column
                        instead of the entire string value in column.
                * 'l' - if string value in column exceeds the current maximum allowed size
                        (currently 16 MB), a NULL is returned instead of an error. This is useful for
                        long-running queries where you do not want long strings causing an error that
                        would make the query fail.
                * 'x' - ignore whitespace.
            Types: str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        Function returns following integer values in a DataFrameColumn:
            * 1 (true) if the entire string value in column matches regexp_string.
            * 0 (false) if the entire string value in column does not match regexp_string.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train")
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
        # Example1: Compares strings in "stats" column and "programming" column and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col=df.stats.regexp_similar(df.programming, 'c'))
        >>> print(res_df)
           masters   gpa     stats programming  admitted  col
        id
        5       no  3.44    Novice      Novice         0    1
        34     yes  3.85  Advanced    Beginner         0    0
        13      no  4.00  Advanced      Novice         1    0
        40     yes  3.95    Novice    Beginner         0    0
        22     yes  3.46    Novice    Beginner         0    0
        19     yes  1.98  Advanced    Advanced         0    1
        36      no  3.00  Advanced      Novice         0    0
        15     yes  4.00  Advanced    Advanced         1    1
        7      yes  2.33    Novice      Novice         1    1
        17      no  3.83  Advanced    Advanced         1    1
    regexp_substr
    teradataml.dataframe.sql.DataFrameColumn.regexp_substr = regexp_substr(regexp_string, position, occurrence, match)
    DESCRIPTION:
        Function extracts a substring from string value in column, that matches a regular
        expression specified by regexp_string.
    PARAMETERS:
        regexp_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            which is to be used as regex.
            Note:
                1. If regexp_string is NULL, NULL is returned.
            Format for the argument: '<dataframe>.<dataframe_column>'.
            Supported column types: CHAR, VARCHAR
            Types: ColumnExpression, str
        position:
            Optional Argument.
            Specifies the position in string value in column from which to start searching.
            Notes:
                1. If the value is greater than the input string length, 0 is returned.
                2. If the value is NULL, the value NULL is returned.
            Types: ColumnExpression, int
            Default Value: 1
        occurrence:
            Optional Argument.
            Specifies the number of the occurrence to be returned.
            Notes:
                1. If the value is greater than the number of matches found, 0 is returned.
                2. If the value is NULL, a NULL result is returned.
            Types: ColumnExpression, int
            Default Value: 1
        match:
            Optional Argument.
            Specifies a character which decides the handling of regex matching.
            Notes:
                1. If a character in the argument is not valid, then that character is ignored.
                2. If match_arg is not specified, is NULL, or is empty:
                    a. The match is case-sensitive.
                    b. A period does not match the newline character.
                    c. string value in column is treated as a single line.
                3. The argument can contain more than one character.
            Permitted values:
                * 'i' - case-insensitive matching.
                * 'c' - case sensitive matching.
                * 'n' - the period character (match any character) can match the newline character.
                * 'm' - string value in column is treated as multiple lines instead of as a single line.
                        With this option, the '^' and '$' characters apply to each line in string value in column
                        instead of the entire string value in column.
                * 'l' - if string value in column exceeds the current maximum allowed size
                        (currently 16 MB), a NULL is returned instead of an error. This is useful for
                        long-running queries where you do not want long strings causing an error that
                        would make the query fail.
                * 'x' - ignore whitespace.
            Types: str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame on 'admissions_train' table.
        >>> df = DataFrame("admissions_train")
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
        # Example1: Extracts "no" substring from "stats" column and
        #           pass it as input to DataFrame.assign().
        >>> res_df = df.assign(col=df.stats.regexp_substr("no", 1, 1, 'i'))
        >>> print(res_df)
           masters   gpa     stats programming  admitted   col
        id
        5       no  3.44    Novice      Novice         0    No
        34     yes  3.85  Advanced    Beginner         0  None
        13      no  4.00  Advanced      Novice         1  None
        40     yes  3.95    Novice    Beginner         0    No
        22     yes  3.46    Novice    Beginner         0    No
        19     yes  1.98  Advanced    Advanced         0  None
        36      no  3.00  Advanced      Novice         0  None
        15     yes  4.00  Advanced    Advanced         1  None
        7      yes  2.33    Novice      Novice         1    No
        17      no  3.83  Advanced    Advanced         1  None
    DataFrameColumn Date and Time Functions
    add_months
    teradataml.dataframe.sql.DataFrameColumn.add_months = add_months()
    DESCRIPTION:
        Function adds an integer number of months to specified date or timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                           (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Adds an integer number of months to specified date or timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.add_months(2))
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates         res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  2018-05-20
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  2019-04-25
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  2017-06-27
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  2019-07-29
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  2018-12-22
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  2017-10-07
    day_occurrence_of_month
    teradataml.dataframe.sql.DataFrameColumn.day_occurrence_of_month = day_occurrence_of_month()
    DESCRIPTION:
        Function returns the nth occurrence of the weekday in the month for the date value in column.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the nth occurrence of the weekday in the month for the date value in column.
        >>> df = df_sales.assign(res = df_sales.dates.day_occurrence_of_month())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20    3
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25    4
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27    4
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29    5
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22    4
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07    1
    day_of_calendar
    teradataml.dataframe.sql.DataFrameColumn.day_of_calendar = day_of_calendar()
    DESCRIPTION:
        Function returns the number of days from the beginning of the business calendar to the specified date or
        timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of days from the beginning of the business calendar to the specified
        #            date or timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.day_of_calendar())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates    res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  43178
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  43520
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  42851
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  43613
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  43394
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  42953
    day_of_month
    teradataml.dataframe.sql.DataFrameColumn.day_of_month = day_of_month()
    DESCRIPTION:
        Function returns the number of days from the beginning of the month to the specified date or timestamp value in a
        column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of days from the beginning of the month to the specified date or timestamp
        #            value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.day_of_month())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20   20
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25   25
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27   27
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29   29
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22   22
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07    7
    day_of_week
    teradataml.dataframe.sql.DataFrameColumn.day_of_week = day_of_week()
    DESCRIPTION:
        Function returns the number of days from the beginning of the week to the specified date or timestamp value in a
        column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of days from the beginning of the week to the specified date or timestamp
        #            value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.day_of_week())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20    3
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25    2
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27    5
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29    4
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22    2
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07    2
    day_of_year
    teradataml.dataframe.sql.DataFrameColumn.day_of_year = day_of_year()
    DESCRIPTION:
        Function returns the number of days from the beginning of the year to the specified date or timestamp value in a
        column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of days from the beginning of the year to the specified date or timestamp
        #            value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.day_of_year())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25   56
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  219
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  295
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  117
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  149
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20   79
    extract
    teradataml.dataframe.sql.DataFrameColumn.extract = extract(self, value, timezone=None)
    DESCRIPTION:
        Extracts a single specified field from any DateTime, Interval or timestamp value,
        converting it to an exact numeric value.
    PARAMETERS:
        value:
            Required Argument.
            Specifies the field which needs to be extracted.
            Permitted Values: YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, TIMEZONE_HOUR, TIMEZONE_MINUTE
            Note:
                * Permitted Values are case insensitive.
            Type: str
        timezone:
            Optional Argument.
            Specifies the timezone string.
            For valid timezone strings, user should check Vantage documentation.
            Type: ColumnExpression or str.
    RETURNS:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("uaf", "Traindata")
        # Create a DataFrame on 'Traindata' table.
        >>> temp_df = DataFrame("Traindata")
        >>> df = temp_df.select(["seq_no", "schedule_date", "arrivalTime"])
        >>> df
                schedule_date          arrivalTime
        seq_no
        26          16/03/26  2016-03-26 12:33:05
        24          16/03/26  2016-03-26 12:25:06
        3           16/03/26  2016-03-26 10:52:05
        22          16/03/26  2016-03-26 12:18:01
        20          16/03/26  2016-03-26 12:10:06
        18          16/03/26  2016-03-26 12:04:01
        8           16/03/26  2016-03-26 11:15:06
        17          16/03/26  2016-03-26 11:56:06
        15          16/03/26  2016-03-26 11:45:00
        13          16/03/26  2016-03-26 11:33:00
        11          16/03/26  2016-03-26 11:26:00
        # Example 1: Extract year from column 'schedule_date'.
        >>> df.assign(col = df.schedule_date.extract('YEAR'))
                schedule_date          arrivalTime   col
        seq_no
        26          16/03/26  2016-03-26 12:33:05  2016
        24          16/03/26  2016-03-26 12:25:06  2016
        3           16/03/26  2016-03-26 10:52:05  2016
        22          16/03/26  2016-03-26 12:18:01  2016
        20          16/03/26  2016-03-26 12:10:06  2016
        18          16/03/26  2016-03-26 12:04:01  2016
        8           16/03/26  2016-03-26 11:15:06  2016
        17          16/03/26  2016-03-26 11:56:06  2016
        15          16/03/26  2016-03-26 11:45:00  2016
        13          16/03/26  2016-03-26 11:33:00  2016
        11          16/03/26  2016-03-26 11:26:00  2016
        # Example 2: Extract hour from column 'arrivalTime'.
        >>> df.assign(col = df.arrivalTime.extract('HOUR'))
                schedule_date          arrivalTime col
        seq_no
        26          16/03/26  2016-03-26 12:33:05  12
        24          16/03/26  2016-03-26 12:25:06  12
        3           16/03/26  2016-03-26 10:52:05  10
        22          16/03/26  2016-03-26 12:18:01  12
        20          16/03/26  2016-03-26 12:10:06  12
        18          16/03/26  2016-03-26 12:04:01  12
        8           16/03/26  2016-03-26 11:15:06  11
        17          16/03/26  2016-03-26 11:56:06  11
        15          16/03/26  2016-03-26 11:45:00  11
        # Example 3: Extract hour from column 'arrivalTime' with offset '-11:00'.
        >>> df.assign(col = df.arrivalTime.extract('HOUR', '-11:00'))
                schedule_date          arrivalTime col
        seq_no
        26          16/03/26  2016-03-26 12:33:05   1
        24          16/03/26  2016-03-26 12:25:06   1
        3           16/03/26  2016-03-26 10:52:05  23
        22          16/03/26  2016-03-26 12:18:01   1
        20          16/03/26  2016-03-26 12:10:06   1
        18          16/03/26  2016-03-26 12:04:01   1
        8           16/03/26  2016-03-26 11:15:06   0
        17          16/03/26  2016-03-26 11:56:06   0
        15          16/03/26  2016-03-26 11:45:00   0
        # Example 4: Extract hour from column 'arrivalTime' with offset 10.
        >>> df.assign(col = df.arrivalTime.extract('HOUR', 10))
                schedule_date          arrivalTime col
        seq_no
        26          16/03/26  2016-03-26 12:33:05  22
        24          16/03/26  2016-03-26 12:25:06  22
        3           16/03/26  2016-03-26 10:52:05  20
        22          16/03/26  2016-03-26 12:18:01  22
        20          16/03/26  2016-03-26 12:10:06  22
        18          16/03/26  2016-03-26 12:04:01  22
        8           16/03/26  2016-03-26 11:15:06  21
        17          16/03/26  2016-03-26 11:56:06  21
        15          16/03/26  2016-03-26 11:45:00  21
        13          16/03/26  2016-03-26 11:33:00  21
        11          16/03/26  2016-03-26 11:26:00  21
    hour
    teradataml.dataframe.sql.DataFrameColumn.hour = hour()
    DESCRIPTION:
        Function returns the number of weeks from the beginning of the year to the specified date or timestamp value in
        a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["ocean_buoys"])
        # Create a DataFrame on 'ocean_buoys' table.
        >>> ocean_df = DataFrame.from_table('ocean_buoys')
        # Example 1: Returns the integer value for hour in the specified timestamp value in a column as a literal.
        >>> res_df = ocean_df.assign(res = ocean_df.TD_TIMECODE.hour())
        >>> print(res_df)
                               TD_TIMECODE  salinity  temperature  res
        buoyid
        0       2014-01-06 08:09:59.999999        55           99    8
        0       2014-01-06 08:10:00.000000        55          100    8
        44      2014-01-06 10:00:24.000000        55           43   10
        1       2014-01-06 09:01:25.122200        55           77    9
        1       2014-01-06 09:02:25.122200        55           78    9
        1       2014-01-06 09:02:25.122200        55           71    9
        1       2014-01-06 09:03:25.122200        55           79    9
        1       2014-01-06 09:03:25.122200        55           72    9
        1       2014-01-06 09:01:25.122200        55           70    9
        0       2014-01-06 08:10:00.000000        55           10    8
    last_friday
    teradataml.dataframe.sql.DataFrameColumn.last_friday = last_friday()
    DESCRIPTION:
        Function returns the date or timestamp of Friday that falls immediately before the specified date or timestamp
        value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the date or timestamp of Friday that falls immediately before the specified date or
        #            timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.last_friday())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/16
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/22
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/21
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/24
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/19
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/04
    last_monday
    teradataml.dataframe.sql.DataFrameColumn.last_monday = last_monday()
    DESCRIPTION:
        Function returns the date or timestamp of Monday that falls immediately before the specified date or timestamp
        value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the date or timestamp of Monday that falls immediately before the specified date or
        #            timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.last_monday())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/25
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/07
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/22
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/24
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/27
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/19
    last_saturday
    teradataml.dataframe.sql.DataFrameColumn.last_saturday = last_saturday()
    DESCRIPTION:
        Function returns the date or timestamp of Saturday that falls immediately before the specified date or timestamp
        value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the date or timestamp of Saturday that falls immediately before the specified date or
        #            timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.last_saturday())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/23
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/05
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/20
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/22
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/25
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/17
    last_sunday
    teradataml.dataframe.sql.DataFrameColumn.last_sunday = last_sunday()
    DESCRIPTION:
        Function returns the date or timestamp of Sunday that falls immediately before the specified date or timestamp
        value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the date or timestamp of Sunday that falls immediately before the specified date or
        #            timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.last_sunday())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/24
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/06
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/21
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/23
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/26
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/18
    last_thursday
    teradataml.dataframe.sql.DataFrameColumn.last_thursday = last_thursday()
    DESCRIPTION:
        Function returns the date or timestamp of Thursday that falls immediately before the specified date or timestamp
        value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the date or timestamp of Thursday that falls immediately before the specified date or
        #            timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.last_thursday())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/21
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/03
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/18
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/27
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/23
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/15
    last_tuesday
    teradataml.dataframe.sql.DataFrameColumn.last_tuesday = last_tuesday()
    DESCRIPTION:
        Function returns the date or timestamp of Tuesday that falls immediately before the specified date or timestamp
        value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the date or timestamp of Tuesday that falls immediately before the specified date or
        #            timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.last_tuesday())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/19
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/01
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/16
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/25
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/28
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/20
    last_wednesday
    teradataml.dataframe.sql.DataFrameColumn.last_wednesday = last_wednesday()
    DESCRIPTION:
        Function returns the date or timestamp of Wednesday that falls immediately before the specified date or timestamp
        value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the date or timestamp of Wednesday that falls immediately before the specified date or
        #            timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.last_wednesday())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/14
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/20
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/26
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/29
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/17
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/02
    minute
    teradataml.dataframe.sql.DataFrameColumn.minute = minute()
    DESCRIPTION:
        Function returns the integer value for minute in the specified timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["ocean_buoys"])
        # Create a DataFrame on 'ocean_buoys' table.
        >>> ocean_df = DataFrame.from_table('ocean_buoys')
        # Example 1: Returns the integer value for minute in the specified timestamp value in a column as a literal.
        >>> res_df = ocean_df.assign(res = ocean_df.TD_TIMECODE.minute())
        >>> print(res_df)
                               TD_TIMECODE  salinity  temperature  res
        buoyid
        1       2014-01-06 09:02:25.122200        55         78.0    2
        1       2014-01-06 09:03:25.122200        55         79.0    3
        1       2014-01-06 09:03:25.122200        55         72.0    3
        0       2014-01-06 08:00:00.000000        55         10.0    0
        0       2014-01-06 08:09:59.999999        55         99.0    9
        0       2014-01-06 08:10:00.000000        55         10.0   10
        0       2014-01-06 08:10:00.000000        55        100.0   10
        44      2014-01-06 10:00:24.000000        55         43.0    0
        0       2014-01-06 08:08:59.999999        55          NaN    8
        1       2014-01-06 09:02:25.122200        55         71.0    2
    month
    teradataml.dataframe.sql.DataFrameColumn.month = month()
    DESCRIPTION:
        Function returns the integer value for months in the specified date or timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the integer value for months in the specified date or timestamp value in a column as a
        #            literal.
        >>> df = df_sales.assign(res = df_sales.dates.month())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25    2
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07    8
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22   10
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27    4
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29    5
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20    3
    month_end
    teradataml.dataframe.sql.DataFrameColumn.month_end = month_end()
    DESCRIPTION:
        Function returns the last date or timestamp of the month that begins immediately before the specified date or
        timestamp value in a column or as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the last date or timestamp of the month that begins immediately before the specified date or
        #           timestamp value in a column or as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.month_end())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/28
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/31
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/31
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/30
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/31
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/31
    month_of_calendar
    teradataml.dataframe.sql.DataFrameColumn.month_of_calendar = month_of_calendar()
    DESCRIPTION:
        Function returns the number of months from the beginning of the business calendar to the specified date or
        timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of months from the beginning of the business calendar to the specified
        #            date or timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.month_of_calendar())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates   res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  1430
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  1412
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  1426
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  1408
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  1433
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  1419
    month_of_quarter
    teradataml.dataframe.sql.DataFrameColumn.month_of_quarter = month_of_quarter()
    DESCRIPTION:
        Function returns the number of months from the beginning of the quarter to the specified date or timestamp value
        in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of months from the beginning of the quarter to the specified date or timestamp
        #            value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.month_of_quarter())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20    3
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25    2
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27    1
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29    2
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22    1
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07    2
    month_of_year
    teradataml.dataframe.sql.DataFrameColumn.month_of_year = month_of_year()
    DESCRIPTION:
        Function returns the number of months from the beginning of the year to the specified date or timestamp value in
        a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of months from the beginning of the year to the specified date or timestamp
        #            value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.month_of_year())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25    2
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07    8
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22   10
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27    4
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29    5
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20    3
    month_start
    teradataml.dataframe.sql.DataFrameColumn.month_start = month_start()
    DESCRIPTION:
        Function returns the first date or timestamp of the month that begins immediately before the specified date or
        timestamp value in a column or as a literal.
    ALTERNATE NAME:
        month_begin
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the first date or timestamp of the month that begins immediately before the specified date or
        #           timestamp value in a column or as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.month_start())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/01
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/01
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/01
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/01
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/01
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/01
    months_between
    teradataml.dataframe.sql.DataFrameColumn.months_between = months_between(self, expression)
    DESCRIPTION:
        Function returns the number of months between value in specified date or timestamp value in a column as a
        literal and date or timestamp value in argument.
        It returns an integer value only when the two dates are same else it returns the fractional portion of the
        result based on a 31-day month.
    PARAMETERS:
        expression:
            Optional Argument.
            Specifies a date or timestamp or timestamp with time zone value.
            Types: DataFrameColumn
            Notes:
                1. If expression is NULL, NULL is returned.
                2. Both date_timestamp_value1 and date_timestamp_value2 should be of the same type.
                3. Since TIMESTAMP values are stored in UTC time within the database and lack a time zone, the session
                   time zone is used to interpret the time stamp value within the function.
                   For TIMESTAMP WITH TIME ZONE values, the time zone component is used to interpret the time stamp
                   value within the function.
                4. If expression is:
                        Later than DataFrameColumn value, the result is negative.
                        Earlier than DataFrameColumn value, the result is positive.
                5. If DataFrameColumn value and expression are either the same days of the month or both last days of
                   months, the result is always an integer. Otherwise, MONTHS_BETWEEN calculates the fractional portion
                   of the result based on a 31-day month and considers the difference in time components of
                   DataFrameColumn value and expression.
                6. The Gregorian calendar is supported.
                7. Timestamps without a time zone are compared in the local time zone. This matters solely for the
                   purpose of determining if two timestamps reside on the same day or are both the last day of a month.
                   Timestamps with a time zone are converted to UTC before being compared in order to ensure the times
                   are in the same zone.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of months between value in specified date or timestamp value in a column as a
        #            literal and date or timestamp value in argument. It returns an integer value only when the two
        #            dates are same else it returns the fractional portion of the result based on a 31-day month
        >>> df = df_sales.assign(res = df_sales.dates.months_between(df_sales.datetime))
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates        res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  14.516129
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  25.677419
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27   3.741935
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  28.806452
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  21.580645
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07   7.096774
    next_day
    teradataml.dataframe.sql.DataFrameColumn.next_day = next_day(self, day_value)
    DESCRIPTION:
        Function returns the date of the first weekday specified as 'day_value' that is later than the specified date or
        timestamp value in a column as a literal.
    PARAMETERS:
        day_value:
            Optional Argument.
            Specifies the day of the week.
            day_value can be any day of the week or its 3-character abbreviation.
            Permitted Values:
                'SUNDAY' or 'SUN'
                'MONDAY' or 'MON'
                'TUESDAY' or 'TUE'
                'WEDNESDAY' or 'WED'
                'THURSDAY' or 'THU'
                'FRIDAY' or 'FRI'
                'SATURDAY' or 'SAT'
            Types: str
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the date of the first weekday specified as 'day_value' that is later than the specified
        #            date or timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.next_day("SUNDAY"))
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/25
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/03/03
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/30
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/06/02
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/28
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/13
    oadd_months
    teradataml.dataframe.sql.DataFrameColumn.oadd_months = oadd_months(self, expression)
    DESCRIPTION:
        Function adds an integer number of months, date or timestamp value in specified date or timestamp value in a
        column as a literal.
    PARAMETERS:
        expression:
            Optional Argument.
            Specifies a date or timestamp value. If date_timestamp_value is NULL, NULL is returned.
            Types: DataFrameColumn
            Note:
                1. Since TIMESTAMP values are stored in UTC time within the database and lack a time zone, the session
                   time zone is used to interpret the time stamp value within the function. For TIMESTAMP WITH TIME ZONE
                   values, the time zone component is used to interpret the time stamp value within the function.
                2. If the day component of expression is the last day of the month, or if the resulting month
                   has fewer days than the day component of expression, oadd_months() returns the last day of
                   the resulting month. Otherwise, oadd_months() returns a value that has the same day component as
                   expression.
                3. The difference between oadd_months() and add_months() is that if a month is added to an end-of-month
                   date in oadd_months(), the function always returns an end-of-month date.
        expression:
            Optional Argument.
            Specifies the number of months to add. If expression is NULL, NULL is returned.
            Types: integer
    RAISES:
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Adds an integer number of months, date or timestamp value in specified date or timestamp value in
        #            a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.oadd_months(2))
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates         res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  2018-05-20
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  2019-04-25
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  2017-06-27
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  2019-07-29
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  2018-12-22
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  2017-10-07
    quarter_end
    teradataml.dataframe.sql.DataFrameColumn.quarter_end = quarter_end()
    DESCRIPTION:
        Function returns the last date or timestamp of the quarter that ends immediately before the specified date or
        timestamp value in a column or as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the last date or timestamp of the quarter that ends immediately before the specified date
        #            or timestamp value in a column or as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.quarter_end())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/31
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/03/31
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/06/30
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/06/30
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/12/31
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/09/30
    quarter_of_calendar
    teradataml.dataframe.sql.DataFrameColumn.quarter_of_calendar = quarter_of_calendar()
    DESCRIPTION:
        Function returns the number of quarters from the beginning of the business calendar to the specified date or
        timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of quarters from the beginning of the business calendar to the specified
        #            date or timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.quarter_of_calendar())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  473
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  477
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  470
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  478
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  476
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  471
    quarter_of_year
    teradataml.dataframe.sql.DataFrameColumn.quarter_of_year = quarter_of_year()
    DESCRIPTION:
        Function returns the number of quarters from the beginning of the year to the specified date or timestamp value
        in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of quarters from the beginning of the year to the specified date or timestamp
        #            value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.quarter_of_year())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25    1
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07    3
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22    4
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27    2
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29    2
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20    1
    quarter_start
    teradataml.dataframe.sql.DataFrameColumn.quarter_start = quarter_start()
    DESCRIPTION:
        Function returns the first date or timestamp of the quarter that begins immediately before the specified date or
        timestamp value in a column or as a literal.
    ALTERNATE NAME:
        year_start
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the first date or timestamp of the quarter that begins immediately before the specified date
        #            or timestamp value in a column or as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.quarter_start())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/01/01
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/01/01
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/01
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/04/01
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/01
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/07/01
    second
    teradataml.dataframe.sql.DataFrameColumn.second = second()
    DESCRIPTION:
        Function returns the integer value for seconds in the specified timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["ocean_buoys"])
        # Create a DataFrame on 'ocean_buoys' table.
        >>> ocean_df = DataFrame.from_table('ocean_buoys')
        # Example 1: Returns the integer value for seconds in the specified timestamp value in a column as a literal.
        >>> res_df = ocean_df.assign(res = ocean_df.TD_TIMECODE.second())
        >>> print(res_df)
                               TD_TIMECODE  salinity  temperature   res
        buoyid
        44      2014-01-06 10:00:25.122200        55           43  25.0
        44      2014-01-06 10:01:25.122200        55           53  25.0
        44      2014-01-06 10:01:25.122200        55           54  25.0
        44      2014-01-06 10:02:25.122200        55           53  25.0
        44      2014-01-06 10:03:25.122200        55           53  25.0
        44      2014-01-06 10:03:25.122200        55           56  25.0
        2       2014-01-06 21:01:25.122200        55           80  25.0
        2       2014-01-06 21:02:25.122200        55           81  25.0
        2       2014-01-06 21:03:25.122200        55           82  25.0
        0       2014-01-06 08:00:00.000000        55           10   0.0
    to_interval
    teradataml.dataframe.sql.DataFrameColumn.to_interval = to_interval(self, value=None, type_=<class 'teradatasqlalchemy.types.INTERVAL_DAY_TO_SECOND'>)
    DESCRIPTION:
        Converts a numeric value or string value into an INTERVAL_DAY_TO_SECOND or INTERVAL_YEAR_TO_MONTH value.
    PARAMETERS:
        value:
            Optional, when column type is VARCHAR or CHAR, otherwise required.
            Specifies the unit of value for numeric value.
            when type_ is INTERVAL_DAY_TO_SECOND permitted values:
                * DAY, HOUR, MINUTE, SECOND
            when type_ is INTERVAL_YEAR_TO_MONTH permitted values:
                * YEAR, MONTH
            Note:
                * Permitted Values are case insensitive.
            Type: str or ColumnExpression
        type_:
            Optional Argument.
            Specifies a teradatasqlalchemy type or an object of a teradatasqlalchemy type
            that the column needs to be cast to.
            Default value: TIMESTAMP
            Permitted Values: INTERVAL_DAY_TO_SECOND or INTERVAL_YEAR_TO_MONTH type.
            Types: teradatasqlalchemy type or object of teradatasqlalchemy type
    Returns:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml", "interval_data")
        # Create a DataFrame on 'interval_data' table.
        >>> df = DataFrame("interval_data")
        >>> df
        id  int_col value_col value_col1        str_col1 str_col2
        2      657    MINUTE      MONTH           PT73H    -P14M
        3     1234    SECOND      MONTH    100 04:23:59    06-10
        1      240      HOUR       YEAR  P100DT4H23M59S  P100Y4M
        0       20       DAY       YEAR    100 04:23:59    04-10
        >>> df.tdtypes
        id                                      INTEGER()
        int_col                                  BIGINT()
        value_col     VARCHAR(length=30, charset='LATIN')
        value_col1    VARCHAR(length=30, charset='LATIN')
        str_col1      VARCHAR(length=30, charset='LATIN')
        str_col2      VARCHAR(length=30, charset='LATIN')
        # Example 1: Convert "int_col" column to INTERVAL_DAY_TO_SECOND with value
        #            provided in "value_col".
        >>> df.assign(col = df.int_col.to_interval(df.value_col))
        id  int_col value_col value_col1        str_col1 str_col2                    col
        2      657    MINUTE      MONTH           PT73H    -P14M      0 10:57:00.000000
        3     1234    SECOND      MONTH    100 04:23:59    06-10      0 00:20:34.000000
        1      240      HOUR       YEAR  P100DT4H23M59S  P100Y4M     10 00:00:00.000000
        0       20       DAY       YEAR    100 04:23:59    04-10     20 00:00:00.000000
        # Example 2: Convert int_col to INTERVAL_YEAR_TO_MONTH when value = 'MONTH'.
        >>> df.assign(col = df.int_col.to_interval('MONTH', INTERVAL_YEAR_TO_MONTH))
        id  int_col value_col value_col1        str_col1 str_col2       col
        2      657    MINUTE      MONTH           PT73H    -P14M     54-09
        3     1234    SECOND      MONTH    100 04:23:59    06-10    102-10
        1      240      HOUR       YEAR  P100DT4H23M59S  P100Y4M     20-00
        0       20       DAY       YEAR    100 04:23:59    04-10      1-08
        # Example 3: Convert string column "str_col1" to INTERVAL_DAY_TO_SECOND.
        >>> df.assign(col = df.str_col1.to_interval())
        id  int_col value_col value_col1        str_col1 str_col2                    col
        2      657    MINUTE      MONTH           PT73H    -P14M      3 01:00:00.000000
        3     1234    SECOND      MONTH    100 04:23:59    06-10    100 04:23:59.000000
        1      240      HOUR       YEAR  P100DT4H23M59S  P100Y4M    100 04:23:59.000000
        0       20       DAY       YEAR    100 04:23:59    04-10    100 04:23:59.000000
        # Example 4: Convert string column "str_col2" to INTERVAL_DAY_TO_MONTH.
        >>> df.assign(col = df.str_col2.to_interval(type_=INTERVAL_YEAR_TO_MONTH))
        id  int_col value_col value_col1        str_col1 str_col2       col
        2      657    MINUTE      MONTH           PT73H    -P14M     -1-02
        3     1234    SECOND      MONTH    100 04:23:59    06-10      6-10
        1      240      HOUR       YEAR  P100DT4H23M59S  P100Y4M    100-04
        0       20       DAY       YEAR    100 04:23:59    04-10      4-10
    to_timestamp
    teradataml.dataframe.sql.DataFrameColumn.to_timestamp = to_timestamp(self, format=None, type_=<class 'teradatasqlalchemy.types.TIMESTAMP'>, timezone=None)
    DESCRIPTION:
        Converts string or integer to a TIMESTAMP data type or TIMESTAMP WITH
        TIME ZONE data type.
        Note:
            * POSIX epoch conversion is implicit in the "to_timestamp" when column
              is integer type. POSIX epoch is the number of seconds that have elapsed
              since midnight Coordinated Universal Time (UTC) of January 1, 1970.
    PARAMETERS:
        format:
            Specifies the format of string column.
            Argument is not required when column is integer type, Otherwise Required.
            For valid 'format' values, see documentation on
            "to_date" or "help(df.col_name.to_date)".
            Type: ColumnExpression or str
        type_:
            Optional Argument.
            Specifies a TIMESTAMP type or an object of a
            TIMESTAMP type that the column needs to be cast to.
            Default value: TIMESTAMP
            Permitted Values: TIMESTAMP data type
            Types: teradatasqlalchemy type or object of teradatasqlalchemy type
        timezone:
            Optional Argument.
            Specifies the timezone string.
            For valid timezone strings, user should check Vantage documentation.
            Type: ColumnExpression or str.
    RETURNS:
        ColumnExpression
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("teradataml", "timestamp_data")
        # Create a DataFrame on 'timestamp_data' table.
        >>> df = DataFrame("timestamp_data")
        >>> df
        id                timestamp_col  timestamp_col1                         format_col     timezone_col
        2  2015-01-08 00:00:12.2+10:00     45678910234  YYYY-MM-DD HH24:MI:SS.FF6 TZH:TZM           GMT+10
        1             2015-01-08 13:00          878986                 YYYY-MM-DD HH24:MI  America Pacific
        0        2015-01-08 00:00:12.2          123456          YYYY-MM-DD HH24:MI:SS.FF6              GMT
        >>> df.tdtypes
        id                                          INTEGER()
        timestamp_col     VARCHAR(length=30, charset='LATIN')
        timestamp_col1                               BIGINT()
        format_col        VARCHAR(length=30, charset='LATIN')
        timezone_col      VARCHAR(length=30, charset='LATIN')
        # Example 1: Convert Epoch seconds to timestamp.
        >>> df.select(['id','timestamp_col1']).assign(col = df.timestamp_col1.to_timestamp())
        id  timestamp_col1                         col
        2     45678910234  3417-07-05 02:10:34.000000
        1          878986  1970-01-11 04:09:46.000000
        0          123456  1970-01-02 10:17:36.000000
        # Example 2: Convert timestamp string to timestamp with timezone in
        #            format mentioned in column "format_col".
        >>> df.select(['id', 'timestamp_col', 'format_col']).assign(col = df.timestamp_col.to_timestamp(df.format_col, TIMESTAMP(ti
        id                timestamp_col                         format_col                             col
        2  2015-01-08 00:00:12.2+10:00  YYYY-MM-DD HH24:MI:SS.FF6 TZH:TZM  2015-01-08 00:00:12.200000+10:00
        1             2015-01-08 13:00                 YYYY-MM-DD HH24:MI  2015-01-08 13:00:00.000000+00:00
        0        2015-01-08 00:00:12.2          YYYY-MM-DD HH24:MI:SS.FF6  2015-01-08 00:00:12.200000+00:00
        # Example 3: Convert Epoch seconds to timestamp with timezone in 'GMT+2' location.
        >>> df.select(['id', 'timestamp_col1', 'format_col']).assign(col = df.timestamp_col1.to_timestamp(df.format_col, TIMESTAMP(
        id  timestamp_col1                         format_col                             col
        2     45678910234  YYYY-MM-DD HH24:MI:SS.FF6 TZH:TZM  3417-07-05 04:10:34.000000+02:00
        1          878986                 YYYY-MM-DD HH24:MI  1970-01-11 06:09:46.000000+02:00
        0          123456          YYYY-MM-DD HH24:MI:SS.FF6  1970-01-02 12:17:36.000000+02:00
    week
    teradataml.dataframe.sql.DataFrameColumn.week = week()
    DESCRIPTION:
        Function returns the number of weeks from the beginning of the year to the specified date or timestamp value in
        a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of weeks from the beginning of the year to the specified date or timestamp
        #            value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.week())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25    9
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07   32
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22   43
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27   17
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29   22
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20   12
    week_end
    teradataml.dataframe.sql.DataFrameColumn.week_end = week_end()
    DESCRIPTION:
        Function returns the last date or timestamp of the week that ends immediately after the specified date or
        timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the last date or timestamp of the week that ends immediately after the specified date or
        #            timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.week_end())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/24
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/03/02
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/29
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/06/01
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/27
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/12
    week_of_calendar
    teradataml.dataframe.sql.DataFrameColumn.week_of_calendar = week_of_calendar()
    DESCRIPTION:
        Function returns the number of weeks from the beginning of the business calendar to the specified date or
        timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of weeks from the beginning of the business calendar to the specified
        #            date or timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.week_of_calendar())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates   res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  6168
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  6217
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  6121
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  6230
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  6199
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  6136
    week_of_month
    teradataml.dataframe.sql.DataFrameColumn.week_of_month = week_of_month()
    DESCRIPTION:
        Function returns the number of weeks from the beginning of the month to the date value in column.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of weeks from the beginning of the month to the date value in column.
        >>> df = df_sales.assign(res = df_sales.dates.week_of_month())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25    4
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07    1
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22    3
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27    4
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29    4
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20    3
    week_of_quarter
    teradataml.dataframe.sql.DataFrameColumn.week_of_quarter = week_of_quarter()
    DESCRIPTION:
        Function returns the number of weeks from the beginning of the quarter to the specified date or timestamp value
        in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of weeks from the beginning of the quarter to the specified date or timestamp
        #            value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.week_of_quarter())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25    8
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07    6
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22    3
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27    4
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29    8
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20   11
    week_of_year
    teradataml.dataframe.sql.DataFrameColumn.week_of_year = week_of_year()
    DESCRIPTION:
        Function returns the number of weeks from the beginning of the year to the specified date or timestamp value in
        a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Import required library.
        >>> import random
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the number of weeks from the beginning of the year to the specified date or timestamp
        #            value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.week_of_year())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates  res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  18/01/13    1
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/07/02   27
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  19/03/12   10
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/12/14   50
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  17/08/25   34
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  17/03/04    9
    week_start
    teradataml.dataframe.sql.DataFrameColumn.week_start = week_start()
    DESCRIPTION:
        Function returns the first date or timestamp of the week that begins immediately before the specified date or
        timestamp value in a column as a literal.
    ALTERNATE NAME:
        week_begin
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the first date or timestamp of the week that begins immediately before the specified date
        #            or timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.week_start())
        >>> print(df)
                              Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Alpha Co            210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/03/18
        Blue Inc             90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/02/24
        Jones LLC           200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/04/23
        Orange Inc          210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/05/26
        Yellow Inc           90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/10/21
        Red Inc             200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/08/06
    year
    teradataml.dataframe.sql.DataFrameColumn.year = year()
    DESCRIPTION:
        Function returns the integer value for year in the specified date or timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the integer value for year in the specified date or timestamp value in a column as a
        #            literal.
        >>> df = df_sales.assign(res = df_sales.dates.year())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates   res
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  2018
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  2019
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  2019
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  2018
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  2017
    year_end
    teradataml.dataframe.sql.DataFrameColumn.year_end = year_end()
    DESCRIPTION:
        Function returns the last date or timestamp of the year that ends immediately before the specified date or
        timestamp value in a column or as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the last date or timestamp of the year that ends immediately before the specified date
        #            or timestamp value in a column or as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.year_end())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/12/31
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/12/31
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/12/31
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/12/31
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/12/31
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/12/31
    year_of_calendar
    teradataml.dataframe.sql.DataFrameColumn.year_of_calendar = year_of_calendar()
    DESCRIPTION:
        Function returns the year of the specified date or timestamp value in a column as a literal.
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the year of the specified date or timestamp value in a column as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.year_of_calendar())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates   res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  2019
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  2018
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  2019
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  2018
    year_start
    teradataml.dataframe.sql.DataFrameColumn.year_start = year_start()
    DESCRIPTION:
        Function returns the first date or timestamp of the year that begins immediately before the specified date or
        timestamp value in a column or as a literal.
    ALTERNATE NAME:
        year_begin
    RAISES:
        TypeError, ValueError, TeradataMlException
    RETURNS:
        DataFrameColumn
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["sales"])
        # Create a DataFrame on 'sales' table.
        >>> df = DataFrame.from_table('sales')
        # Preparing the data.
        >>> df_sales = df.assign(dates = case([(df.accounts == 'Alpha Co', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Blue Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Jones LLC', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Orange Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Yellow Inc', df.datetime + random.randrange(20,1000,3)),
                                (df.accounts == 'Red Inc', df.datetime + random.randrange(20,1000,3))]))
        # Example 1: Returns the first date or timestamp of the year that begins immediately before the specified date
        #            or timestamp value in a column or as a literal.
        >>> df = df_sales.assign(res = df_sales.dates.year_start())
        >>> print(df)
                      Feb    Jan    Mar    Apr    datetime     dates       res
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017  19/02/25  19/01/01
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017  17/08/07  17/01/01
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017  18/10/22  18/01/01
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017  17/04/27  17/01/01
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017  19/05/29  19/01/01
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017  18/03/20  18/01/01
    teradataml: DataFrameColumn Operators
    DataFrameColumn Arithmetic Operators
    __add__
    teradataml.dataframe.sql.DataFrameColumn.__add__ = __add__(self, other)
    Compute the sum between two ColumnExpressions using +.
    This is also the concatenation operator for string-like columns.
    PARAMETERS:
        other :
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Add 100 to the expenditure amount and assign the final amount
        #            to new column 'total_expenditure'.
        >>> df.assign(total_expenditure=df.expenditure + 100)
           start_time_column end_time_column  expenditure  income  investment  total_expenditure
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0              534.0
        2           67/06/30        07/07/10        421.0   465.0       179.0              521.0
        1           67/06/30        07/07/10        415.0   451.0       180.0              515.0
        5           67/06/30        07/07/10        459.0   509.0       211.0              559.0
        4           67/06/30        07/07/10        448.0   493.0       192.0              548.0
        # Example 2: Add expenditure amount to the investment amount and assign the
        #            final amount to new column 'total_investmet'.
        >>> df.assign(total_investmet=df.expenditure + df.investment)
           start_time_column end_time_column  expenditure  income  investment  total_investmet
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0                        619.0
        2           67/06/30        07/07/10        421.0   465.0       179.0                        600.0
        1           67/06/30        07/07/10        415.0   451.0       180.0                        595.0
        5           67/06/30        07/07/10        459.0   509.0       211.0                        670.0
        4           67/06/30        07/07/10        448.0   493.0       192.0                        640.0
    __ﬂoordiv__
    teradataml.dataframe.sql.DataFrameColumn.__ﬂoordiv__ = __ﬂoordiv__(self, other)
    Compute the floor division between two ColumnExpressions using //.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Get the income and investment columns.
        >>> c1 = df.income
        >>> c2 = df.investment
        # Example 1: Divide the income by 2 and assign the divided income to
        #            new column 'divided_income'.
        >>> df.assign(divided_income=c1 // 2)
        # Example 2: Calculate the percent of investment of income and assign the
        #            final amount to new column 'percent_inverstment_'.
        >>> df.assign(percent_inverstment_=df.investment * 100 // df.income)
    __mod__
    teradataml.dataframe.sql.DataFrameColumn.__mod__ = __mod__(self, other)
    Compute the MOD between two ColumnExpressions using %.
    PARAMETERS:
        other :
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Calculate the reminder by taking mod of 2 on income and assign the
        #            reminder to new column 'reminder'.
        >>> df.assign(reminder=df.income.mod(2))
           start_time_column end_time_column  expenditure  income  investment   reminder
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0        1.0
        2           67/06/30        07/07/10        421.0   465.0       179.0        1.0
        1           67/06/30        07/07/10        415.0   451.0       180.0        1.0
        5           67/06/30        07/07/10        459.0   509.0       211.0        1.0
        4           67/06/30        07/07/10        448.0   493.0       192.0        1.0
    __mul__
    teradataml.dataframe.sql.DataFrameColumn.__mul__ = __mul__(self, other)
    Compute the product between two ColumnExpressions using *.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Increase the income for each id by 10 % and assign increased
        #            income to new column 'increased_income'.
        >>> df.assign(increased_income=df.income + df.income * 0.1)
           start_time_column end_time_column  expenditure  income  investment  increased_income
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0             496.1
        4           67/06/30        07/07/10        448.0   493.0       192.0             542.3
        2           67/06/30        07/07/10        421.0   465.0       179.0             511.5
        3           67/06/30        07/07/10        434.0   485.0       185.0             533.5
        5           67/06/30        07/07/10        459.0   509.0       211.0             559.9
        # Example 2: Filter out the rows after increasing the income by 10% is greater than 500.
        >>> df[(df.income + df.income * 0.1) > 500]
           start_time_column end_time_column  expenditure  income  investment
        id
        2           67/06/30        07/07/10        421.0   465.0       179.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
    __neg__
    teradataml.dataframe.sql.DataFrameColumn.__neg__ = __neg__(self)
    Compute the unary negation of the ColumnExpressions using -.
    PARAMETERS:
        None
    RETURNS:
        _SQLColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        13      no  4.00  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        7      yes  2.33    Novice      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        # Example 1: Negate the values in the column 'gpa' and assign those
        #           values to new column 'negate_gpa'.
        >>> df.assign(negate_gpa=-df.gpa)
           masters   gpa     stats programming  admitted  negate_gpa
        id
        5       no  3.44    Novice      Novice         0       -3.44
        34     yes  3.85  Advanced    Beginner         0       -3.85
        13      no  4.00  Advanced      Novice         1       -4.00
        40     yes  3.95    Novice    Beginner         0       -3.95
        22     yes  3.46    Novice    Beginner         0       -3.46
        19     yes  1.98  Advanced    Advanced         0       -1.98
        36      no  3.00  Advanced      Novice         0       -3.00
        15     yes  4.00  Advanced    Advanced         1       -4.00
        7      yes  2.33    Novice      Novice         1       -2.33
        17      no  3.83  Advanced    Advanced         1       -3.83
        # Example 2: Filter out the rows by taking negation of gpa not equal to 3.44 or
        #            admitted equal to 1.
        >>> df[~((df.gpa != 3.44) | (df.admitted == 1))]
                     id masters   gpa   stats  admitted
        programming
        Novice        5      no  3.44  Novice         0
    __radd__
    teradataml.dataframe.sql.DataFrameColumn.__radd__ = __radd__(self, other)
    Compute the rhs sum between two ColumnExpressions using +
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Add 100 to the expenditure amount and assign the final amount
        #            to new column 'total_expenditure'.
        >>> df.assign(total_expenditure=df.expenditure + 100)
           start_time_column end_time_column  expenditure  income  investment  total_expenditure
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0              534.0
        2           67/06/30        07/07/10        421.0   465.0       179.0              521.0
        1           67/06/30        07/07/10        415.0   451.0       180.0              515.0
        5           67/06/30        07/07/10        459.0   509.0       211.0              559.0
        4           67/06/30        07/07/10        448.0   493.0       192.0              548.0
        # Example 2: Add expenditure amount to the investment amount and assign the
        #            final amount to new column 'total_investmet'.
        >>> df.assign(total_investmet=df.expenditure + df.investment)
           start_time_column end_time_column  expenditure  income  investment  total_investmet
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0                        619.0
        2           67/06/30        07/07/10        421.0   465.0       179.0                        600.0
        1           67/06/30        07/07/10        415.0   451.0       180.0                        595.0
        5           67/06/30        07/07/10        459.0   509.0       211.0                        670.0
        4           67/06/30        07/07/10        448.0   493.0       192.0                        640.0
    __rﬂoordiv__
    teradataml.dataframe.sql.DataFrameColumn.__rﬂoordiv__ = __rﬂoordiv__(self, other)
    Compute the floor division between two ColumnExpressions using //.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Divide the income by 2 and assign the divided income
        #            to new column 'divided_income'.
        >>> df.assign(divided_income=c1 // 2)
        # Example 2: Calculate the percent of investment of income and assign the
        #            final output to new column 'percent_inverstment_'.
        >>> df.assign(percent_inverstment_=df.investment * 100 // df.income)
    __rmod__
    teradataml.dataframe.sql.DataFrameColumn.__rmod__ = __rmod__(self, other)
    Compute the MOD between two ColumnExpressions using %.
    Note: string types already override the __mod__ . We cannot override it
          if the string type is the left operand.
    PARAMETERS:
        other :
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression
    RETURNS:
        ColumnExpression, Python literal
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Calculate the reminder by taking mod of 2 on income and assign the
        #            reminder to new column 'reminder'.
        >>> df.assign(reminder=df.income.mod(2))
           start_time_column end_time_column  expenditure  income  investment   reminder
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0        1.0
        2           67/06/30        07/07/10        421.0   465.0       179.0        1.0
        1           67/06/30        07/07/10        415.0   451.0       180.0        1.0
        5           67/06/30        07/07/10        459.0   509.0       211.0        1.0
        4           67/06/30        07/07/10        448.0   493.0       192.0        1.0
    __rmul__
    teradataml.dataframe.sql.DataFrameColumn.__rmul__ = __rmul__(self, other)
    Compute the product between two ColumnExpressions using *.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        # Example 1: Double the income and assign increased
        #            income to new column 'double_income'.
         >>> df.assign(double_income=df.income * 2)
           start_time_column end_time_column  expenditure  income  investment  double_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0          970.0
        2           67/06/30        07/07/10        421.0   465.0       179.0          930.0
        1           67/06/30        07/07/10        415.0   451.0       180.0          902.0
        5           67/06/30        07/07/10        459.0   509.0       211.0         1018.0
        4           67/06/30        07/07/10        448.0   493.0       192.0          986.0
        # Example 2: Filter out the rows after increasing the income by 10% is greater than 500.
        >>> df[(df.income + df.income * 0.1) > 500]
           start_time_column end_time_column  expenditure  income  investment
        id
        2           67/06/30        07/07/10        421.0   465.0       179.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
    __rsub__
    teradataml.dataframe.sql.DataFrameColumn.__rsub__ = __rsub__(self, other)
    Compute the difference between two ColumnExpressions using -.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 1: Subtract 100 from the income amount and assign the final amount
        #            to new column 'remaining_income'.
        >>> df.assign(remaining_income=df.income - 100)
           start_time_column end_time_column  expenditure  income  investment  remaining_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0             385.0
        2           67/06/30        07/07/10        421.0   465.0       179.0             365.0
        1           67/06/30        07/07/10        415.0   451.0       180.0             351.0
        5           67/06/30        07/07/10        459.0   509.0       211.0             409.0
        4           67/06/30        07/07/10        448.0   493.0       192.0             393.0
        # Example 2: Subtract investment amount from the income amount and assign the
        #            final amount to new column 'remaining_income'.
        >>> df.assign(remaining_income=df.income - df.investment)
           start_time_column end_time_column  expenditure  income  investment  remaining_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0             300.0
        2           67/06/30        07/07/10        421.0   465.0       179.0             286.0
        1           67/06/30        07/07/10        415.0   451.0       180.0             271.0
        5           67/06/30        07/07/10        459.0   509.0       211.0             298.0
        4           67/06/30        07/07/10        448.0   493.0       192.0             301.0
    __rtruediv__
    teradataml.dataframe.sql.DataFrameColumn.__rtruediv__ = __rtruediv__(self, other)
    Compute the division between two ColumnExpressions using /.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
       # Example 1: Divide the income by 2 and assign the divided income to new column 'divided_income'.
        >>> df.assign(divided_income=df.income / 2)
           start_time_column end_time_column  expenditure  income  investment  divided_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0           242.5
        2           67/06/30        07/07/10        421.0   465.0       179.0           232.5
        1           67/06/30        07/07/10        415.0   451.0       180.0           225.5
        5           67/06/30        07/07/10        459.0   509.0       211.0           254.5
        4           67/06/30        07/07/10        448.0   493.0       192.0           246.5
        # Example 2: Calculate the percent of investment of income and assign the
        #            final amount to new column 'divided_income'.
        >>> df.assign(divided_income=df.investment * 100 / df.income)
           start_time_column end_time_column  expenditure  income  investment  divided_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0       38.144330
        2           67/06/30        07/07/10        421.0   465.0       179.0       38.494624
        1           67/06/30        07/07/10        415.0   451.0       180.0       39.911308
        5           67/06/30        07/07/10        459.0   509.0       211.0       41.453831
        4           67/06/30        07/07/10        448.0   493.0       192.0       38.945233
    __sub__
    teradataml.dataframe.sql.DataFrameColumn.__sub__ = __sub__(self, other)
    Compute the difference between two ColumnExpressions using -
    Note:
        * Difference between two timestamp columns return value in seconds.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        >>> load_example_data("uaf", "Convolve2RealsLeft")
        >>> timestamp_df = DataFrame("Convolve2RealsLeft")
        >>> timestamp_df
            row_seq                  row_i_time  col_seq               column_i_time    A     B     C     D
        id
        1         1  2018-08-08 08:02:00.000000        0  2018-08-08 08:00:00.000000  1.3  10.3  20.3  30.3
        1         1  2018-08-08 08:02:00.000000        1  2018-08-08 08:02:00.000000  1.4  10.4  20.4  30.4
        1         0  2018-08-08 08:00:00.000000        1  2018-08-08 08:02:00.000000  1.2  10.2  20.2  30.2
        1         0  2018-08-08 08:00:00.000000        0  2018-08-08 08:00:00.000000  1.1  10.1  20.1  30.1
        # Example 1: Subtract 100 from the income amount and assign the final amount
        #            to new column 'remaining_income'.
        >>> df.assign(remaining_income=df.income - 100)
           start_time_column end_time_column  expenditure  income  investment  remaining_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0             385.0
        2           67/06/30        07/07/10        421.0   465.0       179.0             365.0
        1           67/06/30        07/07/10        415.0   451.0       180.0             351.0
        5           67/06/30        07/07/10        459.0   509.0       211.0             409.0
        4           67/06/30        07/07/10        448.0   493.0       192.0             393.0
        # Example 2: Subtract investment amount from the income amount and assign the
        #            final amount to new column 'remaining_income'.
        >>> df.assign(remaining_income=df.income - df.investment)
           start_time_column end_time_column  expenditure  income  investment  remaining_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0             300.0
        2           67/06/30        07/07/10        421.0   465.0       179.0             286.0
        1           67/06/30        07/07/10        415.0   451.0       180.0             271.0
        5           67/06/30        07/07/10        459.0   509.0       211.0             298.0
        4           67/06/30        07/07/10        448.0   493.0       192.0             301.0
        # Example 3: Subtract 2 timestamp columns and assign to new column 'seconds'.
        >>> timestamp_df.assign(seconds = timestamp_df.row_i_time-timestamp_df.column_i_time)
            row_seq                  row_i_time  col_seq               column_i_time    A     B     C     D  seconds
        id
        1         1  2018-08-08 08:02:00.000000        0  2018-08-08 08:00:00.000000  1.3  10.3  20.3  30.3    120.0
        1         1  2018-08-08 08:02:00.000000        1  2018-08-08 08:02:00.000000  1.4  10.4  20.4  30.4      0.0
        1         0  2018-08-08 08:00:00.000000        1  2018-08-08 08:02:00.000000  1.2  10.2  20.2  30.2   -120.0
        1         0  2018-08-08 08:00:00.000000        0  2018-08-08 08:00:00.000000  1.1  10.1  20.1  30.1      0.0
    __truediv__
    teradataml.dataframe.sql.DataFrameColumn.__truediv__ = __truediv__(self, other)
    Compute the division between two ColumnExpressions using /.
    PARAMETERS:
        other :
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
       # Example 1: Divide the income by 2 and assign the final amount to new column 'half_income'.
        >>> df.assign(half_income=df.income / 2)
           start_time_column end_time_column  expenditure  income  investment  half_income
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0        242.5
        2           67/06/30        07/07/10        421.0   465.0       179.0        232.5
        1           67/06/30        07/07/10        415.0   451.0       180.0        225.5
        5           67/06/30        07/07/10        459.0   509.0       211.0        254.5
        4           67/06/30        07/07/10        448.0   493.0       192.0        246.5
        # Example 2: Calculate the percent of investment of income and assign the
        #            final amount to new column 'percent_inverstment_'.
        >>> df.assign(percent_inverstment_=df.investment * 100 / df.income)
           start_time_column end_time_column  expenditure  income  investment  percent_inverstment_
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0             38.144330
        2           67/06/30        07/07/10        421.0   465.0       179.0             38.494624
        1           67/06/30        07/07/10        415.0   451.0       180.0             39.911308
        5           67/06/30        07/07/10        459.0   509.0       211.0             41.453831
        4           67/06/30        07/07/10        448.0   493.0       192.0             38.945233
    DataFrameColumn Logical Operators
    __and__
    teradataml.dataframe.sql.DataFrameColumn.__and__ = __and__(self, other)
    Compute the logical AND between two ColumnExpressions using &.
    The logical AND operator is an operator that performs a logical
    conjunction on two statements.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students who got the admission (i.e., admitted = 1)
        #            and has GPA greater than 3.5.
        >>> df[(df.gpa > 3.5) & (df.admitted == 1)]
           masters   gpa     stats programming  admitted
        id
        21      no  3.87    Novice    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        3       no  3.70    Novice    Beginner         1
        20     yes  3.90  Advanced    Advanced         1
        37      no  3.52    Novice      Novice         1
        35      no  3.68    Novice    Beginner         1
        12      no  3.65    Novice      Novice         1
        18     yes  3.81  Advanced    Advanced         1
        17      no  3.83  Advanced    Advanced         1
        23     yes  3.59  Advanced      Novice         1
    __eq__
    teradataml.dataframe.sql.DataFrameColumn.__eq__ = __eq__(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values equal to the other.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students with gpa equal to 3.44.
        >>> df[df.gpa == 3.44]
           masters   gpa   stats programming  admitted
        id
        5       no  3.44  Novice      Novice         0
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure equal to 415 or income equal to 509.
        >>> df[(df.expenditure == 415) | (df.income == 509)]
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
    __gt__
    teradataml.dataframe.sql.DataFrameColumn.__ge__ = __ge__(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values greater than or equal to the other or not.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students with gpa greater than
        #            or equal to 3.
        >>> df[df.gpa >= 3]
           masters   gpa     stats programming  admitted
        id
        30     yes  3.79  Advanced      Novice         0
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        37      no  3.52    Novice      Novice         1
        14     yes  3.45  Advanced    Advanced         0
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure greater than or equal to 450 and
        #            investment is greater than or equal to 200.
        >>> df[(df.expenditure >= 450) & (df.investment >= 200)]
           start_time_column end_time_column  expenditure  income  investment
        id
        5           67/06/30        07/07/10        459.0   509.0       211.0
    __mul__
    teradataml.dataframe.sql.DataFrameColumn.__gt__ = __gt__(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values greater than the other or not.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all the students with gpa greater than 3.
        >>> df[df.gpa > 3]
           masters   gpa     stats programming  admitted
        id
        14     yes  3.45  Advanced    Advanced         0
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        3       no  3.70    Novice    Beginner         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        37      no  3.52    Novice      Novice         1
        1      yes  3.95  Beginner    Beginner         0
        31     yes  3.50  Advanced    Beginner         1
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure greater than 400 and investment
        #            greater than 170.
        >>> df[(df.expenditure > 400) & (df.investment > 170)]
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
    __invert__
    teradataml.dataframe.sql.DataFrameColumn.__invert__ = __invert__(self)
    Compute the logical NOT of ColumnExpression using ~.
    PARAMETERS:
        None
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get the negation of gpa not equal to 3.44 or
        #            admitted equal to 1.
        >>> df[~((df.gpa != 3.44) | (df.admitted == 1))]
                     id masters   gpa   stats  admitted
        programming
        Novice        5      no  3.44  Novice         0
    __lt__
    teradataml.dataframe.sql.DataFrameColumn.__le__ = __le__(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values less than or equal to the other or not.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students with gpa less than or
        #            equal to 3.
        >>> df[df.gpa <= 3]
           masters   gpa     stats programming  admitted
        id
        24      no  1.87  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        19     yes  1.98  Advanced    Advanced         0
        38     yes  2.65  Advanced    Beginner         1
        7      yes  2.33    Novice      Novice         1
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure less than or equal to
        #            500 and income less than or equal to 480.
        >>> df[(df.expenditure <= 500) & (df.income <= 480)]
           start_time_column end_time_column  expenditure  income  investment
        id
        2           67/06/30        07/07/10        421.0   465.0       179.0
        1           67/06/30        07/07/10        415.0   451.0       180.0
    __rﬂoordiv__
    teradataml.dataframe.sql.DataFrameColumn.__lt__ = __lt__(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values less than the other or not.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students with gpa less than 3.
        >>> df[df.gpa < 3]
           masters   gpa     stats programming  admitted
        id
        24      no  1.87  Advanced      Novice         1
        19     yes  1.98  Advanced    Advanced         0
        38     yes  2.65  Advanced    Beginner         1
        7      yes  2.33    Novice      Novice         1
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure less than 440 and
        #            income greater than 180.
        >>> df[(df.expenditure < 440) & (df.income > 180)]
           start_time_column end_time_column  expenditure  income  investment
        id
        3           67/06/30        07/07/10        434.0   485.0       185.0
    __ne__
    teradataml.dataframe.sql.DataFrameColumn.__ne__ = __ne__(self, other)
    Compare the ColumnExpressions to check if one ColumnExpression
    has values not equal to the other.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students with gpa not equal to 3.44.
        >>> df[df.gpa != 3.44]
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        24      no  1.87  Advanced      Novice         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        3       no  3.70    Novice    Beginner         1
        30     yes  3.79  Advanced      Novice         0
        >>> load_example_data("burst", "finance_data")
        >>> df = DataFrame("finance_data")
        >>> df
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
        # Example 2: Get all rows with expenditure not equal to 415 or income
        #            not equal to 400.
        >>> df[(df.expenditure != 415) | (df.income != 400)]
           start_time_column end_time_column  expenditure  income  investment
        id
        1           67/06/30        07/07/10        415.0   451.0       180.0
        4           67/06/30        07/07/10        448.0   493.0       192.0
        2           67/06/30        07/07/10        421.0   465.0       179.0
        3           67/06/30        07/07/10        434.0   485.0       185.0
        5           67/06/30        07/07/10        459.0   509.0       211.0
    __or__
    teradataml.dataframe.sql.DataFrameColumn.__or__ = __or__(self, other)
    Compute the logical OR between two ColumnExpressions using |.
    The logical OR operator is an operator that performs a
    inclusive disjunction on two statements.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students with gpa greater than
        #            3.5 or 'Advanced' programming skills.
        >>> df[(df.gpa > 3.5) | (df.programming == "Advanced")]
           masters   gpa     stats programming  admitted
        id
        30     yes  3.79  Advanced      Novice         0
        40     yes  3.95    Novice    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        37      no  3.52    Novice      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        20     yes  3.90  Advanced    Advanced         1
        35      no  3.68    Novice    Beginner         1
        14     yes  3.45  Advanced    Advanced         0
    __rand__
    teradataml.dataframe.sql.DataFrameColumn.__rand__ = __rand__(self, other)
    Compute the reverse of logical AND between two ColumnExpressions using &.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students who got the admission (i.e., admitted = 1)
        #            and has GPA greater than 3.5.
        >>> df[(df.gpa > 3.5) & (df.admitted == 1)]
           masters   gpa     stats programming  admitted
        id
        21      no  3.87    Novice    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        3       no  3.70    Novice    Beginner         1
        20     yes  3.90  Advanced    Advanced         1
        37      no  3.52    Novice      Novice         1
        35      no  3.68    Novice    Beginner         1
        12      no  3.65    Novice      Novice         1
        18     yes  3.81  Advanced    Advanced         1
        17      no  3.83  Advanced    Advanced         1
        23     yes  3.59  Advanced      Novice         1
    __ror__
    teradataml.dataframe.sql.DataFrameColumn.__ror__ = __ror__(self, other)
    Compute the reverse of logical OR between two ColumnExpressions using |.
    PARAMETERS:
        other:
            Required Argument.
            Specifies Python literal or another ColumnExpression.
            Types: ColumnExpression, Python literal
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students with gpa greater than
        #            3.5 or 'Advanced' programming skills.
        >>> df[(df.gpa > 3.5) | (df.programming == "Advanced")]
           masters   gpa     stats programming  admitted
        id
        30     yes  3.79  Advanced      Novice         0
        40     yes  3.95    Novice    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        37      no  3.52    Novice      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        20     yes  3.90  Advanced    Advanced         1
        35      no  3.68    Novice    Beginner         1
        14     yes  3.45  Advanced    Advanced         0
    __rxor__
    teradataml.dataframe.sql.DataFrameColumn.__rxor__ = __rxor__(self, other)
    Compute the reverse of logical XOR between two ColumnExpressions using ^.
    PARAMETERS:
        other:
            Required Argument.
            Specifies another ColumnExpression.
            Types: ColumnExpression
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students with gpa greater than
        #            3.5 or programming skills are 'Advanced'.
        >>> df[(df.gpa > 3.5) ^ (df.programming == "Advanced")]
           masters   gpa     stats programming  admitted
        id
        14     yes  3.45  Advanced    Advanced         0
        40     yes  3.95    Novice    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        37      no  3.52    Novice      Novice         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        2      yes  3.76  Beginner    Beginner         0
        35      no  3.68    Novice    Beginner         1
        29     yes  4.00    Novice    Beginner         0
        30     yes  3.79  Advanced      Novice         0
    __xor__
    teradataml.dataframe.sql.DataFrameColumn.__xor__ = __xor__(self, other)
    Compute the logical XOR between two ColumnExpressions using ^.
    The logical XOR operator is an operator that performs a
    exclusive disjunction on two statements.
    PARAMETERS:
        other:
            Required Argument.
            Specifies another ColumnExpression.
            Types: ColumnExpression
    RETURNS:
        ColumnExpression
    RAISES:
        Exception
            A TeradataMlException gets thrown if SQLAlchemy
            throws an exception when evaluating the expression.
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df
           masters   gpa     stats programming  admitted
        id
        15     yes  4.00  Advanced    Advanced         1
        40     yes  3.95    Novice    Beginner         0
        7      yes  2.33    Novice      Novice         1
        22     yes  3.46    Novice    Beginner         0
        38     yes  2.65  Advanced    Beginner         1
        26     yes  3.57  Advanced    Advanced         1
        5       no  3.44    Novice      Novice         0
        24      no  1.87  Advanced      Novice         1
        39     yes  3.75  Advanced    Beginner         0
        30     yes  3.79  Advanced      Novice         0
        # Example 1: Get all students with gpa is greater
        #            than 3.5 or programming skills are 'Advanced'.
        >>> df[(df.gpa > 3.5) ^ (df.programming == "Advanced")]
           masters   gpa     stats programming  admitted
        id
        14     yes  3.45  Advanced    Advanced         0
        40     yes  3.95    Novice    Beginner         0
        39     yes  3.75  Advanced    Beginner         0
        37      no  3.52    Novice      Novice         1
        3       no  3.70    Novice    Beginner         1
        1      yes  3.95  Beginner    Beginner         0
        2      yes  3.76  Beginner    Beginner         0
        35      no  3.68    Novice    Beginner         1
        29     yes  4.00    Novice    Beginner         0
        30     yes  3.79  Advanced      Novice         0
    teradataml: DataFrameColumn Properties
    expression
    teradataml.dataframe.sql.DataFrameColumn.expression
    A reference to the underlying column expression.
    PARAMETERS:
        None
    RETURNS:
        sqlalchemy.sql.elements.ColumnClause
    RAISE:
        None
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df.gpa.expression
        Column('gpa', FLOAT(), table=<admissions_train>, nullable=False)
    name
    teradataml.dataframe.sql.DataFrameColumn.name
    Returns the underlying name attribute of self.expression or None
    if the expression has no name. Note that the name may also refer to
    an alias or label() in sqlalchemy
    PARAMETERS:
        None
    RETURNS:
        str
    RAISE:
        None
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df.gpa.name
        gpa
    table
    teradataml.dataframe.sql.DataFrameColumn.table
    Returns the underlying table attribute of the sqlalchemy.Column
    PARAMETERS:
        None
    RETURNS:
        str
    RAISE:
        None
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df.gpa.table
        Table('admissions_train', MetaData(bind=Engine(teradatasql://alice:***@sdt61582.labs.teradata.com)),
        Column('id', INTEGER(), table=<admissions_train>, nullable=False), Column('masters', VARCHAR(length=5,
        charset='LATIN'), table=<admissions_train>, nullable=False), Column('gpa', FLOAT(), table=<admissions_train>,
        nullable=False), Column('stats', VARCHAR(length=30, charset='LATIN'), table=<admissions_train>, nullable=False),
        Column('programming', VARCHAR(length=30, charset='LATIN'), table=<admissions_train>, nullable=False),
        Column('admitted', INTEGER(), table=<admissions_train>, nullable=False), schema=None)
    type
    teradataml.dataframe.sql.DataFrameColumn.type
    Returns the underlying sqlalchemy type of the current expression.
    PARAMETERS:
        None
    RETURNS:
        teradatasqlalchemy.types
    RAISE:
        None
    EXAMPLES:
        >>> load_example_data("dataframe", "admissions_train")
        >>> df = DataFrame("admissions_train")
        >>> df.gpa.type
        FLOAT()