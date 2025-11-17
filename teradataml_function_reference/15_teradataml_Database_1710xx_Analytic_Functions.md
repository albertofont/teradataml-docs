# teradataml Database 17.10.xx Analytic Functions

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
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
        attribution_out = teradataml.Attribution(data=attribution_sample_table1,
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
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
                    SQLE Engine function supports, else an exception is raised.
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
        # Example 1: Transform the data using BincodeFit object with Variable-Width.
        # Create BincodeFit object.
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
        # Run BincodeTransform.
        obj = BincodeTransform(data=titanic_data,
                               object=bin_code_1.output,
                               object_order_column="TD_MinValue_BINFIT",
                               accumulate=['passenger', 'ticket']
                              )
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Transform the data using BincodeFit object with Equal-Width.
        # Create BincodeFit object.
        bin_code_2 = BincodeFit(data=titanic_data,
                                target_columns='age',
                                method_type='Equal-Width',
                                nbins=2,
                                label_prefix='label_prefix'
                               )
        # Run BincodeTransform.
        obj = BincodeTransform(data=titanic_data,
                               object=bin_code_2.output,
                               accumulate=['passenger', 'ticket']
                              )
        # Print the result DataFrame.
        print(obj.result)
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
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
                    SQLE Engine function supports, else an exception is raised.
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
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
                    SQLE Engine function supports, else an exception is raised.
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
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
                    SQLE Engine function supports, else an exception is raised.
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
    ConvertTo
    ConvertTo
    Functions
    ConvertTo(data=None, target_columns=None, target_datatype=None, **generic_arguments)
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
            casted/converted to another data type.
            Types: str OR list of Strings (str)
        target_datatype:
            Required Argument.
            Specify target data type(s) into which "target_columns" need to be
            converted. If one value is provided, it applies to all "target_columns".
            If more than one value is specified, each "target_datatype" value applies to
            corresponding "target_columns" value (in the order specified by the user).
            Types: str OR list of strs
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
                Specifies whether to put the results of the function in volatile table
                or not. When set to True, results are stored in volatile table,
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
                SQLE Engine function supports, else an exception is raised.
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
        load_example_data("teradataml", "titanic")
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example: Convert datatype of 'fare' to integer.
        obj = ConvertTo(data=titanic_data,
                        target_columns="fare", target_datatype="integer"
                       )
        # Print the result DataFrame.
        print(obj.result)
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
        load_example_data("decisionforestpredict", ["housing_train","housing_test"])
        # Create teradataml DataFrame objects.
        housing_test = DataFrame.from_table("housing_test")
        housing_train = DataFrame.from_table("housing_train")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Train the data, i.e., create a decision forest Model with "tree_type" as classification.
        formula = "homestyle ~ driveway + recroom + fullbase + gashw + airco + prefarea + price + lotsize + bedrooms + bat
        rft_model = DecisionForest(data=housing_train,
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
                                   seed=100
                                   )
        # Run predict on the output of decision forest.
        decision_forest_predict_out = DecisionForestPredict(object = rft_model,
                                                            newdata = housing_test,
                                                            id_column = "sn",
                                                            detailed = False,
                                                            terms = ["homestyle"]
                                                            )
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
            Specifies the names of input_table columns to copy to the output
            teradataml DataFrame.
            Types: str OR list of Strings (str)
        output_responses:
            Optional Argument.
            Required if output_response_probdist is True.
            Specifies all responses in newdata.
            Types: str OR list of Strings (str)
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
        load_example_data("DecisionTreePredict", ["iris_attribute_train",
        "iris_response_train", "iris_attribute_test"])
        # Create teradataml DataFrame objects.
        iris_attribute_test = DataFrame.from_table("iris_attribute_test")
        iris_attribute_train = DataFrame.from_table("iris_attribute_train")
        iris_response_train = DataFrame.from_table("iris_response_train")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function DecisionTreePredict.
        from teradataml import DecisionTreePredict
        # Example 1: First train the data, i.e., create a decision tree Model and then
        #            run predict on the output of decision tree.
        td_decision_tree_out  = DecisionTree(attribute_name_columns = 'attribute',
                                             attribute_value_column = 'attrvalue',
                                             id_columns = 'pid',
                                             attribute_table = iris_attribute_train,
                                             response_table = iris_response_train,
                                             response_column = 'response',
                                             approx_splits = False,
                                             num_splits = 3,
                                             nodesize = 10,
                                             max_depth = 10,
                                             weighted = False,
                                             split_measure = "gini",
                                             output_response_probdist = False)
        # Run predict on the output of DecisionTree.
        decision_tree_predict_out = DecisionTreePredict(newdata=iris_attribute_test,
                                                        newdata_partition_column='pid',
                                                        object=td_decision_tree_out,
                                                        attr_table_groupby_columns='attribute',
                                                        attr_table_pid_columns='pid',
                                                        attr_table_val_column='attrvalue',
                                                        accumulate='pid',
                                                        output_response_probdist=False,
                                                        output_responses=['pid','attribute'])
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
    FillRowid
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
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
                SQLE Engine function supports, else an exception is raised.
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
    FTest
    FTest
    Functions
    FTest(data=None, alpha=None, ﬁrst_sample_variance=None, ﬁrst_sample_column=None, df1=None, second_sample_variance=None,
    second_sample_column=None, df2=2, alternate_hypothesis='two-tailed', **generic_arguments)
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
            Specifies the probability of rejecting the null hypothesis when it is true
            (value below which null hypothesis is rejected).
            "alpha" must be a numeric value in the range [0, 1].
            Default Value: 0.05
            Types: float
        first_sample_column:
            Required if "first_sample_variance" is omitted, disallowed otherwise.
            Specifies the name of the input column that contains the data for the
            first sample population.
            Types: str
        first_sample_variance:
            Required if "first_sample_column" is omitted, disallowed otherwise.
            Specifies the variance of the first sample population.
            Types: float
        df1:
            Required if "first_sample_column" is omitted, disallowed otherwise.
            Specifies the degrees of freedom of the first sample.
            Types: integer
        second_sample_column:
            Required if "second_sample_variance" is omitted, disallowed otherwise.
            Specifies the name of the input column that contains the data for the
            second sample population.
            Types: str
        second_sample_variance:
            Required if "second_sample_column" is omitted, disallowed otherwise.
            Specifies the variance of the second sample population.
            Types: float
        df2:
            Required if "second_sample_column" is omitted, disallowed otherwise.
            Specifies the degrees of freedom of the second sample.
            Types: integer
        alternate_hypothesis:
            Optional Argument.
            Specifies the alternative hypothesis.
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
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
                SQLE Engine function supports, else an exception is raised.
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
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
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
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
                    SQLE Engine function supports, else an exception is raised.
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
                    Specifies whether to persist the results of the function in table or
                    not. When set to True, results are persisted in table; otherwise,
                    results are garbage collected at the end of the session.
                    Default Value: False
                    Types: boolean
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the function in volatile table
                    or not. When set to True, results are stored in volatile table,
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
                SQLE Engine function supports, else an exception is raised.
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
        # Example 1: Run Fit() with all arguments and pass the output to Transform().
        fit_df = Fit(data=iris_input,
                     object=transformation_df,
                     object_order_column='TargetColumn'
                     )
        # Run Transform() with persist as True in order to save the result.
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
    GetRowsWithMissingValues
    GetRowsWithMissingValues
    Functions
    GetRowsWithMissingValues(data=None, target_columns=None, **generic_arguments)
    DESCRIPTION:
        Function displays the rows that have NULL values in the specified input data columns.
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
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
        # Example 1: Get the rows that contain NULL values in columns 'name', 'sex', 'age', 'passenger'.
        obj = GetRowsWithMissingValues(data=titanic_data,
                                       target_columns=['name', 'sex', 'age', 'passenger'])
        # Print the result DataFrame.
        print(obj.result)
    GetRowsWithoutMissingValues
    GetRowsWithoutMissingValues
    Functions
    GetRowsWithoutMissingValues(data=None, target_columns=None, **generic_arguments)
    DESCRIPTION:
        Function displays the rows that have non-NULL values in the specified input table columns.
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
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
        # Get the rows that contain non-NULL values in columns.
        obj = GetRowsWithoutMissingValues(data=titanic_data)
        # Print the result DataFrame.
        print(obj.result)
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
    Histogram
    Histogram
    Functions
    Histogram(data=None, object=None, target_columns=None, method_type=None, nbins=1, inclusion='LEFT', **generic_arguments)
    DESCRIPTION:
        Function calculates the frequency distribution of a data set using any of these methods:
            * Sturges
            * Scott
            * Variable-width
            * Equal-width
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        object:
            Optional Argument.
            Specifies the bin data, needed when "method_type" is set to 'EQUAL-WIDTH' or 'VARIABLE-WIDTH'.
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
                * EQUAL-WIDTH -
                    Requires "object" argument, which specifies the minimum value and the maximum
                    value of the bin in column1 and column2 respectively, and the label of the bin
                    in column3. Maximum number of bins cannot exceed 3500.
                * VARIABLE-WIDTH -
                    Requires "object" argument, which specifies the minimum value of the bins
                    in column1 and the maximum value of the bins in column2.
                    Algorithm for calculating bin width, w:
                    w = (max - min)/k
                    where:
                        min = minimum value of the bins
                        max = maximum value of the bins
                        k = number of intervals into which algorithm divides data set
                        Interval boundaries: min+w, min+2w, …, min+(k-1)w
            Types: str
        nbins:
            Optional Argument, Required when "method_type" is 'Variable-Width' and 'Equal-Width'.
            Specifies the number of bins.
            Default Value: 1
            Types: int
        inclusion:
            Optional Argument.
            Specifies whether points on bin boundaries should be included in the
            bin on the left or the right.
            Default Value: "LEFT"
            Permitted Values: LEFT, RIGHT
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
        # Example 1: Get the frequency distribution of a data set using 'sturges'
        #            method type for the values in column 'age'.
        obj = Histogram(data=titanic_data,
                        target_columns="age",
                        method_type="Sturges")
        # Print the result DataFrame.
        print(obj.result)
        # Example 2: Get the frequency distribution of a data set using 'variable-width'
        #            method type for the values in column 'age' with 3 number of bins.
        obj = Histogram(data=titanic_data,
                        object=min_max_object,
                        target_columns="age",
                        method_type="variable-width",
                        nbins=3)
        # Print the result DataFrame.
        print(obj.result)
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
    NaiveBayesTextClassifierPredict
    NaiveBayesTextClassiﬁerPredict
    Functions
    NaiveBayesTextClassiﬁerPredict(object=None, newdata=None, input_token_column=None, doc_id_columns=None, model_type='MULTINOMIAL', top_k=None,
    model_token_column=None, model_category_column=None, model_prob_column=None, output_prob=False, responses=None, accumulate=None,
    **generic_arguments)
    DESCRIPTION:
        The NaiveBayesTextClassifierPredict() function uses the model generated by the
        NaiveBayesTextClassifier() function to predict the outcomes for a test set
        of data.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame which contains the model
            data generated by the NaiveBayesTextClassifier() function or
            instance of NaiveBayesTextClassifier.
            Types: teradataml DataFrame or NaiveBayesTextClassifier
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
        # Create a model which is output of NaiveBayesTextClassifier.
        nbt_out = NaiveBayesTextClassifier(data = token_table,
                                           token_column = 'token',
                                           doc_id_columns = 'doc_id',
                                           doc_category_column = 'category',
                                           model_type = "Bernoulli",
                                           data_partition_column = 'category')
        # Example: Run NaiveBayesTextClassifierPredict() on model generated by
        #          NaiveBayesTextClassifier() where model_type is "Bernoulli".
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
            Specifies a character or string that separates words in the input text. The
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
            Specifies a string that specifies the punctuation characters for the function
            to remove before evaluating the input text.
            Default Value: "`~#^&*()-"
            Types: str
        reset:
            Optional Argument.
            Specifies a string that specifies the character or string that ends a sentence.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
    OneHotEncodingFit
    OneHotEncodingFit
    Functions
    OneHotEncodingFit(data=None, is_input_dense=None, target_column=None, categorical_values=None, other_column=None, attribute_column=None,
    value_column=None, target_attributes=None, other_attributes=None, **generic_arguments)
    DESCRIPTION:
        Function records all the parameters required for OneHotEncodingTransform() function.
        Such as, target attributes and their categorical values to be encoded and other parameters.
        Output of OneHotEncodingFit() function is used by OneHotEncodingTransform() function for encoding
        the input data. It supports inputs in both sparse and dense format.
        Note:
            * For input to be considered as sparse input, column names must be provided for
             'data_partition_column' argument.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        is_input_dense:
            Required Argument.
            Specifies whether input is in dense format or sparse format.
            Types: boolean
        target_column:
            Required Argument when 'is_input_dense=True', disallowed otherwise.
            Specifies the name of the column in "data" to be encoded.
            Types: str
        categorical_values:
            Required Argument when 'is_input_dense=True', disallowed otherwise.
            Specifies the categorical values to encode in one-hot form.
            Types: str OR list of strs
        other_column:
            Required Argument when 'is_input_dense=True', disallowed otherwise.
            Specifies a category name for values that "categorical_values" does not specify
            (categorical values not to encode in one-hot form).
            Default Value: 'other'
            Types: str
        attribute_column:
            Required Argument when 'is_input_dense=False', disallowed otherwise.
            Specifies the name of the column in "data" which contains attribute
            names.
            Types: str
        value_column:
            Required Argument when 'is_input_dense=False', disallowed otherwise.
            Specifies the name of the column in "data" which contains attribute
            values.
            Types: str
        target_attributes:
            Required Argument when 'is_input_dense=False', disallowed otherwise.
            Specifies one or more attributes to encode in one-hot form. Every target attribute must
            be in "attribute_column".
            Types: str OR list of strs
        other_attributes:
            Optional Argument when 'is_input_dense=False', disallowed otherwise.
            For each target attribute, specifies a category name for attributes that "target_attributes"
            does not specify. The nth "other_attributes" corresponds to the nth "target_attribute".
            Types: str OR list of strs
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
        load_example_data("teradataml", ["titanic"])
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1: Generate fit object to encode "male" and "female" values of column "sex".
        fit_obj = OneHotEncodingFit(data=titanic_data,
                                    is_input_dense=True,
                                    target_column="sex",
                                    categorical_values=["male", "female"],
                                    other_column="other")
        # Print the result DataFrame.
        print(fit_obj.result)
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
        obj = OneHotEncodingTransform(data=titanic_data,
                                      object=fit_obj.result,
                                      is_input_dense=True)
        # Print the result DataFrame.
        print(obj.result)
    OutlierFilterFit
    OutlierFilterFit
    Functions
    OutlierFilterFit(data=None, target_columns=None, group_columns=None, lower_percentile=None, upper_percentile=None, iqr_multiplier=1.5,
    outlier_method=None, replacement_value=None, remove_tail='BOTH', percentile_method=None, **generic_arguments)
    DESCRIPTION:
        The OutlierFilterFit() function calculates the lower percentile, upper percentile,
        count of rows and median for all the "target_columns" provided by the user.
        These metrics for each column help the function OutlierTransform() detect
        outliers in data. It stores parameters from arguments into an output used
        during transformation.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" for which to compute the metrics.
            Types: str OR list of Strings (str)
        lower_percentile:
            Required Argument.
            Specifies the lower range of percentile to be used.
            Types: float
        upper_percentile:
            Required Argument.
            Specifies the upper range of percentile to be used.
            Types: float
        outlier_method:
            Required Argument.
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
            Types: str
        replacement_value:
            Required Argument.
            Specifies the method to handle outliers.
            Permitted Values:
                * DELETE - Do not copy row to output DataFrame.
                * NULL - Copy row to output DataFrame, replacing each outlier with NULL.
                * MEDIAN - Copy row to output DataFrame, replacing each outlier with median
                           value for its group.
                * replacement value - Copy row to output DataFrame, replacing each outlier with
                                      a replacement value. Replacement value must be numeric.
            Types: str, int, float
        percentile_method:
            Required Argument.
            Specifies the teradata percentile methods to be used for calculating the upper
            and lower percentiles of the "target_columns".
            Permitted Values:
                * PERCENTILECONT - Considering continuous distribution.
                * PERCENTILEDISC - Considering discrete distibution.
            Types: str
        group_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) in "data" to group.
            Types: str
        iqr_multiplier:
            Optional Argument.
            Specifies the multiplier of interquartile range for 'TUKEY' filtering.
            Default Value: 1.5
            Types: float
        remove_tail:
            Optional Argument.
            Specifies the tail of the distribution to remove.
            Permitted Values:
                * LOWER - The lower tail.
                * UPPER - The upper tail.
                * BOTH - Both tails.
            Default Value: "BOTH"
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
    RETURNS:
        Instance of OutlierFilterFit.
        Output teradataml DataFrames can be accessed using attribute
        references, such as OutlierFilterFitObj.<attribute_name>.
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
        # Example 1: Generating fit object to find outlier values in column "fare".
        fit_obj = OutlierFilterFit(data=titanic_data,
                                   target_columns="fare",
                                   lower_percentile=0.1,
                                   upper_percentile=0.9,
                                   outlier_method="PERCENTILE",
                                   replacement_value="MEDIAN",
                                   percentile_method="PERCENTILECONT")
        # Print the result DataFrame.
        print(fit_obj.result)
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
        # Generate fit object for column "fare".
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
        obj = OutlierFilterTransform(data=titanic_data,
                                     object=fit_obj.result)
        # Print the result DataFrame.
        print(obj.result)
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
        # Generate feature matrix.
        obj = PolynomialFeaturesTransform(data=numerics,
                                          object=fit_obj.output)
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
        obj = RowNormalizeTransform(data=numerics,
                                    object=fit_obj.output,
                                    accumulate="id_col")
        # Print the result DataFrame.
        print(obj.result)
    ScaleFit
    ScaleFit
    Functions
    ScaleFit(data=None, target_columns=None, scale_method=None, miss_value='KEEP', global_scale=False, multiplier='1', intercept='0', **generic_arguments)
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
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
        # Create teradataml DataFrame.
        scaling_house = DataFrame.from_table("scale_housing")
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
    Scale Transform
    ScaleTransform
    Functions
    ScaleTransform(data=None, object=None, accumulate=None, **generic_arguments)
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
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
                These generic arguments are supported by teradataml if the underlying
                SQLE Engine function supports, else an exception is raised.
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
        # Create teradataml DataFrame.
        scaling_house = DataFrame.from_table("scale_housing")
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
        obj = ScaleTransform(data=scaling_house,
                             object=fit_obj.output,
                             accumulate="price")
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
        obj =  SimpleImputeTransform(data=titanic,
                                     object=fit_obj.output)
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
    ZTest
    ZTest
    Functions
    ZTest(data=None, alpha=0.5, ﬁrst_sample_column=None, second_sample_column=None, alternate_hypothesis='two-tailed', ﬁrst_sample_variance=None,
    second_sample_variance=None, mean_under_h0=None, **generic_arguments)
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
            Default Value: 0.5
            Types: float
        first_sample_column:
            Required Argument.
            Specifies the first sample column in z test.
            Types: str
        second_sample_column:
            Optional Argument.
            Specifies the second sample column in z test.
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
            Required Argument.
            Specifies the first sample variance.
            Types: float
        second_sample_variance:
            Optional Argument.
            Specifies the second sample variance.
            Types: float
        mean_under_h0:
            Optional Argument.
            Specifies the mean under the null hypothesis.
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept.
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
        # Create teradataml DataFrame object.
        titanic_data = DataFrame.from_table("titanic")
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