# teradataml Vantage Analytics Library Functions

Introduction to VALIB functions
Vantage Analytics Library provides the Data Scientists and other users with over 50 advanced
analytic functions built directly in the Advanced SQL Engine, which is a core capability of
Teradata Vantage. These functions support the entire data science process, including exploratory
data analysis, data preparation and feature engineering, hypothesis testing, as well as
statistical and machine learning model building and scoring.
The following are the pre-requisites for running VALIB functions through teradataml:
1. Install the Vantage Analytic Library in Teradata Vantage's Advanced SQL Engine. The library
   and readme file are available here for download.
2. In order to execute the VALIB functions related to Statistical Tests, the Statistical Test
   Metadata tables must be loaded into a database on the system to be analyzed. This can be done
   with the help of Vantage Analytic Library installer. The Statistical Test functions provide a
   parameter called "stats_database" that can be used to specify the database in which these
   tables are installed.
Once the setup is done, the user is ready to use Vantage Analytic Library functions from
teradataml. To execute Vantage Analytic Library functions,
1. Import "valib" object from teradataml as
   from teradataml import valib
2. Set 'configure.val_install_location' to the database name where Vantage Analytics Library
   functions are installed. For example,
   from teradataml import configure
   configure.val_install_location = "SYSLIB"
       # SYSLIB is the database name where Vantage Analytics Library functions are installed.
    3. Datasets used in the teradataml VALIB functions' examples are loaded with Vantage Analytics
       Library installer.
    Properties of VALIB function output object:
    1. All VALIB functions return an object of class <VALIB_function> (say valib_obj).
    2. The following are the attributes of the VALIB function object:
        a. The output teradataml DataFrames, which can be accessed as valib_obj.<output_df_name>.
           Details of the name(s) of the output DataFrame(s) can be found in Teradata Python
           Function Reference Guide for each individual function. The tables corresponding to
           output DataFrames are garbage collected at the end when the connection is closed.
           Users must use copy_to_sql() or DataFrame.to_sql() function to persist the output
           tables.
        b. Input arguments that are passed to the function. Users can access all input arguments as
           valib_obj.<input_argument_x>.
        c. show_query() function to print the underlying VALIB Stored Procedure call and can be
           accessed using valib_obj.show_query().
    Association Rules
    Association
    Association
    Functions
    Association(data, group_column=None, item_column=None, combinations=11, description_data=None, description_identiﬁer=None, description_column=None,
    group_count=None, hierarchy_data=None, low_level_column=None, high_level_column=None, left_lookup_data=None, left_lookup_column=None,
    right_lookup_data=None, right_lookup_column=None, min_conﬁdence=None, min_lift=None, min_support=None, min_zscore=None, order_prob=None,
    process_type=None, reduced_data=None, relaxed_order=None, sequence_column=None, ﬁlter=None, no_support_results=True,
    support_result_preﬁx='ml__valib_association', gen_sql_only=False, charset=None)
    DESCRIPTION:
        Association Rules provide various measures concerning items residing in groups. The
        measures, support, confidence, lift and Z Score, help to determine the likelihood that
        one or more items exist in a group, given that another one or more items exist in the
        same group. The classic example of this type of study is market basket analysis, in
        which the groups are shopping carts and the items are the products purchased in the
        shopping carts. An association rule might indicate the likelihood that a given shopping
        cart contains oranges, given that it also contains apples.
        Association rules consist of a left part and a right part. The left part consists of
        one or more items that are given to reside in a group, and the right part is the
        consequence that one or more items also reside in the given group. The measures are
        defined as follows:
            * Support-Percentage of groups containing the items on the left (left-side support),
              on the right (right-side support), or on both sides of a rule (rule support).
            * Confidence-Percentage of groups containing the left-side items that also contain
              the right-side items.
            * Lift-A measure of how much the probability is raised that the right-side items
              occur in a group given that the left-side items occur in the group.
            * Z Score-A statistical measure of how much the expected and actual values of the
              number of groups containing all the items in the rule varies. (Zero means expected
              and actual are the same.)
        A sequence analysis may be optionally requested, wherein there is a sequence of items
        defined by a "sequence_column" argument, ordering the items on each side of each rule,
        with left-side items preceding the rights-side items. An option is provided called
        "relaxed_order" that can be set to true so that items on the left side and the right
        side can be in any order provided that all left-side items precede all right-side items.
        An output teradataml DataFrame is created for each requested rule combination (1-to-1,
        2-to-1, and so on.).
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to perform Association analysis.
            Types: teradataml DataFrame
        group_column:
            Required Argument.
            Specifies the name of the column representing groups in the association rules.
            Types: str
        item_column:
            Required Argument.
            Specifies the name of the column representing items in the association rules.
            Types: str
        combinations:
            Optional Argument.
            Specifies the combinations of number of items on left side and number of items on
            right side of requested association rules. More than one combination can also be
            requested. For each combination specified, one output DataFrame is generated, i.e.,
            number of outputs DataFrames generated depends on the number of combinations.
            Corresponding output DataFrame is named as "result_{combination}".
            For example,
                combinations = [11, 21]
                above combinations produces an analysis of 1-to-1 and 2-to-1 rules. This will
                result in two output DataFrames 'result_11' and 'result_21'.
            Note:
                If you add the sizes of the left and right sides of a combination, the sum must
                be less than or equal to 5.
            Default Value: 11
            Types: int OR list of Integers (int)
        description_data:
            Optional Argument.
            Specifies the teradataml DataFrame containing description data, which is joined with
            the result.
            Note:
                Arguments "description_data", "description_identifier" and "description_column",
                if used, must be used together.
            Types: teradataml DataFrame
        description_identifier:
            Optional Argument.
            Specifies the name of the column in the "description_data" that is joined with the
            result.
            Note:
                Arguments "description_data", "description_identifier" and "description_column",
                if used, must be used together.
            Types: str
        description_column:
            Optional Argument.
            Specifies the name of the column in the "description_data" that contains descriptive
            item names.
            Note:
                Arguments "description_data", "description_identifier" and "description_column",
                if used, must be used together.
            Types: str
        group_count:
            Optional Argument.
            Specifies the count of the number of groups in the input data. By default it is
            calculated by the function. This is useful when you are processing a reduced input
            set saved in a previous run, so that calculations can be based on the number of
            groups in the original input set and not the reduced set.
            Types: int
        hierarchy_data:
            Optional Argument.
            Specifies the hierarchy data that can be joined with the input data in order to
            reduce the amount of input data and compute association rules at a different
            hierarchical level. Use of this argument has an impact on the data saved in the
            reduced input when the argument "reduced_data" is requested. When this option is
            utilized, listwise deletion is automatically performed, ignoring rows that contain
            a null group, item, or sequence column value.
            Note:
                Arguments "hierarchy_data", "low_level_column" and "high_level_column",
                if used, must be used together.
            Types: teradataml DataFrame
        low_level_column:
            Optional Argument.
            Specifies the lowest level item column in the "hierarchy_data" to be matched with
            the item column in the input data.
            Note:
                Arguments "hierarchy_data", "low_level_column" and "high_level_column",
                if used, must be used together.
            Types: str
        high_level_column:
            Optional Argument.
            Specifies the higher-level item column in the "hierarchy_data".
            Note:
                Arguments "hierarchy_data", "low_level_column" and "high_level_column",
                if used, must be used together.
            Types: str
        left_lookup_data:
            Optional Argument.
            Specifies a left-side lookup data that can be specified to reduce the rules
            reported to only those with left-side items that appear in the lookup dataset.
            Note:
                Arguments "left_lookup_data" and "left_lookup_column", if used, must be used
                together.
            Types: teradataml DataFrame
        left_lookup_column:
            Optional Argument.
            Specifies the name of the column to match with left-side items in rules.
            Note:
                Arguments "left_lookup_data" and "left_lookup_column", if used, must be used
                together.
            Types: str
        right_lookup_data:
            Optional Argument.
            Specifies a right-side lookup data that can be specified to reduce the rules
            reported to only those with right-side items that appear in the lookup dataset.
            Note:
                Arguments "right_lookup_data" and "right_lookup_column", if used, must be used
                together.
            Types: teradataml DataFrame
        right_lookup_column:
            Optional Argument.
            Specifies the name of the column to match with right-side items in rules.
            Note:
                Arguments "right_lookup_data" and "right_lookup_column", if used, must be used
                together.
            Types: str
        min_confidence:
            Optional Argument.
            Specifies the minimum value that the confidence measure of an association rule must
            have before it is included in a result. The range of valid values is 0 to 1 inclusive.
            Types: float
        min_lift:
            Optional Argument.
            Specifies the minimum value that the lift measure of an association rule must have
            before it is included in a result. The range of valid values is 0 to any positive
            numeric value.
            Types: float
        min_support:
            Optional Argument.
            Specifies the minimum value that the support measure of an association rule must
            have before it is included in a result. When this argument is utilized, the size
            of the input data is reduced, potentially impacting the use of the "reduced_data"
            argument. Use of this argument also causes listwise deletion to be performed,
            skipping any input rows that have a null group, item or sequence column value.
            The range of valid values is 0 to 1 inclusive.
            Types: float
        min_zscore:
            Optional Argument.
            Specifies the minimum value that the Z Score measure of an association rule must
            have before it is included in a result.
            Types: float
        order_prob:
            Optional Argument.
            Specifies the probability of correct ordering for sequential analysis. When sequence
            analysis is being performed, by default the algorithm to determines ordering
            probabilities. Value should be non-zero between 0 and 1 (Setting it to 1 effectively
            ignores this principle in lift and Z Score calculations).
            Types: float, int
        process_type:
            Optional Argument.
            Specifies the type of processing.
            Permitted Values:
                * 'all' - All processing is performed, from building support tables to
                  calculating final affinities.
                * 'support' - The single item support result DataFrame is built and then
                  processing is halted. This allows user to view the support result and decide
                  what the minimum support value should be, thus reducing the amount of processing
                  performed. The single item support output DataFrame is named as
                  'support_1_item' and underlying output table is named as
                  'ml__valib_association_1_ITEM_SUPPORT'. If "support_result_prefix" is specified,
                  it replaces 'ml__valib_association' with the provided value in the support
                  table name.
                * 'recalculate' - The final affinity tables are calculated based on support
                  tables already present. This requires that the "no_support_results" parameter
                  was set to False in a previous run so that the support tables are available for
                  recalculating the final affinities.
            Default Value: 'all'
            Types: str
        reduced_data:
            Optional Argument.
            Specified the reduced input data. If input to the analysis is reduced by using the
            "min_support", a "hierarchy_data", or a "filter", the resulting reduced input data
            can be saved for further analysis.
            Note:
                1. This is not affected by the use of a left-side or right-side lookup argument
                   or a "min_confidence", "min_lift", or "min_zscore" arguments.
                2. If further analysis is performed on this data, it may be appropriate to use
                   the "group_count" argument.
            Types: teradataml DataFrame
        relaxed_order:
            Optional Argument.
            Use this option in conjunction with sequence analysis, that is, when a sequence
            column is specified. Relaxed ordering occurs when the items on the left side of
            an association rule may occur in any order (via the sequence column), and the
            same is the case with the right-side items, provided that all left-side items
            precede all right-side items.
            Types: bool
        sequence_column:
            Optional Argument.
            Specifies the name of the column providing sequencing of input items if sequence
            analysis is desired. This might typically be a column of type date or timestamp.
            By default, sequence analysis is not performed.
            Types: str
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for analysis within Association Rules.
            For example,
                filter = "cust_id > 0"
            Note:
                Single quotes within the parameter value must be doubled, such as in
                where=channel <> '' ''. (Ordinarily, the expression would be where=channel <> ' '.
                Instead, the expression ends with quote-quote-blank-quote-quote).
            Types: str
        no_support_results:
            Optional Argument.
            Specifies whether the intermediate support results are required or not. By default,
            support results are not presented in the output.
            Notes:
                1. If "no_support_results" is False and "support_result_prefix" is not used,
                   then the generated underlying support tables are overwritten, if they already
                   exist.
                2. When set to True, support results generated depends on "combinations" parameter.
            Default Value: True
            Types: bool
        support_result_prefix:
            Optional Argument.
            Specifies a string that should be used to as prefix for the underlying table name
            for the support tables which can be accessed using output DataFrames.
            Notes:
                1. Teradata recommends using this when function is to be executed with
                   "process_type" as 'recalculate'. Make sure to use the same value for both the
                   function calls.
                2. If "no_support_results" is False and this is not used, then the generated
                   underlying support tables are overwritten, if they already exist.
            Default Value: 'ml__valib_association'
            Types: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed  
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of Association.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        AssociationObj.<attribute_name>.
        Note:
            Output DataFrames generated can be categorized as follows:
                1. Affinity output DataFrames
                2. Support output DataFrames
            Attribute names of these output DataFrames can be found out using two attributes:
                1. affinity_outputs:
                    This prints the attribute names of the output DataFrames containing the
                    affinity results.
                    For example, this can be accessed as
                        AssociationObj.affinity_outputs
                2. support_outputs:
                    This prints the attribute names of the output DataFrames containing the
                    support results.
                    For example, this can be accessed as
                        AssociationObj.support_outputs
            Output DataFrames generated by the function depend upon following:
                1. When "process_type" is set to 'support':
                    a. No affinity outputs are generated.
                    b. Function generates only two support output DataFrames named as:
                        i. support_1_item
                        ii. group_count
                2. When "no_support_results" is set to True and "process_type" is set to values
                   other than 'support':
                    a. Only affinity outputs are generated, based on the number of combinations
                       requested by the user.
                    b. No support outputs are generated.
                3. When "no_support_results" is set to False and "process_type" is set to values
                   other than 'support':
                    a. Affinity outputs are generated, based on the number of combinations requested.
                    b. Support outputs are also generated depending on the combination(s) requested.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("credit_tran")
        print(df)
        # Example 1: Perform Association analysis using default values.
        obj = valib.Association(data=df, group_column="cust_id", item_column="channel")
        # Print the affinity result. Only affinity result for default combination 11 is produced.
        print(obj.result_11)
        # Example 2: Requests a 1-to-1 and a 2-to-1 analysis, while also requesting 0.1 minimum
        #            support. Rows with blank channel column are also filtered.
        # Note: The blank channel value requires double single quotes,
        #       that is quote-quote-blank-quote-quote.
        obj = valib.Association(data=df,
                                group_column=["cust_id"],
                                item_column="channel",
                                min_support=0.1,
                                filter="channel <>''''",
                                combinations=[11, 21])
        # Executing above call will return two affinity results. Let's check the names of the
        # output DataFrames.
        print(obj.affinity_outputs)
        # Print the results.
        print(obj.result_11)
        print(obj.result_21)
        # Example 3: Request sequence analysis by specifying a "sequence_column" parameter.
        #            Also include optional parameters "min_support" and "filter".
        obj = valib.Association(data=df,
                                group_column=["cust_id"],
                                item_column="channel",
                                min_support=0.1,
                                filter="channel <>''''",
                                sequence_column="tran_date")
        # Executing above call will return one affinity result. Let's check the name of
        # the output DataFrame.
        obj.affinity_outputs
        # Print the results.
        print(obj.result_11)
        # Example 4: Let's generate the support results first and then calculate the final
        #            affinity results.
        # To generate the support results, we set "no_support_results" to False and use
        # 'test_prefix' as a prefix for the table names for the support tables generated.
        obj = valib.Association(data=df,
                                group_column=["cust_id"],
                                item_column="channel",
                                no_support_results=False,
                                support_result_prefix="test_prefix")
        # Print the results.
        print(obj)
        # Let's look at the attribute names of the support results generated.
        print(obj.support_outputs)
        # Print the individual support results.
        print(obj.support_result_01)
        print(obj.support_result_11)
        # Let's look at the attribute names of the affinity results generated.
        print(obj.affinity_outputs)
        # Print the affinity results.
        print(obj.result_11)
        # Re-run the Association function with "process_type" set to 'recalculate' for
        # recalculation of affinity results.
        # To recalculate we shall use the support results generated by the previous function call.
        obj = valib.Association(data=df,
                                group_column=["cust_id"],
                                item_column="channel",
                                process_type="recalculate",
                                support_result_prefix="test_prefix")
        # Print the affinity results.
        print(obj.result_11)
        # Example 5: Generate only SQL for the function, but do not execute the same.
        obj = valib.Association(data=df,
                                group_column=["cust_id"],
                                item_column="channel",
                                no_support_results=False,
                                support_result_prefix="test_prefix",
                                gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    DecisionTree
    DecisionTree
    DecisionTree
    Functions
    DecisionTree(data, columns, response_column, algorithm='gainratio', binning=False, exclude_columns=None, max_depth=100, num_splits=None,
    operator_database=None, pruning=None, charset=None)
    DESCRIPTION:
        The Gain Ratio Extreme Decision Tree function performs decision tree modeling and returns
        a teradataml DataFrame containing one row with two columns. The second column contains an
        XML string representing the resulting decision tree model described in Predictive Model
        Markup Language (PMML).
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to be used for decision tree modeling.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) to be used in decision tree building.
            Occasionally, it can also accept permitted strings to specify all columns, all
            numeric columns or all character columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
                    * 'allcharacter' - all character columns
            Types: str OR list of Strings (str)
        response_column:
            Required Argument.
            Specifies the name of a column whose values are being predicted.
            Types: str
        algorithm:
            Optional Argument.
            Specifies the name of the algorithm that the decision tree uses during building.
            Permitted Values: "gainratio"
            Default Value: "gainratio"
            Types: str
        binning:
            Optional Argument.
            Specifies whether to perform binning on the continuous independent variables
            automatically. When set to True, continuous data is separated into one hundred
            bins. If the column has fewer than one hundred distinct values, this argument
            is ignored.
            Default Value: False
            Types: bool
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the decision tree building.
            If 'all', 'allnumeric' or 'allcharacter' is used in the "columns" argument, this
            argument can be used to exclude specific columns from tree building.
            Types: str OR list of Strings (str)
        max_depth:
            Optional Argument.
            Specifies the maximum number of levels the tree can grow.
            Default Value: 100
            Types: int
        num_splits:
            Optional Argument.
            Specifies how far the decision tree can be split. Unless a node is pure (meaning
            it has only observations with the same dependent value) it splits if each branch
            that can come off this node contains at least this many observations. The default
            is a minimum of two cases for each branch.
            Types: int
        operator_database:
            Optional Argument.
            Specifies the database where the table operators called by Vantage Analytic Library
            reside. If not specified, the library searches the standard search path for table
            operators, including the current database.
            Types: str
        pruning:
            Optional Argument.
            Specifies the style of pruning to use after the tree is fully built.
            Permitted Values: "gainratio", "none" (no pruning)
            Default Value: "gainratio"
            Types: str
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of DecisionTree.
        Output teradataml DataFrames can be accessed using attribute references, such as
        DecisionTreeObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create the required teradataml DataFrame.
        df = DataFrame("customer_analysis")
        print(df)
        # Run DecisionTree() on columns "age", "income" and "nbr_children", with dependent
        # variable "gender".
        obj = valib.DecisionTree(data=df,
                                 columns=["age", "income", "nbr_children"],
                                 response_column="gender",
                                 algorithm="gainratio",
                                 binning=False,
                                 max_depth=5,
                                 num_splits=2,
                                 pruning="gainratio")
        # Print the results.
        print(obj.result)
    DecisionTreeEvaluator
    DecisionTreeEvaluator
    Functions
    DecisionTreeEvaluator(data, model, index_columns=None, response_column=None, accumulate=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        The function creates confusion matrix as XML output string, displaying counts of predicted
        versus actual values of the dependent variable of the decision tree model. It also contains
        counts of correct and incorrect predictions. The function also generates two profile
        DataFrames containing the details about the decisions made during the prediction.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data containing the columns to analyse, representing the dependent
            and independent variables in the analysis.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the teradataml DataFrame generated by VALIB DecisionTree() function,
            containing the decision tree model in PMML format that is used to predict the data.
            Types: teradataml DataFrame
        index_columns:
            Optional Argument.
            Specifies one or more different columns for the primary index of the result output
            DataFrame. By default, the primary index columns of the result output DataFrame are
            the primary index columns of the input DataFrame "data". In addition, the columns
            specified in this argument need to form a unique key for the result output DataFrame.
            Otherwise, there are more than one score for a given observation.
            Types: str OR list of Strings (str)
        response_column:
            Optional Argument.
            Specifies the name of the predicted value column. If this argument is not specified,
            the name of the dependent column in "data" DataFrame is used.
            Types: str
        accumulate:
            Optional Argument.
            Specifies one or more columns from the "data" DataFrame that can be passed to the
            result output DataFrame.
            Types: str OR list of Strings (str)
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of DecisionTreeEvaluator.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        DecisionTreeEvalObj.<attribute_name>.
        Output teradataml DataFrame attribute names are
            1. result
            2. profile_result_1
            3. profile_result_2
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create the required teradataml DataFrame.
        df = DataFrame("customer_analysis")
        print(df)
        # Example 1: Run DecisionTree() on columns "age", "income" and "nbr_children", with dependent
        # variable "gender".
        dt_obj = valib.DecisionTree(data=df,
                                    columns=["age", "income", "nbr_children"],
                                    response_column="gender",
                                    algorithm="gainratio",
                                    binning=False,
                                    max_depth=5,
                                    num_splits=2,
                                    pruning="gainratio")
        # Example 2: Evaluate the decision tree model generated above.
        obj = valib.DecisionTreeEvaluator(data=df,
                                          model=dt_obj.result,
                                          accumulate=["city_name", "state_code"])
        # Print the results.
        print(obj.result)
        print(obj.profile_result_1)
        print(obj.profile_result_2)
        # Example 3: Generate only SQL for the function, but do not execute the same.
        obj = valib.DecisionTreeEvaluator(data=df,
                                          model=decision_tree_obj.result,
                                          accumulate=["city_name", "state_code"],
                                          gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    DecisionTreePredict
    DecisionTreePredict
    Functions
    DecisionTreePredict(data, model, include_conﬁdence=False, index_columns=None, response_column=None, accumulate=None, targeted_value=None,
    gen_sql_only=False, charset=None)
    DESCRIPTION:
        The function predicts the values of the dependent variable in test data, using the model
        created by DecisionTree() VALIB function. The function also generates two profile
        DataFrames containing the details about the decisions made during the prediction.
        Apart from the score and profile DataFrames, the function optionally generates
            1. Confidence factors
            2. Targeted Binary Confidence
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data containing the columns to analyse, representing the
            dependent and independent variables in the analysis.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the teradataml DataFrame generated by VALIB DecisionTree() function,
            containing the decision tree model in PMML format that is used to predict the data.
            Types: teradataml DataFrame
        include_confidence:
            Optional Argument.
            Specifies whether the output DataFrame contain a column indicating how likely it
            is, for a particular leaf node on the tree, that the prediction is correct. If not
            specified or set to 'False', the confidence column is not created.
            Note:
                This argument cannot be specified along with "targeted_value" argument.
            Default Value: False
            Types: bool
        index_columns:
            Optional Argument.
            Specifies one or more different columns for the primary index of the result output
            DataFrame. By default, the primary index columns of the result output DataFrame are
            the primary index columns of the input DataFrame "data". In addition, the columns
            specified in this argument need to form a unique key for the result output DataFrame.
            Otherwise, there are more than one score for a given observation.
            Types: str OR list of Strings (str)
        response_column:
            Optional Argument.
            Specifies the name of the predicted value column. If this argument is not specified,
             the name of the dependent column in "data" DataFrame is used.
            Types: str
        accumulate:
            Optional Argument.
            Specifies one or more columns from the "data" DataFrame that can be passed to the
            result output DataFrame.
            Types: str OR list of Strings (str)
        targeted_value:
            Optional Argument.
            Specifies whether the result output DataFrame contain a column indicating how likely
            it is, for a particular leaf node and targeted value of a predicted result with only
            two values, that the prediction is correct.
            Note:
                This argument cannot be specified along with "include_confidence" argument.
            Permitted values: One of the values in the dependent column used in the argument
                              "response_column".
            Types: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of DecisionTreePredict.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        DecisionTreePredObj.<attribute_name>.
        Output teradataml DataFrame attribute names are
            1. result
            2. profile_result_1
            3. profile_result_2
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create the required teradataml DataFrame.
        df = DataFrame("customer_analysis")
        print(df)
        # Run DecisionTree() on columns "age", "income" and "nbr_children", with dependent
        # variable "gender".
        dt_obj = valib.DecisionTree(data=df,
                                    columns=["age", "income", "nbr_children"],
                                    response_column="gender",
                                    algorithm="gainratio",
                                    binning=False,
                                    max_depth=5,
                                    num_splits=2,
                                    pruning="gainratio")
        # Example 1: Predict the likeliness for a particular leaf node in the tree.
        obj = valib.DecisionTreePredict(data=df,
                                        model=dt_obj.result,
                                        include_confidence=True,
                                        accumulate=["city_name", "state_code"])
        # Print the results.
        print(obj.result)
        print(obj.profile_result_1)
        print(obj.profile_result_2)
        # Example 2: Predict the likeliness for a particular leaf node in the tree and binary
        #            targeted value.
        obj = valib.DecisionTreePredict(data=df,
                                        model=dt_obj.result,
                                        targeted_value="F",
                                        accumulate=["city_name", "state_code"])
        # Print the results.
        print(obj.result)
        print(obj.profile_result_1)
        print(obj.profile_result_2)
        # Example 3: Generate only SQL for the function, but do not execute the same.
        obj = valib.DecisionTreePredict(data=df,
                                        model=decision_tree_obj.result,
                                        targeted_value="F",
                                        accumulate=["city_name", "state_code"],
                                        gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Descriptive Statistics
    Descriptive Statistics Header
    Descriptive statistics provide functions to statistically analyze and explore a data residing 
    on Vantage. Descriptive statistical analysis is valuable for the following reasons:
        * It provides business insight.
        * It uncovers data quality issues, which, if not corrected or compensated for, jeopardize
          the accuracy of analytic models based on the data.
        * It isolates the data used in building analytic models. For example, when an outlying
          value is identified, you can include or exclude that value depending on a business problem.
        * Statistical processes used in analytic modeling require a certain type of data
          distribution. Descriptive statistical analysis help determine the suitability of various
          data elements for model input and suggest transformations that may be required.
    AdaptiveHistogram
    AdaptiveHistogram
    Functions
    AdaptiveHistogram(data, columns=None, bins=10, exclude_columns=None, spike_threshold=10, subdivision_method='means', subdivision_threshold=30,
    ﬁlter=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Histogram analysis reveals the distribution of continuous numeric or date values in a
        column. Histogram analysis is also referred to as binning because it counts the
        occurrence of values in a series of numeric ranges called bins. Adaptive Histogram
        analysis supplements Histogram analysis by offering options to further subdivide the
        distribution. You can specify the frequency percentage above which a single value
        should be treated as a Spike, and a similar percentage above which a bin should be
        subdivided. The Adaptive Histogram analysis modifies the computed equal-sized bins to
        include a separate bin for each spike value, and to further subdivide a bin with an
        excessive number of values, returning counts and boundaries for each resulting bin.
        The subdivision of a bin with too many values is performed by first dividing the bin
        by the same originally requested number of bins, and then merging this with a subdivision
        in the region around the mean value within the bin, the region being the mean +/- the
        standard deviation of the values in the bin. Subdividing can optionally be performed
        using quantiles, giving approximately equally distributed sub-bins. (The quantiles
        option is not recommended for teradataml DataFrames with extremely large numbers of
        rows, such as input DataFrame with billions of rows.)
        Adaptive binning can reduce the number of function calls needed to understand the
        distribution of the values assumed by a column. Without adaptive binning, spike values
        and/or overpopulated bins can distort the bin counts, as they are not separated or
        subdivided unless the Adaptive Histogram function is used. It should be noted that
        adaptive binning does not offer many of the specialized options that the standard
        Histogram analysis does.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to perform adaptive histogram analysis.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) to analyze. Occasionally, it can also
            accept permitted strings to specify all columns, or all numeric columns,
            or all numeric and date columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
                    * 'allnumericanddate' - all numeric and date columns
            Types: str OR list of Strings (str)
        bins:
            Optional Argument.
            Specifies the number of equal width bins to create.
            If multiple columns are requested, multiple bin sizes may be specified, such as
            bins = [5, 10]. If fewer sizes are specified than columns, leftover columns are
            associated with the default size of 10 bins.
            Default Value: 10
            Types: int OR list of Integers (int)
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the analysis, if a column
            specifier such as 'all', 'allnumeric', 'allnumericanddate' is used in the "columns"
            argument.
            Types: str OR list of Strings (str)
        spike_threshold:
            Optional Argument.
            Specifies a percentage of rows, expressed as an integer from 1 to 100, above which
            an individual value of a column is identified as a separate bin. Values with this
            or a larger percentage of rows are identified as a Spike.
            Default Value: 10 (10% of the total number of rows)
            Types: int
        subdivision_method:
            Optional Argument.
            Specifies the option to subdivide the bins.
            Permitted Values:
                * 'means' - Subdivide bins with too many values using a range of +/- the
                            standard deviation around the mean value in the bin.
                * 'quantiles' - Subdivide bins with too many values using quantiles, giving
                                approximately equally distributed bins.
            Default Value: 'means'
            Types: str
        subdivision_threshold:
            Optional Argument.
            Specifies a percentage of rows, expressed as an integer from 0 to 100, above which
            a bin is subdivided into sub-bins. Values with this or a larger percentage of rows
            are subdivided into sub-bins.
            Default Value: 30 (30% of the total number of rows)
            Types: int
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for analysis within Adaptive Histogram.
            For example,
                filter = "cust_id > 0"
            Types: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of AdaptiveHistogram.
        Output teradataml DataFrames can be accessed using attribute references, such as
        AdaptiveHistogramObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Shows execution with 10 equal width bins created by default for the
        #            'income' column.
        #            The default value of 10 is used for "spike_threshold", and 30 for
        #            "subdivision_threshold", with means as the default "subdivision_method".
        obj = valib.AdaptiveHistogram(data=df, columns="income")
        # Print the results.
        print(obj.result)
        # Example 2: Shows execution on two columns with different numbers of bins, 5 for
        #            'income' and 3 for 'age'. Threshold values and other parameters assume
        #            default values.
        obj = valib.AdaptiveHistogram(data=df,
                                      columns=["income", "age"],
                                      bins=[5,3])
        # Print the results.
        print(obj.result)
        # Example 3: Shows execution analysis done on two columns 'income' and 'age' with 5 bins
        #            for 'income' and default for 'age'. The non-default values of 12 is used for
        #            "spike_threshold", and 24 for "subdivision_threshold", with 'quantiles' as
        #            the default "subdivision_method".
        obj = valib.AdaptiveHistogram(data=df,
                                      columns=["income", "age"],
                                      bins=5,
                                      spike_threshold=12,
                                      subdivision_method="quantiles",
                                      subdivision_threshold=24,
                                      filter="cust_id > 0")
        # Print the results.
        print(obj.result)
        # Example 4: Generate only SQL for the function, but do not execute the same.
        obj = valib.AdaptiveHistogram(data=df,
                                      columns=["income", "age"],
                                      bins=[5,3],
                                      gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Explore
    Explore
    Functions
    Explore(data, columns=None, bins=10, bin_style='bins', max_comb_values=10000, max_unique_char_values=100, max_unique_num_values=20,
    min_comb_rows=25000, restrict_freq=True, restrict_threshold=1, statistical_method='population', stats_options=None, distinct=False, ﬁlter=None, gen_sql=False,
    charset=None)
    DESCRIPTION:
        Function performs basic statistical analysis on a set of selected teradataml
        DataFrame(s), or on selected columns from teradataml DataFrame. It stores results
        from four fundamental types of analysis based on simplified versions of the
        Descriptive Statistics analysis:
            1. Values
            2. Statistics
            3. Frequency
            4. Histogram
        Output teradataml DataFrames are produced for each type of analysis.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to perform basic statistical analysis.
            Types: teradataml DataFrame
        columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to analyze.
            Types: str OR list of Strings (str)
        bins:
            Optional Argument.
            Specifies the number of equal width bins to create for Histogram analysis.
            Default Value: 10
            Types: int
        bin_style:
            Optional Argument.
            Specifies the bin style for Histogram analysis.
            Permitted Values: 'bins', 'quantiles'
            Default Value: 'bins'
            Types: str
        max_comb_values:
            Optional Argument.
            Specifies the maximum number of combined values for frequency or histogram analysis.
            Default Value: 10000
            Types: int
        max_unique_char_values:
            Optional Argument.
            Specifies the maximum number of unique character values for unrestricted frequency
            analysis.
            Default Value: 100
            Types: int
        max_unique_num_values:
            Optional Argument.
            Specifies the maximum number of unique date or numeric values for frequency analysis.
            Default Value: 20
            Types: int
        min_comb_rows:
            Optional Argument.
            Specifies the minimum number of rows before frequency or histogram combining attempted.
            Default Value: 25000
            Types: int
        restrict_freq:
            Optional Argument.
            Specifies the restricted frequency processing including prominent values.
            Default Value: True
            Types: bool
        restrict_threshold:
            Optional Argument.
            Specifies the minimum percentage of rows a value must occur in, for inclusion in
            results.
            Default Value: 1
            Types: int
        statistical_method:
            Optional Argument.
            Specifies the method for calculating the statistics.
            Permitted Values: 'population', 'sample'
            Default Value: 'population'
            Types: str
        stats_options:
            Optional Argument.
            Specifies the basic statistics to be calculated for the Statistics analysis.
            Permitted Values:
                * all
                * count (cnt)
                * minimum (min)
                * maximum (max)
                * mean
                * standarddeviation (std)
                * skewness (skew)
                * kurtosis (kurt)
                * standarderror (ste)
                * coefficientofvariance (cv)
                * variance (var)
                * sum
                * uncorrectedsumofsquares (uss)
                * correctedsumofsquares (css)
            Types: str OR list of Strings (str)
        distinct:
            Optional Argument.
            Specifies the unique values count for each selected column when this argument is
            set to True.
            Default Value: False
            Types: bool
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for data exploration.
            For example,
                filter = "cust_id > 0"
            Types: str
        gen_sql:
            Optional Argument.
            Specifies whether to store and return the generated function SQL or not.
            When set to True, function SQL is generated as well as executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.           
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of Explore.
        Output teradataml DataFrames can be accessed using attribute references, such as
        ExploreObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. frequency_output
            2. histogram_output
            3. statistics_output
            4. values_output
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable,
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Shows data exploration with default values.
        obj = valib.Explore(data=df)
        # Print the frequency results.
        print(obj.frequency_output)
        # Print the histogram results.
        print(obj.histogram_output)
        # Print the statistics results.
        print(obj.statistics_output)
        # Print the values results.
        print(obj.values_output)
        # Example 2: Generate SQL for the function and execute the same.
        obj = valib.Explore(data=df,gen_sql=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Frequency
    Frequency
    Functions
    Frequency(data, columns=None, exclude_columns=None, cumulative_option=False, agg_ﬁlter=None, min_percentage=None, pairwise_columns=None,
    stats_columns=None, style='basic', top_n=None, ﬁlter=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Frequency analysis counts the occurrences of individual data values in columns that
        contain categorical data. Frequency analysis is useful in understanding the meaning
        of a particular data element and can point out the need to recode some of the data
        values found, either permanently or in the course of building an analytic data set.
        This function is also useful in analyzing combinations of values occurring in two
        or more columns.
        A Frequency analysis calculates the number of occurrences of each value of the column
        or columns individually or in combination. Additionally, for each value, the percentage
        of rows in the selected DataFrame is provided in descending order starting with the
        most frequently occurring value.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to perform frequency analysis.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) to analyze. Occasionally, it can also accept
            permitted strings to specify all columns, or all numeric columns, or all character
            columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
                    * 'allcharacter' - all numeric and date columns
            Types: str OR list of Strings (str)
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the analysis, if a column
            specifier such as 'all', 'allnumeric', 'allcharacter' is used in the "columns"
            argument.
            Types: str OR list of Strings (str)
        cumulative_option:
            Optional Argument.
            Specifies whether to include rank, cumulative count, and cumulative percent
            information for each frequency value. When set to 'True', this information is
            included otherwise not.
            Note:
                This argument should not be set to 'True' when style is 'pairwise'.
            Default Value: False
            Types: bool
        agg_filter:
            Optional Argument.
            Specifies the clause to restrict returned aggregations.
            For example,
                agg_filter="xpct > 1"
            Note:
                This argument should not be used when "min_percentage" argument is used.
            Types: str
        min_percentage:
            Optional Argument.
            Specifies a value to determine whether to include only frequency values that occur
            a minimum percentage of the time. Setting this to 0 or 0.0 is equivalent to not
            including the argument at all.
            Types: float, int
        pairwise_columns:
            Optional Argument.
            Specified the columns to be paired up with the frequency columns.
            Note:
                Use only when "style" is set to 'pariwise'.
            Types: str OR list of Strings (str)
        stats_columns:
            Optional Argument.
            Specifies the name(s) of column(s) for which the minimum, maximum, mean value,
            and standard deviation are included in the result with the values computed over
            the rows corresponding to the individual values of the frequency columns.
            Note:
                This argument can be used only when "style" is 'basic'.
            Types: str OR list of Strings (str)
        style:
            Optional Argument.
            Specifies the frequency style for the analysis.
            Permitted Values:
                * 'basic' - Counts frequencies of individual column values.
                * 'pairwise' - Counts frequencies of pair-wise combinations of values of
                               selected columns rather than individually. This should not be used
                               when "cumulative_option" is set to 'True'.
                * 'crosstab' - Counts frequencies of combinations of values of selected columns
                               rather than individually.
            Default Value: 'basic'
            Types: str
        top_n:
            Optional Argument.
            Specifies the number of frequency values to include. Using this argument shows
            frequency only for the number of top occurring values entered.
            Note:
                This argument is enabled only if "cumulative_option" is set to 'True'.
            Types: int
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for analysis within Frequency.
            For example,
                filter = "cust_id > 0"
            Types: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of Frequency.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        FrequencyObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Perform frequency analysis using default values.
        obj = valib.Frequency(data=df,
                              columns="years_with_bank")
        # Print the results.
        print(obj.result)
        # Example 2: Calculate frequency values that occur a minimum 10% of the time for
        #            the column 'years_with_bank'.
        obj = valib.Frequency(data=df,
                              columns="years_with_bank",
                              min_percentage=10)
        # Print the results.
        print(obj.result)
        # Example 3: Calculate the frequency values for the column 'years_with_bank'. Include
        #            cumulative measures and get only the top five frequency values.
        obj = valib.Frequency(data=df,
                              columns="years_with_bank",
                              cumulative_option=True,
                              top_n=5)
        # Print the results.
        print(obj.result)
        # Example 4: Calculate the frequencies using 'crosstab' style on two columns 'income'
        #            and 'age'.
        obj = valib.Frequency(data=df,
                              columns=["income", "age"],
                              style="crosstab")
        # Print the results.
        print(obj.result)
        # Example 5: Calculate frequencies using 'pairwise' style by combining one pairwise
        #            column with two frequency columns.
        obj = valib.Frequency(data=df,
                              columns=["gender", "marital_status"],
                              style="pairwise",
                              pairwise_columns="years_with_bank")
        # Print the results.
        print(obj.result)
        # Example 6: Calculate frequencies with inclusion of "stats_columns".
        obj = valib.Frequency(data=df,
                              columns="gender",
                              stats_columns="years_with_bank")
        # Print the results.
        print(obj.result)
        # Example 7: Calculate frequencies by applying filtering using "agg_filter" and
        #            "filter" to introduce a custom where clause and having clause.
        obj = valib.Frequency(data=df,
                              columns="years_with_bank",
                              filter="cust_id > 0",
                              agg_filter="xpct > 1")
        # Print the results.
        print(obj.result)
        # Example 8: Generate only SQL for the function, but do not execute the same.
        obj = valib.Frequency(data=df,
                              columns=["income", "age"],
                              gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Histogram
    Histogram
    Functions
    Histogram(data, columns=None, bins=10, bins_with_boundaries=None, boundaries=None, quantiles=10, widths=None, exclude_columns=None,
    overlay_columns=None, stats_columns=None, hist_style='basic', ﬁlter=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Histogram analysis reveals the distribution of continuous numeric or date values
        in a column. Histogram analysis is also referred to as binning because it counts the
        occurrence of values in a series of numeric ranges called bins. The histogram
        analysis provides a number of ways to define bins, allowing multidimensional binning,
        overlaying of categorical data, and the calculation of numeric statistics within
        bins. If you set the desired number of equal sized data bins, the desired number of
        bins with a nearly equal number of values, a desired width, or the specific boundaries,
        the Histogram analysis separates the data to show its distributional properties.
        It does this by separating the data by bin number and gives counts and percentages
        over the requested rows. Percentages always sum to 100%. Separate options are available
        to specify a number of equal sized data bins in which the analysis determines the
        minimum and maximum value, as well as a user-specified minimum and maximum value.
        If the minimum and maximum are specified, all values less than the minimum are put in
        bin 0, while all values greater than the maximum are put in bin N+1. The same is
        true when the boundary option is specified.
        The Histogram analysis optionally provides subtotals within each bin of the count,
        percentage within the bin and percentage overall for each value or combination of
        values of one or more overlaid columns. Another option is provided to collect simple
        statistics for a binned column or another column of numeric or date type within the
        table, providing the minimum, maximum, mean, and standard deviation. When statistics
        are collected for a date type column, the standard deviation is given in units of days.
        Histogram analysis can be performed on columns of numeric or date data type.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to perform Histogram analysis.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) to analyze. Occasionally, it can also
            accept permitted strings to specify all columns, or all numeric columns, or
            all numeric and date columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
                    * 'allnumericanddate' - all numeric and date columns
            Types: str OR list of Strings (str)
        bins:
            Optional Argument.
            Specifies the number of equal width bins to create.
            If multiple columns are requested, multiple bin sizes may be specified, such
            as bins = [5, 10]. If fewer sizes are specified than columns, left-over columns
            are associated with the default size of 10 bins.
            Default Value: 10
            Types: int OR list of Integers (int)
        bins_with_boundaries:
            Optional Argument.
            Specifies the number of bins spanning a range specified by the minimum and
            maximum values.
            For example,
                bins_with_boundaries = [5,0,200]
                creates 5 bins ranging from 0 to 200.
            If multiple columns are requested, multiple sets of parameters must be
            specified, such as bins_with_boundaries = ["{10, 0, 200000}", "{5, 0, 100}"].
            Note that multiple values are provided as string with numbers enclosed in curly
            braces '{}'. Each such value corresponds to the value in "columns" argument.
            Types: int, str OR list of Integers (int) or Strings (str)
        boundaries:
            Optional Argument.
            Specifies the boundaries that define the bins.
            For example,
                boundaries = [0,50,100,150]
                provides 3 bins between 0 and 150 (0 to 50, 50 to 100, and 100 to 150).
            If multiple columns are requested, multiple sets of parameters must be
            specified, such as boundaries = ["{0, 50000, 100000, 150000}", "{0, 50, 100}"].
            Note that multiple values are provided as string with numbers enclosed in curly
            braces '{}'. Each such value corresponds to the value in "columns" argument.
            Types: int, str OR list of Integers (int) or Strings (str)
        quantiles:
            Optional Argument.
            Specifies the number of approximately equally populated bins to create.
            If multiple columns are requested, multiple quantile sizes may be specified,
            such as quantiles=5, 10. If fewer sizes are specified than columns, left-over
            columns are associated with the default size of 10 quantiles.
            Default Value: 10
            Types: int OR list of Integers (int)
        widths:
            Optional Argument.
            Specifies the width of the bins to create.
            If multiple columns are requested, multiple widths must be specified, such
            as widths = [5, 10]. If fewer sizes are specified than columns, an error
            message displays.
            Types: int OR list of Integers (int)
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the analysis, if a
            column specifier such as 'all', 'allnumeric', 'allnumericanddate' is used
            in the "columns" argument.
            Types: str OR list of Strings (str)
        overlay_columns:
            Optional Argument.
            Specifies a categorical variable with only a few values. If an overlay column
            is specified, frequencies within each bin are calculated for each value of
            that overlay column (frequencies for crosstabs of values are given if more
            than one overlay column is requested).
            Note:
                Use a specific column in either "overlay_columns" or "stats_columns",
                but not both.
            Types: str OR list of Strings (str)
        stats_columns:
            Optional Argument.
            Specifies a list of numeric columns/aliases for which simple statistics are
            calculated (minimum, maximum, mean and standard deviation) in each bin. This
            argument is not available for DATE columns.
            Note:
                Use a specific column in either "overlay_columns" or "stats_columns",
                but not both.
            Types: str OR list of Strings (str)
        hist_style:
            Optional Argument.
            Specifies the histogram style to use for analysis.
            Permitted Values:
                * 'basic' - Creates a histogram for individual columns.
                * 'crosstab' - Creates a multidimensional histogram by combining columns.
            Default Value: 'basic'
            Types: str
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for analysis within Histogram.
            For example,
                filter = "cust_id > 0"
            Types: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of Histogram.
        Output teradataml DataFrames can be accessed using attribute references, such as
        HistogramObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Perform Histogram analysis using default values.
        obj = valib.Histogram(data=df,
                              columns=["income", "age"])
        # Print the results.
        print(obj.result)
        # Example 2: Perform Histogram analysis using "overlay_columns".
        obj = valib.Histogram(data=df,
                              columns=["income", "age"],
                              overlay_columns="gender")
        # Print the results.
        print(obj.result)
        # Example 3: Perform Histogram analysis using "stats_columns".
        obj = valib.Histogram(data=df,
                              columns=["income", "age"],
                              stats_columns="years_with_bank")
        # Print the results.
        print(obj.result)
        # Example 4: Perform Histogram analysis using "crosstab" style, with filter.
        obj = valib.Histogram(data=df,
                              columns=["income", "age"],
                              hist_style="crosstab",
                              filter="income > 0")
        # Print the results.
        print(obj.result)
        # Example 5: Perform Histogram analysis by specifying number of "bins".
        obj = valib.Histogram(data=df,
                              columns="income",
                              bins=5)
        # Print the results.
        print(obj.result)
        # Example 6: Perform Histogram analysis by specifying bin width.
        obj = valib.Histogram(data=df,
                              columns="income",
                              widths=25000)
        # Print the results.
        print(obj.result)
        # Example 7: Perform Histogram analysis by specifying number of quantiles.
        obj = valib.Histogram(data=df,
                              columns="income",
                              quantiles=4)
        # Print the results.
        print(obj.result)
        # Example 8: Perform Histogram analysis by specifying boundaries on single column.
        obj = valib.Histogram(data=df,
                              columns="income",
                              boundaries=[0, 50000, 100000, 150000])
        # Print the results.
        print(obj.result)
        # Example 9: Perform Histogram analysis by specifying boundaries on multiple columns.
        obj = valib.Histogram(data=df,
                              columns=["income", "age"],
                              boundaries=["{0, 50000, 100000, 150000}", "{0,25,50,75}"])
        # Print the results.
        print(obj.result)
        # Example 10: Perform Histogram analysis by specifying bins with boundaries
        #             on single column.
        obj = valib.Histogram(data=df,
                              columns="income",
                              bins_with_boundaries=[10, 0, 200000])
        # Print the results.
        print(obj.result)
        # Example 11: Perform Histogram analysis by specifying boundaries on multiple columns.
        obj = valib.Histogram(data=df,
                              columns=["income", "age"],
                              bins_with_boundaries=["{10, 0, 200000}", "{5, 0, 75}"])
        # Print the results.
        print(obj.result)
        # Example 12: Generate only SQL for the function, but do not execute the same.
        obj = valib.Histogram(data=df,
                              columns=["income", "age"],
                              overlay_columns="gender",
                              gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Overlap
    Overlap
    Functions
    Overlap(data1, columns1, gen_sql_only=False, charset=None, **kwargs)
    DESCRIPTION:
        The function performs Overlap analysis by combining information from multiple
        DataFrames into an analytic data set by providing counts of overlapping key
        fields among pairs of DataFrames. For example, if an analytic data set is being
        built to describe customers, it is useful to know whether the customer, account,
        and transaction tables that provide information about customers refer to the
        same customers.
        Given teradataml DataFrames and corresponding column names, the Overlap analysis
        determines the number of instances of that column which each pair-wise combination
        of DataFrames has in common. The same can also be performed for multiple columns
        taken together.
        Overlap analysis is used to process any data type that is comparable except those
        containing byte data.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the input DataFrame containing the columns on which Overlap analysis
            is to be performed.
            Types: teradataml DataFrame
        columns1:
            Required Argument.
            Specifies the name(s) of the column(s), in "data1" argument, to be used in
            Overlap analysis.
            Types: str OR list of Strings (str)
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
        kwargs:
            Specifies the additional data and columns arguments that can be used with
            "data1" and "columns1" for Overlap analysis.
            data2, ..., dataN:
                Optional Arguments.
                Specifies the additional input DataFrames containing the columns on which
                Overlap analysis is to be performed along with "data1" and "columns1".
                Types: list of teradataml DataFrames
            columns2, ..., columnsN:
                Optional Arguments.
                Specifies the name(s) of the columns(s) of additional input DataFrames to be
                used in the Overlap analysis along with "data1" and "columns1".
                Types: str OR list of Strings (str)
            Notes:
                1.The data and columns related arguments must be in a sequence starting from
                  "data2" and "columns2" respectively.
                2.For each data argument (datai), corresponding columns argument (columnsi)
                  must be specified and vice-versa.
                3.The number of columns in each of the columns related arguments (including
                  "columns1" argument) should be same.
    RETURNS:
        An instance of Overlap.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        OverlapObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        customer = DataFrame("customer")
        customer_analysis = DataFrame("customer_analysis")
        checking_tran = DataFrame("checking_tran")
        credit_tran = DataFrame("credit_tran")
        savings_tran = DataFrame("savings_tran")
        # Example 1: Run Overlap analysis on "cust_id" column present in the DataFrames
        #            'customer' and 'customer_analysis'.
        overlap_obj = valib.Overlap(data1=customer,
                                    data2=customer_analysis,
                                    columns1=["cust_id"],
                                    columns2="cust_id")
        # Print the results.
        print(overlap_obj.result)
        # Example 2: Run Overlap analysis on columns "cust_id" and "tran_id" present in
        #            the DataFrames 'checking_tran', 'credit_tran' and 'savings_tran'.
        overlap_obj = valib.Overlap(data1=checking_tran,
                                    data2=credit_tran,
                                    data3=savings_tran,
                                    columns1=["cust_id", "tran_id"],
                                    columns2=["cust_id", "tran_id"],
                                    columns3=["cust_id", "tran_id"])
        # Print the results.
        print(overlap_obj.result)
        # Example 3: Generate only SQL for the function, but do not execute the same.
        obj = valib.Overlap(data1=customer,
                            data2=customer_analysis,
                            columns1=["cust_id"],
                            columns2="cust_id",
                            gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Statistics
    Statistics
    Functions
    Statistics(data, columns=None, exclude_columns=None, extended_options='none', group_columns=None, statistical_method='population', stats_options=None,
    ﬁlter=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Statistics analysis provides several common and not so common statistical measures
        for numeric data columns. Extended options include additional analyses and measures
        such as Values, Modes, Quantiles, and Ranks. Use statistical measures to understand
        the characteristics and properties of each numeric column, and to look for outlying
        values and anomalies.
        Statistics analysis can be performed on columns of numeric or date data type. For
        columns of type DATE, statistics other than count, minimum, maximum, and mean are
        calculated by first converting to the number of days since 1900.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to perform Statistics analysis.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) to analyze. Occasionally, it can also
            accept permitted strings to specify all columns, or all numeric columns, or
            all numeric and date columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
                    * 'allnumericanddate' - all numeric and date columns
            Types: str OR list of Strings (str)
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the analysis, if a
            column specifier such as 'all', 'allnumeric', 'allnumericanddate' is used
            in the "columns" argument.
            Types: str OR list of Strings (str)
        extended_options:
            Optional Argument.
            Specifies the extended options for calculating statistics.
            Permitted Values: 'all', 'none', 'modes', 'quantiles', 'values', 'rank'
            Default Value: 'none'
            Types: str OR list of Strings (str)
        group_columns:
            Optional Argument.
            Specifies the name(s) of column(s) to perform separate analysis for each group.
            Types: str OR list of Strings (str)
        statistical_method:
            Optional Argument.
            Specifies the statistical method.
            Permitted Values: 'sample', 'population'
            Default Value: 'population'
            Types: str
        stats_options:
            Optional Argument.
            Specifies the basic statistics to be calculated.
            Permitted Values:
                * all
                * count (cnt)
                * minimum (min)
                * maximum (max)
                * mean
                * standarddeviation (std)
                * skewness (skew)
                * kurtosis (kurt)
                * standarderror (ste)
                * coefficientofvariance (cv)
                * variance (var)
                * sum
                * uncorrectedsumofsquares (uss)
                * correctedsumofsquares (css)
            Default Value: ['cnt', 'min', 'max', 'mean', 'std']
            Types: str OR list of Strings (str)
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for analysis within Statistics.
            For example,
                filter = "cust_id > 0"
            Types: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of Statistics.
        Output teradataml DataFrames can be accessed using attribute references, such as
        StatisticsObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Perform Statistics analysis using default values on 'income' column.
        obj = valib.Statistics(data=df, columns="income")
        # Print the results.
        print(obj.result)
        # Example 2: Perform Statistics analysis on 'income' column with values grouped
        #            by 'gender' and only for rows with income greater than 0.
        obj = valib.Statistics(data=df,
                               columns="income",
                               group_columns="gender",
                               filter="income > 0")
        # Print the results.
        print(obj.result)
        # Example 3: Perform Statistics analysis requesting all statistical measures
        #            and extended options.
        obj = valib.Statistics(data=df,
                               columns="income",
                               stats_options="all",
                               extended_options="all")
        # Print the results.
        print(obj.result)
        # Example 4: Perform Statistics analysis requesting specific statistical measures
        #            and extended options and return sample statistics.
        obj = valib.Statistics(data=df,
                               columns="income",
                               stats_options=["cnt", "max", "min", "'mean",
                                              "css", "uss", "kurt", "skew"],
                               extended_options=["modes", "rank"],
                               statistical_method="sample")
        # Print the results.
        print(obj.result)
        # Example 5: Generate only SQL for the function, but do not execute the same.
        obj = valib.Statistics(data=df,
                               columns="income",
                               group_columns="gender",
                               filter="income > 0",
                               gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    TextAnalyzer
    TextAnalyzer
    Functions
    TextAnalyzer(data, columns, exclude_columns=None, analyze_numerics=True, analyze_unicode=False, gen_sql_only=False, charset=None)
    DESCRIPTION:
        When working with character data it is useful to determine the data type and what
        data can be stored in the database. The TextAnalyzer function analyzes character
        data and distinguishes if the field is a numeric type, date, time, timestamp, or
        character data and returns two output DataFrames, one containing the analysis
        results and the second one containing the column data type matrix having the
        progression of data type through the series of steps (mentioned below).
        The function runs a series of tests to distinguish what the correct underlying
        type of each selected column should be. The first test performed on the column is
        the MIN and the MAX test. The MIN and MAX values of a column are retrieved from
        the database and tested to determine what type the values are. The next test is a
        sample test which retrieves a small sample of data for each column and again
        assesses what type they should be. The next test, extended Numeric analysis, is
        for fields that were determined to be numeric and it tries to classify them in a
        more specific category if possible. For instance, a column that is considered a
        FLOAT type after the first two tests might really be a DECIMAL type with 2 decimal
        places. In the next test, a date type is validated to make sure all values in that
        column are truly dates. Finally, if requested, unicode columns are tested to see
        if they contain only Latin characters. This test is called extended Unicode analysis.
        The function can be used on columns of any character data type. Columns of
        non-character data type are passed along to the output as defined in the input data.
        Note:
           This function is available in Vantage Analytic Library 2.0.0.2.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to perform text analysis.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) to analyze. Occasionally, it can also
            accept permitted strings to specify all columns, or all character columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allcharacter' - all character columns
            Types: str OR list of Strings (str)
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the analysis, if a
            column specifier such as 'all', 'allcharacter' is used in the "columns" argument.
            Types: str OR list of Strings (str)
        analyze_numerics:
            Optional Argument.
            Specifies whether to process specific numeric types. If True, the function
            processes numeric types.
            Default Value: True
            Types: bool
        analyze_unicode:
            Optional Argument.
            Specifies whether a column declared to contain Unicode characters actually
            contains only Latin characters. If True, Unicode analysis is performed.
            Default Value: False
            Types: bool
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of TextAnalyzer.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        TextAnalyzerObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1.result
            2.data_type_matrix
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Perform text analysis on all columns with only required arguments
        #            and default values for "analyze_numerics" and "analyze_unicode" arguments.
        obj = valib.TextAnalyzer(data=df,
                                 columns="all")
        # Print the results.
        print(obj.result)
        print(obj.data_type_matrix)
        # Example 2: Perform text analysis, including numeric and unicode analysis, on
        #            columns 'cust_id', 'gender' and 'marital_status'.
        obj = valib.TextAnalyzer(data=df,
                                 columns=["cust_id", "gender", "marital_status"],
                                 analyze_numerics=True,
                                 analyze_unicode=True)
        # Print the results.
        print(obj.result)
        print(obj.data_type_matrix)
        # Example 6: Generate only SQL for the function, but do not execute the same.
        obj = valib.TextAnalyzer(data=df,
                                 columns="all",
                                 gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Values
    Values
    Functions
    Values(data, columns=None, exclude_columns=None, group_columns=None, distinct=False, ﬁlter=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Use Values analysis as the first type of analysis performed on unknown data.
        Values analysis determines the nature and quality of the data. For example, whether
        the data is categorical or continuously numeric, how many null values it contains,
        and so on.
        A Values analysis provides a count of rows, rows with non-null values, rows with
        null values, rows with value 0, rows with a positive value, rows with a negative
        value, and the number of rows containing blanks in the given column. By default,
        unique values are counted, but this calculation can be inhibited for performance
        reasons if desired.
        For a column of non-numeric type, the zero, positive, and negative counts are
        always zero (for example, 000 is not counted as 0). A Values analysis can be
        performed on columns of any data type, though the measures displayed vary according
        to column type.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to perform Values analysis.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) to analyze. Occasionally, it can also
            accept permitted strings to specify all columns, or all numeric columns, or
            all character columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
                    * 'allcharacter' - all numeric and date columns
            Types: str OR list of Strings (str)
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the analysis, if a
            column specifier such as 'all', 'allnumeric', 'allcharacter' is used in the
            "columns" argument.
            Types: str OR list of Strings (str)
        group_columns:
            Optional Argument.
            Specifies the name(s) of column(s) to perform separate analysis for each group.
            Types: str OR list of Strings (str)
        distinct:
            Optional Argument.
            Specifies whether to select unique values count for each selected column.
            Default Value: False
            Types: bool
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for analysis within Values.
            For example,
                filter = "cust_id > 0"
            Types: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of Values.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        ValuesObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Perform Values analysis using default values on 'income' and
        #            'marital_status' columns.
        obj = valib.Values(data=df, columns=["income", "marital_status"])
        # Print the results.
        print(obj.result)
        # Example 2: Perform Values analysis on 'income' column with values grouped by
        #            'gender' and only for rows with income greater than 0.
        obj = valib.Values(data=df, columns="income", group_columns="gender", filter="income > 0")
        # Print the results.
        print(obj.result)
        # Example 3: Generate only SQL for the function, but do not execute the same.
        obj = valib.Values(data=df, 
                           columns=["income", "marital_status"],
                           filter="cust_id > 0",
                           distinct=False,
                           gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Factor Analysis
    PCA
    PCA
    Functions
    PCA(data, columns, exclude_columns=None, cond_ind_threshold=30, min_eigen=1.0, load_report=False, vars_load_report=False, vars_report=False,
    gamma=None, group_columns=None, matrix_input=False, matrix_type='correlation', near_dep_report=False, rotation_type=None, load_threshold=None,
    percent_threshold=None, variance_prop_threshold=0.5, charset=None)
    DESCRIPTION:
        Factor Analysis is one of the most fundamental types of statistical analysis, and
        Principal Components Analysis (PCA), is arguably the most common variety of Factor
        Analysis. In PCA Analysis, a set of variables (denoted by columns) is reduced to a
        smaller number of factors that account for most of the variance in the variables.
        This can be useful in reducing the number of variables by converting them to factors,
        or in gaining insight into the nature of the variables when they are used for further
        data analysis.
        Some of the key features of PCA Analysis are outlined below.
        1. One or more group by columns can optionally be specified so that an input matrix
           is built for each combination of group by column values, and subsequently a
           separate PCA Analysis model is built for each matrix.
        2. A Near Dependency Report is available to identify two or more columns that may be
           collinear. This report can be requested by setting the argument "near_dep_report"
           to 'True' and if desired, the arguments "cond_ind_threshold" and
           "variance_prop_threshold".
        3. Both orthogonal and oblique factor rotations are available. Refer to the
           "rotation_type" parameter.
        4. There are three Prime Factor reports available. Refer to the "load_report",
           "vars_report", and "vars_load_report" arguments.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data containing the columns to perform PCA analysis.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) representing the variables used in
            building a PCA analysis model. Occasionally, it can also accept permitted
            strings to specify all columns or all numeric columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
            Types: str OR list of Strings (str)
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the PCA analysis.
            If 'all' or 'allnumeric' is used in the "columns" argument, this argument
            can be used to exclude specific columns from the analysis.
            Types: str OR list of Strings (str)
        cond_ind_threshold:
            Optional Argument. Required when the argument "near_dep_report" is set to 'True'.
            Specifies the condition index threshold parameter to generate Near Dependency Report.
            Default Value: 30
            Types: float
        min_eigen:
            Optional Argument.
            Specifies the minimum eigen value to include factors for.
            Default Value: 1.0
            Types: float
        load_report:
            Optional Argument.
            Specifies whether to generate Prime Factor Loadings Report in which rows are
            variables and columns are factors, matching each variable with the factor that
            has the biggest absolute loading value with.
            When set to 'True', Prime Factor Loadings Report is generated and added in the
            XML result string.
            Default Value: False
            Types: bool
        vars_load_report:
            Optional Argument.
            Specifies whether to generate Prime Factor Variables with Loadings Report,
            equivalent to Prime Factor Variables Report with the addition of loading values
            that determined the relationship between factors and variables. The absolute
            sizes of the loading values point out the relationship strength and the sign
            its direction, i.e., either a positive or negative correlation.
            When set to 'True', Prime Factor Variables with Loadings Report is generated
            and added in the XML result string.
            Default Value: False
            Types: bool
        vars_report:
            Optional Argument.
            Specifies whether to generate Prime Factor Variables Report in which rows are
            variables and columns are factors, matching variables with their prime factors,
            and if a threshold is used, possibly other than prime factors. (Either a
            threshold percent is specified with the "percent_threshold" argument or a
            threshold loading is specified with the "load_threshold" argument.)
            When set to 'True', Prime Factor Variables Report is generated and added in
            the XML result string.
            Default Value: False
            Types: bool
        gamma:
            Optional Argument. Required when the argument "rotation_type" is set to
            'orthomax' or 'orthomin'.
            Specifies the gamma value to be set when 'orthomax' or 'orthomin' is used in
            "rotation_type" argument.
            Note:
                This argument is ignored for values of "rotation_type" other than
                'orthomax' and 'orthomin'.
            Types: float
        group_columns:
            Optional Argument.
            Specifies the name(s) of the input column(s) dividing the input DataFrame
            "data" into partitions, one for each combination of values in the group by
            columns. For each partition or combination of values, a separate factor model
            is built. The default case is no group by columns.
            Types: str OR list of Strings (str)
        matrix_input:
            Optional Argument.
            Specifies whether the input DataFrame is an extended
            sum-of-squares-and-cross-products (ESSCP) matrix built by the Matrix() VALIB
            function. Use of this feature saves internally building a matrix each time
            this function is performed, providing a significant performance improvement.
            When set to 'True', the columns specified with the "columns" argument may be
            a subset of the columns in the matrix and may be specified in any order. The
            columns must, however, all be present in the matrix. Further, if group by
            columns are specified in the Matrix() call, these same group by columns must
            be specified in this function.
            Note:
                If the input DataFrame "data" represents a saved matrix, set this argument
                to 'True' to get predictable results.
            Default Value: False
            Types: bool
        matrix_type:
            Optional Argument.
            Specifies type of matrix for processing affecting measure and score scaling.
            Permitted Values: 'correlation', 'covariance'
            Default Value: 'correlation'
            Types: str
        near_dep_report:
            Optional Argument.
            Specifies whether to produce an XML report showing columns that are collinear
            as part of the output or not. The report is included in the XML output only
            if collinearity is detected.
            Two threshold arguments are available for this report, "cond_ind_threshold"
            and "variance_prop_threshold".
            Default Value: False
            Types: bool
        rotation_type:
            Optional Argument.
            Specifies the rotation type among various schemes for rotating factors for
            possibly better results. Both orthogonal and oblique rotations are provided.
            Gamma value in the rotation equation assumes a different value for each
            rotation type, with f representing the number of factors and v the number of
            variables. Refer below table:
            Table: Gamma values of different rotation types
             --------------------------------------------------------------------------------------
            |   rotation_type   | gamma value   | Orthogonal/Oblique    | Notes                    |
             --------------------------------------------------------------------------------------
            | equamax           | f/2           | orthogonal            |                          |
            | prthomax          | Set by user   | orthogonal            |                          |
            | parsimax          | v(f-1)/v+f+2  | orthogonal            |                          |
            | quartimax         | 0.0           | orthogonal            |                          |
            | varimax           | 1.0           | orthogonal            |                          |
            | biquartimin       | 0.5           | oblique               | least oblique rotation   |
            | covarimin         | 2.0           | oblique               |                          |
            | orthomin          | Set by user   | oblique               |                          |
            | quartimin         | 0.0           | oblique               | most oblique rotation    |
             --------------------------------------------------------------------------------------
            Types: str
        load_threshold:
            Optional Argument.
            Specifies a threshold factor loading value. If this argument is specified,
            a factor that is not a prime factor may be associated with a variable. This
            argument is used when the argument "vars_report" is set to 'True'; ignored
            otherwise.
            Notes:
                1. This argument and the argument "percent_threshold" cannot both be specified.
                2. This argument is used when the argument "vars_report" is set to 'True';
                   ignored otherwise.
            Types: float
        percent_threshold:
            Optional Argument.
            Specifies a threshold percent. If this argument is specified, a factor that is
            not a prime factor may be associated with a variable.
            Notes:
                1. This argument and the argument "load_threshold" cannot both be specified.
                2. This argument is used when the argument "vars_report" is set to 'True';
                   ignored otherwise.
            Types: float
        variance_prop_threshold:
            Optional Argument. Required when the argument "near_dep_report" is set to 'True'.
            Specifies the variance proportion threshold parameter to generate Near
            Dependency Report.
            Default Value: 0.5
            Types: float
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of PCA.
        Output teradataml DataFrames can be accessed using attribute references, such as
        PCAObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create the required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Generate Near Dependency Report.
        obj = valib.PCA(data=df,
                        columns=["age", "years_with_bank", "nbr_children"],
                        cond_ind_threshold=3,
                        near_dep_report=True,
                        variance_prop_threshold=.3)
        # Print the results.
        print(obj.result)
        # Example 2: Run PCA on two group by columns. The result DataFrame contains one row
        #            for each group by column combination.
        obj = valib.PCA(data=df,
                        columns=["age", "years_with_bank", "nbr_children"],
                        group_columns=["gender", "marital_status"])
        # Print the results.
        print(obj.result)
        # Example 3: Run PCA by taking input from a pre-built matrix. Both the Matrix Build
        #            and PCA Analysis are shown. Note that only a subset of matrix columns
        #            is used.
        mat_obj = valib.Matrix(data=df,
                               columns=["income", "age", "years_with_bank", "nbr_children"],
                               type="esscp")
        obj = valib.PCA(data=mat_obj.result,
                        columns=["age", "years_with_bank", "nbr_children"],
                        matrix_input=True)
        # Print the results.
        print(obj.result)
        # Example 4: Run PCA by taking input from a pre-built matrix with group by columns.
        #            Both the Matrix Build and  PCA Analysis are shown. Note that only a
        #            subset of matrix columns is used.
        mat_obj = valib.Matrix(data=df,
                               columns=["income", "age", "years_with_bank", "nbr_children"],
                               group_columns="gender",
                               type="esscp")
        obj = valib.PCA(data=mat_obj.result,
                        columns=["age", "years_with_bank", "nbr_children"],
                        matrix_input=True,
                        group_columns="gender")
        # Print the results.
        print(obj.result)
        # Example 5: Run PCA with 'varimax' rotation.
        obj = valib.PCA(data=df,
                        columns=["age", "years_with_bank", "nbr_children"],
                        rotation_type="varimax")
        # Print the results.
        print(obj.result)
        # Example 6: Run PCA with Prime Factor reports requested. The "percent_threshold"
        #            argument applies to "vars_report" argument.
        obj = valib.PCA(data=df,
                        columns=["age", "years_with_bank", "nbr_children"],
                        load_report=True,
                        vars_load_report=True,
                        vars_report=True,
                        percent_threshold=0.9)
        # Print the results.
        print(obj.result)
    PCAEvaluator
    PCAEvaluator
    Functions
    PCAEvaluator(data, model, index_columns=None, accumulate=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        The function evaluates the PCA model created by PCA() VALIB function and generates
        an output XML string in result teradataml DataFrame.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data used to evaluate the PCA model.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the teradataml DataFrame generated by VALIB PCA() function, containing
            the PCA model to evaluate.
            Types: teradataml DataFrame
        index_columns:
            Optional Argument.
            Specifies one or more different columns for the primary index of the result
            output DataFrame. By default, the primary index columns of the result output
            DataFrame are the primary index columns of the input DataFrame "data". In
            addition, the columns specified in this argument need to form a unique key for
            the result output DataFrame. Otherwise, there are more than one score for a
            given observation.
            Types: str OR list of Strings (str)
        accumulate:
            Optional Argument.
            Specifies one or more columns from the "data" DataFrame that can be passed to
            the result output DataFrame.
            Types: str OR list of Strings (str)
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of PCAEvaluator.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        PCAEvalObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create the required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Run PCA() on columns "age", "income", "years_with_bank" and "nbr_children".
        pca_obj = valib.PCA(data=df,
                            columns=["age", "years_with_bank", "nbr_children", "income"])
        # Evaluate the PCA model created in the above step.
        obj = valib.PCAEvaluator(data=df,
                                 model=pca_obj.result,
                                 index_columns="cust_id",
                                 accumulate=["age", "years_with_bank", "nbr_children"])
        # Print the results.
        print(obj.result)
        # Example 2: Generate only SQL for the function, but do not execute the same.
        obj = valib.PCAEvaluator(data=df,
                                 model=pca_obj.result,
                                 index_columns="cust_id",
                                 accumulate=["age", "years_with_bank", "nbr_children"],
                                 gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    PCAPredict
    PCAPredict
    Functions
    PCAPredict(data, model, index_columns=None, accumulate=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        The function generates PCA scores using the model created by PCA() VALIB function.
        The scoring process expresses each component as a linear combination of the input
        columns. The result output DataFrame contains one or more index (key) columns and PCA
        score columns, one for each component.
        When PCA analysis was based on a correlation matrix, scoring input data is normalized
        by subtracting the mean and dividing by the standard deviation.
        If multiple factor models were built by means of one or more group by columns, the
        resulting score DataFrame includes these columns and score the grouped input columns
        accordingly.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data containing the columns to get PCA scores.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the teradataml DataFrame generated by VALIB PCA() function, containing
            the PCA model to use in scoring.
            Types: teradataml DataFrame
        index_columns:
            Optional Argument.
            Specifies one or more different columns for the primary index of the result
            output DataFrame. By default, the primary index columns of the result output
            DataFrame are the primary index columns of the input DataFrame "data". In
            addition, the columns specified in this argument need to form a unique key for
            the result output DataFrame. Otherwise, there are more than one score for a
            given observation.
            Types: str OR list of Strings (str)
        accumulate:
            Optional Argument.
            Specifies one or more columns from the "data" DataFrame that can be passed to
            the result output DataFrame.
            Types: str OR list of Strings (str)
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of PCAPredict.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        PCAPredObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create the required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Run PCA() on columns "age", "income", "years_with_bank" and "nbr_children".
        pca_obj = valib.PCA(data=df,
                            columns=["age", "years_with_bank", "nbr_children", "income"])
        # Get PCA scores using the model generated above.
        obj = valib.PCAPredict(data=df,
                               model=pca_obj.result,
                               index_columns="cust_id",
                               accumulate=["age", "years_with_bank", "nbr_children"])
        # Print the results.
        print(obj.result)
        # Example 2: Generate only SQL for the function, but do not execute the same.        
        obj = valib.PCAPredict(data=df,
                               model=pca_obj.result,
                               index_columns="cust_id",
                               accumulate=["age", "years_with_bank", "nbr_children"],
                               gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Fast KMeans Clustering
    KMeans
    Kmeans
    Functions
    KMeans(data, columns, centers, centroids_data=None, exclude_columns=None, max_iter=50, operator_database=None, threshold=0.001, charset=None)
    DESCRIPTION:
        The function performs fast K-Means clustering algorithm and returns cluster means
        and averages. Specifically, the rows associated with positive cluster IDs in output
        contain the average values of each of the clustered columns along with the count for
        each cluster ID. The rows associated with negative cluster IDs contain the variance
        of each of the clustered columns for each cluster ID.
        Note:
            This function is applicable only on columns containing numeric data.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data on which K-Means clustering is to be performed.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) to be used in clustering. Occasionally,
            it can also accept permitted strings to specify all columns or all numeric columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
            Types: str OR list of Strings (str)
        centers:
            Required Argument.
            Specifies the number of clusters to be contained in the cluster model.
            Types: int
        centroids_data:
            Optional Argument.
            Specifies the teradataml DataFrame containing clustering output, which is used
            as initial value for clustering algorithm, instead of using random values. If
            this argument is not specified or None, the function starts with random values.
            Types: teradataml DataFrame
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the clustering analysis.
            If 'all' or 'allnumeric' is used in the "columns" argument, this argument can
            be used to exclude specific columns from the analysis.
            Types: str OR list of Strings (str)
        max_iter:
            Optional Argument.
            Specifies the maximum number of iterations to perform during clustering.
            Default Value: 50
            Types: int
        operator_database:
            Optional Argument.
            Specifies the database where the table operators called by Vantage Analytic
            Library reside. If not specified, the library searches the standard search path
            for table operators, including the current database.
            Types: str
        threshold:
            Optional Argument.
            Specifies the value which determines if the algorithm has converged based on
            how much the cluster centroids change from one iteration to the next.
            Default Value: 0.001
            Types: float
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of KMeans.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        KMeansObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
        Note:
            If the argument "centroids_data" is specified, then "centroids_data" DataFrame
            is overwritten by the result DataFrame of KMeansObj.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create the required teradataml DataFrame.
        df = DataFrame("customer_analysis")
        print(df)
        # Example 1: Run KMeans clustering on the DataFrame 'customer_analysis' with initial
        #            random values. The function uses 'all' for "columns" argument and
        #            excludes all non-numeric columns.
        obj = valib.KMeans(data=df,
                           columns='all',
                           exclude_columns=["cust_id", "gender", "marital_status",
                                            "city_name", "state_code"],
                           centers=3)
        # Print the results.
        print(obj.result)
        # Example 2: Run KMeans clustering on the DataFrame 'customer_analysis' with
        #            pre-existing result DataFrame in "centroids_data" argument.
        # First run KMeans() with initial random values.
        obj = valib.KMeans(data=df,
                           columns=["avg_cc_bal", "avg_ck_bal", "avg_sv_bal"],
                           centers=3,
                           max_iter=5,
                           threshold=0.1)
        # Use KMeans result teradataml DataFrame (from above step) in "centroids_data" argument.
        ob1 = valib.KMeans(data=df,
                           columns=["avg_cc_bal", "avg_ck_bal", "avg_sv_bal"],
                           centers=3,
                           max_iter=10,
                           threshold=0.1,
                           centroids_data=obj.result)
        # Print the results.
        print(ob1.result)
    KMeansPredict
    KmeansPredict
    Functions
    KMeansPredict(data, model, cluster_column='clusterid', index_columns=None, accumulate=None, fallback=False, operator_database=None, charset=None)
    DESCRIPTION:
        The function performs cluster prediction on test data based on the cluster model
        generated by KMeans() VALIB function and generates a teradataml DataFrame containing
        progress report with two columns, a timestamp, and a progress message.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data for which cluster is to be predicted.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the model generated by VALIB KMeans() function.
            Types: teradataml DataFrame
        cluster_column:
            Optional Argument.
            Specifies the name of the column representing cluster identifier.
            Default Value: "clusterid"
            Types: str
        index_columns:
            Optional Argument.
            Specifies the names of one or more columns in the input teradataml DataFrame
            to use as the primary index of the scored output DataFrame.
            Types: str OR list of Strings (str)
        accumulate:
            Optional Argument.
            Specifies the names of one or more columns from the input teradataml DataFrame
            that can be passed along to the output teradataml DataFrame.
            Types: str OR list of Strings (str)
        fallback:
            Optional Argument.
            Specifies an optional flag to indicate ('True'), that the scored output
            teradataml DataFrame has the fallback attribute (that is, have a mirrored copy).
            Default Value: False
            Types: bool
        operator_database:
            Optional Argument.
            Specifies the database where the table operators called by Vantage Analytic
            Library reside. If not specified, the library searches the standard search path
            for table operators, including the current database.
            Types: str
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of KMeansPredict.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        KMeansPredObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create the required teradataml DataFrame.
        df = DataFrame("customer_analysis")
        print(df)
        # Run KMeans() first.
        kmeans_out = valib.KMeans(data=df,
                                  columns=["avg_cc_bal", "avg_ck_bal", "avg_sv_bal"],
                                  centers=3)
        # Use KMeans result teradataml DataFrame(from above step) to predict the clusters
        # for the data in DataFrame 'df'.
        obj = valib.KMeansPredict(data=df,
                                  model=kmeans_out.result,
                                  cluster_column="clusterid",
                                  index_columns="cust_id",
                                  fallback=False,
                                  accumulate=["cust_id", "age", "city_name", "state_code",
                                              "gender"])
        # Print the results.
        print(obj.result)
    Linear Regression
    LinReg
    LinReg
    Functions
    LinReg(data, columns=None, response_column=None, backward=None, backward_only=None, exclude_columns=None, cond_ind_threshold=30,
    constant=False, entrance_criterion=None, forward=None, forward_only=None, group_columns=None, matrix_input=False, near_dep_report=None,
    remove_criterion=None, stats_output=None, stepwise=False, use_fstat=True, use_pvalue=False, variance_prop_threshold=0.5, charset=None)
    DESCRIPTION:
        Linear Regression is one of the fundamental types of predictive modeling algorithms.
        In linear regression, a dependent numeric variable is expressed in terms of the sum
        of one or more independent numeric variables, which are each multiplied by a numeric
        coefficient, usually with a constant term added to the sum of independent variables.
        It is the coefficients of the independent variables together with a constant term
        that comprise a linear regression model. Applying these coefficients to the variables
        (columns) of each observation (row) in a data set is known as scoring, as described
        in Linear Regression Scoring.
        Some of the key features of Linear Regression are outlined below.
            * The Teradata table operator CALCMATRIX is used to build a DataFrame that
              represents an extended cross-products matrix that is the input to the algorithm.
            * One or more group by columns may optionally be specified so that an input
              matrix is built for each combination of group by column values, and subsequently
              a separate linear model is built for each matrix.
        To achieve this, the names of the group by columns are passed to CALCMATRIX as
        parameters, so it includes them as columns in the matrix output it creates.
            * The stepwise feature for Linear Regression is a technique for selecting the
              independent variables in a linear model. It consists of different methods of
              "trying" a variable and adding or removing it from a model by checking either
              a partial F-Statistic or the P-Value of a T-Statistic, at the user's choice.
            * The algorithm is partially scalable because the size of each input matrix
              depends only on the number of independent variables (columns) and not on the
              size of the input DataFrame. The calculations performed on the client workstation
              however are not scalable when group by columns are used, because each model is
              built serially based on each matrix in the matrix DataFrame.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to build a predictive model from.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) representing the independent variables
            used in building a linear regression model. Occasionally, it can also accept
            permitted strings to specify all columns, or all numeric columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
            Types: str OR list of Strings (str)
        response_column:
            Required Argument.
            Specifies the name of the column that represents the dependent variable.
            Types: str
        backward:
            Optional Argument.
            Specifies whether to use backward technique or not. When set to True, starting
            with all independent variables in the model, one backward step is followed by
            one forward step until no variables can be removed. A backward step consists
            of computing the Partial F-Statistic for each variable and removing that with
            the smallest value if it is less than the criterion to remove. The P-value of
            the T-Statistic may be used instead of the Partial F-Statistic. All of the
            P-values for variables in the model can be calculated at once, removing the
            variable with the largest P-Value if greater than the criterion to remove.
            Types: bool
        backward_only:
            Optional Argument.
            Specifies whether to use only backward technique or not. This technique is
            similar to the backward technique, except that a forward step is not performed.
            It starts with all independent variables in the model. Backward steps are
            executed until no more are possible.
            Types: bool
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the specific column(s) to exclude from the analysis,
            if a column specifier such as 'all', 'allnumeric' is used in the "columns"
            argument. For convenience, when the "exclude_columns" argument is used, dependent
            variable and group by columns, if any, are automatically excluded as input
            columns and do not need to be included as "exclude_columns".
            Types: str OR list of Strings (str)
        cond_ind_threshold:
            Optional Argument.
            Specifies the condition index threshold value to use while generating near
            dependency report. This is used when "near_dep_report" is set to True.
            Default Value: 30
            Types: int
        constant:
            Optional Argument.
            Specifies whether the linear model includes a constant term or not. When set
            to True, linear model includes a constant term.
            Default Value: True
            Types: bool
        entrance_criterion:
            Optional Argument.
            Specifies the criterion to enter a variable into the model. The Partial
            F-Statistic must be greater than this value, or the T-Statistic P-value must
            be less than this value, depending on the "use_fstat" or "use_pvalue" argument
            value.
            Default Value: 3.84 if "use_fstat" is True, and 0.05 if "use_pvalue" is True.
            Types: float
        forward:
            Optional Argument.
            Specifies whether to use forward technique or not. When set to True, starting
            with no independent variables in the model, a forward step is performed, adding
            the best choice in explaining the dependent variable's variance, followed by a
            backward step, removing the worst choice. A forward step is made by determining
            the largest partial F-Statistic and adding the corresponding variable to the
            model, provided the statistic is greater than the criterion to enter (see the
            "entrance_criterion").
            An alternative is to use the P-value of the T-Statistic (the ratio of a variable's
            B coefficient to its Standard Error). When the P-value is used, a forward step
            determines the variable with the smallest P-value and adds that variable if the
            P-value is less than the criterion to enter. (If more than one variable has a
            P-value of zero, the F-Statistic is used instead.)
            Types: bool
        forward_only:
            Optional Argument.
            Specifies whether to use only forward technique or not. This technique is similar
            to the forward technique, except that a backward step is not performed.
            Types: bool
        group_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) dividing the input into partitions, one
            for each combination of values in the group by columns. For each partition or
            combination of values a separate linear model is built.
            Types: str OR list of Strings (str)
        matrix_input:
            Optional Argument.
            Specifies whether the input teradataml DataFrame passed to argument "data"
            represents an ESSCP matrix build by Matrix Building function or not, refer
            "valib.Matrix()" function for more details.
            When this is set to True, the input passed to "data" argument represents an
            ESSCP matrix built by the Matrix Building function. Use of this feature saves
            internally building a matrix each time this function is performed, providing
            a significant performance improvement. The columns specified with the "columns"
            argument may be a subset of the columns in this matrix and may be specified in
            any order. The columns must, however, all be present in the matrix. Further,
            if group by columns are specified in the matrix, these same group by columns
            must be specified in this function.
            Note:
                If the input represents a saved matrix, make sure to set matrix_input=True
                because results can otherwise be unpredictable. A saved matrix may look
                like an ordinary teradataml DataFrame to this function.
            Default Value: False
            Types: bool
        near_dep_report:
            Optional Argument.
            Specifies whether to produce an XML report showing columns that may be collinear
            as part of the output or not. The report is included in the XML output only if
            collinearity is detected.
            Two threshold arguments are available for this report, "cond_ind_threshold" and
            "variance_prop_threshold".
            Types: bool
        remove_criterion:
            Optional Argument.
            Specifies the criterion to remove a variable from the model. The Partial
            F-Statistic must be less than this value, or the T-Statistic P-value must be
            greater than this value, depending on the "use_fstat" or "use_pvalue" argument
            value.
            Default Value: 3.84 if "use_fstat" is True, and 0.05 if "use_pvalue" is True.
            Types: float
        stats_output:
            Optional Argument.
            Specifies whether to produce an additional data quality report which includes
            the mean and standard deviation of each model variable, derived from an ESSCP
            matrix. The report is included in the XML output.
            Types: bool
        stepwise:
            Optional Argument.
            Specifies whether to perform a stepwise procedure or not.
            Default Value: False
            Types: bool
        use_fstat:
            Optional Argument.
            Specifies whether to use the partial F-Statistic in assessing whether a variable
            should be added or removed.
            Default Value: True
            Types: bool
        use_pvalue:
            Optional Argument.
            Specifies whether to use the T-Statistic P-value in assessing whether a variable
            should be added or removed.
            Default Value: False
            Types: bool
        variance_prop_threshold:
            Optional Argument.
            Specifies the variance proportion threshold value to use while generating near
            dependency report. This is used when "near_dep_report" is set to True.
            Default Value: 0.5
            Types: float
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of LinReg.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        LinRegObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. model
            2. statistical_measures
            3. xml_reports
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Shows how input columns 'age', 'years_with_bank', and 'nbr_children' are
        #            used to predict 'income'.
        obj = valib.LinReg(data=df,
                           columns=["age", "years_with_bank", "nbr_children"],
                           response_column="income")
        # Print the results.
        print(obj.model)
        print(obj.statistical_measures)
        print(obj.xml_reports)
        # Example 2: Shows how group by columns 'gender' and 'marital_status' result in
        #            2x4=8 models being built.
        obj = valib.LinReg(data=df,
                           columns=["age", "years_with_bank", "nbr_children"],
                           response_column="income",
                           group_columns=["gender", "marital_status"])
        # Print the results.
        print(obj.model)
        print(obj.statistical_measures)
        print(obj.xml_reports)
    LinRegEvaluator
    LinRegEvaluator
    Functions
    LinRegEvaluator(data, model, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Linear regression model evaluation begins with scoring a DataFrame that includes the
        actual values of the dependent variable. The standard error of estimate for the model is
        calculated and reported and is compared to the standard error of estimate reported when
        the model was built. The standard error of estimate is calculated as the square root of
        the average squared residual value over all the observations (as shown below):
        standard error of estimate = sqrt( sum((y-y1)**2)/(n-p-1) ),
        where
            * y1 - the actual value of the dependent variable
            * y  - the predicted value
            * n  - the number of observations
            * p  - the number of independent variables (substituting n-p in the denominator if
                   there is no constant term)
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to evaluate.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the input containing the linear model to use in evaluation. This must be
            the "model" teradataml DataFrame generated by LinReg() function from VALIB or a
            teradataml DataFrame created on a table generated by 'linear' function from
            Vantage Analytic Library.
            Types: teradataml DataFrame
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of LinRegEvaluator.
        Output teradataml DataFrames can be accessed using attribute references, such as
        LinRegEvaluatorObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Shows how linear regression model evaluation is performed.
        # First generate the model using LinReg() function from 'valib'.
        lin_reg_obj = valib.LinReg(data=df,
                                   columns=["age", "years_with_bank", "nbr_children"],
                                   response_column="income")
        # Print the linear regression model.
        print(lin_reg_obj.model)
        # Evaluate the data using the linear regression model generated above.
        obj = valib.LinRegEvaluator(data=df,
                                    model=lin_reg_obj.model)
        # Print the results.
        print(obj.result)
        # Example 2: Generate only SQL for the function, but do not execute the same.
        obj = valib.LinRegEvaluator(data=df,
                                    model=lin_reg_obj.model,
                                    gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    LinRegPredict
    LinRegPredict
    Functions
    LinRegPredict(data, model, index_columns=None, response_column=None, accumulate=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Linear Regression Scoring is the application of a Linear Regression model to an input
        data that contains the same independent variable columns contained in the model. The
        result is an output score data that minimally contains one or more key columns and
        an estimate of the dependent variable in the model.
        Some of the key features of linear scoring are outlined below.
            * If one or more group by columns are present in the input data to be scored and
              the model input data, each row in the input data to be scored is scored using
              the appropriate model in the model input data.
            * If an error such as "Constant columns detected" occurs for a particular
              combination of group by column values, the predicted value of the dependent
              column is null for any row containing that combination of group by column
              values. The error message is also placed in the column name in the model report.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to score.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the input containing the linear model to use in scoring. This must be
            the "model" teradataml DataFrame generated by LinReg() function from VALIB or a
            teradataml DataFrame created on a table generated by 'linear' function from
            Vantage Analytic Library.
            Types: teradataml DataFrame
        index_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) representing the primary index of the
            score output. By default, the primary index columns of the score output are the
            primary index columns of the input. In addition, the index columns need to form
            a unique key for the score output. Otherwise, there are more than one score for
            a given observation.
            Types: str OR list of Strings (str)
        response_column:
            Optional Argument.
            Specifies the name of the predicted value column. If not used, the name of the
            dependent column in the input is used.
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of the column(s) from the input to retain in the output.
            Types: str OR list of Strings (str)
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of LinRegPredict.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        LinRegPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Shows how linear regression model scoring is performed.
        # First generate the model using LinReg() function from 'valib'.
        lin_reg_obj = valib.LinReg(data=df,
                                   columns=["age", "years_with_bank", "nbr_children"],
                                   response_column="income")
        # Print the linear regression model.
        print(lin_reg_obj.model)
        # Score the data using the linear regression model generated above.
        obj = valib.LinRegPredict(data=df,
                                  model=lin_reg_obj.model,
                                  response_column="inc")
        # Print the results.
        print(obj.result)
        # Example 2: Generate only SQL for the function, but do not execute the same.
        obj = valib.LinRegPredict(data=df,
                                  model=lin_reg_obj.model,
                                  response_column="inc",
                                  gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Logistic Regression
    LogReg
    LogReg
    Functions
    LogReg(data, matrix_data=None, columns=None, response_column=None, backward=None, backward_only=None, exclude_columns=None,
    cond_ind_threshold=30, constant=True, convergence=0.001, entrance_criterion=0.05, forward=None, forward_only=None, group_columns=None,
    lift_output=None, max_iter=100, mem_size=None, near_dep_report=None, remove_criterion=0.05, response_value=None, sample=None, stats_output=False,
    stepwise=False, success_output=False, start_threshold=None, end_threshold=None, increment_threshold=None, threshold_output=False,
    variance_prop_threshold=0.5, charset=None)
    DESCRIPTION:
        Logistic Regression is one of the most widely used types of statistical analysis.
        In Logistic Regression, a set of independent variables (in this case, columns) is
        processed to predict the value of a dependent variable (column) that assumes two
        values referred to as response (1) and non-response (0). The user can specify which
        value of the dependent variable to treat as the response, and all other values
        assumed by the dependent variable are treated as non-repsonse. The result is not,
        however, a continuous numeric variable as seen in Linear Regression, but rather a
        probability between 0 and 1 that the response value is assumed by the dependent
        variable.
        There are many types of analysis that lend themselves to the use of Logistic Regression,
        and when scoring a model, benefit from the estimation of a probability rather than
        a fixed value. For example, when predicting who should be targeted for a marketing
        campaign, the scored customers can be ordered by the predicted probability from most
        to least likely, and the top n values taken from the customer list.
        Some of the key features of Logistic Regression are outlined below.
            * The Teradata table operator CALCMATRIX is used to build an ESSCP matrix for
              purposes of validating the input data, such as by checking for constant values.
              Also, to avoid rebuilding this matrix every time the algorithm is run, the user
              may run the Matrix Analysis separately, saving an ESSCP matrix in a teradataml
              DataFrame that can then be input to Logistic Regression.
              Refer "matrix_data" argument.
            * One or more group by columns can optionally be specified so that an input
              matrix is built for each combination of group by column values, and subsequently
              a separate Logistic Regression model is built for each matrix. To achieve
              this, the names of the group by columns are passed to CALCMATRIX as parameters,
              so it includes them as columns in the matrix data it creates.
              Refer "group_columns" argument.
            * The stepwise feature for Logistic Regression is a technique for selecting the
              independent variables in a logistic model. It consists of different methods of
              'trying' variables and adding or removing them from a model through a series of
              forward and backward steps described in the parameter section.
            * A Statistics data is available, displaying the mean and standard deviation of
              each model variable. Refer to the "stats_output" argument.
            * A Success data is available, displaying counts of predicted versus actual
              values of the dependent variable in the logistic model.
            * A Multi-Threshold Success Table is available. Refer "threshold_output" argument.
            * A Lift Table, such as would be used to build a Lift Chart, is available.
              Refer "lift_output" argument.
            * A Near Dependency Report is available to identify two or more columns that
              may be collinear.
            * The algorithm is partially scalable because the size of each input matrix
              depends only on the number of independent variables (columns) and not on the size
              of the input data. The calculations performed on the client workstation however
              are not scalable when group by columns are used, because each model is built
              serially based on each matrix in the matrix data.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to build a logistic regression model from.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) representing the independent variables
            used in building a logistic regression model. Occasionally, it can also accept
            permitted strings to specify all columns, or all numeric columns.
            Permitted Values:
                * Name(s) of the column(s) in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
            Types: str OR list of Strings (str)
        response_column:
            Required Argument.
            Specifies the name of the column that represents the dependent variable being
            predicted.
            Types: str
        backward:
            Optional Argument.
            Specifies whether to take backward steps or not. Backward steps, i.e., removing
            variables from a model, use the P-value of the T-statistic, i.e., the ratio of
            a B-coefficient to its standard error. The variable (column) with the largest
            P-value is removed if the P-value exceeds the criterion to remove.
            Types: bool
        backward_only:
            Optional Argument.
            Specifies whether to use only backward technique or not. This technique is similar
            to the backward technique, except that a forward step is not performed. It starts
            with all independent variables in the model. Backward steps are executed until no
            more are possible.
            Types: bool
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the analysis, if a column
            specifier such as 'all', 'allnumeric' is used in the "columns" argument. By
            default, when the "exclude_columns" argument is used, dependent variable and
            group by columns, if any, are automatically excluded as input columns and do not
            need to be included as "exclude_columns".
            Types: str OR list of Strings (str)
        cond_ind_threshold:
            Optional Argument.
            Specifies the condition index threshold value to use while generating near
            dependency report. This is used when "near_dep_report" is set to True.
            Default Value: 30
            Types: int
        constant:
            Optional Argument.
            Specifies whether the logistic model includes a constant term or not. When set
            to True, model includes a constant term.
            Default Value: True
            Types: bool
        convergence:
            Optional Argument.
            Specifies the convergence criterion such that the algorithm stops iterating when
            the change in log likelihood function falls below this value.
            Default Value: 0.001
            Types: float
        entrance_criterion:
            Optional Argument.
            Specifies the criterion to enter a variable into the model. The W-statistic
            chi-square P-value must be less than this value for a variable to be added.
            Default Value: 0.05
            Types: float
        forward:
            Optional Argument.
            Specifies whether to use forward technique or not. When set to True, in this
            technique, starting with no independent variables in the model, a forward step
            is performed, adding the "best" choice, followed by a backward step, removing
            the worst choice. Refer to the "stepwise" argument for a description of the
            steps in this technique.
            Types: bool
        forward_only:
            Optional Argument.
            Specifies whether to use only forward technique or not. This technique is similar
            to the forward technique, except that a backward step is not performed.
            Types: bool
        group_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) dividing the input into partitions, one
            for each combination of values in the group by columns. For each partition or
            combination of values a separate logistic model and XML report is built.
            Types: str OR list of Strings (str)
        lift_output:
            Optional Argument.
            Specifies whether to build a lift chart or not and add it in the functions output
            string. It splits up the computed probability values into deciles with the usual
            counts and percentages to demonstrate what happens when more and more rows of
            ordered probabilities are accumulated.
            Types: bool
        matrix_data:
            Optional Argument.
            Specifies the input matrix data to use for the analysis. Instead of internally
            building a matrix with the Matrix function each time this analysis is performed,
            the user may build an ESSCP Matrix once with the Matrix Analysis using Matrix()
            function. The matrix can subsequently be read from this data instead of re-building
            it each time. If this is specified, the columns specified with the "columns"
            argument should be a subset of the columns in this matrix and can be specified in
            any order. The columns must however all be present in the matrix. Further, if
            group by columns are specified in the matrix, these same group by columns must
            be specified in this analysis.
            Types: teradataml DataFrame
        max_iter:
            Optional Argument.
            Specifies the maximum number of attempts to converge on a solution.
            Default Value: 100
            Types: int
        mem_size:
            Optional Argument.
            Specifies the memory size in megabytes to allocate for in-memory Logistic
            Regression. If there is too much data to fit in this amount of memory or is set
            to 0 or argument is not specified, normal SQL processing is performed.
            Types: int
        near_dep_report:
            Optional Argument.
            Specifies whether to produce an XML report showing columns that may be
            collinear as part of the output or not. The report is included in the XML
            output only if collinearity is detected.
            Two threshold arguments are available for this report, "cond_ind_threshold" and
            "variance_prop_threshold".
            Types: bool
        remove_criterion:
            Optional Argument.
            Specifies the criterion to remove a variable from the model. The T-Statistic
            P-value must be greater than this value for a variable to be removed.
            Default Value: 0.05
            Types: float
        response_value:
            Optional Argument.
            Specifies the value assumed by the dependent column that is to be treated as
            the response value.
            Types: str
        sample:
            Optional Argument.
            Specifies whether to use sample of the data to be read into memory for processing,
            if the memory size available is less than the amount of data to process. When set
            to True, a sample of data is read.
            Types: bool
        stats_output:
            Optional Argument.
            Specifies whether an optional data quality report should be delivered in the
            function's XML output string or not, which includes the mean and standard
            deviation of each model variable, derived from an ESSCP matrix.
            Default Value: False
            Types: bool
        stepwise:
            Optional Argument.
            Specifies whether to perform a stepwise procedure or not.
            Forward steps, i.e., adding variables to a model, add the variable with the
            smallest chi-square P-value connected to its special W-statistic, provided the
            P-value is less than the criterion to enter.
            Backward steps, i.e., removing variables from a model, use the P-value of the
            T-statistic, i.e., the ratio of a B-coefficient to its standard error. The
            variable (column) with the largest P-value is removed if the P-value exceeds
            the criterion to remove.
            Default Value: False
            Types: bool
        success_output:
            Optional Argument.
            Specifies whether an optional success report should be delivered in the function's
            XML output string or not, which includes the displaying counts of predicted
            versus actual values of the dependent variable of the logistic regression model.
            This report is similar to the Decision Tree Confusion Matrix, but the success
            report only includes two values of the dependent variable, namely response versus
            non-response.
            Default Value: False
            Types: bool
        start_threshold:
            Optional Argument.
            Specifies the beginning threshold value utilized in the Multi-Threshold Success
            output.
            Types: float, int
        end_threshold:
            Optional Argument.
            Specifies the ending threshold value utilized in the Multi-Threshold Success output.
            Types: float, int
        increment_threshold:
            Optional Argument.
            Specifies the difference in threshold values between adjacent rows in the
            Multi-Threshold Success output.
            Types: float, int
        threshold_output:
            Optional Argument.
            Specifies whether the Multi-Threshold Success output should be produced or not
            and included in the XML output string in the result. This report can be thought
            of as a table where each row is a Prediction Success Table, and each row has a
            different threshold value as generated by the "start_threshold", "end_threshold",
            and "increment_threshold" arguments. What is meant by a threshold here is the
            value above which the predicted probability indicates a response.
            Default Value: False
            Types: bool
        variance_prop_threshold:
            Optional Argument.
            Specifies the variance proportion threshold value to use while generating near
            dependency report. This is used when "near_dep_report" is set to True.
            Default Value: 0.5
            Types: float
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of LogReg.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        LogRegObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. model
            2. statistical_measures
            3. xml_reports
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Shows the Near Dependency Report is requested with related options.
        obj = valib.LogReg(data=df,
                           columns=["age", "years_with_bank", "income"],
                           response_column="nbr_children",
                           response_value=1,
                           cond_ind_threshold=3,
                           variance_prop_threshold=0.3)
        # Print the results.
        print(obj.model)
        print(obj.statistical_measures)
        print(obj.xml_reports)
        # Example 2: Shows that 2 group by columns are requested. The output contains 1 row
        #            for each combination of group by column values.
        obj = valib.LogReg(data=df,
                           columns=["age", "years_with_bank", "income"],
                           response_column="nbr_children",
                           group_columns=["gender", "marital_status"])
        # Print the results.
        print(obj.model)
        print(obj.statistical_measures)
        print(obj.xml_reports)
        # Example 3: Shows how a pre-built matrix can be used for generating logistic
        #            regression model.
        # Generate the ESSCP matrix.
        mat_obj = valib.Matrix(data=df,
                               columns=["income", "age", "years_with_bank", "nbr_children"],
                               type="esscp")
        # Print the results.
        print(mat_obj.result)
        # Use the generated matrix in building logistic regression model.
        obj = valib.LogReg(data=df,
                           columns=["age", "years_with_bank", "income"],
                           response_column="nbr_children",
                           response_value=1,
                           matrix_data=mat_obj.result)
        # Print the results.
        print(obj.model)
        print(obj.statistical_measures)
        print(obj.xml_reports)
    LogRegEvaluator
    LogRegEvaluator
    Functions
    LogRegEvaluator(data, model, estimate_column=None, index_columns=None, prob_column=None, accumulate=None, prob_threshold=0.5,
    start_threshold=None, end_threshold=None, increment_threshold=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Logistic Regression function model can be passed to this function to generate evaluation
        reports. Function produces the result containing the following reports in XML format:
            * Success result - This output is delivered in the function's XML output string,
              displaying counts of predicted versus actual values of the dependent variable
              of the logistic regression model. This report is similar to the Decision Tree
              Confusion Matrix, but the Success output only includes two values of the
              dependent variable, namely response versus non-response.
            * Multi - Threshold Success result - This output is delivered in the function's XML
              output string. Report can be thought of as a table where each row is a Prediction
              Success Output, and each row has a different threshold value as generated by the
              "start_threshold", "end_threshold", and "increment_threshold" arguments. What
              is meant by a threshold here is the value above which the predicted probability
              indicates a response.
            * Lift result - Result containing information, such as would be required to build
              a lift chart is available. It splits up the computed probability values into
              deciles with the usual counts and percentages to demonstrate what happens when
              more and more rows of ordered probabilities are accumulated. It is delivered in
              the function's XML output string.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to evaluate.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the input containing the logistic model to use in scoring. This must
            be the "model" teradataml DataFrame generated by LogReg() function from VALIB or
            a teradataml DataFrame created on a table generated by 'logistic' function from
            Vantage Analytic Library.
            Types: teradataml DataFrame
        estimate_column:
            Required Argument.
            Specifies the name of a column in the score output containing the estimated value
            of the dependent variable (column).
            Notes:
                1. Either "estimate_column" or "prob_column" must be requested.
                2. If the estimate column is not unique in the score output, '_tm_' is
                   automatically placed in front of the name.
            Types: str
        index_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) representing the primary index of the
            score output. By default, the primary index columns of the score output are
            the primary index columns of the input. In addition, the index columns need
            to form a unique key for the score output. Otherwise, there are more than one
            score for a given observation.
            Types: str OR list of Strings (str)
        prob_column:
            Optional Argument.
            Specifies the name of a column in the score output containing the probability
            that the dependent value is equal to the response value.
            Notes:
                1. Either "estimate_column" or "prob_column" must be requested.
                2. If the probability column is not unique in the score output, '_tm_' is
                   automatically placed in front of the name.
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of the column(s) from the input to retain in the output.
            Types: str OR list of Strings (str)
        prob_threshold:
            Optional Argument.
            Specifies the probability threshold value. When the probability of the dependent
            variable being 1 is greater than or equal to this value, the estimated value of
            the dependent variable is 1. If less than this value, the estimated value is 0.
            Default Value: 0.5
            Types: float, int
        start_threshold:
            Optional Argument.
            Specifies the beginning threshold value utilized in the Multi-Threshold Success output.
            Types: float, int
        end_threshold:
            Optional Argument.
            Specifies the ending threshold value utilized in the Multi-Threshold Success output.
            Types: float, int
        increment_threshold:
            Optional Argument.
            Specifies the difference in threshold values between adjacent rows in the
            Multi-Threshold Success output.
            Types: float, int
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of LogRegEvaluator.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        LogRegEvaluatorObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Shows how evaluation on logistic model can be performed.
        # Generate a logistic model.
        log_reg_obj = valib.LogReg(data=df,
                                   columns=["age", "years_with_bank", "income"],
                                   response_column="nbr_children",
                                   response_value=0)
        # Print the model.
        print(log_reg_obj.model)
        # Evaluate the model generated above.
        obj = valib.LogRegEvaluator(data=df,
                                    model=log_reg_obj.model,
                                    prob_column="Probability")
        # Print the results.
        print(obj.result)
        # Example 2: Generate only SQL for the function, but do not execute the same.
        obj = valib.LogRegEvaluator(data=df,
                                    model=log_reg_obj.model,
                                    gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    LogRegPredict
    LogRegPredict
    Functions
    LogRegPredict(data, model, estimate_column=None, index_columns=None, prob_column=None, accumulate=None, prob_threshold=0.5, gen_sql_only=False,
    charset=None)
    DESCRIPTION:
        Logistic Regression function model can be passed to a Logistic Regression Scoring
        function to create a score output containing predicted values of the dependent variable.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to score.
            Types: teradataml DataFrame
        model:
            Required Argument.
            Specifies the input containing the logistic model to use in scoring. This must
            be the "model" teradataml DataFrame generated by LogReg() function from VALIB or
            a teradataml DataFrame created on a table generated by 'logistic' function from
            Vantage Analytic Library.
            Types: teradataml DataFrame
        estimate_column:
            Required Argument.
            Specifies the name of a column in the score output containing the estimated value
            of the dependent variable (column).
            Notes:
                1. Either "estimate_column" or "prob_column" must be requested.
                2. If the estimate column is not unique in the score output, '_tm_' is
                   automatically placed in front of the name.
            Types: str
        index_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) representing the primary index of the
            score output. By default, the primary index columns of the score output are the
            primary index columns of the input. In addition, the index columns need to form
            a unique key for the score output. Otherwise, there are more than one score for
            a given observation.
            Types: str OR list of Strings (str)
        prob_column:
            Optional Argument.
            Specifies the name of a column in the score output containing the probability
            that the dependent value is equal to the response value.
            Notes:
                1. Either "estimate_column" or "prob_column" must be requested.
                2. If the probability column is not unique in the score output, '_tm_' is
                   automatically placed in front of the name.
            Types: str
        accumulate:
            Optional Argument.
            Specifies the name(s) of the column(s) from the input to retain in the output.
            Types: str OR list of Strings (str)
        prob_threshold:
            Optional Argument.
            Specifies the probability threshold value. When the probability of the dependent
            variable being 1 is greater than or equal to this value, the estimated value of
            the dependent variable is 1. If less than this value, the estimated value is 0.
            Default Value: 0.5
            Types: float, int
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of LogRegPredict.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        LogRegPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Shows how logistic model scoring can be performed.
        # Generate a logistic model.
        log_reg_obj = valib.LogReg(data=df,
                                   columns=["age", "years_with_bank", "income"],
                                   response_column="nbr_children",
                                   response_value=0)
        # Print the model.
        print(log_reg_obj.model)
        # Score using the model generated above.
        obj = valib.LogRegPredict(data=df,
                                  model=log_reg_obj.model,
                                  prob_column="Probability")
        # Print the results.
        print(obj.result)
        # Example 2: Generate only SQL for the function, but do not execute the same.
        obj = valib.LogRegPredict(data=df,
                                    model=log_reg_obj.model,
                                    prob_column="Probability",
                                    gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Matrix Building
    Matrix
    Matrix
    Functions
    Matrix(data, columns=None, exclude_columns=None, group_columns=None, matrix_output='columns', type='ESSCCP', handle_nulls='IGNORE', ﬁlter=None,
    charset=None)
    DESCRIPTION:
        Matrix builds an extended sum-of-squares-and-cross-products (ESSCP) matrix or other
        derived matrix type from a teradataml DataFrame. Matrix does this with the help of
        Teradata CALCMATRIX table operator provided in Teradata Vantage. The purpose of
        building a matrix depends on the type of matrix built. For example, when a correlation
        matrix is built, view it to determine the correlations or relationships between
        the various columns in the matrix.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to build matrix from.
            Types: teradataml DataFrame
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) used in building one or more matrices.
            Occasionally, it can also accept permitted strings to specify all columns, or all
            numeric columns.
            Note:
                Do not use the following column names, as these are reserved for use by the
                CALCMATRIX table operator:
                    'rownum', 'rowname', 'c', or 's'.
            Permitted Values:
                * Name(s) of the columns in "data".
                * Pre-defined strings:
                    * 'all' - all columns
                    * 'allnumeric' - all numeric columns
            Types: str OR list of Strings (str)
        exclude_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) to exclude from the analysis, if a column
            specifier such as 'all', 'allnumeric' is used in the "columns" argument.
            For convenience, when the "exclude_columns" argument is used, dependent variable
            and group by columns, if any, are automatically excluded as input columns and do
            not need to be included in this argument.
            Types: str OR list of Strings (str)
        group_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) in input teradataml DataFrame to build a
            separate matrix for each combination. If specified, group by columns divide the
            input into parts, one for each combination of values in the group by columns. For
            each combination of values, a separate matrix is built, though they are all stored
            in the same output.
            Note:
                Do not use the following column names, as these are reserved for use by the
                CALCMATRIX table operator:
                    'rownum', 'rowname', 'c', or 's'.
            Types: str OR list of Strings (str)
        matrix_output:
            Optional Argument.
            Specifies the type of matrix output. Matrix output can either be returned as
            COLUMNS in an output teradataml DataFrame or as VARBYTE values, one per column,
            in a reduced output teradataml DataFrame.
            Permitted Values: 'columns', 'varbyte'
            Default Value: 'columns'
            Types: str
        type:
            Optional Argument.
            Specifies the type of matrix to build.
            Permitted Values:
                * 'SSCP' - sum-of-squares-and-cross-products matrix
                * 'ESSCP' - Extended-sum-of-squares-and-cross-products matrix
                * 'CSSCP' - Corrected-sum-of-squares-and-cross-products matrix
                * 'COV' - Covariance matrix
                * 'COR' - Correlation matrix
            Default Value: 'ESSCP'
            Types: str
        handle_nulls:
            Optional Argument.
            Specifies a way to treat null values in selected columns. When set to IGNORE,
            the row that contains the NULL value in a selected column is omitted from
            processing. When set to ZERO, the NULL value is replaced with zero (0) in
            calculations.
            Permitted Values: 'IGNORE', 'ZERO'
            Default Value: 'IGNORE'
            Types: str
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for building the matrix.
            For example,
                filter = "cust_id > 0"
            Types: str
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of Matrix.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        MatrixObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Example 1: Build a 3-by-3 ESSCP matrix on input columns 'age', 'years_with_bank',
        #            and 'nbr_children'.
        obj = valib.Matrix(data=df,
                           columns=["age", "years_with_bank", "nbr_children"])
        # Print the results.
        print(obj.result)
        # Example 2: Build a 3-by-3 CSSCP matrix on input columns 'age', 'years_with_bank',
        #            and 'nbr_children' with null handling, where NULLs are replaced with zero.
        obj = valib.Matrix(data=df,
                           columns=["age", "years_with_bank", "nbr_children"],
                           handle_nulls="zero",
                           type="CSSCP")
        # Print the results.
        print(obj.result)
        # Example 3: Build a 3-by-3 COR matrix by limiting the input data by filtering
        #            rows. Matrix is built on input columns 'age', 'years_with_bank',
        #            and 'nbr_children'.
        obj = valib.Matrix(data=df,
                           columns=["age", "years_with_bank", "nbr_children"],
                           filter="nbr_children > 1",
                           type="COR")
        # Print the results.
        print(obj.result)
        # Example 4: Build two 3-by-3 COV matrices by grouping data on "gender" column.
        obj = valib.Matrix(data=df,
                           columns=["age", "years_with_bank", "nbr_children"],
                           group_columns="gender",
                           type="COV")
        # Print the results.
        print(obj.result)
    Report
    XmlToHtmlReport
    XmlToHtmlReport
    Functions
    XmlToHtmlReport(data, analysis_type, ﬁlter=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Several Analytic Algorithms and Scoring functions output columns of XML.
        This function transforms the XML to HTML, which is easier to view.
        Function outputs the HTML in a column of type Teradata XML. You can copy
        the HTML into a text file, give the text file a name ending in ".html",
        and view it in a browser.
        Alternatively, you can view the HTML in a Jupyter notebook by
        double-clicking the cell that contains HTML to display it in web view.
        Note:
            This function is available in Vantage Analytic Library 2.0.0.3 or
            later.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to run statistical tests.
            Types: teradataml DataFrame
        analysis_type:
            Required Argument.
            Specifies the class name of the function or a SQL function name
            whose XML you want the function to translate to HTML.
            Function outputs different reports in HTML format depending on
            the values passed to the argument.
            Permitted Values:
                * "decisiontree", "DecisionTree".
                  Outputs HTML reports for:
                      * Decision Tree Summary.
                      * Variables.
                      * Text Tree.
                * "decisiontreescore", "DecisionTreeEvaluator".
                  Outputs HTMl reports for:
                      * Decision Tree Scoring Summary—Confusion Matrix.
                * "factor", "PCA".
                  Outputs HTMl reports for:
                      * Factor Analysis Summary.
                      * Variable Statistics.
                      * Eigenvalues.
                      * PCLoadings.
                      * Variance and Absolute Difference.
                      * Near Dependency Report.
                      * Group-by Columns.
                * "factorscore", "PCAEvaluator".
                  Outputs HTMl reports for:
                      * Factor Analysis Scoring Summary.
                * "linear", "LinReg".
                  Outputs HTMl reports for:
                      * Linear Regression Summary.
                      * Group-by Columns.
                      * Near Dependency Report.
                * "logistic", "LogReg".
                  Outputs HTMl reports for:
                      * Prediction Success Table.
                      * Multithreshold Success Table.
                      * Lift Table.
                      * Near Dependency Report.
                      * Group-by Columns.
                      * Variable Statistics.
                * "logisticScore", "LogRegEvaluator".
                  Outputs HTMl reports for:
                      * Prediction Success Table.
                      * Multithreshold Success Table.
                      * Lift Table.
            Types: str
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for analysis.
            For example,
                  filter = "cust_id > 0"
            This parameter is useful only when the function "analysis_type"
            (which outputs the input table name for the report function)
            specified its "group_columns" argument and generated multiple
            XML models.
            Multiple XML models in input table name can cause the function
            to run out of memory. In that case, limit the rows selected for
            analysis with this argument.
            Type: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
        RETURNS:
            An instance of XmlToHtmlReport.
            Output teradataml DataFrames can be accessed using attribute
            references, such as XmlToHtmlReport.<attribute_name>.
            Output teradataml DataFrame attribute name is: result.
        RAISES:
            TeradataMlException, TypeError, ValueError.
        EXAMPLES:
            # Notes:
            #   1. To execute Vantage Analytic Library functions,
            #       a. import "valib" object from teradataml.
            #       b. set 'configure.val_install_location' to the database name where Vantage
            #          analytic library functions are installed.
            #   2. Datasets used in these examples can be loaded using Vantage Analytic Library
            #      installer.
            # Import valib object from teradataml to execute this function.
            from teradataml import valib
            # Set the 'configure.val_install_location' variable.
            from teradataml import configure
            configure.val_install_location = "SYSLIB"
            # Create required teradataml DataFrame.
            df = DataFrame("customer")
            print(df)
            # Example 1: Shows the Near Dependency Report is requested with
            related options.
            obj = valib.LogReg(data=df,
                           columns=["age", "years_with_bank", "income"],
                           response_column="nbr_children",
                           response_value=1,
                           cond_ind_threshold=3,
                           variance_prop_threshold=0.3)
            # Print the results.
            print(obj.model)
            print(obj.statistical_measures)
            print(obj.xml_reports)
            obj = valib.XmlToHtmlReport(data=obj.model, analysis_type="logistic")
            # Print te result.
            print(obj.result)
            # Example 2: Generate only SQL for the function, but do not execute the same.
            obj = valib.XmlToHtmlReport(data=logreg_obj.model, 
                                    analysis_type="logistic",
                                    gen_sql_only=True)
            # Print the generated SQL.
            print(obj.show_query("sql"))
            # Print both generated SQL and stored procedure call.
            print(obj.show_query("both"))
            # Print the stored procedure call.
            print(obj.show_query())
            print(obj.show_query("sp"))
    Statistical Tests
    Statistical Tests Header
    Statistical tests provide a means of testing whether the outcome of an experiment could have
    been accidental. Analytics Library includes the following hypothesis tests, categorized as
    follows:
     -----------------------------------------------------------------------------------------------
    |           Category            |                           Test Names                          |
     -----------------------------------------------------------------------------------------------
    | Parametric Tests              | * Two Sample T-Test for Equal Means                           |
    |                               |       * T Paired                                              |
    |                               |       * T Unpaired                                            |
    |                               |       * T Unpaired with Indicator                             |
    |                               | * F-Test - N-Way                                              |
    |                               | * F-Test/Analysis of Variance (Two Way Unequal Sample Size)   |
     -----------------------------------------------------------------------------------------------
    | Binomial Tests                | * Binomial/Ztest                                              |
    |                               | * Binomial Sign Test                                          |
     -----------------------------------------------------------------------------------------------
    | Tests based on Contingency    | * Chi Square Test                                             |
    |                               | * Median Test                                                 |
     -----------------------------------------------------------------------------------------------
    | Kolmogorov/ Smirnoff Tests    | * Kolmogorov/Smirnoff Test (One Sample)                       |
    |                               | * Lilliefors Test                                             |
    |                               | * Shapiro-Wilk Test                                           |
    |                               | * D'Agostino and Pearson Test                                 |
    |                               | * Smirnov Test                                                |
     -----------------------------------------------------------------------------------------------
    | Rank Tests                    | * Mann-Whitney/Kruskal-Wallis Test                            |
    |                               | * Mann-Whitney/Kruskal-Wallis Independent Tests               |
    |                               | * Wilcoxon Signed Ranks Test                                  |
    |                               | * Friedman Test with Kendall's Coefficient of Concordance &   |
    |                               |   Spearman's Rho                                              |
     -----------------------------------------------------------------------------------------------
    Note:
        Date types are not a standard numeric data type and as such may not function properly 
        in statistical tests.
    BinomialTest
    BinomialTest
    Functions
    BinomialTest(data, ﬁrst_column=None, binomial_prob=0.5, exact_matches='negative', fallback=False, group_columns=None, allow_duplicates=False,
    second_column=None, single_tail=False, stats_database=None, style='binomial', probability_threshold=0.05, gen_sql_only=False, charset=None)
    DESCRIPTION:
        In a binomial test, there are assumed to be N independent trials, each with two
        possible outcomes, each of equal probability. You can choose to perform a binomial
        test, in which the sign of the difference between a first and second column is
        analyzed, or a sign test, in which the sign of a single column is analyzed. In a
        binomial test, user can choose to use a probability different from the default value,
        whereas in a sign test, the binomial probability is fixed at 0.5.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to run statistical tests.
            Types: teradataml DataFrame
        first_column:
            Required Argument.
            Specifies the name of the column to analyze.
            Types: str
        binomial_prob:
            Optional Argument.
            Specifies the binomial probability to use for Binomial Test.
            Note:
                This is not available to use with sign test.
            Default Value: 0.5
            Types: float
        exact_matches:
            Optional Argument.
            Specifies the category to place exact matches in.
            Note:
                This is not allowed with sign test.
            Permitted Values:
                * 'zero' - exact match is discarded.
                * 'positive' - match is placed with values greater than or equal to zero.
                * 'negative' - match is placed with values less than or equal to zero.
            Default Value: 'negative'
            Types: str
        fallback:
            Optional Argument.
            Specifies whether the FALLBACK is requested as in the output result or not.
            Default Value: False (Not requested)
            Types: bool
        group_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) for grouping so that a separate result
            is produced for each value or combination of values in the specified column or
            columns.
            Types: str OR list of Strings (str)
        allow_duplicates:
            Optional Argument.
            Specifies whether duplicates are allowed in the output or not.
            Default Value: False
            Types: bool
        second_column:
            Required argument for binomial test.
            Specifies the name of the column representing the second variable to analyze.
            Note:
                This is not allowed with sign test.
            Types: str
        single_tail:
            Optional Argument.
            Specifies whether to request single-tailed test or not. When set to True, a
            single-tailed test is requested. Otherwise, a two-tailed test is requested.
            Note:
                If the binomial probability is not 0.5, "single_tail" must be set to True.
            Default Value: False
            Types: bool
        stats_database:
            Optional Argument.
            Specifies the database where the statistical test metadata tables are installed.
            If not specified, the source database is searched for these metadata tables.
            Types: str
        style:
            Optional Argument.
            Specifies the test style.
            Permitted Values: 'binomial', 'sign'
            Default Value: 'binomial'
            Types: str
        probability_threshold:
            Optional Argument.
            Specifies the threshold probability, i.e., "alpha" probability, below which the
            null hypothesis is rejected.
            Default Value: 0.05
            Types: float
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of BinomialTest.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        BinomialTestObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        #   3. The Statistical Test metadata tables must be loaded into the database where
        #      Analytics Library is installed.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        custanly = DataFrame("customer_analysis")
        print(custanly)
        # Example 1: A binomial test without any grouping.
        obj = valib.BinomialTest(data= custanly,
                                 first_column="avg_sv_bal",
                                  second_column="avg_ck_bal")
        # Print the results.
        print(obj.result)
        # Example 2: A binomial test with grouping done by gender.
        obj = valib.BinomialTest(data= custanly,
                                 first_column="avg_sv_bal",
                                 second_column="avg_ck_bal",
                                 group_columns="gender")
        # Print the results.
        print(obj.result)
        # Example 3: A sign test without any grouping.
        obj = valib.BinomialTest(data= custanly,
                                 first_column="avg_sv_bal",
                                 style="sign")
        # Print the results.
        print(obj.result)
        # Example 4: A sign test with grouping done by gender.
        obj = valib.BinomialTest(data= custanly,
                                 first_column="avg_sv_bal",
                                 style="sign",
                                 group_columns="gender")
        # Print the results.
        print(obj.result)
        # Example 5: Generate only SQL for the function, but do not execute the same.
        obj = valib.BinomialTest(data= df,
                                 first_column="avg_sv_bal",
                                 group_columns="gender",
                                 second_column="avg_ck_bal",
                                 stats_database="alice",
                                 gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    ChiSquareTest
    ChiSquareTest
    Functions
    ChiSquareTest(data, dependent_column=None, columns=None, fallback=False, ﬁrst_columns=None, group_columns=None, allow_duplicates=False,
    second_columns=None, stats_database=None, style='chisq', probability_threshold=0.05, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Statistical tests of this type are based on a matrix of frequencies or counts.
        A frequency pattern that is non-random is sought in the matrix. Supported tests
        of this type include the following:
            * Chi Square Test - Besides a Chi Square value, other measures are computed
                                in a Chi Square Test, including a Phi Coefficient, Cramer's V,
                                Likelihood Ratio Chi Square, Continuity-Adjusted Chi Square,
                                and Contingency Coefficient.
            * Median Test - A Median Test is a variation of Chi Square Test wherein samples
                            are tested to see if their populations have the same median value.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to run statistical tests.
            Types: teradataml DataFrame
        dependent_column:
            Optional Argument.
            Specifies the name of the numeric column representing dependent variable.
            Note:
                Used only by the Median Test.
            Types: str
        columns:
            Optional Argument.
            Specifies the name(s) of the categorical column(s) representing independent variables.
            Note:
                Used only by the Median Test.
            Types: str OR list of Strings (str)
        fallback:
            Optional Argument.
            Specifies whether the FALLBACK is requested as in the output result or not.
            Default Value: False (Not requested)
            Types: bool
        first_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) representing the first of variable pairs
            for analysis.
            Notes:
                1. Used only by the Chi Square Test.
                2. The number of combinations of "first_columns" and "second_columns" may
                   not exceed 100.
                3. If the product of the number distinct values in these column pairs
                   exceeds 2000, the analysis of that combination is skipped.
            Types: str OR list of Strings (str)
        group_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) for grouping so that a separate result
            is produced for each value or combination of values in the specified column or
            columns.
            Notes:
                Argument can only be used for Median Test.
            Types: str OR list of Strings (str)
        allow_duplicates:
            Optional Argument.
            Specifies whether duplicates are allowed in the output or not.
            Default Value: False
            Types: bool
        second_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) representing the second of variable pairs
            for analysis.
            Notes:
                1. Used only by the Chi Square Test.
                2. The number of combinations of "first_columns" and "second_columns" may not
                   exceed 100.
                3. If the product of the number distinct values in these column pairs exceeds
                   2000, the analysis of that combination is skipped.
            Types: str OR list of Strings (str)
        stats_database:
            Optional Argument.
            Specifies the database where the statistical test metadata tables are installed.
            If not specified, the source database is searched for these metadata tables.
            Types: str
        style:
            Optional Argument.
            Specifies the test style.
            Permitted Values:
                * 'chisq' - Chi Square test.
                * 'median' - Median test.
            Default Value: 'chisq'
            Types: str
        probability_threshold:
            Optional Argument.
            Specifies the threshold probability, i.e., "alpha" probability, below which the
            null hypothesis is rejected.
            Default Value: 0.05
            Types: float
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of ChiSquareTest.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        ChiSquareTestObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        #   3. The Statistical Test metadata tables must be loaded into the database where
        #      Analytics Library is installed.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        custanly = DataFrame("customer_analysis")
        print(custanly)
        # Example 1: Shows a Chi Square test execution.
        obj = valib.ChiSquareTest(data= custanly,
                                 first_columns=["female", "single"],
                                 second_columns=["svacct", "ccacct", "ckacct"], style="chisq")
        # Print the results.
        print(obj.result)
        # Example 2: Shows a Median test execution with group-by option.
        obj = valib.ChiSquareTest(data= custanly,
                                 dependent_column="income",
                                 columns="marital_status",
                                 group_columns="years_with_bank",
                                 style="median",
                                 probability_threshold=0.01)
        # Print the results.
        print(obj.result)
        # Example 3: Generate only SQL for the function, but do not execute the same.
        obj = valib.ChiSquareTest(data= df,
                                 first_columns=["female", "single"],
                                 second_columns=["svacct", "ccacct", "ckacct"],
                                 style="chisq",
                                 gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    KSTest
    KSTest
    Functions
    KSTest(data, dependent_column=None, columns=None, fallback=False, group_columns=None, allow_duplicates=False, stats_database=None, style='ks',
    probability_threshold=0.05, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Statistical tests of this type attempt to determine the likelihood that two
        distribution functions represent the same distribution.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to run statistical tests.
            Types: teradataml DataFrame
        dependent_column:
            Required Argument.
            Specifies the name of the numeric column that is tested to have a normal distribution.
            Types: str
        columns:
            Optional Argument.
            Specifies a categorical variable with two values that indicate the distribution
            to which the "dependent_column" belongs.
            Note:
                Used only by the Smirnov test.
            Types: str OR list of Strings (str)
        fallback:
            Optional Argument.
            Specifies whether the FALLBACK is requested as in the output result or not.
            Default Value: False (Not requested)
            Types: bool
        group_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) for grouping so that a separate result
            is produced for each value or combination of values in the specified column or
            columns.
            Types: str OR list of Strings (str)
        allow_duplicates:
            Optional Argument.
            Specifies whether duplicates are allowed in the output or not.
            Default Value: False
            Types: bool
        stats_database:
            Optional Argument.
            Specifies the database where the statistical test metadata tables are installed.
            If not specified, the source database is searched for these metadata tables.
            Types: str
        style:
            Optional Argument.
            Specifies the test style.
            Permitted Values:
                * 'ks' - Kolmogorov-Smirnov test.
                * 'l' - Lilliefors test.
                * 'sw' - Shapiro-Wilk test.
                * 'p' - D'Agostino and Pearson test.
                * 's' - Smirnov test.
            Default Value: 'ks'
            Types: str
        probability_threshold:
            Optional Argument.
            Specifies the threshold probability, i.e., "alpha" probability, below which
            the null hypothesis is rejected.
            Default Value: 0.05
            Types: float
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of KSTest.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        KSTestObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        #   3. The Statistical Test metadata tables must be loaded into the database where
        #      Analytics Library is installed.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        custanly = DataFrame("customer_analysis")
        print(custanly)
        # Example 1: A Kolmogorov-Smirnov test with group-by option.
        obj = valib.KSTest(data=custanly,
                           dependent_column="income",
                           group_columns="years_with_bank",
                           style="ks")
        # Print the results.
        print(obj.result)
        # Example 2: A Lilliefors test with group-by option.
        obj = valib.KSTest(data=custanly,
                           dependent_column="income",
                           group_columns="years_with_bank",
                           style="l")
        # Print the results.
        print(obj.result)
        # Example 3: A Shapiro-Wilk test with group-by option.
        obj = valib.KSTest(data=custanly,
                           dependent_column="income",
                           group_columns="years_with_bank",
                           style="sw")
        # Print the results.
        print(obj.result)
        # Example 4: A D'Agostino and Pearson test with group-by option.
        obj = valib.KSTest(data=custanly,
                           dependent_column="income",
                           group_columns="years_with_bank",
                           style="p")
        # Print the results.
        print(obj.result)
        # Example 5: A Smirnov test with group-by option.
        obj = valib.KSTest(data=custanly,
                           dependent_column="income",
                           columns="gender",
                           group_columns="years_with_bank",
                           style="s")
        # Print the results.
        print(obj.result)
        # Example 6: Generate only SQL for the function, but do not execute the same.
        obj = valib.KSTest(data=df,
                           dependent_column="income",
                           group_columns="years_with_bank",
                           style="sw",
                           gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    ParametricTest
    ParametricTest
    Functions
    ParametricTest(data, columns=None, dependent_column=None, equal_variance=False, fallback=False, ﬁrst_column=None, ﬁrst_column_values=None,
    group_columns=None, allow_duplicates=False, paired=False, second_column=None, second_column_values=None, stats_database=None, style='t',
    probability_threshold=0.05, with_indicator=False, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Parametric tests make assumptions about the data, such as the observations being
        normally distributed. This can be verified with a test of normality prior to executing
        a parametric test. Both T-Tests and F-Tests are provided. T-Tests can be either paired
        or unpaired, while the unpaired T-Tests can be with or without an indicator variable.
        F-Tests can be 1-way, 2-way or 3-way. 2-way tests can have equal or unequal cell
        counts (count of rows having a combination of distinct column values), while the 3-way
        test must have equal cell counts. A 1-way test has 1 independent input column, a 2-way
        test has 2 independent columns and a 3-way test has 3 independent columns in addition
        to a dependent "column of interest".
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to run statistical tests.
            Types: teradataml DataFrame
        columns:
            Optional Argument.
            Specifies the name(s) of the column(s) representing independent variables to be
            analyzed in a F-Test N-Way with Equal Cell Counts analysis. There can be 1, 2 or
            3 columns listed in this parameter. If 2 or 3 columns, cell counts (the count of
            rows having a combination of distinct column values) should be the same.
            Types: str OR List of Strings (str)
        dependent_column:
            Optional Argument.
            Specifies the name of the column representing the dependent variable in an F-Test.
            Types: str
        equal_variance:
            Optional Argument.
            Specifies whether the variance of the two samples (columns) is assumed to be equal.
            The default assumption is that the variances are not equal.
            Note:
                This is available to use with 'T-test'.
            Default Value: False
            Types: str
        fallback:
            Optional Argument.
            Specifies whether the FALLBACK is requested as in the output result or not.
            Default Value: False (Not requested)
            Types: bool
        first_column:
            Optional Argument.
            Specifies the name of the column representing the first variable to analyze for a
            T-test. For an F-Test, specifies the name of the column representing the first
            independent variable in the analysis.
            Types: str
        first_column_values:
            Optional Argument. Required for a 2-way F-Test with Unequal Cell Counts.
            Specifies a list of the "first_column" values to be included in the analysis.
            Types: int, float, str OR List of Integers, Floats or Strings
        group_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) for grouping so that a separate result
            is produced for each value or combination of values in the specified column or
            columns.
            Note:
                This option is not available for an F 2-way analysis.
            Types: str OR list of Strings (str)
        allow_duplicates:
            Optional Argument.
            Specifies whether duplicates are allowed in the output or not.
            Default Value: False
            Types: bool
        paired:
            Optional Argument.
            Specifies whether the first and second column values are matched with each other.
            When set to True, the mean difference is also analyzed.
            Note:
                This is an option for T-Test.
            Default Value: False
            Types: bool
        second_column:
            Optional Argument.
            Specifies the name of the column representing the second variable to analyze.
            If the "with_indicator" argument is set to True, the second column is used to
            define two analysis categories, one where the second column is negative or zero,
            and another where the second column is positive.
            For an F-Test, specifies the name of the column representing the second independent
            variable in the analysis.
            Note:
                Date Type is not allowed to be used for the paired T-Test.
            Types: str
        second_column_values:
            Optional Argument. Required for a 2-way F-Test with Unequal Cell Counts.
            Specifies a list of the "second_column" values to be included in the analysis.
            Types: int, float, str OR List of Integers, Floats or Strings
        stats_database:
            Optional Argument.
            Specifies the database where the statistical test metadata tables are installed.
            If not specified, the source database is searched for these metadata tables.
            Types: str
        style:
            Optional Argument.
            Specifies the test style.
            Permitted Values:
                * 't' - T-Test paired, unpaired or unpaired with indicator variable
                        (second column).
                * 'fnway' - F-Test N-Way with Equal Cell Counts (1, 2, or 3 columns with same
                            number of cell counts). A cell count is the count of rows having a
                            combination of distinct column values.
                * 'f2way' - F-Test 2-Way with Unequal Cell Counts (2 columns with possibly
                            different numbers of cell counts). A cell count is the count of rows
                            having a combination of distinct column values.
            Default Value: 't'
            Types: str
        probability_threshold:
            Optional Argument.
            Specifies the threshold probability, i.e., "alpha" probability, below which the
            null hypothesis is rejected.
            Default Value: 0.05
            Types: float
        with_indicator:
            Optional Argument.
            Specifies whether the second column is used to indicate there are two analysis
            categories: one for the case where the second column is negative or zero, and
            another when the second column is positive. When this is set to True, then second
            column is used to indicate the analysis categories.
            Note:
                Argument can be used with an un-paired T-Test, i.e., when style is set to
                't' and paired is set to 'False'.
            Default Value: False
            Types: bool
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of ParametricTest.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        ParametricTestObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        #   3. The Statistical Test metadata tables must be loaded into the database where
        #      Analytics Library is installed.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrames.
        custanly = DataFrame("customer_analysis")
        print(custanly)
        cust = DataFrame("customer")
        print(cust)
        # Example 1: Perform T-Test with default values.
        obj = valib.ParametricTest(data=custanly,
                                   first_column="avg_cc_bal",
                                   second_column="avg_sv_bal",
                                   paired=True,
                                   equal_variance=True,
                                   group_columns=["age", "gender"])
        # Print the results.
        print(obj.result)
        # Example 2: Perform One way F-Test.
        obj = valib.ParametricTest(data=cust,
                                   style="fnway",
                                   dependent_column="income",
                                   columns="gender",
                                   probability_threshold=0.01,
                                   group_columns=["years_with_bank", "nbr_children"])
        # Print the results.
        print(obj.result)
        # Example 3: Perform a 2-way F-Test with Unequal Cell Counts.
        obj = valib.ParametricTest(data=cust,
                                   style="f2way",
                                   dependent_column="income",
                                   first_column="years_with_bank",
                                   first_column_values=[0, 1, 2, 3, 4, 5, 6, 7],
                                   second_column="marital_status",
                                   second_column_values=[1, 2, 3, 4],
                                   probability_threshold=0.05)
        # Print the results.
        print(obj.result)
        # Example 4: Generate only SQL for the function, but do not execute the same.
        obj = valib.ParametricTest(data=df,
                                   first_column="years_with_bank",
                                   second_column="marital_status",
                                   paired=True,
                                   equal_variance=True,
                                   group_columns=["years_with_bank", "nbr_children"],
                                   gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    RankTest
    RankTest
    Functions
    RankTest(data, block_column=None, columns=None, dependent_column=None, fallback=False, ﬁrst_column=None, group_columns=None, include_zero=False,
    independent=False, allow_duplicates=False, second_column=None, single_tail=False, stats_database=None, style='mw', probability_threshold=0.05,
    treatment_column=None, gen_sql_only=False, charset=None)
    DESCRIPTION:
        Statistical tests of this type calculate statistics based on the rank of variables
        rather than variable values. In general, data that are ranked and ordinal may be
        analyzed by these tests. Within some restraints, either numeric or non-numeric data
        may be analyzed. Supported rank tests include the following:
            * Mann-Whitney/Kruskal-Wallis Test
            * Wilcoxon Signed Ranks Test
            * Friedman Test with Kendall's Coefficient of Concordance & Spearmans' Rho
        The choice between the Mann-Whitney and Kruskal-Wallis tests is made automatically,
        looking at the number of distinct values of the independent variable. A variation of
        the Mann-Whitney test considers each requested variable individually, rather than
        combined, performing a series of independent tests.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to run statistical tests.
            Types: teradataml DataFrame
        block_column:
            Optional Argument.
            Specifies the name of the column representing blocks.
            Notes:
                1. Used only by the Friedman test.
                2. When pairing treatment and block column values, a division by zero error
                   can occur if unequal cell counts are found.
            Types: str
        dependent_column:
            Optional Argument.
            Specifies the name of the column representing the dependent variable. If
            non-numeric, it will be ranked alphanumerically.
            Note:
               Used only by the Mann-Whitney and Friedman tests.
            Types: str
        columns:
            Optional Argument.
            Specifies the name(s) of the categorical column(s) representing independent variables.
            Note:
                Used only by the Mann-Whitney test.
            Types: str OR list of Strings (str)
        fallback:
            Optional Argument.
            Specifies whether the FALLBACK is requested as in the output result or not.
            Default Value: False (Not requested)
            Types: bool
        first_column:
            Optional Argument.
            Specifies the name of the column that represents the first sample variable.
            Note:
                Used only by the Wilcoxon test.
            Types: str
        group_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) for grouping so that a separate result
            is produced for each value or combination of values in the specified column or
            columns.
            Types: str OR list of Strings (str)
        include_zero:
            Optional Argument.
            Specifies whether to discard cases with zero differences or not. Ordinarily,
            the Wilcoxon test discards cases with zero differences. When set to true,
            includes these cases with the positive count.
            Note:
                Used only by the Wilcoxon test.
            Default Value: False
            Types: bool
        independent:
            Optional Argument.
            Specifies whether variation of the Mann-Whitney test should be performed or not.
            When set to true, Mann-Whitney test variation is performed considering each
            requested variable individually, rather than in combination, performing a series
            of independent tests.
            Note:
                Used only by the Mann-Whitney test.
            Default Value: False
            Types: bool
        allow_duplicates:
            Optional Argument.
            Specifies whether duplicates are allowed in the output or not.
            Default Value: False
            Types: bool
        second_column:
            Optional Argument.
            Specifies the name of the column that represents the second sample variable.
            Note:
                Used only by the Wilcoxon test.
            Types: str
        single_tail:
            Optional Argument.
            Specifies whether to request single-tailed test or not. When ‘True’, a
            single-tailed test is requested. Otherwise, a two-tailed test is requested.
            Notes:
                1. Used only by the Mann-Whitney and Wilcoxon tests.
                2. If the Mann-Whitney test becomes a Kruskall-Wallis test, the single_tail
                   option is invalid.
            Default Value: False
            Types: bool
        stats_database:
            Optional Argument.
            Specifies the database where the statistical test metadata tables are installed.
            If not specified, the source database is searched for these metadata tables.
            Types: str
        style:
            Optional Argument.
            Specifies the test style.
            Permitted Values:
                * 'mw' - Mann-Whitney test.
                * 'friedman' - Friedman test.
                * 'wilcoxon' - Wilcoxon test.
            Default Value: 'mw'
            Types: str
        probability_threshold:
            Optional Argument.
            Specifies the threshold probability, i.e., "alpha" probability, below which
            the null hypothesis is rejected.
            Default Value: 0.05
            Types: float
        treatment_column:
            Optional Argument.
            Specifies the name of the column representing the independent categorical
            variable.
            Notes:
                1. Used only by the Friedman test.
                2. When pairing treatment and block column values, a division by zero error
                   can occur if unequal cell counts are found.
            Types: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of RankTest.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        RankTestObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        #   3. The Statistical Test metadata tables must be loaded into the database where
        #      Analytics Library is installed.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrames.
        custanly = DataFrame("customer_analysis")
        print(custanly)
        cust = DataFrame("customer")
        print(cust)
        # Example 1: Shows the parameters for a Mann-Whitney test with a threshold
        #            probability of 0.01.
        obj = valib.RankTest(data= cust,
                             dependent_column="income",
                             columns="gender",
                             group_columns="years_with_bank",
                             probability_threshold=0.01,
                             style="mw")
        # Print the results.
        print(obj.result)
        # Example 2: Shows the parameters for a set of Mann-Whitney independent tests.
        #            The threshold probability assumes the default value of 0.05.
        obj = valib.RankTest(data= custanly,
                             dependent_column="income",
                             columns=["gender", "ccacct", "svacct"],
                             style="mw")
        # Print the results.
        print(obj.result)
        # Example 3: Shows the parameters for a Wilcoxon Test.
        obj = valib.RankTest(data= custanly,
                             first_column="avg_ck_bal",
                             second_column="avg_sv_bal",
                             group_columns="years_with_bank",
                             style=" wilcoxon")
        # Print the results.
        print(obj.result)
        # Example 4: Shows the parameters for a Friedman Test using a specially prepared
        #            input table called Val_Friedman_WorkTable.
        # Prepare data for test style "friedman" as per example in VAL user guide.
        # The "Friedman" style need same number of rows for each combination treatment_column
        # and "block_column".
        # Let's get the smallest count of value combinations in the gender and marital_status
        # columns from custanly DataFrame.
        min_val = custanly.groupby(["marital_status", "gender"])\
                      .agg({'cust_id': "count"}).select("count_cust_id").min().squeeze().item()
        df_cr = custanly.select(["cust_id", "gender", "marital_status", "income", "ckacct",
                                 "svacct"])
        case_when_then_dict = {(df_cr.gender=="F") & (df_cr.marital_status=="1") : min_val,
                               (df_cr.gender=="F") & (df_cr.marital_status=="2") : min_val,
                               (df_cr.gender=="F") & (df_cr.marital_status=="3") : min_val,
                               (df_cr.gender=="F") & (df_cr.marital_status=="4") : min_val,
                               (df_cr.gender=="M") & (df_cr.marital_status=="1") : min_val,
                               (df_cr.gender=="M") & (df_cr.marital_status=="2") : min_val,
                               (df_cr.gender=="M") & (df_cr.marital_status=="3") : min_val,
                               (df_cr.gender=="M") & (df_cr.marital_status=="4") : min_val}
        df_fried = df_cr.sample(case_when_then=case_when_then_dict)
        # Execute the RankTest() function.
        obj = valib.RankTest(data=df_fried,
                             style="friedman",
                             dependent_column="income",
                             block_column="marital_status",
                             treatment_column="gender")
        # Print the results.
        print(obj.result)
        # Example 5: Generate only SQL for the function, but do not execute the same.
        obj = valib.RankTest(data= df,
                             probability_threshold=0.01,
                             first_column="avg_ck_bal",
                             second_column="avg_sv_bal",
                             group_columns="years_with_bank",
                             style="wilcoxon",
                             gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Variable Transformation
    Transform
    Transform
    Functions
    Transform(data, bins=None, derive=None, one_hot_encode=None, ﬁllna=None, label_encode=None, rescale=None, retain=None, sigmoid=None, zscore=None,
    fallback=False, index_columns=None, unique_index=False, key_columns=None, allow_duplicates=None, nopi=None, ﬁlter=None, gen_sql_only=False,
    charset=None)
    DESCRIPTION:
        The Variable Transformation analysis reads a teradataml DataFrame and produces an
        output containing transformed columns. This is useful when preparing data for input
        to an analytic algorithm. For example, a K-Means Clustering algorithm typically
        produces better results when the input columns are first converted to their Z-Score
        values to put all input variables on an equal footing, regardless of their magnitude.
        Function supports following transformations:
            * Binning - Binning replaces a continuous numeric column with a categorical one
                        to produce ordinal values (for example, numeric categorical values where
                        order is meaningful).
            * Derive - The Derive transformation requires the free-form transformation be
                       specified as a formula.
            * One Hot Encoding - One Hot Encoding is useful when a categorical data element
                                 must be re-expressed as one or more numeric data elements,
                                 creating a binary numeric field for each categorical data value.
            * Missing Value Treatment or Null Replacement.
            * Label Encoding - Allows to re-express existing values of a categorical data
                               column (variable) into a new coding scheme or to correct data
                               quality problems and focus an analysis on a value.
            * Min-Max Scaling - Limits the upper and lower boundaries of the data in a
                                continuous numeric column using a linear rescaling function
                                based on maximum and minimum data values.
            * Retain - Allows copying of one or more columns into the final analytic data set.
            * Sigmoid - Provides rescaling of continuous numeric data using a type of sigmoid
                        or s-shaped function.
            * ZScore - Provides rescaling of continuous numeric data using Z-Scores.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data to perform variable transformations.
            Types: teradataml DataFrame
        bins:
            Optional Argument.
            Specifies one or more instances of Binning Transformation. Binning replaces a
            continuous numeric column with a categorical one to produce ordinal values (for
            example, numeric categorical values where order is meaningful).
            Check the documentation of 'teradataml.analytics.Transformation.Binning' to know
            more about Binning.
            Types: Binning OR List of Binning
        derive:
            Optional Argument.
            Specifies one or more instances of Derive Transformation. This argument allows
            user to perform a free form transformation using arithmetic formula.
            Check the documentation of 'teradataml.analytics.Transformation.Derive' to know
            more about Derive.
            Types: Derive OR List of Derive
        one_hot_encode:
            Optional Argument.
            Specifies one or more instances of OneHotEncoder Transformation. One hot encoding
            allows user to re-express categorical data as one or more numeric data elements,
            creating a binary numeric field for each categorical data value.
            Check the documentation of 'teradataml.analytics.Transformation.OneHotEncoder' to
            know more about OneHotEncoder.
            Types: OneHotEncoder OR List of OneHotEncoder
        fillna:
            Optional Argument.
            Specifies one or more instances of FillNa Transformation. This argument allows
            user to perform a missing value/null replacement transformation.
            Check the documentation of 'teradataml.analytics.Transformation.FillNa' to know
            more about FillNa.
            Types: FillNa OR List of FillNa
        label_encode:
            Optional Argument.
            Specifies one or more instances of LabelEncoder Transformation. This allows
            to re-express existing values of a categorical data column (variable) into a
            new coding scheme.
            Check the documentation of 'teradataml.analytics.Transformation.LabelEncoder' to
            know more about LabelEncoder.
            Types: LabelEncoder OR List of LabelEncoder
        rescale:
            Optional Argument.
            Specifies one or more instances of MinMaxScalar Transformation. This limits
            the upper and lower boundaries of the data in a continuous numeric column using
            a linear rescaling function based on maximum and minimum data values.
            Check the documentation of 'teradataml.analytics.Transformation.MinMaxScalar' to
            know more about MinMaxScalar.
            Types: MinMaxScalar OR List of MinMaxScalar
        retain:
            Optional Argument.
            Specifies one or more instances of Retain Transformation. This argument allows
            user to retain columns from input to output.
            Check the documentation of 'teradataml.analytics.Transformation.Retain' to know
            more about Retain.
            Types: Retain OR List of Retain
        sigmoid:
            Optional Argument.
            Specifies one or more instances of Sigmoid Transformation. This argument allows
            user to perform a rescaling using sigmoid transformation.
            Check the documentation of 'teradataml.analytics.Transformation.Sigmoid' to know
            more about Sigmoid.
            Types: Sigmoid OR List of Sigmoid
        zscore:
            Optional Argument.
            Specifies one or more instances of ZScore Transformation. This argument allows
            user to perform a rescaling using Z-Score transformation.
            Check the documentation of 'teradataml.analytics.Transformation.ZScore' to know
            more about ZScore.
            Types: ZScore OR List of ZScore
        fallback:
            Optional Argument.
            Specifies whether a mirrored copy of underlying table of output DataFrame is
            required or not.
            Default Value: False
            Types: bool
        index_columns:
            Optional Argument.
            Specifies the name(s) of the output column(s) to be used as index in output DataFrame.
            Types: str OR List of Strings (str)
        unique_index:
            Optional Argument.
            Specifies whether the underlying output table should contain a unique primary
            index or not.
            Default Value: False
            Types: bool
        key_columns:
            Optional Argument.
            Specifies the name(s) of the column(s) that can be unique key in input and
            output teradataml DataFrame. When null replacement is requested, i.e., "fillna"
            argument is used either in FillNa transformation or in combination with a
            Binning, Derive, OneHotEncoder, LabelEncoder, MinMaxScalar, Sigmoid, or ZScore
            transformation, the "key_columns" argument must be specified.
            Types: str OR List of Strings (str)
        allow_duplicates:
            Optional Argument.
            Specifies whether output should contain duplicate rows or not.
            Types: bool
        nopi:
            Optional Argument.
            Specifies whether the underlying output table should contain no index columns.
            When True, output table does not contain index columns.
            Note:
                When this argument is set to True, "allow_duplicates" must also be set to True.
            Types: bool
        filter:
            Optional Argument.
            Specifies the clause to filter rows selected for transformation.
            For example,
                filter = "cust_id > 0"
            Types: str
        gen_sql_only:
            Optional Argument.
            Specifies whether to generate only SQL for the function.
            When set to True, function SQL is generated, not executed, which can be accessed 
            using show_query() method, otherwise SQL is just executed but not returned.
            Default Value: False
            Types: bool
        charset:
            Optional Argument.
            Specifies the character set for the table name and column names.
            If this argument is not set, the function takes default value set by
            VAL library.
            Permitted Values:
                * 'UTF8'
                * 'ASCII'
            Types: str
    RETURNS:
        An instance of Transform.
        Output teradataml DataFrames can be accessed using attribute references, such as 
        TransformObj.<attribute_name>.
        Output teradataml DataFrame attribute name is: result.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #   1. To execute Vantage Analytic Library functions,
        #       a. import "valib" object from teradataml.
        #       b. set 'configure.val_install_location' to the database name where Vantage 
        #          analytic library functions are installed.
        #   2. Datasets used in these examples can be loaded using Vantage Analytic Library 
        #      installer.
        # Import valib object from teradataml to execute this function.
        from teradataml import valib
        # Set the 'configure.val_install_location' variable.
        from teradataml import configure
        configure.val_install_location = "SYSLIB"
        # Create required teradataml DataFrame.
        df = DataFrame("customer")
        print(df)
        # Let's look at multiple transformations done via Transform.
        # Example of running individual transformations can be found in the example sections
        # for each Transformation.
        # Binning performed on column "income" with different number of bins.
        bin_1 = Binning(style="bins", value=3, columns="income", out_columns="inc1")
        bin_2 = Binning(style="bins", value=4, columns="income", out_columns="inc2")
        bin_3 = Binning(style="bins", value=5, columns="income", out_columns="inc3")
        # Derive transformation done on "age" column with different arithmetic formula.
        derive_1 = Derive(formula="x/10", columns="age", out_column="age1")
        derive_2 = Derive(formula="x*10", columns="age", out_column="age2")
        derive_3 = Derive(formula="x+10", columns="age", out_column="age3")
        # One hot encoding done for values in columns "gender" and "marital_status".
        dc_1 = OneHotEncoder(values=["M", "F"], columns="gender")
        dc_2 = OneHotEncoder(values=[1, 2, 3, 4], columns="marital_status")
        # Replace null values in column "street_nbr", by using 'literal' and 'mean' null style.
        fn_1 = FillNa(style="literal", value=0, columns="street_nbr")
        fn_2 = FillNa(columns="street_nbr", out_columns="street_nbr2")
        # Recode values in column "marital_status", using different coding schemes.
        rc_1 = LabelEncoder(values={1: "S", 2: "M", 3: "S", 4: "S"}, default="SAME",
                            columns="marital_status", out_columns="mar1")
        rc_2 = LabelEncoder(values={1: "B", 2: "A", 3: "B", 4: "B"}, default=None,
                            columns="marital_status", out_columns="mar2")
        # Values in column "age" are rescaled using MinMaxScalar using
        #   1. Both lower bound and upper bound,
        #   2. Only lower bound,
        #   3. Only upper bound.
        rs_1 = MinMaxScalar(lbound=0, ubound=100, columns="age", out_columns="age4")
        rs_2 = MinMaxScalar(lbound=0, ubound=None, columns="age", out_columns="age5")
        rs_3 = MinMaxScalar(lbound=None, ubound=100, columns="age", out_columns="age6")
        # Rescale values in column "nbr_children" using various sigmoid functions.
        sig_1 = Sigmoid(style="logit", columns="nbr_children", out_columns="nbr1")
        sig_2 = Sigmoid(style="modifiedlogit", columns="nbr_children", out_columns="nbr2")
        sig_3 = Sigmoid(style="tanh", columns="nbr_children", out_columns="nbr3")
        # Rescale values in column "years_with_bank" using Z-Score values.
        zscore = ZScore(columns="years_with_bank", out_columns="ywb")
        # Retain columns "income", "age", "years_with_bank" and "nbr_children" in the
        # transformed output.
        retain = Retain(columns=["income", "age", "years_with_bank", "nbr_children"])
        # Example 1: Execute the Transform function.
        obj = valib.Transform(data=df,
                              bins=[bin_1, bin_2, bin_3],
                              derive=[derive_1, derive_2, derive_3],
                              one_hot_encode=[dc_1, dc_2],
                              fillna=[fn_1, fn_2],
                              label_encode=[rc_1, rc_2],
                              rescale=[rs_1, rs_2, rs_3],
                              retain=retain,
                              sigmoid=[sig_1, sig_2, sig_3],
                              zscore=zscore,
                              key_columns="cust_id")
        # Print the results.
        print(obj.result)
        # Example 2: Generate only SQL for the function, but do not execute the same.
        obj = valib.Transform(data=df,
                              bins=[bin_1, bin_2, bin_3],
                              derive=[derive_1, derive_2, derive_3],
                              one_hot_encode=[dc_1, dc_2],
                              fillna=[fn_1, fn_2],
                              label_encode=[rc_1, rc_2],
                              rescale=[rs_1, rs_2, rs_3],
                              retain=retain,
                              sigmoid=[sig_1, sig_2, sig_3],
                              zscore=zscore,
                              key_columns="cust_id",
                              gen_sql_only=True)
        # Print the generated SQL.
        print(obj.show_query("sql"))
        # Print both generated SQL and stored procedure call.
        print(obj.show_query("both"))
        # Print the stored procedure call.
        print(obj.show_query())
        print(obj.show_query("sp"))
    Transformation Techniques to Use with Transform
    Binning
    teradataml.analytics.Transformations.Binning.__init__ = __init__(self, columns, style='bins', value=10, lbound=None, ubound=None, out_columns=None, datatype=None,
    ﬁllna=None, **kwargs)
    DESCRIPTION:
        Binning allows user to perform bin coding to replace continuous numeric
        column with a categorical one to produce ordinal values (for example,
        numeric categorical values where order is meaningful). Binning uses the
        same techniques used in Histogram analysis, allowing you to choose between:
            1. equal-width bins,
            2. equal-width bins with a user-specified minimum and maximum range,
            3. bins with a user-specified width,
            4. evenly distributed bins, or
            5. bins with user-specified boundaries.
        If the minimum and maximum are specified, all values less than the minimum
        are put into bin 0, while all values greater than the maximum are put into
        bin N+1. The same is true when the boundary option is specified.
        Bin Coding supports numeric and date type columns. If date values are entered,
        the keyword DATE must precede the date value, and do not enclose in single
        quotes.
        Note:
            Output of this function is passed to "bins" argument of "Transform"
            function from Vantage Analytic Library.
    PARAMETERS:
        columns:
            Required Argument.
            Specifies the names of the columns to perform transformation on.
            Types: str or list of str
        style:
            Optional Argument.
            Specifies the bin style to use.
            Permitted Values:
                * "bins":
                    This style allows user to specify equal-width bins without any
                    boundaries. Argument "values" must be used when this style of
                    binning is used.
                * "binswithboundaries":
                    This style allows user to specify equal-width bins with minimum
                    and maximum range. Arguments "values", "lbound" and "ubound" must
                    be used when this style of binning is used.
                    All values less than the minimum are put into bin 0, while all
                    values greater than the maximum are put into bin N+1.
                * "boundaries":
                    This style allows user to specify bins with boundaries.
                    To specify boundaries one should use keyword arguments as:
                        b1 --> To specify first boundary.
                        b2 --> To specify second boundary.
                        b3 --> To specify third boundary.
                        ...
                        bN --> To specify Nth boundary.
                    All values less than the first boundary value are put into bin 0,
                    while all values greater than the last boundary value are put into
                    bin N+1.
                    See "kwargs" description below for more details on how these
                    arguments must be used.
                * "quantiles":
                    This style allows user to specify evenly-distributed bins.
                    Argument "values" must be used when this style of binning is used.
                * "width":
                    This style allows user to specify bins with widths. Argument
                    "values" must be used when this style of binning is used.
            Default Value: 'bins'
            Types: str
        value:
            Optional Argument.
            Specifies the value to be used for bin code transformations.
            When bin style is:
                * 'bins' or 'binswithboundaries' argument specifies the number of bins.
                * 'quantiles' argument specifies the number of quantiles.
                * 'width' argument specifies the bin width.
            Note:
                Ignored when style is 'boundaries'.
            Default Value: 10
            Types: int
        lbound:
            Optional Argument.
            Specifies the minimum boundary value for 'binswithboundaries' style.
            Notes:
                1. Ignored when style is not 'binswithboundaries'.
                2. If date values are entered as string, the keyword 'DATE' must precede
                   the date value, and do not enclose in single quotes OR
                   pass a datetime.date object.
                   For example,
                        value='DATE 1987-06-09'
                        value=date(1987, 6, 9)
            Types: int, float, str, datetime.date
        ubound:
            Optional Argument.
            Specifies the maximum boundary value for 'binswithboundaries' style.
            Notes:
                1. Ignored when style is not 'binswithboundaries'.
                2. If date values are entered as string, the keyword 'DATE' must precede
                   the date value, and do not enclose in single quotes OR
                   pass a datetime.date object.
                   For example,
                        value='DATE 1987-06-09'
                        value=date(1987, 6, 9)
            Types: int, float, str, datetime.date
        out_columns:
            Optional Argument.
            Specifies the names of the output columns.
            Note:
                Number of elements in "columns" and "out_columns" must be same.
            Types: str or list of str
        datatype:
            Optional Argument.
            Specifies the name of the intended datatype of the output column.
            Intended data types for the output column can be specified using either the
            teradatasqlalchemy types or the permitted strings mentioned below:
             -------------------------------------------------------------------
            | If intended SQL Data Type is  | Permitted Value to be passed is   |
            |-------------------------------------------------------------------|
            | bigint                        | bigint                            |
            | byteint                       | byteint                           |
            | char(n)                       | char,n                            |
            | date                          | date                              |
            | decimal(m,n)                  | decimal,m,n                       |
            | float                         | float                             |
            | integer                       | integer                           |
            | number(*)                     | number                            |
            | number(n)                     | number,n                          |
            | number(*,n)                   | number,*,n                        |
            | number(n,n)                   | number,n,n                        |
            | smallint                      | smallint                          |
            | time(p)                       | time,p                            |
            | timestamp(p)                  | timestamp,p                       |
            | varchar(n)                    | varchar,n                         |
            --------------------------------------------------------------------
            Notes:
                1. Argument is ignored if "columns" argument is not used.
                2. char without a size is not supported.
                3. number(*) does not include the * in its datatype format.
            Examples:
                1. If intended datatype for the output column is "bigint", then
                   pass string "bigint" to the argument as shown below:
                   datatype="bigint"
                2. If intended datatype for the output column is "decimal(3,5)", then
                   pass string "decimal,3,5" to the argument as shown below:
                   datatype="decimal,3,5"
            Types: str, BIGINT, BYTEINT, CHAR, DATE, DECIMAL, FLOAT, INTEGER, NUMBER, SMALLINT, TIME,
                   TIMESTAMP, VARCHAR.
        fillna:
            Optional Argument.
            Specifies whether the null replacement/missing value treatment should
            be performed with binning or not. Output of FillNa() can be passed to
            this argument.
            Note:
                If the FillNa object is created with its arguments "columns",
                "out_columns" and "datatype", then values passed in FillNa() arguments
                are ignored. Only nullstyle information is captured from the same.
            Types: FillNa
        kwargs:
            Specifies the keyword arguments to provide the boundaries required
            for binning with bin style 'boundaries'.
            To specify boundaries one should use keyword arguments as:
                b1 --> To specify first boundary.
                b2 --> To specify second boundary.
                b3 --> To specify third boundary.
                ...
                bN --> To specify Nth boundary.
            Notes:
                1. When keyword arguments are used, make sure to specify boundaries
                   in sequence, i.e., b1, b2, b3, ...
                   In case a sequential keyword argument is missing an error is
                   raised.
                2. Keyword arguments specified for the boundaries must start with 'b'.
                3. First boundary must always be specified with "b1" argument.
            Types: int, float, str, datetime.date
    RETURNS:
        An instance of Binning class.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLE:
        # Note:
        #   To run any transformation, user needs to use Transform() function from
        #   Vantage Analytic Library.
        #   To do so import valib first and set the "val_install_location".
        >>> from teradataml import configure, DataFrame, Binning, FillNa, load_example_data, valib
        >>> configure.val_install_location = "SYSLIB"
        >>>
        # Load example data.
        >>> load_example_data("movavg", "ibm_stock")
        >>>
        # Create the required teradataml DataFrame.
        >>> ibm_stock = DataFrame.from_table("ibm_stock")
        >>> ibm_stock
            name    period  stockprice
        id
        183  ibm  62/02/07       552.0
        202  ibm  62/03/07       548.0
        181  ibm  62/02/05       551.0
        242  ibm  62/05/02       482.0
        364  ibm  62/10/25       331.0
        221  ibm  62/04/03       513.0
        38   ibm  61/07/11       473.0
        366  ibm  62/10/29       352.0
        326  ibm  62/08/30       387.0
        61   ibm  61/08/11       497.0
        >>>
        # Example 1: Binning is carried out with "bins" style, i.e. equal-width
        #            binning, with 5 number of bins. Null replacement is also combined
        #            with binning.
        #            "key_columns" argument must be used with Transform() function,
        #            when null replacement is being done.
        >>> fn = FillNa(style="literal", value=0)
        >>> bins = Binning(style="bins", value=5, columns="stockprice", fillna=fn)
        # Execute Transform() function.
        >>> obj = valib.Transform(data=ibm_stock,
        ...                       bins=bins,
        ...                       key_columns="id")
        >>> obj.result
            id  stockprice
        0  263           1
        1  324           2
        2  303           2
        3   99           5
        4   36           3
        5   97           5
        6  160           5
        7   59           4
        8   19           4
        9  122           5
        >>>
        # Example 2: Binning is carried out with multiple styles.
        # 'binswithboundaries' style:
        #   Equal-width bins with a user-specified minimum and maximum range on 'period'
        #   column. Resultant output return the value with the same column name. Number
        #   of bins created are 5.
        >>> bins_1 = Binning(style="binswithboundaries",
        ...                  value=5,
        ...                  lbound="DATE 1962-01-01",
        ...                  ubound="DATE 1962-06-01",
        ...                  columns="period")
        >>>
        # 'boundaries' style:
        #   Bins created with user specified boundaries on 'period' column. Resultant
        #   column is names as 'period2'. Three boundaries are specified with arguments
        #   "b1", "b2" and "b3". When using this style, keyword argument names must
        #   start with 'b' and they should be in sequence b1, b2, ..., bN.
        >>> bins_2 = Binning(style="boundaries",
        ...                  b1="DATE 1962-01-01",
        ...                  b2="DATE 1962-06-01",
        ...                  b3="DATE 1962-12-31",
        ...                  columns="period",
        ...                  out_columns="period2")
        >>>
        # Execute Transform() function.
        >>> obj = valib.Transform(data=ibm_stock,
        ...                       bins=[bins_1, bins_2])
        >>> obj.result
            id  period  period2
        0  223       4        1
        1  345       6        2
        2  120       0        0
        3  343       6        2
        4   57       0        0
        5  118       0        0
        6  200       3        1
        7   80       0        0
        8  162       1        1
        9   40       0        0
        >>>
        # Example 3: Binning is carried out with multiple styles 'quantiles' and
        #            'width'.
        # 'quantiles' style :
        #   Evenly distributed bins on 'stockprice' column. Resultant output returns
        #   the column with name 'stockprice_q'. Number of quantiles considered here
        #   are 4.
        >>> bins_1 = Binning(style="quantiles",
        ...                  value=4,
        ...                  out_columns="stockprice_q",
        ...                  columns="stockprice")
        >>>
        # 'width' style :
        #   Bins with user specified width on 'stockprice' column. Resultant output
        #   returns the column with name 'stockprice_w'. Width considered for binning
        #   is 5.
        >>> bins_2 = Binning(style="width",
        ...                  value=5,
        ...                  out_columns="stockprice_w",
        ...                  columns="stockprice")
        >>>
        # Execute Transform() function.
        >>> obj = valib.Transform(data=ibm_stock,
        ...                       bins=[bins_1, bins_2])
        >>> obj.result
            id  stockprice_q  stockprice_w
        0  183             4            50
        1  202             3            49
        2  181             4            50
        3  242             2            36
        4  364             1             6
        5  221             3            42
        6   38             2            34
        7  366             1            10
        8  326             1            17
        9   61             3            39
        >>>
    Derive
    teradataml.analytics.Transformations.Derive.__init__ = __init__(self, formula, columns, out_column, datatype=None, ﬁllna=None)
    DESCRIPTION:
        The Derive transformation requires the free-form transformation be specified
        as a formula using the following operators, arguments, and functions:
            +, -, **, *, /, %, (, ), x, y, z, abs, exp, ln, log, sqrt
        The arguments x, y, and z can only assume the value of an input column.
        An implied multiply operator is automatically inserted when a number, argument
        (x, y, z), or parenthesis is immediately followed by an argument or parenthesis.
        For example,
            4x means 4*x, xy means x*y, and x(x+1) is equivalent to x*(x+1).
        An example formula for the quadratic equation is below.
            formula="(-y+sqrt(y**2-4xz))/(2x)"
        Note:
            Output of this function is passed to "derive" argument of "Transform"
            function from Vantage Analytic Library.
    PARAMETERS:
        formula:
            Required Argument.
            Specifies the free-form transformation required for Derive.
            Arithmetic formula can be specified as string using following operators,
            arguments, and functions:
                +, -, **, *, /, %, (, ), x, y, z, abs, exp, ln, log, sqrt
            Types: str
        columns:
            Required Argument.
            Specifies the names of the columns to use for formula.
            Types: str or list of str
        out_column:
            Required Argument.
            Specifies the name of the output column.
            Types: str
        datatype:
            Optional Argument.
            Specifies the name of the intended datatype of the output column.
            Intended data types for the output column can be specified using either the
            teradatasqlalchemy types or the permitted strings mentioned below:
             -------------------------------------------------------------------
            | If intended SQL Data Type is  | Permitted Value to be passed is   |
            |-------------------------------------------------------------------|
            | bigint                        | bigint                            |
            | byteint                       | byteint                           |
            | char(n)                       | char,n                            |
            | date                          | date                              |
            | decimal(m,n)                  | decimal,m,n                       |
            | float                         | float                             |
            | integer                       | integer                           |
            | number(*)                     | number                            |
            | number(n)                     | number,n                          |
            | number(*,n)                   | number,*,n                        |
            | number(n,n)                   | number,n,n                        |
            | smallint                      | smallint                          |
            | time(p)                       | time,p                            |
            | timestamp(p)                  | timestamp,p                       |
            | varchar(n)                    | varchar,n                         |
            --------------------------------------------------------------------
            Notes:
                1. Argument is ignored if "columns" argument is not used.
                2. char without a size is not supported.
                3. number(*) does not include the * in its datatype format.
            Examples:
                1. If intended datatype for the output column is "bigint", then
                   pass string "bigint" to the argument as shown below:
                   datatype="bigint"
                2. If intended datatype for the output column is "decimal(3,5)", then
                   pass string "decimal,3,5" to the argument as shown below:
                   datatype="decimal,3,5"
            Types: str, BIGINT, BYTEINT, CHAR, DATE, DECIMAL, FLOAT, INTEGER, NUMBER, SMALLINT, TIME,
                   TIMESTAMP, VARCHAR.
        fillna:
            Optional Argument.
            Specifies whether the null replacement/missing value treatment should
            be performed with derive or not. Output of FillNa() can be passed to
            this argument.
            Note:
                If the FillNa object is created with its arguments "columns",
                "out_columns" and "datatype", then values passed in FillNa() arguments
                are ignored. Only nullstyle information is captured from the same.
            Types: FillNa
    RETURNS:
        An instance of Derive class.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLE:
        # Note:
        #   To run any transformation, user needs to use Transform() function from
        #   Vantage Analytic Library.
        #   To do so import valib first and set the "val_install_location".
        >>> from teradataml import configure, DataFrame, Derive, FillNa, load_example_data, valib
        >>> configure.val_install_location = "SYSLIB"
        >>>
        # Load example data.
        >>> load_example_data("dataframe", "sales")
        >>>
        # Create the required DataFrame.
        >>> sales = DataFrame("sales")
        >>> sales
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        >>>
        # Example: Includes multiple derive transformations.
        # Derive transformation 1 is done with 3 variables, x, y, z, to calculate
        # the total sales for the first quarter for each account.
        >>> fn_1 = FillNa(style='literal', value=0)
        >>> dr_1 = Derive(formula="x+y+z", columns=["Jan", "Feb", "Mar"],
        ...               out_column="q1_sales", fillna=fn_1)
        >>>
        # Derive transformation 2 is done with 2 variables, x, y, to calculate
        # the sale growth from the month of Jan to Feb.
        >>> fn_2 = FillNa(style='median')
        >>> dr_2 = Derive(formula="((y-x)/x)*100", columns=["Jan", "Feb"],
        ...               out_column="feb_growth", fillna=fn_2, datatype='bigint')
        >>>
        # Execute Transform() function.
        >>> obj = valib.Transform(data=sales, derive=[dr_1, dr_2], key_columns="accounts")
        >>> obj.result
             accounts  q1_sales  feb_growth
        0    Alpha Co     625.0           4
        1     Red Inc     490.0          33
        2  Orange Inc       NaN          40
        3   Jones LLC     490.0          33
        4  Yellow Inc       NaN         -40
        5    Blue Inc     235.0          79
        >>>
    FillNa
    teradataml.analytics.Transformations.FillNa.__init__ = __init__(self, style='mean', value=None, columns=None, out_columns=None, datatype=None)
    DESCRIPTION:
        FillNa allows user to perform missing value/null replacement transformations.
        Note:
            Output of this function is passed to "fillna" argument of "Transform"
            function from Vantage Analytic Library.
    PARAMETERS:
        style:
            Optional Argument.
            Specifies the nullstyle for missing value/null value replacement.
            A literal value, the mean, median, mode, or an imputed value joined
            from another table can be used as the replacement value. The median
            value can be requested with or without averaging of two middle values
            when there is an even number of values.
            Literal value replacement is supported for numeric, character, and
            date data types.
            Mean value replacement is supported for columns of numeric type or
            date type.
            Median without averaging, mode, and imputed value replacement are
            valid for any supported type. Median with averaging is supported
            only for numeric and date type.
            Permitted Values: 'literal', 'mean', 'median', 'mode', 'median_wo_mean',
                              'imputed'
            Default Value: 'mean'
            Types: str
        value:
            Optional Argument. Required when "style" is 'literal' or 'imputed'.
            Specifies the value to be used for null replacement transformations.
            Notes:
                1. When "style" is 'imputed', value must be of type teradataml
                   DataFrame.
                2. When "style" is 'literal', value can be of any type.
                3. If date values are entered as string, the keyword 'DATE' must precede
                   the date value, and do not enclose in single quotes OR
                   pass a datetime.date object.
                   For example,
                        value='DATE 1987-06-09'
                        value=date(1987, 6, 9)
            Types: teradataml DataFrame, bool, int, str, float, datetime.date
        columns:
            Optional Argument.
            Specifies the names of the columns.
            Types: str or list of str
        out_columns:
            Optional Argument.
            Specifies the names of the output columns.
            Notes:
                Number of elements in "columns" and "out_columns" must be same.
            Types: str or list of str
        datatype:
            Optional Argument.
            Specifies the name of the intended datatype of the output column.
            Intended data types for the output column can be specified using either the
            teradatasqlalchemy types or the permitted strings mentioned below:
             -------------------------------------------------------------------
            | If intended SQL Data Type is  | Permitted Value to be passed is   |
            |-------------------------------------------------------------------|
            | bigint                        | bigint                            |
            | byteint                       | byteint                           |
            | char(n)                       | char,n                            |
            | date                          | date                              |
            | decimal(m,n)                  | decimal,m,n                       |
            | float                         | float                             |
            | integer                       | integer                           |
            | number(*)                     | number                            |
            | number(n)                     | number,n                          |
            | number(*,n)                   | number,*,n                        |
            | number(n,n)                   | number,n,n                        |
            | smallint                      | smallint                          |
            | time(p)                       | time,p                            |
            | timestamp(p)                  | timestamp,p                       |
            | varchar(n)                    | varchar,n                         |
            --------------------------------------------------------------------
            Notes:
                1. char without a size is not supported.
                2. number(*) does not include the * in its datatype format.
            Examples:
                1. If intended datatype for the output column is "bigint", then
                   pass string "bigint" to the argument as shown below:
                   datatype="bigint"
                2. If intended datatype for the output column is "decimal(3,5)", then
                   pass string "decimal,3,5" to the argument as shown below:
                   datatype="decimal,3,5"
            Types: str, BIGINT, BYTEINT, CHAR, DATE, DECIMAL, FLOAT, INTEGER, NUMBER, SMALLINT, TIME,
                   TIMESTAMP, VARCHAR.
    RETURNS:
        An instance of FillNa class.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLE:
        # Note:
        #   To run any transformation, user needs to use Transform() function from
        #   Vantage Analytic Library.
        #   To do so import valib first and set the "val_install_location".
        >>> from teradataml import configure, DataFrame, FillNa, load_example_data, valib
        >>> configure.val_install_location = "SYSLIB"
        >>>
        # Load example data.
        >>> load_example_data("dataframe", ["sales", "employee_info"])
        >>>
        # Create the required DataFrames.
        >>> sales = DataFrame("sales")
        >>> sales
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        >>>
        # Example 1: Replace missing values in columns 'Jan' and 'Mar', with
        #            a literal value 0. Output columns are named as 'january' and
        #            'march' respectively.
        >>> fillna_literal = FillNa(style="literal", value=0, columns=["Jan", "Mar"],
        ...                         out_columns=["january", "march"])
        >>> obj = valib.Transform(data=sales, fillna=fillna_literal, key_columns="accounts")
        >>> obj.result
             accounts  january  march
        0    Blue Inc       50     95
        1  Orange Inc        0      0
        2     Red Inc      150    140
        3  Yellow Inc        0      0
        4   Jones LLC      150    140
        5    Alpha Co      200    215
        >>>
        # Example 2: Replace missing values in column 'Jan' with 'median' value from
        #            that column. Output column produced in the output is named as
        #            'Jan2'.
        >>> fillna_median = FillNa(style="median", columns="Jan", out_columns="Jan2")
        >>> obj = valib.Transform(data=sales, fillna=fillna_median, key_columns="accounts")
        >>> obj.result
             accounts     Jan2
        0    Alpha Co  200.000
        1     Red Inc  150.000
        2  Orange Inc  150.000
        3   Jones LLC  150.000
        4  Yellow Inc  150.000
        5    Blue Inc   50.000
        >>>
        # Example 3: Replace missing values in column 'Apr' with a median value
        #            without mean from that column.
        >>> fillna_mwm = FillNa(style="median_wo_mean", columns="Apr")
        >>> obj = valib.Transform(data=sales, fillna=fillna_mwm, key_columns="accounts")
        >>> obj.result
             accounts  Apr
        0    Alpha Co  250
        1    Blue Inc  101
        2  Yellow Inc  180
        3   Jones LLC  180
        4     Red Inc  180
        5  Orange Inc  250
        >>>
        # Example 4: Replace missing values in column 'Apr' with 'mode' value from
        #            that column. Output column produced in the output is named as
        #            'Apr2000'.
        >>> fillna_mode = FillNa(style="mode", columns="Apr", out_columns="Apr2000")
        >>> obj = valib.Transform(data=sales, fillna=fillna_mode, key_columns="accounts")
        >>> obj.result
             accounts  Apr2000
        0    Blue Inc      101
        1  Orange Inc      250
        2     Red Inc      250
        3  Yellow Inc      250
        4   Jones LLC      180
        5    Alpha Co      250
        >>>
        # Example 5: Replace missing values in columns 'masters' and 'programming' using
        #            'imputed' style.
        >>> load_example_data("dataframe", ["admissions_train_nulls", "admissions_train"])
        # Dataframe to be used for 'imputed' style replacement.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # DataFrame containing NULL values in columns "programming" and "masters".
        >>> admissions_train_nulls = DataFrame("admissions_train_nulls")
        >>> admissions_train_nulls
           masters   gpa     stats programming  admitted
        id
        5       no  3.44    Novice      Novice         0
        7      yes  2.33    Novice      Novice         1
        22    None  3.46    Novice        None         0
        19     yes  1.98  Advanced    Advanced         0
        15    None  4.00  Advanced    Advanced         1
        17    None  3.83  Advanced    Advanced         1
        34    None  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        36      no  3.00  Advanced      Novice         0
        40     yes  3.95    Novice    Beginner         0
        >>>
        # Replace NULL values in the columns "masters" and "programming"
        # in admissions_train_nulls dataframe with the values in the corresponding
        # columns' values in admissions_train dataframe.
        >>> fillna_imputed = FillNa(style="imputed",
        ...                         columns=["masters", "programming"],
        ...                         value=admissions_train)
        >>> obj = valib.Transform(data=admissions_train_nulls,
        ...                       fillna=fillna_imputed,
        ...                       key_columns="id")
        >>> obj.result
           id masters programming
        0  22     yes    Beginner
        1  36      no      Novice
        2  15     yes    Advanced
        3  38     yes    Beginner
        4   5      no      Novice
        5  17      no    Advanced
        6  34     yes    Beginner
        7  13      no      Novice
        8  26     yes    Advanced
        9  19     yes    Advanced
        >>>
        # Example 6: This example shows how one can operate on date and character
        #            columns. Example also showcases using multiple missing value
        #            treatment techniques in one single call for variable
        #            transformation.
        # Create the required DataFrames.
        >>> einfo = DataFrame("employee_info")
        >>> einfo
                    first_name marks   dob joined_date
        employee_no
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        101              abcde  None  None    02/12/05
        >>>
        # Using literal style for missing value treatment on a date type
        # column "joined_date".
        >>> fillna_1 = FillNa(style="literal", value="DATE 1995-12-23",
        ...                   columns="joined_date", out_columns="date1")
        # Using literal style for missing value treatment on a character type
        # column "first_name". Repalce missing values with 'FNU', i.e.,
        # First Name Unknown.
        >>> fillna_2 = FillNa(style="literal", value="FNU", columns="first_name",
        ...                   out_columns="char1")
        # Using mean value for missing value treatment on a date type
        # column "joined_date".
        >>> fillna_3 = FillNa(style="mean", columns="joined_date",
        ...                   out_columns="date2")
        # Using median value for missing value treatment on a date type
        # column "joined_date".
        >>> fillna_4 = FillNa(style="median", columns="joined_date",
        ...                   out_columns="date2A")
        # Using median value without mean for missing value treatment on
        # a date type column "joined_date".
        >>> fillna_5 = FillNa(style="median_wo_mean", columns="joined_date",
        ...                   out_columns="date3")
        # Using mode value for missing value treatment on a date type
        # column "joined_date".
        >>> fillna_6 = FillNa(style="mode", columns="joined_date",
        ...                   out_columns="date4")
        # Using median value without mean for missing value treatment on
        # a character type column "first_name".
        >>> fillna_7 = FillNa(style="median_wo_mean", columns="first_name",
        ...                   out_columns="char2")
        # Using mode value for missing value treatment on a character type
        # column "first_name".
        >>> fillna_8 = FillNa(style="mode", columns="first_name",
        ...                   out_columns="char3")
        # Perform the missing value transformations using Transform() function
        # from Vantage Analytic Library.
        >>> obj = valib.Transform(data=einfo,
        ...                       fillna=[fillna_1, fillna_2, fillna_3, fillna_4,
        ...                       fillna_5, fillna_6, fillna_7, fillna_8],
        ...                       key_columns="employee_no")
        >>> obj.result
           employee_no     date1  char1     date2    date2A     date3     date4  char2  char3
        0          112  18/12/05    FNU  18/12/05  18/12/05  18/12/05  18/12/05   abcd   abcd
        1          101  02/12/05  abcde  02/12/05  02/12/05  02/12/05  02/12/05  abcde  abcde
        2          100  95/12/23   abcd  60/12/04  60/12/04  02/12/05  02/12/05   abcd   abcd
        >>>
    LabelEncoder
    teradataml.analytics.Transformations.LabelEncoder.__init__ = __init__(self, values, columns, default=None, out_columns=None, datatype=None, ﬁllna=None)
    DESCRIPTION:
        Label encoding a categorical data column is done to re-express existing values
        of a column (variable) into a new coding scheme or to correct data quality
        problems and focus an analysis of a particular value. It allows for mapping
        individual values, NULL values, or any number of remaining values (ELSE
        option) to a new value, a NULL value or the same value.
        Label encoding supports character, numeric, and date type columns.
        Note:
            Output of this function is passed to "label_encode" argument of "Transform"
            function from Vantage Analytic Library.
    PARAMETERS:
        values:
            Required Argument.
            Specifies the values to be label encoded. Values can be specified in
            two formats:
                1. A list of two-tuples, where first value in the tuple is a
                   old value and second value is a new value.
                   For example,
                        values = [(old_val1, new_val2), (old_val2, new_val2)]
                2. A dictionary with key as old value and value as new value.
                   For example,
                        values = {old_val1: new_val2, old_val2: new_val2}
            Note:
                1. If date values are entered as string, the keyword 'DATE' must precede
                   the date value, and do not enclose in single quotes OR
                   pass a datetime.date object.
                   For example,
                       value='DATE 1987-06-09'
                       value=date(1987, 6, 9)
                2. To keep the old value as is, one can pass 'same' as it's new value.
                3. To use NULL values for old or new value, one can either use string
                   'null' or None.
            Types: two-tuple, list of two-tuples, dict
        columns:
            Required Argument.
            Specifies the names of the columns containing values to be label encoded.
            Types: str or list of str
        default:
            Optional Argument.
            Specifies the value assumed for all other cases.
            Permitted Values: None, 'SAME', 'NULL', a literal
            Default Value: None
            Types: bool, float, int, str
        out_columns:
            Optional Argument.
            Specifies the names of the output columns. Value passed to this argument
            also plays a crucial role in determining the output column name.
            Note:
                Number of elements in "columns" and "out_columns" must be same.
            Types: str or list of str
        datatype:
            Optional Argument.
            Specifies the name of the intended datatype of the output column.
            Intended data types for the output column can be specified using either the
            teradatasqlalchemy types or the permitted strings mentioned below:
             -------------------------------------------------------------------
            | If intended SQL Data Type is  | Permitted Value to be passed is   |
            |-------------------------------------------------------------------|
            | bigint                        | bigint                            |
            | byteint                       | byteint                           |
            | char(n)                       | char,n                            |
            | date                          | date                              |
            | decimal(m,n)                  | decimal,m,n                       |
            | float                         | float                             |
            | integer                       | integer                           |
            | number(*)                     | number                            |
            | number(n)                     | number,n                          |
            | number(*,n)                   | number,*,n                        |
            | number(n,n)                   | number,n,n                        |
            | smallint                      | smallint                          |
            | time(p)                       | time,p                            |
            | timestamp(p)                  | timestamp,p                       |
            | varchar(n)                    | varchar,n                         |
            --------------------------------------------------------------------
            Notes:
                1. Argument is ignored if "columns" argument is not used.
                2. char without a size is not supported.
                3. number(*) does not include the * in its datatype format.
            Examples:
                1. If intended datatype for the output column is "bigint", then
                   pass string "bigint" to the argument as shown below:
                   datatype="bigint"
                2. If intended datatype for the output column is "decimal(3,5)", then
                   pass string "decimal,3,5" to the argument as shown below:
                   datatype="decimal,3,5"
            Types: str, BIGINT, BYTEINT, CHAR, DATE, DECIMAL, FLOAT, INTEGER, NUMBER, SMALLINT, TIME,
                   TIMESTAMP, VARCHAR.
        fillna:
            Optional Argument.
            Specifies whether the null replacement/missing value treatment should
            be performed with recoding or not. Output of FillNa() can be passed to
            this argument.
            Note:
                If the FillNa object is created with its arguments "columns",
                "out_columns" and "datatype", then values passed in FillNa() arguments
                are ignored. Only nullstyle information is captured from the same.
            Types: FillNa
    RETURNS:
        An instance of LabelEncoder class.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLE:
        # Note:
        #   To run any transformation, user needs to use Transform() function from
        #   Vantage Analytic Library.
        #   To do so import valib first and set the "val_install_location".
        >>> from teradataml import configure, DataFrame, LabelEncoder, FillNa, load_example_data, valib
        >>> configure.val_install_location = "SYSLIB"
        >>>
        # Load example data.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create the required DataFrame.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
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
        >>>
        # Example 1: Recode all values 'Novice', 'Advanced', and 'Beginner'
        #            in "programming" and "stats" columns.
        #            We will pass values to "label_encode" as dictionary.
        >>> rc = LabelEncoder(values={"Novice": 1, "Advanced": 2, "Beginner": 3}, columns=["stats", "programming"])
        # Execute Transform() function.
        >>> obj = valib.Transform(data=admissions_train, label_encode=rc)
        >>> obj.result
           id stats programming
        0  22     1           3
        1  36     2           1
        2  15     2           2
        3  38     2           3
        4   5     1           1
        5  17     2           2
        6  34     2           3
        7  13     2           1
        8  26     2           2
        9  19     2           2
        >>>
        # Example 2: Recode value 'Novice' as 1 which is passed as tuple to "values"
        #            argument and "label_encode" other values as 0 by passing it to "default"
        #            argument in "programming" and "stats" columns.
        >>> rc = LabelEncoder(values=("Novice", 1), columns=["stats", "programming"], default=0)
        # Execute Transform() function.
        >>> obj = valib.Transform(data=admissions_train, label_encode=rc)
        >>> obj.result
           id stats programming
        0  15     0           0
        1   7     1           1
        2  22     1           0
        3  17     0           0
        4  13     0           1
        5  38     0           0
        6  26     0           0
        7   5     1           1
        8  34     0           0
        9  40     1           0
        >>>
        # Example 3: In this example we encode values differently for multiple columns.
        # For values in "programming" column, recoding will be done as follows:
        #   Novice --> 0
        #   Advanced --> 1 and
        #   Rest of the values as --> NULL
        >>> rc_prog = LabelEncoder(values=[("Novice", 0), ("Advanced", 1)], columns="programming",
        ...                  default=None)
        >>>
        # For values in "stats" column, recoding will be done as follows:
        #   Novice --> N
        #   Advanced --> keep it as is and
        #   Beginner --> NULL
        >>> rc_stats = LabelEncoder(values={"Novice": 0, "Advanced": "same", "Beginner": None},
        ...                   columns="stats")
        >>>
        # For values in "masters" column, recoding will be done as follows:
        #   yes --> 1 and other as 0
        >>> rc_yes = LabelEncoder(values=("yes", 1), columns="masters", default=0,
        ...                 out_columns="masters_yes")
        >>>
        # For values in "masters" column, label encoding will be done as follows:
        #   no --> 1 and other as 0
        >>> rc_no = LabelEncoder(values=("no", 1), columns="masters", default=0,
        ...                out_columns="masters_no")
        >>>
        # Execute Transform() function.
        >>> obj = valib.Transform(data=admissions_train, label_encode=[rc_prog, rc_stats, rc_yes,
        ...                                                      rc_no])
        >>> obj.result
           id programming     stats masters_yes masters_no
        0  13           0  Advanced           0          1
        1  26           1  Advanced           1          0
        2   5           0         0           0          1
        3  19           1  Advanced           1          0
        4  15           1  Advanced           1          0
        5  40        None         0           1          0
        6   7           0         0           1          0
        7  22        None         0           1          0
        8  36           0  Advanced           0          1
        9  38        None  Advanced           1          0
        >>>
    MinMaxScalar
    teradataml.analytics.Transformations.MinMaxScalar.__init__ = __init__(self, columns, lbound=0, ubound=1, out_columns=None, datatype=None, ﬁllna=None)
    DESCRIPTION:
        MinMaxScalar allows rescaling that limits the upper and lower boundaries of the
        data in a continuous numeric column using a linear rescaling function based on
        maximum and minimum data values. MinMaxScalar is useful with algorithms that require
        or work better with data within a certain range. MinMaxScalar is only valid on numeric
        columns, and not columns of type date.
        The rescale transformation formulas are shown in the following examples.
        The l denotes the left bound and r denotes the right bound.
            * When both the lower and upper bounds are specified:
                f(x,l,r) = (l+(x-min(x))(r-l))/(max(x)-min(x))
            * When only the lower bound is specified:
                f(x,l) = x-min(x)+l
            * When only the upper bound is specified:
                f(x,r) = x-max(x)+r
        Rescaling supports only numeric type columns.
        Note:
            Output of this function is passed to "rescale" argument of "Transform"
            function from Vantage Analytic Library.
    PARAMETERS:
        columns:
            Required Argument.
            Specifies the names of the columns to perform transformation on.
            Types: str or list of str
        lbound:
            Optional Argument.
            Specifies the lowerbound value required for rescaling the numeric data.
            If only the lower boundary is supplied, the variable is aligned to this
            value. This can be achieved by passing None to "ubound" argument.
            Default Value: 0
            Types: float, int
        ubound:
            Optional Argument.
            Specifies the upperbound value required for rescaling the numeric data.
            If only an upper boundary value is specified, the variable is aligned to
            this value. This can be achieved by passing None to "lbound" argument.
            Default Value: 1
            Types: float, int
        out_columns:
            Optional Argument.
            Specifies the names of the output columns.
            Note:
                Number of elements in "columns" and "out_columns" must be same.
            Types: str or list of str
        datatype:
            Optional Argument.
            Specifies the name of the intended datatype of the output column.
            Intended data types for the output column can be specified using either the
            teradatasqlalchemy types or the permitted strings mentioned below:
             -------------------------------------------------------------------
            | If intended SQL Data Type is  | Permitted Value to be passed is   |
            |-------------------------------------------------------------------|
            | bigint                        | bigint                            |
            | byteint                       | byteint                           |
            | char(n)                       | char,n                            |
            | date                          | date                              |
            | decimal(m,n)                  | decimal,m,n                       |
            | float                         | float                             |
            | integer                       | integer                           |
            | number(*)                     | number                            |
            | number(n)                     | number,n                          |
            | number(*,n)                   | number,*,n                        |
            | number(n,n)                   | number,n,n                        |
            | smallint                      | smallint                          |
            | time(p)                       | time,p                            |
            | timestamp(p)                  | timestamp,p                       |
            | varchar(n)                    | varchar,n                         |
            --------------------------------------------------------------------
            Notes:
                1. Argument is ignored if "columns" argument is not used.
                2. char without a size is not supported.
                3. number(*) does not include the * in its datatype format.
            Examples:
                1. If intended datatype for the output column is "bigint", then
                   pass string "bigint" to the argument as shown below:
                   datatype="bigint"
                2. If intended datatype for the output column is "decimal(3,5)", then
                   pass string "decimal,3,5" to the argument as shown below:
                   datatype="decimal,3,5"
            Types: str, BIGINT, BYTEINT, CHAR, DATE, DECIMAL, FLOAT, INTEGER, NUMBER, SMALLINT, TIME,
                   TIMESTAMP, VARCHAR.
        fillna:
            Optional Argument.
            Specifies whether the null replacement/missing value treatment should
            be performed with rescaling or not. Output of 'FillNa()' can be passed to
            this argument.
            Note:
                If the FillNa object is created with its arguments "columns",
                "out_columns" and "datatype", then values passed in FillNa() arguments
                are ignored. Only nullstyle information is captured from the same.
            Types: FillNa
    RETURNS:
        An instance of MinMaxScalar class.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLE:
        # Note:
        #   To run any transformation, user needs to use Transform() function from
        #   Vantage Analytic Library.
        #   To do so import valib first and set the "val_install_location".
        >>> from teradataml import configure, DataFrame, MinMaxScalar, FillNa, load_example_data, valib
        >>> configure.val_install_location = "SYSLIB"
        >>>
        # Load example data.
        >>> load_example_data("dataframe", "sales")
        >>>
        # Create the required DataFrames.
        >>> df = DataFrame("sales")
        >>> df
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        >>>
        # Example 1: Rescale values in column "Feb", using the default bounds, which is
        #            with lowerbound as 0 and upperbound as 1.
        >>> rs = MinMaxScalar(columns="Feb")
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, rescale=rs)
        >>> obj.result
             accounts       Feb
        0    Blue Inc  0.000000
        1    Alpha Co  1.000000
        2   Jones LLC  0.916667
        3  Yellow Inc  0.000000
        4  Orange Inc  1.000000
        5     Red Inc  0.916667
        >>>
        # Example 2: Rescale values in column "Feb", using only lowerbound as -1.
        #            To use only lowerbound, one must pass None to "ubound".
        >>> rs = MinMaxScalar(columns="Feb", lbound=-1, ubound=None)
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, rescale=rs)
        >>> obj.result
             accounts    Feb
        0   Jones LLC  109.0
        1  Yellow Inc   -1.0
        2     Red Inc  109.0
        3    Blue Inc   -1.0
        4    Alpha Co  119.0
        5  Orange Inc  119.0
        >>>
        # Example 3: Rescale values in columns "Jan" and "Apr", using only upperbound as 10.
        #            To use only upperbound, one must pass None to "lbound".
        #            We shall also combine this with missing value treatment. We shall replace
        #            missing values with "mode" null style replacement.
        >>> fn = FillNa(style="mode")
        >>> rs = MinMaxScalar(columns=["Jan", "Apr"], lbound=None, ubound=10, fillna=fn)
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, rescale=rs, key_columns="accounts")
        >>> obj.result
             accounts    Jan    Apr
        0    Alpha Co   10.0   10.0
        1    Blue Inc -140.0 -139.0
        2  Yellow Inc  -40.0   10.0
        3   Jones LLC  -40.0  -60.0
        4     Red Inc  -40.0   10.0
        5  Orange Inc  -40.0   10.0
        >>>
        # Example 4: This example shows combining multiple ways of rescaling in one
        #            Transform() call.
        # Rescale values in column "Feb" using lowerbound as -1 and upperbound as 1.
        # Name the output column as "Feb1".
        >>> rs_1 = MinMaxScalar(columns="Feb", lbound=-1, ubound=1, out_columns="Feb1")
        >>>
        # Rescale values in column "Feb" using only upperbound as 1.
        # Name the output column as "FebU".
        >>> rs_2 = MinMaxScalar(columns="Feb", lbound=None, ubound=1, out_columns="FebU")
        >>>
        # Rescale values in column "Feb" using only lowerbound as 0 (default value).
        # Name the output column as "FebL".
        >>> rs_3 = MinMaxScalar(columns="Feb", ubound=None, out_columns="FebL")
        >>>
        # Rescale values in columns "Jan" and "Apr" using default bounds.
        # Name the output columns as "Jan1" and "Apr1".
        # Combine with Missing value treatment, with literal null replacement.
        >>> fn_1 = FillNa(style="literal", value=0)
        >>> rs_4 = MinMaxScalar(columns=["Jan", "Apr"], out_columns=["Jan1", "Apr1"], fillna=fn_1)
        >>>
        # Rescale values in columns "Jan" and "Apr" using default bounds.
        # Name the output columns as "Jan2" and "Apr2".
        # Combine with Missing value treatment, with median null replacement.
        >>> fn_2 = FillNa(style="median")
        >>> rs_5 = MinMaxScalar(columns=["Jan", "Apr"], out_columns=["Jan2", "Apr2"], fillna=fn_2)
        >>>
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, rescale=[rs_1, rs_2, rs_3, rs_4, rs_5],
        ...                       key_columns="accounts")
        >>> obj.result
             accounts      Feb1   FebU   FebL  Jan1   Apr1      Jan2      Apr2
        0    Blue Inc -1.000000 -119.0    0.0  0.25  0.404  0.000000  0.000000
        1    Alpha Co  1.000000    1.0  120.0  1.00  1.000  1.000000  1.000000
        2   Jones LLC  0.833333   -9.0  110.0  0.75  0.720  0.666667  0.530201
        3  Yellow Inc -1.000000 -119.0    0.0  0.00  0.000  0.666667  0.765101
        4  Orange Inc  1.000000    1.0  120.0  0.00  1.000  0.666667  1.000000
        5     Red Inc  0.833333   -9.0  110.0  0.75  0.000  0.666667  0.765101
        >>>
    OneHotEncoder
    teradataml.analytics.Transformations.OneHotEncoder.__init__ = __init__(self, values, columns, style='dummy', reference_value=None, out_columns=None, datatype=None,
    ﬁllna=None)
    DESCRIPTION:
        One hot encoding is useful when a categorical data element must be re-expressed
        as one or more numeric data elements, creating a binary numeric field for
        each categorical data value. One hot encoding supports character, numeric,
        and date type columns.
        One hot encoding is offered in two forms: dummy-coding and contrast-coding.
            * In dummy-coding, a new column is produced for each listed value, with
              a value of 0 or 1 depending on whether that value is assumed by the
              original column. If a column assumes n values, new columns can be
              created for all n values, (or for only n-1 values, because the nth
              column is perfectly correlated with the first n-1 columns).
            * Alternately, given a list of values to contrast-code along with a
              reference value, a new column is produced for each listed value, with
              a value of 0 or 1 depending on whether that value is assumed by the
              original column, or a value of -1 if that original value is equal to
              the reference value.
        Note:
            Output of this function is passed to "one_hot_encode" argument of
            "Transform" function from Vantage Analytic Library.
    PARAMETERS:
        values:
            Required Argument.
            Specifies the values to code and optionally the name of the
            resulting output column.
            Note:
                1. If date values are entered as string, the keyword 'DATE' must precede
                   the date value, and do not enclose in single quotes OR
                   pass a datetime.date object.
                   For example,
                        value='DATE 1987-06-09'
                        value=date(1987, 6, 9)
                2. Use a dict to pass value when result output column is to be named.
                   key of the dictionary must be the value to code and value must be
                   either None, in case result output column is not to be named or a
                   string if it is to be named.
                   For example,
                      values = {"Male": M, "Female": None}
                      In the example above,
                        - we would like to name the output column as 'M' for one hot
                          encoded values for "Male" and
                        - for the one hot encoding values of "Female" we would like to
                          have the output name contain/same as that of "Female", thus
                          None is passed as a value.
            Types: bool, float, int, str, dict, datetime.date or list of booleans, floats, integers,
                   strings, datetime.date
        columns:
            Required Argument.
            Specifies the name of the column. Value passed to this argument
            also plays a crucial role in determining the output column name.
            Types: str
        style:
            Optional Argument.
            Specifies the one hot encoding style to use.
            Permitted Values: 'dummy', 'contrast'
            Default Value: 'dummy'
            Types: str
        reference_value:
            Required Argument when "style" is 'contrast', ignored otherwise.
            Specifies the reference value to use for 'contrast' style. If original
            value in the column is equal to the reference value then -1 is returned
            for the same.
            Types: bool, int, float, str, datetme.date
        out_columns:
            Optional Argument.
            Specifies the name of the output column. Value passed to this argument
            also plays a crucial role in determining the output column name.
            Types: str
        datatype:
            Optional Argument.
            Specifies the name of the intended datatype of the output column.
            Intended data types for the output column can be specified using either the
            teradatasqlalchemy types or the permitted strings mentioned below:
             -------------------------------------------------------------------
            | If intended SQL Data Type is  | Permitted Value to be passed is   |
            |-------------------------------------------------------------------|
            | bigint                        | bigint                            |
            | byteint                       | byteint                           |
            | char(n)                       | char,n                            |
            | date                          | date                              |
            | decimal(m,n)                  | decimal,m,n                       |
            | float                         | float                             |
            | integer                       | integer                           |
            | number(*)                     | number                            |
            | number(n)                     | number,n                          |
            | number(*,n)                   | number,*,n                        |
            | number(n,n)                   | number,n,n                        |
            | smallint                      | smallint                          |
            | time(p)                       | time,p                            |
            | timestamp(p)                  | timestamp,p                       |
            | varchar(n)                    | varchar,n                         |
            --------------------------------------------------------------------
            Notes:
                1. Argument is ignored if "columns" argument is not used.
                2. char without a size is not supported.
                3. number(*) does not include the * in its datatype format.
            Examples:
                1. If intended datatype for the output column is "bigint", then
                   pass string "bigint" to the argument as shown below:
                   datatype="bigint"
                2. If intended datatype for the output column is "decimal(3,5)", then
                   pass string "decimal,3,5" to the argument as shown below:
                   datatype="decimal,3,5"
            Types: str, BIGINT, BYTEINT, CHAR, DATE, DECIMAL, FLOAT, INTEGER, NUMBER, SMALLINT, TIME,
                   TIMESTAMP, VARCHAR.
        fillna:
            Optional Argument.
            Specifies whether the null replacement/missing value treatment should
            be performed with one hot encoding or not. Output of FillNa() can be
            passed to this argument.
            Note:
                If the FillNa object is created with its arguments "columns",
                "out_columns" and "datatype", then values passed in FillNa() arguments
                are ignored. Only nullstyle information is captured from the same.
            Types: FillNa
    NOTES:
        Output column names for the transformation using Transform() function depends
        on "values", "columns" and "out_columns" arguments. Here is how output column
        names are determined:
            1. If "values" is not dictionary:
                a. If "out_columns" is not passed, then output column is formed
                   using the value in "values" and column name passed to "columns".
                   For example,
                        If values=["val1", "val2"] and columns="col"
                        then, output column names are:
                            'val1_col' and 'val2_col'
                b. If "out_columns" is passed, then output column is formed
                   using the value in "values" and column name passed to "out_columns".
                   For example,
                        If values=["val1", "val2"], columns="col", and
                        out_columns="ocol" then, output column names are:
                            'val1_ocol' and 'val2_ocol'
            2. If "values" is a dictionary:
                a. If value in a dictionary is not None, then that value is used
                   as output column name.
                   For example:
                        If values = {"val1": "v1"} then output column name is "v1".
                b. If value in a dictionary is None, then rules specified in point 1
                   are applied to determine the output column name.
    RETURNS:
        An instance of OneHotEncoder class.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLE:
        # Note:
        #   To run any transformation, user needs to use Transform() function from
        #   Vantage Analytic Library.
        #   To do so import valib first and set the "val_install_location".
        >>> from teradataml import configure, DataFrame, OneHotEncoder, FillNa, load_example_data, valib
        >>> configure.val_install_location = "SYSLIB"
        >>>
        # Load example data.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create the required DataFrame.
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
        >>>
        # Example 1: Encode all values 'Novice', 'Advanced', and 'Beginner'
        #            in "programming" column using "dummy" style.
        >>> dc = OneHotEncoder(values=["Novice", "Advanced", "Beginner"], columns="programming")
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, one_hot_encode=dc, key_columns="id")
        >>> obj.result
           id  Novice_programming  Advanced_programming  Beginner_programming
        0   5                   1                     0                     0
        1  34                   0                     0                     1
        2  13                   1                     0                     0
        3  40                   0                     0                     1
        4  22                   0                     0                     1
        5  19                   0                     1                     0
        6  36                   1                     0                     0
        7  15                   0                     1                     0
        8   7                   1                     0                     0
        9  17                   0                     1                     0
        >>>
        # Example 2: Encode all values 'Novice', 'Advanced', and 'Beginner'
        #            in "programming" column using "dummy" style. Also, pass
        #            "out_columns" argument, to control the name of the output column.
        >>> dc = OneHotEncoder(style="dummy", values=["Novice", "Advanced", "Beginner"],
        ...                    columns="programming", out_columns="prog")
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, one_hot_encode=dc, key_columns="id")
        >>> obj.result
           id  Novice_prog  Advanced_prog  Beginner_prog
        0  15            0              1              0
        1   7            1              0              0
        2  22            0              0              1
        3  17            0              1              0
        4  13            1              0              0
        5  38            0              0              1
        6  26            0              1              0
        7   5            1              0              0
        8  34            0              0              1
        9  40            0              0              1
        >>>
        # Example 3: Encode all values 'Novice', 'Advanced', and 'Beginner'
        #            in "programming" column using "dummy" style. Example shows
        #            why and how to pass values using dictionary. By passing dictionary,
        #            we should be able to control the name of the output columns.
        #            In this example, we would like to name the output column for
        #            value 'Advanced' as 'Adv', 'Beginner' as 'Beg' and for 'Novice'
        #            we would like to use default mechanism.
        >>> values = {"Novice": None, "Advanced": "Adv", "Beginner": "Beg"}
        >>> dc = OneHotEncoder(style="dummy", values=values, columns="programming")
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, one_hot_encode=dc, key_columns="id")
        >>> obj.result
           id  Novice_programming  Adv  Beg
        0  13                   1    0    0
        1  26                   0    1    0
        2   5                   1    0    0
        3  19                   0    1    0
        4  15                   0    1    0
        5  40                   0    0    1
        6   7                   1    0    0
        7  22                   0    0    1
        8  36                   1    0    0
        9  38                   0    0    1
        >>>
        # Example 4: Encode all values 'Novice', 'Advanced', and 'Beginner'
        #            in "programming" column using "dummy" style.
        #            Example shows controling of the output column name with dictionary
        #            and "out_columns" argument.
        #            In this example, we would like to name the output column for
        #            value 'Advanced' as 'Adv', 'Beginner' as 'Beg', 'Novice' as 'Nov_prog'.
        >>> values = {"Novice": None, "Advanced": "Adv", "Beginner": "Beg"}
        >>> dc = OneHotEncoder(style="dummy", values=values, columns="programming",
        ...                    out_columns="prog")
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, one_hot_encode=dc, key_columns="id")
        >>> obj.result
           id  Novice_prog  Adv  Beg
        0  15            0    1    0
        1   7            1    0    0
        2  22            0    0    1
        3  17            0    1    0
        4  13            1    0    0
        5  38            0    0    1
        6  26            0    1    0
        7   5            1    0    0
        8  34            0    0    1
        9  40            0    0    1
        >>>
        # Example 5: Encode 'yes' value in "masters" column using "contrast" style
        #            with reference value as 0.
        >>> dc = OneHotEncoder(style="contrast", values="yes", reference_value=0,
        ...                    columns="masters")
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, one_hot_encode=dc, key_columns="id")
        >>> obj.result
           id  yes_masters
        0  15            1
        1   7            1
        2  22            1
        3  17            0
        4  13            0
        5  38            1
        6  26            1
        7   5            0
        8  34            1
        9  40            1
        >>>
        # Example 6: Encode all values in "programming" column using "contrast" style
        #            with reference_value as 'Advanced'.
        >>> values = {"Advanced": "Adv", "Beginner": "Beg", "Novice": "Nov"}
        >>> dc = OneHotEncoder(style="contrast", values=values, reference_value="Advanced",
        ...                    columns="programming")
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, one_hot_encode=dc, key_columns="id")
        >>> obj.result
           id  Adv  Beg  Nov
        0  15    1   -1   -1
        1   7    0    0    1
        2  22    0    1    0
        3  17    1   -1   -1
        4  13    0    0    1
        5  38    0    1    0
        6  26    1   -1   -1
        7   5    0    0    1
        8  34    0    1    0
        9  40    0    1    0
        >>>
        # Example 7: Example shows combining multiple one hot encoding styles on
        #            different columns.
        # Encode all values in 'programming' column using 'dummy' encoding style.
        >>> dc_prog_dummy = OneHotEncoder(values=["Novice", "Advanced", "Beginner"],
        ...                               columns="programming", out_columns="prog")
        >>>
        # Encode all values in 'stats' column using 'dummy' encoding style.
        # Also, combine it with null replacement.
        >>> values = {"Advanced": "Adv", "Beginner": "Beg"}
        >>> fillna = FillNa("literal", "Advanced")
        >>> dc_stats_dummy = OneHotEncoder(values=values, columns="stats", fillna=fillna)
        >>>
        # Encode 'yes' in 'masters' column using 'contrast' encoding style.
        # Reference value used is 'no'.
        >>> dc_mast_contrast = OneHotEncoder(style="contrast", values="yes", reference_value="no",
        ...                                  columns="masters")
        >>>
        # Encode all values in 'programming' column using 'contrast' encoding style.
        # Reference value used is 'Advanced'.
        >>> dc_prog_contrast = OneHotEncoder(style="contrast",
        ...                                  values=["Novice", "Advanced", "Beginner"],
        ...                                  reference_value="Advanced",
        ...                                  columns="programming")
        >>>
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df,
        ...                       one_hot_encode=[dc_prog_dummy, dc_stats_dummy,
        ...                                   dc_mast_contrast, dc_prog_contrast],
        ...                       key_columns="id")
        >>> obj.result
           id  Novice_prog  Advanced_prog  Beginner_prog  Adv  Beg  yes_masters  Novice_programming  Advanced_programming  Beginner
        0  13            1              0              0    1    0           -1                   1                     0          
        1  26            0              1              0    1    0            1                  -1                     1          
        2   5            1              0              0    0    0           -1                   1                     0          
        3  19            0              1              0    1    0            1                  -1                     1          
        4  15            0              1              0    1    0            1                  -1                     1          
        5  40            0              0              1    0    0            1                   0                     0          
        6   7            1              0              0    0    0            1                   1                     0          
        7  22            0              0              1    0    0            1                   0                     0          
        8  36            1              0              0    1    0           -1                   1                     0          
        9  38            0              0              1    1    0            1                   0                     0          
        >>>
    Retain
    teradataml.analytics.Transformations.Retain.__init__ = __init__(self, columns, out_columns=None, datatype=None)
    DESCRIPTION:
        Retain option allows you to copy one or more columns into the final
        analytic data set. By default, the result column name is the same as
        the input column name, but this can be changed. If a specific type is
        specified, it results in casting the retained column.
        The Retain transformation is supported for all valid data types.
        Note:
            Output of this function is passed to "retain" argument of "Transform"
            function from Vantage Analytic Library.
    PARAMETERS:
        columns:
            Required Argument.
            Specifies the names of the columns to retain.
            Types: str or list of str
        out_columns:
            Optional Argument.
            Specifies the names of the output columns.
            Note:
                Number of elements in "columns" and "out_columns" must be same.
            Types: str or list of str
        datatype:
            Optional Argument.
            Specifies the name of the intended datatype of the output column.
            Intended data types for the output column can be specified using either the
            teradatasqlalchemy types or the permitted strings mentioned below:
             -------------------------------------------------------------------
            | If intended SQL Data Type is  | Permitted Value to be passed is   |
            |-------------------------------------------------------------------|
            | bigint                        | bigint                            |
            | byteint                       | byteint                           |
            | char(n)                       | char,n                            |
            | date                          | date                              |
            | decimal(m,n)                  | decimal,m,n                       |
            | float                         | float                             |
            | integer                       | integer                           |
            | number(*)                     | number                            |
            | number(n)                     | number,n                          |
            | number(*,n)                   | number,*,n                        |
            | number(n,n)                   | number,n,n                        |
            | smallint                      | smallint                          |
            | time(p)                       | time,p                            |
            | timestamp(p)                  | timestamp,p                       |
            | varchar(n)                    | varchar,n                         |
            --------------------------------------------------------------------
            Notes:
                1. Argument is ignored if "columns" argument is not used.
                2. char without a size is not supported.
                3. number(*) does not include the * in its datatype format.
            Examples:
                1. If intended datatype for the output column is "bigint", then
                   pass string "bigint" to the argument as shown below:
                   datatype="bigint"
                2. If intended datatype for the output column is "decimal(3,5)", then
                   pass string "decimal,3,5" to the argument as shown below:
                   datatype="decimal,3,5"
            Types: str, BIGINT, BYTEINT, CHAR, DATE, DECIMAL, FLOAT, INTEGER, NUMBER, SMALLINT, TIME,
                   TIMESTAMP, VARCHAR.
    RETURNS:
        An instance of Retain class.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLE:
        # Note:
        #   To run any transformation, user needs to use Transform() function from
        #   Vantage Analytic Library.
        #   To do so import valib first and set the "val_install_location".
        >>> from teradataml import configure, DataFrame, load_example_data, valib, Retain
        >>> configure.val_install_location = "SYSLIB"
        >>>
        # Load example data.
        >>> load_example_data("dataframe", "sales")
        >>>
        # Create the required DataFrames.
        >>> sales = DataFrame("sales")
        >>> sales
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        >>>
        # Example: Shows retaining some column unchanged and some with name or datatype
        #          change.
        # Retain columns "accounts" and "Feb" as is.
        >>> rt_1 = Retain(columns=["accounts", "Feb"])
        >>>
        # Retain column "Jan" with name as "january".
        >>> rt_2 = Retain(columns="Jan", out_columns="january")
        >>>
        # Retain column "Mar" and "Apr" with name as "march" and "april" with
        # datatype changed to 'bigint'.
        >>> rt_3 = Retain(columns=["Mar", "Apr"], out_columns=["march", "april"],
        ...               datatype="bigint")
        >>>
        # Execute Transform() function.
        >>> obj = valib.Transform(data=sales, retain=[rt_1, rt_2, rt_3])
        >>> obj.result
             accounts   accounts1    Feb  january  march  april
        0    Alpha Co    Alpha Co  210.0    200.0  215.0  250.0
        1    Blue Inc    Blue Inc   90.0     50.0   95.0  101.0
        2  Yellow Inc  Yellow Inc   90.0      NaN    NaN    NaN
        3   Jones LLC   Jones LLC  200.0    150.0  140.0  180.0
        4     Red Inc     Red Inc  200.0    150.0  140.0    NaN
        5  Orange Inc  Orange Inc  210.0      NaN    NaN  250.0
        >>>
    Sigmoid
    teradataml.analytics.Transformations.Sigmoid.__init__ = __init__(self, columns, style='logit', out_columns=None, datatype=None, ﬁllna=None)
    DESCRIPTION:
        Sigmoid transformation allows rescaling of continuous numeric data in a more
        sophisticated way than the Rescaling transformation function. In a Sigmoid
        transformation, a numeric column is transformed using a type of sigmoid or
        s-shaped function.
        These non-linear transformations are more useful in data mining than a linear
        Rescaling transformation. The Sigmoid transformation is supported for numeric
        columns only.
        For absolute values of x greater than or equal to  36, the value of the
        sigmoid function is effectively 1 for positive arguments or 0 for negative
        arguments, within about 15 digits of significance.
        Note:
            Output of this function is passed to "sigmoid" argument of "Transform"
            function from Vantage Analytic Library.
    PARAMETERS:
        columns:
            Required Argument.
            Specifies the names of the columns to scale.
            Types: str or list of str
        style:
            Optional Argument.
            Specifies the style of sigmoid function to use.
            Permitted Values:
                * "logit":
                    The logit function produces a continuously increasing value
                    between 0 and 1.
                * "modifiedlogit":
                    The modified logit function is twice the logit minus 1 and
                    produces a value between -1 and 1.
                * "tanh":
                    The hyperbolic tangent function also produces a value between
                    -1 and 1.
            Default Value: 'logit'
            Types: str
        out_columns:
            Optional Argument.
            Specifies the names of the output columns.
            Note:
                Number of elements in "columns" and "out_columns" must be same.
            Types: str or list of str
        datatype:
            Optional Argument.
            Specifies the name of the intended datatype of the output column.
            Intended data types for the output column can be specified using either the
            teradatasqlalchemy types or the permitted strings mentioned below:
             -------------------------------------------------------------------
            | If intended SQL Data Type is  | Permitted Value to be passed is   |
            |-------------------------------------------------------------------|
            | bigint                        | bigint                            |
            | byteint                       | byteint                           |
            | char(n)                       | char,n                            |
            | date                          | date                              |
            | decimal(m,n)                  | decimal,m,n                       |
            | float                         | float                             |
            | integer                       | integer                           |
            | number(*)                     | number                            |
            | number(n)                     | number,n                          |
            | number(*,n)                   | number,*,n                        |
            | number(n,n)                   | number,n,n                        |
            | smallint                      | smallint                          |
            | time(p)                       | time,p                            |
            | timestamp(p)                  | timestamp,p                       |
            | varchar(n)                    | varchar,n                         |
            --------------------------------------------------------------------
            Notes:
                1. Argument is ignored if "columns" argument is not used.
                2. char without a size is not supported.
                3. number(*) does not include the * in its datatype format.
            Examples:
                1. If intended datatype for the output column is "bigint", then
                   pass string "bigint" to the argument as shown below:
                   datatype="bigint"
                2. If intended datatype for the output column is "decimal(3,5)", then
                   pass string "decimal,3,5" to the argument as shown below:
                   datatype="decimal,3,5"
            Types: str, BIGINT, BYTEINT, CHAR, DATE, DECIMAL, FLOAT, INTEGER, NUMBER, SMALLINT, TIME,
                   TIMESTAMP, VARCHAR.
        fillna:
            Optional Argument.
            Specifies whether the null replacement/missing value treatment should
            be performed with sigmoid transformation or not. Output of FillNa() can be
            passed to this argument.
            Note:
                If the FillNa object is created with its arguments "columns",
                "out_columns" and "datatype", then values passed in FillNa() arguments
                are ignored. Only nullstyle information is captured from the same.
            Types: FillNa
    RETURNS:
        An instance of Sigmoid class.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLE:
        # Note:
        #   To run any transformation, user needs to use Transform() function from
        #   Vantage Analytic Library.
        #   To do so import valib first and set the "val_install_location".
        >>> from teradataml import configure, DataFrame, FillNa, Sigmoid, load_example_data, valib
        >>> configure.val_install_location = "SYSLIB"
        >>>
        # Load example data.
        >>> load_example_data("dataframe", "sales")
        >>>
        # Create the required teradataml DataFrame.
        >>> sales = DataFrame("sales")
        >>> sales
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        >>>
        # Example 1: Scale values in columns "Jan" and "Mar" using sigmoid function "tanh".
        #            Combine the scaling with null replacement.
        >>> fn = FillNa(style="literal", value=0)
        >>> sig = Sigmoid(style="tanh", columns=["Jan", "Mar"], fillna=fn)
        # Execute Transform() function.
        >>> obj = valib.Transform(data=sales, sigmoid=sig, key_columns="accounts")
        >>> obj.result
             accounts  Jan  Mar
        0    Alpha Co  1.0  1.0
        1     Red Inc  1.0  1.0
        2  Orange Inc  0.0  0.0
        3   Jones LLC  1.0  1.0
        4  Yellow Inc  0.0  0.0
        5    Blue Inc  1.0  1.0
        >>>
        # Example 2: Rescaling with Sigmoid is carried out with multiple styles.
        >>> load_example_data("dataframe", "iris_test")
        >>> df = DataFrame("iris_test")
        >>> df
             sepal_length  sepal_width  petal_length  petal_width  species
        id
        5             5.0          3.6           1.4          0.2        1
        60            5.2          2.7           3.9          1.4        2
        15            5.8          4.0           1.2          0.2        1
        30            4.7          3.2           1.6          0.2        1
        40            5.1          3.4           1.5          0.2        1
        80            5.7          2.6           3.5          1.0        2
        120           6.0          2.2           5.0          1.5        3
        70            5.6          2.5           3.9          1.1        2
        20            5.1          3.8           1.5          0.3        1
        65            5.6          2.9           3.6          1.3        2
        >>>
        # Rescale values in columns "sepal_length", "sepal_width", "petal_length"
        # and "petal_width" with 'logit' (default) sigmoid function.
        >>> sig_1 = Sigmoid(columns=["sepal_length", "sepal_width", "petal_length",
        ...                          "petal_width"],
        ...                 out_columns=["sl", "sw", "pl", "pw"])
        >>>
        # Rescale values in columns "sepal_length", "sepal_width", "petal_length"
        # and "petal_width" with 'tanh' sigmoid function.
        >>> sig_2 = Sigmoid(style="tanh",
        ...                 columns=["sepal_length", "sepal_width", "petal_length",
        ...                          "petal_width"],
        ...                 out_columns=["sl_t", "sw_t", "pl_t", "pw_t"])
        >>>
        # Rescale values in columns "sepal_length" and "sepal_width" with 'modifiedlogit'
        # sigmoid function.
        # Combine it with null replacement using 'median' style.
        >>> fn = FillNa(style="median")
        >>> sig_3 = Sigmoid(style="modifiedlogit", columns=["sepal_length", "sepal_width"],
        ...                 out_columns=["sl_ml", "sw_ml"], fillna=fn)
        >>>
        # Execute Transform() function.
        >>> obj = valib.Transform(data=df, sigmoid=[sig_1, sig_2, sig_3],
        ...                       key_columns="id")
        >>> obj.result
            id        sl        sw        pl        pw      sl_t      sw_t      pl_t      pw_t     sl_ml     sw_ml
        0    5  0.993307  0.973403  0.802184  0.549834  0.999909  0.998508  0.885352  0.197375  0.986614  0.946806
        1   60  0.994514  0.937027  0.980160  0.802184  0.999939  0.991007  0.999181  0.885352  0.989027  0.874053
        2   15  0.996982  0.982014  0.768525  0.549834  0.999982  0.999329  0.833655  0.197375  0.993963  0.964028
        3   30  0.990987  0.960834  0.832018  0.549834  0.999835  0.996682  0.921669  0.197375  0.981973  0.921669
        4   40  0.993940  0.967705  0.817574  0.549834  0.999926  0.997775  0.905148  0.197375  0.987880  0.935409
        5   80  0.996665  0.930862  0.970688  0.731059  0.999978  0.989027  0.998178  0.761594  0.993330  0.861723
        6  120  0.997527  0.900250  0.993307  0.817574  0.999988  0.975743  0.999909  0.905148  0.995055  0.800499
        7   70  0.996316  0.924142  0.980160  0.750260  0.999973  0.986614  0.999181  0.800499  0.992632  0.848284
        8   20  0.993940  0.978119  0.817574  0.574443  0.999926  0.999000  0.905148  0.291313  0.987880  0.956237
        9   65  0.996316  0.947846  0.973403  0.785835  0.999973  0.993963  0.998508  0.861723  0.992632  0.895693
        >>>
    ZScore
    teradataml.analytics.Transformations.ZScore.__init__ = __init__(self, columns, out_columns=None, datatype=None, ﬁllna=None)
    DESCRIPTION:
        ZScore will allows rescaling of continuous numeric data in a more
        sophisticated way than a Rescaling transformation. In a Z-Score
        transformation, a numeric column is transformed into its Z-score based
        on the mean value and standard deviation of the data in the column.
        Z-Score transforms each column value into the number of standard
        deviations from the mean value of the column. This non-linear transformation
        is useful in data mining rather than in a linear Rescaling transformation.
        The Z-Score transformation supports both numeric and date type input data.
        Note:
            Output of this function is passed to "zscore" argument of "Transform"
            function from Vantage Analytic Library.
    PARAMETERS:
        columns:
            Required Argument.
            Specifies the name(s) of the column(s) to perform transformation on.
            Types: str or list of str
        out_columns:
            Optional Argument.
            Specifies the names of the output columns.
            Note:
                Number of elements in "columns" and "out_columns" must be same.
            Types: str or list of str
        datatype:
            Optional Argument.
            Specifies the name of the intended datatype of the output column.
            Intended data types for the output column can be specified using either the
            teradatasqlalchemy types or the permitted strings mentioned below:
             -------------------------------------------------------------------
            | If intended SQL Data Type is  | Permitted Value to be passed is   |
            |-------------------------------------------------------------------|
            | bigint                        | bigint                            |
            | byteint                       | byteint                           |
            | char(n)                       | char,n                            |
            | date                          | date                              |
            | decimal(m,n)                  | decimal,m,n                       |
            | float                         | float                             |
            | integer                       | integer                           |
            | number(*)                     | number                            |
            | number(n)                     | number,n                          |
            | number(*,n)                   | number,*,n                        |
            | number(n,n)                   | number,n,n                        |
            | smallint                      | smallint                          |
            | time(p)                       | time,p                            |
            | timestamp(p)                  | timestamp,p                       |
            | varchar(n)                    | varchar,n                         |
            --------------------------------------------------------------------
            Notes:
                1. Argument is ignored if "columns" argument is not used.
                2. char without a size is not supported.
                3. number(*) does not include the * in its datatype format.
            Examples:
                1. If intended datatype for the output column is "bigint", then
                   pass string "bigint" to the argument as shown below:
                   datatype="bigint"
                2. If intended datatype for the output column is "decimal(3,5)", then
                   pass string "decimal,3,5" to the argument as shown below:
                   datatype="decimal,3,5"
            Types: str, BIGINT, BYTEINT, CHAR, DATE, DECIMAL, FLOAT, INTEGER, NUMBER, SMALLINT, TIME,
                   TIMESTAMP, VARCHAR.
        fillna:
            Optional Argument.
            Specifies whether the null replacement/missing value treatment should
            be performed with Z-Score transformation or not. Output of 'FillNa()'
            can be passed to this argument.
            Note:
                If the FillNa object is created with its arguments "columns",
                "out_columns" and "datatype", then values passed in FillNa() arguments
                are ignored. Only nullstyle information is captured from the same.
            Types: FillNa
    RETURNS:
        An instance of ZScore class.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLE:
        # Note:
        #   To run any transformation, user needs to use Transform() function from
        #   Vantage Analytic Library.
        #   To do so import valib first and set the "val_install_location".
        >>> from teradataml import configure, DataFrame, FillNa, load_example_data, valib, ZScore
        >>> configure.val_install_location = "SYSLIB"
        >>>
        # Load example data.
        >>> load_example_data("dataframe", "sales")
        >>>
        # Create the required DataFrames.
        >>> sales = DataFrame("sales")
        >>> sales
                      Feb    Jan    Mar    Apr    datetime
        accounts
        Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
        Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
        Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
        Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
        Red Inc     200.0  150.0  140.0    NaN  04/01/2017
        Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
        >>>
        # Example 1: Rescaling with ZScore is carried out on "Feb" column.
        >>> zs = ZScore(columns="Feb")
        # Execute Transform() function.
        >>> obj = valib.Transform(data=sales, zscore=zs)
        >>> obj.result
             accounts       Feb
        0    Blue Inc -1.410220
        1    Alpha Co  0.797081
        2   Jones LLC  0.613139
        3  Yellow Inc -1.410220
        4  Orange Inc  0.797081
        5     Red Inc  0.613139
        >>>
        # Example 2: Rescaling with ZScore is carried out with multiple columns "Jan"
        #            and "Apr" with null replacement using "mode" style.
        >>> fn = FillNa(style="mode")
        >>> zs = ZScore(columns=["Jan", "Apr"], out_columns=["january", "april"], fillna=fn)
        # Execute Transform() function.
        >>> obj = valib.Transform(data=sales, zscore=zs, key_columns="accounts")
        >>> obj.result
             accounts   january     april
        0    Blue Inc -2.042649 -1.993546
        1    Alpha Co  1.299867  0.646795
        2   Jones LLC  0.185695 -0.593634
        3  Yellow Inc  0.185695  0.646795
        4  Orange Inc  0.185695  0.646795
        5     Red Inc  0.185695  0.646795
        >>>