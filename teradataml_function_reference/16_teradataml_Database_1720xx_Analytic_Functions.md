# teradataml Database 17.20.xx Analytic Functions

PATH AND PATTERN ANALYSIS functions
Attribution
Attribution
Functions
Attribution(data=None, data_optional=None, data_optional2=None, data_optional3=None, data_optional4=None, conversion_data=None, excluding_data=None,
optional_data=None, model1_type=None, model2_type=None, event_column=None, timestamp_column=None, window_size=None, **generic_arguments)
    DESCRIPTION:
        The Attribution() function is used in web page analysis, where it lets
        companies assign weights to pages before certain events, such as
        buying a product.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        data_optional:
            Optional Argument.
            Specifies the click stream data, which the function uses to compute
            attributions.
            Types: teradataml DataFrame
        data_optional2:
            Optional Argument.
            Specifies the click stream data, which the function uses to compute
            attributions.
            Types: teradataml DataFrame
        data_optional3:
            Optional Argument.
            Specifies the click stream data, which the function uses to compute
            attributions.
            Types: teradataml DataFrame
        data_optional4:
            Optional Argument.
            Specifies the click stream data, which the function uses to compute
            attributions.
            Types: teradataml DataFrame
        conversion_data:
            Required Argument.
            Specifies one varchar column (conversion_events) containing conversion
            event values.
            Types: teradataml DataFrame
        excluding_data:
            Optional Argument.
            Specifies one varchar column (excluding_events) containing excluding
            cause event values.
            Types: teradataml DataFrame
        optional_data:
            Optional Argument.
            Specifies one varchar column (optional_events) containing optional
            cause event values.
            Types: teradataml DataFrame
        model1_type:
            Required Argument.
            Specifies the type and specification of the first model.
            For example:
                +----+----------------------+
                | id |        model         |
                +----+----------------------+
                | 0  |   SEGMENT_SECONDS    |
                |    |                      |
                | 1  |   6:0.5:UNIFORM:NA   |
                |    |                      |
                | 2  | 8:0.3:LAST_CLICK:NA  |
                |    |                      |
                | 3  | 6:0.2:FIRST_CLICK:NA |
                +----+----------------------+
            Types: teradataml DataFrame
        model2_type:
            Optional Argument.
            Specifies the type and distributions of the second model.
            For example:
                +----+--------------------------------+
                | id |             model              |
                +----+--------------------------------+
                | 0  |          SEGMENT_ROWS          |
                |    |                                |
                | 1  |  3:0.5:EXPONENTIAL:0.5,SECOND  |
                |    |                                |
                | 2  | 4:0.3:WEIGHTED:0.4,0.3,0.2,0.1 |
                |    |                                |
                | 3  |      3:0.2:FIRST_CLICK:NA      |
                +----+--------------------------------+
            Types: teradataml DataFrame
        event_column:
            Required Argument.
            Specifies the name of the input column that contains the clickstream
            events.
            Types: str
        timestamp_column:
            Required Argument.
            Specifies the name of the input column that contains the timestamps
            of the clickstream events.
            Types: str
        window_size:
            Required Argument.
            Specifies how to determine the maximum window size for the
            attribution calculation: rows:K: Consider the maximum number of
            events to be attributed, excluding events of types specified in
            excluding_event_table, which means assigning attributions to at most
            K effective events before the current impact event.seconds:K:
            Consider the maximum time difference between the current impact event
            and the earliest effective event to be attributed. rows:K&seconds:K2:
            Consider both constraints and comply with the stricter one.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQLE function supports it, else an exception is raised.
    RETURNS:
        Instance of Attribution.
        Output teradataml DataFrames can be accessed using attribute
        references, such as AttributionObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the data to run the example
        load_example_data("attribution", ["attribution_sample_table1",
        "attribution_sample_table2" , "conversion_event_table",
        "optional_event_table", "model1_table", "model2_table"])
        # Create teradataml DataFrame objects
        attribution_sample_table1 = DataFrame.from_table("attribution_sample_table1")
        attribution_sample_table2 = DataFrame.from_table("attribution_sample_table2")
        conversion_event_table = DataFrame.from_table("conversion_event_table")
        optional_event_table = DataFrame.from_table("optional_event_table")
        model1_table = DataFrame.from_table("model1_table")
        model2_table = DataFrame.from_table("model2_table")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Assign attribution weights to events and channels.
        attribution_out = Attribution(data=attribution_sample_table1,
                                      data_partition_column="user_id",
                                      data_order_column="time_stamp",
                                      data_optional=attribution_sample_table2,
                                      data_optional_partition_column='user_id',
                                      data_optional_order_column='time_stamp',
                                      event_column="event",
                                      conversion_data=conversion_event_table,
                                      optional_data=optional_event_table,
                                      timestamp_column = "time_stamp",
                                      window_size = "rows:10&seconds:20",
                                      model1_type=model1_table,
                                      model2_type=model2_table)
        # Print the results DataFrame.
        print(attribution_out.result)
    NPath
    NPath
    Functions
    NPath(data1=None, mode=None, pattern=None, symbols=None, result=None, ﬁlter=None, data2=None, data3=None, data4=None, data5=None, data6=None,
    data7=None, data8=None, data9=None, data10=None, **generic_arguments)
    DESCRIPTION:
    The NPath() function scans a set of rows, looking for patterns that you
    specify. For each set of input rows that matches the pattern, NPath
    produces a single output row. The function provides a flexible
    pattern-matching capability that lets you specify complex patterns in
    the input data and define the values that are output for each matched
    input set.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the input teradataml DataFrame containing the input data set.
            Types: teradataml DataFrame
        mode:
            Required Argument.
            Specifies the pattern-matching mode:
                * OVERLAPPING:
                    The function finds every occurrence of the pattern in
                    the partition, regardless of whether it is part of a previously
                    found match. Therefore, one row can match multiple symbols in a
                    given matched pattern.
                * NONOVERLAPPING:
                    The function begins the next pattern search at the
                    row that follows the last pattern match. This is the default
                    behavior of many commonly used pattern matching utilities, including
                    the UNIX grep utility.
            Permitted Values: OVERLAPPING, NONOVERLAPPING
            Types: str
        pattern:
            Required Argument.
            Specifies the pattern for which the function searches. You compose
            pattern with the symbols that you define in the symbols argument,
            operators, and parentheses.
            When patterns have multiple operators, the function applies
            them in order of precedence, and applies operators of equal
            precedence from left to right. To specify that a subpattern must
            appear a specific number of times, use the Range-Matching
            Feature.
            The basic pattern operators in decreasing order of precedence
                "pattern", "pattern.", "pattern?", "pattern*", "pattern+",
                "pattern1.pattern2", "pattern1|pattern2", "^pattern", "pattern$"
            To force the function to evaluate a subpattern first, enclose it in parentheses.
            Example:
                ^A.(B|C)+.D?.X*.A$
                The preceding pattern definition matches any set of rows
                whose first row starts with the definition of symbol A,
                followed by a non-empty sequence of rows, each of which
                meets the definition of either symbol B or C, optionally
                followed by one row that meets the definition of symbol D,
                followed by any number of rows that meet the definition of
                symbol X, and ending with a row that ends with the definition of symbol A.
            You can use parentheses to define precedence rules. Parentheses are
            recommended for clarity, even where not strictly required.
            Types: str
        symbols:
            Required Argument.
            Specifies the symbols that appear in the values of the pattern and
            result arguments. The col_expr is an expression whose value is a
            column name, symbol is any valid identifier, and symbol_predicate is
            a SQL predicate (often a column name).
            For example, the 'symbols' argument for analyzing website visits might
            look like this:
                Symbols
                (
                 pagetype = "homepage" AS H,
                 pagetype <> "homepage" AND  pagetype <> "checkout" AS PP,
                 pagetype = "checkout" AS CO
                )
            The symbol is case-insensitive; however, a symbol of one or two
            uppercase letters is easy to identify in patterns.
            If col_expr represents a column that appears in multiple input
            DataFrames, then you must qualify the ambiguous column name with
            the SQL name corresponding to it's teradataml DataFrame name.
            For example:
                Symbols
                (
                 input1.pagetype = "homepage" AS H,
                 input1.pagetype = "thankyou" AS T,
                 input2.adname = "xmaspromo" AS X,
                 input2.adname = "realtorpromo" AS R
                )
            The mapping from teradataml DataFrame name to its corresponding SQL name
            is as shown below:
                * data1: input1
                * data2: input2
                * data3: input3
            You can create symbol predicates that compare a row to a previous
            or subsequent row, using a LAG or LEAD operator.
            LAG Expression Syntax:
                { current_expr operator LAG (previous_expr, lag_rows [, default]) |
                LAG (previous_expr, lag_rows [, default]) operator current_expr }
            LAG and LEAD Expression Rules:
                • A symbol definition can have multiple LAG and LEAD expressions.
                • A symbol definition that has a LAG or LEAD expression cannot have an OR operator.
                • If a symbol definition has a LAG or LEAD expression and the input
                  is not a table, you must create an alias of the input query.
            Types: str OR list of Strings (str)
        result:
            Required Argument.
            Specifies the output columns. The col_expr is an expression whose value
            is a column name; it specifies the values to retrieve from the
            matched rows. The function applies aggregate function to these
            values.
            Supported aggregate functions:
                • SQL aggregate functions are [AVG, COUNT, MAX, MIN, SUM].
                • ML Engine nPath sequence aggregate functions.
            The function evaluates this argument once for every matched pattern
            in the partition (that is, it outputs one row for each pattern match).
            Note:
                For col_expr representing a column that appears in multiple input
                DataFrames, you must qualify the ambiguous column name with the SQL
                name corresponding to it's teradataml DataFrame name. Please see the
                description of the 'symbols' parameter for the mapping from teradataml
                DataFrame name to the SQL name.
            Types: str OR list of Strings (str)
        filter:
            Optional Argument.
            Specifies filters to impose on the matched rows. The function
            combines the filter expressions using the AND operator.
            The filter_expression syntax is:
                symbol_expression comparison_operator symbol_expression
            The two symbol expressions must be type-compatible.
            The symbol_expression syntax is:
                { FIRST | LAST }(column_with_expression OF [ANY](symbol[,...]))
            The column_with_expression cannot contain the operator AND or OR, and
            all its columns must come from the same input. If the function has
            multiple inputs, then column_with_expression and symbol must come
            from the same input.
            The comparison_operator is either <, >, <=, >=, =, or <>.
            Note:
                For column_with_expression representing a column that appears in
                multiple input DataFrames, you must qualify the ambiguous column name with
                the SQL name corresponding to it's teradataml DataFrame name. Please see
                the description of the 'symbols' parameter for the mapping from teradataml
                DataFrame name to the SQL name.
            Types: str OR list of Strings (str)
        data2:
            Optional Arguments.
            Specifies the additional optional input teradataml DataFrames containing the input data.
            Types: teradataml DataFrame
        data3:
            Optional Arguments.
            Specifies the additional optional input teradataml DataFrames containing the input data.
            Types: teradataml DataFrame
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of NPath.
        Output teradataml DataFrames can be accessed using attribute
        references, such as NPathObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load example data.
        load_example_data("NPath",["impressions","clicks2", "tv_spots", "clickstream"])
        # Create input teradataml dataframes.
        impressions = DataFrame.from_table("impressions")
        clicks2 = DataFrame.from_table("clicks2")
        tv_spots = DataFrame.from_table("tv_spots")
        clickstream = DataFrame.from_table("clickstream")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Search for pattern '(imp|tv_imp)*.click' in the provided
        #           data(impressions, clicks2, tv_spots).
        # Run NPath function with the required patterns to get the rows which
        # have specified pattern. Rows that matches the pattern.
        obj = NPath(data1=impressions,
                    data1_partition_column='userid',
                    data1_order_column='ts',
                    data2=clicks2,
                    data2_partition_column='userid',
                    data2_order_column='ts',
                    data3=tv_spots,
                    data3_partition_column='ts',
                    data3_order_column='ts',
                    result=['COUNT(* of imp) as imp_cnt','COUNT(* of tv_imp) as tv_imp_cnt'],
                    mode='nonoverlapping',
                    pattern='(imp|tv_imp)*.click',
                    symbols=['true as imp','true as click','true as tv_imp'])
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Search for pattern 'home.clickview*.checkout' in the provided
        #           data set clickstream.
        # Run NPath function with the required patterns to get the rows which
        # has specified pattern and filter the rows with the filter,
        # where filter and result have ML Engine nPath sequence aggregate functions
        # like 'FIRST', 'COUNT' and 'LAST'.
        obj = NPath(data1=clickstream,
                    data1_partition_column='userid',
                    data1_order_column='clicktime',
                    result=['FIRST(userid of ANY(home, checkout, clickview)) AS userid',
                            'FIRST (sessionid of ANY(home, checkout, clickview)) AS sessioinid',
                            'COUNT (* of any(home, checkout, clickview)) AS cnt',
                            'FIRST (clicktime of ANY(home)) AS firsthome',
                            'LAST (clicktime of ANY(checkout)) AS lastcheckout'],
                    mode='nonoverlapping',
                    pattern='home.clickview*.checkout',
                    symbols=["pagetype='home' AS home",
                             "pagetype <> 'home' AND pagetype <> 'checkout' AS clickview",
                             "pagetype='checkout' AS checkout"],
                    filter = "FIRST (clicktime OF ANY (home)) <"
                             "FIRST (clicktime of any(checkout))")
        # Print the result DataFrame.
        print(obj.result)
    Sessionize
    Sessionize
    Functions
    Sessionize(data=None, time_column=None, time_out=None, click_lag=None, emit_null=False, **generic_arguments)
    DESCRIPTION:
        Sessionize() function maps each click in a session to a unique session identifier.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        time_column:
            Required Argument.
            Specifies the name of the input column that contains the click
            times.
            Note: The "time_column" must also be an "order_column".
            Types: str
        time_out:
            Required Argument.
            Specifies the number of seconds at which the session times out. If
            "time_out" seconds elapse after a click, then the next click
            starts a new session.
            Types: float
        click_lag:
            Optional Argument.
            Specifies the minimum number of seconds between clicks for the
            session user to be considered human. If clicks are more frequent,
            indicating that the user is a bot, the function ignores the session.
            The "click_lag" must be less than "time_out".
            Types: float
        emit_null:
            Optional Argument.
            Specifies whether to output rows that have NULL values in their
            session id and rapid fire columns, even if their timestamp_column has
            a NULL value.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of Sessionize.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SessionizeObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("sessionize", ["sessionize_table"])
        # Create teradataml DataFrame object.
        sessionize_data = DataFrame.from_table("sessionize_table")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Mapping each click in a session to a unique session identifier.
        #            by partition column 'partition_id' and order column 'clicktime'.
        obj = teradataml.Sessionize(data=sessionize_data,
                                    data_partition_column='partition_id',
                                    data_order_column='clicktime',
                                    time_column='clicktime',
                                    time_out=60.0,
                                    click_lag=0.2)
        # Print the result DataFrame.
        print(obj.result)
    ASSOCIATION ANALYSIS functions
    Apriori
    Apriori
    Functions
    Apriori(data=None, target_column=None, id_column=None, partition_columns=None, max_len=2, delimiter=',', is_dense_input=False, patterns_or_rules=None,
    support=0.01, **generic_arguments)
    DESCRIPTION:
        The Apriori() function finds patterns and calculates different statistical metrics to
        understand the influence of the occurrence of a set of items on others.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_column:
            Required Argument.
            Specifies the input teradataml DataFrame column which contains the data to filter.
            Types: str
        id_column:
            Optional Argument.
            Specifies the name of the column that uniquely groups the items that are purchased together. 
            Applicable only when `is_dense_input` is False.
            Types: str
        partition_columns:
            Optional Argument.
            Specifies the column name(s) in the "data" to partition the input.
            Types: str or list of str
        max_len:
            Optional Argument.
            Specifies the maximum number of items in the item set. 
            "max_len" must be greater than or equal to 1 and less than or equal to 20.
            Default Value: 2
            Types: int
        delimiter:
            Optional Argument, Required when "is_dense_input" is set to True.
            Specifies a character or string that separates words in the input text.
            Default Value: ","
            Types: str
        is_dense_input:
            Optional Argument.
            Specifies whether input data is in dense format or not. 
            When set to True, function considers the  data is in dense format. 
            Otherwise function considers  data is not in dense format.
            Default Value: False
            Types: bool
        patterns_or_rules:
            Optional Argument.
            Specifies whether to emit PATTERNS or RULES as output.
            Permitted Values: "PATTERNS", "RULES"
            Types: str
        support:
            Optional Argument.
            Specifies the support value (minimum occurrence threshold) of the itemset.
            Default Value: 0.01
            Types: float
        **generic_arguments:
            Optional Argument.
            Specifies the generic keyword arguments SQLE functions accept. Below are the generic
            keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results are
                    garbage collected at the end of the session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table; otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQLE Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of Apriori.
        Output teradataml DataFrames can be accessed using attribute references, such as AprioriObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in the example from teradataml.
        #     3. Function will raise an error if not supported on the Vantage user is connected to.
        # Load the example data.
        load_example_data("apriori", ["trans_dense","trans_sparse"])
        # Create teradataml DataFrame objects.
        dense_table = DataFrame.from_table("trans_dense")
        sparse_table = DataFrame.from_table("trans_sparse")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function Apriori.
        from teradataml import Apriori
        # Example 1: Find patterns in the input data with DENSE DATA, PARTITION, RULES .
        Apriori_out = Apriori(data=dense_table, target_column="item",
                              partition_columns=["location"], max_len=2,
                              patterns_or_rules="rules", support=0.01)
        # Print the result DataFrame.
        print(Apriori_out.result)
        # Example 2: Find patterns in the input data with SPARSE DATA, NO PARTITIONS, PATTERNS.
        Apriori_out = Apriori(data=sparse_table, target_column="item",
                              id_column="tranid", max_len=3)
        # Print the result DataFrame.
        print(Apriori_out.result)
    CFilter
    CFilter
    Functions
    CFilter(data=None, target_column=None, transaction_id_columns=None, partition_columns=None, max_distinct_items=100, **generic_arguments)
    DESCRIPTION:
        Function calculates several statistical measures of how likely 
        each pair of items is to be purchased together.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_column:
            Required Argument.
            Specifies name of the column from the "data" containing data for filtration.
            Types: str
        transaction_id_columns:
            Required Argument.
            Specifies the name of the columns in "data" containing transaction id that defines the groups of items listed
            in the input columns that are purchased together.
            Types: str OR list of Strings (str)
        partition_columns:
            Optional Argument.
            Specifies the name of the column in "data" to partition the data on.
            Types: str OR list of Strings (str)
        max_distinct_items:
            Optional Argument.
            Specifies the maximum size of the item set. 
            Default Value: 100
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of CFilter.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as CFilterObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the 
        #        function in user space.
        #     2. User can import the function, if it is available on 
        #        Vantage user is connected to.
        #     3. To check the list of analytic functions available on 
        #        Vantage user connected to, use 
        #        "display_analytic_functions()".
        # Load the example data.
        load_example_data("dataframe", ["grocery_transaction"])
        # Create teradataml DataFrame objects.
        df = DataFrame.from_table("grocery_transaction")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function CFilter.
        from teradataml import CFilter
        # Example 1: CFilter function to calculate the statistical measures
        #            of how likely each pair of items is to be purchased together, without
        #            specifying the partition_columns.
        CFilter_out = CFilter(data=df, 
                              target_column='item',
                              transaction_id_columns = 'tranid',
                              max_distinct_items=100)
        # Print the result DataFrame.
        print(CFilter_out.result)
        # Example 2: CFilter function to calculate the statistical measures
        #            of how likely each pair of items is to be purchased together,
        #            specifying the partition_columns.
        CFilter_out2 = CFilter(data=df, 
                               target_column='item', 
                               transaction_id_columns = 'tranid',
                               partiton_columns='storeid',
                               max_distinct_items=100)
        # Print the result DataFrame.
        print(CFilter_out2.result)
    MODEL INTERPRETATION functions
    Shap
    Shap
    Functions
    Shap(data=None, object=None, id_column=None, training_function=None, model_type='Regression', input_columns=None, detailed=False, accumulate=None,
    num_parallel_trees=1000, num_boost_rounds=10, **generic_arguments)
    DESCRIPTION:
        Function to get explanation for individual predictions 
        (feature contributions) in a machine learning model based on the 
        co-operative game theory optimal Shapley values.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the input data column name that has the unique identifier
            for each row in the "data".
            Types: str
        training_function:
            Required Argument.
            Specifies the model type name.
            Permitted Values: TD_GLM, TD_DECISIONFOREST, TD_XGBOOST
            Types: str
        model_type:
            Required Argument.
            Specifies the operation to be performed on input data.
            Default Value: "Regression"
            Permitted Values: Regression, Classification
            Types: str
        input_columns:
            Required Argument.
            Specifies the names of the columns in "data" used for 
            training the model (predictors, features or independent variables).
            Types: str OR list of Strings (str)
        detailed:
            Optional Argument.
            Specifies whether to output detailed shap information about the 
            forest trees.
            Note: 
                * It is only supported for "TD_XGBOOST" and "TD_DECISIONFOREST" 
                  training functions.
            Default Value: False
            Types: bool
        accumulate:
            Optional Argument.
            Specifies the names of the input columns to copy to the output teradataml DataFrame.
            Types: str OR list of Strings (str)
        num_parallel_trees:
            Optional Argument.
            Specify the number of parallel boosted trees. Each boosted tree 
            operates on a sample of data that fits in an AMPs memory.
            Note:
                * By default, "num_parallel_trees" is chosen equal to the number of AMPs with 
                  data.
            Default Value: 1000
            Types: int
        num_boost_rounds:
            Optional Argument.
            Specifies the number of iterations to boost the weak classifiers. The 
            iterations must be an int in the range [1, 100000].
            Default Value: 10
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of Shap.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as ShapObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. output
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the 
        #        function in user space.
        #     2. User can import the function, if it is available on 
        #        Vantage user is connected to.
        #     3. To check the list of analytic functions available on 
        #        Vantage user connected to, use 
        #        "display_analytic_functions()".
        # Load the example data.
        load_example_data("byom", "iris_input")
        load_example_data("teradataml", ["cal_housing_ex_raw"])
        # Create teradataml DataFrame objects.
        iris_input = DataFrame("iris_input")
        data_input = DataFrame.from_table("cal_housing_ex_raw")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function Shap.
        from teradataml import Shap, XGBoost, DecisionForest, SVM
        # Example 1: Shap for classification model.
        XGBoost_out = XGBoost(data=iris_input,
                              input_columns=['sepal_length', 'sepal_width', 'petal_length', 'petal_width'],
                              response_column = 'species',
                              model_type='Classification',
                              iter_num=25)
        Shap_out = Shap(data=iris_input, 
                        object=XGBoost_out.result, 
                        id_column='id',
                        training_function="TD_XGBOOST", 
                        model_type="Classification",
                        input_columns=['sepal_length', 'sepal_width', 'petal_length', 'petal_width'], 
                        detailed=True)
        # Print the result DataFrame.
        print(Shap_out.output_data)
        # Example 2: Shap for regression model.
        from teradataml import ScaleFit, ScaleTransform
        # Scale "target_columns" with respect to 'STD' value of the column.
        fit_obj = ScaleFit(data=data_input,
                           target_columns=['MedInc', 'HouseAge', 'AveRooms',
                                            'AveBedrms', 'Population', 'AveOccup',
                                            'Latitude', 'Longitude'],
                           scale_method="STD")
        # Transform the data.
        transform_obj = ScaleTransform(data=data_input,
                                       object=fit_obj.output,
                                       accumulate=["id", "MedHouseVal"])
        decision_forest_out = DecisionForest(data=transform_obj.result,
                                             input_columns=['MedInc', 'HouseAge', 'AveRooms',
                                                            'AveBedrms', 'Population', 'AveOccup',
                                                            'Latitude', 'Longitude'],
                                             response_column="MedHouseVal",
                                             model_type="Regression",
                                             max_depth = 10
                                             )
        Shap_out2 = Shap(data=transform_obj.result, 
                         object=decision_forest_out.result,
                         id_column='id',
                         training_function="TD_DECISIONFOREST",
                         model_type="Regression",
                         input_columns=
    ['MedInc', 'HouseAge', 'AveRooms','AveBedrms', 'Population', 'AveOccup','Latitude', 'Longitude'],
                         detailed=True)
        # Print the result DataFrame.
        print(Shap_out2.output_data)
        # Example 3: Shap for GLM model.
        from teradataml import GLM
        GLM_out = GLM(data=transform_obj.result,
                      input_columns=['MedInc', 'HouseAge', 'AveRooms',
                                     'AveBedrms', 'Population', 'AveOccup',
                                     'Latitude', 'Longitude'],
                      response_column="MedHouseVal",
                      family="GAUSSIAN")
        Shap_out3 = Shap(data=transform_obj.result, 
                         object=GLM_out.result,
                         id_column='id',
                         training_function="TD_GLM",
                         model_type="Regression",
                         input_columns=
    ['MedInc', 'HouseAge', 'AveRooms','AveBedrms', 'Population', 'AveOccup','Latitude', 'Longitude'],
                         detailed=False)
        # Print the result DataFrame.
        print(Shap_out3.output_data)
    MODEL SCORING functions
    DecisionForestPredict
    DecisionForestPredict
    Functions
    DecisionForestPredict(object=None, newdata=None, id_column=None, detailed=False, terms=None, output_prob=False, output_responses=None, **generic_argum
    DESCRIPTION:
        The DecisionForestPredict() function uses the model generated by the
        DecisionForest() function to generate predictions on a response variable for a
        test set of data.
        The model can be stored in either a teradataml DataFrame or a DecisionForest object.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame which contains the model data generated by
            the DecisionForest() function or instance of DecisionForest.
            Types: teradataml DataFrame or DecisionForest
        newdata:
            Required Argument.
            Specifies the name of the teradataml DataFrame containing the
            attribute names and the values.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies a column containing a unique identifier for each test point
            in the test set.
            Types: str
        detailed:
            Optional Argument.
            Specifies whether to output detailed information about the forest
            trees; that is, the decision tree and the specific tree information,
            including task index and tree index for each tree.
            Default Value: False
            Types: bool
        terms:
            Optional Argument.
            Specifies the names of the newdata columns to copy to the output
            teradataml DataFrame.
            Types: str OR list of Strings (str)
        output_prob:
            Optional Argument.
            Specifies whether to output probabilities.
            Default Value: False
            Types: bool
        output_responses:
            Optional Argument.
            Specifies responses for which to output probabilities.
            Types: str OR list of strs
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of DecisionForestPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as DecisionForestPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("byom", "iris_input")
        # Create teradataml DataFrames.
        iris_input = DataFrame("iris_input")
        # Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
        iris_sample = iris_input.sample(frac=[0.8, 0.2])
        # Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required f
        iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
        # Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required fo
        iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Train the data, i.e., create a decision forest Model with "tree_type" as classification.
        formula = "species ~ sepal_length + sepal_width + petal_length + petal_width"
        rft_model = DecisionForest(data=iris_train,
                                   formula = formula,
                                   tree_type="classification",
                                   ntree=50,
                                   tree_size=100,
                                   nodesize=1,
                                   variance=0.0,
                                   max_depth=12,
                                   maxnum_categorical=20,
                                   mtry=3,
                                   mtry_seed=100,
                                   seed=100)
        # Run predict on the output of decision forest.
        decision_forest_predict_out = DecisionForestPredict(object = rft_model,
                                                            newdata = iris_test,
                                                            id_column = "id",
                                                            detailed = False,
                                                            terms = ["species"])
        # Print the result DataFrame.
        print(decision_forest_predict_out.result)
    DecisionTreePredict
    DecisionTreePredict
    Functions
    DecisionTreePredict(object=None, newdata=None, attr_table_groupby_columns=None, attr_table_pid_columns=None, attr_table_val_column=None,
    output_response_probdist=False, accumulate=None, output_responses=None, **generic_arguments)
    DESCRIPTION:
        The DecisionTreePredict() function applies a tree model to a data
        input, outputting predicted labels for each data point.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the name of the teradataml DataFrame containing the output
            model from DecisionTree() function or instance of DecisionTree.
            Types: teradataml DataFrame or DecisionTree
        newdata:
            Required Argument.
            Specifies the name of the teradataml DataFrame containing the
            attribute names and the values.
            Types: teradataml DataFrame
        attr_table_groupby_columns:
            Required Argument.
            Specifies the names of the columns on which newdata is
            partitioned. Each partition contains one attribute of the input data.
            Types: str OR list of Strings (str)
        attr_table_pid_columns:
            Required Argument.
            Specifies the names of the columns that define the data point
            identifiers.
            Types: str OR list of Strings (str)
        attr_table_val_column:
            Required Argument.
            Specifies the name of the column that contains the input values.
            Types: str
        output_response_probdist:
            Optional Argument.
            Specifies whether to output probabilities.
            Note: "output_response_probdist" argument can accept input value True
                  only when teradataml is connected to Vantage 1.0 Maintenance
                  Update 2 version or later.
            Default Value: False
            Types: bool
        accumulate:
            Optional Argument.
            Specifies the name(s) of the input teradataml DataFrame column(s) to
            copy to the output.
            Types: str OR list of Strings (str)
        output_responses:
            Optional Argument.
            Required if "output_response_probdist" is True.
            Specifies all responses in newdata.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of DecisionTreePredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as DecisionTreePredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #    1. Get the connection to Vantage, before importing the function in user space.
        #    2. User can import the function, if it is available on the Vantage user is connected to.
        #    3. To check the list of analytic functions available on the Vantage user connected to,
        #       use "display_analytic_functions()"
        # Load the example data.
        load_example_data("DecisionTreePredict", [ "iris_attribute_test", "iris_attribute_output"])
        # Create teradataml DataFrame objects.
        iris_attribute_test = DataFrame.from_table("iris_attribute_test")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function DecisionTreePredict.
        from teradataml import DecisionTreePredict
        # Example 1: First create dataframe of trained Decision Tree Model and then
        #            perform prediction using "DecisionTreePredict()" function.
        # Create dataframe of trained Decision Tree Model.
        td_decision_tree_out  = DataFrame("iris_attribute_output")
        # Run predict on the trained decision tree model.
        decision_tree_predict_out = DecisionTreePredict(newdata=iris_attribute_test,
                                                        newdata_partition_column='pid',
                                                        object=td_decision_tree_out,
                                                        attr_table_groupby_columns='attribute',
                                                        attr_table_pid_columns='pid',
                                                        attr_table_val_column='attrvalue',
                                                        accumulate='attribute')
        # Print output DataFrame.
        print(decision_tree_predict_out.result)
    GLMPredict
    GLMPredict
    Functions
    GLMPredict(modeldata=None, newdata=None, terms=None, family=None, linkfunction='CANONICAL', output_prob=False, output_responses=None,
    **generic_arguments)
    DESCRIPTION:
    The GLMPredict() function uses the model generated by the function GLM()
    to perform generalized linear model prediction on new input data.
    Note:
        * The GLMPredict() function accepts models from the GLM() function in MLE,
          whereas the TDGLMPredict() function accepts models from GLM() function in SQLE.
    PARAMETERS:
        modeldata:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data
            generated by GLM() function or an instance of GLM.
            Types: teradataml DataFrame or GLM
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
            Types: teradataml DataFrame
        terms:
            Optional Argument.
            Specifies the names of newdata columns to copy to the output
            teradataml DataFrame.
            Types: str OR list of Strings (str)
        family:
            Optional Argument.
            Specifies the distribution exponential family. The default value is
            read from modeldata. If you specify this argument, you must give it
            the same value that you used for the Family argument of the function
            when you generated the model.
            Permitted Values: LOGISTIC, BINOMIAL, POISSON, GAUSSIAN, GAMMA,
            INVERSE_GAUSSIAN, NEGATIVE_BINOMIAL
            Types: str
        linkfunction:
            Optional Argument.
            The canonical link functions (default link functions) and the link
            functions that are allowed for each exponential family.
            Note:
                Use the same value that you used for the Link argument of the
                function  when you generated the model.
            Default Value: "CANONICAL"
            Permitted Values: CANONICAL, IDENTITY, INVERSE, LOG,
            COMPLEMENTARY_LOG_LOG, SQUARE_ROOT, INVERSE_MU_SQUARED, LOGIT,
            PROBIT, CAUCHIT
            Types: str
        output_prob:
            Optional Argument.
            Specifies whether to output probabilities.
            Default Value: False
            Types: bool
        output_responses:
            Optional Argument.
            Specifies responses for which to output probabilities.
            Types: str OR list of strs
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of GLMPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as GLMPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the data to run the example.
        load_example_data("GLMPredict", "admissions_test")
        load_example_data("teradataml", "glm_admissions_model")
        # Create teradataml DataFrame objects.
        admissions_test = DataFrame.from_table("admissions_test")
        # Create teradataml DataFrame object for model data.
        glm_admissions_model = DataFrame.from_table("glm_admissions_model")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Predicting if student is admitted or not with probabilities of
        #            the both the responses. Response '1' means admitted and '0' means
        #            not admitted.
        obj = teradataml.GLMPredict(modeldata = glm_admissions_model,
                                    newdata = admissions_test,
                                    newdata_order_column="id",
                                    modeldata_order_column=["attribute", "predictor"],
                                    family="LOGISTIC",
                                    terms=["id", "masters", "gpa", "stats", "programming", "admitted"],
                                    linkfunction="CANONICAL",
                                    output_prob=True,
                                    output_responses=["1", "0"])
        # Print the result DataFrame.
        print(obj.result)
    GLMPredictPerSegment
    GLMPredictPerSegment
    Functions
    GLMPredictPerSegment(newdata=None, object=None, id_column=None, accumulate=None, output_prob=False, output_responses=None,
    partition_column=None, **generic_arguments)
    DESCRIPTION:
        The GLMPredictPerSegment() function uses the model generated by
        the GLMPerSegment() function to predict target values (regression)
        and class labels (classification) on new input data.
        Notes:
            * All input features must be numeric. The categorical columns should be converted
              to numerical columns as preprocessing step, such as using the following functions:
                  * OneHotEncodingFit() and OneHotEncodingTransform().
                  * OrdinalEncodingFit() and OrdinalEncodingTransform().
                  * TargetEncodingFit() and TargetEncodingTransform().
              The OneHotEncodingFit() and OneHotEncodingTransform() functions support segment
              functions. However, the OrdinalEncodingFit() and OrdinalEncodingTransform(),
              TargetEncodingFit() and TargetEncodingTransform() functions do not support segment
              functions. You must run these functions one-by-one on each partition.
            * The preprocessing steps carried out for GLMPerSegment() should be done for
              the test data set as well before prediction.
            * Prediction accuracy metrics such as MSE, precision, recall, ROC are not
              generated by the function. The user should use RegressionEvaluator(),
              ClassificationEvaluator() and ROC() functions as post-processing steps.
              These functions do not support segment functions and the workaround is to run
              these functions one by one on each partition.
            * Any observation with missing value in an input column is ignored and it shows
              in the output with specific error code. User can use some imputation function,
              such as SimpleImputeFit() and SimpleImputeTransform() to do imputation or filling
              of missing values.
    PARAMETERS:
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data
            generated by GLMPerSegment() function or an instance of GLMPerSegment.
            Types: teradataml DataFrame or GLMPerSegment
        id_column:
            Required Argument.
            Specifies the input data column name that uniquely
            identifies an observation in the input data.
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output.
            Types: str OR list of Strings (str)
        output_prob:
            Optional Argument.
            Specifies whether function should output the probability for each response.
            Default Value: False
            Types: bool
        output_responses:
            Optional Argument.
            Specifies the class labels for output probabilities. A label must be 0 or 1.
            If not specified, then the function outputs the probability of the predicted response.
            Note:
                * The "output_responses" argument works only when "output_prob"
                  is True.
            Types: str OR list of Strings (str)
        partition_column:
            Optional Argument.
            Specifies the name of the "input_data" columns on which to partition the input.
            The name should be consistent with the "data_partition_column" name. If the
            "data_partition_column" is unicode with foreign language characters, then it is
            necessary to specify "partition_column" argument.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQLE Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of GLMPredictPerSegment.
        Output teradataml DataFrames can be accessed using attribute
        references, such as GLMPredictPerSegmentObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("decisionforestpredict", ["housing_train", "housing_test"])
        # Create teradataml DataFrame objects.
        housing_train = DataFrame.from_table("housing_train")
        housing_test = DataFrame.from_table("housing_test")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Filter the rows from train dataset with homestyle as Classic and Eclectic.
        binomial_housing_train = DataFrame('housing_train', index_label="homestyle")
        binomial_housing_train = binomial_housing_train.filter(like = 'ic', axis = 'rows')
        binomial_housing_test = DataFrame('housing_test', index_label="homestyle")
        binomial_housing_test = binomial_housing_test.filter(like = 'ic', axis = 'rows')
        # GLMPerSegment() function requires features in numeric format for processing,
        # so dropping the non-numeric columns.
        binomial_housing_train = binomial_housing_train.drop(columns=["driveway", "recroom",
                                                                      "gashw", "airco", "prefarea",
                                                                      "fullbase"])
        gaussian_housing_train = binomial_housing_train.drop(columns="homestyle")
        binomial_housing_test = binomial_housing_test.drop(columns=["driveway", "recroom",
                                                                    "gashw", "airco", "prefarea",
                                                                    "fullbase"])
        gaussian_housing_test = binomial_housing_test.drop(columns="homestyle")
        # Transform the train dataset categorical values to encoded values.
        housing_train_ordinal_encodingfit = OrdinalEncodingFit(
            target_column='homestyle',
            data=binomial_housing_train)
        housing_train_ordinal_encodingtransform = OrdinalEncodingTransform(
            data=binomial_housing_train,
            object=housing_train_ordinal_encodingfit.result,
            accumulate=["sn", "price", "lotsize",
                        "bedrooms", "bathrms", "stories"])
        housing_test_ordinal_encodingfit = OrdinalEncodingFit(
            target_column='homestyle',
            data=binomial_housing_test)
        housing_test_ordinal_encodingtransform = OrdinalEncodingTransform(
            data=binomial_housing_test,
            object=housing_test_ordinal_encodingfit.result,
            accumulate=["sn", "price", "lotsize",
                        "bedrooms", "bathrms", "stories"])
        # Example 1: Train the model using the 'Gaussian' family.
        #            Predict the price using GLMPredictPerSegment().
        # Train the model using the 'Gaussian' family.
        GLMPerSegment_out_1 = GLMPerSegment(data=gaussian_housing_train,
                                            data_partition_column="stories",
                                            input_columns=['garagepl', 'lotsize', 'bedrooms', 'bathrms'],
                                            response_column="price",
                                            family="Gaussian",
                                            iter_max=1000,
                                            batch_size=9)
        # Predict the price using GLMPredictPerSegment().
        GLMPredictPerSegment_out_1 = GLMPredictPerSegment(newdata=gaussian_housing_test,
                                                          newdata_partition_column="stories",
                                                          object=GLMPerSegment_out_1,
                                                          object_partition_column="stories",
                                                          id_column="sn")
        # Print the result DataFrame.
        print(GLMPredictPerSegment_out_1.result)
        # Example 2: Train the model using the 'Binomial' family.
        #            Predict the homestyle using GLMPredictPerSegment().
        # Train the model using the 'Binomial' family.
        GLMPerSegment_out_2 = GLMPerSegment(data=housing_train_ordinal_encodingtransform.result,
                                            data_partition_column="stories",
                                            input_columns=['price', 'lotsize', 'bedrooms', 'bathrms'],
                                            response_column="homestyle",
                                            family="Binomial",
                                            iter_max=100)
        # Predict the homestyle using GLMPredictPerSegment().
        GLMPredictPerSegment_out_2 = GLMPredictPerSegment(
            newdata=housing_test_ordinal_encodingtransform.result,
            newdata_partition_column="stories",
            object=GLMPerSegment_out_2,
            object_partition_column="stories",
            id_column="sn",
            output_prob=True,
            output_responses=["0", "1"]
            )
        # Print the result DataFrame.
        print(GLMPredictPerSegment_out_2.result)
    KMeansPredict
    KMeansPredict
    Functions
    KMeansPredict(data=None, object=None, accumulate=None, output_distance=False, **generic_arguments)
    DESCRIPTION:
        The KMeansPredict() function uses the cluster centroids in
        the KMeans() function output to assign the input data points
        to the cluster centroids.
        Notes:
            * This function requires the UTF8 client character
              set for UNICODE data.
            * This function does not support Pass Through
              Characters (PTCs).
            * For information about PTCs, see Teradata Vantage™ -
              Analytics Database International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame generated by
            KMeans() function or the instance of KMeans.
            Types: teradataml DataFrame or KMeans
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output. By default, the function copies no input teradataml
            DataFrame columns to the output.
            Types: str OR list of Strings (str)
        output_distance:
            Optional Argument.
            Specifies whether to return the distance between
            each data point and the nearest cluster.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of KMeansPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as KMeansPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("kmeans", "computers_train1")
        # Create teradataml DataFrame objects.
        computers_train1 = DataFrame.from_table("computers_train1")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Grouping a set of observations into 2 clusters in which
        # each observation belongs to the cluster with the nearest mean.
        KMeans_out = KMeans(id_column="id",
                            target_columns=['price', 'speed'],
                            data=computers_train1,
                            num_clusters=2)
        # Print the result DataFrames.
        print(KMeans_out.result)
        print(KMeans_out.model_data)
        # Example 1 : Assign the input data points to the cluster centroid
        #             using the model generated by the KMeans() function.
        #             Note that teradataml DataFrame representing the model
        #             is passed as input to "object".
        KMeansPredict_out = KMeansPredict(object=KMeans_out.result,
                                          data=computers_train1)
        # Print the result DataFrames.
        print(KMeansPredict_out.result)
        # Example 2 : Assign the input data points to the cluster centroid
        #             using the model generated by the KMeans() function.
        #             Note that model is passed as instance of KMeans to "object".
        KMeansPredict_out_1 = KMeansPredict(data=computers_train1,
                                            object=KMeans_out,
                                            accumulate="ram",
                                            output_distance=False)
        # Print the result DataFrames.
        print(KMeansPredict_out_1.result)
    NaiveBayesPredict
    NaiveBayesPredict
    Functions
    NaiveBayesPredict(modeldata=None, newdata=None, id_col=None, responses=None, formula=None, **generic_arguments)
    DESCRIPTION:
        NaiveBayesPredict() function predicts the outcomes for a test set of data
        using the model output generated by the NaiveBayes() function.
    PARAMETERS:
        modeldata:
            Required Argument.
            Specifies the teradataml DataFrame which contains the model
            data generated by the NaiveBayes() function or instance of NaiveBayes.
            Types: teradataml DataFrame or NaiveBayes
        newdata:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        id_col:
            Required Argument.
            Specifies the name of the column that contains the ID that uniquely
            identifies the test input data.
            Types: str
        responses:
            Required Argument.
            Specifies a list of Responses to output.
            Types: str OR list of Strings (str)
        formula:
            Optional Argument.
            Required when the modeldata is a teradataml DataFrame.
            A string consisting of "formula".
            Specifies the model to be fitted. Only basic formula of the
            "col1 ~ col2 + col3 +..." form is supported and all variables
            must be from the same virtual data frame object. The response
            should be column of type real, numeric, integer or boolean.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments which SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in table or not.
                    When set to True, results are persisted in table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table or not.
                    When set to True, results are stored in volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQLE Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of NaiveBayesPredict.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as NaiveBayesPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #    1. Get the connection to Vantage, before importing the function in user space.
        #    2. User can import the function, if it is available on the Vantage user is connected to.
        #    3. To check the list of analytic functions available on the Vantage user connected to,
        #       use "display_analytic_functions()"
        # Load the data to run the example.
        load_example_data("teradataml", ["nb_iris_input_test", "nbp_iris_model"])
        # Create teradataml DataFrame objects.
        nb_iris_input_test = DataFrame.from_table("nb_iris_input_test")
        nbp_iris_model = DataFrame.from_table("nbp_iris_model")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function NaiveBayesPredict.
        from teradataml import NaiveBayesPredict
        # Example 1: Predict the 'species' on the test data nb_iris_input_test
        #            using the NaiveBayes model nbp_iris_model.
        naive_bayes_predict = teradataml.NaiveBayesPredict(newdata=nb_iris_input_test,
                                                           modeldata=nbp_iris_model,
                                                           id_col="id",
                                                           responses=["virginica", "setosa", "versicolor"],
                                                           formula="species ~ petal_length + sepal_width + petal_width + s
                                                           )
        # Print the result DataFrame.
        print(naive_bayes_predict.result)
    OneClassSVMPredict
    OneClassSVMPredict
    Functions
    OneClassSVMPredict(object=None, newdata=None, id_column=None, accumulate=None, output_prob=False, output_responses=None, **generic_arguments)
    DESCRIPTION:
        The OneClassSVMPredict() function uses the model generated
        by the function OneClassSVM() to predicts target class labels
        (classification) on new input data. Output values are 0 and 1.
        A value of 1 corresponds to a 'normal' observation, and a value
        of 0 is assigned to 'outlier' observations.
        Notes:
            * Standardize input features using ScaleFit() and ScaleTransform()
              before using the function.
            * The function takes only numeric features.
            * The categorical features must be converted to numeric values prior to prediction.
            * Rows with missing (null) values are skipped by the function during prediction.
            * For prediction results evaluation, user can use ClassificationEvaluator() or
              ROC() as the postprocessing step.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data
            generated by OneClassSVM() function or an instance of OneClassSVM.
            Types: teradataml DataFrame or OneClassSVM
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the name of the column that uniquely identifies an
            observation in the test data.
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output.
            Types: str OR list of Strings (str)
        output_prob:
            Optional Argument.
            Specifies whether the function should output the probability for each
            response.
            Note:
                * Only applicable if "model_type" is 'Classification'.
            Default Value: False
            Types: bool
        output_responses:
            Optional Argument.
            Specifies the class labels for which to output probabilities.
            A label must be 0 or 1. If not specified, the function outputs the
            probability of the predicted response.
            Note:
                * Only applicable if "output_prob" is 'True'.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of OneClassSVMPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as OneClassSVMPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["cal_housing_ex_raw"])
        # Create teradataml DataFrame objects.
        data_input = DataFrame.from_table("cal_housing_ex_raw")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Scale "target_columns" with respect to 'STD' value of the column.
        fit_obj = ScaleFit(data=data_input,
                           target_columns=['MedInc', 'HouseAge', 'AveRooms',
                                           'AveBedrms', 'Population', 'AveOccup',
                                           'Latitude', 'Longitude'],
                           scale_method="STD")
        # Transform the data.
        transform_obj = ScaleTransform(data=data_input,
                                       object=fit_obj.output,
                                       accumulate=["id", "MedHouseVal"])
        # Train the input data by OneClassSVM which helps model
        # to find anomalies in transformed data.
        one_class_svm=OneClassSVM(data=transform_obj.result,
                                  input_columns=['MedInc', 'HouseAge', 'AveRooms',
                                                 'AveBedrms', 'Population', 'AveOccup',
                                                 'Latitude', 'Longitude'],
                                  local_sgd_iterations=537,
                                  batch_size=1,
                                  learning_rate='constant',
                                  initial_eta=0.01,
                                  lambda1=0.1,
                                  alpha=0.0,
                                  momentum=0.0,
                                  iter_max=1
                                  )
        # Example 1 : Using trained data by OneClassSVM model, predict whether observation
        #             is outlier or normal in the form of '0' or '1' on "newdata".
        OneClassSVMPredict_out1 = OneClassSVMPredict(object = one_class_svm.result,
                                                     newdata = transform_obj.result,
                                                     id_column = "id"
                                                     )
        # Print the result DataFrame.
        print(OneClassSVMPredict_out1.result)
        # Example 2 : Using trained data by OneClassSVM model, predict whether observation
        #             is outlier or normal  in the form of '0' or '1' also provides probability
        #             of outcome of '0' and '1' on "newdata".
        OneClassSVMPredict_out2 = OneClassSVMPredict(object = one_class_svm,
                                                     newdata = transform_obj.result,
                                                     id_column = "id",
                                                     accumulate="MedInc",
                                                     output_prob=True,
                                                     output_responses=["0", "1"]
                                                     )
        # Print the result DataFrame.
        print(OneClassSVMPredict_out2.result)
    SVMPredict
    SVMPredict
    Functions
    SVMPredict(object=None, newdata=None, id_column=None, accumulate=None, output_prob=False, output_responses=None, model_type='Classiﬁcation',
    **generic_arguments)
    DESCRIPTION:
        The SVMPredict() function uses the model generated by the function SVM() to
        predict target values (regression) and class labels (classification) on new
        input data.
        Notes:
            * Standardize input features using ScaleFit() and ScaleTransform()
              before using the function.
            * The function takes only numeric features. The categorical features
              must be converted to numeric values prior to prediction.
            * Rows with missing (NULL) values are skipped by the function during prediction.
            * For prediction results evaluation, user can use RegressionEvaluator(),
              ClassificationEvaluator() or ROC() function as postprocessing step.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the
            model data generated by SVM() function or the instance
            of SVM.
            Types: teradataml DataFrame or SVM
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the name of the column that uniquely identifies an
            observation in the test data.
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to
            copy to the output.
            Types: str OR list of Strings (str)
        output_prob:
            Optional Argument.
            Specifies whether the function should output the probability for each
            response.
            Note:
                Only applicable when "model_type" is 'CLASSIFICATION'.
            Default Value: False
            Types: bool
        output_responses:
            Optional Argument.
            Specifies the class labels to output probabilities. A label must be 0 or 1.
            If not specified, the function outputs the probability of the predicted response.
            Note:
                Only applicable when "output_prob" is 'True'.
            Types: str OR list of strs
        model_type:
            Optional Argument.
            Specifies the type of the analysis.
            Note:
                * Required for Regression problem.
            Permitted Values: 'Classification', 'Regression'
            Default Value: 'Classification'
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in table or not. When set to True,
                    results are persisted in table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in volatile table or not. When set to
                    True, results are stored in volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQLE Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of SVMPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SVMPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["cal_housing_ex_raw"])
        # Create teradataml DataFrame objects.
        data_input = DataFrame.from_table("cal_housing_ex_raw")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Scale "target_columns" with respect to 'STD' value of the column.
        fit_obj = ScaleFit(data=data_input,
                           target_columns=['MedInc', 'HouseAge', 'AveRooms',
                                           'AveBedrms', 'Population', 'AveOccup',
                                           'Latitude', 'Longitude'],
                           scale_method="STD")
        # Transform the data.
        transform_obj = ScaleTransform(data=data_input,
                                       object=fit_obj.output,
                                       accumulate=["id", "MedHouseVal"])
        # Example 1 : Predict target values (Regression) for test data using an SVM model
        #             trained by SVM().
        # Train the transformed data using SVM() where "model_type" is 'Regression'.
        svm_obj1 = SVM(data=transform_obj.result,
                      input_columns=['MedInc', 'HouseAge', 'AveRooms',
                                     'AveBedrms', 'Population', 'AveOccup',
                                     'Latitude', 'Longitude'],
                      response_column="MedHouseVal",
                      model_type="Regression"
                      )
        # SVMPredict() predicts target values using regression model by SVM().
        SVMPredict_out1 = SVMPredict(newdata=transform_obj.result,
                                     object=svm_obj1.result,
                                     id_column="id",
                                     accumulate="MedHouseVal",
                                     model_type="Regression"
                                     )
        # Print the result DataFrame.
        print(SVMPredict_out1.result)
        # Example 2 : Predict target values (Classification) for test data using an SVM model
        #             trained by SVM().
        # Train the transformed data using SVM() where "model_type" is 'Classification'.
        svm_obj2 = SVM(data=transform_obj.result,
                      input_columns=['MedInc', 'HouseAge', 'AveRooms',
                                     'AveBedrms', 'Population', 'AveOccup',
                                     'Latitude', 'Longitude'],
                      response_column="MedHouseVal",
                      model_type="Classification",
                      batch_size=12,
                      iter_max=301,
                      lambda1=0.1,
                      alpha=0.5,
                      iter_num_no_change=60,
                      tolerance=0.01,
                      intercept=False,
                      class_weights="0:1.0,1:0.5",
                      learning_rate="INVTIME",
                      initial_data=0.5,
                      decay_rate=0.5,
                      momentum=0.6,
                      nesterov_optimization=True,
                      local_sgd_iterations=1,
                      )
        # SVMPredict() predicts target values using classification model by SVM() and
        # instance of SVM passed to SVMPredict.
        SVMPredict_out2 = SVMPredict(newdata=transform_obj.result,
                                     object=svm_obj2,
                                     id_column="id",
                                     accumulate="MedHouseVal",
                                     output_prob=True,
                                     output_responses=["0", "1"]
                                     )
        # Print the result DataFrame.
        print(SVMPredict_out2.result)
    SVMSparsePredict
    SVMSparsePredict
    Functions
    SVMSparsePredict(object=None, newdata=None, sample_id_column=None, attribute_column=None, value_column=None, accumulate_label=None,
    output_class_num=1, output_prob=True, output_responses=None, **generic_arguments)
    DESCRIPTION:
    The SVMSparsePredict() function takes the model generated by the
    SVMSparse() trainer function and a set of test samples (in sparse
    format) and outputs a prediction for each sample.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data
            generated by SVMSparse() function or instance of SVMSparse.
            Types: teradataml DataFrame or SVMSparse
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input test data.
            Types: teradataml DataFrame
        sample_id_column:
            Required Argument.
            Specifies the name of the newdata column that contains the
            identifiers of the test samples. The newdata table must be
            partitioned by this column.
            Types: str
        attribute_column:
            Required Argument.
            Specifies the name of the newdata column that contains the
            attributes of the test samples.
            Types: str
        value_column:
            Optional Argument.
            Specifies the name of the newdata column that contains the
            attribute values. By default, each attribute has the value 1.
            Types: str
        accumulate_label:
            Optional Argument.
            Specifies the names of the newdata columns to copy to the
            output teradataml DataFrame.
            Types: str OR list of Strings (str)
        output_class_num:
            Optional Argument.
            Specifies the number of class labels to appear in the output
            teradataml DataFrame, with its corresponding prediction confidence.
            Valid only for multiple-class models.
            Default Value: 1
            Types: int
        output_prob:
            Optional Argument.
            Specifies whether to display output probability for the predicted
            category.
            Default Value: True
            Types: bool
        output_responses:
            Optional Argument.
            Specifies responses for which to output probabilities.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of SVMSparsePredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SVMSparsePredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the data to run the example.
        load_example_data("SVMSparsePredict",["svm_iris_input_test"])
        load_example_data("teradataml", "svm_iris_model")
        # Create teradataml DataFrame
        svm_train = DataFrame.from_table("svm_iris_model")
        svm_iris_input_test = DataFrame.from_table("svm_iris_input_test")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: teradataml DataFrame containing the model data is passed as input
        #            to object argument.
        svm_sparse_predict_result1 = teradataml.SVMSparsePredict(newdata=svm_iris_input_test,
                                                                 object=svm_train,
                                                                 attribute_column='attribute',
                                                                 sample_id_column='id',
                                                                 value_column='value1',
                                                                 accumulate_label='species')
        # Print the result DataFrame
        print(svm_sparse_predict_result1.result)
        # Example 2: Predict the species and display probability for "setosa" and "virginica".
        svm_sparse_predict_result2 = teradataml.SVMSparsePredict(newdata=svm_iris_input_test,
                                                                 object=svm_train,
                                                                 attribute_column='attribute',
                                                                 sample_id_column='id',
                                                                 value_column='value1',
                                                                 accumulate_label='species',
                                                                 output_prob= True,
                                                                 output_responses= ['setosa', 'virginica'])
        # Print the result DataFrame.
        print(svm_sparse_predict_result2.result)
    SVMSparsePredict
    TDDecisionForestPredict
    Functions
    TDDecisionForestPredict(newdata=None, object=None, id_column=None, detailed=False, output_prob=False, output_responses=None, accumulate=None,
    **generic_arguments)
    DESCRIPTION:
        TDDecisionForestPredict() function uses the model output by DecisionForest()
        function to analyze the input data and make predictions. This function outputs
        the probability that each observation is in the predicted class.
        Processing times are controlled by the number of trees in the model.
        When the number of trees is more than what can fit in memory, then
        the trees are cached in a local spool space.
    PARAMETERS:
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame which contains the model data generated by
            the DecisionForest() function or instance of DecisionForest.
            Types: teradataml DataFrame or DecisionForest
        id_column:
            Required Argument.
            Specifies a column containing a unique identifier for each test point
            in the test set.
            Types: str
        detailed:
            Optional Argument.
            Specifies whether to output detailed information about the decision
            tree and the specific tree information, including task index and
            tree index for each tree.
            Default Value: False
            Types: bool
        output_prob:
            Optional Argument.
            Specifies whether to output probability for each response.
            Notes:
                * If "output_prob" is false the function outputs only
                  the probability of the predicted class.
                * The "output_prob" must be true when using "output_responses".
                * The "output_prob" only works with a classification model.
            Default Value: False
            Types: bool
        output_responses:
            Optional Argument.
            Specifies the classes to output the probabilities. By default it
            output only the probability of the predicted class.
            Note:
                * The "output_responses" only works with a classification model.
            Types: str OR list of str(s)
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output. By default, the function copies no input teradataml DataFrame columns
            to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQLE Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of TDDecisionForestPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as TDDecisionForestPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("decisionforest", ["boston"])
        load_example_data("byom", "iris_input")
        # Create teradataml DataFrame objects.
        boston = DataFrame.from_table("boston")
        iris_input = DataFrame("iris_input")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : This example takes boston data as input, and generates the Regression
        #             model using DecisionForest(). Using TDDecisionForestPredict() function
        #             to predict the medv with the Regression model generated by XGBoost().
        # Create 2 samples of input data - sample 1 will have 80% of total rows and
        # sample 2 will have 20% of total rows.
        boston_sample = boston.sample(frac=[0.8, 0.2])
        # Create train dataset from sample 1 by filtering on "sampleid" and drop
        # "sampleid" column as it is not required for training model.
        boston_train = boston_sample[boston_sample.sampleid == "1"].drop("sampleid", axis = 1)
        # Create test dataset from sample 2 by filtering on "sampleid" and
        # drop "sampleid" column as it is not required for scoring.
        boston_test = boston_sample[boston_sample.sampleid == "2"].drop("sampleid", axis = 1)
        # Training the model.
        DecisionForest_out = DecisionForest(data = boston_train,
                                            input_columns = ['crim', 'zn', 'indus', 'chas', 'nox', 'rm',
                                                            'age', 'dis', 'rad', 'tax', 'ptratio', 'black',
                                                            'lstat'],
                                            response_column = 'medv',
                                            max_depth = 12,
                                            num_trees = 4,
                                            min_node_size = 1,
                                            mtry = 3,
                                            mtry_seed = 1,
                                            seed = 1,
                                            tree_type = 'REGRESSION')
        # TDDecisionForestPredict() predicts the result using generated Regression model by
        # DecisionForest() and "newdata".
        TDDecisionForestPredict_out = TDDecisionForestPredict(newdata=boston_test,
                                                              object=DecisionForest_out,
                                                              id_column="id")
        # Print the result DataFrame.
        print(TDDecisionForestPredict_out.result)
        # Example 2 : This example takes iris_input data, and generates the Classification
        #             model using DecisionForest(). Using TDDecisionForestPredict() function
        #             to predict the species with the Classification model generated by XGBoost().
        #             Provides the classes for which to output the probabilities.
        # Create 2 samples of input data - sample 1 will have 80% of total rows and
        # sample 2 will have 20% of total rows.
        iris_sample = iris_input.sample(frac=[0.8, 0.2])
        # Create train dataset from sample 1 by filtering on "sampleid" and drop
        # "sampleid" column as it is not required for training model.
        iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
        # Create test dataset from sample 2 by filtering on "sampleid" and
        # drop "sampleid" column as it is not required for scoring.
        iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
        # Training the model.
        DecisionForest_out_2 = DecisionForest(data=iris_train,
                                              input_columns=
    ['sepal_length', 'sepal_width', 'petal_length', 'petal_width'],
                                              response_column="species",
                                              tree_type="CLASSIFICATION")
        # TDDecisionForestPredict() predicts the result using generated Regression model by
        # DecisionForest() and "newdata".
        TDDecisionForestPredict_out_2 = TDDecisionForestPredict(newdata=iris_test,
                                                                object=DecisionForest_out_2,
                                                                id_column="id",
                                                                output_prob=True,
                                                                output_responses=['1', '2', '3'])
        # Print the result DataFrame.
        print(TDDecisionForestPredict_out_2.result)
    TDGLMPredict
    TDGLMPredict
    Functions
    TDGLMPredict(object=None, newdata=None, id_column=None, accumulate=None, output_prob=False, output_responses=None, partition_column=None,
    family='GAUSSIAN', **generic_arguments)
    DESCRIPTION:
        The TDGLMPredict() function predicts target values (regression) and class labels
        (classification) for test data using a model generated by the GLM().
        Notes:
            * Before using the features in the function, user must standardize the Input features
                using ScaleFit() and ScaleTransform() functions.
            * The function only accepts numeric features. Therefore, user must convert the categorical
                features to numeric values before prediction.
            * The function skips the rows with missing (null) values during prediction.
            * User can use RegressionEvaluator(), ClassificationEvaluator(), or ROC() function as a
                post-processing step for evaluating prediction results.
            * The TDGLMPredict() function accepts models from GLM() function in SQLE.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data generated by GLM()
            function or the instance of GLM.
            Types: teradataml DataFrame or GLM
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the name of the column that uniquely identifies an
            observation in the test data.
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output. By default, the function copies no input teradataml DataFrame columns
            to the output.
            Types: str OR list of Strings (str)
        output_prob:
            Optional Argument.
            Specifies whether the function should output the probability for each
            response.
            Note:
                Only applicable if the "family" is 'Binomial'.
            Default Value: False
            Types: bool
        output_responses:
            Optional Argument.
            Specifies the class labels for which to output probabilities.
            A label must be 0 or 1. If not specified, the function outputs the
            probability of the predicted response.
            Note:
                Only applicable if "output_prob" is True.
            Types: str OR list of strs
        partition_column:
            Optional Argument.
            Specifies the column names of "data" on which to partition the input.
            Types: str OR list of Strings (str)
        family:
            Optional Argument.
            Specifies the distribution exponential family.
            Permitted Values: 'GAUSSIAN', 'BINOMIAL'
            Default Value: 'GAUSSIAN'
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQLE Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of TDGLMPredict.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as TDGLMPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "cal_housing_ex_raw")
        # Create teradataml DataFrame objects.
        df=DataFrame.from_table("cal_housing_ex_raw")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: This example takes raw housing data, and does the following
        #            Uses ScaleFit() to standardize the data.
        #            Uses ScaleTransform() to transform the data.
        #            Uses GLM() to generate a model.
        #            Uses TDGLMPredict() to predict target values.
        # Scale "target_columns" with respect to 'STD' value of the column.
        fit_obj = ScaleFit(data=df,
                           target_columns=['MedInc', 'HouseAge', 'AveRooms', 'AveBedrms', 'Population', 'AveOccup',
                           'Latitude', 'Longitude'],
                           scale_method="STD")
        # Scale values specified in the input data using the fit data generated by the Scale() function above.
        obj = ScaleTransform(object=fit_obj.output,
                             data=df,
                             accumulate=["id","MedHouseVal"])
        # Generate regression model using generalized linear model(GLM).
        answer = GLM(input_columns=["MedInc", "HouseAge", "AveRooms", "AveBedrms", "Population", "AveOccup",
                    "Latitude", "Longitude"],
                    response_column="MedHouseVal",
                    data=obj.result,
                    nesterov=False)
        # TDGLMPredict() predicts 'MedHouseVal' using generated regression model by GLM and newdata.
        # Note that teradataml DataFrame representing the model is passed as input to "object".
        TDGLMPredict_out = TDGLMPredict(newdata=obj.result,
                                        object=answer.result,
                                        accumulate="MedHouseVal",
                                        id_column="id")
        # Print the result DataFrame.
        print(TDGLMPredict_out.result)
        # Example 2: TDGLMPredict() predicts the 'MedHouseVal' using generated regression model
        #            by GLM and newdata. Note that model is passed as instance of GLM to "object".
        TDGLMPredict_out1 = TDGLMPredict(newdata=obj.result,
                                         object=answer,
                                         accumulate="MedHouseVal",
                                         id_column="id")
        # Print the result DataFrame.
        print(TDGLMPredict_out1.result)
        # Example 3 : TDGLMPredict() predicts the 'medv' using generated regression model by GLM
        #             using stepwise regression algorithm.   
        #             This example uses the boston dataset and scales the data.
        #             Scaled data is used as input data to generate the GLM model and predict the target values.
        # loading the example data
        load_example_data("decisionforest", ["boston"])
        load_example_data('glm', ['housing_train_segment', 'housing_train_parameter', 'housing_train_attribute'])
        # Create teradataml DataFrame objects.
        boston_df = DataFrame('boston')
        housing_seg = DataFrame('housing_train_segment')
        housing_parameter = DataFrame('housing_train_parameter')
        housing_attribute = DataFrame('housing_train_attribute')
        # scaling the data
        # Scale "target_columns" with respect to 'STD' value of the column.
        fit_obj = ScaleFit(data=boston_df,
                        target_columns=
    ['crim','zn','indus','chas','nox','rm','age','dis','rad','tax','ptratio','black','lstat',],
                        scale_method="STD")
        # Scale values specified in the input data using the fit data generated by the ScaleFit() function above.
        obj = ScaleTransform(object=fit_obj.output,
                            data=boston_df,
                            accumulate=["id","medv"])
        boston = obj.result
        # Generate generalized linear model(GLM) using stepwise regression algorithm.
        glm_1 = GLM(data=boston,
                    input_columns=['indus','chas','nox','rm'],
                    response_column='medv',
                    family='GAUSSIAN',
                    lambda1=0.02,
                    alpha=0.33,
                    batch_size=10,
                    learning_rate='optimal',
                    iter_max=36,
                    iter_num_no_change=100,
                    tolerance=0.0001,
                    initial_eta=0.02,
                    stepwise_direction='backward',
                    max_steps_num=10)
        # Predict target values using generated regression model by GLM and newdata.
        res = TDGLMPredict(id_column="id",
                            newdata=boston,
                            object=glm_1,
                            accumulate='medv')
        # Print the result DataFrame.
        print(res.result)
        # Example 4 : TDGLMPredict() predicts the 'medv' using generated regression model by GLM
        #             stepwise regression algorithm with initial_stepwise_columns.
        glm_2 = GLM(data=boston,
                    input_columns=
    ['crim','zn','indus','chas','nox','rm','age','dis','rad','tax','ptratio','black','lstat'],
                    response_column='medv',
                    family='GAUSSIAN',
                    lambda1=0.02,
                    alpha=0.33,
                    batch_size=10,
                    learning_rate='optimal',
                    iter_max=36,
                    iter_num_no_change=100,
                    tolerance=0.0001,
                    initial_eta=0.02,
                    stepwise_direction='bidirectional',
                    max_steps_num=10,
                    initial_stepwise_columns=['rad','tax']
            )
        # Predict target values using generated regression model by GLM and newdata.
        res = TDGLMPredict(id_column="id",
                            newdata=boston,
                            object=glm_2,
                            accumulate='medv')
        # Print the result DataFrame.
        print(res.result)
        # Example 5 : TDGLMPredict() predicts the 'price' using generated regression model by GLM
        #             using partition by key.
        glm_3 = GLM(data=housing_seg,
                    input_columns=
    ['bedrooms', 'bathrms', 'stories', 'driveway', 'recroom', 'fullbase', 'gashw', 'airco'],
                    response_column='price',
                    family='GAUSSIAN',
                    batch_size=10,
                    iter_max=1000,
                    data_partition_column='partition_id'
            )
        # Predict target values using generated regression model by GLM and newdata.
        res = TDGLMPredict(id_column="sn",
                            newdata=housing_seg,
                            object=glm_3,
                            accumulate='price',
                            newdata_partition_column='partition_id',
                            object_partition_column='partition_id')
        # Print the result DataFrame.
        print(res.result)
        # Example 6 : TDGLMPredict() predicts the 'price' using generated regression model by GLM
        #             using partition by key with attribute data.
        glm_4 = GLM(data=housing_seg,
                    input_columns=
    ['bedrooms', 'bathrms', 'stories', 'driveway', 'recroom', 'fullbase', 'gashw', 'airco'],
                    response_column='price',
                    family='GAUSSIAN',
                    batch_size=10,
                    iter_max=1000,
                    data_partition_column='partition_id',
                    attribute_data = housing_attribute,
                    attribute_data_partition_column = 'partition_id'
            )
        # Predict target values using generated regression model by GLM and newdata.
        res = TDGLMPredict(id_column="sn",
                newdata=housing_seg,
                object=glm_4,
                accumulate='price',
                newdata_partition_column='partition_id',
                object_partition_column='partition_id')
        # Print the result DataFrame.
        print(res.result)
        # Example 7 : TDGLMPredict() predicts the 'homestyle' using generated generalized linear model by GLM
        #             using partition by key with parameter data.
        glm_5 = GLM(data=housing_seg,
                    input_columns=
    ['bedrooms', 'bathrms', 'stories', 'driveway', 'recroom', 'fullbase', 'gashw', 'airco'],
                    response_column='homestyle',
                    family='binomial',
                    iter_max=1000,
                    data_partition_column='partition_id',
                    parameter_data = housing_parameter,
                    parameter_data_partition_column = 'partition_id'
            )
        res = TDGLMPredict(id_column="sn",
                            newdata=housing_seg,
                            object=glm_5,
                            accumulate='homestyle',
                            newdata_partition_column='partition_id',
                            object_partition_column='partition_id')
        # Print the result DataFrame.
        print(res.result)
    TDNaiveBayesPredict
    TDNaiveBayesPredict
    Functions
    TDNaiveBayesPredict(data=None, object=None, id_column=None, numeric_inputs=None, categorical_inputs=None, attribute_name_column=None,
    attribute_value_column=None, responses=None, output_prob=False, accumulate=None, **generic_arguments)
    DESCRIPTION:
        Function predicts classification label using model generated by NaiveBayes function
        for a test set of data.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the  teradataml DataFrame containing the model data
            or instance of NaiveBayes.
            Types: teradataml DataFrame or NaiveBayes
        id_column:
            Required Argument.
            Specifies the name of the column that uniquely identifies an 
            observation in the "data".
            Types: str
        numeric_inputs:
            Optional Argument.
            Specifies the name of the columns in "data" containing numeric attributes values.
            Types: str OR list of Strings (str)
        categorical_inputs:
            Optional Argument.
            Specifies the name of the columns in "data" containing categorical attributes values.
            Types: str OR list of Strings (str)
        attribute_name_column:
            Optional Argument.
            Specifies the name of the columns in "data" containing attributes names.
            Types: str
        attribute_value_column:
            Optional Argument.
            Specifies the name of the columns in "data" containing attributes values.
            Types: str
        responses:
            Optional Argument.
            Specifies a list of responses to output.
            Types: str OR list of strs
        output_prob:
            Optional Argument.
            Specifies whether to output the probability for each response.
            Default Value: False
            Types: bool
        accumulate:
            Optional Argument.
            Specify the names of the columns in "data" that need to be copied 
            from the input to output teradataml DataFrame.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of  NaiveBayesPredict.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as  NaiveBayesPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the 
        #        function in user space.
        #     2. User can import the function, if it is available on 
        #        Vantage user is connected to.
        #     3. To check the list of analytic functions available on 
        #        Vantage user connected to, use 
        #        "display_analytic_functions()".
        # Load the example data.
        load_example_data("decisionforestpredict", ["housing_train", "housing_test"])
        # Create teradataml DataFrame objects.
        housing_train = DataFrame.from_table("housing_train")
        housing_test = DataFrame.from_table("housing_test")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function  TDNaiveBayesPredict.
        from teradataml import  TDNaiveBayesPredict, NaiveBayes, Unpivoting
        # Example 1: TDNaiveBayesPredict function to predict the classification label using Dense input.
        NaiveBayes_out = NaiveBayes(data=housing_train, response_column='homestyle', 
                                    numeric_inputs=['price','lotsize','bedrooms','bathrms','stories','garagepl'], 
                                    categorical_inputs=['driveway','recroom','fullbase','gashw','airco','prefarea'])
        NaiveBayesPredict_out = TDNaiveBayesPredict(data=housing_test, object=NaiveBayes_out.result, id_column='sn',
                                                    numeric_inputs=
    ['price','lotsize','bedrooms','bathrms','stories','garagepl'],
                                                    categorical_inputs=
    ['driveway','recroom','fullbase','gashw','airco','prefarea'],
                                                    responses=['Classic', 'Eclectic', 'bungalow'],
                                                    accumulate='homestyle',
                                                    output_prob=True
                                                    )
        # Print the result DataFrame.
        print( NaiveBayesPredict_out.result)
        # Example 2: TDNaiveBayesPredict function to predict the classification label using Sparse input.
        # Unpivoting the data for sparse input to naive bayes.
        upvt_train = Unpivoting(data = housing_train, id_column = 'sn', 
                                target_columns = ['price','lotsize','bedrooms','bathrms','stories','garagepl',
                                                  'driveway','recroom','fullbase','gashw','airco','prefarea'],
                                attribute_column = "AttributeName",
                                value_column = "AttributeValue",
                                accumulate = 'homestyle')
        upvt_test = Unpivoting(data = housing_test, id_column = 'sn', 
                               target_columns = ['price','lotsize','bedrooms','bathrms','stories','garagepl','driveway',
                                                 'recroom','fullbase','gashw','airco','prefarea'],
                               attribute_column = "AttributeName", value_column = "AttributeValue",
                               accumulate = 'homestyle')
        NaiveBayes_out1 = NaiveBayes(data=upvt_train.result, 
                                     response_column='homestyle',
                                     attribute_name_column='AttributeName', 
                                     attribute_value_column='AttributeValue',
                                     numeric_attributes=['price','lotsize','bedrooms','bathrms','stories','garagepl'], 
                                     categorical_attributes=
    ['driveway','recroom','fullbase','gashw','airco','prefarea'])
        NaiveBayesPredict_out1 = TDNaiveBayesPredict(data=upvt_test.result, object=NaiveBayes_out1, id_column='sn',
                                                     attribute_name_column='AttributeName',
                                                     attribute_value_column='AttributeValue',
                                                     responses=['Classic', 'Eclectic', 'bungalow'],
                                                     accumulate='homestyle',
                                                     output_prob=True
                                                     )
        # Print the result DataFrame.
        print( NaiveBayesPredict_out1.result)
    XGBoostPredict
    XGBoostPredict
    Functions
    XGBoostPredict(newdata=None, object=None, id_column=None, num_boosted_tree=1000, iter_num=3, accumulate=None, output_prob=False,
    model_type='REGRESSION', output_responses=None, detailed=False, **generic_arguments)
    DESCRIPTION:
        The XGBoostPredict() function runs the predictive algorithm based on the model generated
        by XGBoost(). The XGBoost() function, also known as eXtreme Gradient Boosting, performs
        classification or regression analysis on datasets.
        XGBoostPredict() performs prediction for test input data using multiple simple trees in
        the trained model. The test input data should have the same attributes as used during
        the training phase, which can be up to 2046. These attributes are used to score based
        on the trees in the model.
        The output contains prediction for each data point in the test data based on regression
        or classification. The prediction probability is computed based on the majority vote
        from participating trees. A higher probability implies a more confident prediction by
        the model. Majority of the trees result in the same prediction.
        Notes:
            * The processing time is controlled by (proportional to):
                * The number of boosted trees used for prediction from the model (controlled
                  by "num_boosted_trees").
                * The number of iterations (sub-trees) used for prediction from the model
                  in each boosted tree (controlled by "iter_num").
            A careful choice of these parameters can be used to control the processing time.
            When the boosted trees size grows more than what can fit in memory, the trees are
            cached in a local spool space, which may impact the performance of the function
            compared to the case when all trees fit in memory.
    PARAMETERS:
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data generated by XGBoost()
            function or the instance of XGBoost.
            Types: teradataml DataFrame or XGBoost
        id_column:
            Required Argument.
            Specifies the input data column name that contains a unique
            identifier for each test point in the test set.
            Note:
                * Input column names with double quotation marks are not allowed for this function.
            Types: str
        num_boosted_tree:
            Optional Argument.
            Specifies how many boosted trees to use to make predictions.
            A combination of both 'task_Index' and 'tree_num' in the model table determines
            the AMPID and number of trees generated by that AMP. As we order the model table
            with these two arguments, the number of boosted trees that are loaded are based
            on this order.
            For example, if there are two AMPs on the system and AMP 1 ('task_index': 0) generates
            three boosted trees ('tree_num':1,2,3), while AMP 2 ('task_index': 1) generate two boosted
            trees ('tree_num': 1,2). Then, "num_boosted_tree"(4) loads three boosted trees from AMP1
            ('task_index': 0) and one boosted tree from AMP2 ('task_index': 1).
            As one boosted tree is skipped altogether from loading it in memory and making predictions,
            this results in a faster elapsed time for queries compared to loading all trees in memory.
            However, this can also lead to loss in prediction accuracy. In addition, any unique tree
            is determined by 'task_index', 'tree_id' and 'iter_num' in the model table.
            Default Value: 1000
            Types: int
        iter_num:
            Optional Argument.
            Specifies how many iterations to load for each boosted tree to make predictions.
            For example, AMP1 ('task_index':0) generates three boosted trees ('tree_num': 1,2,3)
            with each tree having four iterations ('iter':1,2,3,4). There are 12 trees in total.
            "iter_num"(2) only loads two iterations per boosted tree, that is, only six trees
            are loaded for this example.
            As some trees are skipped from being loaded in memory and predictions are made
            without them, this results in a faster elapsed time for queries compared to
            loading all trees in memory, but also lead to loss in prediction accuracy.
            Default Value: 3
            Types: int
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output. By default, the function copies no input teradataml DataFrame columns
            to the output.
            Types: str OR list of Strings (str)
        output_prob:
            Optional Argument.
            Specifies whether the function should output the probability for each
            "output_responses".
            Notes:
                * If "output_prob" is true and "output_responses" are not provided,
                  output the probability of the predicted class.
                * The "output_prob" argument works only when "model_type" is
                  'Classification'.
            Default Value: False
            Types: bool
        model_type:
            Optional Argument.
            Specifies whether the analysis is a regression (continuous response variable) or
            a multiple-class classification (predicting result from the number of classes).
            For 'Classification', output the prediction column as integers. These integral
            values represent different categories, and so are better observed as an integer
            column. To make the output schema for prediction column as an integer, set
            "model_type" as 'Classification'.
            Permitted Values:
                * Regression
                * Classification
            Default Value: Regression
            Types: str
        output_responses:
            Optional Argument.
            Specifies the classes for which to output probabilities.
            Notes:
                * If "output_prob" is true and "output_responses" are not provided,
                  output the probability of the predicted class.
                * The "output_responses" argument works only when "model_type" is
                  'Classification'.
            Types: str OR list of str(s)
        detailed:
            Optional Argument.
            Specifies whether to output detailed information of each prediction.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQLE Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of XGBoostPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as XGBoostPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "titanic")
        load_example_data("byom", "iris_input")
        # Create teradataml DataFrame objects.
        titanic = DataFrame.from_table("titanic")
        iris_input = DataFrame("iris_input")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: This example takes titanic data as input, and generates the Regression
        #            model using XGBoost(). Using XGBoostPredict() function to predict the
        #            fare with the Regression model generated by XGBoost().
        # Create 2 samples of input data - sample 1 will have 80% of total rows and
        # sample 2 will have 20% of total rows.
        titanic_sample = titanic.sample(frac=[0.8, 0.2])
        # Create train dataset from sample 1 by filtering on "sampleid" and drop
        # "sampleid" column as it is not required for training model.
        titanic_train = titanic_sample[titanic_sample.sampleid == "1"].drop("sampleid", axis = 1)
        # Create test dataset from sample 2 by filtering on "sampleid" and
        # drop "sampleid" column as it is not required for scoring.
        titanic_test = titanic_sample[titanic_sample.sampleid == "2"].drop("sampleid", axis = 1)
        XGBoost_out_1 = XGBoost(data=titanic_train,
                                input_columns=["age", "survived", "pclass"],
                                response_column = 'fare',
                                max_depth=3,
                                lambda1 = 1000.0,
                                model_type='Regression',
                                seed=-1,
                                shrinkage_factor=0.1,
                                iter_num=2)
        # XGBoostPredict() predicts the result using generated Regression model by
        # XGBoost() and "newdata".
        XGBoostPredict_out_1 = XGBoostPredict(newdata=titanic_test,
                                              object=XGBoost_out_1.result,
                                              id_column='passenger',
                                              object_order_column=['task_index', 'tree_num',
                                                                   'iter', 'tree_order'])
        # Print the result DataFrame.
        print(XGBoostPredict_out_1.result)
        # Example 2: This example takes titanic data, and generates the Classification
        #            model using XGBoost(). Using XGBoostPredict() function to predict the fare
        #            with the Classification model generated by XGBoost(). Provides the classes for
        #            which to output the probabilities.
        # Create 2 samples of input data - sample 1 will have 80% of total rows and
        # sample 2 will have 20% of total rows.
        iris_sample = iris_input.sample(frac=[0.8, 0.2])
        # Create train dataset from sample 1 by filtering on "sampleid" and drop
        # "sampleid" column as it is not required for training model.
        iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
        # Create test dataset from sample 2 by filtering on "sampleid" and
        # drop "sampleid" column as it is not required for scoring.
        iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
        # Training the model.
        XGBoost_out_2 = XGBoost(data=iris_train,
                                input_columns=['sepal_length', 'sepal_width', 'petal_length', 'petal_width'],
                                response_column = 'species',
                                max_depth=3,
                                lambda1 = 10000.0,
                                model_type='Classification',
                                seed=-1,
                                shrinkage_factor=0.1,
                                iter_num=2)
        # XGBoostPredict() predicts the result using generated Classification model by
        # XGBoost() and XGBoost object as input.
        XGBoostPredict_out_2 = XGBoostPredict(newdata=iris_test,
                                              object=XGBoost_out_2,
                                              id_column='id',
                                              model_type='Classification',
                                              num_boosted_trees=3,
                                              iter_num=2,
                                              output_prob=True,
                                              output_responses=['1', '2', '3'],
                                              object_order_column=['task_index', 'tree_num', 'iter',
                                                                   'class_num', 'tree_order'])
        # Print the result DataFrame.
        print(XGBoostPredict_out_2.result)
    FEATURE ENGINEERING TRANSFORM functions
    Antiselect
    Antiselect
    Functions
    Antiselect(data=None, exclude=None, **generic_arguments)
    DESCRIPTION:
        Antiselect() function returns all columns except those specified in the Exclude syntax element.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        exclude:
            Required Argument.
            Specifies the names of the columns not to return.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of Antiselect.
        Output teradataml DataFrames can be accessed using attribute
        references, such as AntiselectObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("antiselect", ["antiselect_input"])
        # Create teradataml DataFrame object.
        antiselect_input = DataFrame.from_table("antiselect_input")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Returns all columns except those specified in the exclude argument.
        obj = Antiselect(data=antiselect_input,
                         exclude=['rowids', 'orderdate', 'discount', 'province', 'custsegment'])
        # Print the result.
        print(obj.result)
    BincodeFit
    BincodeFit
    Functions
    BincodeFit(data=None, ﬁt_data=None, target_columns=None, method_type=None, nbins=None, label_preﬁx=None, target_colnames=None,
    minvalue_column=None, maxvalue_column=None, label_column=None, **generic_arguments)
    DESCRIPTION:
        The BinCodeFit() function outputs a DataFrame of information to input to
        BinCodeTransform() function, which bin-codes the specified input DataFrame.
        Bin-coding is typically used to convert numeric data to categorical data by
        binning the numeric data into multiple numeric bins (intervals).
        The bins can have a fixed-width with auto-generated labels or can have variable
        widths and labels.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        fit_data:
            Optional Argument.
            Specifies the input teradataml DataFrame containing binning parameters for
            VARIABLE-WIDTH. It is not needed for EQUAL-WIDTH.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the input teradataml DataFrame columns to generate bins information
            and binning parameters on.
            Types: str OR list of Strings (str)
        method_type:
            Required Argument.
            Specifies the Method Type which will be used for histogram computation.
            Permitted Values: EQUAL-WIDTH, VARIABLE-WIDTH
            Types: str
        nbins:
            Optional Argument.
            Specifies the number of bins to be used when "method_type" is
            EQUAL-WIDTH. It is not needed for VARIABLE-WIDTH. If one value is provided,
            it applies to all target columns, if more than one value is
            specified, "nbins" values apply to "target_columns" in the order
            specified by the user.
            Types: int OR list of ints
        label_prefix:
            Optional Argument.
            Specify the label prefix to be used when MethodType is EQUAL-WIDTH. If
            one value is provided, it applies to all target columns. If more than
            one value is specified, "label_prefix" values apply to "target_columns"
            in the order specified by the user.
            Default Value:  target column names.
            Types: str OR list of strs
        target_colnames:
            Optional Argument.
            Specifies the "fit_data" column which contains column name for
            which bins are specified.
            Default Value: ColumnName.
            Types: str
        minvalue_column:
            Optional Argument.
            Specifies the "fit_data" column which contains Min Value for the
            specified bins.
            Default Value: MinValue.
            Types: str
        maxvalue_column:
            Optional Argument.
            Specifies the "fit_data" column which contains Max Value for the
            specified bins.
            Default Value: MaxValue.
            Types: str
        label_column:
            Optional Argument.
            Specifies the "fit_data" column which contains label for which
            bins are specified.
            Default Value: Label.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQLE function supports it, else an exception is raised.
    RETURNS:
        Instance of BincodeFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as BincodeFitObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. output
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic", "bin_fit_ip"])
        # Create teradataml DataFrame objects.
        titanic_data = DataFrame.from_table("titanic")
        bin_fit_ip = DataFrame.from_table("bin_fit_ip")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Transform the data using BincodeFit object with Variable-Width.
        bin_code_1 = BincodeFit(data=titanic_data,
                                fit_data=bin_fit_ip,
                                fit_data_order_column = ['minVal', 'maxVal'],
                                target_columns='age',
                                minvalue_column='minVal',
                                maxvalue_column='maxVal',
                                label_column='label',
                                method_type='Variable-Width',
                                label_prefix='label_prefix'
                               )
        # Print the result.
        print(bin_code_1.output)
        # Example 2: Transform the data using BincodeFit object with Equal-Width.
        bin_code_2 = BincodeFit(data=titanic_data,
                                target_columns='age',
                                method_type='Equal-Width',
                                nbins=2,
                                label_prefix='label_prefix'
                               )
        # Print the result.
        print(bin_code_2.output)
    BincodeTransform
    BincodeTransform
    Functions
    BincodeTransform(data=None, object=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        The BincodeTransform() function is used to convert the continuous numeric data to
        categorical data. The BincodeTransform() function takes the output of BincodeFit()
        function to apply the transform on input DataFrame.
        BincodeTransform() function takes two DataFrames as input:
            1. Input dataframe which contains numeric data to be converted to categorical
               data.
            2. Output of BincodeFit() function which contains the binning data.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the binning parameters
            generated by BincodeFit() function or the instance of BincodeFit.
            Types: teradataml DataFrame or BincodeFit
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output. By default, the function copies no input teradataml
            DataFrame columns to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
                Note:
                    These generic arguments are supported by teradataml if the underlying
                    SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of BincodeTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as BincodeTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic", "bin_fit_ip"])
        # Create teradataml DataFrame objects.
        titanic_data = DataFrame.from_table("titanic")
        bin_fit_ip = DataFrame.from_table("bin_fit_ip")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Convert the continuous numeric data in the column 'age' to
        #            categorical data.
        # First generate a model using BinCodeFit() function to bin-code the values
        # in column 'age' using the 'Variable-Width' method type.
        bin_code_1 = BincodeFit(data=titanic_data,
                                fit_data=bin_fit_ip,
                                fit_data_order_column = ['minVal', 'maxVal'],
                                target_columns='age',
                                minvalue_column='minVal',
                                maxvalue_column='maxVal',
                                label_column='label',
                                method_type='Variable-Width',
                                label_prefix='label_prefix'
                               )
        # Apply the transformation on the input data using the model generated above.
        # Note the model is passed as a teradataml DataFrame.
        obj = BincodeTransform(data=titanic_data,
                               object=bin_code_1.output,
                               object_order_column="TD_MinValue_BINFIT",
                               accumulate=['passenger', 'ticket']
                              )
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Convert the continuous numeric data in the column 'age' to
        #            categorical data.
        # First generate a model using BinCodeFit() function to bin-code the values
        # in column 'age' using the 'Equal-Width' method type.
        bin_code_2 = BincodeFit(data=titanic_data,
                                target_columns='age',
                                method_type='Equal-Width',
                                nbins=2,
                                label_prefix='label_prefix'
                               )
        # Apply the transformation on the input data using the model generated above.
        # Note that model is passed as instance of BincodeFit to "object".
        obj = BincodeTransform(data=titanic_data,
                               object=bin_code_2,
                               accumulate=['passenger', 'ticket']
                              )
        # Print the result DataFrame.
        print(obj.result)
    ColumnTransformer
    ColumnTransformer
    Functions
    ColumnTransformer(input_data=None, bincode_ﬁt_data=None, function_ﬁt_data=None, nonlinearcombine_ﬁt_data=None, onehotencoding_ﬁt_data=None,
    ordinalencoding_ﬁt_data=None, outlierﬁlter_ﬁt_data=None, polynomialfeatures_ﬁt_data=None, rownormalize_ﬁt_data=None, scale_ﬁt_data=None,
    simpleimpute_ﬁt_data=None, ﬁllrowid_column_name=None, **generic_arguments)
    DESCRIPTION:
        The ColumnTransformer() function transforms the input data columns in a single operation.Provide only
        the FIT dataframes generated by the Fit analytic functions, and the function runs all transformations
        that user require in a single operation.
        The function performs the following transformations:
                * Scale Transform
                * Bincode Transform
                * Function Transform
                * NonLinearCombine Transform
                * OutlierFilter Transform
                * PolynomialFeatures Transform
                * RowNormalize Transform
                * OrdinalEncoding Transform
                * OneHotEncoding Transform
                * SimpleImpute Transform
        User must create the FIT dataframe before using the function and must be provided in the same order
        as in the training data sequence to transform the dataset. The FIT dataframe can have maximum of
        128 columns.
        Note:
            * ColumnTransformer() function works only with python 3.6 and above.
    PARAMETERS:
        input_data:
            Required Argument.
            Specifies the teradataml DataFrame that contains input data.
            Types: teradataml DataFrame
        bincode_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the BincodeFit() function.
            Types: teradataml DataFrame
        function_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the Fit() function.
            Types: teradataml DataFrame
        nonlinearcombine_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the NonLinearCombineFit() function.
            One can pass multiple NonLinearCombineFit() function's output using argument names
            as nonlinearcombine_fit_data1, nonlinearcombine_fit_data2 and so on.
            Notes:
                    * Function considers the arguments which are in sequence and ignores the arguments
                    which are out of sequence.
                    Example - If arguments are nonlinearcombine_fit_data, nonlinearcombine_fit_data1,
                    nonlinearcombine_fit_data2, nonlinearcombine_fit_data4, the function considers only
                    nonlinearcombine_fit_data, nonlinearcombine_fit_data1 and nonlinearcombine_fit_data2
                    for processing and ignores nonlinearcombine_fit_data4 since it is out of sequence.
                    * Argument sequence starts from "nonlinearcombine_fit_data", followed by
                    "nonlinearcombine_fit_data1", "nonlinearcombine_fit_data2", so on..
            Types: teradataml DataFrame
        onehotencoding_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the OneHotEncodingFit() function.
            Types: teradataml DataFrame
        ordinalencoding_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the OrdinalEncodingFit() function.
            Types: teradataml DataFrame
        outlierfilter_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the OutlierFilterFit() function.
            Types: teradataml DataFrame
        polynomialfeatures_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the PolynomialFeaturesFit() function.
            Types: teradataml DataFrame
        rownormalize_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the RowNormalizeFit() function.
            Types: teradataml DataFrame
        scale_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the ScaleFit() function.
            Types: teradataml DataFrame
        simpleimpute_fit_data:
            Optional Argument.
            Specifies the teradataml DataFrame generated by the SimpleImputeFit() function.
            Types: teradataml DataFrame
        fillrowid_column_name:
            Optional Argument.
            Specifies the name for the output column in which unique identifiers for each row are
            populated.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQLE Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of ColumnTransformer.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ColumnTransformerObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame objects.
        titanic = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Perform Bincode transformation and OneHotEncoding transformation using fit functions and
        #             ColumnTransformer() function.
        bin_code = BincodeFit(data=titanic,
                              target_columns='age',
                              method_type='Equal-Width',
                              nbins=2,
                              label_prefix='label_prefix')
        one_hot_encoding = OneHotEncodingFit(data=titanic,
                                             is_input_dense=True,
                                             target_column="sex",
                                             categorical_values=["male", "female"],
                                             other_column="other")
        ColumnTransformer_out = ColumnTransformer(fillrowid_column_name="output_value",
                                                  input_data=titanic,
                                                  bincode_fit_data=bin_code.output,
                                                  onehotencoding_fit_data=one_hot_encoding.result)
        # Print the result DataFrame.
        print(ColumnTransformer_out.result)
        # Example 2: Perform multiple NonLinearCombineFit transformation using fit function and
        #            ColumnTransformer() function.
        # Create multiple NonLinearCombineFit objects and pass those to ColumnTransformer.
        NonLinearCombineFit_out1 = NonLinearCombineFit(data = titanic,
                                                       target_columns = ["sibsp", "parch"],
                                                       formula = "Y=(X0+X1+1)",
                                                       result_column = "total_cost")
        NonLinearCombineFit_out2 = NonLinearCombineFit(data = titanic,
                                                       target_columns = [ "fare"],
                                                       formula = "Y=(X0+1)",
                                                       result_column = "total_fare_cost2")
        NonLinearCombineFit_out3 = NonLinearCombineFit(data = titanic,
                                                       target_columns = [ "fare"],
                                                       formula = "Y=(X0+3)",
                                                       result_column = "total_fare_cost3")
        ColumnTransformer_out2 = ColumnTransformer(input_data=titanic,
                                                   nonlinearcombine_fit_data=NonLinearCombineFit_out1.result,
                                                   nonlinearcombine_fit_data_order_column=['parch'],
                                                   nonlinearcombine_fit_data1=NonLinearCombineFit_out2.result,
                                                   nonlinearcombine_fit_data1_order_column=['fare'],
                                                   nonlinearcombine_fit_data2=NonLinearCombineFit_out3.result)
        # Print the result DataFrame.
        print(ColumnTransformer_out2.result)
        # Example 3: Perform BincodeFit transformation along with OrdinalEncodingFit and
        #            OneHotEncodingFit transformation using fit functions and ColumnTransformer()
        #            function.
        # Create BincodeFit object along with OrdinalEncodingFit and OneHotEncodingFit
        # objects, and pass those to ColumnTransformer.
        bin_code = BincodeFit(data=titanic,
                              target_columns='age',
                              method_type='Equal-Width',
                              nbins=2,
                              label_prefix='label_prefix')
        one_hot_encoding = OneHotEncodingFit(data=titanic,
                                              is_input_dense=True,
                                              target_column="embarked",
                                              categorical_values=["c", "s"],
                                              other_column="other")
        ordinal_encodingfit = OrdinalEncodingFit(target_column='sex', data=titanic)
        ColumnTransformer_out3 = ColumnTransformer(fillrowid_column_name="output_value",
                                                   input_data=titanic,
                                                   bincode_fit_data=bin_code.output,
                                                   onehotencoding_fit_data=one_hot_encoding.result,
                                                   ordinalencoding_fit_data=ordinal_encodingfit.result)
        # Print the result DataFrame.
        print(ColumnTransformer_out3.result)
    Fit
    Fit
    Functions
    Fit(data=None, object=None, **generic_arguments)
    DESCRIPTION:
        The Fit() function determines whether specified numeric transformations can be
        applied to specified "target_columns" and outputs a DataFrame to use as input
        "data" for Transform() function, which does the transformations.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the transformation teradataml DataFrame that contains transformation
            information for the columns in "data".
            Types: teradataml DataFrame
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
                Note:
                    These generic arguments are supported by teradataml if the underlying
                    SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of Fit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as FitObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["iris_input", "transformation_table"])
        # Create teradataml DataFrame objects.
        iris_input = DataFrame.from_table("iris_input")
        transformation_df = DataFrame.from_table("transformation_table")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Run Fit() with all arguments.
        fit_df = Fit(data=iris_input,
                     object=transformation_df,
                     object_order_column='TargetColumn'
                    )
        # Print the result DataFrame.
        print(fit_df)
    NonLinearCombineFit
    NonLinearCombineFit
    Functions
    NonLinearCombineFit(data=None, target_columns=None, formula=None, result_column='TD_CombinedValue', **generic_arguments)
    DESCRIPTION:
        The NonLinearCombineFit() function returns the target columns and a 
        specified formula which uses the non-linear combination of existing features.
        Notes:
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
            * For information about PTCs, see Teradata Vantage™ - Analytics Database 
              International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" to run the 
            non-linear combination.
            Types: str OR list of Strings (str)
        formula:
            Required Argument.
            Specifies the formula to be used for non-linear combination.
            Types: str
        result_column:
            Optional Argument.
            Specifies the name of the new feature column generated by the Transform function.
            This function saves the specified formula in this column.
            Default Value: 'TD_CombinedValue'
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of NonLinearCombineFit.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as NonLinearCombineFitObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "titanic")
        # Create teradataml DataFrame objects.
        titanic = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Create model to obtain total cost for passenger 
        #             using formula ('sibsp' + 'parch' + 1) * 'fare'.
        NonLinearCombineFit_out = NonLinearCombineFit(data = titanic,
                                                      target_columns = ["sibsp", "parch", "fare"],
                                                      formula = "Y=(X0+X1+1)*X2",
                                                      result_column = "total_cost")
        # Print the result DataFrames.
        print(NonLinearCombineFit_out.result)
        print(NonLinearCombineFit_out.output_data)
    NonLinearCombineTransform
    NonLinearCombineTransform
    Functions
    NonLinearCombineTransform(data=None, object=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        The NonLinearCombineTransform() function generates a new feature
        by taking a non-linear combination of existing features using the
        parameters from the output of NonLinearCombineFit() function.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the fit parameters
            and target columns, generated by the NonLinearCombineFit() function
            or the instance of NonLinearCombineFit.
            Types: teradataml DataFrame or NonLinearCombineFit
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s)
            to copy to the output.
            By default, the function copies no input teradataml
            DataFrame columns to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of NonLinearCombineTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as NonLinearCombineTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "titanic")
        # Create teradataml DataFrame objects.
        titanic = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Create model to obtain total cost for passenger using formula
        # ('sibsp' + 'parch' + 1) * 'fare'.
        Fit_out = NonLinearCombineFit(data = titanic,
                                      target_columns = ["sibsp", "parch", "fare"],
                                      formula = "Y=(X0+X1+1)*X2",
                                      result_column = "total_cost")
        # Example 1 : Get the total cost for each passenger.
        NonLinearCombineTransform_out = NonLinearCombineTransform(data = titanic,
                                                                  object = Fit_out,
                                                                  accumulate="passenger")
        # Print the result DataFrame.
        print(NonLinearCombineTransform_out.result)
    OneHotEncodingFit
    OneHotEncodingFit
    Functions
    OneHotEncodingFit(data=None, category_data=None, target_column=None, attribute_column=None, value_column=None, is_input_dense=None,
    approach='LIST', categorical_values=None, target_column_names=None, categories_column=None, other_column='other', category_counts=None,
    target_attributes=None, other_attributes=None, **generic_arguments)
    DESCRIPTION:
        The OneHotEncodingFit() function outputs a DataFrame of attributes and categorical
        values to input to OneHotEncodingTransform() function, which encodes them as
        one-hot numeric vectors.
        Notes:
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
            * This function does not support KanjiSJIS or Graphic data types.
            * For input to be considered as sparse input, column names should be
              provided for 'data_partition_column' argument.
            * In case of dense input, only allowed value for 'data_partition_column'
              is PartitionKind.ANY and that for 'category_data_partition_column' is
              PartitionKind.DIMENSION.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        category_data:
            Optional Argument.
            Specifies the data containing the input categories for 'LIST' approach.
            Types: teradataml DataFrame
        target_column:
            Required when "is_input_dense" is set to 'True', disallowed otherwise.
            Specifies the name of the column in "data" to be encoded.
            Note:
                * The maximum number of unique columns in the "target_column"
                  argument is 2018.
            Types: str OR list of Strings (str)
        attribute_column:
            Required when "is_input_dense" is set to 'False', disallowed otherwise.
            Specifies the name of the column in "data" which contains attribute
            names.
            Types: str
        value_column:
            Required when "is_input_dense" is set to 'False', disallowed otherwise.
            Specifies the name of the column in "data" which contains attribute
            values.
            Types: str
        is_input_dense:
            Required Argument.
            Specifies whether input is in dense format or sparse format.
            Note:
                "category_data" is meant for dense input format and not for sparse format.
            Types: bool
        approach:
            Optional Argument.
            Specifies whether to determine categories automatically from the
            input data (AUTO approach) or the user provided list (LIST approach).
            Default Value: "LIST"
            Permitted Values: AUTO, LIST
            Types: str
        categorical_values:
            Required when "approach" is set to 'LIST' and a single value
            is present in "target_column", optional otherwise.
            Specifies the list of categories that need to be encoded in
            the desired order.
            When only one target column is provided, category values are read
            from this argument. Otherwise, they will be read from the
            "category_data".
            Notes:
                * The number of characters in "target_column_names" plus the
                  number of characters in the category specified in the
                  "categorical_values" argument must be less than 128 characters.
                * The maximum number of categories in the "categorical_values"
                  argument is 2018.
            Types: str OR list of Strings (str)
        target_column_names:
            Required when "category_data" is used, optional otherwise.
            Specifies the "category_data" column which contains the
            names of the target columns.
            Types: str
        categories_column:
            Required when "category_data" is used, optional otherwise.
            Specifies the "category_data" column which contains the
            category values.
            Types: str
        other_column:
            Optional when "is_input_dense" is set to 'True', disallowed otherwise.
            Specifies the column name for the column representing one-hot encoding
            for values other than the ones specified in the "categorical_values"
            argument or "category_data" or categories found through the 'auto'
            approach.
            Default Value: 'other'
            Types: str
        category_counts:
            Required when "category_data" is used or "approach" is
            set to 'auto', optional otherwise.
            Specifies the category counts for each of the "target_column".
            The number of values in "category_counts" should be the same
            as the number of "target_column".
            Types: str OR list of Strings (str)
        target_attributes:
            Required when "is_input_dense" is set to 'False', disallowed otherwise.
            Specifies one or more attributes to encode in one-hot form. Every target attribute must
            be in "attribute_column".
            Types: str OR list of Strings (str)
        other_attributes:
            Optional when "is_input_dense" is set to 'False', disallowed otherwise.
            For each target attribute, specifies a category name for attributes that "target_attributes"
            does not specify. The nth "other_attributes" corresponds to the nth "target_attribute".
            Notes:
                * The number of characters in values specified in the "target_attributes" argument
                  plus the number of characters in values specified in the "other_attributes"
                  argument must be less than 128 characters.
                * The number of values passed to the "target_attributes" argument and "other_attributes"
                  argument must be equal.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of OneHotEncodingFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as OneHotEncodingFitObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic", "cat_table"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        cat_data = DataFrame.from_table("cat_table")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Generate fit object to encode 'male' and 'female' values of column 'sex'.
        fit_obj1 = OneHotEncodingFit(data=titanic_data,
                                     is_input_dense=True,
                                     target_column="sex",
                                     categorical_values=["male", "female"],
                                     other_column="other")
        # Print the result DataFrame.
        print(fit_obj1.result)
        # Example 2: Generate fit object to encode column 'sex' and 'embarked' in dataset.
        fit_obj2 = OneHotEncodingFit(data=titanic_data,
                                     is_input_dense=True,
                                     approach="auto",
                                     target_column=["sex", "embarked"],
                                     category_counts=[2, 3],
                                     other_column="other")
        # Print the result DataFrame.
        print(fit_obj2.result)
        # Example 3: Generate fit object when "category_data" is used.
        fit_obj3 = OneHotEncodingFit(data=titanic_data,
                                     category_data=cat_data,
                                     target_column_names="column_name",
                                     categories_column="category",
                                     is_input_dense=True,
                                     target_column=["sex", "embarked", "name"],
                                     category_counts=[2, 4, 6],
                                     other_column="other")
        # Print the result DataFrame.
        print(fit_obj3.result)
        # Example 4: Generate fit object when "approach" is set to 'LIST'.
        fit_obj4 = OneHotEncodingFit(data=titanic_data,
                                     is_input_dense=True,
                                     approach="list",
                                     categorical_values=['male','female'],
                                     target_column=["sex"],
                                     other_column="other")
        # Print the result DataFrame.
        print(fit_obj4.result)
    OneHotEncodingTransform
    OneHotEncodingTransform
    Functions
    OneHotEncodingTransform(data=None, object=None, is_input_dense=None, **generic_arguments)
    DESCRIPTION:
        Function encodes specified attributes and categorical values as one-hot numeric vectors,
        using OneHotEncodingFit() function output.
        Notes:
            * In case of sparse input, neither 'data_partition_column' nor
              'object_partition_column' can be used independently.
            * In case of dense input, if 'data_partition_column' is having value
              PartitionKind.ANY, then 'object_partition_column' should have value
              PartitionKind.DIMENSION.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the encoding parameters generated by
            OneHotEncodingFit() function or the instance of OneHotEncodingFit.
            Types: teradataml DataFrame or OneHotEncodingFit
        is_input_dense:
            Required Argument.
            Specifies whether input is in dense format or sparse format.
            Types: boolean
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of OneHotEncodingTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as OneHotEncodingTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Transform categorical column "sex" to numerical columns "sex_male", "sex_female",
        #            and "sex_other" using OneHotEncodingFit() and OneHotEncodingTransform().
        # Generate fit object for column "sex".
        fit_obj = OneHotEncodingFit(data=titanic_data,
                                    is_input_dense=True,
                                    target_column="sex",
                                    categorical_values=["male", "female"],
                                    other_column="other")
        # Print the result DataFrame.
        print(fit_obj.result)
        # Encode "male" and "female" values of column "sex".
        # Note that teradataml DataFrame representing the model is passed as
        # input to "object".
        obj = OneHotEncodingTransform(data=titanic_data,
                                      object=fit_obj.result,
                                      is_input_dense=True)
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Encode "male" and "female" values of column "sex".
        #            Note that model is passed as instance of OneHotEncodingFit
        #            to "object".
        obj1 = OneHotEncodingTransform(data=titanic_data,
                                       object=fit_obj,
                                       is_input_dense=True)
        # Print the result DataFrame.
        print(obj1.result)
    OrdinalEncodingFit
    OrdinalEncodingFit
    Functions
    OrdinalEncodingFit(data=None, category_data=None, target_column=None, approach='AUTO', categories=None, ordinal_values=None,
    target_column_names=None, categories_column=None, ordinal_values_column=None, start_value=0, default_value=None, **generic_arguments)
    DESCRIPTION:
        OrdinalEncodingFit() function identifies distinct categorical
        values from the input data or a user-defined list and generates
        the distinct categorical values along with the ordinal value for
        each category.
        Notes:
            * Function requires the UTF8 client character set for UNICODE data.
            * Function does not support Pass Through Characters (PTCs).
            * Function does not support KanjiSJIS or Graphic data types.
            * The maximum number of unique categories in a particular column is 4000.
            * The maximum category length is 128 characters.
            * NULL categories are not encoded.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data containing the categorical target column.
            Types: teradataml DataFrame
        category_data:
            Optional Argument.
            Specifies the data containing the input categories for 'LIST' approach.
            Types: teradataml DataFrame
        target_column:
            Required Argument.
            Specifies the name of the categorical input target column.
            Note:
                The maximum number of unique columns in the "target_column" argument is 2018.
            Types: str OR list of Strings (str)
        approach:
            Optional Argument.
            Specifies whether to determine categories automatically from the
            input data (AUTO approach) or determine categories from the list
            provided by user (LIST approach).
            Default Value: "AUTO"
            Permitted Values: AUTO, LIST
            Types: str
        categories:
            Optional Argument.
            Specifies the list of categories that need to be encoded in the
            desired order.
            Notes:
                * If only one target column is provided, category values
                  read from this argument. Otherwise, they read from the
                  "category_data".
                * Required, when user use the 'LIST' approach and a single
                  target column.
            Types: str OR list of strs
        ordinal_values:
            Optional Argument.
            Specifies the custom ordinal values to replace the categories,
            when user use the 'LIST' approach for encoding the categorical values.
            If user does not provide the "ordinal_values" and the "start_value",
            then by default, the first category contains the default start value
            '0', and the last category is assigned a value that is one lesser than
            the total number of categories.
            For example, if there are three categories, then the categories contain
            the values 0, 1, 2 respectively.
            However, if user only specify the ordinal values, then each ordinal value
            is associated with a categorical value. For example, if there are three categories
            and the ordinal values are 3, 4, 5 then the ordinal values are assigned to the
            respective categories.
            The OrdinalEncodingFit() function returns an error when the ordinal value
            count does not match the categorical value count or if both the ordinal
            values and the start value are provided.
            Notes:
                * User can either use the "ordinal_values" or the "start_value" argument
                  in the syntax.
                * If only one target column is provided, ordinal values are read from this argument.
                  Otherwise, they are read from the "category_data".
                * If omitted, ordinal values are generated based on the "start_value" argument.
            Types: int OR list of ints
        target_column_names:
            Required when "category_data" is used, optional otherwise.
            Specifies the "category_data" column which contains the names of the
            target columns.
            Types: str
        categories_column:
            Required when "category_data" is used, optional otherwise.
            Specifies the "category_data" column which contains the category values.
            Types: str
        ordinal_values_column:
            Required when "category_data" is used, optional otherwise.
            Specifies the "category_data" column which contains the ordinal values.
            If omitted, ordinal values will be generated based on the "start_value"
            argument.
            Types: str
        start_value:
            Optional Argument.
            Specifies the starting value for ordinal values list.
            Default Value: 0
            Types: int
        default_value:
            Optional Argument.
            Specifies the ordinal value to use when category is not found.
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of OrdinalEncodingFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as OrdinalEncodingFitObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic","cat_table"])
        # Create teradataml DataFrame objects.
        titanic = DataFrame.from_table("titanic")
        cat_data = DataFrame.from_table("cat_table")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Identyfying distinct categorical values from the input.
        ordinal_encodingfit_res_1 = OrdinalEncodingFit(target_column='sex',
                                                       data=titanic)
        # Print the result DataFrame.
        print(ordinal_encodingfit_res_1.result)
        # Example 2: Identifying distinct categorical values from the input and
        #            returns the distinct categorical values along with the ordinal
        #            value for each category.
        ordinal_encodingfit_res_2 = OrdinalEncodingFit(target_column='sex',
                                                       approach='LIST',
                                                       categories=['category0', 'category1'],
                                                       ordinal_values=[1, 2],
                                                       start_value=0,
                                                       default_value=-1,
                                                       data=titanic)
        # Print the result DataFrame.
        print(ordinal_encodingfit_res_2.result)
        # Example 3: Provide ordinal values to "target_column" using
        #            dataset by "category_data".
        ordinal_encodingfit_res_3 = OrdinalEncodingFit(target_column=['name','sex','ticket','cabin','embarked'],
                                                       category_data=cat_data,
                                                       approach='LIST',
                                                       target_column_names="column_name",
                                                       categories_column="category",
                                                       ordinal_values_column="ordinal_value",
                                                       default_value=[-1, -10, -15, 20, 0],
                                                       data=titanic)
        # Print the result DataFrame.
        print(ordinal_encodingfit_res_3.result)
    OrdinalEncodingTransform
    OrdinalEncodingTransform
    Functions
    OrdinalEncodingTransform(data=None, object=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        The OrdinalEncodingTransform() function maps the categorical value
        to a specified ordinal value using the OrdinalEncodingFit()
        function output.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the ordinalencoding parameters
            generated by OrdinalEncodingFit() function or the instance of OrdinalEncodingFit.
            Types: teradataml DataFrame or OrdinalEncodingFit
        accumulate:
            Optional Argument.
            Specifies the input teradataml DataFrame column(s) to copy to the
            output. By default, the function copies no input teradataml
            DataFrame columns to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of OrdinalEncodingTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as OrdinalEncodingTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame objects.
        titanic = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Transform the data by identyfying distinct categorical values from the input
        #            using OrdinalEncodingFit.
        ordinal_encodingfit_res_1 = OrdinalEncodingFit(target_column='sex',
                                                       data=titanic)
        ordinal_encoding_transform_out_1 = OrdinalEncodingTransform(data = titanic,
                                                                    object = ordinal_encodingfit_res_1.result,
                                                                    accumulate='age'
                                                                    )
        # Print the result DataFrame.
        print(ordinal_encoding_transform_out_1.result)
        # Example 2: Transform the data by identyfying distinct categorical values
        #            from the input and returns the distinct categorical values
        #            along with the ordinal value for each category.
        ordinal_encodingfit_res_2 = OrdinalEncodingFit(target_column='sex',
                                                       approach='LIST',
                                                       categories=['category0', 'category1'],
                                                       ordinal_values=[1, 2],
                                                       start_value=0,
                                                       default_value=-1,
                                                       data=titanic)
        ordinal_encoding_transform_out_2 = OrdinalEncodingTransform(data = titanic,
                                                                    object = ordinal_encodingfit_res_2,
                                                                    accumulate='age'
                                                                    )
        # Print the result DataFrame.
        print(ordinal_encoding_transform_out_2.result)
    Pivoting
    Pivoting
    Functions
    Pivoting(data=None, partition_columns=None, target_columns=None, accumulate=None, rows_per_partition=None, pivot_column=None, pivot_keys=None,
    pivot_keys_alias=None, default_pivot_values=None, aggregation=None, delimiters=None, combined_column_sizes=None, truncate_columns=None,
    output_column_names=None, **generic_arguments)
    DESCRIPTION:
        Function pivots the data, that is, changes the data from 
        sparse format to dense format.
        Notes:
            * 'data_partition_column' is required argument for partitioning the input data.
            * Provide either the 'rows_per_partition', 'pivot_column', or 'aggregation' arguments 
              along with required arguments.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame to be pivoted.
            Types: teradataml DataFrame
        partition_columns:
            Required Argument.
            Specifies the name of the column(s) in "data" on which to partition the 
            input.
            Types: str OR list of Strings (str)
        target_columns:
            Required Argument.
            Specifies the name of the column(s) in "data" which contains the data for 
            pivoting.
            Types: str OR list of Strings (str)
        accumulate:
            Optional Argument.
            Specifies the name of the column(s) in "data" to copy to the output. 
            By default, the function copies no input table columns to the output.
            Types: str OR list of Strings (str)
        rows_per_partition:
            Optional Argument.
            Specifies the maximum number of rows in the partition.
            Types: int
        pivot_column:
            Optional Argument.
            Specifies the name of the column in "data" that contains the pivot keys.
            Note:
                * This argument is not needed when 'rows_per_partition' is provided.
            Types: str
        pivot_keys:
            Optional Argument.
            Specifies the names of the pivot keys, if "pivot_column" is specified. 
            Notes:
                * This argument is not needed when 'rows_per_partition' is provided.
                * 'pivot_keys' are required when 'pivot_column' is specified.
            Types: str OR list of Strings (str)
        pivot_keys_alias:
            Optional Argument.
            Specifies the alias names of the pivot keys, if 'pivot_column' is specified.
            Note:
                * This argument is not needed when 'rows_per_partition' is provided.
            Types: str OR list of Strings (str)
        default_pivot_values:
            Optional Argument.
            Specifies one default value for each pivot_key. The nth 
            default_pivot_value applies to the nth pivot_key.
            Note:
                * This argument is not needed when 'rows_per_partition' is provided.
            Types: str OR list of Strings (str)
        aggregation:
            Optional Argument.
            Specifies the aggregation for the target columns. 
            Provide a single value {CONCAT | UNIQUE_CONCAT | SUM | 
            MIN | MAX | AVG}  which will be applicable to all target columns or 
            specify multiple values for multiple target columns in 
            following format: ['ColumnName:{CONCAT|UNIQUE_CONCAT|SUM|MIN|MAX|AVG}',...].
            Types: str OR list of Strings (str)
        delimiters:
            Optional Argument.
            Specifies the delimiter to be used for concatenating the values of a target column. 
            Provide a single delimiter value applicable to all target columns or 
            specify multiple delimiter values for multiple target columns 
            in the following format: ['ColumnName:single_char',...].
            Note:
                * This argument is not needed when 'aggregation' is not specified.
            Types: str OR list of Strings (str)
        combined_column_sizes:
            Optional Argument.
            Specifies the maximum size of the concatenated string.
            Provide a single integer value that applies to all target columns or 
            specify multiple size values for multiple target columns 
            in the following format ['ColumnName:size_value',...].
            Note:
                * This argument is not needed when 'aggregation' is not specified.
            Types: int OR  str OR list of Strings (str)
        truncate_columns:
            Optional Argument.
            Specifies columns from the target columns for which 
            to truncate the concatenated string if it exceeds the specified size.
            Note:
                * This argument is not needed when 'aggregation' is not specified.
            Types: str OR list of Strings (str)
        output_column_names:
            Optional Argument.
            Specifies the column name to be used for the output column. The nth 
            column name value applies to the nth output column.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of  Pivoting.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as  PivotingObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the 
        #        function in user space.
        #     2. User can import the function, if it is available on 
        #        Vantage user is connected to.
        #     3. To check the list of analytic functions available on 
        #        Vantage user connected to, use 
        #        "display_analytic_functions()".
        # Load the example data.
        load_example_data('unpivot', 'titanic_dataset_unpivoted')
        load_example_data('unpivot', 'star_pivot')
        # Create teradataml DataFrame objects.
        titanic_unpvt = DataFrame.from_table('titanic_dataset_unpivoted')
        star = DataFrame.from_table('star_pivot')
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function  Pivoting.
        from teradataml import  Pivoting
        # Example 1 : Pivot the input data using 'rows_per_partition'.
        pvt1 = Pivoting(data = titanic_unpvt, 
                        partition_columns = 'passenger', 
                        target_columns = 'AttributeValue',
                        accumulate = 'survived', 
                        rows_per_partition = 2,
                        data_partition_column='passenger',
                        data_order_column='AttributeName')
        # Print the result DataFrame.
        print( pvt1.result)
        # Example 2 : Pivot the input data using 'pivot_column' and 'pivot_keys'.
        pvt2 = Pivoting(data = titanic_unpvt, 
                        partition_columns = 'passenger', 
                        target_columns = 'AttributeValue',
                        accumulate = 'survived', 
                        pivot_column = 'AttributeName',
                        pivot_keys = ['pclass','gender'],
                        data_partition_column = 'passenger')
        # Print the result DataFrame.
        print( pvt2.result)
        # Example 3 : Pivot the input data with multiple target columns and
        #             multiple aggregation functions.
        pvt3 = Pivoting(data = star, 
                        partition_columns = ['country', 'state'],
                        target_columns = ['sales', 'cogs', 'rating'],
                        accumulate = 'yr',
                        pivot_column = 'qtr',
                        pivot_keys = ['Q1','Q2','Q3'],
                        aggregation = ['sales:SUM','cogs:AVG','rating:CONCAT'],
                        delimiters = '|', 
                        combined_column_sizes = 64001,
                        data_partition_column = ['country', 'state'],
                        data_order_column = ['qtr'])
        # Print the result DataFrame.
        print( pvt3.result)
        # Example 4 : Pivot the input data with multiple target columns and
        #             multiple aggregation functions.
        pvt4 = Pivoting(data = star, 
                        partition_columns = 'country', 
                        target_columns = ['sales', 'cogs', 'state','rating'],
                        accumulate = 'yr', 
                        aggregation = ['sales:SUM','cogs:AVG','state:UNIQUE_CONCAT','rating:CONCAT'], 
                        delimiters = '|', 
                        combined_column_sizes = ['state:5', 'rating:10'],
                        data_partition_column='country',
                        data_order_column='state')
        # Print the result DataFrame.
        print( pvt4.result)
        # Example 5 : Pivot the input data with truncate columns.
        pvt5 = Pivoting(data = star, 
                        partition_columns = ['state'],
                        target_columns = ['country', 'rating'],
                        accumulate = 'yr',
                        pivot_column = 'qtr',
                        pivot_keys = ['Q1','Q2','Q3'],
                        aggregation = 'CONCAT',
                        combined_column_sizes = 10,
                        truncate_columns = 'country',
                        data_partition_column = 'qtr',
                        data_order_column='state')
        # Print the result DataFrame.
        print( pvt5.result)
        # Example 6 : Pivot the input data with output column names.
        pvt6 = Pivoting(data = star, 
                        partition_columns = ['country','state'],
                        target_columns = ['sales', 'cogs', 'rating'],
                        accumulate = 'yr',
                        rows_per_partition = 3,
                        output_column_names=['sales_q1','sales_q2','sales_q3','cogs_q1','cogs_q2',
                                             'cogs_q3','rating_q1','rating_q2','rating_q3'],
                        data_partition_column = 'qtr',
                        data_order_column=['country','state'])
        # Print the result DataFrame.
        print( pvt6.result)
    PolynomialFeaturesFit
    PolynomialFeaturesFit
    Functions
    PolynomialFeaturesFit(data=None, target_columns=None, include_bias=True, interaction_only=False, degree=2, **generic_arguments)
    DESCRIPTION:
        PolynomialFeaturesFit() function stores all the specified values in the argument in a DataFrame format.
        All polynomial combinations of the features with degrees less than or equal to the specified degree are
        generated. For example, for a two-dimensional input sample [x, y], the degree-2 polynomial features are
        [x, y, x-squared, xy, y-squared, 1].
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" for which polynomial
            features needs to be generated.
            Types: str OR list of Strings (str)
        include_bias:
            Optional Argument.
            Specifies whether to include bias column in the output or not.
            A bias column acts as an intercept term in a linear model.
            Default Value: True
            Types: bool
        interaction_only:
            Optional Argument.
            Specifies whether to output polynomial combinations only for interaction features
            (features that are products of at most degree distinct input features).
            Default Value: False
            Types: bool
        degree:
            Optional Argument.
            Specifies the maximum degree of the input features to output polynomial combinations.
            Permitted Values: 1, 2, 3
            Default Value: 2
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of PolynomialFeaturesFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as PolynomialFeaturesFitObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. output_data
            2. output
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["numerics"])
        # Create teradataml DataFrame object.
        numerics = DataFrame.from_table("numerics")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Create fit object to create polynomial features for columns
        #            "integer_col" and "smallint_col".
        fit_obj = PolynomialFeaturesFit(data=numerics,
                                        target_columns=["integer_col", "smallint_col"],
                                        degree=2)
        # Print the result DataFrame.
        print(fit_obj.output)
        print(fit_obj.output_data)
    PolynomialFeaturesTransform
    PolynomialFeaturesTransform
    Functions
    PolynomialFeaturesTransform(data=None, object=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        PolynomialFeaturesTransform() function generates a feature matrix of all polynomial
        combinations of the feature by extracting the target column, degree, bias and interaction
        information from the output of the PolynomialFeaturesFit() function.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the output of generated by
            PolynomialFeaturesFit() function or the instance of PolynomialFeaturesFit.
            Types: teradataml DataFrame or PolynomialFeaturesFit
        accumulate:
            Optional Argument.
            Specifies the names of input teradataml DataFrame columns to copy to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of PolynomialFeaturesTransform.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as 
        PolynomialFeaturesTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["numerics"])
        # Create teradataml DataFrame object.
        numerics = DataFrame.from_table("numerics")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Generate feature DataFrame for all 2D polynomial combination of columns
        #            "integer_col"and "smallint_col".
        fit_obj = PolynomialFeaturesFit(data=numerics,
                                        target_columns=["integer_col", "smallint_col"],
                                        degree=2)
        # Print the result DataFrame.
        print(fit_obj.output)
        print(fit_obj.output_data)
        # Generate feature matrix. Note that teradataml DataFrame representing
        # the model is passed as input to "object".
        obj = PolynomialFeaturesTransform(data=numerics,
                                            object=fit_obj.output)
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Generate feature matrix. Note that model is passed as instance of
        #            PolynomialFeaturesFit to "object".
        obj1 = PolynomialFeaturesTransform(data=numerics,
                                           object=fit_obj)
        # Print the result DataFrame.
        print(obj1.result)
    RandomProjectionFit
    RandomProjectionFit
    Functions
    RandomProjectionFit(data=None, target_columns=None, num_components=None, seed=None, epsilon=0.1, density=0.33333333,
    projection_method='GAUSSIAN', output_featurenames_preﬁx='td_rpj_feature', **generic_arguments)
    DESCRIPTION:
        The RandomProjectionFit() function returns a random projection matrix
        based on the specified arguments.
        The function also returns the required parameters for transforming the
        input data into lower-dimensional data. The RandomProjectionTransform()
        function uses the RandomProjectionFit() output to reduce the
        dimensionality of the input data.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the columns/features for dimensionality reduction.
            Types: str OR list of Strings (str)
        num_components:
            Required Argument.
            Specifies the target dimension(number of features) into which data
            points from the original dimension will be projected.
            The "num_components" value cannot be greater than the original
            dimension (number of features) and must satisfy the Johnson-Lindenstrauss
            Lemma result. The minimum value allowed for the "num_components" argument
            is calculated using the RandomProjectionMinComponents() function.
            Types: int
        seed:
            Optional Argument.
            Specifies the random seed the function uses for repeatable results.
            The algorithm uses the seed to generate a random projection matrix.
            The seed must be a non-negative integer value.
            Default Value: The Random Seed value is used for generating a random
            projection matrix, and hence the output is non-deterministic.
            Types: int
        epsilon:
            Optional Argument.
            Specifies a value to control distortion introduced while projecting
            the data to a lower dimension. The amount of distortion increases
            if you increase the value.
            Accepts value between 0 and 1.
            Default Value: 0.1
            Types: float OR int
        density:
            Optional Argument.
            Specifies the approximate ratio of non-zero elements in the random
            projection matrix when SPARSE is used as the "projection_method".
            Permitted Values: (0,1]
            Default Value: 0.33333333
            Types: float OR int
        projection_method:
            Optional Argument.
            Specifies the method name for generating the random projection matrix.
            Default Value: "GAUSSIAN"
            Permitted Values: GAUSSIAN, SPARSE
            Types: str
        output_featurenames_prefix:
            Optional Argument.
            Specifies the prefix for the output column names.
            Default Value: "td_rpj_feature"
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of RandomProjectionFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as RandomProjectionFitObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["stock_movement"])
        # Create teradataml DataFrame objects.
        stock_movement = DataFrame.from_table("stock_movement")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Get random projection matrix for
        #             stock_movement DataFrame.
        RandomProjectionFit_out = RandomProjectionFit(data = stock_movement,
                                                      target_columns = "1:",
                                                      epsilon = 0.9,
                                                      num_components = 343
                                                      )
        # Print the result DataFrames.
        print(RandomProjectionFit_out.result)
        print(RandomProjectionFit_out.output_data)
    RandomProjectionMinComponents
    RandomProjectionFit
    Functions
    RandomProjectionFit(data=None, target_columns=None, num_components=None, seed=0, epsilon=0.1, density=0.33333333, projection_method='GAUSSIAN',
    output_featurenames_preﬁx='td_rpj_feature', **generic_arguments)
    DESCRIPTION:
        The RandomProjectionFit() function returns a random projection matrix based on the specified
        arguments.
        The function also returns the required parameters for transforming the input data into
        lower-dimensional data. The RandomProjectionTransform() function uses the RandomProjectionFit() output to
        reduce the dimensionality of the input data.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the columns/features for dimensionality reduction.
            Types: str OR list of Strings (str)
        num_components:
            Required Argument.
            Specifies the target dimension(number of features) into which data
            points from the original dimension will be projected.
            The "num_components" value cannot be greater than the original
            dimension (number of features) and must satisfy the Johnson-Lindenstrauss
            Lemma result. The minimum value allowed for the "num_components" argument
            is calculated using the RandomProjectionMinComponents() function.
            Types: int
        seed:
            Optional Argument.
            Specifies the random seed the function uses for repeatable results.
            The algorithm uses the seed to generate a random projection matrix.
            The seed must be a non-negative integer value.
            Default Value: The Random Seed value is used for generating a random
            projection matrix, and hence the output is non-deterministic.
            Types: int
        epsilon:
            Optional Argument.
            Specifies a value to control distortion introduced while projecting
            the data to a lower dimension. The amount of distortion increases
            if you increase the value.
            Accepts value between 0 and 1.
            Default Value: 0.1
            Types: float OR int
        density:
            Optional Argument.
            Specifies the approximate ratio of non-zero elements in the random
            projection matrix when SPARSE is used as the "projection_method".
            Permitted Values: (0,1]
            Default Value: 0.33333333
            Types: float
        projection_method:
            Optional Argument.
            Specifies the method name for generating the random projection matrix.
            Default Value: "GAUSSIAN"
            Permitted Values: GAUSSIAN, SPARSE
            Types: str
        output_featurenames_prefix:
            Optional Argument.
            Specifies the prefix for the output column names.
            Default Value: "td_rpj_feature"
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of RandomProjectionFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as RandomProjectionFitObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["stock_movement"])
        # Create teradataml DataFrame objects.
        stock_movement = DataFrame.from_table("stock_movement")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Get random projection matrix DataFrame for
        #             stock_movement DataFrame from column at index 1 to 10.
        RandomProjectionFit_out = RandomProjectionFit(data = stock_movement,
                                                      target_columns = "1:10",
                                                      epsilon = 0.9,
                                                      num_components = 343
                                                      )
        # Print the result DataFrames.
        print(RandomProjectionFit_out.result)
        print(RandomProjectionFit_out.output_data)
    RandomProjectionTransform
    RandomProjectionTransform
    Functions
    RandomProjectionTransform(object=None, data=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        The RandomProjectionTransform() function converts the
        high-dimensional input data to a low-dimensional space
        using the RandomProjectionFit() function output.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the output generated by
            RandomProjectionFit() function or the instance of RandomProjectionFit.
            Types: teradataml DataFrame or RandomProjectionFit
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output. By default, only transformed columns are present in the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the
                function in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Default Value: False
                Types: bool
            volatile:
                Optional Argument.
                Specifies whether to put the results of the
                function in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
        Function allows the user to partition, hash, order or local
        order the input data. These generic arguments are available
        for each argument that accepts teradataml DataFrame as
        input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or
                list of str (Strings)
            * "<input_data_arg_name>_hash_column" accepts str or list
                of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list
                of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if
            the underlying SQL Engine function supports, else an
            exception is raised.
    RETURNS:
        Instance of RandomProjectionTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as
        RandomProjectionTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "stock_movement")
        # Create teradataml DataFrame objects.
        stock_movement = DataFrame.from_table("stock_movement")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Get random projection matrix for
        #             stock_movement DataFrame.
        fit_obj = RandomProjectionFit(data = stock_movement,
                                      target_columns = "1:",
                                      epsilon = 0.9,
                                      num_components = 343)
        # Generate feature matrix. Note that teradataml DataFrame representing
        # the model is passed as input to "object".
        RandomProjectionTransform_out = RandomProjectionTransform(object = fit_obj.result,
                                                                  data = stock_movement)
        # Print the result DataFrame.
        print(RandomProjectionTransform_out.result)
        # Example 2 : Generate feature matrix. Note that model is passed as instance of
        #             RandomProjectionFit to "object".
        RandomProjectionTransform_out1 = RandomProjectionTransform(object = fit_obj,
                                                                   data = stock_movement)
        # Print the result DataFrame.
        print(RandomProjectionTransform_out1.result)
    RowNormalizeFit
    RowNormalizeFit
    Functions
    RowNormalizeFit(data=None, target_columns=None, approach='UNITVECTOR', base_column=None, base_value=None, **generic_arguments)
    DESCRIPTION:
        RowNormalizeFit() function outputs a DataFrame containing parameters and specified input columns
        to input to RowNormalizeTransform() function, which normalizes the input columns row-wise.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" to normalize.
            Types: str OR list of Strings (str)
        approach:
            Optional Argument.
            Specifies the method to use for row wise normalization.
            Permitted Values:
                * UNITVECTOR - X' = X / (sqrt (Σᵢϵ₍₁, ₙ₎ Xᵢ²))
                * FRACTION - X' = X / (Σᵢϵ₍₁, ₙ₎ Xᵢ)
                * PERCENTAGE - X' = X*100 / (Σᵢϵ₍₁, ₙ₎ Xᵢ)
                * INDEX - X' = V + ((X - B) / B) * 100
                In the normalizing formulas:
                    X' is the normalized value.
                    X is the original value.
                    B is the value in the base column.
                    V is the base value.
            Default Value: "UNITVECTOR"
            Types: str
        base_column:
            Required when "approach" is set to 'INDEX', ignored otherwise.
            Specifies the base column to be used in INDEX "approach" formula.
            Types: str
        base_value:
            Required when "approach" is set to 'INDEX', ignored otherwise.
            Specifies the base value to be used in INDEX "approach" formula.
            Types: float OR int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of RowNormalizeFit.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as RowNormalizeFitObj.<attribute_name>.
        Output teradataml DataFrame attribute name are:
            1. output_data
            2. output
    RAISES
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["numerics"])
        # Create teradataml DataFrame object.
        numerics = DataFrame.from_table("numerics")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Create fit object to normalize ""smallint_col" and "integer_col"
        #            columns using "INDEX" approach, "integer_col" as base column
        #            and base value as 100.0.
        fit_obj = RowNormalizeFit(data=numerics,
                                  target_columns=["integer_col", "smallint_col"],
                                  approach="INDEX",
                                  base_column="integer_col",
                                  base_value=100.0)
        # Print the result DataFrame.
        print(fit_obj.output)
        print(fit_obj.output_data)
    RowNormalizeTransform
    RowNormalizeTransform
    Functions
    RowNormalizeTransform(data=None, object=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        RowNormalizeTransform() function normalizes input columns row-wise, using
        RowNormalizeFit() function output.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the output generated by
            RowNormalizeFit() function or the instance of RowNormalizeFit.
            Types: teradataml DataFrame or RowNormalizeFit
        accumulate:
            Optional Argument.
            Specifies the names of input teradataml DataFrame columns to copy to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of RowNormalizeTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as RowNormalizeTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["numerics"])
        # Create teradataml DataFrame.
        numerics = DataFrame.from_table("numerics")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Normalize "smallint_col" and "integer_col" columns using "INDEX"
        #            approach, "integer_col" as base column and base value as 100.
        fit_obj = RowNormalizeFit(data=numerics,
                                  target_columns=["integer_col", "smallint_col"],
                                  approach="INDEX",
                                  base_column="integer_col",
                                  base_value=100.0)
        # Print the result DataFrame.
        print(fit_obj.output)
        print(fit_obj.output_data)
        # Normalize the columns and accumulate the result by "id_col" column.
        # Note that teradataml DataFrame representing the model is passed as
        # input to "object".
        obj = RowNormalizeTransform(data=numerics,
                                    object=fit_obj.output,
                                    accumulate="id_col")
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Function to normalize the columns and accumulate the result
        #            by "id_col" column. Note that model is passed as instance of
        #            RowNormalizeFit to "object".
        obj1 = RowNormalizeTransform(data=numerics,
                                     object=fit_obj,
                                     accumulate="id_col")
        # Print the result DataFrame.
        print(obj1.result)
    ScaleFit
    ScaleFit
    Functions
    ScaleFit(data=None, target_columns=None, scale_method=None, miss_value='KEEP', global_scale=False, multiplier='1', intercept='0', parameter_data=None,
    attribute_data=None, partition_columns=None, ignoreinvalid_locationscale=False, unused_attributes='UNSCALED', attribute_name_column=None,
    attribute_value_column=None, target_attributes=None, **generic_arguments)
    DESCRIPTION:
        ScaleFit() function outputs statistics to input to ScaleTransform() function,
        which scales specified input DataFrame columns.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the input teradataml DataFrame column(s) for which to output statistics.
            The columns must contain numeric data in the range (-1e³⁰⁸, 1e³⁰⁸).
            Note:
                * This argument cannot be used with "target_attributes", "attribute_name_column", 
                 "attribute_value_column".
            Types: str OR list of Strings (str)
        scale_method:
            Required Argument.
            Specifies the scale method to be used for scaling. If one value is
            provided, it applies to all target columns. If more than one value is
            specified, scale method values applies to target columns values in the
            order specified by the user.
            ScaleTransform() function uses the location and scale values in the
            following formula to scale target column value X to scaled value X':
                X' = intercept + multiplier * ((X - location)/scale)
            "intercept" and "multiplier" arguments determine intercept and multiplier.
            In the table, Xmin, Xmax, and XMean are the minimum, maximum, and mean
            values of the target column.
            +--------------+---------------------------+-----------------+------------------------------+
            | scale_method |        Description        |     location    |            scale             |
            +--------------+---------------------------+-----------------+------------------------------+
            |    MAXABS    |  Maximum absolute value.  |        0        |         Maximum |X|          |
            |              |                           |                 |                              |
            |     MEAN     |           Mean.           |      XMean      |              1               |
            |              |                           |                 |                              |
            |   MIDRANGE   |         Midrange.         |  (Xmax+Xmin)/2  |        (Xmax-Xmin)/2         |
            |              |                           |                 |                              |
            |    RANGE     |           Range.          |       Xmin      |          Xmax-Xmin           |
            |              |                           |                 |                              |
            |   RESCALE    |  Rescale using specified  | See table after |       See table after        |
            |              | lower bound, upper bound, | RESCALE syntax. |       RESCALE syntax.        |
            |              |     or both.See syntax    |                 |                              |
            |              |     after this table.     |                 |                              |
            |              |                           |                 |                              |
            |     STD      |    Standard deviation.    |      XMean      |   √(∑((Xi - Xmean)2)/ N)     |
            |              |                           |                 |     where N is count of      |
            |              |                           |                 |        valid values.         |
            |              |                           |                 |                              |
            |     SUM      |            Sum.           |        0        |              ΣX              |
            |              |                           |                 |                              |
            |     USTD     |     Unbiased standard     |      XMean      | √(∑((Xi - Xmean)2)/ (N - 1)) |
            |              |         deviation.        |                 |     where N is count of      |
            |              |                           |                 |        valid values.         |
            +--------------+---------------------------+-----------------+------------------------------+
            RESCALE ({ lb=lower_bound | ub=upper_bound | lb=lower_bound, ub=upper_bound })
            +------------------------+-----------------------------+----------------------------+
            |                        |           location          |           scale            |
            +------------------------+-----------------------------+----------------------------+
            |    Lower bound only    |      Xmin - lower_bound     |             1              |
            |                        |                             |                            |
            |    Upper bound only    |      Xmax - upper_bound     |             1              |
            |                        |                             |                            |
            | Lower and upper bounds |     Xmin - (lower_bound/    |       (Xmax - Xmin)/       |
            |                        | (upper_bound- lower_bound)) | (upper_bound- lower_bound) |
            +------------------------+-----------------------------+----------------------------+
            Permitted Values:
                * MAXABS
                * MEAN
                * MIDRANGE
                * RANGE
                * RESCALE
                * STD
                * SUM
                * USTD
            Types: str OR list of Strings (str)
        miss_value:
            Optional Argument.
            Specifies how to process NULL values in input.
            Permitted Values:
                * KEEP: Keep NULL values.
                * ZERO: Replace each NULL value with zero.
                * LOCATION: Replace each NULL value with its location value.
            Default Value: "KEEP"
            Types: str
        global_scale:
            Optional Argument.
            Specifies whether all input columns are scaled to the same location
            and scale. When set to False, each input column is scaled separately.
            Default Value: False
            Types: bool
        multiplier:
            Optional Argument.
            Specifies one or more multiplying factors(multiplier) to apply to the input data.
            If only one multiplier is specified, it applies to all target columns.
            If a list of multipliers is specified, each multiplier applies to the
            corresponding target column.
            Default Value: "1"
            Types: str OR list of String (str)
        intercept:
            Optional Argument.
            Specifies one or more addition factors(intercept) incrementing the scaled results.
            If only one intercept specified, it applies to all target columns.
            If a list of intercepts is specified, each intercept applies to the
            corresponding target column.
            The syntax of intercept is:
                [-]{number | min | mean | max }
            where min, mean, and max are the global minimum, maximum, mean values
            in the corresponding columns. The function scales the values of min,
            mean, and max.
                For example, if intercept is "- min" and multiplier is
                1, the scaled result is transformed to a non-negative sequence
                according to this formula, where scaledmin is the scaled value:
                    X = -scaledmin + 1 * (X - location)/scale.
            Default Value: "0"
            Types: str OR list of String (str)
        parameter_data:
            Optional Argument.
            Specifies the input teradataml DataFrame containing the parameters.
            Note:
                * This is valid when "data_partition_column" is used.
            Types: teradataml DataFrame
        attribute_data:
            Optional Argument.
            Specifies the input teradataml DataFrame containing the attributes.
            Note:
                * This is valid when "data_partition_column" is used.
            Types: teradataml DataFrame
        partition_columns:
            Optional Argument.
            Specifies the column name in the "data" to partition the input.
            Types: str OR list of Strings (str)
        ignoreinvalid_locationscale:
            Optional Argument.
            Specifies whether to ignore invalid values of location and scale parameters.
            Default Value: False
            Types: bool
        unused_attributes:
            Optional Argument.
            Specifies whether to emit out unused attributes of different partitions
            as unscaled values or NULLs (for dense input).
            Permitted Values: 'NULLIFY', 'UNSCALED'
            Default Value: 'UNSCALED'
            Types: str
        attribute_name_column:
            Optional Argument.
            Specifies the column name in the "attribute_data" which contains attribute names.
            Note:
                * This is required for sparse input.
            Types: str
        attribute_value_column:
            Optional Argument.
            Specifies the column name in the "attribute_data" which contains attribute values.
            Note:
                * This is required for sparse input.
            Types: str
        target_attributes:
            Optional Argument.
            Specifies the attributes for which scaling should be performed.
            Note:
                * This is required for sparse input.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of ScaleFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ScaleFitObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. output
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["scale_housing"])
        load_example_data('scale', ["scale_attributes", "scale_parameters",
        "scale_input_partitioned", "scale_input_sparse","scale_input_part_sparse"])
        # Create teradataml DataFrame.
        scaling_house = DataFrame.from_table("scale_housing")
        scale_attribute = DataFrame.from_table("scale_attributes")
        scale_parameter = DataFrame.from_table("scale_parameters")
        scale_inp_part = DataFrame.from_table("scale_input_partitioned")
        scale_inp_sparse = DataFrame.from_table("scale_input_sparse")
        scale_inp_part_sparse = DataFrame.from_table("scale_input_part_sparse")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Create statistics to scale "lotsize" with respect to
        #            mean value of the column.
        fit_obj = ScaleFit(data=scaling_house,
                           target_columns="lotsize",
                           scale_method="MEAN",
                           miss_value="KEEP",
                           global_scale=False,
                           multiplier="1",
                           intercept="0")
        # Print the result DataFrame.
        print(fit_obj.output)
        print(fit_obj.output_data)
        # Example 2: Create statistics to scale "fare" and "age" columns
        #            with respect to maximum absolute value with partition column
        #            for dense input.
        fit_obj = ScaleFit(data=scale_inp_part,
                           attribute_data=scale_attribute,
                           parameter_data=scale_parameter,
                           target_columns=['fare', 'age'],
                           scale_method="maxabs",
                           miss_value="zero",
                           global_scale=False,
                           data_partition_column='pid',
                           attribute_data_partition_column='pid',
                           parameter_data_partition_column='pid')
        # Print the result DataFrame.
        print(fit_obj.output)
        print(fit_obj.output_data)
        # Example 3: Create statistics to scale "fare" column with respect to
        #            range for sparse input.
        fit_obj = ScaleFit(data=scale_inp_sparse,
                           target_attribute=['fare'],
                           scale_method="range",
                           miss_value="keep",
                           global_scale=False,
                           attribute_name_column='attribute_column',
                           attribute_value_column='attribute_value')
        # Print the result DataFrame.
        print(fit_obj.output)
        print(fit_obj.output_data)
        # Example 4: Create statistics to scale "fare" column with respect to
        #            maximum absolute value for sparse input with partition column.
        fit_obj = ScaleFit(data=scale_inp_part_sparse,
                            parameter_data=scale_parameter,
                            attribute_data=scale_attribute,
                            scale_method="maxabs",
                            miss_value="zero",
                            global_scale=False,
                            attribute_name_column='attribute_column',
                            attribute_value_column='attribute_value',
                            data_partition_column='pid',
                            attribute_data_partition_column='pid',
                            parameter_data_partition_column='pid')
        # Print the result DataFrame.
        print(fit_obj.output)
        print(fit_obj.output_data)
    Scale Transform
    ScaleTransform
    Functions
    ScaleTransform(data=None, object=None, accumulate=None, attribute_name_column=None, attribute_value_column=None, **generic_arguments)
    DESCRIPTION:
        ScaleTransform() function scales specified columns in input data, using ScaleFit() function output.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the output of generated by
            ScaleFit() function or the instance of ScaleFit.
            Types: teradataml DataFrame or ScaleFit
        accumulate:
            Optional Argument.
            Specifies the names of input teradataml DataFrame columns to copy to the output.
            Types: str OR list of Strings (str)
        attribute_name_column:
            Optional Argument.
            Specifies the column name in the "attribute_data" which contains attribute names.
            Note:
                * This is required for sparse input.
            Types: str
        attribute_value_column:
            Optional Argument.
            Specifies the column name in the "attribute_data" which contains attribute values.
            Note:
                * This is required for sparse input.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of ScaleTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ScaleTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["scale_housing"])
        load_example_data('scale', ["scale_attributes", "scale_parameters",
        "scale_input_partitioned", "scale_input_sparse","scale_input_part_sparse"])
        # Create teradataml DataFrame.
        scaling_house = DataFrame.from_table("scale_housing")
        scale_attribute = DataFrame.from_table("scale_attributes")
        scale_parameter = DataFrame.from_table("scale_parameters")
        scale_inp_part = DataFrame.from_table("scale_input_partitioned")
        scale_inp_sparse = DataFrame.from_table("scale_input_sparse")
        scale_inp_part_sparse = DataFrame.from_table("scale_input_part_sparse")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Scale "lotsize" with respect to mean value of the column.
        fit_obj = ScaleFit(data=scaling_house,
                           target_columns="lotsize",
                           scale_method="MEAN",
                           miss_value="KEEP",
                           global_scale=False,
                           multiplier="1",
                           intercept="0")
        # Print the result DataFrame.
        print(fit_obj.output)
        # Scale "lotsize" column.
        # Note that teradataml DataFrame representing the model is passed as
        # input to "object".
        obj = ScaleTransform(data=scaling_house,
                             object=fit_obj.output,
                             accumulate="price")
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Scale "lotsize" column. Note that model is passed as instance of
        #            ScaleFit to "object".
        obj1 = ScaleTransform(data=scaling_house,
                              object=fit_obj,
                              accumulate="price")
        # Print the result DataFrame.
        print(obj1.result)
        # Example 3: Create statistics to scale "fare" and "age" columns with respect to
        #            maximum absolute value for partitioned input.
        fit_obj = ScaleFit(data=scale_inp_part,
                           attribute_data=scale_attribute,
                           parameter_data=scale_parameter,
                           target_columns=['fare', 'age'],
                           scale_method="maxabs",
                           miss_value="zero",
                           global_scale=False,
                           data_partition_column='pid',
                           attribute_data_partition_column='pid',
                           parameter_data_partition_column='pid')
        obj = ScaleTransform(data=scale_inp_part,
                             object=fit_obj.output,
                             accumulate=['pid','passenger'],
                             data_partition_column='pid',
                             object_partition_column='pid')
        # Print the result DataFrame.
        print(obj.result)
        # Example 4: Create statistics to scale "fare" column with respect to
        #            range for sparse input.
        fit_obj = ScaleFit(data=scale_inp_sparse,
                           target_attribute=['fare'],
                           scale_method="range",
                           miss_value="keep",
                           global_scale=False,
                           attribute_name_column='attribute_column',
                           attribute_value_column='attribute_value')
        obj = ScaleTransform(data=scale_inp_sparse,
                             object=fit_obj.output,
                             accumulate=['passenger'],
                             attribute_name_column='attribute_column',
                             attribute_value_column='attribute_value'
                     )
        # Print the result DataFrame.
        print(obj.result)
        # Example 5: Create statistics to scale "fare" column with respect to
        #            maximum absolute value for sparse input with partition column.
        fit_obj = ScaleFit(data=scale_inp_part_sparse,
                           parameter_data=scale_parameter,
                           attribute_data=scale_attribute,
                           scale_method="maxabs",
                           miss_value="zero",
                           global_scale=False,
                           attribute_name_column='attribute_column',
                           attribute_value_column='attribute_value',
                           data_partition_column='pid',
                           attribute_data_partition_column='pid',
                           parameter_data_partition_column='pid')
        obj = ScaleTransform(data=scale_inp_part_sparse,
                             object=fit_obj.output,
                             accumulate=["passenger",'pid'],
                             attribute_name_column='attribute_column',
                             attribute_value_column='attribute_value',
                             object_partition_column='pid',
                             data_partition_column='pid')
        # Print the result DataFrame.
        print(obj.result)
    SimpleImputeFit
    SimpleImputeFit
    Functions
    SimpleImputeFit(data=None, stats_columns=None, literals_columns=None, partition_column=None, stats=None, literals=None, **generic_arguments)
    DESCRIPTION:
        SimpleImputeFit() function outputs values to substitute for missing
        values in the input data. The output values are input to SimpleImputeTransform()
        function, which makes the substitutions.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        stats_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) in "data" for which to calculate
            the statistics.
            Types: str OR list of Strings (str)
        literals_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) in "data" for which to impute literals.
            Types: str OR list of Strings (str)
        partition_column:
            Optional Argument.
            Specifies the name(s) of the column(s) in "data" to partition on.
            Types: str OR list of Strings (str)
        stats:
            Optional Argument.
            Specifies the stats to compute on input teradataml DataFrame columns.
            Permitted Values: MIN, MAX, MEAN, MEDIAN, MODE
            Types: str OR list of Strings (str)
        literals:
            Optional Argument.
            Specifies the literal value to impute on input teradataml DataFrame
            columns.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of SimpleImputeFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SimpleImputeFitObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. output
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame.
        titanic = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Create stats for "fare" column and impute value "2"
        #            in "pclass" column.
        fit_obj = SimpleImputeFit(data=titanic,
                                  stats_columns="fare",
                                  literals_columns="pclass",
                                  partition_column="sex",
                                  stats="median",
                                  literals="2")
        # Print the result DataFrame.
        print(fit_obj.output)
        print(fit_obj.output_data)
    SimpleImputeTransform
    SimpleImputeTransform
    Functions
    SimpleImputeTransform(data=None, object=None, **generic_arguments)
    DESCRIPTION:
        SimpleImputeTransform() function substitutes specified values for missing values
        in the input data. The specified values is generated by SimpleImputeFit() function
        output.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the output of SimpleImputeFit() function
            or the instance of SimpleImputeFit.
            Types: teradataml DataFrame or SimpleImputeFit
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of SimpleImputeTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SimpleImputeTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame.
        titanic = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Fill missing values of "age" column with median value and
        #            impute value "General" on "cabin" column.
        fit_obj = SimpleImputeFit(data=titanic,
                                  stats_columns="age",
                                  literals_columns="cabin",
                                  stats="median",
                                  literals="General")
        # Print the result DataFrame.
        print(fit_obj.output)
        # Impute the values for missing values.
        # Note that teradataml DataFrame representing the model is passed as
        # input to "object".
        obj =  SimpleImputeTransform(data=titanic,
                                     object=fit_obj.output)
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Impute the values for missing values. Note that model is passed
        #            as instance of SimpleImputeFit to "object".
        obj1 =  SimpleImputeTransform(data=titanic,
                                      object=fit_obj)
        # Print the result DataFrame.
        print(obj1.result)
    SimpleImputeTransform
    SMOTE
    Functions
    SMOTE(data=None, encoding_data=None, id_column=None, response_column=None, input_columns=None, categorical_columns=None,
    median_standard_deviation=None, minority_class=None, oversampling_factor=5, sampling_strategy='smote', ﬁll_sampleid=True,
    noninput_columns_value='sample', n_neighbors=5, seed=None, **generic_arguments)
    DESCRIPTION:
        SMOTE() function generates data by oversampling a minority class using 
        smote, adasyn, borderline-2 or smote-nc algorithms.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        encoding_data:
            Optional Argument, Required when "sampling_strategy" is set to 'smotenc' algorithm.
            Specifies the teradataml dataframe containing the ordinal encoding information.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the name of the column in "data" that 
            uniquely identifies a data sample.
            Types: str
        response_column:
            Optional Argument.
            Specifies the name of the column in "data" that contains the 
            numeric value to be used as the response value for a sample.
            Types: str
        input_columns:
            Required Argument.
            Specifies the name of the input columns in "data" for oversampling.
            Types: str OR list of Strings (str)
        categorical_columns:
            Optional Argument, Required when "sampling_strategy" is set to 'smotenc' algorithm.
            Specifies the name of the categorical columns in the "data" that 
            the function uses for oversampling with smotenc.
            Types: str OR list of Strings (str)
        median_standard_deviation:
            Optional Argument, Required when "sampling_strategy" is set to 'smotenc' algorithm.
            Specifies the median of the standard deviations computed over the 
            numerical input columns.
            Types: float
        minority_class:
            Required Argument.
            Specifies the minority class for which synthetic samples need to be 
            generated. 
            Note:
                * The label for minority class under response column must be numeric integer.
            Types: str
        oversampling_factor:
            Optional Argument.
            Specifies the factor for oversampling the minority class.
            Default Value: 5
            Types: float
        sampling_strategy:
            Optional Argument.
            Specifies the oversampling algorithm to be used to create synthetic samples.
            Default Value: "smote"
            Permitted Values: "smote", "adasyn", "borderline", "smotenc"
            Types: str
        fill_sampleid:
            Optional Argument.
            Specifies whether to include the id of the original observation used 
            to generate each synthetic observation.
            Default Value: True
            Types: bool
        noninput_columns_value:
            Optional Argument.
            Specifies the value to put in a sample column for columns not 
            specified as input columns.
            Default Value: "sample"
            Permitted Values: "sample", "neighbor", "null"
            Types: str
        n_neighbors:
            Optional Argument.
            Specifies the number of nearest neighbors for choosing the sample to 
            be used in oversampling.
            Default Value: 5
            Types: int
        seed:
            Optional Argument.
            Specifies the random seed the algorithm uses for repeatable results. 
            The function uses the seed for random interpolation and generate the 
            synthetic sample.
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of SMOTE.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as SMOTEObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the 
        #        function in user space.
        #     2. User can import the function, if it is available on 
        #        Vantage user is connected to.
        #     3. To check the list of analytic functions available on 
        #        Vantage user connected to, use 
        #        "display_analytic_functions()".
        # Load the example data.
        load_example_data("dataframe", "iris_test")
        load_example_data("teradataml", "titanic")
        # Create teradataml DataFrame objects.
        iris_input = DataFrame.from_table("iris_test").iloc[:25]
        titanic_input = DataFrame("titanic").iloc[:50]
        # Create Encoding DataFrame objects.
        encoded_data = OrdinalEncodingFit(data=titanic_input,
                                          target_column=['sex','embarked'],
                                          approach="AUTO")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function SMOTE.
        from teradataml import SMOTE
        # Example 1 : Generate synthetic samples using smote algorithm.
        smote_out = SMOTE(data = iris_input,
                          n_neighbors = 5,
                          id_column='id',
                          minority_class='3',
                          response_column='species',
                          input_columns =['sepal_length', 'sepal_width', 'petal_length', 'petal_width'],
                          oversampling_factor=2,
                          sampling_strategy='smote',
                          seed=10)
        # Print the result DataFrame.
        print(smote_out.result)
        # Example 2 : Generate synthetic samples using smotenc algorithm with categorical columns.
        smote_out2 = SMOTE(data = titanic_input, 
                           encoding_data = encoded_data.result, 
                           id_column = 'passenger', 
                           response_column = 'survived', 
                           input_columns = ['parch', 'age', 'sibsp'], 
                           categorical_columns = ['sex', 'embarked'],
                           median_standard_deviation = 31.47806044604718,
                           minority_class = '1', 
                           oversampling_factor = 5,
                           sampling_strategy = "smotenc", 
                           noninput_columns_value = "null", 
                           n_neighbors = 5)
        # Print the result DataFrame.
        print(smote_out2.result)
    TargetEncodingFit
    TargetEncodingFit
    Functions
    TargetEncodingFit(data=None, category_data=None, encoder_method=None, target_columns=None, response_column=None, alpha_prior=None,
    beta_prior=None, alpha_priors=None, num_distinct_responses=None, u0_prior=None, v0_prior=None, alpha0_prior=None, beta0_prior=None,
    default_values=None, **generic_arguments)
    DESCRIPTION:
        The TargetEncodingFit() function generally uses the likelihood or expected
        value of the target variable for each category and encodes that category with
        that value. This technique works for both binary classification and regression
        and for multiclass classification a similar technique is applied, which encodes
        the categorical variable with k new variables, where k is the number of classes.
        The TargetEncodingFit() function takes the input data and a categorical data as
        input and generates the required hyperparameters, which will be used by the
        TargetEncodingTransform() function for encoding the categorical values.
        Notes:
            * This function requires the UTF8 client character set.
            * This function does not support Pass-Through Characters (PTCs).
            * This function does not support KanjiSJIS or Graphic data types.
            * The maximum number of unique categories in the particular
              column is 4000.
            * The maximum category length is 128 characters.
            * Columns with a large number of distinct categories can have an
              impact on query execution time.
        Usage considerations for TargetEncodingFit() function are:
            * The input data in the TargetEncodingFit() function can have no
              partition at all or have data_partition_column="ANY" .
            * The TargetEncodingFit() function requires a category data to be
              passed as a dimension. The category data should be generated by the
              CategoricalSummary() function.
            * Null categories will not be encoded.
            * The "default_values" argument should be provided to TargetEncodingFit()
              if user want to assign any target value for missing categories in the
              TargetEncodingTransform() function.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data containing the categorical
            target columns.
            Types: teradataml DataFrame
        category_data:
            Required Argument.
            Specifies the data containing the unique categories and their counts
            for each target columns.
            Types: teradataml DataFrame
        encoder_method:
            Required Argument.
            Specifies the encoder method:
                * If the response variable is following a binary classification,
                  for example, values are either 0 or 1, use "encoder_method" as 'CBM_BETA'.
                * If the response variable is following a multi-class classification,
                  for example, values are (1,...,k, where k is the number of classes),
                  use "encoder_method" as 'CBM_DIRICHLET'.
                * If the response variable is following a regression, for example,
                  values are contiguous numeric values, use "encoder_method" as
                  'CBM_GAUSSIAN_INVERSE_GAMMA'.
            Notes:
                * The maximum length supported is 128.
                * "encoder_method" are not case sensitive.
            Permitted Values: CBM_BETA, CBM_DIRICHLET, CBM_GAUSSIAN_INVERSE_GAMMA
            Types: str
        target_columns:
            Required Argument.
            Specifies the column from the "data" that contains the categorical values
            to be encoded.
            Notes:
                * The maximum length supported is 128.
                * The maximum list length is 2018.
                * "target_columns" are not case sensitive.
            Types: str OR list of Strings (str)
        response_column:
            Required Argument.
            Specifies column from the "data" that contains the response values.
            Notes:
                * The maximum length supported is 128.
                * "response_column" are not case sensitive.
            Types: str
        alpha_prior:
            Optional Argument.
            Specifies the prior parameter of the 'CBM_BETA' encoder method.
            Types: int
        beta_prior:
            Optional Argument.
            Specifies the prior parameter of the 'CBM_BETA' encoder method.
            Types: int
        alpha_priors:
            Optional Argument.
            Specifies the prior parameter of the 'CBM_DIRICHLET' encoder method.
            Notes:
                * The number of values specified in this argument must be equal to
                  "num_distinct_responses" value.
                * The maximum list length is 2018.
            Types: int OR list of ints
        num_distinct_responses:
            Required when "encoder_method" is 'CBM_DIRICHLET',
            optional otherwise.
            Specifies the number of distinct values present in the
            "response_column".
            Types: int
        u0_prior:
            Optional Argument.
            Specifies the prior parameter of the 'CBM_GAUSSIAN_INVERSE_GAMMA'
            encoder method.
            Types: int
        v0_prior:
            Optional Argument.
            Specifies the prior parameter of the 'CBM_GAUSSIAN_INVERSE_GAMMA'
            encoder method.
            Types: int
        alpha0_prior:
            Optional Argument.
            Specifies the prior parameter of the 'CBM_GAUSSIAN_INVERSE_GAMMA'
            encoder method.
            Types: int
        beta0_prior:
            Optional Argument.
            Specifies the prior parameter of the 'CBM_GAUSSIAN_INVERSE_GAMMA'
            encoder method.
            Types: int
        default_values:
            Optional Argument.
            Specifies the values to use when the category is not found during transform.
            When only one value is specified, it will be applied to all the target columns,
            otherwise the number of default values must be equal to the number of target
            columns.
            Note:
                * The maximum list length is 2018.
            Types: int OR list of ints
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of TargetEncodingFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as TargetEncodingFitObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame objects.
        data_input = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Find the distinct values and counts for column 'sex' and 'embarked'.
        categorical_summ = CategoricalSummary(data = data_input,
                                              target_columns = ["sex", "embarked"]
                                              )
        # Find the distinct count of 'sex' and 'embarked' in which only 2 column should be present
        # name 'ColumnName' and 'CategoryCount'.
        category_data=categorical_summ.result.groupby('ColumnName').count()
        category_data = category_data.assign(drop_columns = True,
                                             ColumnName = category_data.ColumnName,
                                             CategoryCount = category_data.count_DistinctValue)
        # Example 1 : Generates the required hyperparameters when "encoder_method" is
        #             'CBM_BETA'.
        TargetEncodingFit_out1 = TargetEncodingFit(data = data_input,
                                                   category_data = category_data,
                                                   encoder_method = 'CBM_BETA',
                                                   target_columns = ['sex', 'embarked'],
                                                   response_column = 'survived',
                                                   default_values = [-1, -2]
                                                   )
        # Print the result DataFrame.
        print(TargetEncodingFit_out1.result)
        print(TargetEncodingFit_out1.output_data)
        # Example 2 : Generates the required hyperparameters when "encoder_method" is
        #             'CBM_DIRICHLET'.
        TargetEncodingFit_out2 = TargetEncodingFit(data = data_input,
                                                   category_data = category_data,
                                                   encoder_method = 'CBM_DIRICHLET',
                                                   target_columns = ['sex', 'embarked'],
                                                   response_column = 'pclass',
                                                   num_distinct_responses = 3
                                                   )
        # Print the result DataFrame.
        print(TargetEncodingFit_out2.result)
        print(TargetEncodingFit_out2.output_data)
        # Example 3 : Generates the required hyperparameters when "encoder_method" is
        #             'CBM_GAUSSIAN_INVERSE_GAMMA'.
        TargetEncodingFit_out3 = TargetEncodingFit(data = data_input,
                                                   category_data = category_data,
                                                   encoder_method = 'CBM_GAUSSIAN_INVERSE_GAMMA',
                                                   target_columns = ['sex', 'embarked'],
                                                   response_column = 'age',
                                                   default_values = [-1, -2]
                                                   )
        # Print the result DataFrame.
        print(TargetEncodingFit_out3.result)
        print(TargetEncodingFit_out3.output_data)
    TargetEncodingTransform
    TargetEncodingTransform
    Functions
    TargetEncodingTransform(data=None, object=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        The TargetEncodingTransform() function takes the input data
        and a Fit data generated by the TargetEncodingFit() function
        for encoding the categorical values.
        Notes:
            * This function requires the UTF8 client character set.
            * This function does not support Pass-Through Characters (PTCs).
            * This function does not support KanjiSJIS or Graphic data types.
        Usage considerations for TargetEncodingTransform are:
            * Errors are generated in these cases:
                * When the Fit data does not meet the criteria.
                * When category from input data is not found in the Fit data and
                  the "default_values" argument is also not used during
                  TargetEncodingFit() function.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the TargetEncodingFit
            parameters generated by TargetEncodingFit() function or the instance
            of TargetEncodingFit.
            Types: teradataml DataFrame or TargetEncodingFit
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to
            be copied to the output.
            Notes:
                 * The maximum length supported is 128.
                 * The maximum list length is 2047.
                 * "accumulate" are not case sensitive.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of TargetEncodingTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as TargetEncodingTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame objects.
        data_input = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Find the distinct values and counts for column 'sex' and 'embarked'.
        categorical_summ = CategoricalSummary(data=data_input,
                                              target_columns = ["sex", "embarked"]
                                              )
        # Find the distinct count of 'sex' and 'embarked' in which only 2 column should be present
          name 'ColumnName' and 'CategoryCount'.
        category_data=categorical_summ.result.groupby('ColumnName').count()
        category_data = category_data.assign(drop_columns = True,
                                             ColumnName = category_data.ColumnName,
                                             CategoryCount = category_data.count_DistinctValue)
        # Generates the required hyperparameters when "encoder_method" is 'CBM_BETA'.
        TargetEncodingFit_out = TargetEncodingFit(data = data_input,
                                                  category_data = category_data,
                                                  encoder_method = 'CBM_BETA',
                                                  target_columns = ['sex', 'embarked'],
                                                  response_column = 'survived',
                                                  default_values = [-1, -2]
                                                  )
        # Example 1 : Encode the column 'sex' and 'embarked'.
        TargetEncodingTransform_out = TargetEncodingTransform(data = data_input,
                                                              object = TargetEncodingFit_out,
                                                              accumulate = "passenger"
                                                              )
        # Print the result DataFrame.
        print(TargetEncodingTransform_out.result)
    TFIDF
    TFIDF
    Functions
    TFIDF(data=None, doc_id_column=None, token_column=None, tf_normalization='NORMAL', idf_normalization='LOG', regularization='NONE', accumulate=None,
    **generic_arguments)
    DESCRIPTION:
        Function takes any document set and computes the Term Frequency (TF), 
        Inverse Document Frequency (IDF), and Term Frequency Inverse Document 
        Frequency (TF-IDF) scores for each term.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame that contains
            the document id and the term.
            Types: teradataml DataFrame
        doc_id_column:
            Required Argument.
            Specifies the name of the column in "data" that contains the 
            document identifier.
            Types: str
        token_column:
            Required Argument.
            Specifies the name of the column in "data" that contains the tokens.
            Types: str
        tf_normalization:
            Optional Argument.
            Specifies the normalization method for calculating the term frequency (TF).
            Default Value: "NORMAL"
            Permitted Values: BOOL, COUNT, NORMAL, LOG, AUGMENT
            Types: str
        idf_normalization:
            Optional Argument.
            Specifies the normalization method for calculating the inverse 
            document frequency (IDF).
            Default Value: "LOG"
            Permitted Values: UNARY, LOG, LOGNORM, SMOOTH
            Types: str
        regularization:
            Optional Argument.
            Specifies the regularization method for calculating the TF-IDF score. 
            Default Value: "NONE"
            Permitted Values: L2, L1, NONE
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of TFIDF.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as  TFIDFObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the 
        #        function in user space.
        #     2. User can import the function, if it is available on 
        #        Vantage user is connected to.
        #     3. To check the list of analytic functions available on 
        #        Vantage user connected to, use 
        #        "display_analytic_functions()".
        # Load the example data.
        load_example_data('naivebayestextclassifier',"token_table")
        # Create teradataml DataFrame objects.
        inp = DataFrame.from_table('token_table')
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function  TFIDF.
        from teradataml import  TFIDF
        # Example 1 : Compute the TF, IDF and TF-IDF scores
        #             for each term in the input data.
        TFIDF_out = TFIDF(data=inp, 
                          doc_id_column='doc_id',
                          token_column='token',
                          tf_normalization = "LOG", 
                          idf_normalization = "SMOOTH", 
                          regularization = "L2",
                          accumulate=['category'])
        # Print the result DataFrame.
        print(TFIDF_out.result)
    Transform
    Transform
    Functions
    Transform(data=None, object=None, id_columns=None, **generic_arguments)
    DESCRIPTION:
        The Transform() function applies numeric transformations to input columns,
        using Fit() output.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame which contains the model data generated by
            the Fit() function or instance of Fit.
            Types: teradataml DataFrame or Fit
        id_columns:
            Optional Argument.
            Specifies the input teradataml DataFrame numeric columns to exactly
            copy to the output. By default, all numeric columns will be converted
            to float.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of Transform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as TransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["iris_input", "transformation_table"])
        # Create teradataml DataFrame objects.
        iris_input = DataFrame.from_table("iris_input")
        transformation_df = DataFrame.from_table("transformation_table")
        transformation_df = transformation_df.drop(['id'], axis=0)
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Run Fit() with all arguments which will validate numeric
        # transformations can be applied or not present in transformation_df DataFrame.
        # and pass the output to Transform().
        fit_df = Fit(data=iris_input,
                     object=transformation_df,
                     object_order_column='TargetColumn'
                     )
        # Run Transform() with persist as True in order to save the result.
        # Note that teradataml DataFrame representing the model is passed as
        # input to "object".
        transform_result = Transform(data=iris_input,
                                     data_partition_column='sepal_length',
                                     data_order_column='sepal_length',
                                     object=fit_df.result,
                                     object_order_column='TargetColumn',
                                     id_columns=['species', 'id'],
                                     persist=True
                                     )
        # Print the result DataFrame.
        print(transform_result.result)
        # Example 2: Transform the 'petal_length', 'sepal_length', 'petal_width',
        #            'sepal_width' according to transformation_df DataFrame.
        #            Note that model is passed as instance of FIT to "object".
        transform_result1 = Transform(data=iris_input,
                                      data_partition_column='sepal_length',
                                      data_order_column='sepal_length',
                                      object=fit_df,
                                      object_order_column='TargetColumn',
                                      id_columns=['species', 'id'],
                                      persist=True
                                      )
        # Print the result DataFrame.
        print(transform_result1.result)
    Unpivoting
    Unpivoting
    Functions
    Unpivoting(data=None, id_column=None, target_columns=None, alias_names=None, attribute_column='AttributeName', value_column='AttributeValue',
    accumulate=None, include_nulls=False, input_types=False, output_varchar=False, indexed_attribute=False, include_datatypes=False, **generic_arguments)
    DESCRIPTION:
        Function unpivots the data, that is, changes the data from
        dense format to sparse format.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the name of the column in "data" which contains the input data identifier.
            Types: str
        target_columns:
            Required Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) which contains the data for  
            unpivoting.
            Types: str OR list of Strings (str)
            Optional Argument.
            Specifies alternate names for the values in the 'attribute_column'.
            Types: str OR list of strs
        alias_names:
            Optional Argument.
            Specifies alternate names for the values in the 'attribute_column'. 
            column.
            Types: str OR list of strs
        attribute_column:
            Optional Argument.
            Specifies the name of the column in the output DataFrame, which holds the names of pivoted columns.
            Default Value: "AttributeName"
            Types: str
        value_column:
            Optional Argument.
            Specifies the name of the column in the output DataFrame, which holds the values of pivoted columns.
            Default Value: "AttributeValue"
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the output.
            By default, the function copies no input teradataml DataFrame columns to the output.
            Types: str OR list of Strings (str)
        include_nulls:
            Optional Argument.
            Specifies whether or not to include nulls in the transformation.
            Default Value: False
            Types: bool
        input_types:
            Optional Argument.
            Specifies whether attribute values should be organized into multiple columns based on data type groups.
            Note:
                * 'input_types' argument cannot be used when output_varchar is set to True.
            Default Value: False
            Types: bool
        output_varchar:
            Optional Argument.
            Specifies whether to output the 'value_column' in varchar format regardless of its data type.
            Note:
                * 'output_varchar' argument cannot be used when input_types is set to True.
            Default Value: False
            Types: bool
        indexed_attribute:
            Optional Argument.
            Specifies whether to output the column indexes instead of column names in AttributeName column. 
            When set to True, outputs the column indexes instead of column names.
            Default Value: False
            Types: bool
        include_datatypes:
            Optional Argument.
            Specifies whether to output the original datatype name. When set to True,
            outputs the original datatype name.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.        
    RETURNS:
        Instance of Unpivoting.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as UnpivotingObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the 
        #        function in user space.
        #     2. User can import the function, if it is available on 
        #        Vantage user is connected to.
        #     3. To check the list of analytic functions available on 
        #        Vantage user connected to, use 
        #        "display_analytic_functions()".
        # Load the example data.
        load_example_data('unpivot', 'unpivot_input')
        # Create teradataml DataFrame objects.
        upvt_inp = DataFrame('unpivot_input')
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function  Unpivoting.
        from teradataml import  Unpivoting
        # Example 1 : Unpivot the data.
        upvt1 = Unpivoting(data = upvt_inp, 
                           id_column = 'sn', 
                           target_columns = 'city',
                           accumulate = 'week',
                           include_nulls = True)
        # Print the result DataFrame.
        print( upvt1.result)
        # Example 2 : Unpivot the data with alternate names for the values in
        #             the AttributeName output column.
        upvt2= Unpivoting(data = upvt_inp, 
                          id_column = 'sn',
                          target_columns = 'city',
                          alias_names = 'city_us', 
                          attribute_column = "Attribute", 
                          value_column = "value",
                          accumulate = 'week', 
                          include_nulls = True)
        # Print the result DataFrame.
        print( upvt2.result)
        # Example 3 : Unpivot the data with multiple target columns and output
        #             data types.
        upvt3 = Unpivoting(data = upvt_inp, 
                           id_column = 'sn', 
                           target_columns = ['city','pressure'],
                           attribute_column = "Attribute", 
                           value_column = "value",
                           accumulate = 'week', 
                           include_nulls = True,
                           indexed_attribute = True, 
                           include_datatypes = True)
        # Print the result DataFrame.
        print( upvt3.result)
        # Example 4 : Unpivot the data with multiple target columns and output
        #             the input types.
        upvt4 = Unpivoting(data = upvt_inp, 
                           id_column = 'sn', 
                           target_columns = ['city','temp'],
                           accumulate = 'week', 
                           include_nulls = True, 
                           input_types = True)
        # Print the result DataFrame.
        print( upvt4.result)
    DATA EXPLORATION functions
    CategoricalSummary
    CategoricalSummary
    Functions
    CategoricalSummary(data=None, target_columns=None, **generic_arguments)
    DESCRIPTION:
        The CategoricalSummary() function displays the distinct values and their counts for
        each specified input DataFrame column.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" for which
            categorical summary needs to be determined.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
                Note:
                    These generic arguments are supported by teradataml if the underlying
                    SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of CategoricalSummary.
        Output teradataml DataFrames can be accessed using attribute
        references, such as CategoricalSummaryObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "titanic")
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example: Find the distinct values and counts for column 'sex'.
        obj = CategoricalSummary(data=titanic_data,
                                 target_columns="sex"
                                )
        # Print the result DataFrame.
        print(obj.result)
    ColumnSummary
    ColumnSummary
    Functions
    ColumnSummary(data=None, target_columns=None, **generic_arguments)
    DESCRIPTION:
        The ColumnSummary() function can be used to take a quick look at the columns,
        their datatypes, and summary of NULLs/non-NULLs for a given table.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the input teradataml DataFrame columns for which column
            summary needs to be determined.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
                Note:
                    These generic arguments are supported by teradataml if the underlying
                    SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of ColumnSummary.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ColumnSummaryObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "titanic")
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example: Find datatypes, NULL and non-NULL counts etc.
        obj = ColumnSummary(data=titanic_data,
                            target_columns=['age', 'pclass', 'embarked', 'cabin']
                           )
        # Print the result DataFrame.
        print(obj.result)
    GetRowsWithMissingValues
    GetRowsWithMissingValues
    Functions
    GetRowsWithMissingValues(data=None, target_columns=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        The GetRowsWithMissingValues() function displays the rows that have
        NULL values in the specified input data columns.
        Notes:
            * This function requires the UTF8 client character set for
              UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
              For information about PTCs, see Teradata Vantage™ -
              Analytics Database International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) in "data", in which NULL value should be checked.
            By default, all columns of the input teradataml DataFrame are considered as target.
            Types: str OR list of Strings (str)
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in volatile a table, otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
                Note:
                    These generic arguments are supported by teradataml if the underlying
                    SQLE function supports it, else an exception is raised.
    RETURNS:
        Instance of GetRowsWithMissingValues.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as GetRowsWithMissingValuesObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Get the rows that contain NULL values in columns
        #            'name', 'sex', 'age', 'passenger'.
        obj = GetRowsWithMissingValues(data=titanic_data,
                                       target_columns=['name', 'sex', 'age', 'passenger'])
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Get the rows that contain NULL values in columns
        #            'name', 'sex', 'age', 'passenger' by specifying
        #            input teradataml dataframe columns to copy to the
        #            output.
        obj1 = GetRowsWithMissingValues(data=titanic_data,
                                        target_columns=['name', 'sex', 'age', 'passenger'],
                                        accumulate=["survived", "pclass"])
        # Print the result DataFrame.
        print(obj1.result)
    Histogram
    Histogram
    Functions
    Histogram(data=None, object=None, target_columns=None, method_type=None, nbins=1, inclusion='LEFT', groupby_columns=None, **generic_arguments)
    DESCRIPTION:
        The Histogram() function calculates the frequency distribution of a data set
        using any of these methods:
            * Sturges
            * Scott
            * Variable-width
            * Equal-width
        Notes:
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required when "method_type" is 'VARIABLE-WIDTH', optional otherwise.
            Specifies the bin data.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" to perform computations on.
            Types: str OR list of Strings (str)
        method_type:
            Required Argument.
            Specifies the method type to be used for histogram computation.
            Permitted Values:
                * STURGES -
                    Algorithm for calculating bin width, w:
                         w = r/(1 + log₂ n)
                         where:
                             w = bin width
                             r = data value range
                             n = number of elements in data set
                         Sturges algorithm performs best if data is normally distributed
                         and n is at least 30.
                * SCOTT -
                    Algorithm for calculating bin width, w:
                         w = 3.49s/(n^1/3)
                         where:
                             w = bin width
                             s = standard deviation of data values
                             n = number of elements in data set
                             r = data value range
                             Number of bins: r/w
                         Scott algorithm performs best on normally distributed data.
                * VARIABLE-WIDTH -
                    Requires "object" argument, which specifies the minimum value and the
                    maximum value of the bin.
                    When one target column is specified, specify the minimum value
                    in column1 in "object" data , maximum value in column2 in "object"
                    data, and label of the bin in column3.
                    When more than one target column is specified, specify the
                    column name present in "target_column" attribute in column1
                    in "object" data, minimum value in column2 in "object" data,
                    maximum value in column3 in "object" data, and the label of
                    the bin in column4 in "object" data.
                    Note:
                        * The maximum number of bins cannot exceed 10000 per column.
                * EQUAL-WIDTH -
                    Algorithm for calculating bin width, w:
                    w = (max - min)/k
                    where:
                        min = minimum value of the bins
                        max = maximum value of the bins
                        k = number of intervals into which algorithm divides data set
                        Interval boundaries: min+w, min+2w, …, min+(k-1)w
                    "object" argument is optional.
                    When "object" data is omitted, Histogram() function internally computes
                    the min value and max value from the input data for the target columns.
                    When "object" data is specified, the user can specify in the following
                    manner:
                        * When one target column is specified, specify min value in column1
                          in "object" data and max value in column2 in "object" data.
                        * When more than one target column is specified, specify the
                          column name present in "target_column" attribute in column1
                          in "object" data, min value in column2 in "object" data,
                          and max value in column3 in "object" data.
            Types: str
        nbins:
            Required when "method_type" is 'VARIABLE-WIDTH' and 'EQUAL-WIDTH',
            optional otherwise.
            Specifies the number of bins.
            Notes:
                * When only one value is specified, it is applied to all the target columns.
                  Otherwise, the number of "nbins" values must be equal to the number of
                  target columns.
                * The maximum "nbins" value is 10000.
            Default Value: 1
            Types: int OR list of ints
        inclusion:
            Optional Argument.
            Specifies whether points on bin boundaries should be included in the
            bin on the left or the right.
            Note:
                * When only one value is specified, it is applied to all the target columns.
                  Otherwise, the number of "inclusion" values must be equal to the number
                  of target columns.
            Default Value: "LEFT"
            Permitted Values: LEFT, RIGHT
            Types: str OR list of Strings (str)
        groupby_columns:
            Optional Argument.
            Specifies the names of the input data columns that contain the group
            values for binning.
            Notes:
                * This argument must not have columns that are already specified
                  in "target_columns".
                * This argument does not support range.
                * The maximum number of unique columns in the "groupby_columns"
                  argument is 2042.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of Histogram.
        Output teradataml DataFrames can be accessed using attribute
        references, such as HistogramObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic", "min_max_titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Create teradataml DataFrame object for minimum and maximum value of bins
        # "Young age", "Middle Age" and, "Old Age".
        min_max_object = DataFrame.from_table("min_max_titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Get the frequency distribution of a data set using 'STURGES'
        #            method type for the values in column 'age'.
        obj1 = Histogram(data=titanic_data,
                         target_columns="age",
                         method_type="STURGES")
        # Print the result DataFrame.
        print(obj1.result)
        # Example 2: Get the frequency distribution of a data set using 'VARIABLE-WIDTH'
        #            method type for the values in column 'age' with 3 number of bins.
        obj2 = Histogram(data=titanic_data,
                         object=min_max_object,
                         target_columns="age",
                         method_type="VARIABLE-WIDTH",
                         nbins=3)
        # Print the result DataFrame.
        print(obj2.result)
        # Example 3: Get the frequency distribution of a data set with respect
        #            to 'sex' column using 'EQUAL-WIDTH' method type for the
        #            values in column 'age' and 'fare' with 3 and 2 number
        #            of bins respectively.
        obj3 = Histogram(data=titanic_data,
                         target_columns=["age", "fare"],
                         method_type="EQUAL-WIDTH",
                         nbins=[3,2],
                         groupby_columns=["sex"])
        # Print the result DataFrame.
        print(obj3.result)
    MovingAverage
    MovingAverage
    Functions
    MovingAverage(data=None, target_columns=None, alpha=0.1, start_rows=2, window_size=10, include_ﬁrst=False, mavgtype='C', **generic_arguments)
    DESCRIPTION:
        MovingAverage() function computes average values in a series, using the specified moving average type.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the name of the teradataml DataFrame that contains the
            columns.
            Types: teradataml DataFrame
        target_columns:
            Optional Argument.
            Specifies the input column names for which the moving average is to
            be computed. If you omit this argument, then the function copies
            every input column to the output teradataml DataFrame but does not
            compute moving average.
            Types: str OR list of Strings (str)
        alpha:
            Optional Argument.
            Specifies the damping factor, a value in the range [0, 1], which
            represents a percentage in the range [0, 100]. For example, if alpha
            is 0.2, then the damping factor is 20%. A higher alpha discounts
            older observations faster.
            Default Value: 0.1
            Types: float
        start_rows:
            Optional Argument.
            Specifies the number of rows at the beginning of the time series that
            the function "skips" before it begins the calculation of the
            exponential moving average. The function uses the arithmetic average
            of these rows as the initial value of the exponential moving average.
            The value n must be an integer.
            Default Value: 2
            Types: int
        window_size:
            Optional Argument.
            Specifies the number of previous values to include in the computation
            of the simple moving average.
            Default Value: 10
            Types: int
        include_first:
            Optional Argument.
            Specifies whether the first "start_rows" rows should be included in the
            output or not.
            Default Value: False
            Types: bool
        mavgtype:
            Optional Argument.
            Specifies the moving average type that needs to be used for computing
            moving averages of TargetColumns.
            Default Value: "C"
            Permitted Values: C, S, M, W, E, T
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of MovingAverage.
        Output teradataml DataFrames can be accessed using attribute
        references, such as MovingAverageObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("movavg", ["ibm_stock"])
        # Create teradataml DataFrame object.
        ibm_stock = DataFrame.from_table("ibm_stock")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Compute the average values in a series, using the 'C' moving average type.
        obj = MovingAverage(data=ibm_stock,
                            data_partition_column='stockprice',
                            data_order_column='stockprice',
                            include_first=False,
                            alpha=0.1,
                            start_rows=2,
                            window_size=10,
                            mavgtype='C')
        # Print the result DataFrame.
        print(obj.result)
    QQNorm
    QQNorm
    Functions
    QQNorm(data=None, target_columns=None, rank_columns=None, output_columns=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        Function determines if values in input data columns follow normal distribution or not. 
        It returns the quantiles of the column values and corresponding theoretical quantile
        values from a normal distribution. If the column values are normally distributed, then
        the quantiles of column values and normal quantile values appear in a straight line
        when plotted on a 2D graph.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" to generate standard normal quantiles.
            Types: str OR list of Strings (str)
        rank_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" containing rank values for "target_columns".
            Types: str OR list of Strings (str)
        output_columns:
            Optional Argument.
            Specifies the name(s) of the output column(s) to be generated that contain the theoretical
            quantiles of the target column(s). If not specified, name(s) will be generated as
            "<column name in target_columns>_theoretical_quantiles".
            Types: str OR list of strs
        accumulate:
            Optional Argument.
            Specifies the names of input teradataml DataFrame columns to copy to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of QQNorm.
        Output teradataml DataFrames can be accessed using attribute
        references, such as QQNormObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the sample rank_df DataFrame.
        load_example_data("teradataml", ["rank_table"])
        # Create teradataml DataFrame object.
        rank_df = DataFrame.from_table("rank_table")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Get theoretical quantile values for 'age' and 'fare'.
        obj = QQNorm(data=rank_df,
                     target_columns=["age", "fare"],
                     rank_columns=["rank_age", "rank_fare"])
        # Print the result DataFrame.
        print(obj.result)
    UnivariateStatistics
    UnivariateStatistics
    Functions
    UnivariateStatistics(newdata=None, target_columns=None, partition_columns=None, stats='ALL', centiles=[1, 5, 10, 25, 50, 75, 90, 95, 99], trim_percentile=20,
    **generic_arguments)
    DESCRIPTION:
        UnivariateStatistics() function displays descriptive statistics for each specified numeric input DataFrame column.
    PARAMETERS:
        newdata:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" for which univariate
            statistics need to be displayed.
            Types: str OR list of Strings (str)
        partition_columns:
            Optional Argument.
            Specifies the names of the input partition columns.
            Types: str OR list of Strings (str)
        stats:
            Optional Argument.
            Specifies the statistics to calculate.
            Permitted Values:
                * SUM
                * COUNT or CNT
                * MAXIMUM or MAX
                * MINIMUM or MIN
                * MEAN
                * UNCORRECTED SUM OF SQUARES or USS
                * NULL COUNT or NLC
                * POSITIVE VALUES COUNT or PVC
                * NEGATIVE VALUES COUNT or NVC
                * ZERO VALUES COUNT or ZVC
                * TOP5 or TOP
                * BOTTOM5 or BTM
                * RANGE or RNG
                * GEOMETRIC MEAN or GM
                * HARMONIC MEAN or HM
                * VARIANCE or VAR
                * STANDARD DEVIATION or STD
                * STANDARD ERROR or SE
                * SKEWNESS or SKW
                * KURTOSIS or KUR
                * COEFFICIENT OF VARIATION or CV
                * CORRECTED SUM OF SQUARES or CSS
                * MODE
                * MEDIAN or MED
                * UNIQUE ENTITY COUNT or UEC
                * INTERQUARTILE RANGE or IQR
                * TRIMMED MEAN or TM
                * PERCENTILES or PRC
                * ALL
            Default Value: 'ALL'
            Types: str OR list of Strings (str)
        centiles:
            Optional Argument.
            Specifies the percentile to calculate.
            The function ignores Centiles unless Stats specifies PERCENTILES, PRC, or ALL.
            Default Value: [1, 5, 10, 25, 50, 75, 90, 95, 99]
            Types: int or list of int
        trim_percentile:
            Optional Argument.
            Specifies the trimmed lower percentile.
            Default Value: 20
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of UnivariateStatistics.
        Output teradataml DataFrames can be accessed using attribute
        references, such as UnivariateStatisticsObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Display descriptive statistics of input DataFrame
        #            column "fare" by partitioning "sex" and "age".
        obj = UnivariateStatistics(newdata=titanic_data,
                                   target_columns='fare',
                                   partition_columns=['sex', 'age'],
                                   stats='ALL',
                                   centiles=[1, 5, 10, 25, 50, 75, 90, 95, 99],
                                   trim_percentile=20)
        # Print the result DataFrame.
        print(obj.result)
    WhichMax
    WhichMax
    Functions
    WhichMax(data=None, target_column=None, **generic_arguments)
    DESCRIPTION:
        WhichMax() function displays all rows that have the maximum value in specified input DataFrame column.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_column:
            Required Argument.
            Specifies the name of the column in "data" to check the maximum value.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of WhichMax.
        Output teradataml DataFrames can be accessed using attribute
        references, such as WhichMaxObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Display all rows that have the maximum value in 'age' column.
        obj = WhichMax(data=titanic_data,
                      target_column='age')
        # Print the result DataFrame.
        print(obj.result)
    WhichMin
    WhichMin
    Functions
    WhichMin(data=None, target_column=None, **generic_arguments)
    DESCRIPTION:
        WhichMin() function displays all rows that have the minimum value in specified input DataFrame column.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_column:
            Required Argument.
            Specifies the name of the column in "data" to check the minimum value.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of WhichMin.
        Output teradataml DataFrames can be accessed using attribute
        references, such as WhichMinObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Display all rows that have the minimum value in 'age' column.
        obj = WhichMin(data=titanic_data,
                       target_column='age')
        # Print the result DataFrame.
        print(obj.result)
    TEXT ANALYTIC functions
    NaiveBayesTextClassifierPredict
    NaiveBayesTextClassiﬁerPredict
    Functions
    NaiveBayesTextClassiﬁerPredict(object=None, newdata=None, input_token_column=None, doc_id_columns=None, model_type='MULTINOMIAL', top_k=None,
    model_token_column=None, model_category_column=None, model_prob_column=None, output_prob=False, responses=None, accumulate=None,
    **generic_arguments)
    DESCRIPTION:
        The NaiveBayesTextClassifierPredict() function uses the model generated by the
        NaiveBayesTextClassifierTrainer() function to predict the outcomes for a test set
        of data.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame which contains the model
            data generated by the NaiveBayesTextClassifierTrainer() function or
            instance of NaiveBayesTextClassifierTrainer.
            Types: teradataml DataFrame or NaiveBayesTextClassifierTrainer
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input test
            data.
            Types: teradataml DataFrame
        input_token_column:
            Required Argument.
            Specifies the name of the newdata column that contains the tokens.
            Types: str
        doc_id_columns:
            Required Argument.
            Specifies the names of the newdata columns that contain the
            document identifier.
            Types: str OR list of Strings (str)
        model_type:
            Optional Argument.
            Specifies the model type of the text classifier.
            Permitted Values: 'MULTINOMIAL', 'BERNOULLI'
            Default Value: 'MULTINOMIAL'
            Types: str
        top_k:
            Optional Argument.
            Specifies the number of most likely prediction categories to output
            with their log-likelihood values (for example, the top 10 most likely
            prediction categories). The default is all prediction categories.
            Types: int
        model_token_column:
            Optional Argument.
            Specifies the name of the object column that contains the
            tokens. The default value is the first column of object.
            Types: str
        model_category_column:
            Optional Argument.
            Specifies the name of the object column that contains the
            prediction categories. The default value is the second column of
            object.
            Types: str
        model_prob_column:
            Optional Argument.
            Specifies the name of the object column that contains the token
            counts. The default value is the third column of object.
            Types: str
        output_prob:
            Optional Argument.
            Specifies whether to output probabilities.
            Default Value: False
            Types: bool
        responses:
            Optional Argument.
            Specifies a list of Responses to output.
            Types: str OR list of strs
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output. By default, the function copies no input teradataml
            DataFrame columns to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of NaiveBayesTextClassifierPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as
        NaiveBayesTextClassifierPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
        result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("NaiveBayesTextClassifierPredict",["complaints_tokens_test","token_table"])
        # Create teradataml DataFrame objects.
        token_table = DataFrame("token_table")
        complaints_tokens_test = DataFrame("complaints_tokens_test")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Create a model which is output of NaiveBayesTextClassifierTrainer.
        nbt_out = NaiveBayesTextClassifierTrainer(data = token_table,
                                                  token_column = 'token',
                                                  doc_id_column = 'doc_id',
                                                  doc_category_column = 'category',
                                                  model_type = "Bernoulli",
                                                  data_partition_column = 'category')
        # Example: Run NaiveBayesTextClassifierPredict() on model generated by
        #          NaiveBayesTextClassifierTrainer() where model_type is "Bernoulli".
        nbt_predict_out = NaiveBayesTextClassifierPredict(object = nbt_out,
                                                          newdata = complaints_tokens_test,
                                                          input_token_column = 'token',
                                                          doc_id_columns = 'doc_id',
                                                          model_type = "Bernoulli",
                                                          model_token_column = 'token',
                                                          model_category_column = 'category',
                                                          model_prob_column = 'prob',
                                                          newdata_partition_column = 'doc_id')
        # Print the result DataFrame.
        print(nbt_predict_out.result)
    NaiveBayesTextClassifierTrainer
    NaiveBayesTextClassiﬁerTrainer
    Functions
    NaiveBayesTextClassiﬁerTrainer(data=None, doc_category_column=None, token_column=None, doc_id_column=None, model_type='MULTINOMIAL',
    **generic_arguments)
    DESCRIPTION:
        The NaiveBayesTextClassifierTrainer() function calculates the conditional probabilities for 
        token-category pairs, the prior probabilities, and the missing token probabilities for 
        all categories. The trainer function trains the model with the probability values, and 
        the predict function uses the values to classify documents into categories.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        doc_category_column:
            Required Argument.
            Specifies the name of the input data column that contains the 
            document category.
            Types: str
        token_column:
            Required Argument.
            Specifies the name of the input data column that contains the tokens.
            Types: str
        doc_id_column:
            Optional Argument.
            Specifies the name of the input data column that contains the 
            document identifier.
            Types: str
        model_type:
            Optional Argument.
            Specifies the model type of the text classifier. 
            Default Value: "MULTINOMIAL"
            Permitted Values: MULTINOMIAL, BERNOULLI
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of NaiveBayesTextClassifierTrainer.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as 
        NaiveBayesTextClassifierTrainerObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. model_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("textparser", ["complaints", "stop_words"])
        # Create teradataml DataFrame objects.
        complaints = DataFrame.from_table("complaints")
        stop_words = DataFrame.from_table("stop_words")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Tokenize the "text_column" and accumulate result by "doc_id" and "category".
        complaints_tokenized = TextParser(data=complaints,
                                          text_column="text_data",
                                          object=stop_words,
                                          remove_stopwords=True,
                                          accumulate=["doc_id", "category"])
        # Example 1 : Calculate the conditional probabilities for token-category pairs.
        NaiveBayesTextClassifierTrainer_out = NaiveBayesTextClassifierTrainer(data=complaints_tokenized.result,
                                                                              token_column="token",
                                                                              doc_category_column="category")
        # Print the result DataFrames.
        print(NaiveBayesTextClassifierTrainer_out.result)
        print(NaiveBayesTextClassifierTrainer_out.model_data)
    NERExtractor
    NERExtractor
    Functions
    NERExtractor(data=None, user_deﬁned_data=None, rules_data=None, text_column=None, input_language='EN', show_context=0, accumulate=None,
    **generic_arguments)
    DESCRIPTION:
        NERExtractor() performs Named Entity Recognition (NER) on input text 
        according to user-defined dictionary words or regular expression (regex) patterns.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        user_defined_data:
            Required Argument.
            Specifies the teradataml DataFrame which contains user defined words and the corresponding entity label.
            Types: teradataml DataFrame
        rules_data:
            Required Argument.
            Specifies the teradataml DataFrame which contains user-
    defined regex patterns and the corresponding entity label.
            Types: teradataml DataFrame
        text_column:
            Required Argument.
            Specifies the name of the teradataml DataFrame column that will be used for NER search.
            Types: str
        input_language:
            Optional Argument.
            Specifies the language of input text.
            Default Value: "EN"
            Types: str
        show_context:
            Optional Argument.
            Specifies the number of words before and after the matched entity. If leading or trailing
            words are less than "show_context", then ellipsis (...) are added. Must be a positive value
            less than 10.
            Default Value: 0
            Types: int
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the output.
            table to output.
            Types: str or list of str
        **generic_arguments:
            Optional Argument.
            Specifies the generic keyword arguments SQLE functions accept. Below are the generic
            keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results are
                    garbage collected at the end of the session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table; otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQLE Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of NERExtractor.
        Output teradataml DataFrames can be accessed using attribute references, such as TDNERExtractorObj.
    <attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in the example from teradataml.
        #     3. Function will raise an error if not supported on the Vantage user is connected to.
        # Load the example data.
        load_example_data("tdnerextractor", ["ner_input_eng", "ner_dict", "ner_rule"])
        # Create teradataml DataFrame objects.
        df = DataFrame.from_table("ner_input_eng")
        user_defined_words = DataFrame.from_table("ner_dict")
        rules = DataFrame.from_table("ner_rule")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function NERExtractor.
        from teradataml import NERExtractor
        # Example 1: Perform Named Entity Recognition (NER) using Rules and Dict with Accumulate.
        NER_out = NERExtractor(data=df,
                               user_defined_data=user_defined_words,
                               rules_data=rules,
                               text_column=["txt"],
                               input_language="en",
                               show_context=3,
                               accumulate=["id"])
        # Print the result DataFrame.
        print(NER_out.result)
    NGramSplitter
    NGramSplitter
    Functions
    NGramSplitter(data=None, text_column=None, delimiter=' ', grams=None, overlapping=True, to_lower_case=True, punctuation='`~#^&*()-', reset='.,?!',
    total_gram_count=False, total_count_column='totalcnt', accumulate=None, n_gram_column='ngram', num_grams_column='n', frequency_column='frequency',
    **generic_arguments)
    DESCRIPTION:
        The NGramSplitter function tokenizes (splits) an input stream of text and
        outputs n multigrams (called n-grams) based on the specified
        delimiter and reset parameters. NGramSplitter provides more flexibility than
        standard tokenization when performing text analysis. Many two-word
        phrases carry important meaning (for example, "machine learning")
        that unigrams (single-word tokens) do not capture. This, combined
        with additional analytical techniques, can be useful for performing
        sentiment analysis, topic identification and document classification.
        Note: This function is only available when teradataml is connected
              to Vantage 1.1 or later versions.
    PARAMETERS:
        data:
            Required Argument.
            Specifies input teradataml DataFrame, where each row contains a document
            to be tokenized. The input teradataml DataFrame can have additional rows,
            some or all of which the function returns in the output table.
            Types: teradataml DataFrame
        text_column:
            Required Argument.
            Specifies the name of the column that contains the input text. The column
            must have a SQL string data type.
            Types: str
        delimiter:
            Optional Argument.
            Specifies a character or string or a regular expression that separates words in the input text. The
            default value is the set of all whitespace characters which includes
            the characters for space, tab, newline, carriage return and some
            others.
            Default Value: "[\s]+"
            Types: str
        grams:
            Required Argument.
            Specifies the length, in words, of each n-gram (that is, the value of n).
            A value_range has the syntax integer1 - integer2, where integer1 <= integer2.
            The values of n, integer1, and integer2 must be positive.
            Types: str
        overlapping:
            Optional Argument.
            Specifies a Boolean value that specifies whether the function allows
            overlapping n-grams. When this value is "true" (the default), each
            word in each sentence starts an n-gram, if enough words follow it (in
            the same sentence) to form a whole n-gram of the specified size. For
            information on sentences, see the description of the reset argument.
            Default Value: True
            Types: bool
        to_lower_case:
            Optional Argument.
            Specifies a Boolean value that specifies whether the function converts all
            letters in the input text to lowercase.
            Default Value: True
            Types: bool
        punctuation:
            Optional Argument.
            Specifies a string or a regular expression that specifies the punctuation characters for the function
            to remove before evaluating the input text.
            Default Value: "`~#^&*()-"
            Types: str
        reset:
            Optional Argument.
            Specifies a string or a regular expression that specifies the character or string that ends a sentence.
            At the end of a sentence, the function discards any partial n-grams and searches
            for the next n-gram at the beginning of the next sentence. An n-gram
            cannot span two sentences.
            Default Value: ".,?!"
            Types: str
        total_gram_count:
            Optional Argument.
            Specifies whether the function returns the total
            number of ngrams in the document (that is, in the row). If you
            specify "true", then the name of the returned column is specified by
            the total_count_column argument.
            Note: The total number of n-grams is not necessarily the number
            of unique ngrams.
            Default Value: False
            Types: bool
        total_count_column:
            Optional Argument.
            Specifies the name of the column to return if the value of the total_gram_count
            argument is True.
            Default Value: "totalcnt"
            Types: str
        accumulate:
            Optional Argument.
            Specifies the names of the columns to return for each n-gram. These columns
            cannot have the same names as those specified by the arguments ngram,
            num_grams_column, and total_count_column. By default, the function
            returns all input columns for each n-gram.
            Types: str OR list of Strings (str)
        n_gram_column:
            Optional Argument.
            Specifies the name of the column that is to contain the generated n-grams.
            Default Value: "ngram"
            Types: str
        num_grams_column:
            Optional Argument.
            Specifies the name of the column that is to contain the length of n-gram (in
            words).
            Default Value: "n"
            Types: str
        frequency_column:
            Optional Argument.
            Specifies the name of the column that is to contain the count of each unique
            n-gram (that is, the number of times that each unique n-gram appears
            in the document).
            Default Value: "frequency"
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of NGramSplitter.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ngramsplitterObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("ngrams", ["paragraphs_input"])
        # Create teradataml DataFrame object.
        paragraphs_input = DataFrame.from_table("paragraphs_input")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Creating teradataml dataframe by calculating the
        #            similarity between two strings.
        obj = NGramSplitter(data=paragraphs_input,
                            text_column='paratext',
                            n_gram_column='ngram',
                            num_grams_column='n',
                            frequency_column='frequency',
                            total_count_column='totalcnt',
                            grams='4-6',
                            overlapping=True,
                            to_lower_case=True,
                            delimiter=' ',
                            punctuation='`~#^&*()-',
                            reset='.,?!',
                            total_gram_count=False)
        # Print the result DataFrame.
        print(obj.result)
    SentimentExtractor
    SentimentExtractor
    Functions
    SentimentExtractor(data=None, cust_dict=None, add_dict=None, text_column=None, accumulate=None, analysis_type='DOCUMENT', priority='NONE',
    output_type='ALL', **generic_arguments)
    DESCRIPTION:
        The SentimentExtractor() function uses a dictionary model
        to extract the sentiment (positive, negative, or neutral)
        of each input document or sentence.
        The dictionary model consists of WordNet, a lexical database
        of the English language, and these negation words (no, not,
        neither, never, and similar negation words).
        The function handles negated sentiments as follows:
            * -1 if the sentiment is negated (for example, "I am not happy")
            * -1 if the sentiment and a negation word are separated by one
              word (for example, "I am not very happy")
            * +1 if the sentiment and a negation word are separated by two
              or more words (for example, "I am not saying I am happy")
        Notes:
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
            * For information about PTCs, see Teradata Vantage™ - Analytics Database
              International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
            * Only the English language is supported.
            * The max length supported for sentiment word in the dictionary data
              is 128 characters.
            * The Max length of the sentiment_words output column is 32000 characters.
              If the sentiment_words output column value exceeds this limit, then a
              triple dot(...) displays at the end of the string.
            * The Max length of the content output column is 32000 characters;
              that is, the supported maximum length of a sentence is 32000.
            * User can have up to 10 words in a sentiment phrase.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        cust_dict:
            Optional Argument.
            Specifies the input teradataml DataFrame containing custom dictionary data,
            to use non-default custom dictionary data.
            Types: teradataml DataFrame
        add_dict:
            Optional Argument.
            Specifies the input teradataml DataFrame containing additional entries,
            to add additional entries to either "cust_dict"
            or default dictionary.
            Types: teradataml DataFrame
        text_column:
            Required Argument.
            Specifies the "data" column that contains the text data
            for sentiment analysis.
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s)
            to copy to the output. By default, the function copies no
            input teradataml DataFrame columns to the output.
            Types: str OR list of Strings (str)
        analysis_type:
            Optional Argument.
            Specifies the level of analysis, whether to analyze each document
            or each sentence in a document.
            Permitted Values:
                * DOCUMENT - Analyzes each document.
                * SENTENCE - Analyzes each sentence in a document.
            Default Value: "DOCUMENT"
            Types: str
        priority:
            Optional Argument.
            Specifies the highest priority when returning results.
            Permitted Values: 
                * NONE - Provide all results the same priority.
                * NEGATIVE_RECALL - Provide the highest priority to negative
                                    results, including those with lower-confidence
                                    sentiment classifications (maximizes number of
                                    negative results returned).
                * NEGATIVE_PRECISION - Provide the highest priority to negative
                                       results with high-confidence sentiment classifications.
                * POSITIVE_RECALL - Provide the highest priority to positive results,
                                    including those with lower-confidence sentiment
                                    classifications (maximizes number of positive results returned).
                * POSITIVE_PRECISION - Provide the highest priority to positive results with high
                                       confidence sentiment classifications.
            Default Value: "NONE"
            Types: str
        output_type:
            Optional Argument.
            Specifies the kind of results to return.
            Permitted Values:
                * ALL - Returns all results.
                * POS - Returns only results with positive sentiments.
                * NEG - Returns only results with negative sentiments.
                * NEU - Returns only results with neutral sentiments.
            Default Value: "ALL"
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of SentimentExtractor.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SentimentExtractorObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. output_dictionary_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("sentimentextractor", ["sentiment_extract_input",
                                                  "sentiment_word_input",
                                                  "additional_table"])
        # Create teradataml DataFrame objects.
        sentiment_extract_input = DataFrame.from_table("sentiment_extract_input")
        sentiment_word_input = DataFrame.from_table("sentiment_word_input")
        additional_table = DataFrame.from_table("additional_table")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Extracting the sentiment (positive, negative, or neutral)
        #             of each input document or sentence.
        sentimentextractor_out = SentimentExtractor(text_column="review",
                                                    data=sentiment_extract_input)
        # Print the result DataFrame.
        print(sentimentextractor_out.result)
        print(sentimentextractor_out.output_dictionary_data)
        # Example 2 : Extracting the sentiment (positive, negative, or neutral)
        #             of each input document by specifying custom dictionary data
        #             and adding additional entries to custom dictionary data.
        sentimentextractor_out_1 = SentimentExtractor(text_column="review",
                                                      accumulate=['id', 'product'],
                                                      analysis_type="DOCUMENT",
                                                      priority="NONE",
                                                      output_type="ALL",
                                                      data=sentiment_extract_input,
                                                      cust_dict=sentiment_word_input,
                                                      add_dict=additional_table)
        # Print the result DataFrame.
        print(sentimentextractor_out_1.result)
        print(sentimentextractor_out_1.output_dictionary_data)
    TextMorph
    TextMorph
    Functions
    TextMorph(data=None, word_column=None, pos=None, single_output=False, postag_column=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        TextMorph() function generate morphs of given words in the input dataset.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        word_column:
            Required Argument.
            Specifies the name of the input column that contains words for which morphs are to be generated.
            Types: str
        pos:
            Optional Argument.
            Specifies the part of speech (POS) to output.
            Permitted Values: "NOUN", "VERB", "ADV", "ADJ"
            Types: str or list of str
        single_output:
            Optional Argument.
            Specifies whether to output only one morph for each word. If set to `False`, 
            the function outputs all morphs for each word.
            Default Value: False
            Types: bool
        postag_column:
            Optional Argument.
            Specifies the name of the  column in data that contains the part-of-speech (POS) 
            tags of the words, output by the function TD_POSTagger.
            Types: str
        accumulate:
            Optional Argument.
            Specifies the names of the input columns to copy to the output table.
            Types: str or list of str
        **generic_arguments:
            Optional Argument.
            Specifies the generic keyword arguments SQLE functions accept. Below are the generic
            keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results are
                    garbage collected at the end of the session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table; otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQLE Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of TextMorph.
        Output teradataml DataFrames can be accessed using attribute references, such as TDTextMorphObj.
    <attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in the example from teradataml.
        #     3. Function will raise an error if not supported on the Vantage user is connected to.
        # Load the example data.
        load_example_data("textmorph", ["words_input","pos_input"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("words_input")
        data2 = DataFrame.from_table("pos_input")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function TextMorph.
        from teradataml import TextMorph
        # Example 1: Generate morphs for words in the input dataset.
        TextMorph_out = TextMorph(data=data1,
                                  word_column="data2",
                                  pos=["noun", "verb"],
                                  single_output=True,
                                  accumulate=["id"])
        # Print the result DataFrame.
        print(TextMorph_out.result)
        Example 2 : Generate morphs for words in the input dataset with POS tags.
        TextMorph_pos = TextMorph(data=data2,
                                  word_column="word",
                                  postag_column="pos_tag",
                                  accumulate=["id","pos_tag"])
        # Print the result DataFrame.
        print(TextMorph_pos.result)
    TextParser
    TextParser
    Functions
    TextParser(data=None, object=None, text_column=None, enforce_token_limit=False, convert_to_lowercase=True, stem_tokens=False, remove_stopwords=False,
    accumulate=None, delimiter=' \t\n\x0c\r', delimiter_regex=None, punctuation='!#$%&()*+,-./:;?@\\^_`{|}~', token_col_name=None, doc_id_column=None,
    list_positions=False, token_frequency=False, output_by_word=True, **generic_arguments)
        DESCRIPTION:
            The TextParser() function can parse text and perform the following operations:
                * Tokenize the text in the specified column
                * Remove the punctuations from the text and convert the text to lowercase
                * Remove stop words from the text and convert the text to their root forms
                * Create a row for each word in the output dataframe
                * Perform stemming; that is, the function identifies the common root form of a word
                  by removing or replacing word suffixes
                Notes:
                    * The stems resulting from stemming may not be actual words. For example, the stem
                      for 'communicate' is 'commun' and the stem for 'early' is 'earli'
                      (trailing 'y' is replaced by 'i').
                    * This function requires the UTF8 client character set.
                    * This function does not support Pass Through Characters (PTCs).
                    * For information about PTCs, see Teradata Vantage™ - Analytics Database International
                       Character Set Support.
                    * This function does not support KanjiSJIS or Graphic data types.
        PARAMETERS:
            data:
                Required Argument.
                Specifies the input teradataml DataFrame.
                Types: teradataml DataFrame
            object:
                Optional Argument.
                Specifies the teradataml DataFrame containing stop words.
                Types: teradataml DataFrame
            text_column:
                Required Argument.
                Specifies the name of the input data column whose contents are to be tokenized.
                Types: str
            enforce_token_limit:
                Optional Argument.
                Specifies whether to throw an informative error when finding token larger than
                64K/32K or silently discard those larger tokens.
                Default Value: False
                Types: bool
            convert_to_lowercase:
                Optional Argument.
                Specifies whether to convert the text in "text_column" to lowercase.
                Default Value: True
                Types: bool
            stem_tokens:
                Optional Argument.
                Specifies whether to convert the text in "text_column" to their root forms.
                Default Value: False
                Types: bool
            remove_stopwords:
                Optional Argument.
                Specifies whether to remove stop words from the text in "text_column" before
                parsing.
                Default Value: False
                Types: bool
            accumulate:
                Optional Argument.
                Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
                output. By default, the function copies no input teradataml
                DataFrame columns to the output.
                Types: str OR list of Strings (str)
            delimiter:
                Optional Argument.
                Specifies the word delimiter to apply to the text in the specified column in the 
                "text_column" element.
                Default Value: " \t\n\f\r"
                Types: str
            delimiter_regex:
                Optional Argument.
                Specifies a Perl Compatible regular expression that represents the word delimiter.
                Types: str
            punctuation:
                Optional Argument.
                Specifies the punctuation characters to replace with a space in the input text.
                Default Value: "!#$%&()*+,-./:;?@\^_`{|}~"
                Types: str
            token_col_name:
                Optional Argument.
                Specifies the name for the output column that contains the individual words from
                the text of the specified column in the "text_column" element.
                Types: str
            doc_id_column:
                Optional Argument.
                Specifies the name of the column that uniquely identifies a row in the input table.
                Types: str
            list_positions:
                Optional Argument.
                Specifies whether to output the positions of a word in list form.
                Default Value: False
                Types: bool
            token_frequency:
                Optional Argument.
                Specifies whether to output the frequency for each token.
                Default Value: False
                Types: bool
            output_by_word:
                Optional Argument.
                Specifies whether to output each token in a separate row or all tokens in one.
                Default Value: True
                Types: bool
            **generic_arguments:
                Specifies the generic keyword arguments SQLE functions accept. Below 
                are the generic keyword arguments:
                    persist:
                        Optional Argument.
                        Specifies whether to persist the results of the 
                        function in a table or not. When set to True, 
                        results are persisted in a table; otherwise, 
                        results are garbage collected at the end of the 
                        session.
                        Default Value: False
                        Types: bool
                    volatile:
                        Optional Argument.
                        Specifies whether to put the results of the 
                        function in a volatile table or not. When set to 
                        True, results are stored in a volatile table, 
                        otherwise not.
                        Default Value: False
                        Types: bool
                Function allows the user to partition, hash, order or local 
                order the input data. These generic arguments are available 
                for each argument that accepts teradataml DataFrame as 
                input and can be accessed as:    
                    * "<input_data_arg_name>_partition_column" accepts str or 
                        list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list 
                        of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list 
                        of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
                Note:
                    These generic arguments are supported by teradataml if 
                    the underlying SQL Engine function supports, else an 
                    exception is raised.
        RETURNS:
            Instance of TextParser.
            Output teradataml DataFrames can be accessed using attribute
            references, such as TextParserObj.<attribute_name>.
            Output teradataml DataFrame attribute name is:
                result
        RAISES:
            TeradataMlException, TypeError, ValueError
        EXAMPLES:
            # Notes:
            #     1. Get the connection to Vantage to execute the function.
            #     2. One must import the required functions mentioned in
            #        the example from teradataml.
            #     3. Function will raise error if not supported on the Vantage
            #        user is connected to.
            # Load the example data.
            load_example_data("textparser", ["complaints", "stop_words"])
            # Create teradataml DataFrame objects.
            complaints = DataFrame.from_table("complaints")
            stop_words = DataFrame.from_table("stop_words")
            # Check the list of available analytic functions.
            display_analytic_functions()
            # Example 1 : Remove all the stop words from "text_data" column
            #             and accumulate it by "doc_id" column.
            TextParser_out = TextParser(data=complaints,
                                        text_column="text_data",
                                        object=stop_words,
                                        remove_stopwords=True,
                                        accumulate="doc_id")
            # Print the result DataFrame.
            print(TextParser_out.result)
            # Example 2 : Convert words in "text_data" column into their root forms.
            TextParser_out = TextParser(data=complaints,
                                        text_column="text_data",
                                        convert_to_lowercase=True,
                                        stem_tokens=True)
            # Print the result DataFrame.
            print(TextParser_out.result)
            # Example 3 : Tokenize  words in "text_data" column using delimiter regex,
            #             convert tokens to lowercase and output token positions in a list format
            TextParser_out = TextParser(data=complaints,
                                        text_column="text_data",
                                        doc_id_column="doc_id",
                                        delimeter_regex="[ \t\f\r\n]+]",
                                        list_positions=True,
                                        convert_to_lowercase=True,
                                        output_by_word=False)
            # Print the result DataFrame.
            print(TextParser_out.result)
    WordEmbeddings
    WordEmbeddings
    Functions
    WordEmbeddings(data=None, model=None, id_column=None, model_text_column=None, model_vector_columns=None, primary_column=None,
    secondary_column=None, operation='token-embedding', accumulate=None, convert_to_lowercase=True, remove_stopwords=False, stem_tokens=False,
    **generic_arguments)
    DESCRIPTION:
        The WordEmbeddings() function produces vectors for each piece of text and
        finds the similarity between the texts.
        Word embedding is the representation of a word in multi-dimensional
        space such that words with similar meanings have similar embedding.
        Each word is mapped to a vector of real numbers that represent the word.
        The function contains training and prediction using models. The models
        contain each possible token and its corresponding vectors. The closer
        the distance between the vectors the more the similarity. Function operations
        are token-embedding, doc-embedding, token2token-similarity, and doc2doc-similarity.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the teradataml DataFrame which contains the model
            vectors for all the possible tokens.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the input data column name that has the
            unique identifier for each row in the input.
            Types: str
        model_text_column:
            Required Argument.
            Specifies the column that contains the token in the "model".
            Types: str
        model_vector_columns:
            Required Argument.
            Specifies range of columns in the "model" data that contains
            real value vector.
            Types: str OR list of Strings (str)
        primary_column:
            Required Argument.
            Specifies name of the input data column that contains the text.
            Types: str
        secondary_column:
            Optional Argument.
            Specifies name of the input data column that contains the text.
            Note:
                * This field is applicable for the 'token2token-similarity' and
                  'doc2doc-similarity' operations only.
            Types: str
        operation:
            Optional Argument.
            Specifies the operation to be performed on the data.
            Permitted Values: token-embedding, doc-embedding,
                              token2token-similarity, doc2doc-similarity
            Default Value: 'token-embedding'
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to
            copy to the output.
            Note:
                * This is not applicable with the 'token-embedding' operation.
            Types: str OR list of Strings (str)
        convert_to_lowercase:
            Optional Argument.
            Specifies whether to convert input data to lower case or not.
            When set to True, input data is converted to lower case.
            Otherwise, input data is not converted to lower case.
            Default Value: True
            Types: bool
        remove_stopwords:
            Optional Argument.
            Specifies whether to remove stop words from the input data.
            Note:
                * This is not applicable with the 'token2token-similarity'
                  operation.
            Default Value: False
            Types: bool
        stem_tokens:
            Optional Argument.
            Specifies whether to convert word to its root word in the input data.
            For example, convert 'going' to 'go'.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of WordEmbeddings.
        Output teradataml DataFrames can be accessed using attribute
        references, such as WordEmbeddingsObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["word_embed_model","word_embed_input_table1","word_embed_input_table2"])
        # Create teradataml DataFrame objects.
        model_input = DataFrame.from_table("word_embed_model")
        data_input1 = DataFrame.from_table("word_embed_input_table1")
        data_input2 = DataFrame.from_table("word_embed_input_table2")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Generate vectors for each words present in column 'doc1'
        #             using the word embedding model and 'token-embedding' operation.
        #             Each word is assigned with vectors and closer the distance
        #             between two vectors greater the similarity between two words.
        WordEmbeddings_out1 = WordEmbeddings(data = data_input1,
                                             model = model_input,
                                             id_column = "doc_id",
                                             model_text_column = "token",
                                             model_vector_columns = ["v1", "v2", "v3", "v4"],
                                             primary_column = "doc1",
                                             operation = "token-embedding"
                                             )
        # Print the result DataFrame.
        print(WordEmbeddings_out1.result)
        # Example 2 : Generate vectors for each row present in column 'doc1'
        #             using the word embedding model and 'doc-embedding' operation.
        #             Each row with para or sentence is assigned with vectors and
        #             closer the distance between two vectors greater the similarity
        #             between two para or sentence.
        WordEmbeddings_out2 = WordEmbeddings(data = data_input1,
                                             model = model_input,
                                             id_column = "doc_id",
                                             model_text_column = "token",
                                             model_vector_columns = ["v1", "v2", "v3", "v4"],
                                             primary_column = "doc1",
                                             operation = "doc-embedding",
                                             accumulate = "doc1"
                                             )
        # Print the result DataFrame.
        print(WordEmbeddings_out2.result)
        # Example 3 : Find the similarity between two columns 'token1' and 'token2' in 'data_input2',
        #             using the word embedding model and 'token2token-similarity' operation.
        WordEmbeddings_out3 = WordEmbeddings(data = data_input2,
                                             model = model_input,
                                             id_column = "token_id",
                                             model_text_column = "token",
                                             model_vector_columns = ["v1", "v2", "v3", "v4"],
                                             primary_column = "token1",
                                             secondary_column = "token2",
                                             operation = "token2token-similarity",
                                             accumulate = ["token1", "token2"]
                                             )
        # Print the result DataFrame.
        print(WordEmbeddings_out3.result)
        # Example 4 : Find the similarity between two columns 'doc1' and 'doc2' in 'data_input1',
        #             using the word embedding model and 'doc2doc-similarity' operation.
        WordEmbeddings_out4 = WordEmbeddings(data = data_input1,
                                             model = model_input,
                                             id_column = "doc_id",
                                             model_text_column = "token",
                                             model_vector_columns = ["v1", "v2", "v3", "v4"],
                                             primary_column = "doc1",
                                             secondary_column = "doc2",
                                             operation = "doc2doc-similarity",
                                             accumulate = ["doc1", "doc2"]
                                             )
        # Print the result DataFrame.
        print(WordEmbeddings_out4.result)
    DATA CLEANING functions
    ConvertTo
    ConvertTo
    Functions
    ConvertTo(data=None, target_columns=None, target_datatype=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        The ConvertTo() function converts the specified input DataFrame columns to
        specified data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the input teradataml DataFrame columns which needs to be
            casted/converted to a different data type.
            Types: str OR list of Strings (str)
        target_datatype:
            Required Argument.
            Specifies target data type(s) into which "target_columns" need to be
            converted. If one value is provided, it applies to all "target_columns".
            If more than one value is specified, each "target_datatype" value applies to
            corresponding "target_columns" value (in the order specified by the user).
            Types: str OR list of Strings (str)
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to
            copy to the output. By default, the function copies all input teradataml
            DataFrame columns to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of ConvertTo.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ConvertToObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Convert datatype of 'fare' to integer.
        ConvertTo_out = ConvertTo(data = titanic,
                                  target_columns = ["fare"],
                                  target_datatype = ["integer"],
                                  accumulate=['passenger', 'name', 'ticket']
                                  )
        # Print the result DataFrame.
        print(ConvertTo_out.result)
    .GetFutileColumns
    GetFutileColumns
    Functions
    GetFutileColumns(data=None, object=None, category_summary_column='ColumnName', threshold_value=0.95, **generic_arguments)
    DESCRIPTION:
        GetFutileColumns() function returns the futile column names if either
        of the conditions is met:
            * If all the values in the columns are unique
            * If all the values in the columns are the same
            * If the count of distinct values in the columns divided by the 
              count of the total number of rows in the input data is greater than 
              or equal to the threshold value
            Notes:
                * This function requires the UTF8 client character set for UNICODE data.
                * This function does not support Pass Through Characters (PTCs).
                  For information about PTCs, see Teradata Vantage™ - Advanced SQL Engine
                  International Character Set Support.
                * This function does not support KanjiSJIS or Graphic data types.
                * This function works only for categorical data.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the categorical summary
            generated by CategoricalSummary() or the instance of CategoricalSummary.
            Types: teradataml DataFrame or CategoricalSummary
        category_summary_column:
            Optional Argument.
            Specifies the column from categorical summary DataFrame which provides names of 
            the columns in "data".
            Default Value: 'ColumnName'
            Types: str
        threshold_value:
            Optional Argument.
            Specifies the threshold value for the columns in "data".
            Default Value: 0.95
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table.
                    When set to True, results are persisted in a table;
                    otherwise, results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table.
                    When set to True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of GetFutileColumns.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as GetFutileColumnsObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame objects.
        titanic = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Get futiles columns from target columns 'cabin', 'sex', and 'ticket'.
        CategoricalSummary_out = CategoricalSummary(data=titanic,
                                                    target_columns=["cabin", "sex", "ticket"])
        GetFutileColumns_out = GetFutileColumns(data=titanic,
                                                object=CategoricalSummary_out,
                                                category_summary_column="ColumnName",
                                                threshold_value=0.7
                                                )
        # Print the result DataFrame.
        print(GetFutileColumns_out.result)
    GetRowsWithoutMissingValues
    GetRowsWithoutMissingValues
    Functions
    GetRowsWithoutMissingValues(data=None, target_columns=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        The GetRowsWithoutMissingValues() function displays the rows that
        have non-NULL values in the specified input data columns.
        Notes:
            * This function requires the UTF8 client character set for
              UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
              For information about PTCs, see Teradata Vantage™ - Advanced SQL
              Engine International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) in "data", in which NULL value should be checked.
            By default, all columns of the input teradataml DataFrame are considered as target.
            Types: str OR list of Strings (str)
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
                Note:
                    These generic arguments are supported by teradataml if the underlying
                    SQLE function supports it, else an exception is raised.
    RETURNS:
        Instance of GetRowsWithoutMissingValues.
        Output teradataml DataFrames can be accessed using attribute
        references, such as GetRowsWithoutMissingValuesObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Get the rows that contain non-NULL values in columns.
        obj = GetRowsWithoutMissingValues(data=titanic_data)
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Get the rows that contain non-NULL values in columns.
        #            by specifying input teradataml dataframe columns to
        #            copy to the output.
        obj1 = GetRowsWithoutMissingValues(data=titanic_data,
                                          target_columns=['name', 'sex', 'age', 'passenger'],
                                          accumulate=["survived", "pclass"])
        # Print the result DataFrame.
        print(obj1.result)
    OutlierFilterFit
    OutlierFilterFit
    Functions
    OutlierFilterFit(data=None, target_columns=None, group_columns=None, lower_percentile=0.05, upper_percentile=0.95, iqr_multiplier=1.5,
    outlier_method='PERCENTILE', replacement_value='DELETE', remove_tail='BOTH', percentile_method='PERCENTILEDISC', **generic_arguments)
    DESCRIPTION:
        The OutlierFilterFit() function calculates the lower_percentile,
        upper_percentile, count of rows and median for all the "target_columns"
        provided by the user. These metrics for each column helps the
        function OutlierTransform() detect outliers in the input table. It also
        stores parameters from arguments into a FIT table used during
        transformation.
        Notes:
            * This function requires the UTF8 client character set for
              UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
            * For information about PTCs, see Teradata Vantage™ - Analytics
              Database International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
            * This function does not support "data_partition_column" and "data_order_column" 
              if the corresponding Vantage version is greater than or equal to 17.20.03.20.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" for which
            to compute the metrics.
            Types: str OR list of Strings (str)
        group_columns:
            Optional Argument.
            Specifies the input data column for which stats calculation needs to
            be grouped together.
            Types: str
        lower_percentile:
            Optional Argument.
            Specifies lower range of percentile to be used to detect if value is
            outlier or not.
            Default Value: 0.05
            Types: int
        upper_percentile:
            Optional Argument.
            Specifies upper range of percentile to be used to detect if value is
            outlier or not.
            Default Value: 0.95
            Types: int
        iqr_multiplier:
            Optional Argument.
            Specifies the multiplier of interquartile range for "Tukey" filtering.
            Default Value: 1.5
            Types: int
        outlier_method:
            Optional Argument.
            Specifies the method for filtering the outliers.
            Permitted Values:
                * PERCENTILE - [min_value, max_value].
                * TUKEY - [Q1 - k*(Q3-Q1), Q1 + k*(Q3-Q1)]
                          where:
                            Q1 = 25th quartile of data
                            Q3 = 75th quartile of data
                            k = interquantile range multiplier (see "iqr_multiplier")
                * CARLING - Q2 ± c*(Q3-Q1)
                            where:
                                Q2 = median of data
                                Q1 = 25th quartile of data
                                Q3 = 75th quartile of data
                                c = (17.63*r - 23.64) / (7.74*r - 3.71)
                                r = count of rows in group_columns if you specify "group_columns",
                                    otherwise count of rows in "data"
            Default Value: "PERCENTILE"
            Types: str
        replacement_value:
            Optional Argument.
            Specifies the method to handle outliers.
            Permitted Values:
                * DELETE - Do not copy row to output DataFrame.
                * NULL - Copy row to output DataFrame, replacing each outlier with NULL.
                * MEDIAN - Copy row to output DataFrame, replacing each outlier with median
                           value for its group.
                * REPLACEMET VALUE - Copy row to output DataFrame, replacing each outlier with
                                      a replacement value. Replacement value must be numeric.
            Default Value: "DELETE"
            Types: str, int, float
        remove_tail:
            Optional Argument.
            Specifies the tail of the distribution to remove.
            Permitted Values:
                * LOWER - The lower tail.
                * UPPER - The upper tail.
                * BOTH - Both tails.
            Default Value: "BOTH"
            Types: str
        percentile_method:
            Optional Argument.
            Specifies the teradata percentile methods to be used for calculating
            the upper and lower percentiles of the "target_columns".
            Permitted Values:
                * PERCENTILECONT - Considering continuous distribution.
                * PERCENTILEDISC - Considering discrete distibution.
            Default Value: "PERCENTILEDISC"
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of OutlierFilterFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as OutlierFilterFitObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame objects.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Generating fit object to find outlier values in column "fare".
        OutlierFilterFit_out = OutlierFilterFit(data = titanic_data,
                                                target_columns = "fare")
        # Print the result DataFrame.
        print(OutlierFilterFit_out.result)
        print(OutlierFilterFit_out.output_data)
    OutlierFilterTransform
    OutlierFilterTransform
    Functions
    OutlierFilterTransform(data=None, object=None, **generic_arguments)
    DESCRIPTION:
        OutlierFilterTransform() function filters the outliers from the input teradataml DataFrame.
        OutlierFilterTransform() uses the result DataFrame from OutlierFilterFit() function to get
        statistics like median, count of rows, lower percentile and upper percentile for every column
        specified in target columns argument and filters the outliers in the input data.
        Notes:
            * Partitioning of input data and model is allowed using 'data_partition_column' and
              'object_partition_column' only if 'group_columns' are passed while creating model
              using OutlierFilterFit() function.
            * Neither 'data_partition_column' nor 'object_partition_column' can be used independently.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the outlier metrics generated by
            OutlierFilterFit() function or an instance of OutlierFilterFit.
            Types: teradataml DataFrame or OutlierFilterFit
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of OutlierFilterTransform.
        Output teradataml DataFrames can be accessed using attribute
        references, such as OutlierFilterTransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Finding outliers in column "fare" using "PERCENTILE" method.
        #            Generate fit object for column "fare".
        fit_obj = OutlierFilterFit(data=titanic_data,
                                   target_columns="fare",
                                   lower_percentile=0.1,
                                   upper_percentile=0.9,
                                   outlier_method="PERCENTILE",
                                   replacement_value="MEDIAN",
                                   percentile_method="PERCENTILECONT")
        # Print the result DataFrame.
        print(fit_obj.result)
        # Find the outliers by transforming fit object result DataFrame.
        # Note that teradataml DataFrame representing the model is passed as
        # input to "object".
        obj = OutlierFilterTransform(data=titanic_data,
                                     object=fit_obj.result)
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Find outliers in column "fare" using "PERCENTILE"
        #            method. Note that model is passed as instance of
        #            OutlierFilterFit to "object".
        obj1 = OutlierFilterTransform(data=titanic_data,
                                      object=fit_obj)
        # Print the result DataFrame.
        print(obj1.result)
    Pack
    Pack
    Functions
    Pack(data=None, input_columns=None, output_column=None, delimiter=',', include_column_name=True, col_cast=False, accumulate=None,
    **generic_arguments)
    DESCRIPTION:
        The Pack() function packs data from multiple input DataFrame columns into a single column.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        input_columns:
            Optional Argument.
            Specifies the names of the input columns to pack into a single output
            column. These names become the column names of the virtual columns.
            By default, all input teradataml DataFrame columns are packed into a
            single output column. If you specify this argument, but do not
            specify all input teradataml DataFrame columns, the function copies
            the unspecified input tablecolumns to the output table.
            Types: str OR list of Strings (str)
        output_column:
            Required Argument.
            Specifies the name to give to the packed output column.
            Types: str
        delimiter:
            Optional Argument.
            Specifies the delimiter (a string) that separates the virtual columns
            in the packed data.
            Default Value: ","
            Types: str
        include_column_name:
            Optional Argument.
            Specifies whether to label each virtual column value with its column
            name (making the virtual column "input_column:value").
            Default Value: True
            Types: bool
        col_cast:
            Optional Argument.
            Specifies whether to get better elapsed times with use cases involving numeric
            columns to be packed.
            Default Value: False
            Types: bool
        accumulate:
            Optional Argument.
            Specifies the input teradataml DataFrame columns to copy to the
            output table. By default, the function copies no input teradataml
            DataFrame columns to the output table.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of Pack.
        Output teradataml DataFrames can be accessed using attribute
        references, such as PackObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("pack", ["ville_temperature"])
        # Create teradataml DataFrame object.
        ville_temperature = DataFrame.from_table("ville_temperature")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Packs data from multiple input DataFrame columns
        #            into a single column.
        obj = Pack(data=ville_temperature,
                   input_columns=['city','state','period','temp_f'],
                   output_column='packed_data',
                   delimiter=',',
                   accumulate='city',
                   include_column_name=True)
        # Print the result DataFrame.
        print(obj.result)
    StringSimilarity
    StringSimilarity
    Functions
    StringSimilarity(data=None, comparison_columns=None, case_sensitive=None, accumulate=None, **generic_arguments)
    DESCRIPTION:
        StringSimilarity() function calculates the similarity between two
        strings, using either the Jaro, Jaro-Winkler, N-Gram, or Levenshtein
        distance.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        comparison_columns:
            Required Argument.
            Specifies pairs of input teradataml DataFrame columns that contain
            strings to be compared (column1 and column2), how to compare them
            (comparison_type), and (optionally) a constant and the name of the
            output column for their similarity (output_column). The similarity is
            a value in the range [0, 1].
            For comparison_type, use one of these values:
                * "jaro": Jaro distance.
                * "jaro_winkler": Jaro-Winkler distance (1 for an exact match, 0 otherwise).
                  If you specify this comparison type, you can specify the value of
                  factor p with constant. 0 ≤ p ≤ 0.25.
                  Default: p = 0.1
                * "n_gram": N-gram similarity.
                  If you specify this comparison type, you can specify the value of N with
                  constant.
                  Default: N = 2
                * "LD": Levenshtein distance
                  The number of edits needed to transform one string into the other,
                  where edits include insertions, deletions, or substitutions of
                  individual characters.
                * "LDWS": Levenshtein distance without substitution.
                  Number of edits needed to transform one string into the other using only
                  insertions or deletions of individual characters.
                * "OSA": Optimal string alignment distance.
                  Number of edits needed to transform one string into the other.
                  Edits are insertions, deletions, substitutions, or transpositions of
                  characters. A substring can be edited only once.
                * "DL": Damerau-Levenshtein distance.
                  Like OSA, except that a substring can be edited any number of times.
                * "hamming": Hamming distance.
                  Number of positions where corresponding characters differ (that is,
                  minimum number of substitutions needed to transform one string into the
                  other) for strings of equal length, otherwise -1 for strings of unequal
                  length.
                * "LCS": Longest common substring.
                  Length of longest substring common to both strings.
                * "jaccard": Jaccard index-based comparison.
                * "cosine": Cosine similarity.
                * "soundexcode": Only for English strings. -1 if either string has a
                  non-English character, otherwise, 1 if their soundex codes are the same
                  and 0 otherwise.
            You can specify a different comparison_type for every pair of columns.
            The default output_column is "sim_i", where i is the sequence number of the
            column pair.
            Types: str OR list of Strings (str)
        case_sensitive:
            Optional Argument.
            Specifies whether string comparison is case-sensitive. The default
            value is False. You can specify either one value for all pairs or
            one value for each pair. If you specify one value for each pair, then
            the ith value applies to the ith pair.
            Types: bool OR list of bools
        accumulate:
            Optional Argument.
            Specifies the names of input teradataml DataFrame columns to be
            copied to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine   
                function supports, else an exception is raised.
    RETURNS:
        Instance of StringSimilarity.
        Output teradataml DataFrames can be accessed using attribute
        references, such as StringSimilarityObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("stringsimilarity", ["strsimilarity_input"])
        # Create teradataml DataFrame object.
        strsimilarity_input = DataFrame.from_table("strsimilarity_input")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Creating teradataml dataframe by calculating the
        #            similarity between two strings.
        obj = StringSimilarity(data = strsimilarity_input,
                               comparison_columns=['jaro (src_text1, tar_text) AS jaro1_sim',
                                                   'LD (src_text1, tar_text) AS ld1_sim',
                                                   'n_gram (src_text1, tar_text, 2) AS ngram1_sim',
                                                   'jaro_winkler (src_text1, tar_text, 0.1) AS jw1_sim'],
                               case_sensitive = True,
                               accumulate = ["id","src_text1","tar_text"])
        # Print the result DataFrame.
        print(obj.result)
    Unpack
    Unpack
    Functions
    Unpack(data=None, input_column=None, output_columns=None, output_datatypes=None, delimiter=',', column_length=None, regex='(.*)', regex_set=1,
    exception=False, accumulate=None, **generic_arguments)
    DESCRIPTION:
        The Unpack() function unpacks data from a single packed column into
        multiple columns. The packed column is composed of multiple virtual columns,
        which become the output columns. To determine the virtual  columns, the function
        must have either the delimiter that separates them in the packed column or their
        lengths.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the teradataml DataFrame containing the input attributes.
            Types: teradataml DataFrame
        input_column:
            Required Argument.
            Specifies the name of the input column that contains the packed data.
            Types: str
        output_columns:
            Required Argument.
            Specifies the names to give to the output columns, in the order in
            which the corresponding virtual columns appear in "input_column". If you
            specify fewer output column names than there are in virtual input
            columns, the function ignores the extra virtual input columns. That
            is, if the packed data contains x+y virtual columns and the
            "output_columns" argument specifies x output column names, the function
            assigns the names to the first x virtual columns and ignores the
            remaining y virtual columns.
            Types: str OR list of Strings (str)
        output_datatypes:
            Required Argument.
            Specifies the datatypes of the unpacked output columns.Supported
            output.datatypes are VARCHAR, int, float, TIME, DATE, and
            TIMESTAMP. If "output_datatypes" specifies only one value and
            "output_columns" specifies multiple columns, then the specified value
            applies to every output_column. If "output_datatypes" specifies
            multiple values, then it must specify a value for each output_column.
            The nth datatype corresponds to the nth output_column.The function
            can output only 16 VARCHAR columns.
            Types: str OR list of Strings (str)
        delimiter:
            Optional Argument.
            Specifies the delimiter (a string) that separates the virtual
            columns in the packed data. If the virtual columns are separated by a
            delimiter, then specify the delimiter with this argument; otherwise, specify
            the "column_length" argument. Do not specify both - this argument and
            the "column_length" argument.
            Default Value: ","
            Types: str
        column_length:
            Optional Argument.
            Specifies the lengths of the virtual columns; therefore, to use this
            argument, you must know the length of each virtual column. If
            "column_length" specifies only one value and "output_columns" specifies
            multiple columns, then the specified value applies to every
            output_column. If "column_length" specifies multiple values, then it
            must specify a value for each output_column. The nth datatype
            corresponds to the nth output_column. However, the last column_name
            can be an asterisk (*), which represents a single virtual column that
            contains the remaining data. For example, if the first three virtual
            columns have the lengths 2, 1, and 3, and all remaining data belongs
            to the fourth virtual column, you can specify "column_length" as
            ("2", "1", "3", *). If you specify this argument, you must omit the
            delimiter argument.
            Types: str OR list of Strings (str)
        regex:
            Optional Argument.
            Specifies a regular expression that describes a row of packed data,
            enabling the function to find the data values. A row of packed data
            contains one data value for each virtual column, but the row might
            also contain other information (such as the virtual column name). In
            the "regex", each data value is enclosed in parentheses.
            For example, suppose that the packed data has two virtual columns,
            age and sex, and that one row of packed data is: age:34,sex:male The
            "regex" that describes the row is ".*:(.*)". The ".*:"
            matches the virtual column names, age and sex, and the "(.*)" matches
            the values, 34 and male. The default "regex" is "(.*)"
            which matches the whole string (between delimiters, if any). When
            applied to the preceding sample row, the default "regex" causes the function
            to return "age:34" and "sex:male" as data values. To represent multiple data
            groups in "regex", use multiple pairs of parentheses. By default, the last
            data group in "regex" represents the data value (other data groups are
            assumed to be virtual column names or unwanted data). If a different
            data group represents the data value, specify its group number with
            the "regex_set" argument.
            Default Value: "(.*)"
            Types: str
        regex_set:
            Optional Argument.
            Specifies the ordinal number of the data group in "regex" that represents the
            data value in a virtual column. By default, the last data group in "regex"
            represents the data value.
            For example, suppose that "regex" is: "([a-zA-Z]*):(.*)". If
            group number is "1", then "([a-zA-Z]*)" represents the data value. If
            group number is "2", then "(.*)" represents the data value.
            Default Value: 1
            Types: int
        exception:
            Optional Argument.
            Specifies whether the function ignores rows that contain invalid data;
            By default the function to fails if it encounters a row with invalid data.
            Default Value: False
            Types: bool
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the
            output. By default, the function copies no input teradataml
            DataFrame columns to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQLE function supports it, else an exception is raised.
    RETURNS:
        Instance of Unpack.
        Output teradataml DataFrames can be accessed using attribute
        references, such as UnpackObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("Unpack",["ville_tempdata","ville_tempdata1"])
        # Create teradataml DataFrame objects.
        ville_tempdata1 = DataFrame.from_table("ville_tempdata1")
        ville_tempdata = DataFrame.from_table("ville_tempdata")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Delimiter separates Virtual Columns.
        #           The input table, ville_tempdata, is a collection of temperature readings
        #           for two cities, Nashville and Knoxville, in the state of Tennessee.
        #           In the column of packed data, the delimiter comma (,) separates the virtual
        #           columns.
        unpack_out1 = Unpack(data=ville_tempdata,
                             input_column='packed_temp_data',
                             output_columns=['city','state','temp_f'],
                             output_datatypes=['varchar','varchar','real'],
                             delimiter=',',
                             regex='(.*)',
                             regex_set=1,
                             exception=True)
        # Print the results DataFrame.
        print(unpack_out1.result)
        # Example 2: No Delimiter separates Virtual Columns.
        #            The input, ville_tempdata1, contains same data as the previous example,
        #            except that no delimiter separates the virtual columns in the packed data.
        #            To enable the function to determine the virtual columns, the function call
        #            specifies the column lengths.
        unpack_out2 = Unpack(data=ville_tempdata1,
                             input_column='packed_temp_data',
                             output_columns=['city','state','temp_f'],
                             output_datatypes=['varchar','varchar','real'],
                             column_length=['9','9','4'],
                             regex='(.*)',
                             regex_set=1,
                             exception=True)
        # Print the results DataFrame.
        print(unpack_out2.result)
    HYPOTHESIS TESTING functions
    ANOVA
    ANOVA
    Functions
    ANOVA(data=None, group_columns=None, alpha=0.05, group_name_column=None, group_value_column=None, group_names=None, num_groups=None,
    **generic_arguments)
    DESCRIPTION:
        The ANOVA() function performs one-way ANOVA (Analysis of Variance) on
        a data set with two or more groups. ANOVA is a statistical test that
        analyzes the difference between the means of more than two groups.
        The null hypothesis (H0) of ANOVA is that there is no difference among
        group means. However, if any one of the group means is significantly
        different from the overall mean, then the null hypothesis is rejected.
        You can use one-way Anova when you have data on an independent variable
        with at least three levels and a dependent variable.
        For example, assume that your independent variable is insect spray type,
        and you have data on spray type A, B, C, D, E, and F. You can use one-way
        ANOVA to determine whether there is any difference in the dependent variable,
        insect count based on the spray type used.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        group_columns:
            Optional Argument.
            Specifies the names of the columns in "data" to use in the computation.
            Note:
                Users must specify at least two columns in "group_columns" list.
            Types: list of Strings (str)
        alpha:
            Optional Argument.
            Specifies the probability of rejecting the null hypothesis when the null
            hypothesis is true.
            Default Value: 0.05
            Types: float
        group_name_column:
            Optional Argument.
            Specifies the column name in "data" containing the names of the groups  
            included in the computation.
            Note:
                * This argument is used when data contains group names in a column
                    and group values in another column.
                * This argument must be used in conjunction with "group_value_column".
            Types: str
        group_value_column:
            Optional Argument.
             Specifies the column name in "data" containing the values for each group member.
            Note:
                * This argument is used when data contains group values in a column
                    and group names in another column.
                * This argument must be used in conjunction with "group_name_column".
            Types: str
        group_names:
            Optional Argument.
            Specifies the names of the groups included in the computation.
            Note:
                * This argument is used when data contains group values in a column
                  and group names in another column.
            Types: list of Strings (str)
        num_groups:
            Optional Argument.
             Specifies the number of different groups in the "data" included 
            in the computation.
            Note:
                * This argument is used when data contains group values in a column
                  and group names in another column.
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of ANOVA.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ANOVAObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["insect_sprays"])
        load_example_data("ztest", 'insect2Cols')
        # Create teradataml DataFrame objects.
        insect_sprays = DataFrame.from_table("insect_sprays")
        insect_gp = DataFrame.from_table("insect2Cols")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Perform one-way anova analysis on a data set with
        #             two or more groups.
        ANOVA_out_1 = ANOVA(data = insect_sprays,
                            alpha = 0.05
                            )
        # Print the result DataFrame.
        print(ANOVA_out_1.result)
        # Example 2 : Perform one-way anova analysis on a data set with more
        #             than two groups and group_columns argument specified.
        ANOVA_out_2 = ANOVA(data = insect_sprays,
                            group_columns=insect_sprays.columns[2:5],
                            alpha = 0.05
                            )
        # Print the result DataFrame.
        print(ANOVA_out_2.result)
        # Example 3 : Perform one-way anova analysis on a data set with more
        #             than two groups and group_name_column, group_value_column,
        #             group_names.
        ANOVA_out_3 = ANOVA(data = insect_gp,
                            group_name_column='groupName',
                            group_value_column='groupValue',
                            group_names=['groupA', 'groupB', 'groupC'])
        # Print the result DataFrame.
        print(ANOVA_out_3.result)
        # Example 4 : Perform one-way anova analysis on a data set with more
        #             than two groups and num_groups.
        ANOVA_out_4 = ANOVA(data = insect_gp,
                            group_name_column='groupName',
                            group_value_column='groupValue',
                            num_groups=6)
        # Print the result DataFrame.
        print(ANOVA_out_4.result)
    ChiSq
    ChiSq
    Functions
    ChiSq(data=None, alpha=0.05, **generic_arguments)
    DESCRIPTION:
        The ChiSq() function performs Pearson's chi-squared (χ2) test for independence,
        which determines if there is a statistically significant difference between
        the expected and observed frequencies in one or more categories of a
        contingency table (also called a cross tabulation).
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        alpha:
            Optional Argument.
            Specifies the probability below which the null hypothesis is rejected.
            "alpha" must be a numeric value in the range [0, 1].
            Default Value: 0.05
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
                Note:
                    These generic arguments are supported by teradataml if the underlying
                    SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of ChiSq.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ChiSq.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. output_data
            2. output
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "chi_sq")
        # Create teradataml DataFrame object.
        chi_sq_data = DataFrame.from_table("chi_sq")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example: Run ChiSq() with all arguments.
        obj = ChiSq(data=chi_sq_data,
                    alpha=0.5)
        # Print the output DataFrame and expected values DataFrame.
        print(obj.output)
        print(obj.output_data)
    FTest
    FTest
    Functions
    FTest(data=None, alpha=0.05, ﬁrst_sample_variance=None, ﬁrst_sample_column=None, df1=None, second_sample_variance=None,
    second_sample_column=None, df2=2, alternate_hypothesis='two-tailed', sample_name_column=None, sample_value_column=None, ﬁrst_sample_name=None,
    second_sample_name=None, **generic_arguments)
    DESCRIPTION:
        The FTest() function performs an F-test, for which the test statistic follows an
        F-distribution under the Null hypothesis.
        Function compares the variances of two independent populations.
        If the variances are significantly different, the FTest() function rejects the
        Null hypothesis, indicating that the variances may not come from the same
        underlying population.
        Use the function to compare statistical models that have been fitted to a
        data set, to identify the model that best fits the population from which the data
        were sampled.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        alpha:
            Optional Argument.
            Specifies the probability of rejecting the null 
            hypothesis when the null hypothesis is true.
            Note:
                * "alpha" must be a numeric value in the range [0, 1].
            Default Value: 0.05
            Types: float
        first_sample_column:
            Optional Argument.
            Specifies the first sample column in F-Test.
            Note:
                * This argument must be specified with "first_sample_variance" and "df1"
                  or allowed combination is "first_sample_column" with 
                  "second_sample_variance" and "df2".
                * This argument cannot be used in conjunction with "sample_name_column"
                  and "sample_value_column".
            Types: str
        first_sample_variance:
            Optional Argument.
            Specifies the first sample variance.
            Note:
                * This argument must be specified with "first_sample_column" and "df1"
                  or other allowed combination is "second_sample_column" with 
                  "first_sample_variance" and "df1".
            Types: float
        df1:
            Optional Argument.
            Specifies the degrees of freedom of the first sample.
            Note:
                * This argument must be specified with "first_sample_column" and 
                  "first_sample_variance".
            Types: integer
        second_sample_column:
            Optional Argument.
            Specifies the second sample column in F-Test.
            Note:
                * This argument must be specified with "second_sample_variance" and "df2"
                  or allowed combination is "second_sample_column" with "first_sample_variance" 
                  and "df1".
                * This argument cannot be used in conjunction with "sample_name_column"
                  and "sample_value_column".
            Types: str
        second_sample_variance:
            Optional Argument.
            Specifies the second sample variance.
            Note:
                * This argument must be specified with "second_sample_column" and "df2"
                  or allowed combination is "first_sample_column" with 
                  "second_sample_variance" and df2.
            Types: float
        df2:
            Optional Argument.
            Specifies the degree of freedom of the second sample.
            Note:
                * This argument must be specified with "second_sample_column" and 
                  "second_sample_variance".
            Types: integer
        alternate_hypothesis:
            Optional Argument.
            Specifies the alternate hypothesis.
            Permitted Values:
                * lower-tailed - Alternate hypothesis (H 1): μ < μ0.
                * upper-tailed - Alternate hypothesis (H 1): μ > μ0.
                * two-tailed - Rejection region is on two sides of sampling distribution
                               of test statistic.
                               Two-tailed test considers both lower and upper tails of
                               distribution of test statistic.
                               Alternate hypothesis (H 1): μ ≠ μ0
            Default Value: two-tailed
            Types: str
        sample_name_column:
            Optional Argument.
            Specifies the column name in "data" containing the names of the samples
            included in the F-Test.
            Types: str
        sample_value_column:
            Optional Argument.
            Specifies the column name in "data" containing the values for each sample member.
            Types: str
        first_sample_name:
            Optional Argument.
            Specifies the name of the first sample included in the F-Test.
            Types: str
        second_sample_name:
            Optional Argument.
            Specifies the name of the second sample included in the F-Test.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of FTest.
        Output teradataml DataFrames can be accessed using attribute
        references, such as FTestObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "titanic")
        load_example_data("ztest", 'insect2Cols')
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        insect_gp = DataFrame.from_table("insect2Cols")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Run FTest() with first_sample_variance, second_sample_variance,
        #            df1 and df2.
        obj = FTest(data=titanic_data, alpha=0.5,
                    second_sample_column="parch",
                    alternate_hypothesis="two-tailed",
                    first_sample_variance=5,
                    second_sample_variance=8,
                    df1=1, df2=2
                    )
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Run FTest() with only required arguments.
        obj = FTest(data=titanic_data,
                    second_sample_column="parch",
                    second_sample_variance=8,
                    df2=2
                    )
        # Print the result DataFrame.
        print(obj.result)
        # Example 3: Run FTest() with sample_name_column, sample_value_column,
        #            first_sample_name and second_sample_name.
        obj = FTest(data=insect_gp,
                    sample_value_column='groupValue',
                    sample_name_column='groupName',
                    first_sample_name='groupE',
                    second_sample_name='groupC')
        # Print the result DataFrame.
        print(obj.result)
        # Example 4: Run FTest() with sample_name_column, sample_value_column,
        #            first_sample_name and second_sample_name.
        obj = FTest(data=insect_gp,
                    sample_value_column='groupValue',
                    sample_name_column='groupName',
                    first_sample_name='groupE',
                    second_sample_variance=100.0,
                    df2=25)
        # Print the result DataFrame.
        print(obj.result)
        # Example 5: Run FTest() with sample_name_column, sample_value_column,
        #            second_sample_name and first_sample_variance.
        obj = FTest(data=insect_gp,
                    sample_value_column='groupValue',
                    sample_name_column='groupName',
                    second_sample_name='groupC',
                    first_sample_variance=85.0,
                    df1=19)
        # Print the result DataFrame.
        print(obj.result)
    ZTest
    ZTest
    Functions
    ZTest(data=None, alpha=0.05, ﬁrst_sample_column=None, second_sample_column=None, alternate_hypothesis='two-tailed', ﬁrst_sample_variance=None,
    second_sample_variance=None, mean_under_h0=0, sample_name_column=None, sample_value_column=None, ﬁrst_sample_name=None,
    second_sample_name=None, **generic_arguments)
    DESCRIPTION:
        ZTest() function tests the equality of two means under the assumption that the
        population variances are known. For large samples, sample variances
        approximate population variances, so it uses sample variances
        instead of population variances in the test statistic.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        alpha:
            Optional Argument.
            Specifies the value of alpha in hypothesis test function.
            Default Value: 0.05
            Types: float
        first_sample_column:
            Optional Argument.
            Specifies the first sample column in Z-Test.
            Types: str
        second_sample_column:
            Optional Argument.
            Specifies the second sample column in Z-Test.
            Types: str
        alternate_hypothesis:
            Optional Argument.
            Specifies the alternate hypothesis.
            Permitted Values:
                * lower-tailed - Alternate hypothesis (H 1): μ < μ0.
                * upper-tailed - Alternate hypothesis (H 1): μ > μ0.
                * two-tailed - Rejection region is on two sides of sampling distribution
                               of test statistic.
                               Two-tailed test considers both lower and upper tails of
                               distribution of test statistic.
                               Alternate hypothesis (H 1): μ ≠ μ0
            Default Value: "two-tailed"
            Types: str
        first_sample_variance:
            Optional Argument.
            Specifies the first sample variance.
            Types: float
        second_sample_variance:
            Optional Argument.
            Specifies the second sample variance.
            Types: float
        mean_under_h0:
            Optional Argument.
            Specifies the mean under the null hypothesis.
            Default Value: 0
            Types: float
        sample_name_column:
            Optional Argument.
            Specifies the column in the "data" containing the names of the samples 
            included in the Z-Test.
            Note:
                * This argument is used when data contains sample names in a column
                  and sample values in another column.
            Types: str
        sample_value_column:
            Optional Argument.
            Specifies the column in the "data" containing the values for each sample member.
            Note:
                * This argument is used when data contains sample names in a column
                  and sample values in another column.
            Types: str
        first_sample_name:
            Optional Argument.
            Specifies the name of the first sample included in the Z-Test.
            Note:
                * This argument is used when data contains sample names in a column
                  and sample values in another column.
            Types: str
        second_sample_name:
            Optional Argument.
            Specifies the name of the second sample included in the Z-Test.
            Note:
                * This argument is used when data contains sample names in a column
                  and sample values in another column.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports, else an exception is raised.
    RETURNS:
        Instance of ZTest.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ZTestObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        load_example_data('ztest', 'boston2cols')
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        bostonCol = DataFrame.from_table("boston2cols")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Perform ZTest analysis on input data column that
        #            contains data for the first sample population and
        #            variance of the first sample population.
        obj = ZTest(data=titanic_data,
                    first_sample_column='age',
                    first_sample_variance=5)
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Perform ZTest analysis on input data column that
        #            contains data for the first and second sample
        #            population and variance of the first and second sample
        #            population by specifying data_partition_column as ANY.
        # To partition data using ANY, one must import 'PartitionKind' module first,
        # then pass PartitionKind.ANY as input to "data_partition_column" argument.
        from teradataml import PartitionKind
        obj = ZTest(data=titanic_data,
                    alpha=0.5,
                    data_partition_column=PartitionKind.ANY,
                    data_order_column='pclass',
                    first_sample_column='age',
                    second_sample_column='parch',
                    alternate_hypothesis='two-tailed',
                    first_sample_variance=5,
                    second_sample_variance=8,
                    mean_under_h0=0)
        # Print the result DataFrame.
        print(obj.result)
        # Example 3: Perform ZTest analysis on input data column that
        #            contains data for the first and second sample
        #            population and variance of the first and second sample
        #            population by specifying sample_name_column, sample_value_column,
        #            first_sample_name and second_sample_name.
        obj = ZTest(data=bostonCol,
                    first_sample_name='NOX',
                    second_sample_name='RM',
                    sample_name_column='groupName',
                    sample_value_column='groupValue')
        # Print the result DataFrame.
        print(obj.result)
        # ExPerform ZTest analysis on input data column that
        #            contains data for the first sample population and
        #            variance of the first sample population by specifying
        #            sample_name_column, sample_value_column.
        obj = ZTest(data=boston,
                    first_sample_name='NOX',
                    sample_name_column='groupName',
                    sample_value_column='groupValue')
        # Print the result DataFrame.
        print(obj.result)
    MODEL EVALUATION functions
    ClassificationEvaluator
    ClassiﬁcationEvaluator
    Functions
    ClassiﬁcationEvaluator(data=None, observation_column=None, prediction_column=None, num_labels=None, labels=None, **generic_arguments)
    DESCRIPTION:
        In classification problems, a confusion matrix is used to visualize the
        performance of a classifier. The confusion matrix contains predicted labels
        represented across the row-axis and actual labels represented across the column-axis.
        Each cell in the confusion matrix corresponds to the count of occurrences
        of labels in the test data. The ClassificationEvaluator() function evaluate
        and emits various metrics of classification model based on its predictions on the data.
        Apart from accuracy, the secondary output data returns micro, macro, and weighted-averaged
        metrics of precision, recall, and F1-score values.
        Notes:
             * The function works for multi-class scenarios as well. In any case, the
               primary output data contains class-level metrics, whereas the secondary
               output data contains metrics that are applicable across classes.
             * The function works only when columns specified in 'observation_column'
               and 'prediction_column' has same teradata types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the teradataml DataFrame, containing expected and predicted labels.
            Types: teradataml DataFrame
        observation_column:
            Required Argument.
            Specifies the column name in "data" containing observation labels.
            Types: str
        prediction_column:
            Required Argument.
            Specifies the column name in "data" containing predicted labels.
            Types: str
        num_labels:
            Optional Argument.
            Specifies the number of labels in the dataset.
            Note:
                Argument is ignored if "labels" argument is used.
            Allowed Values: 1 <= num_labels <= 512
            Types: int
        labels:
            Optional Argument.
            Specifies the list of all predicted labels in the input.
            Provide either "num_labels" argument or "labels" argument.
            Types: str OR list of str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of ClassificationEvaluator.
        Output teradataml DataFrames can be accessed using attribute
        references, such as  ClassificationEvaluatorObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Example 1 : Evaluate the classification model generated to predict the labels
        #             'crash', 'nocrash' using the predicted data.
        # Load the example data.
        load_example_data("textparser", ["complaints", "stop_words"])
        # Create teradataml DataFrame objects.
        complaints = DataFrame.from_table("complaints")
        stop_words = DataFrame.from_table("stop_words")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Tokenize the "text_column" and accumulate result by "doc_id" and "category".
        complaints_tokenized = TextParser(data=complaints,
                                          text_column="text_data",
                                          object=stop_words,
                                          remove_stopwords=True,
                                          accumulate=["doc_id", "category"])
        # Calculate the conditional probabilities for token-category pairs.
        NaiveBayesTextClassifierTrainer_out = NaiveBayesTextClassifierTrainer(data=complaints_tokenized.result,
                                                                              token_column="token",
                                                                              doc_category_column="category")
        # Print the result DataFrames.
        print(NaiveBayesTextClassifierTrainer_out.result)
        print(NaiveBayesTextClassifierTrainer_out.model_data)
        # Score the data using NaiveBayesTextClassifierPredict() on model generated by
        # NaiveBayesTextClassifier() where model_type is "MULTINOMIAL".
        nbt_predict_out = NaiveBayesTextClassifierPredict(object = NaiveBayesTextClassifierTrainer_out.model_data,
                                                          newdata = complaints_tokenized.result,
                                                          input_token_column = 'token',
                                                          accumulate="category",
                                                          doc_id_columns = 'doc_id')
        # Print the result DataFrame.
        print(nbt_predict_out.result)
        # Convert prediction column and category column to same DataType.
        predicted_data = ConvertTo(data = nbt_predict_out.result,
                                   target_columns = ["category", "prediction"],
                                   target_datatype = ["VARCHAR(charlen=20,charset=UNICODE,casespecific=NO)"])
        # Evaluate classification.
        ClassificationEvaluator_obj = ClassificationEvaluator(data=predicted_data.result,
                                                              observation_column='category',
                                                              prediction_column='prediction',
                                                              labels=['no_crash','crash'])
        # Print the result DataFrames.
        print(ClassificationEvaluator_obj.result)
        print(ClassificationEvaluator_obj.output_data)
    RegressionEvaluator
    RegressionEvaluator
    Functions
    RegressionEvaluator(data=None, observation_column=None, prediction_column=None, metrics=None, independent_features_num=None,
    freedom_degrees=None)
    DESCRIPTION:
        The RegressionEvaluator() function computes metrics to evaluate and compare
        multiple models and summarizes how close predictions are to their expected
        values.
        Notes:
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
            * For information about PTCs, see Teradata Vantage™ - Analytics Database
              International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        observation_column:
            Required Argument.
            Specifies the column name in "data" containing observation labels.
            Types: str
        prediction_column:
            Required Argument.
            Specifies the column name in "data" containing predicted labels.
            Types: str
        metrics:
            Optional Argument.
            Specifies the list of evaluation metrics. The function returns
            the following metrics if the list is not provided:
                MAE:
                    Mean absolute error (MAE) is the arithmetic average of the
                    absolute errors between observed values and predicted values.
                MSE:
                    Mean squared error (MSE) is the average of the squares of
                    the errors between observed values and predicted values.
                MSLE:
                    Mean Square Log Error (MSLE) is the relative difference
                    between the log-transformed observed values and predicted
                    values.
                MAPE:
                    Mean Absolute Percentage Error (MAPE) is the mean or
                    average of the absolute percentage errors of forecasts.
                MPE:
                    Mean percentage error (MPE) is the computed average
                    of percentage errors by which predicted values differ from
                    observed values.
                RMSE:
                    Root means squared error (MSE) is the square root of the
                    average of the squares of the errors between observed
                    values and predicted values.
                RMSLE:
                    Root means Square Log Error (MSLE) is the square root
                    of the relative difference between the log-transformed
                    observed values and predicted values.
                R2:
                    R Squared (R2) is the proportion of the variation in the
                    dependent variable that is predictable from the independent
                    variable(s).
                AR2:
                    Adjusted R-squared (AR2) is a modified version of R-squared
                    that has been adjusted for the independent variable(s) in
                    the model.
                EV:
                    Explained variation (EV) measures the proportion to which
                    a mathematical model accounts for the variation (dispersion)
                    of a given data set.
                ME:
                    Max-Error (ME) is the worst-case error between observed
                    values and predicted values.
                MPD:
                    Mean Poisson Deviance (MPD) is equivalent to Tweedie
                    Deviances when the power parameter value is 1.
                MGD:
                    Mean Gamma Deviance (MGD) is equivalent to Tweedie
                    Deviances when the power parameter value is 2.
                FSTAT:
                    F-statistics (FSTAT) conducts an F-test. An F-test
                    is any statistical test in which the test statistic has an
                    F-distribution under the null hypothesis.
                    * F_score:
                        F_score value from the F-test.
                    * F_Critcialvalue:
                        F critical value from the F-test. (alpha, df1, df2,
                        UPPER_TAILED), alpha = 95%
                    * p_value:
                        Probability value associated with the F_score value
                        (F_score, df1, df2, UPPER_TAILED)
                    * F_conclusion:
                        F-test result, either 'reject null hypothesis' or
                        'fail to reject null hypothesis'. If F_score >
                        F_Critcialvalue, then 'reject null hypothesis'
                        Else 'fail to reject null hypothesis'
            Types: str OR list of strs
        independent_features_num:
            Optional Argument.
            Specifies the number of independent variables in the model.
            Required with Adjusted R Squared metric, else ignored.
            Types: int
        freedom_degrees:
            Optional Argument.
            Specifies the numerator degrees of freedom (df1) and denominator
            degrees of freedom (df2). Required with fstat metric, else ignored.
            Types: int OR list of ints
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of RegressionEvaluator.
        Output teradataml DataFrames can be accessed using attribute
        references, such as RegressionEvaluatorObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "val"
        # Create required teradataml DataFrame.
        # Load the example data.
        load_example_data("teradataml", "titanic")
        # Create teradataml DataFrame objects.
        titanic = DataFrame.from_table("titanic")
        # First generate linear regression model using LinReg() function from 'valib'.
        lin_reg_obj = valib.LinReg(data=titanic,
                                   columns=["age", "survived", "pclass"],
                                   response_column="fare")
        # Score the data using the linear regression model generated above.
        obj = valib.LinRegPredict(data=titanic,
                                  model=lin_reg_obj.model,
                                  accumulate = "fare",
                                  response_column="fare_prediction")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Compute 'RMSE', 'R2' and 'FSTAT' metrics to evaluate
        #             the model.
        RegressionEvaluator_out = RegressionEvaluator(data = obj.result,
                                                      observation_column = "fare",
                                                      prediction_column = "fare_prediction",
                                                      freedom_degrees = [1, 2],
                                                      independent_features_num = 2,
                                                      metrics = ['RMSE','R2','FSTAT'])
        # Print the result DataFrame.
        print(RegressionEvaluator_out.result)
    ROC
    ROC
    Functions
    ROC(data=None, probability_column=None, observation_column=None, model_id_column=None, positive_class='1', num_thresholds=50, auc=True, gini=True,
    **generic_arguments)
    DESCRIPTION:
        The Receiver Operating Characteristic (ROC) function accepts
        a set of prediction-actual pairs for a binary classification
        model and calculates the following values for a range
        of discrimination thresholds:
            * True positive rate (TPR)
            * False positive rate (FPR)
            * The area under the ROC curve (AUC)
            * Gini coefficient
        A receiver operating characteristic (ROC) curve shows the
        performance of a binary classification model as its discrimination
        threshold varies. For a range of thresholds, the curve plots the
        true positive rate against the false-positive rate.
        Notes:
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
              For information about PTCs, see Teradata Vantage™ - Analytics Database
              International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame that contains the
            prediction-actual pairs for a binary classifier.
            Types: teradataml DataFrame
        probability_column:
            Required Argument.
            Specifies the input column in "data" that contains
            the predictions.
            Types: str
        observation_column:
            Required Argument.
            Specifies the input column in "data" that contains
            the actual classes.
            Types: str
        model_id_column:
            Optional Argument.
            Specifies the input column in "data" that contains the
            model or partition identifiers for the ROC curves.
            Types: str
        positive_class:
            Optional Argument.
            Specifies the label of the positive class.
            Default Value: '1'
            Types: str
        num_thresholds:
            Optional Argument.
            Specifies the number of threshold for the function to use. The
            "num_threshold" must be in the range [1, 10000]. The
            function uniformly distributes the thresholds between 0 and 1.
            Default Value: 50
            Types: int
        auc:
            Optional Argument.
            Specifies whether the function displays the AUC calculated from the
            ROC values(thresholds, false positive rates, and true positive rates).
            Default Value: True
            Types: bool
        gini:
            Optional Argument.
            Specifies whether the function displays the gini coefficient
            calculated from the ROC values.
            The Gini coefficient is an inequality measure among the values of
            a frequency distribution. A Gini coefficient of 0 indicates that
            all values are the same. The closer the Gini coefficient is to 1,
            the more unequal are the values in the distribution.
            Default Value: True
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of ROC.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ROCObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("roc", ["roc_input"])
        # Create teradataml DataFrame objects.
        roc_input = DataFrame.from_table("roc_input")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Calculating True-Positive Rate (TPR), False-Positive Rate (FPR),
        #             Area Under the ROC Curve (AUC), Gini Coefficient for a range
        #             of discrimination thresholds.
        roc_out = ROC(probability_column="probability",
                      observation_column="observation",
                      model_id_column="model_id",
                      positive_class="1",
                      data=roc_input)
        # Print the result DataFrame.
        print(roc_out.result)
        print(roc_out.output_data)
    Silhouette
    Silhouette
    Functions
    Silhouette(data=None, accumulate=None, id_column=None, cluster_id_column=None, target_columns=None, output_type='SCORE', **generic_arguments)
    DESCRIPTION:
        The Silhouette() function refers to a method of interpretation and validation of consistency within
        clusters of data. The function determines how well the data is clustered among clusters.
        The silhouette value determines the similarity of an object to its cluster (cohesion) compared to
        other clusters (separation). The silhouette plot displays a measure of how close each point in one
        cluster is to the points in the neighbouring clusters and thus provides a way to assess parameters
        like the optimal number of clusters.
        The silhouette scores and its definitions are as follows:
            1: Data is appropriately clustered
            -1: Data is not appropriately clustered
            0: Datum is on the border of two natural clusters
        Notes:
            * The algorithm used in this function is of the order of N*N (where N is the number of rows). Hence,
              expect the query to run significantly longer as the number of rows increases in the input data.
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
                For information about PTCs, see Teradata Vantage™ - Analytics Database International Character Set
                Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the output.
            Note:
                Applicable only when "output_type" is set to 'SAMPLE_SCORES'.
            Types: str OR list of Strings (str)
        id_column:
            Required Argument.
            Specifies the column which is the unique identifier of input rows.
            Types: str
        cluster_id_column:
            Required Argument.
            Specifies the column containing assigned cluster IDs for input data points.
            Types: str
        target_columns:
            Required Argument.
            Specifies the columns/features to be used for calculating silhouette score.
            Types: str OR list of Strings (str)
        output_type:
            Optional Argument.
            Specifies the output type or format.
            Permitted Values:
                * SCORE: Outputs Average Silhouette Score,
                * SAMPLE_SCORES: Outputs Silhouette Score for each input sample,
                * CLUSTER_SCORES: Outputs Average Silhouette Score for each cluster.
            Default Value: "SCORE"
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of Silhouette.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SilhouetteObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["mobile_data"])
        # Create teradataml DataFrame objects.
        mobile_data = DataFrame.from_table("mobile_data")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Find the silhouette score for each input sample.
        Silhouette_result1 = Silhouette(accumulate=['feature'],
                                        id_column="row_id",
                                        cluster_id_column="userid",
                                        target_columns='"value"',
                                        output_type="SAMPLE_SCORES",
                                        data=mobile_data)
        # Print the result DataFrame.
        print(Silhouette_result1.result)
        # Example 2: Find average silhouette score of all input samples.
        Silhouette_result2 = Silhouette(id_column="row_id",
                                        cluster_id_column="userid",
                                        target_columns=['"value"'],
                                        data=mobile_data,
                                        output_type="SCORE")
        # Print the result DataFrame.
        print(Silhouette_result2.result)
        # Example 3: Find average silhouette scores of input samples for each cluster.
        Silhouette_result3 = Silhouette(id_column="row_id",
                                        cluster_id_column="userid",
                                        target_columns=['"value"'],
                                        data=mobile_data,
                                        output_type="CLUSTER_SCORES")
        # Print the result DataFrame.
        print(Silhouette_result3.result)
    TrainTestSplit
    TrainTestSplit
    Functions
    TrainTestSplit(data=None, id_column=None, stratify_column=None, seed=None, train_size=0.75, test_size=0.25, **generic_arguments)
    DESCRIPTION:
        The TrainTestSplit() function simulates how a model would
        perform on new data. The function divides the dataset into
        train and test subsets to evaluate machine learning algorithms
        and validate processes. The first subset is used to train
        the model. The second subset is used to make predictions and
        compare the predictions to actual values.
        Notes:
            * The TrainTestSplit() function gives consistent results
              across multiple runs on same machine. With different machines,
              it might produce different train and test datasets.
            * Requires the UTF8 client character set for UNICODE data.
            * Does not support Pass Through Characters (PTCs).
            * Does not support KanjiSJIS or graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame on which split
            to be performed.
            Types: teradataml DataFrame
        id_column:
            Optional Argument.
            Specifies the input data column name that has the
            unique identifier for each row in the input.
            Notes:
                * Mandatory when "seed" argument is present so that the
                  output of TrainTestSplit() function is deterministic
                  across multiple function calls.
            Types: str
        stratify_column:
            Optional Argument.
            Specifies column name that contains the labels indicating
            which data needs to be stratified.
            Types: str
        seed:
            Optional Argument.
            Specifies the seed value that controls the shuffling applied
            to the data before applying the split. Pass an int for reproducible
            output across multiple function calls.
            Notes:
                * When the argument is not specified, different
                  runs of the query generate different outputs.
                * It must be in the range [0, 2147483647]
            Types: int
        train_size:
            Optional Argument.
            Specifies the size of the train dataset.
            Notes:
                * If both "train_size" and "test_size" arguments are specified,
                  then their sum must be equal to 1.
                * "train_size" and "test_size" should be greater than the number
                  of classes when using stratify.
                * If the input data does not have an identifier column, then
                  FillRowId() function can be used to generate one.
                * It must be in the range (0, 1)
            Default Value: 0.75
            Types: float
        test_size:
            Optional Argument.
            Specifies the size of the test dataset.
            Note:
                * It must be in the range (0, 1)
            Default Value: 0.25
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of TrainTestSplit.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as TrainTestSplitObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame objects.
        data_input = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Split the input data to test dataset and train dataset,
        #             with ratio of test:train is 20:80. Note that output
        #             of TrainTestSplit() function contains 'TD_IsTrainRow'
        #             column in which '0' represent test data and '1'
        #             represent train data.
        TrainTestSplit_out = TrainTestSplit(data = data_input,
                                            id_column="passenger",
                                            train_size=0.80,
                                            test_size=0.20,
                                            seed=42)
        # Print the result DataFrame.
        print(TrainTestSplit_out.result)
    MODEL TRAINING functions
    DecisionForest
    DecisionForest
    Functions
    DecisionForest(formula=None, data=None, input_columns=None, response_column=None, max_depth=5, num_trees=-1, min_node_size=1, mtry=-1, mtry_seed=1,
    tree_type='REGRESSION', tree_size=-1, coverage_factor=1.0, min_impurity=0.0, **generic_arguments)
    DESCRIPTION:
        The decision forest model function is an ensemble algorithm used for classification
        and regression predictive modeling problems. It is an extension of bootstrap
        aggregation (bagging) of decision trees. Typically, constructing a decision tree
        involves evaluating the value for each input feature in the data to select a split point.
        The function reduces the features to a random subset (that can be considered at each split point);
        the algorithm can force each decision tree in the forest to be very different to 
        improve prediction accuracy.
        The function uses a training dataset to create a predictive model. The DecisionForestPredict()
        function uses the model created by the DecisionForest() function for making predictions.
        The function supports regression, binary, and multi-class classification.
        Notes:
            * All input features are numeric. Convert the categorical columns to numerical
              columns as preprocessing step.
            * For classification, class labels ("response_column" values) can only be integers.
            * Any observation with a missing value in an input column is skipped and 
              not used for training. One can use either SimpleImpute() or FillNa() and valib.Transform() function
              to assign missing values.
        The number of trees built by the function depends on the "num_trees", "tree_size",
        "coverage_factor" values, and the data distribution in the cluster. The trees are constructed
        in parallel by all the AMPs, which have a non-empty partition of data.
            * When you specify the "num_trees" value, the number of trees built by the function is adjusted as:
                "Number_of_trees = Num_AMPs_with_data * (num_trees/Num_AMPs_with_data)" 
            * To find out number of AMPs with data value, please use hashamp() + 1 function 
              from teradataml extension with sqlalchemy.
            * When you do not specify the "num_trees" value, the number of trees built by an AMP is calculated as:
                "Number_of_AMP_trees = coverage_factor * Num_Rows_AMP / tree_size"
              The number of trees built by the function is the sum of Number_of_AMP_trees.
            * The "tree_size" value determines the sample size used to build a tree in the forest and
              depends on the memory available to the AMP. By default, this value is computed internally
              by the function. The function reserves approximately 40% of its available memory to store
              the input sample, while the rest is used to build the tree.
    PARAMETERS:
        formula:
            Required Argument when "input_columns" and "response_column" are not provided,
            optional otherwise.
            Specifies a string consisting of "formula". Specifies the model to be fitted. 
            Only basic formula of the "col1 ~ col2 + col3 +..." form are 
            supported and all variables must be from the same teradataml 
            DataFrame object. The response should be column of type float, int or bool.
            Notes:
                * The function only accepts numeric features. User must convert the categorical
                  features to numeric values, before passing to the formula.
                * In case, categorical features are passed to formula, those are ignored, and
                  only numeric features are considered.
                * Provide either "formula" argument or "input_columns" and "response_column" arguments.
            Types: str
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        input_columns:
            Required Argument when "formula" is not provided, optional otherwise.
            Specifies the names of the input DataFrame columns to be used for
            training the model (predictors, features or independent variables).
            Note:
                * Provide either "formula" argument or "input_columns" and
                  "response_column" arguments.
            Types: str OR list of Strings (str)
        response_column:
            Required Argument when "formula" is not provided, optional otherwise.
            Specifies the name of the column containing the class label for 
            classification or target value (dependent variable) for regression.
            Note:
                * Provide either "formula" argument or "input_columns" and
                  "response_column" arguments.
            Types: str
        max_depth:
            Optional Argument.
            Specifies a decision tree stopping criterion. If the tree reaches a 
            depth past this value, the algorithm stops looking for splits. 
            Decision trees can grow to (2^(max_depth+1)-1) nodes. This stopping 
            criterion has the greatest effect on the performance of the function
            Note:
                Must be a non-negative integer value.
            Default Value: 5
            Types: int
        num_trees:
            Optional Argument.
            Specifies the number of trees to grow in the forest model. When 
            specified, the number of trees must be greater than or equal to the 
            number of AMPs with data. By default, the function builds the minimum 
            number of trees that provides the input dataset with coverage based 
            on "coverage_factor".
            Default Value: -1
            Types: int
        min_node_size:
            Optional Argument.
            Specifies the minimum number of observations in a tree node.
            The algorithm stops splitting a node if the number of observations
            in the node is equal to or smaller than this value. You must specify
            a non-negative integer value. 
            Default Value: 1
            Types: int
        mtry:
            Optional Argument.
            Specifies the number of features from input columns for evaluating
            the best split of a node. A higher value improves the splitting and
            performance of a tree. A smaller value improves the robustness of 
            the forest and prevents it from overfitting. When the value is -1,
            all variables are used for each split.
            Default Value: -1
            Types: int
        mtry_seed:
            Optional Argument.
            Specifies  the random seed that the algorithm uses for the "mtry" argument. 
            Default Value: 1
            Types: int
        seed:
            Optional Argument.
            Specifies the random seed that the algorithm uses for repeatable results.
            Default Value: 1
            Types: int
        tree_type:
            Optional Argument.
            Specifies whether the analysis is a regression (continuous response 
            variable) or a multiple-class classification (predicting result from 
            the number of classes).
            Default Value: "REGRESSION"
            Permitted Values: REGRESSION, CLASSIFICATION
            Types: str
        tree_size:
            Optional Argument.
            Specifies the number of rows that each tree uses as its input dataset. 
            The function builds a tree using either the number of rows on an AMP, 
            the number of rows that fit into the AMP"s memory (whichever is less),
            or the number of rows given by the "tree_size" argument. By 
            default, this value is the minimum of the number of rows on an AMP,
            and the number of rows that fit into the AMP"s memory.
            Default Value: -1
            Types: int
        coverage_factor:
            Optional Argument.
            Specifies the level of coverage for the dataset while building trees,
            in percentage. 
            For example, 1.25 = 125% coverage.
            Notes:
                * "coverage_factor" can only be used when "num_trees" is not specified.
                * When "num_trees" is specified, coverage depends on the value of 
                  the "num_trees".
                * When "num_trees" is not specified, "num_trees" is chosen to achieve 
                  level of coverage specified by this argument.
                * A higher coverage level will ensure a higher probability of each row in input 
                  data to be selected during the tree building process (at the cost of building 
                  more trees).
                * Because of internal sampling in bootstrapping, some rows may be chosen
                  multiple times, and some not at all.
            Default Value: 1.0
            Types: float OR int
        min_impurity:
            Optional Argument.
            Specifies the minimum impurity at which the tree stops splitting 
            further down. For regression, a criteria of squared error is used 
            whereas for classification, gini impurity is used.
            Default Value: 0.0
            Types: float OR int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.    
    RETURNS:
        Instance of DecisionForest.
        Output teradataml DataFrames can be accessed using attribute
        references, such as DecisionForestObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("decisionforest", ["boston"])
        # Create teradataml DataFrame objects.
        boston_sample = DataFrame.from_table("boston")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Generate decision forest regression model using
        #             input dataframe and input_columns and response_column
        #             instead of formula.
        DecisionForest_out = DecisionForest(data = boston_sample, 
                                input_columns = [ 'crim', 'zn', 'indus', 'chas', 'nox', 'rm',
                                                'age', 'dis', 'rad', 'tax', 'ptratio', 'black',
                                                'lstat'], 
                                response_column = 'medv', 
                                max_depth = 12, 
                                num_trees = 4, 
                                min_node_size = 1, 
                                mtry = 3, 
                                mtry_seed = 1, 
                                seed = 1, 
                                tree_type = 'REGRESSION')
        # Print the result DataFrame.
        print(DecisionForest_out.result)  
        # Example 2 : Generate decision forest regression model using
        #             input teradataml dataframe and provided formula.
        DecisionForest_out = DecisionForest(data = boston_sample, 
                                formula = "medv ~ crim + zn + indus + chas + nox + rm + age + dis + rad + tax + ptratio + 
                                max_depth = 12, 
                                num_trees = 4, 
                                min_node_size = 1, 
                                mtry = 3, 
                                mtry_seed = 1, 
                                seed = 1, 
                                tree_type = 'REGRESSION')
        # Print the result DataFrame.
        print(DecisionForest_out.result)
    GLM
    GLM
    Functions
    GLM(formula=None, data=None, input_columns=None, response_column=None, family='GAUSSIAN', iter_max=300, batch_size=10, lambda1=0.02, alpha=0.15, iter
    class_weights='0:1.0, 1:1.0', learning_rate=None, initial_eta=0.05, decay_rate=0.25, decay_steps=5, momentum=0.0, nesterov=True, local_sgd_iterations=0, stepwis
    attribute_data=None, parameter_data=None, iteration_mode='BATCH', partition_column=None, **generic_arguments)
    DESCRIPTION:
        The generalized linear model (GLM) function performs regression and classification
        analysis on data sets, where the response follows an exponential family distribution
        and supports the following models:
            * Regression (GAUSSIAN family): The loss function is squared error.
            * Binary Classification (BINOMIAL family): The loss function is logistic and
                                                       implements logistic regression.
                                                       The only response values are 0 or 1.
        The function uses the Minibatch Stochastic Gradient Descent (SGD) algorithm that is highly
        scalable for large datasets. The algorithm estimates the gradient of loss in minibatches,
        which is defined by the "batch_size" argument and updates the model with a learning rate using
        the "learning_rate" argument.
        The function also supports the following approaches:
            * L1, L2, and Elastic Net Regularization for shrinking model parameters.
            * Accelerated learning using Momentum and Nesterov approaches.
        The function uses a combination of "iter_num_no_change" and "tolerance" arguments
        to define the convergence criterion and runs multiple iterations (up to the specified
        value in the "iter_max" argument) until the algorithm meets the criterion.
        The function also supports LocalSGD, a variant of SGD, that uses "local_sgd_iterations"
        on each AMP to run multiple batch iterations locally followed by a global iteration.
        The weights from all mappers are aggregated in a reduce phase and are used to compute
        the gradient and loss in the next iteration. LocalSGD lowers communication costs and
        can result in faster learning and convergence in fewer iterations, especially when there
        is a large cluster size and many features.
        Due to gradient-based learning, the function is highly-sensitive to feature scaling.
        Before using the features in the function, you must standardize the Input features
        using ScaleFit() and ScaleTransform() functions.
        The function only accepts numeric features. Therefore, before training, you must convert
        the categorical features to numeric values.
        The function skips the rows with missing (null) values during training.
        The function output is a trained GLM model that is used as an input to the TDGLMPredict()
        function. The model also contains model statistics of MSE, Loglikelihood, AIC, and BIC.
        You can use RegressionEvaluator(), ClassificationEvaluator(), and ROC() functions to perform
        model evaluation as a post-processing step.
    PARAMETERS:
        formula:
            Required Argument when "input_columns" and "response_column" are not provided,
            optional otherwise.
            Specifies a string consisting of "formula". Specifies the model to be fitted.
            Only basic formula of the "col1 ~ col2 + col3 +..." form are
            supported and all variables must be from the same teradataml
            DataFrame object.
            Notes:
                * The function only accepts numeric features. User must convert the categorical
                  features to numeric values, before passing to the formula.
                * In case, categorical features are passed to formula, those are ignored, and
                  only numeric features are considered.
                * Provide either "formula" argument or "input_columns" and "response_column" arguments.
            Types: str
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        input_columns:
            Required Argument when "formula" is not provided, optional otherwise.
            Specifies the name(s) of the column(s) in "data" to be used for
            training the model (predictors, features or independent variables).
            Note:
                * Provide either "formula" argument or "input_columns" and "response_column" arguments.
            Types: str OR list of Strings (str)
        response_column:
            Required Argument when "formula" is not provided, optional otherwise.
            Specifies the name of the column that contains the class label for
            classification or target value (dependent variable) for regression.
            Note:
                * Provide either "formula" argument or "input_columns" and "response_column" arguments.
            Types: str
        family:
            Optional Argument.
            Specifies the distribution exponential family.
            Permitted Values: BINOMIAL, GAUSSIAN
            Default Value: GAUSSIAN
            Types: str
        iter_max:
            Optional Argument.
            Specifies the maximum number of iterations over the training data
            batches. If the batch size is 0, "iter_max" equals the number of
            epochs (an epoch is a single pass over entire training data). If
            there are 1000 rows in an AMP, and batch size is 10, then 100
            iterations will result into one epoch and 500 iterations will result
            into 5 epochs over this AMP's data. Because it is not guaranteed
            that the data will be equally distributed on all AMPs, this may
            result into different number of epochs for other AMPs.
            Note:
                * It must be a positive value less than 10,000,000.
            Default Value: 300
            Types: int
        batch_size:
            Optional Argument.
            Specifies the number of observations (training samples) to be parsed
            in one mini-batch. The value '0' indicates no mini-batches, the entire
            dataset is processed in each iteration, and the algorithm becomes Gradient
            Descent. A value higher than the number of rows on any AMP will also default
            to Gradient Descent.
            Note:
                * It must be a non-negative integer value.
            Default Value: 10
            Types: int
        lambda1:
            Optional Argument.
            Specifies the amount of regularization to be added. The higher the
            value, the stronger the regularization. It is also used to compute
            the learning rate when the "learning_rate" is set to 'OPTIMAL'.
            A value '0' means no regularization.
            Note:
                * It must be a non-negative float value.
            Default Value: 0.02
            Types: float OR int
        alpha:
            Optional Argument.
            Specifies the Elasticnet parameter for penalty computation. It only
            becomes effective when "lambda1" greater than 0. The value represents
            the contribution ratio of L1 in the penalty. A value '1.0' indicates
            L1 (LASSO) only, a value '0' indicates L2 (Ridge) only, and a value
            in between is a combination of L1 and L2. Default value is 0.15.
            Note:
                * It must be a float value between 0 and 1.
            Default Value: 0.15(15% L1, 85% L2)
            Types: float OR int
        iter_num_no_change:
            Optional Argument.
            Specifies the number of iterations (batches) with no improvement in
            loss (including the tolerance) to stop training (early stopping).
            A value of 0 indicates no early stopping and the algorithm will
            continue till "iter_max" iterations are reached.
            Note:
                * It must be a non-negative integer value.
            Default Value: 50
            Types: int
        tolerance:
            Optional Argument.
            Specifies the stopping criteria in terms of loss function improvement.
            Training stops the following condition is met:
            loss > best_loss - "tolerance" for "iter_num_no_change" times.
            Notes:
                * Only applicable when "iter_num_no_change" greater than 0.
                * It must be a non-negative value.
            Default Value: 0.001
            Types: float OR int
        intercept:
            Optional Argument.
            Specifies whether to estimate intercept or not based on
            whether "data" is already centered or not.
            Default Value: True
            Types: bool
        class_weights:
            Optional Argument.
            Specifies the weights associated with classes. If the weight of a class is omitted,
            it is assumed to be 1.0.
            Note:
                * Only applicable for 'BINOMIAL' family. The format is '0:weight,1:weight'.
                  For example, '0:1.0,1:0.5' will give twice the weight to each observation
                  in class 0.
            Default Value: "0:1.0, 1:1.0"
            Types: str
        learning_rate:
            Optional Argument.
            Specifies the learning rate algorithm for SGD iterations.
            Permitted Values: CONSTANT, OPTIMAL, INVTIME, ADAPTIVE
            Default Value:
                * 'INVTIME' for 'GAUSSIAN' family , and
                * 'OPTIMAL' for 'BINOMIAL' family.
            Types: str
        initial_eta:
            Optional Argument.
            Specifies the initial value of eta for the learning rate. When
            the "learning_rate" is 'CONSTANT', this value is applicable for
            all iterations.
            Default Value: 0.05
            Types: float OR int
        decay_rate:
            Optional Argument.
            Specifies the decay rate for the learning rate.
            Note:
                * Only applicable for 'INVTIME' and 'ADAPTIVE' learning rates.
            Default Value: 0.25
            Types: float OR int
        decay_steps:
            Optional Argument.
            Specifies the decay steps (number of iterations) for the 'ADAPTIVE'
            learning rate. The learning rate changes by decay rate after the
            specified number of iterations are completed.
            Default Value: 5
            Types: int
        momentum:
            Optional Argument.
            Specifies the value to use for the momentum learning rate optimizer.
            A larger value indicates a higher momentum contribution. A value of 0
            means the momentum optimizer is disabled. For a good momentum contribution,
            a value between 0.6 and 0.95 is recommended.
            Note:
                * It must be a non-negative float value between 0 and 1.
            Default Value: 0.0
            Types: float OR int
        nesterov:
            Optional Argument.
            Specifies whether to apply Nesterov optimization to the momentum optimizer 
            or not.
            Note:
                * Only applicable when "momentum" greater than 0
            Default Value: True
            Types: bool
        local_sgd_iterations:
            Optional Argument.
            Specifies the number of local iterations to be used for Local SGD
            algorithm. A value of 0 implies Local SGD is disabled. A value higher
            than 0 enables Local SGD and that many local iterations are performed
            before updating the weights for the global model. With Local SGD algorithm,
            recommended values for arguments are as follows:
                * local_sgd_iterations: 10
                * iter_max: 100
                * batch_size: 50
                * iter_num_no_change: 5
            Note:
                * It must be a positive integer value.
            Default Value: 0
            Types: int
        stepwise_direction:
            Optional Argument.
            Specify the type of stepwise algorithm to be used.
            Permitted Values: 'FORWARD', 'BACKWARD', 'BOTH', 'BIDIRECTIONAL'
            Types: str
        max_steps_num:
            Optional Argument.
            Specifies the maximum number of steps to be used for the Stepwise Algorithm.
            Note:
                *  The "max_steps_num" must be in the range [1, 2147483647].
            Default Value: 5
            Types: int
        attribute_data:
            Optional Argument.
            Specifies the teradataml DataFrame containing the attribute data.
            Note:
                * This is valid when "data_partition_column" argument is used.
            Types: teradataml DataFrame
        parameter_data:
            Optional Argument.
            Specifies the teradataml DataFrame containing the parameter data.
            Note:
                * This is valid when "data_partition_column" argument is used.
            Types: teradataml DataFrame
        iteration_mode:
            Optional Argument.
            Specifies the iteration mode.
            Note:
                * This is valid when "data_partition_column" argument is used.
            Permitted Values: 'BATCH', 'EPOCH'
            Default Value: 'BATCH'
            Types: str
        partition_column:
            Optional Argument.
            Specifies the column names of "data" on which to partition the input.
            The name should be consistent with the "data_partition_column".
            Note:
                * If the "data_partition_column" is unicode with foreign language characters, 
                  it is necessary to specify "partition_column" argument. 
                * Column range is not supported for "partition_column" argument. 
                * This is valid when "data_partition_column" argument is used.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of GLM.
        Output teradataml DataFrames can be accessed using attribute
        references, such as GLMObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("dataframe", "admissions_train")
        # Create teradataml DataFrame objects.
        admissions_train = DataFrame.from_table("admissions_train")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # GLM() function requires features in numeric format for processing,
        # so first let's transform categorical columns to numerical columns
        # using VAL Transform() function.
        # Import required libraries.
        from teradataml import valib, OneHotEncoder, Retain
        # Configure VAL install location.
        configure.val_install_location = "VAL"
        # Define encoders for categorical columns.
        masters_code = OneHotEncoder(values=["yes", "no"],
                                     columns="masters",
                                     out_columns="masters")
        stats_code = OneHotEncoder(values=["Advanced", "Novice"],
                                   columns="stats",
                                   out_columns="stats")
        programming_code = OneHotEncoder(values=["Advanced", "Novice", "Beginner"],
                                         columns="programming",
                                         out_columns="programming")
        # Retain numerical columns.
        retain = Retain(columns=["admitted", "gpa"])
        # Transform categorical columns to numeric columns.
        glm_numeric_input = valib.Transform(data=admissions_train,
                                            one_hot_encode=[masters_code, stats_code,
                                            programming_code],
                                            retain=retain)
        # Example 1 : Generate generalized linear model(GLM) using
        #             input dataframe and provided formula.
        GLM_out_1 = GLM(formula = "admitted ~ gpa + yes_masters + no_masters + Advanced_stats + Novice_stats + Advanced_pr
                        data = glm_numeric_input.result,
                        learning_rate = 'INVTIME',
                        momentum = 0.0
                        )
        # Print the result DataFrame.
        print(GLM_out_1.result)
        print(GLM_out_1.output_data)
        # Example 2 : Generate generalized linear model(GLM) using
        #             input dataframe and input_columns and response_column
        #             instead of formula.
        GLM_out_2 = GLM(input_columns= ["gpa", "yes_masters", "no_masters",
                                        "Advanced_stats", "Novice_stats",
                                        "Advanced_programming", "Novice_programming",
                                        "Beginner_programming"],
                        response_column = "admitted",
                        data = glm_numeric_input.result,
                        learning_rate = 'INVTIME',
                        momentum = 0.0
                        )
        # Print the result DataFrame.
        print(GLM_out_2.result)
        print(GLM_out_2.output_data)
        # Example 3 : Generate generalized linear model(GLM) using stepwise regression algorithm.
        #             This example uses the boston dataset and scales the data.
        #             Scaled data is used as input data to generate the GLM model.
        # loading the example data
        load_example_data("decisionforest", ["boston"])
        load_example_data('glm', ['housing_train_segment', 'housing_train_parameter', 'housing_train_attribute'])
        # Create teradataml DataFrame objects.
        boston_df = DataFrame('boston')
        housing_seg = DataFrame('housing_train_segment')
        housing_parameter = DataFrame('housing_train_parameter')
        housing_attribute = DataFrame('housing_train_attribute')
        # Scaling the data
        # Scale "target_columns" with respect to 'STD' value of the column.
        fit_obj = ScaleFit(data=boston_df,
                        target_columns=['crim','zn','indus','chas','nox','rm','age','dis','rad','tax','ptratio','black','l
                        scale_method="STD")
        # Scale values specified in the input data using the fit data generated by the ScaleFit() function above.
        obj = ScaleTransform(object=fit_obj.output,
                            data=boston_df,
                            accumulate=["id","medv"])
        boston = obj.result
        # Generate generalized linear model(GLM) using stepwise regression algorithm.
        glm_1 = GLM(data=boston,
                    input_columns=['indus','chas','nox','rm'],
                    response_column='medv',
                    family='GAUSSIAN',
                    lambda1=0.02,
                    alpha=0.33,
                    batch_size=10,
                    learning_rate='optimal',
                    iter_max=36,
                    iter_num_no_change=100,
                    tolerance=0.0001,
                    initial_eta=0.02,
                    stepwise_direction='backward',
                    max_steps_num=10)
        # Print the result DataFrame.
        print(glm_1.result)
        # Example 4 : Generate generalized linear model(GLM) using
        #             stepwise regression algorithm with initial_stepwise_columns.
        glm_2 = GLM(data=boston,
                    input_columns=['crim','zn','indus','chas','nox','rm','age','dis','rad','tax','ptratio','black','lstat'
                    response_column='medv',
                    family='GAUSSIAN',
                    lambda1=0.02,
                    alpha=0.33,
                    batch_size=10,
                    learning_rate='optimal',
                    iter_max=36,
                    iter_num_no_change=100,
                    tolerance=0.0001,
                    initial_eta=0.02,
                    stepwise_direction='bidirectional',
                    max_steps_num=10,
                    initial_stepwise_columns=['rad','tax']
            )
        # Print the result DataFrame.
        print(glm_2.result)
        # Example 5 : Generate generalized linear model(GLM) using partition by key.
        glm_3 = GLM(data=housing_seg,
                    input_columns=['bedrooms', 'bathrms', 'stories', 'driveway', 'recroom', 'fullbase', 'gashw', 'airco'],
                    response_column='price',
                    family='GAUSSIAN',
                    batch_size=10,
                    iter_max=1000,
                    data_partition_column='partition_id'
                    )
        # Print the result DataFrame.
        print(glm_3.result)
        # Example 6 : Generate generalized linear model(GLM) using partition by key with attribute data.
        glm_4 = GLM(data=housing_seg,
                    input_columns=['bedrooms', 'bathrms', 'stories', 'driveway', 'recroom', 'fullbase', 'gashw', 'airco'],
                    response_column='price',
                    family='GAUSSIAN',
                    batch_size=10,
                    iter_max=1000,
                    data_partition_column='partition_id',
                    attribute_data = housing_attribute,
                    attribute_data_partition_column = 'partition_id'
                    )
        # Print the result DataFrame.
        print(glm_4.result)
        # Example 7 : Generate generalized linear model(GLM) using partition by key with parameter data
        glm_5 = GLM(data=housing_seg,
                    input_columns=['bedrooms', 'bathrms', 'stories', 'driveway', 'recroom', 'fullbase', 'gashw', 'airco'],
                    response_column='homestyle',
                    family='binomial',
                    iter_max=1000,
                    data_partition_column='partition_id',
                    parameter_data = housing_parameter,
                    parameter_data_partition_column = 'partition_id'
                    )
        # Print the result DataFrame.
        print(glm_5.result)
    GLMPerSegment
    GLMPerSegment
    Functions
    GLMPerSegment(formula=None, data=None, input_columns=None, response_column=None, attribute_data=None, parameter_data=None, family='GAUSSIAN',
    iter_max=300, batch_size=10, lambda1=0.02, alpha=0.15, iter_num_no_change=50, tolerance=0.001, intercept=True, class_weights='0:1.0, 1:1.0',
    learning_rate=None, initial_eta=0.05, decay_rate=0.25, decay_steps=5, momentum=0.0, nesterov=True, iteration_mode='BATCH', partition_column=None,
    **generic_arguments)
    DESCRIPTION:
        The GLM() function is used to train the whole data set as one model. The
        GLMPerSegment() function is a partition-by-key function to create a single
        model for each partition.
        The following operations are supported for GLMPerSegment():
            * Gaussian linear regression.
            * Binomial logistic regression for binary classification.
            * Iteration modes batch and epoch.
            * Regularization using L1, L2 and Elasticnet.
            * Mini-batch gradient descent for numeric optimization algorithm.
            * Training support with and without intercept.
            * Class-weighted modeling.
            * Learning rate support for mini-batch gradient descent using constant,
              dynamic and hybrid gradients.
            * Learning rate optimization algorithms for mini-batch gradient
              descent using plain, momentum, and nesterov gradients.
        Notes:
            * The order column can be optionally applied to guarantee the result in each
              run is deterministic. The situation of indeterministic result in a partition
              can occur if the "batch_size" argument is less than the number of rows in
              the partition. However, adding order columns can influence the performance.
            * A model is generated from the GLMPerSegment(), and a model from GLM()
              should be the same when the "batch_size" argument is not less than the
              number of rows in the corresponding partition of the model.
            * GLMPerSegment() takes all features as numeric input. Categorical columns
              must be converted to numeric columns as preprocessing step, such as using
              OneHotEncodingFit()/OneHotEncodingTransform(), OrdinalEncodingFit()/
              OrdinalEncodingTransform(), and TargetEncodingFit()/TargetEncodingTransform().
            * Any observation with a missing value in an input column is ignored and
              not used fortraining. You can use some imputation function, such as
              SimpleImputeFit()/SimpleImputeTransform() to do imputation of missing values.
            * Best practice is to standardize the dataset before using GLMPerSegment().
              Standardization, also known as feature scaling, makes a better model and
              converges quicker.
            * Model evaluation metrics of MSE, Loglikelihood, AIC, and BIC are generated by
              GLMPerSegment(). For additional regression and classification metrics, you
              should use RegressionEvaluator(), ClassificationEvaluator() and ROC()
              functions as post-processing step.
            * GLMPerSegment() supports binary classification only.
            * "response_column" for classification accepts values of 0 and 1 for two
              classes in the response column.
            * A maximum of 2046 features are supported due to the limitation imposed
              by the maximum number of columns (2048) in a input data.
            * "batch_size" and "learning_rate" are directly related. With a larger "batch_size",
               "learning_rate" can be increased for faster training with fewer iterations.
            * "iter_num" and "iter_num_no_change" are criteria used to stop learning. To force
               the function to run through all iterations, disable the "iter_num_no_change"
               by specifying "iter_num_no_change" to 0.
            * User need to try different combinations to find the best values for a particular
              use case.
            * When an unsupported data type is passed in "input_columns" or "response_column",
              the error message is of the following format:
              Unsupported data type for column index n in argument InputColumns.
              In the message, n refers to the index of the column based on an input to the
              function comprising "input_columns" and "response_column only. This is due to
              the rest of the columns in input are not needed by the function and internal
              optimizer does not expose them to the function. Due to this, n might be different
              from the actual index in the input data.
    PARAMETERS:
        formula:
            Required Argument when "input_columns" and "response_column" are not
            provided, optional otherwise.
            Specifies a string consisting of "formula" which is the model to be fitted.
            Only basic formula of the "col1 ~ col2 + col3 +..." form are
            supported and all variables must be from the same teradataml
            DataFrame object.
            Notes:
                * The function only accepts numeric features. User must convert the
                  categorical features to numeric values, before passing to the formula.
                * In case categorical features are passed to formula, those are ignored,
                  and only numeric features are considered.
                * Provide either "formula" argument or "input_columns" and
                  "response_column" arguments.
            Types: str
        data:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
            Types: teradataml DataFrame
        attribute_data:
            Optional Argument.
            Specifies the teradataml DataFrame containing a subset of features
            to be used with respect to each partition.
            Types: teradataml DataFrame
        parameter_data:
            Optional Argument.
            Specifies the teradataml DataFrame containing a subset of parameters
            to be used with respect to each partition.
            Types: teradataml DataFrame
        input_columns:
            Required argument when "response_column" is provided and "formula" is not
            provided, optional otherwise.
            Specifies the name(s) of the teradataml DataFrame column(s) that need
            to be used for training the model (predictors, features, or
            independent variables).
            Types: str OR list of Strings (str)
        response_column:
            Required argument when "response_column" is provided and "formula" is not
            provided, optional otherwise.
            Specifies the name of the column that contains the class label for binary
            classification when "family" is 'Binomial', or target value (dependent
            variable) for 'Regression' when "family" is 'Gaussian'.
            Types: str
        family:
            Optional Argument.
            Specifies the distribution exponential family.
            Permitted Values:
                * BINOMIAL
                * GAUSSIAN
            Default Value: GAUSSIAN
            Types: str
        iter_max:
            Optional Argument.
            Specifies the maximum number of iterations over the training data
            batches. If batch size is 0, then "iter_max" equals the number of epochs.
            Note:
                * The "iter_max" must be in the range [1, 10000000].
            Default Value: 300
            Types: int
        batch_size:
            Optional Argument.
            Specifies the number of observations (training samples) to be parsed in
            one mini-batch. Must be a non-negative integer value. A value of 0
            indicates no mini-batches, so the entire input is processed in each
            iteration, and the algorithm becomes (Batch) Gradient Descent. A
            value higher than the number of rows on any partition also default
            to Batch Gradient Descent.
            Note:
                * The "iter_max" must be in the range [0, 2147483647].
            Default Value: 10
            Types: int
        lambda1:
            Optional Argument.
            Specifies the amount of regularization to be added. The higher the
            value, the stronger the regularization. It is also used to compute
            learning rate when "learning_rate" is set to 'Optimal'.
            A value of '0' means no regularization.
            Notes:
                * The "lambda1" must be in the range [0, 1e7].
                * The "lambda1" must be a non-negative float value.
            Default Value: 0.02
            Types: float OR int
        alpha:
            Optional Argument.
            Specifies the Elasticnet parameter for penalty computation. It is
            only effective if "lambda1" is greater than 0. The value represents
            the contribution ratio of L1 in the penalty. A value of 1.0 indicates
            L1 (LASSO) only, a value of 0 indicates L2 (Ridge) only, and a value
            in between is a combination of L1 and L2.
            Note:
                * The "alpha" must be in the range [0, 1].
            Default Value: 0.15
            Types: float OR int
        iter_num_no_change:
            Optional Argument.
            Specifies the number of iterations with no improvement in loss, including
            the "tolerance", to stop training. A value of 0 indicates no early
            stopping and the algorithm continues until "iter_max" iterations are reached.
            Note:
                * The "iter_num_no_change" must be in the range [0, 2147483647].
            Default Value: 50
            Types: int
        tolerance:
            Optional Argument.
            Specifies the stopping criteria in terms of loss function improvement.
            Training stops when loss is greater than best_loss – tolerance for
            "iter_num_no_change" times.
            Notes:
                * The "tolerance" must be in the range [1e-7, 1e7].
                * The "tolerance" works only when "iter_num_no_change"
                  is greater than 0.
                * The "tolerance" must be a non-negative value.
            Default Value: 0.001
            Types: float OR int
        intercept:
            Optional Argument.
            Specifies intercept should be estimated based on whether data is
            already centered or not.
            Default Value: True
            Types: bool
        class_weights:
            Optional Argument.
            Specifies the weights associated with classes. The format is
            '0:weight,1:weight'. For example, '0:1.0,1:0.5' gives twice
            as much weight to each observation in class 0. If the weight of
            a class is omitted, then it is assumed to be 1.0.
            Note:
                * The "class_weights" argument works only when "family" is
                  'Binomial'.
            Default Value: 0:1.0, 1:1.0
            Types: str
        learning_rate:
            Optional Argument.
            Specifies the Learning rate algorithm.
            Permitted Values:
                * CONSTANT
                * OPTIMAL
                * INVTIME
                * ADAPTIVE
            Default Value:
                * 'INVTIME' when "family" is set to 'Gaussian'
                * 'OPTIMAL' when "family" is set to 'Binomial'
            Types: str
        initial_eta:
            Optional Argument.
            Specifies the initial value of eta for learning rate. For constant,
            this value is the learning rate for all iterations.
            Note:
                * The "initial_eta" must be in the range [1e-7, 1e7].
            Default Value: 0.05.
            Types: float OR int
        decay_rate:
            Optional Argument.
            Specifies the decay rate for learning rate (invtime and adaptive).
            Note:
                * The "decay_rate" must be in the range [1e-7, 1e7].
            Default Value: 0.25.
            Types: float OR int
        decay_steps:
            Optional Argument.
            Specifies the decay steps (number of iterations) for adaptive learning rate.
            The learning rate changes by decay rate after this many number of iterations.
            Note:
                * The "decay_steps" must be in the range [1, 2147483647].
            Default Value: 5
            Types: int
        momentum:
            Optional Argument.
            Specifies the value to use for momentum learning rate optimizer.
            A larger value indicates higher momentum contribution. A value of 0
            means momentum optimizer is disabled. For a good momentum contribution,
            a value between 0.6-0.95 is recommended.
            Note:
                * The "momentum" must be in the range [0, 1].
            Default Value: 0.0
            Types: float OR int
        nesterov:
            Optional Argument.
            Specifies the indicator that nesterov optimization should be
            applied to Momentum Optimizer or not. Default is True when momentum
            is greater than 0, otherwise False.
            Default Value: True
            Types: bool
        iteration_mode:
            Optional Argument.
            Specifies the iteration mode.
            Permitted Values:
                * Batch: One iteration per batch. After processing rows in a
                  batch, update the weight of the parameters and proceed to the
                   next iteration.
                * Epoch: One iteration per epoch. After processing all rows
                  in a partition (with one or more batches), update the weight
                  of the parameters and proceed to the next epoch.
            Default Value: Batch
            Types: str OR list of Strings (str)
        partition_column:
            Optional Argument.
            Specifies the name of the "input_columns" on which to partition the input.
            The name should be consistent with the "data_partition_column". If the
            "data_partition_column" is unicode with foreign language characters, then
            it is necessary to specify "partition_column" argument.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQLE Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of GLMPerSegment.
        Output teradataml DataFrames can be accessed using attribute
        references, such as GLMPerSegmentObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("decisionforestpredict", ["housing_train"])
        load_example_data("teradataml", ["housing_train_attribute", "housing_train_parameter"])
        # Create teradataml DataFrame objects.
        housing_train = DataFrame.from_table("housing_train")
        housing_train_attribute = DataFrame.from_table("housing_train_attribute")
        housing_train_parameter = DataFrame.from_table("housing_train_parameter")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Filter the rows from train dataset with homestyle as Classic and Eclectic.
        binomial_housing_train = DataFrame('housing_train', index_label="homestyle")
        binomial_housing_train = binomial_housing_train.filter(like = 'ic', axis = 'rows')
        # GLMPerSegment() function requires features in numeric format for processing,
        # so dropping the non-numeric columns.
        binomial_housing_train = binomial_housing_train.drop(columns=["driveway", "recroom",
                                                                      "gashw", "airco", "prefarea",
                                                                      "fullbase"])
        gaussian_housing_train = binomial_housing_train.drop(columns="homestyle")
        # Transform the train dataset categorical values to encoded values.
        housing_train_ordinal_encodingfit = OrdinalEncodingFit(
            target_column='homestyle',
            data=binomial_housing_train)
        housing_train_ordinal_encodingtransform = OrdinalEncodingTransform(
            data=binomial_housing_train,
            object=housing_train_ordinal_encodingfit.result,
            accumulate=["sn", "price", "lotsize",
                        "bedrooms", "bathrms", "stories"])
        # Example 1: Train the model using the 'Gaussian' family.
        GLMPerSegment_out_1 = GLMPerSegment(data=gaussian_housing_train,
                                            data_partition_column="stories",
                                            input_columns=['garagepl', 'lotsize', 'bedrooms', 'bathrms'],
                                            response_column="price",
                                            family="Gaussian",
                                            iter_max=1000,
                                            batch_size=9)
        # Print the result DataFrame.
        print(GLMPerSegment_out_1.result)
        # Example 2: Train the model using the 'Binomial' family, formula argument and
        #            subset of features and parameters to be used with respect to
        #            "partition_id".
        formula = "homestyle ~ price + lotsize + bedrooms + bathrms"
        GLMPerSegment_out_2 = GLMPerSegment(data=housing_train_ordinal_encodingtransform.result,
                                            data_partition_column="stories",
                                            formula = formula,
                                            attribute_data=housing_train_attribute,
                                            attribute_data_partition_column="partition_id",
                                            parameter_data=housing_train_parameter,
                                            parameter_data_partition_column="partition_id",
                                            family="Binomial",
                                            iter_max=100)
        # Print the result DataFrame.
        print(GLMPerSegment_out_2.result)
    KMeans
    KMeans
    Functions
    KMeans(data=None, centroids_data=None, id_column=None, target_columns=None, num_clusters=None, seed=None, threshold=0.0395, iter_max=10,
    num_init=1, output_cluster_assignment=False, initialcentroids_method='RANDOM', **generic_arguments)
    DESCRIPTION:
        The K-means() function groups a set of observations into k clusters
        in which each observation belongs to the cluster with the nearest mean
        (cluster centers or cluster centroid). This algorithm minimizes the
        objective function, that is, the total Euclidean distance of all data points
        from the center of the cluster as follows:
            1. Specify or randomly select k initial cluster centroids.
            2. Assign each data point to the cluster that has the closest centroid.
            3. Recalculate the positions of the k centroids.
            4. Repeat steps 2 and 3 until the centroids no longer move.
        The algorithm doesn't necessarily find the optimal configuration as it
        depends significantly on the initial randomly selected cluster centers.
        User can run the function multiple times to reduce the effect of this limitation.
        Also, this function returns the within-cluster-squared-sum, which user can use to
        determine an optimal number of clusters using the Elbow method.
        Notes:
            * This function doesn't consider the "data" and "centroids_data"
              input rows that have a NULL entry in the specified "target_columns".
            * The function can produce deterministic output across different machine
              configurations if user provide the "centroids_data".
            * The function randomly samples the initial centroids from the "data",
              if "centroids_data" not provided. In this case, use of "seed"
              argument makes the function output deterministic on a machine with an
              assigned configuration. However, using the "seed" argument won't guarantee
              deterministic output across machines with different configurations.
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
            * For information about PTCs, see Teradata Vantage™ - Analytics Database
              International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        centroids_data:
            Optional Argument.
            Specifies the input teradataml DataFrame containing
            set of initial centroids.
            Note:
                * This argument is not required if "num_clusters" provided.
                * If provided, the function uses the initial centroids
                  from this input.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the input data column name that has the
            unique identifier for each row in the input.
            Types: str
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" for clustering.
            Types: str OR list of Strings (str)
        num_clusters:
            Optional Argument.
            Specifies the number of clusters to be created.
            Note:
                This argument is not required if "centroids_data" provided.
            Types: int
        seed:
            Optional Argument.
            Specifies a non-negative integer value to randomly select the initial
            cluster centroid positions from the input.
            Note:
                * This argument is not required if "centroids_data" provided.
                * Random integer value will be used for "seed", if not passed.
            Types: int
        threshold:
            Optional Argument.
            Specifies the convergence threshold. The algorithm converges if the distance
            between the centroids from the previous iteration and the current iteration
            is less than the specified value.
            Default Value: 0.0395
            Types: float OR int
        iter_max:
            Optional Argument.
            Specifies the maximum number of iterations for the K-means algorithm.
            The algorithm stops after performing the specified number of iterations
            even if the convergence criterion is not met.
            Default Value: 10
            Types: int
        num_init:
            Optional Argument.
            Specifies the number of times, the k-means algorithm will be run with different
            initial centroid seeds. The function will emit out the model having
            the least value of Total Within Cluster Squared Sum.
            Note:
                This argument is not required if "centroids_data" is provided.
            Default Value: 1
            Types: int
        output_cluster_assignment:
            Optional Argument.
            Specifies whether to output Cluster Assignment information.
            Default Value: False
            Types: bool
        initialcentroids_method:
            Optional Argument.
            Specifies the initialization method to be used for selecting initial set of centroids.
            Permitted Values: 'RANDOM', 'KMEANS++'
            Default Value: 'RANDOM'
            Note:
                * This argument is not required if "centroids_data" is provided.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of KMeans.
        Output teradataml DataFrames can be accessed using attribute
        references, such as KMeansObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. model_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("kmeans", "computers_train1")
        load_example_data("kmeans",'kmeans_table')
        # Create teradataml DataFrame objects.
        computers_train1 = DataFrame.from_table("computers_train1")
        kmeans_tab = DataFrame('kmeans_table')
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Grouping a set of observations into 2 clusters in which
        #             each observation belongs to the cluster with the nearest mean.
        KMeans_out = KMeans(id_column="id",
                            target_columns=['price', 'speed'],
                            data=computers_train1,
                            num_clusters=2)
        # Print the result DataFrames.
        print(KMeans_out.result)
        print(KMeans_out.model_data)
        # Example 2 : Grouping a set of observations by specifying initial
        #             centroid data.
        # Get the set of initial centroids by accessing the group of rows
        # from input data.
        kmeans_initial_centroids_table = computers_train1.loc[[19, 97]]
        kmeans_initial_centroids = kmeans_tab.loc[[2, 4]]
        KMeans_out_1 = KMeans(id_column="id",
                              target_columns=['price', 'speed'],
                              data=computers_train1,
                              centroids_data=kmeans_initial_centroids_table)
        # Print the result DataFrames.
        print(KMeans_out_1.result)
        print(KMeans_out_1.model_data)
        # Example 3 : Grouping a set of observations into 2 clusters by
        #             specifying the number of clusters and seed value
        #             with output cluster assignment information.
        obj = KMeans(data=kmeans_tab,
             id_column='id',
             target_columns=['c1', 'c2'],
             threshold=0.0395,
             iter_max=3,
             centroids_data=kmeans_initial_centroids,
             output_cluster_assignment=True
            )
        # Print the result DataFrames.
        print(obj.result)
        # Example 4 : Grouping a set of observations into 3 clusters by
        #             specifying the number of clusters for initial centroids
        #             method as KMEANS++.
        obj = KMeans(data=kmeans_tab,
             id_column='id',
             target_columns=['c1', 'c2'],
             threshold=0.0395,
             iter_max=3,
             num_clusters=3,
             output_cluster_assignment=True,
             initialcentroids_method="KMEANS++"
            )
        # Print the result DataFrames.
        print(obj.result)
    KNN
    KNN
    Functions
    KNN(test_data=None, train_data=None, id_column=None, input_columns=None, model_type='classiﬁcation', k=5, accumulate=None, response_column=None,
    voting_weight=0, tolerance=1e-07, output_prob=False, output_responses=None, emit_neighbors=None, emit_distances=False, **generic_arguments)
    DESCRIPTION:
        The KNN() function classifies data objects based on proximity to other 
        data objects with known categories.
    PARAMETERS:
        test_data:
            Required Argument.
            Specifies the input teradataml DataFrame containing the test data.
            Types: teradataml DataFrame
        train_data:
            Required Argument.
            Specifies the teradataml DataFrame containing the train data.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the name of the column that uniquely identifies a data object 
            both in train and test data.
            Types: str
        input_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in train data that the 
            function uses to compute the distance between a test object and 
            the train objects. The test data must also have these columns.
            Types: str OR list of Strings (str)
        model_type:
            Optional Argument.
            Specifies the model type for KNN function.
            Default Value: "classification"
            Permitted Values: regression, classification, neighbors
            Types: str
        k:
            Optional Argument.
            Specifies the number of nearest neighbors to use in the algorithm.
            Any positive integer value > 0 and <= 1000 can be chosen.
            Default Value: 5
            Types: int
        accumulate:
            Optional Argument.
            Specifies the name(s) of the column(s) in test data
            to be copied to output.
            Types: str OR list of Strings (str)
        response_column:
            Optional Argument. Required when model type is regression or classification.
            Specifies the name of the train data column that contains the 
            numeric response variable values to be used for prediction in KNN 
            based regression or classification.
            Types: str
        voting_weight:
            Optional Argument.
            Specifies the voting weight of the train object for determining 
            the class of the test object as a function of the distance between 
            the train and test objects. The voting weight is calculated as w, 
            where w=1/POWER(distance, voting_weight) and distance is the distance 
            between the test object and the train object. Must be a 
            non-negative real number.
            Default Value: 0
            Types: float OR int
        tolerance:
            Optional Argument.
            Specifies the user to define the smallest distance to be considered. 
            When a non-zero voting weight is used, the case of zero distance 
            causes the weight (w=1/POWER(distance, voting_weight)) to be undefined.
            For any distance under the given tolerance, the weight is calculated as 
            w=1/POWER(tolerance, voting_weight).
            Default Value: 1.0E-7
            Types: float OR int
        output_prob:
            Optional Argument.
            Specifies whether the function should output the probability for each 
            response specified in "response_column". If "response_column" is not given, 
            outputs the probability of the predicted response.
            Default Value: False
            Types: bool
        output_responses:
            Optional Argument.
            Specifies the class labels for which to output probabilities.
            Types: str OR list of strs
        emit_neighbors:
            Optional Argument.
            Specifies whether the neighbors are to be emitted in the output.
            Default Value: False
            Types: bool
        emit_distances:
            Optional Argument.
            Specifies whether the neighbor distances are to be emitted in the output.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts bool
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of KNN.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as KNNObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("knn", ["computers_train1_clustered", "computers_test1"])
        # Create teradataml DataFrame objects.
        computers_test1 = DataFrame.from_table("computers_test1")
        computers_train1_clustered = DataFrame.from_table("computers_train1_clustered")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Generate fit object for column "computer_category".
        fit_obj = OneHotEncodingFit(data=computers_train1_clustered,
                                    is_input_dense=True,
                                    target_column="computer_category",
                                    categorical_values=["ultra", "special"],
                                    other_column="other")
        # Encode "ultra" and "special" values of column "computer_category".
        computers_train1_encoded = OneHotEncodingTransform(data=computers_train1_clustered,
                                                           object=fit_obj.result,
                                                           is_input_dense=True)
        # Example 1: Map the test computer data to "special" category.
        KNN_out = KNN(train_data = computers_train1_encoded.result,
                      test_data = computers_test1,
                      k = 50,
                      response_column = "computer_category_special",
                      id_column="id",
                      output_prob=False,
                      input_columns = ["price", "speed", "hd", "ram", "screen"],
                      voting_weight = 1.0,
                      emit_distances=False)
        # Print the result DataFrame.
        print(KNN_out.result)
        # Example 2: Get the distance of 10 nearest neighbours based on "price", "speed" and "hd".
        KNN_out = KNN(train_data = computers_train1_encoded.result,
                      test_data = computers_test1,
                      k=10,
                      model_type="neighbors",
                      id_column="id",
                      input_columns = ["price", "speed", "hd"],
                      emit_distances=True,
                      emit_neighbors=True)
        # Print the result DataFrame.
        print(KNN_out.result)
    NaiveBayes
    NaiveBayes
    Functions
    NaiveBayes(data=None, response_column=None, numeric_inputs=None, categorical_inputs=None, attribute_name_column=None, attribute_value_column=None,
    attribute_type=None, numeric_attributes=None, categorical_attributes=None, **generic_arguments)
    DESCRIPTION:
        Function generates classification model using NaiveBayes 
        algorithm.
        The Naive Bayes classification algorithm uses a training dataset with known discrete outcomes
        and either discrete or continuous numeric input variables, along with categorical variables, to generate a model.
        This model can then be used to predict the outcomes of future observations based on their input variable values.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame .
            Types: teradataml DataFrame
        response_column:
            Required Argument.
            Specifies the name of the column in "data" containing response values.
            Types: str
        numeric_inputs:
            Optional Argument.
            Specifies the names of the columns in "data" containing numeric attributes values.
            Types: str OR list of Strings (str)
        categorical_inputs:
            Optional Argument.
            Specifies the names of the columns in "data" containing categorical attributes values.
            Types: str OR list of Strings (str)
        attribute_name_column:
            Optional Argument.
            Specifies the names of the columns in "data" containing attributes names.
            Types: str
        attribute_value_column:
            Optional Argument.
            Specifies the names of the columns in "data" containing attributes values.
            Types: str
        attribute_type:
            Optional Argument, Required if "data" is in sparse format and
            both "numeric_attributes" and "categorical_attributes" are not provided.
            Specifies the attribute type. 
            Permitted Values: 
                * ALLNUMERIC - if all the attributes are of numeric type.
                * ALLCATEGORICAL - if all the attributes are of categorical type.
            Types: str
        numeric_attributes:
            Optional Argument.
            Specifies the numeric attributes names.
            Types: str OR list of strs
        categorical_attributes:
            Optional Argument.
            Specifies the categorical attributes names.
            Types: str OR list of strs
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of  NaiveBayes.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as  NaiveBayesObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the 
        #        function in user space.
        #     2. User can import the function, if it is available on 
        #        Vantage user is connected to.
        #     3. To check the list of analytic functions available on 
        #        Vantage user connected to, use 
        #        "display_analytic_functions()".
        # Load the example data.
        load_example_data("decisionforestpredict", ["housing_train", "housing_test"])
        # Create teradataml DataFrame objects.
        housing_train = DataFrame.from_table("housing_train")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function  NaiveBayes.
        from teradataml import  NaiveBayes, Unpivoting
        # Example 1: NaiveBayes function to generate classification model using Dense input.
        NaiveBayes_out = NaiveBayes(data=housing_train, response_column='homestyle', 
                                    numeric_inputs=['price','lotsize','bedrooms','bathrms','stories','garagepl'], 
                                    categorical_inputs=['driveway','recroom','fullbase','gashw','airco','prefarea'])
        # Print the result DataFrame.
        print( NaiveBayes_out.result)
        # Example 2: NaiveBayes function to generate classification model using Sparse input.
        # Unpivoting the data for sparse input to naive bayes.
        upvt_data = Unpivoting(data = housing_train, id_column = 'sn',
                               target_columns = ['price','lotsize','bedrooms','bathrms','stories','garagepl','driveway',
                                                 'recroom','fullbase','gashw','airco','prefarea'],
                               attribute_column = "AttributeName", value_column = "AttributeValue",
                               accumulate = 'homestyle')
        NaiveBayes_out = NaiveBayes(data=upvt_data.result, 
                                    response_column='homestyle',
                                    attribute_name_column='AttributeName', 
                                    attribute_value_column='AttributeValue',
                                    numeric_attributes=['price','lotsize','bedrooms','bathrms','stories','garagepl'], 
                                    categorical_attributes=['driveway','recroom','fullbase','gashw','airco','prefarea'])
        # Print the result DataFrame.
        print( NaiveBayes_out.result)
    OneClassSVM
    OneClassSVM
    Functions
    OneClassSVM(data=None, input_columns=None, iter_max=300, batch_size=10, lambda1=0.02, alpha=0.15, iter_num_no_change=50, tolerance=0.001,
    intercept=True, learning_rate='OPTIMAL', initial_eta=0.05, decay_rate=0.25, decay_steps=5, momentum=0.0, nesterov=False, local_sgd_iterations=0,
    **generic_arguments)
    DESCRIPTION:
        The OneClassSVM() is a linear support vector machine (SVM) that performs
        classification analysis on datasets to identify outliers or novelty.
        This function supports these models:
            * Classification (loss: hinge). During the training, all the data is
              assumed to belong to a single class (value 1), therefore "response_column"
              is not needed by the model. For OneClassSVMPredict(), output values are 0
              or 1. A value of 0 corresponds to an outlier, and 1 to a normal observation/instance.
        OneClassSVM() is implemented using Minibatch Stochastic Gradient Descent (SGD) algorithm,
        which is highly scalable for large datasets.
        The function output is a trained one-class SVM model, which can be input to the
        OneClassSVMPredict() for prediction. The model also contains model statistics of MSE,
        Loglikelihood, AIC, and BIC.
        Notes:
            * The categorical columns should be converted to numerical columns as preprocessing
              step (for example, OneHotEncoding(), OrdinalEncoding()). OneClassSVM() takes all
              features as numeric input.
            * For a good model, dataset should be standardized before feeding to OneClassSVM()
              as a preprocessing step (for example, using ScaleFit() and ScaleTranform()).
            * The rows with missing values are ignored during training and prediction of
              OneClassSVM/OneClassSVMPredict. Consider filling up those rows using imputation
              (SimpleImputeFit() and SimpleImputeTrasform()) or other mechanism to train
              on rows with missing values.
            * The function supports linear SVMs only.
            * A maximum of 2046 features are supported due to the limitation imposed by the maximum
              number of columns (2048) in a database table for OneClassSVM().
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        input_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" to be used for
            training the model (predictors, features, or independent variables).
            Types: str OR list of Strings (str)
        iter_max:
            Optional Argument.
            Specifies the maximum number of iterations (mini-batches) over the
            training data batches.
             Note:
                * It must be a positive value less than 10,000,000.
            Default Value: 300
            Types: int
        batch_size:
            Optional Argument.
            Specifies the number of observations (training samples) processed in a
            single mini-batch per AMP. The value '0' indicates no mini-batches, the entire
            dataset is processed in each iteration, and the algorithm becomes Gradient
            Descent. A value higher than the number of rows on any AMP will also default
            to Gradient Descent.
            Notes:
                * It must be a non-negative integer value.
                * It must be in the range [0, 2147483647]
            Default Value: 10
            Types: int
        lambda1:
            Optional Argument.
            Specifies the amount of regularization to be added. The higher the
            value, the stronger the regularization. It is also used to compute
            the learning rate when the "learning_rate" is set to 'OPTIMAL'.
            A value of '0' means no regularization.
            Note:
                * It must be a non-negative float value.
            Default Value: 0.02
            Types: float OR int
        alpha:
            Optional Argument.
            Specifies the Elasticnet parameter for penalty computation. It only
            becomes effective when "lambda1" is greater than 0. The value represents
            the contribution ratio of L1 in the penalty. A value '1.0' indicates
            L1 (LASSO) only, a value '0' indicates L2 (Ridge) only, and a value
            in between is a combination of L1 and L2.
            Note:
                * It must be a float value between 0 and 1.
            Default Value: 0.15(15% L1, 85% L2)
            Types: float OR int
        iter_num_no_change:
            Optional Argument.
            Specifies the number of iterations (mini-batches) with no improvement in
            loss including the "tolerance" to stop training. A value of '0' indicates
            no early stopping and the algorithm continues until "iter_max"
            iterations are reached.
            Notes:
                * It must be a non-negative integer value.
                * It must be in the range [0, 2147483647]
            Default Value: 50
            Types: int
        tolerance:
            Optional Argument.
            Specifies the stopping criteria in terms of loss function improvement.
            Notes:
                * Applicable when "iter_num_no_change" is greater than '0'.
                * It must be a positive value.
            Default Value: 0.001
            Types: float OR int
        intercept:
            Optional Argument.
            Specifies whether "intercept" should be estimated or not based on
            whether "data" is already centered or not.
            Default Value: True
            Types: bool
        learning_rate:
            Optional Argument.
            Specifies the learning rate algorithm for SGD iterations.
            Permitted Values: CONSTANT, OPTIMAL, INVTIME, ADAPTIVE
            Default Value: OPTIMAL
            Types: str
        initial_eta:
            Optional Argument.
            Specifies the initial value of eta for the learning rate. When
            the "learning_rate" is 'CONSTANT', this value is applicable for
            all iterations.
            Default Value: 0.05
            Types: float OR int
        decay_rate:
            Optional Argument.
            Specifies the decay rate for the learning rate.
            Note:
                * Only applicable for 'INVTIME' and 'ADAPTIVE' learning rates.
            Default Value: 0.25
            Types: float OR int
        decay_steps:
            Optional Argument.
            Specifies the decay steps (number of iterations) for the 'ADAPTIVE'
            learning rate. The learning rate changes by decay rate after the
            specified number of iterations are completed.
            Note:
                * It must be in the range [0, 2147483647]
            Default Value: 5
            Types: int
        momentum:
            Optional Argument.
            Specifies the value to use for the momentum learning rate
            optimizer.A larger value indicates a higher momentum contribution.
            A value of '0' means the momentum optimizer is disabled. For a
            good momentum contribution, a value between 0.6-0.95 is recommended.
            Note:
                * It must be a non-negative float value between 0 and 1.
            Default Value: 0.0
            Types: float OR int
        nesterov:
            Optional Argument.
            Specifies whether Nesterov optimization should be applied to the
            momentum optimizer or not.
            Note:
                * Applicable when "momentum" is greater than 0.
            Default Value: False
            Types: bool
        local_sgd_iterations:
            Optional Argument.
            Specifies the number of local iterations to be used for Local SGD
            algorithm. A value of 0 implies Local SGD is disabled. A value higher
            than 0 enables Local SGD and that many local iterations are performed
            before updating the weights for the global model. With Local SGD algorithm,
            recommended values for arguments are as follows:
                * local_sgd_iterations: 10
                * iter_max:100
                * batch_size: 50
                * iter_num_no_change: 5
            Note:
                * It must be a positive integer value.
            Default Value: 0
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of OneClassSVM.
        Output teradataml DataFrames can be accessed using attribute
        references, such as OneClassSVMObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("larpredict", ["diabetes"])
        # Create teradataml DataFrame objects.
        data_input = DataFrame.from_table("diabetes")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 :  Train OneClassSVM model using "input_columns"
        #              which helps in identifying the input data whether
        #              it is normal or novelty when result of OneClassSVM
        #              is passed to OneClassSVMPredict.
        one_class_svm1=OneClassSVM(data=data_input,
                                   input_columns=['age', 'sex', 'bmi',
                                                  'map1', 'tc', 'ldl',
                                                  'hdl', 'tch', 'ltg',
                                                  'glu', 'y'],
                                   local_sgd_iterations=537,
                                   batch_size=1,
                                   learning_rate='CONSTANT',
                                   initial_eta=0.01,
                                   lambda1=0.1,
                                   alpha=0.0,
                                   momentum=0.0,
                                   iter_max=1
                                   )
        # Print the result DataFrame.
        print(one_class_svm1.result)
        print(one_class_svm1.output_data)
        # Example 2 :  Train OneClassSVM model using "input_columns",
        #              "learning_rate" set to 'ADAPTIVE', "momentum"
        #              set to '0.6' for better results.
        one_class_svm2=OneClassSVM(data=data_input,
                                   input_columns=['age', 'sex', 'bmi',
                                                  'map1', 'tc', 'ldl',
                                                  'hdl', 'tch', 'ltg',
                                                  'glu', 'y'],
                                   local_sgd_iterations=537,
                                   batch_size=1,
                                   learning_rate='ADAPTIVE',
                                   initial_eta=0.01,
                                   lambda1=0.1,
                                   alpha=0.0,
                                   momentum=0.6,
                                   iter_max=100
                                   )
        # Print the result DataFrame.
        print(one_class_svm2.result)
        print(one_class_svm2.output_data)
    SVM
    SVM
    Functions
    SVM(formula=None, data=None, input_columns=None, response_column=None, model_type='Classiﬁcation', iter_max=300, epsilon=0.1, batch_size=10,
    lambda1=0.02, alpha=0.15, iter_num_no_change=50, tolerance=0.001, intercept=True, class_weights='0:1.0, 1:1.0', learning_rate=None, initial_eta=0.05,
    decay_rate=0.25, decay_steps=5, momentum=0.0, nesterov=False, local_sgd_iterations=0, **generic_arguments)
    DESCRIPTION:
        The SVM() function is a linear support vector machine (SVM) that performs
        classification and regression analysis on data sets.
        This function supports these models:
            * Regression (loss: epsilon_insensitive).
            * Classification (loss: hinge). Only binary classification is supported. The
              only response values are 0 or 1.
        SVM() is implemented using Minibatch Stochastic Gradient Descent (SGD) algorithm,
        which is highly scalable for large datasets.
        Due to gradient-based learning, the function is highly sensitive to feature scaling.
        Before using the features in the function, you must standardize the Input features
        using ScaleFit() and ScaleTransform() functions. The function only accepts numeric
        features. Therefore, before training, you must convert the categorical features to
        numeric values. The function skips the rows with missing (null) values during training.
        The function output is a trained SVM model, which can be input to the SVMPredict()
        for prediction. The model also contains model statistics of mean squared error (MSE),
        Loglikelihood, Akaike information criterion (AIC), and Bayesian information criterion (BIC).
        Further model evaluation can be done as a post-processing step using functions such as
        RegressionEvaluator(), ClassificationEvaluator(), and ROC().
    PARAMETERS:
        formula:
            Required Argument when "input_columns" and "response_column" are not provided,
            optional otherwise.
            Specifies a string consisting of "formula" which is the model to be fitted.
            Only basic formula of the "col1 ~ col2 + col3 +..." form are
            supported and all variables must be from the same teradataml
            DataFrame object.
            Notes:
                * The function only accepts numeric features. User must convert the categorical
                  features to numeric values, before passing to the formula.
                * In case categorical features are passed to formula, those are ignored, and
                  only numeric features are considered.
                * Provide either "formula" argument or "input_columns" and "response_column" arguments.
            Types: str
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        input_columns:
            Required Argument when "formula" is not provided, optional otherwise.
            Specifies the name(s) of the column(s) in "data" to be used for
            training the model (predictors, features or independent variables).
            Note:
                * Provide either "formula" argument or "input_columns" and "response_column" arguments.
            Types: str OR list of Strings (str)
        response_column:
            Required Argument when "formula" is not provided, optional otherwise.
            Specifies the name of the column that contains the class label for
            classification or target value (dependent variable) for regression.
            Note:
                * Provide either "formula" argument or "input_columns" and "response_column" arguments.
            Types: str
        model_type:
            Optional Argument.
            Specifies the type of the analysis.
            Permitted Values: Regression, Classification
            Default Value: 'Classification'
            Types: str
        iter_max:
            Optional Argument.
            Specifies the maximum number of iterations (mini-batches) over the
            training data batches.
            Note:
                * It must be a positive value less than 10,000,000.
            Default Value: 300
            Types: int
        epsilon:
            Optional Argument.
            Specifies the epsilon threshold for 'Regression' (the value of epsilon
            for epsilon_insensitive loss). Any difference between the current prediction
            and the correct label is ignored within this threshold.
            Default Value: 0.1
            Types: float OR int
        batch_size:
            Optional Argument.
            Specifies the number of observations (training samples) processed in a
            single mini-batch per AMP. The value '0' indicates no mini-batches, the entire
            dataset is processed in each iteration, and the algorithm becomes Gradient
            Descent. A value higher than the number of rows on any AMP will also default
            to Gradient Descent.
            Note:
                * It must be a non-negative integer value.
            Default Value: 10
            Types: int
        lambda1:
            Optional Argument.
            Specifies the amount of regularization to be added. The higher the
            value, the stronger the regularization. It is also used to compute
            the learning rate when the "learning_rate" is set to 'OPTIMAL'.
            A value of '0' means no regularization.
            Note:
                * It must be a non-negative float value.
            Default Value: 0.02
            Types: float OR int
        alpha:
            Optional Argument.
            Specifies the Elasticnet parameter for penalty computation. It only
            becomes effective when "lambda1" greater than 0. The value represents
            the contribution ratio of L1 in the penalty. A value '1.0' indicates
            L1 (LASSO) only, a value '0' indicates L2 (Ridge) only, and a value
            in between is a combination of L1 and L2.
            Note:
                * It must be a float value between 0 and 1.
            Default Value: 0.15(15% L1, 85% L2)
            Types: float OR int
        iter_num_no_change:
            Optional Argument.
            Specifies the number of iterations (mini-batches) with no improvement in
            loss including the tolerance to stop training. A value of '0' indicates
            no early stopping and the algorithm continues until "iter_max"
            iterations are reached.
            Note:
                * It must be a non-negative integer value.
            Default Value: 50
            Types: int
        tolerance:
            Optional Argument.
            Specifies the stopping criteria in terms of loss function improvement.
            Notes:
                * Applicable when "iter_num_no_change" is greater than '0'.
                * It must be a positive value.
            Default Value: 0.001
            Types: float OR int
        intercept:
            Optional Argument.
            Specifies whether intercept should be estimated or not based on
            whether "data" is already centered or not.
            Default Value: True
            Types: bool
        class_weights:
            Optional Argument.
            Specifies the weights associated with classes. If the weight of a class is omitted,
            it is assumed to be 1.0.
            Note:
                * Only applicable when "model_type" is set to 'Classification' . The format is
                  '0:weight,1:weight'. For example, '0:1.0,1:0.5' will give twice the
                  weight to each observation in class 0.
            Default Value: "0:1.0, 1:1.0"
            Types: str
        learning_rate:
            Optional Argument.
            Specifies the learning rate algorithm for SGD iterations.
            Permitted Values: CONSTANT, OPTIMAL, INVTIME, ADAPTIVE
            Default Value:
                * 'INVTIME' when "model_type" is set to 'Regression'
                * 'OPTIMAL' when "model_type" is set to 'Classification'
            Types: str
        initial_eta:
            Optional Argument.
            Specifies the initial value of eta for the learning rate. When
            the "learning_rate" is 'CONSTANT', this value is applicable for
            all iterations.
            Default Value: 0.05
            Types: float OR int
        decay_rate:
            Optional Argument.
            Specifies the decay rate for the learning rate.
            Note:
                * Only applicable for 'INVTIME' and 'ADAPTIVE' learning rates.
            Default Value: 0.25
            Types: float OR int
        decay_steps:
            Optional Argument.
            Specifies the decay steps (number of iterations) for the 'ADAPTIVE'
            learning rate. The learning rate changes by decay rate after the
            specified number of iterations are completed.
            Default Value: 5
            Types: int
        momentum:
            Optional Argument.
            Specifies the value to use for the momentum learning rate optimizer.
            A larger value indicates a higher momentum contribution.
            A value of 0 means the momentum optimizer is disabled.  For a
            good momentum contribution, a value between 0.6-0.95 is recommended.
            Note:
                * It must be a non-negative float value between 0 and 1.
            Default Value: 0.0
            Types: float OR int
        nesterov:
            Optional Argument.
            Specifies whether Nesterov optimization should be applied to the
            momentum optimizer or not.
            Note:
                * Applicable when "momentum" is greater than 0.
            Default Value: False
            Types: bool
        local_sgd_iterations:
            Optional Argument.
            Specifies the number of local iterations to be used for Local SGD
            algorithm. A value of 0 implies that Local SGD is disabled. A value higher
            than 0 enables Local SGD and that many local iterations are performed
            before updating the weights for the global model. With Local SGD algorithm,
            recommended values for this argument are as follows:
                * local_sgd_iterations: 10
                * iter_max:100
                * batch_size: 50
                * iter_num_no_change: 5
            Note:
                * It must be a positive integer value.
            Default Value: 0
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of SVM.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SVMObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["cal_housing_ex_raw"])
        # Create teradataml DataFrame objects.
        data_input = DataFrame.from_table("cal_housing_ex_raw")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Scale "target_columns" with respect to 'STD' value of the column.
        fit_obj = ScaleFit(data=data_input,
                           target_columns=['MedInc', 'HouseAge', 'AveRooms',
                                           'AveBedrms', 'Population', 'AveOccup',
                                           'Latitude', 'Longitude'],
                           scale_method="STD")
        # Transform the data.
        transform_obj = ScaleTransform(data=data_input,
                                       object=fit_obj.output,
                                       accumulate=["id", "MedHouseVal"])
        # Example 1 : Train the transformed data using SVM when "model_type" is 'Regression'
        #             and default values provided.
        obj1 = SVM(data=transform_obj.result,
                  input_columns=['MedInc', 'HouseAge', 'AveRooms',
                                 'AveBedrms', 'Population', 'AveOccup',
                                 'Latitude', 'Longitude'],
                  response_column="MedHouseVal",
                  model_type="Regression"
                  )
        # Print the result DataFrame.
        print(obj1.result)
        print(obj1.output_data)
        # Example 2 : Train the transformed data using SVM when "model_type" is 'Classification'
        #             when "learning_rate" is 'INV_TIME'.
        obj2 = SVM(data=transform_obj.result,
                  input_columns=['MedInc', 'HouseAge', 'AveRooms',
                                 'AveBedrms', 'Population', 'AveOccup',
                                 'Latitude', 'Longitude'],
                  response_column="MedHouseVal",
                  model_type="Classification",
                  batch_size=12,
                  iter_max=301,
                  lambda1=0.1,
                  alpha=0.5,
                  iter_num_no_change=60,
                  tolerance=0.01,
                  intercept=False,
                  class_weights="0:1.0,1:0.5",
                  learning_rate="INVTIME",
                  initial_data=0.5,
                  decay_rate=0.5,
                  momentum=0.6,
                  nesterov=True,
                  local_sgd_iterations=1,
                  )
        # Print the result DataFrame.
        print(obj2.result)
        print(obj2.output_data)
        # Example 3 : Generate linear support vector machine(SVM) when "learning_rate"
        #             is 'ADAPTIVE' and "class_weight" is '0:1.0,1:0.5'.
        obj3 = SVM(data=transform_obj.result,
                  input_columns=['MedInc', 'HouseAge', 'AveRooms',
                                 'AveBedrms', 'Population', 'AveOccup',
                                 'Latitude', 'Longitude'],
                  response_column="MedHouseVal",
                  model_type="Classification",
                  batch_size=1,
                  iter_max=1,
                  lambda1=0.0,
                  iter_num_no_change=60,
                  tolerance=0.01,
                  intercept=False,
                  class_weights="0:1.0,1:0.5",
                  learning_rate="ADAPTIVE",
                  initial_data=0.1,
                  decay_rate=0.5,
                  momentum=0.7,
                  nesterov=True,
                  local_sgd_iterations=1,
                  )
        # Print the result DataFrame.
        print(obj3.result)
        print(obj3.output_data)
        # Example 4 : Generate linear support vector machine(SVM) when "decay_rate" is 0.5
        #             and "model_type" is 'regression'.
        obj4 = SVM(data=transform_obj.result,
                  input_columns=['MedInc', 'HouseAge', 'AveRooms',
                                 'AveBedrms', 'Population'],
                  response_column="MedHouseVal",
                  model_type="Regression",
                  decay_rate=0.5,
                  momentum=0.7,
                  nesterov=True,
                  local_sgd_iterations=1,
                  )
        # Print the result DataFrame.
        print(obj4.result)
        print(obj4.output_data)
        # Example 5 : Generate linear support vector machine(SVM) using
        #             input dataframe and provided formula and "model_type" is 'regression'.
        formula = "MedHouseVal~MedInc + HouseAge + AveRooms + AveBedrms + Population + AveOccup + Latitude + Longitude"
        obj5 = SVM(data=transform_obj.result,
                  formula=formula,
                  model_type="Regression"
                  )
        # Print the result DataFrame.
        print(obj5.result)
        print(obj5.output_data)
    VectorDistance
    VectorDistance
    Functions
    VectorDistance(target_data=None, reference_data=None, target_id_column=None, target_feature_columns=None, ref_id_column=None,
    ref_feature_columns=None, distance_measure='COSINE', topk=10, **generic_arguments)
    DESCRIPTION:
        The VectorDistance() function accepts a dataframe of target vectors and a dataframe of reference vectors and
        returns output containing the distance between target-reference pairs.
        The function computes the distance between the target pair and the reference pair from the same input
        if you provide only one input.
        You must have the same column order in the "target_feature_columns" argument and the "ref_feature_columns"
        argument. The function ignores the feature values during distance computation if the value is either
        None, NAN, or INF.
        The function returns N*N output if you use the "topk" value as -1 because the function includes
        all reference vectors in the output.
        Notes:
            * The algorithm used in this function is of the order of N*N (where N is the number of rows).
                Hence, expect the function to run significantly longer as the number of rows increases in either
                the "target_data" or the "reference_data".
            * Because the reference data is copied to the spool for each AMP before running the query, the user
                spool limits the size and scalability of the input.
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
                For information about PTCs, see Teradata Vantage™ - Analytics Database International Character Set
                Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        target_data:
            Required Argument.
            Specifies the teradataml DataFrame containing target data vectors.
            Types: teradataml DataFrame
        reference_data:
            Optional Argument.
            Specifies the teradataml DataFrame containing reference data vectors.
            Types: teradataml DataFrame
        target_id_column:
            Required Argument.
            Specifies the name of the "target_data" column that contains
            identifiers of the target data vectors.
            Types: str
        target_feature_columns:
            Required Argument.
            Specifies the names of the "target_data" columns that contain features
            of the target data vectors.
            Note:
                You can specify up to 2018 feature columns.
            Types: str OR list of Strings (str)
        ref_id_column:
            Optional Argument.
            Specifies the name of the "reference_data" column that contains
            identifiers of the reference data vectors.
            Types: str
        ref_feature_columns:
            Optional Argument.
            Specifies the names of the "reference_data" columns that contain
            features of the reference data vectors.
            Note:
                You can specify up to 2018 feature columns.
            Types: str OR list of Strings (str)
        distance_measure:
            Optional Argument.
            Specifies the distance type to compute between the target and the reference vector.
            Default Value: "COSINE"
            Permitted Values:
                * Cosine: Cosine distance between the target vector and the reference vector.
                * Euclidean: Euclidean distance between the target vector and the reference vector.
                * Manhattan: Manhattan distance between the target vector and the reference vector.
            Types: str OR list of strs
        topk:
            Optional Argument.
            Specifies the maximum number of closest reference vectors to include in the output table
            for each target vector. The value k is an integer between 1 and 100.
            Default Value: 10
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQLE Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of VectorDistance.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as VectorDistanceObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("vectordistance", ["target_mobile_data_dense", "ref_mobile_data_dense"])
        # Create teradataml DataFrame objects.
        target_mobile_data_dense=DataFrame("target_mobile_data_dense")
        ref_mobile_data_dense=DataFrame("ref_mobile_data_dense")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Compute the cosine, euclidean, manhattan distance between the target and reference vectors.
        VectorDistance_out = VectorDistance(target_id_column="userid",
                                            target_feature_columns=['CallDuration', 'DataCounter', 'SMS'],
                                            ref_id_column="userid",
                                            ref_feature_columns=['CallDuration', 'DataCounter', 'SMS'],
                                            distance_measure=['Cosine', 'Euclidean', 'Manhattan'],
                                            topk=2,
                                            target_data=target_mobile_data_dense,
                                            reference_data=ref_mobile_data_dense)
        # Print the result DataFrame.
        print(VectorDistance_out.result)
    XGBoost
    XGBoost
    Functions
    XGBoost(formula=None, data=None, input_columns=None, response_column=None, max_depth=5, num_boosted_trees=-1, min_node_size=1, seed=1,
    model_type='REGRESSION', coverage_factor=1.0, min_impurity=0.0, lambda1=1, shrinkage_factor=0.5, column_sampling=1.0, iter_num=10, tree_size=-1,
    base_score=0.0, **generic_arguments)
    DESCRIPTION:
        The XGBoost() function, also known as eXtreme Gradient Boosting, is an implementation
        of the gradient boosted decision tree algorithm designed for speed and performance.
        It has recently been dominating applied machine learning.
        In gradient boosting, each iteration fits a model to the residuals (errors) of the
        previous iteration to correct the errors made by existing models. The predicted
        residual is multiplied by this learning rate and then added to the previous
        prediction. Models are added sequentially until no further improvements can be made.
        It is called gradient boosting because it uses a gradient descent algorithm to minimize
        the loss when adding new models.
        Gradient boosting involves three elements:
            * A loss function to be optimized.
            * A weak learner to make predictions.
            * An additive model to add weak learners to minimize the loss function.
        The loss function used depends on the type of problem being solved. For example, regression
        may use a squared error and binary classification may use binomial. A benefit of the gradient
        boosting is that a new boosting algorithm does not have to be derived for each loss function.
        Instead, it provides a generic enough framework that any differentiable loss function can be
        used. The XGBoost() function supports both regression and classification predictive
        modeling problems. The model that it creates is used in the XGBoostPredict() function
        for making predictions.
        The XGBoost() function supports the following features.
            * Regression
            * Multiple-Class and binary classification
        Notes:
            * When a dataset is small, best practice is to distribute the data to one AMP.
              To do this, create an identifier column as a primary index, and use the same
              value for each row.
            * For Classification (softmax), a maximum of 500 classes are supported.
            * For Classification, while the creating DataFrame for the function input,
              the DataFrame column must have a deterministic output. Otherwise, the function
              may not run successfully or return the correct output.
            * The processing time is controlled by (proportional to):
                * The number of boosted trees (controlled by "num_boosted_trees", "tree_size",
                  and "coverage_factor").
                * The number of iterations (sub-trees) in each boosted tree (controlled by "iternum").
                * The complexity of an iteration (controlled by "max_depth", "min_nod_size",
                  "column_sampling", "min_impurity").
              A careful choice of these parameters can be used to control the processing time.
              For example, changing "coverage_factor" from 1.0 to 2.0 doubles the number of boosted trees,
              which as a result, doubles the execution time roughly.
    PARAMETERS:
        formula:
            Required Argument when "input_columns" and "response_column" are not provided,
            optional otherwise.
            Specifies a string consisting of "formula" which is the model to be fitted.
            Only basic formula of the "col1 ~ col2 + col3 +..." form are
            supported and all variables must be from the same teradataml
            DataFrame object.
            Notes:
                * The function only accepts numeric features. User must convert the categorical
                  features to numeric values, before passing to the formula.
                * In case categorical features are passed to formula, those are ignored, and
                  only numeric features are considered.
                * Provide either "formula" argument or "input_columns" and "response_column" arguments.
            Types: str
        data:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
            Types: teradataml DataFrame
        input_columns:
            Required Argument when "formula" is not provided, optional otherwise.
            Specifies the name(s) of the teradataml DataFrame column(s) that need to be used
            for training the model (predictors, features, or independent variables).
            Note:
                * Input column names with double quotation marks are not allowed for this function.
            Types: str OR list of Strings (str)
        response_column:
            Required Argument when "formula" is not provided, optional otherwise.
            Specifies the name of the column that contains the class label for
            classification or target value (dependent variable) for regression.
            Types: str
        model_type:
            Optional Argument.
            Specifies whether the analysis is a regression (continuous response variable) or
            a multiple-class classification (predicting result from the number of classes).
            Default Value: Regression
            Permitted Values:
                * Regression
                * Classification
            Types: str
        max_depth:
            Optional Argument.
            Specifies a decision tree stopping criterion. If the tree reaches a
            depth past this value, the algorithm stops looking for splits.
            Decision trees can grow to (2^(max_depth+1)-1) nodes. This stopping
            criterion has the greatest effect on the performance of the function.
            The maximum value is 2147483647.
            Note:
                * The "max_depth" must be in the range [1, 2147483647].
            Default Value: 5
            Types: int
        num_boosted_trees:
            Optional Argument.
            Specifies the number of parallels boosted trees. Each boosted tree operates on
            a sample of data that fits in an AMP memory. By default, it is chosen equal
            to the number of AMPs with data. If "num_boosted_trees" is greater than
            the number of AMPs with data, each boosting operates on a sample of the input data,
            and the function estimates sample size (number of rows) using this formula:
            sample_size = total_number_of_input_rows / number_of_trees
            The sample_size must fit in an AMP memory. It always uses the sample size
            (or tree size) that fits in an AMP memory to build tree models and ignores
            those rows cannot fit in memory. A higher "num_boosted_trees" value may improve
            function run time but may decrease prediction accuracy.
            Note:
                 * The "num_boosted_trees" must be in the range [-1, 10000]
            Default Value: -1
            Types: int
        min_node_size:
             Optional Argument.
             Specifies a decision tree stopping criterion, which is the minimum size of any
             node within each decision tree.
             Note:
                 * The "min_node_size" must be in the range [1, 2147483647].
             Default Value: 1
             Types: int
        seed:
            Optional Argument.
            Specifies an integer value to use in determining the random seed for
            column sampling.
            Note:
                * The "seed" must be in the range [-2147483648, 2147483647].
            Default Value: 1
            Types: int
        coverage_factor:
            Optional Argument.
            Specifies the level of coverage for the dataset while boosting trees
            (in percentage, for example, 1.25 = 125% coverage). "coverage_factor"
            can only be used if "num_boosted_trees" is not supplied. When "num_boosted_trees"
            is specified, coverage depends on the value of "num_boosted_trees".
            If "num_boosted_trees" is not specified, "num_boosted_trees" is chosen to achieve
            this level of coverage specified by "coverage_factor".
            Note:
                * The "seed" must be in the range (0, 10.0].
            Default Value: 1.0
            Types: float OR int
        min_impurity:
            Optional Argument.
            Specifies the minimum impurity at which the tree stops splitting further down.
            For regression, a criteria of squared error is used, whereas for classification,
            gini impurity is used.
            Note:
                * The "min_impurity" must be in the range [0.0, 1.79769313486231570815e+308].
            Default Value: 0.0
            Types: float OR int
        lambda1:
            Optional Argument.
            Specifies the L2 regularization that the loss function uses while boosting trees.
            The higher the lambda, the stronger the regularization effect.
            Notes:
                * The "lambda1" must be in the range [0, 100000].
                * The value 0 specifies no regularization.
            Default Value: 1
            Types: float OR int
        shrinkage_factor:
            Optional Argument.
            Specifies the learning rate (weight) of a learned tree in each boosting step.
            After each boosting step, the algorithm multiplies the learner by shrinkage to
            factor make the boosting process more conservative.
            Notes:
                * The "shrinkage_factor" is a DOUBLE PRECISION value in the range (0, 1].
                * The value 1 specifies no shrinkage.
            Default Value: 0.5
            Types: float
        column_sampling:
            Optional Argument.
            Specifies the fraction of features to sample during boosting.
            Note:
                * The "column_sampling" must be in the range (0, 1].
            Default Value: 1.0
            Types: float
        iter_num:
            Optional Argument.
            Specifies the number of iterations (rounds) to boost the weak classifiers.
            Note:
                * The "iter_num" must be in the range [1, 100000].
            Default Value: 10
            Types: int
        tree_size:
            Optional Argument.
            Specifies the number of rows that each tree uses as its input data set.
            The function builds a tree using either the smaller of the number of rows
            on an AMP and the number of rows that fit into the AMP memory,
            or the number of rows given by the "tree_size" argument. By default,
            this argument takes the smaller of the number of rows on an AMP and
            the number of rows that fit into the AMP memory.
            Note:
                * The "tree_size" must be in the range [-1, 2147483647].
            Default Value: -1
            Types: int
        base_score:
            Optional Argument.
            Specifies the initial prediction value for all data points.
            Note:
                * The "base_score" must be in the range [-1e50, 1e50].
            Default Value: 0.0
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQLE Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of XGBoost.
        Output teradataml DataFrames can be accessed using attribute
        references, such as XGBoostObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
            2. output_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "titanic")
        load_example_data("byom", "iris_input")
        # Create teradataml DataFrame objects.
        titanic = DataFrame.from_table("titanic")
        iris_input = DataFrame("iris_input")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Train the model using features 'age', 'survived' and 'pclass'
        #            whereas target value as 'fare'.
        XGBoost_out_1 = XGBoost(data=titanic,
                                input_columns=["age", "survived", "pclass"],
                                response_column = 'fare',
                                max_depth=3,
                                lambda1 = 1000.0,
                                model_type='Regression',
                                seed=-1,
                                shrinkage_factor=0.1,
                                iter_num=2)
        # Print the result DataFrame.
        print(XGBoost_out_1.result)
        print(XGBoost_out_1.output_data)
        # Example 2: Improve the function run time by specifying "num_boosted_trees"
        #            value greater than the number of AMPs.
        XGBoost_out_2 = XGBoost(data=titanic,
                                input_columns=["age", "survived", "pclass"],
                                response_column = 'fare',
                                max_depth=3,
                                lambda1 = 1000.0,
                                model_type='Regression',
                                seed=-1,
                                shrinkage_factor=0.1,
                                num_boosted_tres=10,
                                iter_num=2)
        # Print the result DataFrame.
        print(XGBoost_out_2.result)
        print(XGBoost_out_2.output_data)
        # Example 3: Train the model using titanic input and provided the "formula".
        formula = "fare ~ age + survived + pclass"
        XGBoost_out_3 = XGBoost(data=titanic,
                                formula=formula,
                                max_depth=3,
                                lambda1 = 10000.0,
                                model_type='Regression',
                                seed=-1,
                                shrinkage_factor=0.1,
                                iter_num=2)
        # Print the result DataFrame.
        print(XGBoost_out_3.result)
        print(XGBoost_out_3.output_data)
        # Example 4: Train the model using features 'sepal_length', 'sepal_width',
        #            'petal_length', 'petal_width' whereas target value as 'species'
        #             and model type as 'Classification'.
        XGBoost_out_4 = XGBoost(data=iris_input,
                                input_columns=['sepal_length', 'sepal_width', 'petal_length', 'petal_width'],
                                response_column = 'species',
                                max_depth=3,
                                lambda1 = 10000.0,
                                model_type='Classification',
                                seed=-1,
                                shrinkage_factor=0.1,
                                iter_num=2)
        # Print the result DataFrame.
        print(XGBoost_out_4.result)
        print(XGBoost_out_4.output_data)
    FEATURE ENGINEERING UTILITY functions
    FillRowId
    FillRowId
    Functions
    FillRowId(data=None, row_id_column=None, **generic_arguments)
    DESCRIPTION:
        The FillRowId() function adds a column of unique row identifiers to the
        input DataFrame.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        row_id_column:
            Optional Argument.
            Specifies a name for the ID column in the output.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or
                    not. When set to True, results are persisted in a table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table
                    or not. When set to True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: boolean
                Function allows the user to partition, hash, order or local order the input
                data. These generic arguments are available for each argument that accepts
                teradataml DataFrame as input and can be accessed as:
                    * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                    * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                    * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of FillRowID.
        Output teradataml DataFrames can be accessed using attribute
        references, such as FillRowIDObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", "titanic")
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example: Add column "PassengerId" with unique row identifiers to the input
        #          teradataml DataFrame.
        obj = FillRowId(data=titanic_data,
                        row_id_column='PassengerId'
                       )
        # Print the result DataFrame.
        print(obj.result)
    NumApply
    NumApply
    Functions
    NumApply(data=None, target_columns=None, output_columns=None, accumulate=None, apply_method=None, sigmoid_style=None, in_place=None,
    **generic_arguments)
    DESCRIPTION:
        Apply predefined numeric operation on specified target columns.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" to perform numeric operations on.
            Types: str OR list of Strings (str)
        apply_method:
            Required Argument.
            Specifies the numeric operator/method.
            Permitted Values:
                * EXP - Raises e (base of natural logarithms) to power of value,
                        where e = 2.71828182845905.
                * LOG - Computes base 10 logarithm of value.
                * SIGMOID - Applies sigmoid function to value. See "sigmoid_style".
                * SININV - Computes inverse hyperbolic sine of value.
                * TANH - Computes hyperbolic tangent of value.
            Types: str
        in_place:
            Optional Argument.
            Specifies whether the output columns have the same names as the target columns.
            When set to True, function effectively replaces each value in each target column
            with the result of applying "apply_method" to it, otherwise copies the target
            columns to the output and adds output columns whose values are the result of
            applying "apply_method" to each value.
            No target columns can be part of the "accumulate" column.
            Default Value: True
            Types: boolean
        output_columns:
            Optional Argument.
            Specifies the name(s) of the output column(s) to be generated.
            An output column name cannot exceed 128 characters.
            By default, with "in_place" set to False, 'target_column_operator'; otherwise same as
            "target_column" names.
            Notes:
                1. If any 'target_column_operator' exceeds 128 characters, specify an "output_column" for each
                   target_column.
                2. Ignored with "in_place" set to True.
            Types: str OR list of strs
        accumulate:
            Optional Argument.
            Specifies the name(s) of input teradataml DataFrame column(s) to copy to the output.
            Types: str OR list of Strings (str)
        sigmoid_style:
            Optional Argument, required when "apply_method" is 'sigmoid'.
            Specifies the sigmoid style.
            Permitted Values:
                * LOGIT
                * MODIFIEDLOGIT
                * TANH
            Default Value: LOGIT
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of NumApply.
        Output teradataml DataFrames can be accessed using attribute
        references, such as NumApplyObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["numerics"])
        # Create teradataml DataFrame object.
        numerics = DataFrame.from_table("numerics")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Apply "log" method to column "num1" in numerics.
        obj = NumApply(data=numerics,
                       target_columns="integer_col",
                       apply_method="log",
                       in_place=True)
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Apply "sigmoid" method and "tanh" as sigmoid style.
        obj = NumApply(data=numerics,
                       target_columns="decimal_col",
                       output_columns="out1",
                       apply_method="sigmoid",
                       sigmoid_style="tanh",
                       in_place=False)
        # Print the result DataFrame.
        print(obj.result)
    RoundColumns
    RoundColumns
    Functions
    RoundColumns(data=None, target_columns=None, precision_digit=0, accumulate=None, **generic_arguments)
    DESCRIPTION:
        Function to round the values of each specified input DataFrame column to a
        specified number of decimal places.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" to round every value to
            precision digits.
            Types: str OR list of Strings (str)
        precision_digit:
            Optional Argument.
            Specifies the number of decimal places to which to round values for
            the "target_columns".
            If positive, the function rounds values to the right of the decimal point.
            If negative, the function rounds values to the left of the decimal point.
            If not provided, the function rounds the column values to 0 places.
            Note:
                If the column values have the DECIMAL/NUMERIC data type with a precision less
                than 38, then the function increases the precision by 1. For example, when a
                DECIMAL (4,2) value of 99.99 is rounded to 0 places, the function returns a
                DECIMAL (5,2) value, 100.00. However, if the precision is 38, then the function
                only reduces the scale value by 1 unless the scale is 0. For example, the
                function returns a DECIMAL (38, 36) value of 99.999999999 as a DECIMAL
                (38, 35) value, 100.00.
            Default Value: 0
            Types: int
        accumulate:
            Optional Argument.
            Specifies the names of input teradataml DataFrame columns to copy to the output.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying
                SQL Engine function supports, else an exception is raised.
    RETURNS:
        Instance of RoundColumns.
        Output teradataml DataFrames can be accessed using attribute
        references, such as RoundColumnsObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Rounding "fare" column to 2 decimal places.
        obj = RoundColumns(data=titanic_data,
                           target_columns="fare",
                           precision_digit=2,
                           accumulate=['pclass', 'sex'])
        # Print the result DataFrame.
        print(obj.result)
    StrApply
    StrApply
    Functions
    StrApply(data=None, target_columns=None, output_columns=None, accumulate=None, string_operation=None, operating_side=None, in_place=True,
    string=None, escape_string=None, is_case_speciﬁc=True, ignore_trailing_blank=False, string_length=None, start_index=None, **generic_arguments)
    DESCRIPTION:
        StrApply() function applies a specified string operator to the specified input DataFrame columns.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data"
            to perform string operations on.
            Types: str OR list of Strings (str)
        output_columns:
            Optional Argument.
            Specifies the output column names that contains the output of the
            string operation on the columns values.
            Types: str OR list Strings (str)
        accumulate:
            Optional Argument.
            Specifies the names of input teradataml DataFrame columns to copy to
            the output.
            Types: str OR list of Strings (str)
        string_operation:
            Required Argument.
            Specifies the string method to use on the target columns.
            Permitted Values:
                * TOUPPER
                * TOLOWER
                * strTRIM
                * strCON
                * strPAD
                * strLIKE
                * strINDEX
                * INITCAP
                * TRIMSPACES
                * CHARTOHEXINT
                * strREVERSE
                * SUBstr
                * GETNCHARS
                * UNICODEstr
            Types: str
        operating_side:
            Optional Argument.
            Specifies the operating side to consider while performing operations
            like 'stringPad', 'getNchars'.
            Permitted Values: LEFT, RIGHT
            Default Value: LEFT
            Types: str
        in_place:
            Optional Argument.
            Specifies whether to use the same column name for the resulted target
            columns.
            Default Value: True
            Types: bool
        string:
            Optional Argument.
            Specifies the names of the strings to use as input while applying
            string operation.
            Types: str OR list of Strings (str)
        escape_string:
            Optional Argument.
            Specifies the names of the string to use as an escape string while
            applying stringLike operation.
            Types: str OR list of Strings (str)
        is_case_specific:
            Optional Argument.
            Specifies whether to consider uppercase letters(e.g. "A") and lowercase
            letters(e.g. "a") as same or different.
            Default Value: True
            Types: bool
        ignore_trailing_blank:
            Optional Argument.
            Specifies whether to ignore trailing blanks in the column strings. Used
            if the stringOperation is 'StringLike' only.
            Default Value: False
            Types: bool
        string_length:
            Optional Argument.
            Specifies the length of a string.
            Types: int
        start_index:
            Optional Argument.
            Specifies the start index of the target column string. Used only if
            the string operation is 'SubString'.
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in a table or not.
                    When set to True, results are persisted in a table; otherwise, results
                    are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in a volatile table or not.
                    When set to True, results are stored in a volatile table, otherwise not.
                    Default Value: False
                    Types: boolean
            Function allows the user to partition, hash, order or local order the input
            data. These generic arguments are available for each argument that accepts
            teradataml DataFrame as input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if the underlying SQL Engine
                function supports it, else an exception is raised.
    RETURNS:
        Instance of StrApply.
        Output teradataml DataFrames can be accessed using attribute
        references, such as StrApplyObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Apply 'TOUPPER' string operator to the
        #            specified input "name" columns.
        obj = StrApply(data=titanic_data,
                       target_columns='name',
                       string_operation='TOUPPER',
                       in_place=False)
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Apply StrApply using all the arguments.
        obj = StrApply(data=titanic_data,
                       data_partition_column='age',
                       data_order_column='age',
                       target_columns='name',
                       accumulate='passenger',
                       output_columns='str_op_output',
                       string_operation='TOUPPER',
                       in_place=False,
                       persist=True)
        # Print the result DataFrame.
        print(obj.result)