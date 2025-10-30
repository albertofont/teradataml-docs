# teradataml Bring Your Own Analytics

DataikuPredict
DataikuPredict
Functions
DataikuPredict(modeldata=None, newdata=None, accumulate=None, model_output_ﬁelds=None, overwrite_cached_models=False, is_debug=False, **generic_arg
DESCRIPTION:
    The DataikuPredict() function is used to score data in Vantage
    with a model that has been created outside Vantage and exported
    to Vantage using Dataiku format.
PARAMETERS:
    modeldata:
        Required Argument.
        Specifies the model teradataml DataFrame to be used for
        scoring.
        Types: teradataml DataFrame
    newdata:
        Required Argument.
        Specifies the input teradataml DataFrame that contains
        the data to be scored.
        Types: teradataml DataFrame
    accumulate:
        Required Argument.
        Specifies the name(s) of input teradataml DataFrame column(s) to
        copy to the output. By default, the function copies all input
        teradataml DataFrame columns to the output.
        Types: str OR list of Strings (str) OR Feature OR list of Features
    model_output_fields:
        Optional Argument.
        Specifies the columns of the json output that the user wants
        to specify as individual columns instead of the entire json
        report.
        Types: str OR list of Strings (str)
    overwrite_cached_models:
        Optional Argument.
        Specifies the model name that needs to be removed from the cache.
        If a model is loaded into the memory of the node fits in the cache,
        it stays in the cache until being evicted to make space for another
        model that needs to be loaded. Therefore, a model can remain in the
        cache even after completion of function call. Other queries that
        use the same model can use it, saving the cost of reloading it
        into memory. User may overwrite a cached model only when it has been
        updated, to make sure that the Predict function uses the updated
        model instead of the cached model.
        Note:
            Do not use the "overwrite_cached_models" argument except when trying
            to replace a previously cached model. This applies to any model type
            (PMML, H2O Open Source, DAI, ONNX, and Dataiku). Using this argument
            in other cases, including in concurrent queries or multiple times
            within a short period of time, may lead to an OOM error from garbage
            collection not being fast enough.
        Permitted Values:
            'current_cached_model', '*', 'true', 't', 'yes', '1', 'false', 'f', 'no',
             'n', or '0'.
        Default Values: "false"
        Types: bool
    is_debug:
        Optional Argument.
        Specifies whether debug statements are added to a trace table or not.
        When set to True, debug statements are added to a trace table that must
        be created beforehand.
        Notes:
            * Only available with BYOM version 3.00.00.02 and later.
            * To save logs for debugging, user can create an error log by using
              the is_debug=True parameter in the predict functions.
              A database trace table is used to collect this information which
              does impact performance of the function, so using small data input
              sizes is recommended.
            * To generate this log, user must do the following:
                  1. Create a global trace table with columns vproc_ID BYTE(2),
                     Sequence INTEGER, Trace_Output VARCHAR(31000)
                  2. Turn on session function tracing:
                       SET SESSION FUNCTION TRACE USING '' FOR TABLE <trace_table_name_created_in_step_1>;
                  3. Execute function with "is_debug" set to True.
                  4. Debug information is logged to the table created in step 1.
                  5. To turn off the logging, either disconnect from the session or
                     run following SQL:
                       SET SESSION FUNCTION TRACE OFF;
                  The trace table is temporary and the information is deleted if user
                  logs off from the session. If long term persistence is necessary,
                  user can copy the table to a permanent table before leaving the
                  session.
        Default Value: False
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept. Below
        are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the
                function in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Default Value: False
                Types: bool
            volatile:
                Optional Argument.
                Specifies whether to put the results of the
                function in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
        Function allows the user to partition, hash, order or local
        order the input data. These generic arguments are available
        for each argument that accepts teradataml DataFrame as
        input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or
              list of str (Strings) or PartitionKind
            * "<input_data_arg_name>_hash_column" accepts str or list
              of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list
              of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if
            the underlying SQL Engine function supports, else an
            exception is raised.
RETURNS:
    Instance of DataikuPredict.
    Output teradataml DataFrame can be accessed using attribute
    references, such as  DataikuPredictObj.<attribute_name>.
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
    #     4. To execute BYOM functions, set 'configure.byom_install_location' to the
    #        database name where BYOM functions are installed.
    # Import required libraries / functions.
    import os, teradataml
    from teradataml import get_connection, DataFrame
    from teradataml import save_byom, retrieve_byom, load_example_data
    from teradataml import configure, display_analytic_functions, execute_sql
    # Load example data.
    load_example_data("byom", "iris_test")
    # Create teradataml DataFrame objects.
    iris_test = DataFrame.from_table("iris_test")
    # Set install location of BYOM functions.
    configure.byom_install_location = "mldb"
    # Check the list of available analytic functions.
    display_analytic_functions(type="BYOM")
    # Load model file into Vantage.
    model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
                              "models", "dataiku_iris_data_ann_thin")
    save_byom("dataiku_iris_data_ann_thin", model_file, "byom_models")
    # Retrieve model.
    modeldata = retrieve_byom("dataiku_iris_data_ann_thin", table_name="byom_models")
    # Example 1: Score data in Vantage with a model that has
    #            been created outside the Vantage by removing all the
    #            all cached models.
    DataikuPredict_out_1 = DataikuPredict(newdata=iris_test,
                                          modeldata=modeldata,
                                          accumulate=['id', 'sepal_length', 'petal_length'],
                                          overwrite_cached_models="*")
    # Print the results.
    print(DataikuPredict_out_1.result)
    # Example 2: Example to show case the trace table usage using
    #            is_debug=True.
    # Create the trace table.
    crt_tbl_query = 'CREATE GLOBAL TEMPORARY TRACE TABLE BYOM_Trace                         (vproc_ID       BYTE(2)   
    execute_sql(crt_tbl_query)
    # Turn on the session function.
    execute_sql("SET SESSION FUNCTION TRACE USING '' FOR TABLE BYOM_Trace;")
    # Execute the DataikuPredict() function using is_debug=True.
    DataikuPredict_out_2 = DataikuPredict(newdata=iris_test,
                                          modeldata=modeldata,
                                          accumulate=['id', 'sepal_length', 'petal_length'],
                                          overwrite_cached_models="*",
                                          is_debug=True)
    # Print the results.
    print(DataikuPredict_out_2.result)
    # View the trace table information.
    trace_df = DataFrame.from_table("BYOM_Trace")
    print(trace_df)
    # Turn off the session function
    execute_sql("SET SESSION FUNCTION TRACE OFF;")
DataRobotPredict
DataRobotPredict
Functions
DataRobotPredict(modeldata=None, newdata=None, accumulate=None, model_output_ﬁelds=None, overwrite_cached_models=False, is_debug=False,
**generic_arguments)
DESCRIPTION:
    The DataRobotPredict() function is used to score data in Vantage with a model that has been
    created outside Vantage and exported to Vantage using DataRobot format.
PARAMETERS:
    modeldata:
        Required Argument.
        Specifies the model teradataml DataFrame to be used for scoring.
        Types: teradataml DataFrame
    newdata:
        Required Argument.
        Specifies the input teradataml DataFrame that contains the data to be scored.
        Types: teradataml DataFrame
        Note:
            The input columns containing Date or Timestamp types should be converted to character type before
            running DatRobotPredict().
    accumulate:
        Required Argument.
        Specifies the name(s) of input teradataml DataFrame column(s) to
        copy to the output.
        Types: str OR list of Strings (str) OR Feature OR list of Features
    model_output_fields:
        Optional Argument.
        Specifies the columns of the json output that the user wants to
        specify as individual columns instead of the entire json report.
        Types: str OR list of Strings (str)
    overwrite_cached_models:
        Optional Argument.
        Specifies the model name that needs to be removed from the cache.
        When a model loaded into the memory of the node fits in the cache,
        it stays in the cache until being evicted to make space for another
        model that needs to be loaded. Therefore, a model can remain in the
        cache even after the completion of function execution. Other functions
        that use the same model can use it, saving the cost of reloading it
        into memory. User should overwrite a cached model only when it is updated,
        to make sure that the Predict function uses the updated model instead
        of the cached model.
        Note:
            Do not use the "overwrite_cached_models" argument except when user
            is trying to replace a previously cached model. Using the argument
            in other cases, including in concurrent queries or multiple times
            within a short period of time lead to an OOM error.
        Default behavior: The function does not overwrite cached models.
        Permitted Values: true, t, yes, y, 1, false, f, no, n, 0, *,
                          current_cached_model
        Types: str OR list of Strings (str)
    is_debug:
        Optional Argument.
        Specifies whether debug statements are added to a trace table or not.
        When set to True, debug statements are added to a trace table that must
        be created beforehand.
        Notes:
            * Only available with BYOM version 3.00.00.02 and later.
            * To save logs for debugging, user can create an error log by using
              the is_debug=True parameter in the predict functions.
              A database trace table is used to collect this information which
              does impact performance of the function, so using small data input
              sizes is recommended.
            * To generate this log, user must do the following:
                  1. Create a global trace table with columns vproc_ID BYTE(2),
                     Sequence INTEGER, Trace_Output VARCHAR(31000)
                  2. Turn on session function tracing:
                       SET SESSION FUNCTION TRACE USING '' FOR TABLE <trace_table_name_created_in_step_1>;
                  3. Execute function with "is_debug" set to True.
                  4. Debug information is logged to the table created in step 1.
                  5. To turn off the logging, either disconnect from the session or
                     run following SQL:
                       SET SESSION FUNCTION TRACE OFF;
                  The trace table is temporary and the information is deleted if user
                  logs off from the session. If long term persistence is necessary,
                  user can copy the table to a permanent table before leaving the
                  session.
        Default Value: False
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept. Below
        are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the
                function in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Default Value: False
                Types: bool
            volatile:
                Optional Argument.
                Specifies whether to put the results of the
                function in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
        Function allows the user to partition, hash, order or local
        order the input data. These generic arguments are available
        for each argument that accepts teradataml DataFrame as
        input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or
              list of str (Strings) or PartitionKind
            * "<input_data_arg_name>_hash_column" accepts str or list
              of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list
              of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if
            the underlying SQL Engine function supports, else an
            exception is raised.
RETURNS:
    Instance of DataRobotPredict.
    Output teradataml DataFrames can be accessed using attribute
    references, such as DataRobotPredictObj.<attribute_name>.
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
    #     4. To execute BYOM functions, set 'configure.byom_install_location' to the
    #        database name where BYOM functions are installed.
    # Import required libraries / functions.
    import os, teradataml
    from teradataml import get_connection, DataFrame
    from teradataml import save_byom, retrieve_byom, load_example_data
    from teradataml import configure, display_analytic_functions, execute_sql
    # Load example data.
    load_example_data("byom", "iris_test")
    # Create teradataml DataFrame objects.
    iris_test = DataFrame.from_table("iris_test")
    # Set install location of BYOM functions.
    configure.byom_install_location = "mldb"
    # Check the list of available analytic functions.
    display_analytic_functions(type="BYOM")
    # Load model file into Vantage.
    model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
                              "models", "dr_iris_rf")
    save_byom("dr_iris_rf", model_file, "byom_models")
    # Retrieve model.
    modeldata = retrieve_byom("dr_iris_rf", table_name="byom_models")
    # Example 1: Score data in Vantage with a model that has
    #            been created outside the Vantage by removing all the
    #            all cached models.
    Datarobotpredict_1 = DataRobotPredict(newdata=iris_test,
                                          modeldata=modeldata,
                                          accumulate=['id', 'sepal_length', 'petal_length'],
                                          overwrite_cached_models="*")
    # Print the results.
    print(Datarobotpredict_1.result)
H2OPredict
H2OPredict
Functions
H2OPredict(modeldata=None, newdata=None, accumulate=None, model_output_ﬁelds=None, overwrite_cached_models=None, model_type='OpenSource', enable
DESCRIPTION:
    The H2OPredict() function performs a prediction on each row of the input table
    using a model previously trained in H2O and then loaded into the database.
    The model uses an interchange format called MOJO and it is loaded to
    Teradata database in a table by the user as a blob.
    The model data prepared by user should have a model id for each model
    (residing as a MOJO object) created by the user.
    H2OPredict() supports Driverless AI and H2O-3 MOJO models.
    H2O Driverless AI (DAI) provides a number of transformations.
    The following transformers are available for regression and classification
    (multiclass and binary) experiments:
        * Numeric
        * Categorical
        * Time and Date
        * Time Series
        * NLP (test)
        * Image
PARAMETERS:
    newdata:
        Required Argument.
        Specifies the input teradataml DataFrame that contains the data to be
        scored.
        Types: teradataml DataFrame
    modeldata:
        Required Argument.
        Specifies the model teradataml DataFrame to be used for scoring.
        Note:
            * Use `retrieve_byom()` to get the teradataml DataFrame that contains the model.
        Types: teradataml DataFrame
    accumulate:
        Required Argument.
        Specifies the name(s) of input teradataml DataFrame column(s)
        to copy to the output DataFrame.
        Types: str OR list of Strings (str) OR Feature OR list of Features
    model_output_fields:
        Optional Argument.
        Specifies the output fields to add as individual columns instead of the
        entire JSON output. Specify fields with a comma-separated list.
        Types: str OR list of Strings (str)
    overwrite_cached_models:
        Optional Argument.
        Specifies the model name that needs to be removed from the cache.
        When a model loaded into the memory of the node fits in the cache,
        it stays in the cache until being evicted to make space for another
        model that needs to be loaded. Therefore, a model can remain in the
        cache even after the completion of function execution. Other functions
        that use the same model can use it, saving the cost of reloading it
        into memory. User should overwrite a cached model only when it is updated,
        to make sure that the Predict function uses the updated model instead
        of the cached model.
        Note:
            Do not use the "overwrite_cached_models" argument except when user
            is trying to replace a previously cached model. Using the argument
            in other cases, including in concurrent queries or multiple times
            within a short period of time lead to an OOM error.
        Default behavior: The function does not overwrite cached models.
        Permitted Values: true, t, yes, y, 1, false, f, no, n, 0, *,
                          current_cached_model
        Types: str OR list of Strings (str)
    model_type:
        Optional Argument.
        Specifies the model type for H2O model prediction.
        Default Value: "OpenSource"
        Permitted Values: DAI, OpenSource
        Types: str OR list of Strings (str)
    enable_options:
        Optional Argument.
        Specifies feature option values to have them appear in the JSON output:
            * contributions: The contribution of each input feature
                             towards the prediction.
            * stageProbabilities: Prediction probabilities of trees in each
                                  stage or iteration.
            * leafNodeAssignments: The leaf placements of the row in all the
                                   trees in the tree-based model.
        When the feature options are not specified, the features are considered
        false and the following values are not populated in the output JSON:
            * contributions (applies only to binomial or regression models)
            * leafNodeAssignments and stageProbabilities (applies to binomial,
              regression, multinomial, and AnomalyDetection models)
        Permitted Values: contributions, stageProbabilities, leafNodeAssignments
        Types: str OR list of Strings (str)
    is_debug:
        Optional Argument.
        Specifies whether debug statements are added to a trace table or not.
        When set to True, debug statements are added to a trace table that must
        be created beforehand.
        Notes:
            * Only available with BYOM version 3.00.00.02 and later.
            * To save logs for debugging, user can create an error log by using
              the is_debug=True parameter in the predict functions.
              A database trace table is used to collect this information which
              does impact performance of the function, so using small data input
              sizes is recommended.
            * To generate this log, user must do the following:
                  1. Create a global trace table with columns vproc_ID BYTE(2),
                     Sequence INTEGER, Trace_Output VARCHAR(31000)
                  2. Turn on session function tracing:
                       SET SESSION FUNCTION TRACE USING '' FOR TABLE <trace_table_name_created_in_step_1>;
                  3. Execute function with "is_debug" set to True.
                  4. Debug information is logged to the table created in step 1.
                  5. To turn off the logging, either disconnect from the session or
                     run following SQL:
                       SET SESSION FUNCTION TRACE OFF;
                  The trace table is temporary and the information is deleted if user
                  logs off from the session. If long term persistence is necessary,
                  user can copy the table to a permanent table before leaving the
                  session.
        Default Value: False
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept. Below
        are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the
                function in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Default Value: False
                Types: bool
            volatile:
                Optional Argument.
                Specifies whether to put the results of the
                function in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
        Function allows the user to partition, hash, order or local
        order the input data. These generic arguments are available
        for each argument that accepts teradataml DataFrame as
        input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or
              list of str (Strings) or PartitionKind
            * "<input_data_arg_name>_hash_column" accepts str or list
              of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list
              of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if
            the underlying SQL Engine function supports, else an
            exception is raised.
RETURNS:
    Instance of H2OPredict.
    Output teradataml DataFrame can be accessed using attribute
    references, such as  H2OPredictObj.<attribute_name>.
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
    #     4. To execute BYOM functions, set 'configure.byom_install_location' to the
    #        database name where BYOM functions are installed.
    # Import required libraries / functions.
    import os, teradataml
    from teradataml import save_byom, retrieve_byom, load_example_data
    from teradataml import configure, display_analytic_functions, execute_sql
    # Load example data.
    load_example_data("byom", "iris_test")
    # Create teradataml DataFrame objects.
    iris_test = DataFrame.from_table("iris_test")
    # Set install location of BYOM functions.
    configure.byom_install_location = "mldb"
    # Check the list of available analytic functions.
    display_analytic_functions(type="BYOM")
    # Example 1: This example scores the data on Vantage using a GLM model generated
    #            outside of Vantage. The example performs prediction with H2OPredict
    #            function using this GLM model in mojo format generated by H2O.
    #            Corresponding values are specified for the "model_type", "enable_options",
    #            "model_output_fields" and "overwrite.cached.models". This will erase
    #            entire cache.
    # Load model file into Vantage.
    model_file = os.path.join(os.path.dirname(teradataml.__file__), "data", "models", "iris_mojo_glm_h2o_model")
    save_byom("iris_mojo_glm_h2o_model", model_file, "byom_models")
    # Retrieve model.
    modeldata = retrieve_byom("iris_mojo_glm_h2o_model", table_name="byom_models")
    result = H2OPredict(newdata=iris_test,
                        newdata_partition_column='id',
                        newdata_order_column='id',
                        modeldata=modeldata,
                        modeldata_order_column='model_id',
                        model_output_fields=['label', 'classProbabilities'],
                        accumulate=['id', 'sepal_length', 'petal_length'],
                        overwrite_cached_models='*',
                        enable_options='stageProbabilities',
                        model_type='OpenSource'
                        )
    # Print the results.
    print(result.result)
    # Example 2: This example scores the data on Vantage using a XGBoost model generated
    #            outside of Vantage. The example performs prediction with H2OPredict
    #            function using this XGBoost model in mojo format generated by H2O.
    #            Corresponding values are specified for the "model_type", "enable_options",
    #            "model_output_fields" and "overwrite.cached.models". This will erase
    #            entire cache.
    # Load model file into Vantage.
    model_file = os.path.join(os.path.dirname(teradataml.__file__), "data", "models", "iris_mojo_xgb_h2o_model")
    save_byom("iris_mojo_xgb_h2o_model", model_file, "byom_models")
    # Retrieve model.
    modeldata = retrieve_byom("iris_mojo_xgb_h2o_model", table_name="byom_models")
    result = H2OPredict(newdata=iris_test,
                        newdata_partition_column='id',
                        newdata_order_column='id',
                        modeldata=modeldata,
                        modeldata_order_column='model_id',
                        model_output_fields=['label', 'classProbabilities'],
                        accumulate=['id', 'sepal_length', 'petal_length'],
                        overwrite_cached_models='*',
                        enable_options='stageProbabilities',
                        model_type='OpenSource'
                        )
    # Print the results.
    print(result.result)
    # Example 3: Example performs prediction with H2OPredict function using the licensed
    #            model in mojo format generated outside of Vantage and with id
    #            'licensed_model1'from the data 'byom_licensed_models' and associated
    #            license key stored in column 'license_key' of the table 'license'
    #            present in the schema 'mldb'.
    # Retrieve model.
    modeldata = retrieve_byom('licensed_model1',
                              table_name='byom_licensed_models',
                              license='license_key',
                              is_license_column=True,
                              license_table_name='license',
                              license_schema_name='mldb')
    result = H2OPredict(newdata=iris_test,
                        newdata_partition_column='id',
                        newdata_order_column='id',
                        modeldata=modeldata,
                        modeldata_order_column='model_id',
                        model_output_fields=['label', 'classProbabilities'],
                        accumulate=['id', 'sepal_length', 'petal_length'],
                        overwrite_cached_models='*',
                        enable_options='stageProbabilities',
                        model_type='OpenSource'
                        )
    # Print the results.
    print(result.result)
    # Example 4: Example to show case the trace table usage using
    #            is_debug=True.
    # Create the trace table.
    crt_tbl_query = 'CREATE GLOBAL TEMPORARY TRACE TABLE BYOM_Trace                         (vproc_ID       BYTE(2)   
    execute_sql(crt_tbl_query)
    # Turn on tracing for the session.
    execute_sql("SET SESSION FUNCTION TRACE USING '' FOR TABLE BYOM_Trace;")
    modeldata = retrieve_byom("iris_mojo_glm_h2o_model", table_name="byom_models")
    # Execute the H2OPredict() function using is_debug=True.
    result = H2OPredict(newdata=iris_test,
                        newdata_partition_column='id',
                        newdata_order_column='id',
                        modeldata=modeldata,
                        modeldata_order_column='model_id',
                        model_output_fields=['label', 'classProbabilities'],
                        accumulate=['id', 'sepal_length', 'petal_length'],
                        overwrite_cached_models='*',
                        enable_options='stageProbabilities',
                        model_type='OpenSource',
                        is_debug=True
                        )
    # Print the results.
    print(result.result)
    # View the trace table information.
    trace_df = DataFrame.from_table("BYOM_Trace")
    print(trace_df)
    # Turn off tracing for the session.
    execute_sql("SET SESSION FUNCTION TRACE OFF;")
ONNXEmbeddings
ONNXEmbeddings
Functions
ONNXEmbeddings(newdata=None, modeldata=None, tokenizerdata=None, accumulate=None, model_output_tensor=None, encode_max_length=512,
show_model_properties=False, output_column_preﬁx='emb_', output_format='VARBYTE(3072)', overwrite_cached_models='false', is_debug=False,
enable_memory_check=False, **generic_arguments)
DESCRIPTION:
    The ONNXEmbeddings() function is used to calculate embeddings values in
    Vantage with a HuggingFace model that has been created outside Vantage
    and exported to Vantage using ONNX format.
PARAMETERS:
    newdata:
        Required Argument.
        Specifies the input teradataml DataFrame that contains
        the data to be scored.
        Types: teradataml DataFrame
    modeldata:
        Required Argument.
        Specifies the model teradataml DataFrame to be used for
        scoring.
        Note:
            * Use `retrieve_byom()` to get the teradataml DataFrame that contains the model.
        Types: teradataml DataFrame
    tokenizerdata:
        Required Argument.
        Specifies the tokenizer teradataml DataFrame
        which contains the tokenizer json file.
        Types: teradataml DataFrame
    accumulate:
        Required Argument.
        Specifies the name(s) of input teradataml DataFrame column(s) to
        copy to the output. By default, the function copies all input
        teradataml DataFrame columns to the output.
        Types: str OR list of Strings (str) OR Feature OR list of Features
    model_output_tensor:
        Required Argument.
        Specifies the column of the model's possible output fields
        that the user wants to calculate and output.
        Types: str
    encode_max_length:
        Optional Argument.
        Specifies the maximum length of the tokenizer output token
        encodings(only applies for models with symbolic dimensions).
        Default Value: 512
        Types: int
    show_model_properties:
        Optional Argument.
        Specifies the default or expanded "model_input_fields_map" based on
        input model for defaults or "model_input_fields_map" for expansion.
        Default Value: False
        Types: bool
    output_column_prefix:
        Optional Argument.
        Specifies the column prefix for each of the output columns
        when using float32 "output_format".
        Default Value: "emb_"
        Types: str
    output_format:
        Optional Argument.
        Specifies the output format for the model embeddings output.
        Default Value: "VARBYTE(3072)"
        Types: str
    overwrite_cached_models:
        Optional Argument.
        Specifies the model name that needs to be removed from the cache.
        When a model loaded into the memory of the node fits in the cache,
        it stays in the cache until being evicted to make space for another
        model that needs to be loaded. Therefore, a model can remain in the
        cache even after the completion of function execution. Other functions
        that use the same model can use it, saving the cost of reloading it
        into memory. User should overwrite a cached model only when it is updated,
        to make sure that the Predict function uses the updated model instead
        of the cached model.
        Note:
            Do not use the "overwrite_cached_models" argument except when user
            is trying to replace a previously cached model. Using the argument
            in other cases, including in concurrent queries or multiple times
            within a short period of time lead to an OOM error.
        Default Value: "false"
        Permitted Values: true, t, yes, y, 1, false, f, no, n, 0, *,
                          current_cached_model
        Types: str
    is_debug:
        Optional Argument.
        Specifies whether debug statements are added to a trace table or not.
        When set to True, debug statements are added to a trace table that must
        be created beforehand.
        Notes:
            * Only available with BYOM version 3.00.00.02 and later.
            * To save logs for debugging, user can create an error log by using
              the is_debug=True parameter in the predict functions.
              A database trace table is used to collect this information which
              does impact performance of the function, so using small data input
              sizes is recommended.
            * To generate this log, user must do the following:
                  1. Create a global trace table with columns vproc_ID BYTE(2),
                     Sequence INTEGER, Trace_Output VARCHAR(31000)
                  2. Turn on session function tracing:
                       SET SESSION FUNCTION TRACE USING '' FOR TABLE <trace_table_name_created_in_step_1>;
                  3. Execute function with "is_debug" set to True.
                  4. Debug information is logged to the table created in step 1.
                  5. To turn off the logging, either disconnect from the session or
                     run following SQL:
                       SET SESSION FUNCTION TRACE OFF;
                  The trace table is temporary and the information is deleted if user
                  logs off from the session. If long term persistence is necessary,
                  user can copy the table to a permanent table before leaving the
                  session.
        Default Value: False
        Types: bool
    enable_memory_check:
        Optional Argument.
        Specifies whether there is enough native memory for large models.
        Default Value: True
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept. Below
        are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the
                function in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Default Value: False
                Types: bool
            volatile:
                Optional Argument.
                Specifies whether to put the results of the
                function in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
        Function allows the user to partition, hash, order or local
        order the input data. These generic arguments are available
        for each argument that accepts teradataml DataFrame as
        input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or
              list of str (Strings) or PartitionKind
            * "<input_data_arg_name>_hash_column" accepts str or list
              of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list
              of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if
            the underlying SQL Engine function supports, else an
            exception is raised.
RETURNS:
    Instance of ONNXEmbeddings.
    Output teradataml DataFrame can be accessed using attribute
    references, such as  ONNXEmbeddings.<attribute_name>.
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
    #     4. To execute BYOM functions, set 'configure.byom_install_location' to the
    #        database name where BYOM functions are installed.
    # Import required libraries / functions.
    import os, teradataml
    from teradataml import get_connection, DataFrame
    from teradataml import save_byom, retrieve_byom, load_example_data
    from teradataml import configure, display_analytic_functions, execute_sql
    # Load example data.
    load_example_data("byom", "amazon_reviews_25")
    # Create teradataml DataFrame objects.
    amazon_reviews_25 = DataFrame.from_table("amazon_reviews_25")
    # Assigning txt column name to rev_txt column.
    amazon_reviews_25 = amazon_reviews_25.assign(txt=amazon_reviews_25.rev_text)
    # Set install location of BYOM functions.
    configure.byom_install_location = "td_mldb"
    # Check the list of available analytic functions.
    display_analytic_functions(type="BYOM")
    # Retrieve model.
    modeldata = retrieve_byom("bge-small-en-v1.5", table_name="onnx_models")
    tokenizerdata = retrieve_byom("bge-small-en-v1.5", table_name="embeddings_tokenizers")
    # Assigning tokenizer_id, tokenizer to model_id, model in embeddings_tokenizers.
    tokenizerdata_a1 = tokenizerdata.assign(tokenizer_id=tokenizerdata.model_id)
    tokenizerdata_a2 = tokenizerdata_a1.assign(tokenizer=tokenizerdata_a1.model)
    # Example 1: Calculate embedding values in Vantage with a bge-small-en-v1.5
    #            model that has been created outside the Vantage by removing all
    #            the all cached models.
    ONNXEmbeddings_out_1 = ONNXEmbeddings(modeldata=modeldata,
                                          tokenizerdata=tokenizerdata_a2.select(['tokenizer_id', 'tokenizer']),
                                          newdata=amazon_reviews_25.select(["rev_id", "txt"]),
                                          accumulate='rev_id',
                                          model_output_tensor='sentence_embedding'
                                          )
    # Print the results.
    print(ONNXEmbeddings_out_1.result)
    # Example 2: Showcasing the model properties of bge-small-en-v1.5 model that has been
    #            created outside the Vantage by showcasing.
    ONNXEmbeddings_out_2 = ONNXEmbeddings(modeldata=modeldata,
                                          tokenizerdata=tokenizerdata_a2.select(['tokenizer_id', 'tokenizer']),
                                          newdata=amazon_reviews_25.select(["rev_id", "txt"]),
                                          accumulate='rev_id',
                                          model_output_tensor='sentence_embedding',
                                          show_model_properties=True
                                          )
    # Print the results.
    print(ONNXEmbeddings_out_2.result)
ONNXPredict
ONNXPredict
Functions
ONNXPredict(newdata=None, modeldata=None, accumulate=None, model_output_ﬁelds=None, overwrite_cached_models='false', show_model_input_ﬁelds_map=F
DESCRIPTION:
    The ONNXPredict() function is used to score data in Vantage with a model that has
    been created outside Vantage and exported to Vantage using ONNX format.
    For classical machine learning models on structured data, Vantage has a large set
    of transformation functions in both the Vantage Analytics Library and Analytics Database
    Analytic functions. User can use these functions to prepare the input data that the
    classical machine learning models expect. However, there are no transformation or
    conversion functions in Vantage to prepare tensors for unstructured data (text,
    images, video and audio) for ONNX models. The data must be preprocessed before
    loading to Vantage to conform the tensors into a shape that the ONNX models expect.
    As long as the data is in the form expected by your ONNX model, it can be scored by
    ONNXPredict().
    ONNXPredict() supports models in ONNX format. Several training frameworks support
    native export functionality to ONNX, such as Chainer, Caffee2, and PyTorch.
    User can also convert models from several toolkits like scikit-learn, TensorFlow,
    Keras, XGBoost, H2O, and Spark ML to ONNX.
PARAMETERS:
    newdata:
        Required Argument.
        Specifies the teradataml DataFrame containing the input test data.
        Types: teradataml DataFrame
    modeldata:
        Required Argument.
        Specifies the teradataml DataFrame containing the model data
        to be used for scoring.
        Note:
            * Use `retrieve_byom()` to get the teradataml DataFrame that contains the model.
        Types: teradataml DataFrame
    accumulate:
        Required Argument.
        Specifies the name(s) of input teradataml DataFrame column(s) to
        copy to the output.
        Types: str OR list of Strings (str) OR Feature OR list of Features
    model_output_fields:
        Optional Argument.
        Specifies the column(s) of the json output that the user wants to
        specify as individual columns instead of the entire json_report.
        Types: str OR list of Strings (str)
    overwrite_cached_models:
        Optional Argument.
        Specifies the model name that needs to be removed from the cache.
        When a model loaded into the memory of the node fits in the cache,
        it stays in the cache until being evicted to make space for another
        model that needs to be loaded. Therefore, a model can remain in the
        cache even after the completion of function execution. Other functions
        that use the same model can use it, saving the cost of reloading it
        into memory. User should overwrite a cached model only when it is updated,
        to make sure that the Predict function uses the updated model instead
        of the cached model.
        Note:
            Do not use the "overwrite_cached_models" argument except when user
            is trying to replace a previously cached model. Using the argument
            in other cases, including in concurrent queries or multiple times
            within a short period of time lead to an OOM error.
        Default Value: "false"
        Permitted Values: true, t, yes, y, 1, false, f, no, n, 0, *,
                          current_cached_model
        Types: str
    show_model_input_fields_map:
        Optional Argument.
        Specifies When set to 'True', the function does not predict the "newdata",
        instead shows the currently defined fields fully expanded.
        Example:
            If "model_input_fields_map" is x=[1:4] and "show_model_input_fields_map"
            is set to 'True', the output is returned as a varchar column:
                model_input_fields_map='x=sepal_len,sepal_wid,petal_len,petal_wid'
        When "model_input_fields_map" is not specified, the expected default
        mapping is based on the ONNX inputs defined in the model is shown.
        When set to 'False', which is the default value the function predict the
        "newdata".
        Default Value: False
        Types: bool
    model_input_fields_map:
        Optional Argument.
        Specifies the output fields to add as individual columns instead of the
        entire JSON output.
        Types: str OR list of Strings (str)
    is_debug:
        Optional Argument.
        Specifies whether debug statements are added to a trace table or not.
        When set to True, debug statements are added to a trace table that must
        be created beforehand.
        Notes:
            * Only available with BYOM version 3.00.00.02 and later.
            * To save logs for debugging, user can create an error log by using
              the is_debug=True parameter in the predict functions.
              A database trace table is used to collect this information which
              does impact performance of the function, so using small data input
              sizes is recommended.
            * To generate this log, user must do the following:
                  1. Create a global trace table with columns vproc_ID BYTE(2),
                     Sequence INTEGER, Trace_Output VARCHAR(31000)
                  2. Turn on session function tracing:
                       SET SESSION FUNCTION TRACE USING '' FOR TABLE <trace_table_name_created_in_step_1>;
                  3. Execute function with "is_debug" set to True.
                  4. Debug information is logged to the table created in step 1.
                  5. To turn off the logging, either disconnect from the session or
                     run following SQL:
                       SET SESSION FUNCTION TRACE OFF;
                  The trace table is temporary and the information is deleted if user
                  logs off from the session. If long term persistence is necessary,
                  user can copy the table to a permanent table before leaving the
                  session.
        Default Value: False
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept. Below
        are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the
                function in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Default Value: False
                Types: bool
            volatile:
                Optional Argument.
                Specifies whether to put the results of the
                function in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
        Function allows the user to partition, hash, order or local
        order the input data. These generic arguments are available
        for each argument that accepts teradataml DataFrame as
        input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or
              list of str (Strings) or PartitionKind
            * "<input_data_arg_name>_hash_column" accepts str or list
              of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list
              of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if
            the underlying SQL Engine function supports, else an
            exception is raised.
RETURNS:
    Instance of ONNXPredict.
    Output teradataml DataFrames can be accessed using attribute
    references, such as ONNXPredictObj.<attribute_name>.
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
    #     4. To execute BYOM functions, set 'configure.byom_install_location' to the
    #        database name where BYOM functions are installed.
    # Import required libraries / functions.
    import os, teradataml
    from teradataml import DataFrame, load_example_data, configure, execute_sql
    from teradataml import save_byom, retrieve_byom, display_analytic_functions
    # Load the example data.
    load_example_data("byom", ["iris_test"])
    # Create teradataml DataFrame objects.
    iris_test = DataFrame("iris_test")
    # Set install location of BYOM functions.
    configure.byom_install_location = "mldb"
    # Check the list of available analytic functions.
    display_analytic_functions(type="BYOM")
    # Load model file into Vantage.
    model_file_path = os.path.join(os.path.dirname(teradataml.__file__), "data", "models")
    skl_model_file = os.path.join(model_file_path,
                                  "iris_db_dt_model_sklearn.onnx")
    skl_floattensor_model_file = os.path.join(model_file_path,
                                              "iris_db_dt_model_sklearn_floattensor.onnx")
    # Save ONNX models.
    save_byom("iris_db_dt_model_sklearn",
              skl_model_file, "byom_models")
    save_byom("iris_db_dt_model_sklearn_floattensor",
              skl_floattensor_model_file, "byom_models")
    # Retrieve ONNX model.
    # The 'iris_db_dt_model_sklearn' created with each input variable mapped
    # to a single input tensor, then converted this model into ONNX format
    # with scikit-learn-onnx, and then used to predict the flower species.
    # This model trained using iris_test dataset with scikit-learn.
    skl_model = retrieve_byom("iris_db_dt_model_sklearn",
                              table_name="byom_models")
    # The 'iris_db_dt_model_sklearn_floattensor' created by using an input array of
    # four float32 values and named float_input, then converted this model into ONNX
    # format with scikit-learn-onnx, and then used to predict the flower species.
    # This model trained using iris_test dataset with scikit-learn.
    skl_floattensor_model = retrieve_byom("iris_db_dt_model_sklearn_floattensor",
                                table_name="byom_models")
    # Example 1: Example performs prediction with ONNXPredict function using trained
    #            'skl_model' model in onnx format generated outside of Vantage.
    ONNXPredict_out = ONNXPredict(accumulate="id",
                                  newdata=iris_test,
                                  modeldata=skl_model)
    # Print the results.
    print(ONNXPredict_out.result)
    # Example 2: Example performs prediction with ONNXPredict function using trained
    #            'skl_floattensor_model' model in onnx format generated
    #            outside of Vantage, where input DataFrame columns match the order
    #            used when generating the model, by specifying "model_input_fields_map"
    #            to define the columns.
    ONNXPredict_out1 = ONNXPredict(accumulate="id",
                                   model_output_fields="output_probability",
                                   overwrite_cached_models="*",
                                   model_input_fields_map='float_input=sepal_length, sepal_width, petal_length, petal_
                                   newdata=iris_test,
                                   modeldata=skl_floattensor_model)
    # Print the result DataFrame.
    print(ONNXPredict_out1.result)
    # Example 3: Example to show case the trace table usage using
    #            is_debug=True.
    # Create the trace table.
    crt_tbl_query = 'CREATE GLOBAL TEMPORARY TRACE TABLE BYOM_Trace                         (vproc_ID       BYTE(2)   
    execute_sql(crt_tbl_query)
    # Turn on tracing for the session.
    execute_sql("SET SESSION FUNCTION TRACE USING '' FOR TABLE BYOM_Trace;")
    # Execute the ONNXPredict() function using is_debug=True.
    ONNXPredict_out2 = ONNXPredict(accumulate="id",
                                  newdata=iris_test,
                                  modeldata=skl_model,
                                  is_debug=True)
    # Print the results.
    print(ONNXPredict_out2.result)
    # View the trace table information.
    trace_df = DataFrame.from_table("BYOM_Trace")
    print(trace_df)
    # Turn off tracing for the session.
    execute_sql("SET SESSION FUNCTION TRACE OFF;")
PMMLPredict
PMMLPredict
Functions
PMMLPredict(newdata=None, modeldata=None, accumulate=None, model_output_ﬁelds=None, overwrite_cached_models=None, is_debug=False, **generic_argum
DESCRIPTION:
    The function transforms the input data during model training as part
    of a pipeline. The generated model, stored in XML format, includes
    the preprocessing steps. During model prediction, the transformations are
    applied to the input data and the transformed data is scored by the PMMLPredict().
    PMML supports the following input data transformations:
        * Normalization: Scales continuous or discrete input values to specified range.
                         Python Function: MinMaxScaler
        * Discretization: Maps continuous input values to discrete values.
                          Python Function: CutTransformer
        * Value Mapping: Maps discrete input values to other discrete values.
                         Python Functions: StandardScalar, LabelEncoder
        * Function Mapping: Maps input values to values derived from applying a function.
                            Python Function: FunctionTransformer
    PMMLPredict() function supports the following external models:
        * Anomaly Detection
        * Association Rules
        * Cluster
        * General Regression
        * k-Nearest Neighbors
        * Naive Bayes
        * Neural Network
        * Regression
        * Ruleset
        * Scorecard
        * Random Forest
        * Decision Tree
        * Vector Machine
        * Multiple Models
PARAMETERS:
    newdata:
        Required Argument.
        Specifies the input teradataml DataFrame that contains the data to be scored.
        Types: teradataml DataFrame
    modeldata:
        Required Argument.
        Specifies the model teradataml DataFrame to be used for scoring.
        Note:
            * Use `retrieve_byom()` to get the teradataml DataFrame that contains the model.
        Types: teradataml DataFrame
    accumulate:
        Required Argument.
        Specifies the name(s) of input teradataml DataFrame column(s)
        to copy to the output DataFrame.
        Types: str OR list of Strings (str) OR Feature OR list of Features
    model_output_fields:
        Optional Argument.
        Specifies the scoring fields to output as individual columns
        instead of outputting the JSON string that contains all the
        scoring output fields.
        User should not specify "model_output_fields" that is not in the
        JSON string, otherwise system will throw an error.
        Default behavior: The function outputs the JSON string that contains
                          all the output fields in the output data column
                          json_report.
        Types: str OR list of Strings (str)
    overwrite_cached_models:
        Optional Argument.
        Specifies the model name that needs to be removed from the cache.
        When a model loaded into the memory of the node fits in the cache,
        it stays in the cache until being evicted to make space for another
        model that needs to be loaded. Therefore, a model can remain in the
        cache even after the completion of function execution. Other functions
        that use the same model can use it, saving the cost of reloading it
        into memory. User should overwrite a cached model only when it is updated,
        to make sure that the Predict function uses the updated model instead
        of the cached model.
        Note:
            Do not use the "overwrite_cached_models" argument except when user
            is trying to replace a previously cached model. Using the argument
            in other cases, including in concurrent queries or multiple times
            within a short period of time lead to an OOM error.
        Default behavior: The function does not overwrite cached models.
        Permitted Values: true, t, yes, y, 1, false, f, no, n, 0, *,
                          current_cached_model
        Types: str OR list of Strings (str)
    is_debug:
        Optional Argument.
        Specifies whether debug statements are added to a trace table or not.
        When set to True, debug statements are added to a trace table that must
        be created beforehand.
        Notes:
            * Only available with BYOM version 3.00.00.02 and later.
            * To save logs for debugging, user can create an error log by using
              the is_debug=True parameter in the predict functions.
              A database trace table is used to collect this information which
              does impact performance of the function, so using small data input
              sizes is recommended.
            * To generate this log, user must do the following:
                  1. Create a global trace table with columns vproc_ID BYTE(2),
                     Sequence INTEGER, Trace_Output VARCHAR(31000)
                  2. Turn on session function tracing:
                       SET SESSION FUNCTION TRACE USING '' FOR TABLE <trace_table_name_created_in_step_1>;
                  3. Execute function with "is_debug" set to True.
                  4. Debug information is logged to the table created in step 1.
                  5. To turn off the logging, either disconnect from the session or
                     run following SQL:
                       SET SESSION FUNCTION TRACE OFF;
                  The trace table is temporary and the information is deleted if user
                  logs off from the session. If long term persistence is necessary,
                  user can copy the table to a permanent table before leaving the
                  session.
        Default Value: False
        Types: bool
    **generic_arguments:
        Specifies the generic keyword arguments SQLE functions accept. Below
        are the generic keyword arguments:
            persist:
                Optional Argument.
                Specifies whether to persist the results of the
                function in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Default Value: False
                Types: bool
            volatile:
                Optional Argument.
                Specifies whether to put the results of the
                function in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
        Function allows the user to partition, hash, order or local
        order the input data. These generic arguments are available
        for each argument that accepts teradataml DataFrame as
        input and can be accessed as:
            * "<input_data_arg_name>_partition_column" accepts str or
              list of str (Strings) or PartitionKind
            * "<input_data_arg_name>_hash_column" accepts str or list
              of str (Strings)
            * "<input_data_arg_name>_order_column" accepts str or list
              of str (Strings)
            * "local_order_<input_data_arg_name>" accepts boolean
        Note:
            These generic arguments are supported by teradataml if
            the underlying SQL Engine function supports, else an
            exception is raised.
RETURNS:
    Instance of PMMLPredict.
    Output teradataml DataFrames can be accessed using attribute
    references, such as PMMLPredictObj.<attribute_name>.
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
    #     4. To execute BYOM functions, set 'configure.byom_install_location' to the
    #        database name where BYOM functions are installed.
    # Import required libraries / functions.
    import os, teradataml
    from teradataml import DataFrame, load_example_data, create_context, execute_sql
    from teradataml import save_byom, retrieve_byom, configure, display_analytic_functions
    # Load example data.
    load_example_data("byom", "iris_test")
    # Create teradataml DataFrame objects.
    iris_test = DataFrame.from_table("iris_test")
    # Set install location of BYOM functions.
    configure.byom_install_location = "mldb"
    # Check the list of available analytic functions.
    display_analytic_functions(type="BYOM")
    # Example 1: This example scores the data on Vantage using a GLM model generated
    #            outside of Vantage. The example performs prediction with PMMLPredict
    #            function using this GLM model in PMML format generated by open source
    #            model. Corresponding values are specified for the "overwrite_cached_models".
    #            This will erase entire cache.
    # Load model file into Vantage.
    model_file = os.path.join(os.path.dirname(teradataml.__file__), "data", "models", "iris_db_glm_model.pmml")
    save_byom("iris_db_glm_model", model_file, "byom_models")
    # Retrieve model.
    modeldata = retrieve_byom("iris_db_glm_model", table_name="byom_models")
    result = PMMLPredict(
            modeldata = modeldata,
            newdata = iris_test,
            accumulate = ['id', 'sepal_length', 'petal_length'],
            overwrite_cached_models = '*',
            )
    # Print the results.
    print(result.result)
    # Example 2: This example scores the data on Vantage using a XGBoost model generated
    #            outside of Vantage. The example performs prediction with PMMLPredict
    #            function using this XGBoost model in PMML format generated by open source
    #            model. Corresponding values are specified for the "overwrite_cached_models".
    #            This will erase entire cache.
    # Load model file into Vantage.
    model_file = os.path.join(os.path.dirname(teradataml.__file__), "data", "models", "iris_db_xgb_model.pmml")
    save_byom("iris_db_xgb_model", model_file, "byom_models")
    # Retrieve model.
    modeldata = retrieve_byom("iris_db_xgb_model", table_name="byom_models")
    result = PMMLPredict(
            modeldata = modeldata,
            newdata = iris_test,
            accumulate = ['id', 'sepal_length', 'petal_length'],
            overwrite_cached_models = '*',
            )
    # Print the results.
    print(result.result)
    # Example 3: Example to show case the trace table usage using
    #            is_debug=True.
    # Create the trace table.
    crt_tbl_query = 'CREATE GLOBAL TEMPORARY TRACE TABLE BYOM_Trace                         (vproc_ID       BYTE(2)   
    execute_sql(crt_tbl_query)
    # Turn on tracing for the session.
    execute_sql("SET SESSION FUNCTION TRACE USING '' FOR TABLE BYOM_Trace;")
    modeldata = retrieve_byom("iris_db_glm_model", table_name="byom_models")
    # Execute the PMMLPredict() function using is_debug=True.
    result = PMMLPredict(
            modeldata = modeldata,
            newdata = iris_test,
            accumulate = ['id', 'sepal_length', 'petal_length'],
            overwrite_cached_models = '*',
            is_debug=True
            )
    # Print the results.
    print(result.result)
    # View the trace table information.
    trace_df = DataFrame.from_table("BYOM_Trace")
    print(trace_df)
    # Turn off tracing for the session.
    execute_sql("SET SESSION FUNCTION TRACE OFF;")
Model Cataloging
delete_byom
teradataml.catalog.byom.delete_byom = delete_byom(model_id, table_name=None, schema_name=None)
DESCRIPTION:
    Delete a model from the user specified table in Teradata Vantage.
PARAMETERS:
    model_id:
        Required Argument.
        Specifies the the unique model identifier of the model to be deleted.
        Types: str
    table_name:
        Optional Argument.
        Specifies the name of the table to delete the model from.
        Notes:
            * One must either specify this argument or set the byom model catalog table
                name using set_byom_catalog().
            * If none of these arguments are set, exception is raised; If both arguments
                are set, the settings in delete_byom() take precedence and is used for
                function execution when saving an model.
        Types: str
    schema_name:
        Optional Argument.
        Specifies the name of the schema/database in which the table specified in
        "table_name" is looked up.
        Notes:
            * One must either specify this argument and table_name argument
                or set the byom model catalog schema and table name using set_byom_catalog().
            * If none of these arguments are set, exception is raised; If both arguments
                are set, the settings in delete_byom() take precedence and is used for
                function execution when saving an model.
            * If user specifies schema_name argument table_name argument has to be specified,
                else exception is raised.
        Types: str
RETURNS:
    None.
RAISES:
    TeradataMlException
EXAMPLES:
    >>> import teradataml, os, datetime
    >>> model_file = os.path.join(os.path.dirname(teradataml.__file__), 'data', 'models', 'iris_kmeans_model')
    >>> from teradataml import save_byom, delete_byom
    >>> save_byom('model3', model_file, 'byom_models')
    Model is saved.
    >>> save_byom('model4', model_file, 'byom_models', schema_name='test')
    Model is saved.
    >>> save_byom('model5', model_file, 'byom_models', schema_name='test')
    Model is saved.
    >>> save_byom('model4', model_file, 'byom_models')
    Model is saved.
    >>> save_byom('model5', model_file, 'byom_models')
    Model is saved.
    # Example 1 - Delete a model with id 'model3' from the table byom_models.
    >>> delete_byom(model_id='model3', table_name='byom_models')
    Model is deleted.
    >>>
    # Example 2 - Delete a model with id 'model4' from the table byom_models
    #             and the table is in "test" DataBase.
    >>> delete_byom(model_id='model4', table_name='byom_models', schema_name='test')
    Model is deleted.
    >>>
    # Example 3 - Delete a model with id 'model4' from the model cataloging table 'byom_models'
    #             set by set_byom_catalog().
    >>> set_byom_catalog(table_name='byom_models', schema_name='alice')
    The model cataloging parameters are set to table_name='byom_models' and schema_name='alice'
    >>> delete_byom(model_id='model4')
    Model is deleted.
    # Example 4 - Set the cataloging table_name to 'byom_models'
    #             and delete the model in table other than model catalog table 'byom_licensed_models'.
    >>> set_byom_catalog(table_name='byom_models', schema_name= alice')
    The model cataloging parameters are set to table_name='byom_models' and schema_name='alice'
    >>> save_byom('licensed_model2', model_file=model_file, table_name='byom_licensed_models')
    Created the model table 'byom_licensed_models' as it does not exist.
    Model is saved.
    >>> delete_byom(model_id='licensed_model2', table_name='byom_licensed_models')
    Model is deleted.
get_license
teradataml.catalog.byom.get_license = get_license()
DESCRIPTION:
    Get the license information set by set_license() function at the session level.
PARAMETERS:
    None.
RETURNS:
    None.
RAISES:
    None.
EXAMPLES:
    >>> import os, teradataml
    >>> from teradataml import save_byom, get_license, set_license
    # Example 1: When license is passed as a string.
    >>> set_license(license='eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI',
                    source='string')
    The license parameters are set.
    The license is: eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
    >>> get_license()
    The license is: eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
    # Example 2: When license is present in the column='license_data' and table='byom_licensed_models'.
    >>> set_license(license='license_data', table_name='byom_licensed_models', schema_name='alice',
                    source='column')
    The license parameters are set.
    The license is present in the table='byom_licensed_models', schema='alice' and
    column='license_data'.
    >>> get_license()
    The license is stored in:
    table = 'byom_licensed_models'
    schema = 'alice'
    column = 'license_data'
    >>>
list_byom
teradataml.catalog.byom.list_byom = list_byom(table_name=None, schema_name=None, model_id=None)
DESCRIPTION:
    Function to list models.
PARAMETERS:
    table_name:
        Optional Argument.
        Specifies the name of the table to list models from.
        Notes:
            * One must either specify this argument or set the byom model catalog table
                name using set_byom_catalog().
            * If none of these arguments are set, exception is raised; If both arguments
                are set, the settings in list_byom() take precedence and is used for
                function execution when saving an model.
        Types: str
    schema_name:
        Optional Argument.
        Specifies the name of the schema/database in which the table specified in
        "table_name" is looked up.
        Notes:
            * One must either specify this argument and table_name argument
                or set the byom model catalog schema and table name using set_byom_catalog().
            * If none of these arguments are set, exception is raised; If both arguments
                are set, the settings in list_byom() take precedence and is used for
                function execution when saving an model.
            * If user specifies schema_name argument table_name argument has to be specified,
                else exception is raised.
        Types: str
    model_id:
        Optional Argument.
        Specifies the unique model identifier of the model(s). If specified,
        the models with either exact match or a substring match, are listed.
        Types: str OR list
RETURNS:
    None.
RAISES:
    TeradataMlException, TypeError
EXAMPLES:
    >>> import teradataml, os, datetime
    >>> model_file = os.path.join(os.path.dirname(teradataml.__file__), 'data', 'models', 'iris_kmeans_model')
    >>> from teradataml import save_byom, list_byom
    >>> save_byom('model7', model_file, 'byom_models')
    Model is saved.
    >>> save_byom('iris_model1', model_file, 'byom_models')
    Model is saved.
    >>> save_byom('model8', model_file, 'byom_models', schema_name='test')
    Model is saved.
    >>> save_byom('iris_model1', model_file, 'byom_licensed_models')
    Model is saved.
    >>>
    # Example 1 - List all the models from the table byom_models.
    >>> list_byom(table_name='byom_models')
                                    model
    model_id
    model7       b'504B03041400080808...'
    iris_model1  b'504B03041400080808...'
    >>>
    # Example 2 - List all the models with model_id containing 'iris' string.
    #             List such models from 'byom_models' table.
    >>> list_byom(table_name='byom_models', model_id='iris')
                                    model
    model_id
    iris_model1  b'504B03041400080808...'
    >>>
    # Example 3 - List all the models with model_id containing either 'iris'
    #             or '7'. List such models from 'byom_models' table.
    >>> list_byom(table_name='byom_models', model_id=['iris', '7'])
                                    model
    model_id
    model7       b'504B03041400080808...'
    iris_model1  b'504B03041400080808...'
    >>>
    # Example 4 - List all the models from the 'byom_models' table and table is
    #             in 'test' DataBase.
    >>> list_byom(table_name='byom_models', schema_name='test')
                                    model
    model_id
    model8       b'504B03041400080808...'
    >>>
    # Example 5 - List all the models from the model cataloging table
    #             set by set_byom_catalog().
    >>> set_byom_catalog(table_name='byom_models', schema_name='alice')
    The model cataloging parameters are set to table_name='byom_models' and schema_name='alice'
    >>> list_byom()
                                    model
    model_id
    model8       b'504B03041400080808...'
    # Example 6 - List all the models from the table other than model cataloging table
    #             set at the session level.
    >>> set_byom_catalog(table_name='byom_models', schema_name= alice')
    The model cataloging parameters are set to table_name='byom_models' and schema_name='alice'
    >>> list_byom(table_name='byom_licensed_models')
                                       model
    model_id
    iris_model1         b'504B03041400080808...'
retrieve_byom
teradataml.catalog.byom.retrieve_byom = retrieve_byom(model_id, table_name=None, schema_name=None, license=None, is_license_column=False,
license_table_name=None, license_schema_name=None, require_license=False, return_addition_columns=False)
DESCRIPTION:
    Function to retrieve a saved model. Output of this function can be
    directly passed as input to the PMMLPredict and H2OPredict functions.
    Some models generated, such as H2O-DAI has license associated with it.
    When such models are to be used for scoring, one must retrieve the model
    by passing relevant license information. Please refer to "license_key"
    for more details.
PARAMETERS:
    model_id:
        Required Argument.
        Specifies the unique model identifier of the model to be retrieved.
        Types: str
    table_name:
        Optional Argument.
        Specifies the name of the table to retrieve external model from.
        Notes:
            * One must either specify this argument or set the byom model catalog table
                name using set_byom_catalog().
            * If none of these arguments are set, exception is raised; If both arguments
                are set, the settings in retrieve_byom() take precedence and is used for
                function execution.
        Types: str
    schema_name:
        Optional Argument.
        Specifies the name of the schema/database in which the table specified in
        "table_name" is looked up.
        Notes:
            * One must either specify this argument and table_name argument
                or set the byom model catalog schema and table name using set_byom_catalog().
            * If none of these arguments are set, exception is raised; If both arguments
                are set, the settings in retrieve_byom() take precedence and is used for
                function execution.
            * If user specifies schema_name argument table_name argument has to be specified,
                else exception is raised.
        Types: str
    license:
        Optional Argument.
        Specifies the license key information in different ways specified as below:
        * If the license key is stored in a variable, user can pass it as string.
        * If the license key is stored in table, then pass a column name containing
          the license. Based on the table which has license information stored,
            * If the information is stored in the same model table as that of the
              model, one must set "is_license_column" to True.
            * If the information is stored in the different table from that of the
              "table_name", one can specify the table name and schema name using
              "license_table_name" and "license_schema_name" respectively.
        Types: str
    is_license_column:
        Optional Argument.
        Specifies whether license key specified in "license" is a license key
        or column name. When set to True, "license" contains the column name
        containing license data, otherwise contains the actual license key.
        Default Value: False
        Types: str
    license_table_name:
        Optional Argument.
        Specifies the name of the table which holds license key. One can specify this
        argument if license is stored in a table other than "table_name".
        Types: str
    license_schema_name:
        Optional Argument.
        Specifies the name of the Database associated with the "license_table_name".
        If not specified, current Database would be considered for "license_table_name".
        Types: str
    require_license:
        Optional Argument.
        Specifies whether the model to be retrieved is associated with a license.
        If True, license information set by the set_license() is retrieved.
        Note:
            If license parameters are passed, then this argument is ignored.
        Default value: False
        Types: bool
    return_addition_columns:
        Optional Argument.
        Specifies whether to return additional columns saved during save_byom() along with
        model_id and model columns.
        Default value: False
        Types: bool
    Note:
        The following table describes the system behaviour in different scenarios:
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
        |       In retrieve_byom()     |          In set_byom_catalog()      |     System Behavior                 |
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
        | table_name     | schema_name | table_name            | schema_name |                                     |
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
        | Set            | Set         | Set                   | Set         |  schema_name and table_name in      |
        |                |             |                       |             |  retrieve_byom() are used for       |
        |                |             |                       |             |  function execution.                |
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
        | Set            | Set         | Not set               | Not set     |  schema_name and table_name in      |
        |                |             |                       |             |  retrieve_byom() is used for        |
        |                |             |                       |             |  function execution.                |
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
        | Not set        | Not set     | Set                   | Set         |  table_name from retrieve_byom()    |
        |                |             |                       |             |  is used and schema name            |
        |                |             |                       |             |  associated with the current        |
        |                |             |                       |             |  context is used.                   |
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
        | Not set        | Set         | Set                   | Set         |  Exception is raised.               |
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
        | Not set        | Not set     | Set                   | Set         |  table_name and schema_name         |
        |                |             |                       |             |  from set_byom_catalog()            |
        |                |             |                       |             |  are used for function execution.   |
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
        | Not set        | Not set     | Set                   | Not set     |  table_name from set_byom_catalog() |
        |                |             |                       |             |  is used and schema name            |
        |                |             |                       |             |  associated with the current        |
        |                |             |                       |             |  context is used.                   |
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
        |  Not set       | Not set     | Not set               | Not set     |  Exception is raised                |
        +----------------+-------------+-----------------------+-------------+-------------------------------------+
RETURNS:
    teradataml DataFrame
RAISES:
    TeradataMlException, TypeError
EXAMPLES:
    >>> import teradataml, os, datetime
    >>> model_file = os.path.join(os.path.dirname(teradataml.__file__), 'data', 'models', 'iris_kmeans_model')
    >>> from teradataml import save_byom, retrieve_byom, get_context
    >>> save_byom('model5', model_file, 'byom_models')
    Model is saved.
    >>> save_byom('model6', model_file, 'byom_models', schema_name='test')
    Model is saved.
    >>> # Save the license in an addtional column named "license_data" in the model table.
    >>> save_byom('licensed_model1', model_file, 'byom_licensed_models', additional_columns=
{"license_data": "A5sUL9KU_kP35Vq"})
    Created the model table 'byom_licensed_models' as it does not exist.
    Model is saved.
    >>> # Store the license in a table.
    >>> license = 'eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI'
    >>> lic_table = 'create table license (id integer between 1 and 1,license_key varchar(2500)) unique primary index(id);'
    >>> execute_sql(lic_table)
    <sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000014AAFF27080>
    >>> execute_sql("insert into license values (1, 'peBVRtjA-ib')")
    <sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000014AAFF27278>
    >>>
    # Example 1 - Retrieve a model with id 'model5' from the table 'byom_models'.
    >>> df = retrieve_byom('model5', table_name='byom_models')
    >>> df
                                 model
    model_id
    model5    b'504B03041400080808...'
    # Example 2 - Retrieve a model with id 'model6' from the table 'byom_models'
    #             and the table is in 'test' DataBase.
    >>> df = retrieve_byom('model6', table_name='byom_models', schema_name='test')
    >>> df
                                 model
    model_id
    model6    b'504B03041400080808...'
    # Example 3 - Retrieve a model with id 'model5' from the table 'byom_models'
    #             with license key stored in a variable 'license'.
    >>> df = retrieve_byom('model5', table_name='byom_models', license=license)
    >>> df
                                 model                                                         license
    model_id
    model5    b'504B03041400080808...'  eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
    >>>
    # Example 4 - Retrieve a model with id 'licensed_model1' and associated license
    #             key stored in table 'byom_licensed_models'. License key is stored
    #             in column 'license_data'.
    >>> df = retrieve_byom('licensed_model1',
    ...                    table_name='byom_licensed_models',
    ...                    license='license_data',
    ...                    is_license_column=True)
    >>> df
                                        model          license
    model_id
    licensed_model1  b'504B03041400080808...'  A5sUL9KU_kP35Vq
    >>>
    # Example 5 - Retrieve a model with id 'licensed_model1' from the table
    #             'byom_licensed_models' and associated license key stored in
    #             column 'license_key' of the table 'license'.
    >>> df = retrieve_byom('licensed_model1',
    ...                    table_name='byom_licensed_models',
    ...                    license='license_key',
    ...                    is_license_column=True,
    ...                    license_table_name='license')
    >>> df
                                        model      license
    model_id
    licensed_model1  b'504B03041400080808...'  peBVRtjA-ib
    >>>
    # Example 6 - Retrieve a model with id 'licensed_model1' from the table
    #             'byom_licensed_models' and associated license key stored in
    #             column 'license_key' of the table 'license' present in the
    #             schema 'mldb'.
    >>> df = retrieve_byom('licensed_model1',
    ...                    table_name='byom_licensed_models',
    ...                    license='license_key',
    ...                    is_license_column=True,
    ...                    license_table_name='license',
    ...                    license_schema_name='mldb')
    >>> df
                                        model      license
    model_id
    licensed_model1  b'504B03041400080808...'  peBVRtjA-ib
    >>>
    # Example 7 - Retrieve a model with id 'model5' from the table 'byom_models'
    #             with license key stored by set_license in a variable 'license'.
    #             The catalog information is set using set_byom_catalog()
    #             to table_name='byom_models', schema_name='alice'
    #             schema_name='alice' and is used to retrieve the model.
    >>> set_byom_catalog(table_name='byom_models', schema_name='alice')
    The model cataloging parameters are set to table_name='byom_models' and
    schema_name='alice'
    >>> set_license(license=license, source='string')
    The license parameters are set.
    The license is: eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
    >>> df = retrieve_byom('model5', require_license=True)
    >>> df
                                 model                                                         license
    model_id
    model5    b'504B03041400080808...'  eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
    >>>
    # Example 8 - Retrieve a model with id 'model5' from the table 'byom_models'
    #             with license key stored by set_license in a file. Since the
    #             schema name is not provided, default schema is used.
    >>> license_file = os.path.join(os.path.dirname(teradataml.__file__),
    ...                             'data', 'models', 'License_file.txt')
    >>> set_license(license=license_file, source='file')
    The license parameters are set.
    The license is: license_string
    >>> df = retrieve_byom('model5', table_name='byom_models', require_license=True)
    >>> df
                                 model         license
    model_id
    model5    b'504B03041400080808...'  license_string
    # Example 9 - Retrieve a model with id 'licensed_model1' and associated license
    #             key stored in column 'license_key' of the table 'license' present
    #             in the schema 'alice'. The byom catalog and license information is
    #             set using set_byom_catalog() and set_license() respectively.
    #             Function is executed with license parameters passed,
    #             which overrides the license information set at the session level.
    >>> set_byom_catalog(table_name='byom_licensed_models', schema_name='alice')
    The model cataloging parameters are set to table_name='byom_licensed_models'
    and schema_name='alice'
    >>> set_license(license=license, source='string')
    The license parameters are set.
    The license is: eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
    >>> df = retrieve_byom('licensed_model1', license='license_key',
    ...                    is_license_column=True, license_table_name='license')
    >>> df
                                                        model      license
                    model_id
                    licensed_model1  b'504B03041400080808...'  peBVRtjA-ib
    # Example 10 - Retrieve a model with id 'licensed_model1' from the table
    #              'byom_licensed_models' and associated license key stored in
    #              column 'license_data' of the table 'byom_licensed_models'.
    #              The byom catalog and license information is already set
    #              at the session level, passing the table_name to the
    #              function call overrides the byom catalog information
    #              at the session level.
    >>> set_byom_catalog(table_name='byom_models', schema_name='alice')
    The model cataloging parameters are set to table_name='byom_models' and
    schema_name='alice'
    >>> set_license(license='license_data', table_name='byom_licensed_models',
                    schema_name='alice', source='column')
    The license parameters are set.
    The license is present in the table='byom_licensed_models', schema='alice'
    and column='license_data'.
    >>> df = retrieve_byom('licensed_model1', table_name='byom_licensed_models',
    ...                    require_license=True)
    >>> df
                                        model          license
    model_id
    licensed_model1  b'504B03041400080808...'  A5sUL9KU_kP35Vq
    # Example 11 - If require license=False which is the default value for the above example,
    #              the license information is not retrieved.
    >>> df = retrieve_byom('licensed_model1', table_name='byom_licensed_models')
    >>> df
                                        model
    model_id
    licensed_model1  b'504B03041400080808...'
    # Example 12 - Retrieve a model with id 'licensed_model1' from the table along with all
    #              additional columns saved during save_byom().
    >>> df = retrieve_byom('licensed_model1', table_name='byom_licensed_models',
                           return_addition_columns=True)
    >>> df
                                        model     license_data
    model_id
    licensed_model1  b'504B03041400080808...'  A5sUL9KU_kP35Vq
save_byom
teradataml.catalog.byom.save_byom = save_byom(model_id, model_ﬁle, table_name=None, schema_name=None, additional_columns=None,
additional_columns_types=None)
DESCRIPTION:
    Function to save externally trained models in Teradata Vantage in the
    specified table. Function allows user to save various models stored in
    different formats such as PMML, MOJO etc. If the specified model table
    exists in Vantage, model data is saved in the same, otherwise model table
    is created first based on the user parameters and then model data is
    saved. See below 'Note' section for more details.
    Notes:
        If user specified table exists, then
            a. Table must have at least two columns with names and types as
               specified below:
               * 'model_id' of type VARCHAR of any length and
               * 'model' column of type BLOB.
            b. User can choose to have the additional columns as well to store
               additional information of the model. This information can be passed
               using "additional_columns" parameter. See "additional_columns"
               argument description for more details.
        If user specified table does not exist, then
            a. Function creates the table with the name specified in "table_name".
            b. Table is created in the schema specified in "schema_name". If
               "schema_name" is not specified, then current schema is considered
               for "schema_name".
            c. Table is created with columns:
                * 'model_id' with type specified in "additional_columns_types". If
                  not specified, table is created with 'model_id' column as VARCHAR(128).
                * 'model' with type specified in "additional_columns_types". If
                  not specified, table is created with 'model' column as BLOB.
                * Columns specified in "additional_columns" parameter. See "additional_columns"
                  argument description for more details.
                * Datatypes of these additional columns are either taken from
                  the values passed to "additional_columns_types" or inferred
                  from the values passed to the "additional_columns". See
                  "additional_columns_types" argument description for more details.
PARAMETERS:
    model_id:
        Required Argument.
        Specifies the unique model identifier for model.
        Types: str.
    model_file:
        Required Argument.
        Specifies the absolute path of the file which has model information.
        Types: str
    table_name:
        Optional Argument.
        Specifies the name of the table where model is saved. If "table_name"
        does not exist, this function creates table according to "additional_columns"
        and "additional_columns_types".
        Notes:
            * One must either specify this argument or set the byom model catalog table
                name using set_byom_catalog().
            * If none of these arguments are set, exception is raised; If both arguments
                are set, the settings in save_byom() take precedence and is used for
                function execution when saving an model.
        Types: str
    schema_name:
        Optional Argument.
        Specifies the name of the schema/database in which the table specified in
        "table_name" is looked up.
        Notes:
            * One must either specify this argument and table_name argument
                or set the byom model catalog schema and table name using set_byom_catalog().
            * If none of these arguments are set, exception is raised; If both arguments
                are set, the settings in save_byom() take precedence and is used for
                function execution when saving an model.
            * If user specifies schema_name argument table_name argument has to be specified,
                else exception is raised.
        Types: str
    additional_columns:
        Optional Argument.
        Specifies the additional information about the model to be saved in the
        model table. Additional information about the model is passed as key value
        pair, where key is the name of the column and value is data to be stored
        in that column for the model being saved.
        Notes:
             1. Following are the allowed types for the values passed in dictionary:
                * int
                * float
                * str
                * bool
                * datetime.datetime
                * datetime.date
                * datetime.time
             2. "additional_columns" does not accept keys model_id and model.
        Types: str
    additional_columns_types:
        Optional Argument.
        Specifies the column type of additional columns. These column types are used
        while creating the table using the columns specified in "additional_columns"
        argument. Additional column datatype information is passed as key value pair
        with key being the column name and value as teradatasqlalchemy.types.
        Notes:
             1. If, any of the column type for additional columns are not specified in
                "additional_columns_types", it then derives the column type according
                the below table:
                +---------------------------+-----------------------------------------+
                |     Python Type           |        teradatasqlalchemy Type          |
                +---------------------------+-----------------------------------------+
                | str                       | VARCHAR(1024)                           |
                +---------------------------+-----------------------------------------+
                | int                       | INTEGER                                 |
                +---------------------------+-----------------------------------------+
                | bool                      | BYTEINT                                 |
                +---------------------------+-----------------------------------------+
                | float                     | FLOAT                                   |
                +---------------------------+-----------------------------------------+
                | datetime                  | TIMESTAMP                               |
                +---------------------------+-----------------------------------------+
                | date                      | DATE                                    |
                +---------------------------+-----------------------------------------+
                | time                      | TIME                                    |
                +---------------------------+-----------------------------------------+
             2. Columns model_id, with column type as VARCHAR and model, with column type
                as BLOB are mandatory for table. So, for the columns model_id and model,
                acceptable values for "additional_columns_types" are VARCHAR and BLOB
                respectively.
             3. This argument is ignored if table exists.
        Types: dict
RETURNS:
    None.
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> import teradataml, os, datetime
    >>> model_file = os.path.join(os.path.dirname(teradataml.__file__), 'data', 'models', 'iris_kmeans_model')
    >>> from teradataml import save_byom
    # Example 1 - Create table "byom_model" with additional columns by specifying the type
    #             of the columns as below and save the model in it.
    #             +---------------------------+-----------------------------------------+
    #             |     Column name           |        Column Type                      |
    #             +---------------------------+-----------------------------------------+
    #             | model_id                  | VARCHAR(128)                            |
    #             +---------------------------+-----------------------------------------+
    #             | model                     | BLOB                                    |
    #             +---------------------------+-----------------------------------------+
    #             | Description               | VARCHAR(2000)                           |
    #             +---------------------------+-----------------------------------------+
    #             | UserId                    | NUMBER(5)                               |
    #             +---------------------------+-----------------------------------------+
    #             | ProductionReady           | BYTEINT                                 |
    #             +---------------------------+-----------------------------------------+
    #             | ModelEfficiency           | NUMBER(11,10)                           |
    #             +---------------------------+-----------------------------------------+
    #             | ModelSavedTime            | TIMESTAMP                               |
    #             +---------------------------+-----------------------------------------+
    #             | ModelGeneratedDate        | DATE                                    |
    #             +---------------------------+-----------------------------------------+
    #             | ModelGeneratedTime        | TIME                                    |
    #             +---------------------------+-----------------------------------------+
    #
    >>> save_byom('model1',
    ...           model_file,
    ...           'byom_models',
    ...           additional_columns={"Description": "KMeans model",
    ...                               "UserId": "12345",
    ...                               "ProductionReady": False,
    ...                               "ModelEfficiency": 0.67412,
    ...                               "ModelSavedTime": datetime.datetime.now(),
    ...                               "ModelGeneratedDate":datetime.date.today(),
    ...                               "ModelGeneratedTime": datetime.time(hour=0,minute=5,second=45,microsecond=110)
    ...                               },
    ...           additional_columns_types={"Description": VARCHAR(2000),
    ...                                    "UserId": NUMBER(5),
    ...                                    "ProductionReady": BYTEINT,
    ...                                    "ModelEfficiency": NUMBER(11,10),
    ...                                    "ModelSavedTime": TIMESTAMP,
    ...                                    "ModelGeneratedDate": DATE,
    ...                                    "ModelGeneratedTime": TIME}
    ...           )
    Created the table 'byom_models' as it does not exist.
    Model is saved.
    >>>
    # Example 2 - Create table "byom_model1" in "test" DataBase, with additional columns
    #             by not specifying the type of the columns and once table is created,
    #             save the model in it.
    >>> save_byom('model1',
    ...           model_file,
    ...           'byom_models1',
    ...           additional_columns={"Description": "KMeans model",
    ...                               "UserId": "12346",
    ...                               "ProductionReady": False,
    ...                               "ModelEfficiency": 0.67412,
    ...                               "ModelSavedTime": datetime.datetime.now(),
    ...                               "ModelGeneratedDate":datetime.date.today(),
    ...                               "ModelGeneratedTime": datetime.time(hour=0,minute=5,second=45,microsecond=110)
    ...                               },
    ...           schema_name='test'
    ...           )
    Created the table 'byom_models1' as it does not exist.
    Model is saved.
    >>>
    # Example 3 - Save the model in the existing table "byom_models".
    >>> save_byom('model2',
    ...           model_file,
    ...           'byom_models',
    ...           additional_columns={"Description": "KMeans model duplicated"}
    ...           )
    Model is saved.
    >>>
    # Example 3 - Set the cataloging parameters and save the model
    #             in the existing table "byom_models".
    >>> set_byom_catalog(table_name='byom_models', schema_name='alice')
    The model cataloging parameters are set to table_name='byom_models' and schema_name='alice'
    >>> save_byom('model3', model_file=model_file)
    Model is saved.
    # Example 4 - Set the cataloging table_name to 'byom_models'
    #             and save the model in table 'byom_licensed_models' other than model catalog table.
    >>> set_byom_catalog(table_name='byom_models', schema_name='alice')
    The model cataloging parameters are set to table_name='byom_models' and schema_name='alice'
    >>> save_byom('licensed_model2', model_file=model_file, table_name='byom_licensed_models',
    ...           additional_columns={"license_data": "A5sUL9KU_kP35Vq"})
    Created the model table 'byom_licensed_models' as it does not exist.
    Model is saved.
    >>>
set_byom_catalog
teradataml.catalog.byom.set_byom_catalog = set_byom_catalog(table_name, schema_name=None)
DESCRIPTION:
    Function to set the BYOM model catalog information to be used by
    BYOM model cataloging APIs such as:
        * delete_byom
        * list_byom
        * retrieve_byom
        * save_byom
        * set_license
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the name of the table to be used for BYOM model cataloging.
        This table will be used for saving, retrieving BYOM model information
        by BYOM model cataloging APIs.
        Types: str.
    schema_name:
        Optional Argument.
        Specifies the name of the schema/database in which the table specified in
        "table_name" is looked up. If not specified, then table is looked
        up in current schema/database.
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException
EXAMPLES:
    >>> from teradataml import set_byom_catalog
    # Example 1 - Set global parameters table_name = 'model_table_name' and schema_name = 'model_schema_name';
    >>> set_byom_catalog(table_name='model_table_name', schema_name='model_schema_name')
    The model cataloging parameters are set to table_name='model_table_name' and schema_name='model_schema_name'
set_license
teradataml.catalog.byom.set_license = set_license(license, table_name=None, schema_name=None, source='string')
DESCRIPTION:
    Function sets the license information associated with the externally
    generated model at the session level to be used by 'retrieve_byom()'.
PARAMETERS:
    license:
        Required Argument.
        Specifies the license key information that can be passed as:
            * a variable.
            * in a file.
            * name of the column containing license information in a table
              specified by "table_name" argument.
        Note:
            Argument "source" must be set accordingly.
        Types: str
    table_name:
        Required Argument.
        Specifies the table name containing the license information.
        Note:
            Argument "table_name" and "schema_name"
            both should be specified or both should be None.
        Types: str
    schema_name:
        Optional Argument.
        Specifies the name of the schema in which the table specified in
        "table_name" is looked up.
        Note:
            Argument "table_name" and "schema_name"
            both should be specified or both should be None.
        Types: str
    source:
        Required Argument.
        Specifies whether license key specified in "license" is a string, file
        or column name.
        Default value: string
        Permitted values: string, file, column
RETURNS:
    None.
RAISES:
    TeradataMlException
EXAMPLES:
    >>> import os
    >>> from teradataml import save_byom, retrieve_byom, get_context, set_license, set_byom_catalog
    # Example 1: When license is passed as a string.
    >>> set_license(license='eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI',
    ...             table_name=None, schema_name=None, source='string')
    The license parameters are set.
    The license is : eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
    # Example 2: When license is stored in a file and file is passed as input to "license".
    #            "source" must be set to "file".
    >>> license_file = os.path.join(os.path.dirname(teradataml.__file__),
    ...                             'data', 'models', 'License_file.txt')
    >>> set_license(license=license_file, source='file')
    The license parameters are set.
    The license is: license_string
    # Example 3: When license is present in the byom model catalog table itself.
    # Store a model with license information in the model table.
    >>> model_file = os.path.join(os.path.dirname(teradataml.__file__),
    ...                           'data', 'models', 'iris_kmeans_model')
    >>> save_byom('licensed_model1', model_file, 'byom_licensed_models',
    ...           additional_columns={"license_data": "A5sUL9KU_kP35Vq"})
    Created the model table 'byom_licensed_models' as it does not exist.
    Model is saved.
    >>> set_byom_catalog(table_name='byom_licensed_models', schema_name='alice')
    The model cataloging parameters are set to table_name='byom_licensed_models'
    and schema_name='alice'
    >>> set_license(license='license_data', source='column')
    The license parameters are set.
    The license is present in the table='byom_licensed_models',schema='alice' and
    column='license_data'.
    # Example 4: Set the license information using the license stored in a column
    #            'license_key' of a table 'license_table'.
    # Create a table and insert the license information in the table.
    >>> license = 'eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI'
    >>> lic_table = 'create table license (id integer between 1 and 1,
    license_key varchar(2500)) unique primary index(id);'
    >>> get_context().execute(lic_table)
    <sqlalchemy.engine.cursor.LegacyCursorResult object at 0x000001DC4F2EE9A0>
    >>> get_context().execute("insert into license values (1, 'peBVRtjA-ib')")
    <sqlalchemy.engine.cursor.LegacyCursorResult object at 0x000001DC4F2EEF10>
    >>> set_license(license='license_key', table_name='license', schema_name='alice',
    ...             source='column')
    The license parameters are set.
    The license is present in the table='license', schema='alice' and column='license_key'.
Workflows
H2OPredict
Deep Learning
H2OPredict() using Deep Learning model.¶
Setup¶
In [1]:
import getpass
import tempfile
from teradataml import create_context, DataFrame, save_byom, retrieve_byom, \
delete_byom, list_byom, remove_context, load_example_data, db_drop_table
from teradataml.options.configure import configure
from teradataml.analytics.byom.H2OPredict import H2OPredict
import h2o
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
load_example_data("byom", "iris_input")
WARNING: Skipped loading table iris_input since it already exists in the database.
In [4]:
iris_input = DataFrame("iris_input")
In [5]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows.
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [6]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
17
5.4
3.9
1.3
0.4
1
38
4.9
3.6
1.4
0.1
1
76
6.6
3.0
4.4
1.4
2
122 5.6
2.8
4.9
2.0
3
59
6.6
2.9
4.6
1.3
2
80
5.7
2.6
3.5
1.0
2
120 6.0
2.2
5.0
1.5
3
118 7.7
3.8
6.7
2.2
3
19
5.7
3.8
1.7
0.3
1
61
5.0
2.0
3.5
1.0
2
In [7]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[7]:
id
sepal_length sepal_width petal_length petal_width species
66
6.7
3.1
4.4
1.4
2
97
5.7
2.9
4.2
1.3
2
146 6.7
3.0
5.2
2.3
3
118 7.7
3.8
6.7
2.2
3
62
5.9
3.0
4.2
1.5
2
38
4.9
3.6
1.4
0.1
1
108 7.3
2.9
6.3
1.8
3
24
5.1
3.3
1.7
0.5
1
150 5.9
3.0
5.1
1.8
3
122 5.6
2.8
4.9
2.0
3
Prepare dataset for a creating Deep Learning Model.¶
In [8]:
h2o.init()
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_train_pd = iris_train.to_pandas()
h2o_df = h2o.H2OFrame(iris_train_pd)
h2o_df
Checking whether there is an H2O instance running at http://localhost:54321 ..... not found.
Attempting to start a local H2O server...
; Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
  Starting server from C:\Users\pg255042\Anaconda3\envs\teraml\lib\site-packages\h2o\backend\bin\h2o.jar
  Ice root: C:\Users\pg255042\AppData\Local\Temp\tmpncr7fdcl
  JVM stdout: C:\Users\pg255042\AppData\Local\Temp\tmpncr7fdcl\h2o_pg255042_started_from_python.out
  JVM stderr: C:\Users\pg255042\AppData\Local\Temp\tmpncr7fdcl\h2o_pg255042_started_from_python.err
  Server is running at http://127.0.0.1:54321
Connecting to H2O server at http://127.0.0.1:54321 ... successful.
Warning: Your H2O cluster version is too old (5 months and 1 day)!Please download and install the latest version from http://h2o.ai/d
H2O_cluster_uptime:
02 secs
H2O_cluster_timezone:
Asia/Kolkata
H2O_data_parsing_timezone: UTC
H2O_cluster_version:
3.34.0.1
H2O_cluster_version_age:
5 months and 1 day !!!
H2O_cluster_name:
H2O_from_python_pg255042_n0d5ha
H2O_cluster_total_nodes:
1
H2O_cluster_free_memory:
7.052 Gb
H2O_cluster_total_cores:
0
H2O_cluster_allowed_cores:
0
H2O_cluster_status:
locked, healthy
H2O_connection_url:
http://127.0.0.1:54321
H2O_connection_proxy:
{"http": null, "https": null}
H2O_internal_security:
False
H2O_API_Extensions:
Amazon S3, Algos, AutoML, Core V3, TargetEncoder,
Core V4
Python_version:
3.6.12 ﬁnal
Parse progress: |████████████████████████████████████████████████████████████████| (done) 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6.3
3.3
6
2.5
3
5.1
3.4
1.5
0.2
1
5.7
3.8
1.7
0.3
1
6.7
3
5
1.7
2
6.7
3.1
5.6
2.4
3
6.3
3.3
4.7
1.6
2
6.6
2.9
4.6
1.3
2
6.4
3.2
5.3
2.3
3
5.4
3.9
1.3
0.4
1
Out[8]:
Train Deep Learning Model.¶
In [9]:
#Import required libraries.
from h2o.estimators.deeplearning import H2ODeepLearningEstimator
In [10]:
# Add the code for training model.
h2o_df["species"] = h2o_df["species"].asfactor()
predictors = h2o_df.columns
response = "species"
In [11]:
dl_model=H2ODeepLearningEstimator()
In [12]:
dl_model.train(x=predictors, y=response, training_frame=h2o_df)
deeplearning Model Build progress: |█████████████████████████████████████████████| (done) 100%
Model Details
=============
H2ODeepLearningEstimator :  Deep Learning
Model Key:  DeepLearning_model_python_1644984159615_1
Status of Neuron Layers: predicting species, 3-class classification, multinomial distribution, CrossEntropy loss, 41,803 weights/bias
layer units
type dropout l1 l2 mean_rate
rate_rms momentum mean_weight weight_rms
mean_bias
bias_rms
0
1
4
Input
0
1
2
200
Rectiﬁer 0
0 0 0.00446281 0.00367723 0
-0.00802765
0.107465
0.491997
0.0113614
2
3
200
Rectiﬁer 0
0 0 0.0263239
0.0926596
0
-0.00342238
0.0713205
0.982641
0.158966
3
4
3
Softmax
0 0 0.0154025
0.0718914
0
-0.0056864
0.400361
-0.000312151 0.00322719
ModelMetricsMultinomial: deeplearning
** Reported on train data. **
MSE: 0.07083642778722152
RMSE: 0.26615113711427485
LogLoss: 0.2858728266432885
Mean Per-Class Error: 0.08333333333333333
AUC: NaN
AUCPR: NaN
Multinomial auc values: Table is not computed because it is disabled (model parameter 'auc_type' is set to AUTO or NONE) or due to do
Multinomial auc_pr values: Table is not computed because it is disabled (model parameter 'auc_type' is set to AUTO or NONE) or due to
Confusion Matrix: Row labels: Actual class; Column labels: Predicted class
1
2
3
Error
Rate
0 43.0 0.0
0.0
0.000000 0 / 43
1 0.0
37.0 0.0
0.000000 0 / 37
2 0.0
10.0 30.0 0.250000 10 / 40
3 43.0 47.0 30.0 0.083333 10 / 120
Top-3 Hit Ratios: 
k hit_ratio
0 1 0.916667
1 2 1.000000
2 3 1.000000
Scoring History: 
timestamp duration training_speed epochs iterations samples training_rmse training_logloss training_r2 training_classiﬁcation_error training_auc training_p
0
2022-02-
16
09:32:43
0.000
sec
None
0.0
0
0.0
NaN
NaN
NaN
NaN
NaN
NaN
1
2022-02-
16
09:32:44
1.183
sec
779 obs/sec
1.0
1
120.0
0.312775
0.394377
0.858434
0.141667
NaN
NaN
2
2022-02-
16
09:32:45
1.293
sec
4580 obs/sec
10.0
10
1200.0
0.266151
0.285873
0.897493
0.083333
NaN
NaN
Variable Importances: 
variable relative_importance scaled_importance percentage
0 petal_width
1.000000
1.000000
0.268083
1 sepal_length 0.931197
0.931197
0.249638
2 petal_length 0.924395
0.924395
0.247814
3 sepal_width
0.874601
0.874601
0.234465
Out[12]:
Save the model in MOJO format.¶
In [13]:
# Saving h2o model to a file.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = dl_model.save_mojo(path=f"{temp_dir.name}", force=True)
Save the model in Vantage.¶
In [16]:
# Saving the h2o model in vantage.
save_byom(model_id="h2o_dl_iris", model_file=model_file_path, table_name="byom_models")
Model is saved.
List the models from Vantage.¶
In [17]:
# List the models from "byom_models".
list_byom("byom_models")
                                model
model_id                             
h2o_dl_iris  b'504B03041400080808...'
Retrieve the model from Vantage.¶
In [18]:
model=retrieve_byom(model_id="h2o_dl_iris", table_name="byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [19]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [20]:
result = H2OPredict(newdata=iris_test,
                    newdata_partition_column='id',
                    newdata_order_column='id',
                    modeldata=model,
                    modeldata_order_column='model_id',
                    model_output_fields=['label', 'classProbabilities'],
                    accumulate=['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models='*',
                    enable_options='stageProbabilities',
                    model_type='OpenSource'
                   )
In [21]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".H2OPredict(
ON "MLDB"."ml__select__1644990039784978" AS InputTable
PARTITION BY "id"
ORDER BY "id" 
ON (select model_id,model from "MLDB"."ml__filter__1644985511527604") AS ModelTable
DIMENSION
ORDER BY "model_id"
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('label','classProbabilities')
OverwriteCachedModel('*')
EnableOptions('stageProbabilities')
) as sqlmr
In [22]:
# Print the result.
result.result
Out[22]:
id sepal_length petal_length prediction label
classprobabilities
65 5.6
3.6
2
2
{"1": 2.913569781015937E-6,"2": 0.9999970798430343,"3": 6.5871847449533734E-9}
24 5.1
1.7
1
1
{"1": 0.9997385917776745,"2": 2.614082223254326E-4,"3": 3.965376132206562E-21}
73 6.3
4.9
2
2
{"1": 3.7928238391692515E-12,"2": 0.9996728607633336,"3": 3.271392328735716E-4}
30 4.7
1.6
1
1
{"1": 0.9999850304779964,"2": 1.4969522003627505E-5,"3": 1.1243029210372163E-23}
47 5.1
1.6
1
1
{"1": 0.9999996053508485,"2": 3.946491514700323E-7,"3": 3.1879575130755833E-25}
12 4.8
1.6
1
1
{"1": 0.999996809388207,"2": 3.1906117930260754E-6,"3": 2.1626858803931788E-24}
35 4.9
1.5
1
1
{"1": 0.999831072526406,"2": 1.6892747359393979E-4,"3": 4.731732528448486E-23}
37 5.5
1.3
1
1
{"1": 0.9998930293707516,"2": 1.069706292484114E-4,"3": 6.175983138020294E-24}
36 5.0
1.2
1
1
{"1": 0.9999369345961132,"2": 6.306540388681125E-5,"3": 6.3237409338068505E-24}
1
5.1
1.4
1
1
{"1": 0.9999935881770767,"2": 6.411822923278436E-6,"3": 1.293839606039162E-24}
Cleanup.¶
In [23]:
# Delete the saved Model.
delete_byom("h2o_dl_iris", table_name="byom_models")
Model is deleted.
In [24]:
# Drop model table.
db_drop_table("byom_models")
Out[24]:
True
In [25]:
# Drop input data table.
db_drop_table("iris_input")
Out[25]:
True
In [26]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[26]:
True
In [ ]:
Distributed Random Forest
H2OPredict() using Distributed Random Forest model.¶
Setup¶
In [1]:
import tempfile
import getpass
from teradataml import create_context, DataFrame, save_byom, retrieve_byom, \
delete_byom, list_byom, remove_context, load_example_data, db_drop_table
from teradataml.options.configure import configure
from teradataml.analytics.byom.H2OPredict import H2OPredict
import h2o
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
WARNING: Skipped loading table iris_input since it already exists in the database.
In [4]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[4]:
id
sepal_length sepal_width petal_length petal_width species
57
6.3
3.3
4.7
1.6
2
59
6.6
2.9
4.6
1.3
2
36
5.0
3.2
1.2
0.2
1
78
6.7
3.0
5.0
1.7
2
93
5.8
2.6
4.0
1.2
2
101 6.3
3.3
6.0
2.5
3
141 6.7
3.1
5.6
2.4
3
17
5.4
3.9
1.3
0.4
1
116 6.4
3.2
5.3
2.3
3
19
5.7
3.8
1.7
0.3
1
In [5]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
95
5.6
2.7
4.2
1.3
2
59
6.6
2.9
4.6
1.3
2
30
4.7
3.2
1.6
0.2
1
76
6.6
3.0
4.4
1.4
2
18
5.1
3.5
1.4
0.3
1
89
5.6
3.0
4.1
1.3
2
87
6.7
3.1
4.7
1.5
2
121 6.9
3.2
5.7
2.3
3
148 6.5
3.0
5.2
2.0
3
122 5.6
2.8
4.9
2.0
3
Prepare dataset for creating an Distributed Random Forest model.¶
In [6]:
h2o.init()
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_train_pd = iris_train.to_pandas()
h2o_df = h2o.H2OFrame(iris_train_pd)
h2o_df
Checking whether there is an H2O instance running at http://localhost:54321 ..... not found.
Attempting to start a local H2O server...
  Java Version: java version "11.0.2" 2019-01-15 LTS; Java(TM) SE Runtime Environment 18.9 (build 11.0.2+9-LTS); Java HotSpot(TM) 64-
  Starting server from /Users/gp186005/anaconda3/lib/python3.7/site-packages/h2o/backend/bin/h2o.jar
  Ice root: /var/folders/92/yt4b9fh178x2xhc_tnhpvzsr0000gn/T/tmpurl5ttsm
  JVM stdout: /var/folders/92/yt4b9fh178x2xhc_tnhpvzsr0000gn/T/tmpurl5ttsm/h2o_gp186005_started_from_python.out
  JVM stderr: /var/folders/92/yt4b9fh178x2xhc_tnhpvzsr0000gn/T/tmpurl5ttsm/h2o_gp186005_started_from_python.err
  Server is running at http://127.0.0.1:54321
Connecting to H2O server at http://127.0.0.1:54321 ... successful.
H2O_cluster_uptime:
15 secs
H2O_cluster_timezone:
America/Los_Angeles
H2O_data_parsing_timezone: UTC
H2O_cluster_version:
3.32.1.6
H2O_cluster_version_age:
1 month and 21 days
H2O_cluster_name:
H2O_from_python_gp186005_ip5q0u
H2O_cluster_total_nodes:
1
H2O_cluster_free_memory:
4 Gb
H2O_cluster_total_cores:
12
H2O_cluster_allowed_cores:
12
H2O_cluster_status:
accepting new members, healthy
H2O_connection_url:
http://127.0.0.1:54321
H2O_connection_proxy:
{"http": null, "https": null}
H2O_internal_security:
False
H2O_API_Extensions:
Amazon S3, XGBoost, Algos, AutoML, Core V3,
TargetEncoder, Core V4
Python_version:
3.7.3 ﬁnal
Parse progress: |█████████████████████████████████████████████████████████| 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6.3
3.3
6
2.5
3
5.1
3.4
1.5
0.2
1
5.6
2.8
4.9
2
3
4.9
3.6
1.4
0.1
1
6.7
3.1
5.6
2.4
3
5.7
2.6
3.5
1
2
6.6
2.9
4.6
1.3
2
6.7
3
5
1.7
2
4.8
3
1.4
0.1
1
Out[6]:
Train Distributed Random Forest Model.¶
In [7]:
# Import required libraries.
from h2o.estimators import H2ORandomForestEstimator
In [8]:
# Add the code for training model. 
h2o_df["species"] = h2o_df["species"].asfactor()
predictors = h2o_df.columns
response = "species"
In [9]:
iris_rf = H2ORandomForestEstimator(ntrees=100, max_depth=0)
In [10]:
iris_rf.train(x=predictors, y=response, training_frame=h2o_df)
drf Model Build progress: |███████████████████████████████████████████████| 100%
Save the model in MOJO format.¶
In [11]:
# Saving H2O Model to a file.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = iris_rf.save_mojo(path=f"{temp_dir.name}", force=True)
Save the model in Vantage.¶
In [13]:
# Save the H2O Model in Vantage.
save_byom("h2o_rf_iris", 
          model_file_path,
          table_name="byom_models", 
          additional_columns={"description": "Random forest model generated using H2O"}
         )
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [14]:
# List the models from "byom_models".
list_byom("byom_models")
                                model                              description
model_id                                                                      
h2o_rf_iris  b'504B03041400080808...'  Random forest model generated using H2O
Retrieve the model from Vantage.¶
In [15]:
# Retrieve the model from vantage using the model id 'h2o_rf_iris'.
modeldata = retrieve_byom(model_id="h2o_rf_iris", table_name="byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [16]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [17]:
result = H2OPredict(newdata=iris_test,
                    newdata_partition_column='id',
                    newdata_order_column='id',
                    modeldata=modeldata,
                    modeldata_order_column='model_id',
                    model_output_fields=['label', 'classProbabilities'],
                    accumulate=['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models='*',
                    enable_options='stageProbabilities',
                    model_type='OpenSource'
                   )
In [18]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".H2OPredict(
ON "MLDB"."ml__select__16344897347247" AS InputTable
PARTITION BY "id"
ORDER BY "id" 
ON (select model_id,model from "MLDB"."ml__filter__16344924296580") AS ModelTable
DIMENSION
ORDER BY "model_id"
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('label','classProbabilities')
OverwriteCachedModel('*')
EnableOptions('stageProbabilities')
) as sqlmr
In [19]:
# Print the result.
result.result
Out[19]:
id sepal_length petal_length prediction label
classprobabilities
69 6.2
4.5
2
2
{"1": 7.458774870464025E-4,"2": 0.9582039807214917,"3": 0.04105014179146193}
37 5.5
1.3
1
1
{"1": 0.9896364785244597,"2": 0.010090797219753938,"3": 2.727242557863893E-4}
44 5.0
1.6
1
1
{"1": 0.999729802749471,"2": 0.0,"3": 2.7019725052900413E-4}
16 5.7
1.5
1
1
{"1": 0.9896364785244597,"2": 0.010090797219753938,"3": 2.727242557863893E-4}
81 5.5
3.8
2
2
{"1": 7.309748693940461E-4,"2": 0.9989990253874769,"3": 2.6999974312908804E-4}
18 5.1
1.4
1
1
{"1": 0.999729802749471,"2": 0.0,"3": 2.7019725052900413E-4}
24 5.1
1.7
1
1
{"1": 0.999729802749471,"2": 0.0,"3": 2.7019725052900413E-4}
25 4.8
1.9
1
1
{"1": 0.999729802749471,"2": 0.0,"3": 2.7019725052900413E-4}
34 5.5
1.4
1
1
{"1": 0.9896364785244597,"2": 0.010090797219753938,"3": 2.727242557863893E-4}
4
4.6
1.5
1
1
{"1": 0.999729802749471,"2": 0.0,"3": 2.7019725052900413E-4}
Cleanup.¶
In [20]:
# Delete the saved Model.
delete_byom("h2o_rf_iris", table_name="byom_models")
Model is deleted.
In [21]:
# Drop model table.
db_drop_table("byom_models")
Out[21]:
True
In [22]:
# Drop input data table.
db_drop_table("iris_input")
Out[22]:
True
In [23]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[23]:
True
In [ ]:
GBM
H2OPredict() using GBM model.¶
Setup¶
In [1]:
import tempfile
import getpass
from teradataml import create_context, DataFrame, save_byom, retrieve_byom, \
delete_byom, list_byom, remove_context, load_example_data, db_drop_table
from teradataml.options.configure import configure
from teradataml.analytics.byom.H2OPredict import H2OPredict
import h2o
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [4]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[4]:
id
sepal_length sepal_width petal_length petal_width species
120 6.0
2.2
5.0
1.5
3
59
6.6
2.9
4.6
1.3
2
99
5.1
2.5
3.0
1.1
2
61
5.0
2.0
3.5
1.0
2
78
6.7
3.0
5.0
1.7
2
101 6.3
3.3
6.0
2.5
3
141 6.7
3.1
5.6
2.4
3
17
5.4
3.9
1.3
0.4
1
38
4.9
3.6
1.4
0.1
1
19
5.7
3.8
1.7
0.3
1
In [5]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
7
4.6
3.4
1.4
0.3
1
70
5.6
2.5
3.9
1.1
2
47
5.1
3.8
1.6
0.2
1
108 7.3
2.9
6.3
1.8
3
18
5.1
3.5
1.4
0.3
1
51
7.0
3.2
4.7
1.4
2
68
5.8
2.7
4.1
1.0
2
87
6.7
3.1
4.7
1.5
2
3
4.7
3.2
1.3
0.2
1
74
6.1
2.8
4.7
1.2
2
Prepare dataset for a creating Gradient Boosting Machine model.¶
In [6]:
h2o.init()
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_train_pd = iris_train.to_pandas()
h2o_df = h2o.H2OFrame(iris_train_pd)
h2o_df
Checking whether there is an H2O instance running at http://localhost:54321 . connected.
H2O_cluster_uptime:
3 mins 26 secs
H2O_cluster_timezone:
America/Los_Angeles
H2O_data_parsing_timezone: UTC
H2O_cluster_version:
3.32.1.6
H2O_cluster_version_age:
1 month and 21 days
H2O_cluster_name:
H2O_from_python_gp186005_ip5q0u
H2O_cluster_total_nodes:
1
H2O_cluster_free_memory:
4.000 Gb
H2O_cluster_total_cores:
12
H2O_cluster_allowed_cores:
12
H2O_cluster_status:
locked, healthy
H2O_connection_url:
http://localhost:54321
H2O_connection_proxy:
{"http": null, "https": null}
H2O_internal_security:
False
H2O_API_Extensions:
Amazon S3, XGBoost, Algos, AutoML, Core V3,
TargetEncoder, Core V4
Python_version:
3.7.3 ﬁnal
Parse progress: |█████████████████████████████████████████████████████████| 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6.3
3.3
6
2.5
3
5.1
3.4
1.5
0.2
1
5.6
2.8
4.9
2
3
4.9
3.6
1.4
0.1
1
6.7
3.1
5.6
2.4
3
6
2.2
5
1.5
3
5.7
3.8
1.7
0.3
1
6.7
3
5
1.7
2
5.4
3.9
1.3
0.4
1
Out[6]:
Train Gradient Boosting Machine Model.¶
In [7]:
# Import required libraries.
from h2o.estimators import H2OGradientBoostingEstimator
In [8]:
# Add the code for training model. 
h2o_df["species"] = h2o_df["species"].asfactor()
predictors = h2o_df.columns
response = "species"
In [9]:
gbm_model = H2OGradientBoostingEstimator(nfolds=5, seed=1111, keep_cross_validation_predictions = True)
In [10]:
gbm_model.train(x=predictors, y=response, training_frame=h2o_df)
gbm Model Build progress: |███████████████████████████████████████████████| 100%
Save the model in MOJO format.¶
In [11]:
# Saving H2O Model to a file.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = gbm_model.save_mojo(path=f"{temp_dir.name}", force=True)
Save the model in Vantage.¶
In [12]:
# Save the H2O Model in Vantage.
save_byom(model_id="h2o_gbm_iris", model_file=model_file_path, table_name="byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [13]:
# List the models from "byom_models".
list_byom("byom_models")
                                 model
model_id                              
h2o_gbm_iris  b'504B03041400080808...'
Retrieve the model from Vantage.¶
In [14]:
# Retrieve the model from vantage using the model name 'h2o_gbm_iris'.
model=retrieve_byom("h2o_gbm_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [15]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [16]:
# Score the model on 'iris_test' data.
result = H2OPredict(newdata=iris_test,
                    newdata_partition_column='id',
                    newdata_order_column='id',
                    modeldata=model,
                    modeldata_order_column='model_id',
                    model_output_fields=['label', 'classProbabilities'],
                    accumulate=['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models='*',
                    enable_options='stageProbabilities',
                    model_type='OpenSource'
                   )
In [17]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".H2OPredict(
ON "MLDB"."ml__select__16344085400480" AS InputTable
PARTITION BY "id"
ORDER BY "id" 
ON (select model_id,model from "MLDB"."ml__filter__16344957882721") AS ModelTable
DIMENSION
ORDER BY "model_id"
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('label','classProbabilities')
OverwriteCachedModel('*')
EnableOptions('stageProbabilities')
) as sqlmr
In [18]:
# Print the result.
result.result
Out[18]:
id sepal_length petal_length prediction label
classprobabilities
39 4.4
1.3
1
1
{"1": 0.9984670598659316,"2": 8.48035477847887E-4,"3": 6.849046562205058E-4}
21 5.4
1.7
1
1
{"1": 0.9986887048769342,"2": 8.077411856592843E-4,"3": 5.035539374064341E-4}
45 5.1
1.9
1
1
{"1": 0.9984626318895735,"2": 0.0010341969303511627,"3": 5.031711800753143E-4}
1
5.1
1.4
1
1
{"1": 0.9989079708314166,"2": 5.881927758866791E-4,"3": 5.03836392696891E-4}
25 4.8
1.9
1
1
{"1": 0.9985815074040594,"2": 7.316813309016052E-4,"3": 6.868112650390427E-4}
13 4.8
1.4
1
1
{"1": 0.9984670598659316,"2": 8.48035477847887E-4,"3": 6.849046562205058E-4}
32 5.4
1.5
1
1
{"1": 0.998077533821194,"2": 0.0014197556779644385,"3": 5.027105008415158E-4}
81 5.5
3.8
2
2
{"1": 0.0014301867800769756,"2": 0.9913702502168862,"3": 0.007199563003036926}
8
5.0
1.5
1
1
{"1": 0.9986866811669394,"2": 8.095375733412047E-4,"3": 5.037812597193045E-4}
6
5.4
1.7
1
1
{"1": 0.99846517157086,"2": 0.0010319025506690632,"3": 5.029258784710217E-4}
Cleanup.¶
In [19]:
# Delete the saved Model.
delete_byom("h2o_gbm_iris", table_name="byom_models")
Model is deleted.
In [20]:
# Drop model table.
db_drop_table("byom_models")
Out[20]:
True
In [21]:
# Drop input data table.
db_drop_table("iris_input")
Out[21]:
True
In [22]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[22]:
True
In [ ]:
GLM
H2OPredict() using GLM model.¶
Setup¶
In [2]:
import tempfile
import getpass
from teradataml import create_context, DataFrame, save_byom, retrieve_byom, \
delete_byom, list_byom, remove_context, load_example_data, db_drop_table
from teradataml.options.configure import configure
from teradataml.analytics.byom.H2OPredict import H2OPredict
import h2o
In [3]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [4]:
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [5]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
120 6.0
2.2
5.0
1.5
3
78
6.7
3.0
5.0
1.7
2
76
6.6
3.0
4.4
1.4
2
101 6.3
3.3
6.0
2.5
3
139 6.0
3.0
4.8
1.8
3
19
5.7
3.8
1.7
0.3
1
59
6.6
2.9
4.6
1.3
2
99
5.1
2.5
3.0
1.1
2
17
5.4
3.9
1.3
0.4
1
38
4.9
3.6
1.4
0.1
1
In [6]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
55
6.5
2.8
4.6
1.5
2
36
5.0
3.2
1.2
0.2
1
30
4.7
3.2
1.6
0.2
1
61
5.0
2.0
3.5
1.0
2
93
5.8
2.6
4.0
1.2
2
141 6.7
3.1
5.6
2.4
3
9
4.4
2.9
1.4
0.2
1
49
5.3
3.7
1.5
0.2
1
38
4.9
3.6
1.4
0.1
1
99
5.1
2.5
3.0
1.1
2
Prepare dataset for a creating Generalised Linear Regression model.¶
In [7]:
h2o.init()
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_train_pd = iris_train.to_pandas()
h2o_df = h2o.H2OFrame(iris_train_pd)
h2o_df
Checking whether there is an H2O instance running at http://localhost:54321 . connected.
H2O_cluster_uptime:
8 mins 57 secs
H2O_cluster_timezone:
America/Los_Angeles
H2O_data_parsing_timezone: UTC
H2O_cluster_version:
3.32.1.6
H2O_cluster_version_age:
1 month and 21 days
H2O_cluster_name:
H2O_from_python_gp186005_ip5q0u
H2O_cluster_total_nodes:
1
H2O_cluster_free_memory:
3.998 Gb
H2O_cluster_total_cores:
12
H2O_cluster_allowed_cores:
12
H2O_cluster_status:
locked, healthy
H2O_connection_url:
http://localhost:54321
H2O_connection_proxy:
{"http": null, "https": null}
H2O_internal_security:
False
H2O_API_Extensions:
Amazon S3, XGBoost, Algos, AutoML, Core V3,
TargetEncoder, Core V4
Python_version:
3.7.3 ﬁnal
Parse progress: |█████████████████████████████████████████████████████████| 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6.3
3.3
6
2.5
3
5.1
3.4
1.5
0.2
1
5.6
2.8
4.9
2
3
4.9
3.6
1.4
0.1
1
6.7
3.1
5.6
2.4
3
5.7
2.6
3.5
1
2
6.6
2.9
4.6
1.3
2
6.6
3
4.4
1.4
2
5.4
3.9
1.3
0.4
1
Out[7]:
Train Genralised Linear Regression Model.¶
In [8]:
# Import required libraries.
from h2o.estimators.glm import H2OGeneralizedLinearEstimator
In [9]:
# Add the code for training model. 
h2o_df["species"] = h2o_df["species"].asfactor()
predictors = h2o_df.columns
response = "species"
In [10]:
glm_model = H2OGeneralizedLinearEstimator()
In [11]:
glm_model.train(x=predictors, y=response, training_frame=h2o_df)
glm Model Build progress: |███████████████████████████████████████████████| 100%
Save the model in MOJO format.¶
In [12]:
# Saving H2O Model to a file.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = glm_model.save_mojo(path=f"{temp_dir.name}", force=True)
Save the model in Vantage.¶
In [13]:
# Save the H2O Model in Vantage.
save_byom(model_id="h2o_glm_iris", model_file=model_file_path, table_name="byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [14]:
# List the models from "byom_models".
list_byom("byom_models")
                                 model
model_id                              
h2o_glm_iris  b'504B03041400080808...'
Retrieve the model from Vantage.¶
In [15]:
# Retrieve the model from vantage using the model name 'h2o_glm_iris'.
model=retrieve_byom(model_id="h2o_glm_iris", table_name="byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [16]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [17]:
# Score the model on 'iris_test' data.
result = H2OPredict(newdata=iris_test,
                    newdata_partition_column='id',
                    newdata_order_column='id',
                    modeldata=model,
                    modeldata_order_column='model_id',
                    model_output_fields=['label', 'classProbabilities'],
                    accumulate=['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models='*',
                    enable_options='stageProbabilities',
                    model_type='OpenSource'
                   )
In [18]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".H2OPredict(
ON "MLDB"."ml__select__16344331845030" AS InputTable
PARTITION BY "id"
ORDER BY "id" 
ON (select model_id,model from "MLDB"."ml__filter__16345163360268") AS ModelTable
DIMENSION
ORDER BY "model_id"
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('label','classProbabilities')
OverwriteCachedModel('*')
EnableOptions('stageProbabilities')
) as sqlmr
In [19]:
# Print the result.
result.result
Out[19]:
id sepal_length petal_length prediction label
classprobabilities
88 6.3
4.4
2
2
{"1": 1.1783597564925874E-4,"2": 0.967322787872112,"3": 0.03255937615223876}
26 5.0
1.6
1
1
{"1": 0.9845786767142926,"2": 0.01542132328543173,"3": 2.757285440583015E-13}
60 5.2
3.9
2
2
{"1": 0.011952398134030913,"2": 0.9802572428184518,"3": 0.00779035904751729}
31 4.8
1.6
1
1
{"1": 0.9932486533040435,"2": 0.006751346695857105,"3": 9.95222274874653E-14}
45 5.1
1.9
1
1
{"1": 0.9975636889484245,"2": 0.0024363110515080834,"3": 6.740997390287097E-14}
50 5.0
1.4
1
1
{"1": 0.9965247902402493,"2": 0.003475209759737982,"3": 1.2509455236308859E-14}
54 5.5
4.0
2
2
{"1": 0.0014189199773822644,"2": 0.9860378216942937,"3": 0.012543258328324173}
57 6.3
4.7
2
2
{"1": 0.0015271539667560878,"2": 0.9282112143323902,"3": 0.07026163170085384}
36 5.0
1.2
1
1
{"1": 0.9963562311141493,"2": 0.003643768885841865,"3": 8.783774210261064E-15}
3
4.7
1.3
1
1
{"1": 0.9977891670380162,"2": 0.002210832961974951,"3": 8.89184767577873E-15}
Cleanup.¶
In [20]:
# Delete the saved Model from the table byom_models, using the model id h2o_glm_iris.
delete_byom("h2o_glm_iris", table_name="byom_models")
Model is deleted.
In [21]:
# Drop model table.
db_drop_table("byom_models")
Out[21]:
True
In [22]:
# Drop input data table.
db_drop_table("iris_input")
Out[22]:
True
In [23]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[23]:
True
In [ ]:
GLRM
H2OPredict() using Generalized Low Rank model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import create_context, DataFrame, save_byom, retrieve_byom, \
delete_byom, list_byom, remove_context, load_example_data, db_drop_table
from teradataml.options.configure import configure
from teradataml.analytics.byom.H2OPredict import H2OPredict
import h2o
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
# Load the example data. 
load_example_data("byom", "iris_input")
In [4]:
# Create the teradataml DataFrames.
iris_input = DataFrame("iris_input")
In [5]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [6]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
120 6.0
2.2
5.0
1.5
3
59
6.6
2.9
4.6
1.3
2
99
5.1
2.5
3.0
1.1
2
61
5.0
2.0
3.5
1.0
2
78
6.7
3.0
5.0
1.7
2
141 6.7
3.1
5.6
2.4
3
17
5.4
3.9
1.3
0.4
1
34
5.5
4.2
1.4
0.2
1
38
4.9
3.6
1.4
0.1
1
122 5.6
2.8
4.9
2.0
3
In [7]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[7]:
id
sepal_length sepal_width petal_length petal_width species
95
5.6
2.7
4.2
1.3
2
74
6.1
2.8
4.7
1.2
2
114 5.7
2.5
5.0
2.0
3
61
5.0
2.0
3.5
1.0
2
5
5.0
3.6
1.4
0.2
1
13
4.8
3.0
1.4
0.1
1
11
5.4
3.7
1.5
0.2
1
49
5.3
3.7
1.5
0.2
1
76
6.6
3.0
4.4
1.4
2
122 5.6
2.8
4.9
2.0
3
Prepare dataset for creating a Generalized Low Rank model.¶
In [8]:
h2o.init()
Checking whether there is an H2O instance running at http://localhost:54321 ..... not found.
Attempting to start a local H2O server...
; Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
  Starting server from C:\Users\pg255042\Anaconda3\envs\teraml\lib\site-packages\h2o\backend\bin\h2o.jar
  Ice root: C:\Users\pg255042\AppData\Local\Temp\tmp75i0b0gf
  JVM stdout: C:\Users\pg255042\AppData\Local\Temp\tmp75i0b0gf\h2o_pg255042_started_from_python.out
  JVM stderr: C:\Users\pg255042\AppData\Local\Temp\tmp75i0b0gf\h2o_pg255042_started_from_python.err
  Server is running at http://127.0.0.1:54321
Connecting to H2O server at http://127.0.0.1:54321 ... successful.
Warning: Your H2O cluster version is too old (5 months and 7 days)!Please download and install the latest version from http://h2o.ai/
H2O_cluster_uptime:
02 secs
H2O_cluster_timezone:
Asia/Kolkata
H2O_data_parsing_timezone: UTC
H2O_cluster_version:
3.34.0.1
H2O_cluster_version_age:
5 months and 7 days !!!
H2O_cluster_name:
H2O_from_python_pg255042_ie998m
H2O_cluster_total_nodes:
1
H2O_cluster_free_memory:
7.052 Gb
H2O_cluster_total_cores:
16
H2O_cluster_allowed_cores:
16
H2O_cluster_status:
locked, healthy
H2O_connection_url:
http://127.0.0.1:54321
H2O_connection_proxy:
{"http": null, "https": null}
H2O_internal_security:
False
H2O_API_Extensions:
Amazon S3, Algos, AutoML, Core V3, TargetEncoder,
Core V4
Python_version:
3.6.12 ﬁnal
In [9]:
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_train_pd = iris_train.to_pandas()
h2o_df = h2o.H2OFrame(iris_train_pd)
h2o_df
Parse progress: |████████████████████████████████████████████████████████████████| (done) 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6.3
3.3
6
2.5
3
5.1
3.4
1.5
0.2
1
6.6
2.9
4.6
1.3
2
6.7
3
5
1.7
2
6.7
3.1
5.6
2.4
3
5.7
2.6
3.5
1
2
5.1
2.5
3
1.1
2
6.6
3
4.4
1.4
2
5.4
3.9
1.3
0.4
1
Out[9]:
Train Generalized Low Rank Model.¶
In [10]:
# Import required libraries.
from h2o.estimators import H2OGeneralizedLowRankEstimator
In [11]:
# Add the code for training model. 
h2o_df["species"] = h2o_df["species"].asfactor()
predictors = h2o_df.columns
response = "species"
In [12]:
glrm_model = H2OGeneralizedLowRankEstimator(k=4,
                                            loss="quadratic",
                                            gamma_x=0.5,
                                            gamma_y=0.5,
                                            max_iterations=700,
                                            recover_svd=True,
                                            init="SVD",
                                            transform="standardize")
In [13]:
glrm_model.train(x=predictors, y=response, training_frame=h2o_df)
glrm Model Build progress: |█████████████████████████████████████████████████████| (done) 100%
Model Details
=============
H2OGeneralizedLowRankEstimator :  Generalized Low Rank Modeling
Model Key:  GLRM_model_python_1645512818794_1
Model Summary: 
number_of_iterations ﬁnal_step_size ﬁnal_objective_value
0
164.0
0.000054
2.439693
ModelMetricsGLRM: glrm
** Reported on train data. **
MSE: NaN
RMSE: NaN
Sum of Squared Error (Numeric): 2.4396928934183713
Misclassification Error (Categorical): 0.0
Scoring History: 
timestamp
duration iterations step_size
objective
0
2022-02-22 12:23:42 0.194 sec 0.0
0.666667
267.142579
1
2022-02-22 12:23:42 0.202 sec 1.0
0.444444
267.142579
2
2022-02-22 12:23:42 0.204 sec 2.0
0.222222
267.142579
3
2022-02-22 12:23:42 0.204 sec 3.0
0.233333
115.558661
4
2022-02-22 12:23:42 0.204 sec 4.0
0.245000
81.991977
5
2022-02-22 12:23:42 0.212 sec 5.0
0.163333
81.991977
6
2022-02-22 12:23:42 0.214 sec 6.0
0.108889
81.991977
7
2022-02-22 12:23:42 0.214 sec 7.0
0.114333
78.045369
8
2022-02-22 12:23:42 0.214 sec 8.0
0.120050
32.878496
9
2022-02-22 12:23:42 0.224 sec 9.0
0.126053
26.644778
10
2022-02-22 12:23:42 0.224 sec 10.0
0.132355
22.624142
11
2022-02-22 12:23:42 0.224 sec 11.0
0.138973
21.787503
12
2022-02-22 12:23:42 0.224 sec 12.0
0.145922
20.384177
13
2022-02-22 12:23:42 0.232 sec 13.0
0.153218
20.325123
14
2022-02-22 12:23:42 0.234 sec 14.0
0.102145
20.325123
15
2022-02-22 12:23:42 0.234 sec 15.0
0.107252
19.113147
16
2022-02-22 12:23:42 0.234 sec 16.0
0.112615
18.151736
17
2022-02-22 12:23:42 0.234 sec 17.0
0.118246
16.879529
18
2022-02-22 12:23:42 0.234 sec 18.0
0.124158
15.773687
19
2022-02-22 12:23:42 0.234 sec 19.0
0.130366
14.635686
See the whole table with table.as_data_frame()
Out[13]:
Save the model in MOJO format.¶
In [14]:
# Saving H2O Model to a file.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = glrm_model.save_mojo(path=f"{temp_dir.name}", force=True)
Save the model in Vantage.¶
In [15]:
# Save the H2O Model in Vantage.
save_byom(model_id="h2o_glrm_iris", model_file=model_file_path, table_name="byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [16]:
# List the models from "byom_models".
list_byom("byom_models")
                                  model
model_id                               
h2o_glrm_iris  b'504B03041400080808...'
Retrieve the model from Vantage.¶
In [17]:
# Retrieve the model from vantage using the model name 'h2o_glrm_iris'.
model=retrieve_byom(model_id="h2o_glrm_iris", table_name="byom_models")
In [18]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [19]:
# Score the model on 'iris_test' data.
result = H2OPredict(newdata=iris_test,
                    newdata_partition_column='id',
                    newdata_order_column='id',
                    modeldata=model,
                    modeldata_order_column='model_id',
                    accumulate=['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models='*',
                    model_type='OpenSource'
                   )
In [20]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".H2OPredict(
ON "MLDB"."ml__select__1645515524758530" AS InputTable
PARTITION BY "id"
ORDER BY "id" 
ON (select model_id,model from "MLDB"."ml__filter__1645515219191021") AS ModelTable
DIMENSION
ORDER BY "model_id"
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [21]:
# Print the result.
result.result
Out[21]:
id sepal_length petal_length prediction
json_report
22 5.1
1.5
{"dimensions":[-0.8755884190821872,0.4472017598306644,0.0532540017688253,0.2505514991946427],"reconstructed":
[-1.009702009214914,1.5104019390305319,-1.3566496362877019,-1.1886252973763436,0.0]}
69 6.2
4.5
{"dimensions":[0.44062561105843706,-0.7916202873653729,-0.3336130726414549,-0.4420232986958351],"reconstructed":
[0.4809519782109252,-1.93648862326932,0.5148422067271403,0.049406695050708264,1.0]}
70 5.6
3.9
{"dimensions":[0.13632624776477564,-0.6378740362476013,-0.0722942433882558,-0.33335869761244624],"reconstructed":
[-0.27350355503007406,-1.2936172770748544,0.07635317739547576,-0.10145571777357752,1.0]}
38 4.9
1.4
{"dimensions":[-0.8875034739578439,0.1932973956542112,0.008587761196810413,-0.04397949380212729],"reconstructed":
[-1.0909535331053124,1.2128618884339772,-1.4153364081567559,-1.335276578823825,0.0]}
79 6.0
4.5
{"dimensions":[0.26771446285926187,-0.6336090550181356,0.09822169481263057,-1.1825998615107771],"reconstructed":
[0.19960990942625179,-0.3716295612296847,0.4469025366948274,0.3939021784864294,1.0]}
28 5.2
1.5
{"dimensions":[-0.7745605227270265,0.2835218241950847,-0.11580179457286491,0.19887924875050728],"reconstructed":
[-0.7606347036494236,0.9995229315594196,-1.254574771653111,-1.2761817446023618,0.0]}
32 5.4
1.5
{"dimensions":[-0.7716361460682887,0.2706672006585394,-0.2502528329963879,0.1877470447422418],"reconstructed":
[-0.5507176169053385,0.8985721493293917,-1.2721708549892874,-1.4227990192352702,0.0]}
49 5.3
1.5
{"dimensions":[-0.8194097876612587,0.39619978768187836,-0.09611918939413275,0.06587828131922056],"reconstructed":
[-0.6329177334723323,1.455303697358055,-1.273552156703061,-1.2628573264621934,0.0]}
60 5.2
3.9
{"dimensions":[0.1025619380710769,-0.5344523996790395,0.27905786476378636,-0.3166428876496549],"reconstructed":
[-0.7654455093212404,-0.8313998540458245,0.1012621944000312,0.2715011240655108,1.0]}
15 5.8
1.2
{"dimensions":[-0.8718901017234719,0.7039189705709991,-0.2396379306648317,0.037308632699902235],"reconstructed":
[-0.0754781649926874,2.166104713835355,-1.287416301738693,-1.3662770290526147,0.0]}
Cleanup.¶
In [22]:
# Delete the saved Model from the table byom_models, using the model id h2o_glrm_iris.
delete_byom("h2o_glrm_iris", table_name="byom_models")
Model is deleted.
In [23]:
# Drop model table.
db_drop_table("byom_models")
Out[23]:
True
In [24]:
# Drop input data table.
db_drop_table("iris_input")
Out[24]:
True
In [25]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[25]:
True
Isolation Forest
H2OPredict() using Isolation Forest model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import create_context, DataFrame, save_byom, retrieve_byom, \
delete_byom, list_byom, remove_context, load_example_data, db_drop_table
from teradataml.options.configure import configure
from teradataml.analytics.byom.H2OPredict import H2OPredict
import h2o
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
# Load the example data. 
load_example_data("byom", "iris_input")
WARNING: Skipped loading table iris_input since it already exists in the database.
In [4]:
# Create the teradataml DataFrames.
iris_input = DataFrame("iris_input")
In [5]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [6]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
116 6.4
3.2
5.3
2.3
3
139 6.0
3.0
4.8
1.8
3
34
5.5
4.2
1.4
0.2
1
40
5.1
3.4
1.5
0.2
1
120 6.0
2.2
5.0
1.5
3
122 5.6
2.8
4.9
2.0
3
59
6.6
2.9
4.6
1.3
2
99
5.1
2.5
3.0
1.1
2
80
5.7
2.6
3.5
1.0
2
17
5.4
3.9
1.3
0.4
1
In [7]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[7]:
id
sepal_length sepal_width petal_length petal_width species
60
5.2
2.7
3.9
1.4
2
139 6.0
3.0
4.8
1.8
3
51
7.0
3.2
4.7
1.4
2
120 6.0
2.2
5.0
1.5
3
55
6.5
2.8
4.6
1.5
2
59
6.6
2.9
4.6
1.3
2
36
5.0
3.2
1.2
0.2
1
97
5.7
2.9
4.2
1.3
2
118 7.7
3.8
6.7
2.2
3
101 6.3
3.3
6.0
2.5
3
Prepare dataset for creating an Isolation Forest.¶
In [8]:
h2o.init()
Checking whether there is an H2O instance running at http://localhost:54321 ..... not found.
Attempting to start a local H2O server...
; Java HotSpot(TM) 64-Bit Server VM (build 17.0.2+8-LTS-86, mixed mode, sharing)
  Starting server from c:\users\ar255086\appdata\local\programs\python\python37\lib\site-packages\h2o\backend\bin\h2o.jar
  Ice root: C:\Users\ar255086\AppData\Local\Temp\tmpbvd7h_jb
  JVM stdout: C:\Users\ar255086\AppData\Local\Temp\tmpbvd7h_jb\h2o_ar255086_started_from_python.out
  JVM stderr: C:\Users\ar255086\AppData\Local\Temp\tmpbvd7h_jb\h2o_ar255086_started_from_python.err
  Server is running at http://127.0.0.1:54321
Connecting to H2O server at http://127.0.0.1:54321 ... successful.
H2O_cluster_uptime:
01 secs
H2O_cluster_timezone:
Asia/Kolkata
H2O_data_parsing_timezone:
UTC
H2O_cluster_version:
3.36.0.3
H2O_cluster_version_age:
1 month and 4 days
H2O_cluster_name:
H2O_from_python_ar255086_tizp5y
H2O_cluster_total_nodes:
1
H2O_cluster_free_memory:
7.934 Gb
H2O_cluster_total_cores:
16
H2O_cluster_allowed_cores:
16
H2O_cluster_status:
locked, healthy
H2O_connection_url:
http://127.0.0.1:54321
H2O_connection_proxy:
{"http": null, "https": null}
H2O_internal_security:
False
Python_version:
3.7.8 ﬁnal
In [9]:
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_train_pd = iris_train.to_pandas()
h2o_df = h2o.H2OFrame(iris_train_pd)
h2o_df
Parse progress: |████████████████████████████████████████████████████████████████| (done) 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6.3
3.3
6
2.5
3
5.1
3.4
1.5
0.2
1
5.6
2.8
4.9
2
3
4.9
3.6
1.4
0.1
1
5.4
3.9
1.3
0.4
1
5.7
2.6
3.5
1
2
5.7
3.8
1.7
0.3
1
6.7
3
5
1.7
2
6
3
4.8
1.8
3
Out[9]:
Train an isolation forest model using H2O.¶
In [10]:
# Import required libraries.
from h2o.estimators import H2OIsolationForestEstimator
In [11]:
# Add the code for training model. 
h2o_df["species"] = h2o_df["species"].asfactor()
predictors = h2o_df.columns
response = "species"
In [12]:
Isolation_Forest_model = H2OIsolationForestEstimator(sample_rate = 0.1,
                                    max_depth = 20,
                                    ntrees = 50)
In [13]:
Isolation_Forest_model.train(x=predictors, y=response, training_frame=h2o_df)
isolationforest Model Build progress: |██████████████████████████████████████████| (done) 100%
Model Details
=============
H2OIsolationForestEstimator :  Isolation Forest
Model Key:  IsolationForest_model_python_1647851494184_1
Model Summary: 
number_of_trees number_of_internal_trees model_size_in_bytes min_depth max_depth mean_depth min_leaves max_leaves mean_leaves
0
50.0
50.0
8034.0
1.0
9.0
4.84
1.0
18.0
8.16
ModelMetricsAnomaly: isolationforest
** Reported on train data. **
Anomaly Score: 2.316002830125688
Normalized Anomaly Score: 0.43456615424340295
Scoring History: 
timestamp
duration number_of_trees mean_tree_path_length mean_anomaly_score
0
2022-03-21 14:01:38 0.012 sec 0.0
NaN
NaN
1
2022-03-21 14:01:38 0.085 sec 1.0
2.061947
0.469027
2
2022-03-21 14:01:38 0.095 sec 2.0
2.025210
0.389916
3
2022-03-21 14:01:38 0.103 sec 3.0
2.378531
0.533128
4
2022-03-21 14:01:39 0.110 sec 4.0
2.309117
0.517569
5
2022-03-21 14:01:39 0.113 sec 5.0
1.849858
0.516714
6
2022-03-21 14:01:39 0.120 sec 6.0
1.876891
0.541036
7
2022-03-21 14:01:39 0.123 sec 7.0
1.602302
0.543549
8
2022-03-21 14:01:39 0.130 sec 8.0
1.847429
0.511028
9
2022-03-21 14:01:39 0.135 sec 9.0
1.982093
0.507326
10
2022-03-21 14:01:39 0.141 sec 10.0
2.222943
0.452714
11
2022-03-21 14:01:39 0.146 sec 11.0
2.321387
0.411510
12
2022-03-21 14:01:39 0.153 sec 12.0
2.291757
0.395788
13
2022-03-21 14:01:39 0.159 sec 13.0
2.281198
0.406280
14
2022-03-21 14:01:39 0.165 sec 14.0
2.286250
0.428304
15
2022-03-21 14:01:39 0.172 sec 15.0
2.336795
0.431602
16
2022-03-21 14:01:39 0.185 sec 16.0
2.258393
0.423775
17
2022-03-21 14:01:39 0.194 sec 17.0
2.214138
0.405702
18
2022-03-21 14:01:39 0.201 sec 18.0
2.228324
0.410006
19
2022-03-21 14:01:39 0.209 sec 19.0
2.345957
0.412195
See the whole table with table.as_data_frame()
Out[13]:
Save the model in MOJO format.¶
In [14]:
# Saving H2O Model to a file.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = Isolation_Forest_model.save_mojo(path=f"{temp_dir.name}", force=True)
Save the model in Vantage.¶
In [15]:
# Save the H2O Model in Vantage.
save_byom(model_id="h2o_Isolation_Forest_iris", model_file=model_file_path, table_name="byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [16]:
# List the models from "byom_models".
list_byom("byom_models")
                                              model
model_id                                           
h2o_Isolation_Forest_iris  b'504B03041400080808...'
Retrieve the model from Vantage.¶
In [17]:
# Retrieve the model from vantage using the model name 'h2o_Isolation_Forest_iris'.
model=retrieve_byom(model_id="h2o_Isolation_Forest_iris", table_name="byom_models")
In [18]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [19]:
# Score the model on 'iris_test' data.
result = H2OPredict(newdata=iris_test,
                    newdata_partition_column='id',
                    newdata_order_column='id',
                    modeldata=model,
                    modeldata_order_column='model_id',
                    model_output_fields=['normalizedScore'],
                    accumulate=['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models='*',
                    model_type='OpenSource'
                   )
In [20]:
# Print the query.
print(result.show_query())
SELECT * FROM "alice".H2OPredict(
ON "ALICE"."ml__select__1647853163572773" AS InputTable
PARTITION BY "id"
ORDER BY "id" 
ON (select model_id,model from "ALICE"."ml__filter__1647856509161774") AS ModelTable
DIMENSION
ORDER BY "model_id"
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('normalizedScore')
OverwriteCachedModel('*')
) as sqlmr
In [21]:
# Print the result.
result.result
Out[21]:
id sepal_length petal_length prediction
normalizedscore
50 5.0
1.4
2.5
0.32098765432098764
60 5.2
3.9
2.52
0.30864197530864196
73 6.3
4.9
2.5
0.32098765432098764
9
4.4
1.4
2.32
0.43209876543209874
66 6.7
4.4
2.04
0.6049382716049383
46 4.8
1.4
2.5
0.32098765432098764
53 6.9
4.9
2.1
0.5679012345679012
59 6.6
4.6
2.14
0.5432098765432098
11 5.4
1.5
2.42
0.37037037037037035
38 4.9
1.4
2.24
0.48148148148148145
Cleanup.¶
In [22]:
# Delete the saved Model from the table byom_models, using the model id h2o_Isolation_Forest_iris.
delete_byom("h2o_Isolation_Forest_iris", table_name="byom_models")
Model is deleted.
In [23]:
# Drop model table.
db_drop_table("byom_models")
Out[23]:
True
In [24]:
# Drop input data table.
db_drop_table("iris_input")
Out[24]:
True
In [25]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[25]:
True
KMeans Clustering
H2OPredict() using KMeans model.¶
Setup¶
In [1]:
import tempfile
import getpass
import teradataml as td
from teradataml import create_context, remove_context, load_example_data, DataFrame, \
db_drop_table, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
from teradataml.analytics.byom.H2OPredict import H2OPredict
import h2o
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [4]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[4]:
id
sepal_length sepal_width petal_length petal_width species
99
5.1
2.5
3.0
1.1
2
120 6.0
2.2
5.0
1.5
3
57
6.3
3.3
4.7
1.6
2
141 6.7
3.1
5.6
2.4
3
139 6.0
3.0
4.8
1.8
3
61
5.0
2.0
3.5
1.0
2
38
4.9
3.6
1.4
0.1
1
78
6.7
3.0
5.0
1.7
2
17
5.4
3.9
1.3
0.4
1
80
5.7
2.6
3.5
1.0
2
In [5]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
47
5.1
3.8
1.6
0.2
1
127 6.2
2.8
4.8
1.8
3
62
5.9
3.0
4.2
1.5
2
139 6.0
3.0
4.8
1.8
3
104 6.3
2.9
5.6
1.8
3
38
4.9
3.6
1.4
0.1
1
116 6.4
3.2
5.3
2.3
3
3
4.7
3.2
1.3
0.2
1
51
7.0
3.2
4.7
1.4
2
40
5.1
3.4
1.5
0.2
1
Prepare dataset for creating a KMeans model.¶
In [6]:
h2o.init()
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_train_pd = iris_train.to_pandas()
h2o_df_train = h2o.H2OFrame(iris_train_pd)
h2o_df_train
Checking whether there is an H2O instance running at http://localhost:54321 . connected.
H2O_cluster_uptime:
9 mins 58 secs
H2O_cluster_timezone:
America/Los_Angeles
H2O_data_parsing_timezone: UTC
H2O_cluster_version:
3.32.1.6
H2O_cluster_version_age:
1 month and 21 days
H2O_cluster_name:
H2O_from_python_gp186005_ip5q0u
H2O_cluster_total_nodes:
1
H2O_cluster_free_memory:
3.998 Gb
H2O_cluster_total_cores:
12
H2O_cluster_allowed_cores:
12
H2O_cluster_status:
locked, healthy
H2O_connection_url:
http://localhost:54321
H2O_connection_proxy:
{"http": null, "https": null}
H2O_internal_security:
False
H2O_API_Extensions:
Amazon S3, XGBoost, Algos, AutoML, Core V3,
TargetEncoder, Core V4
Python_version:
3.7.3 ﬁnal
Parse progress: |█████████████████████████████████████████████████████████| 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6.3
3.3
6
2.5
3
5.1
3.4
1.5
0.2
1
6.6
2.9
4.6
1.3
2
4.9
3.6
1.4
0.1
1
6.7
3.1
5.6
2.4
3
6
2.2
5
1.5
3
5.1
2.5
3
1.1
2
6.7
3
5
1.7
2
5.4
3.9
1.3
0.4
1
Out[6]:
In [7]:
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_test_pd = iris_test.to_pandas()
h2o_df_test = h2o.H2OFrame(iris_test_pd)
h2o_df_test
Parse progress: |█████████████████████████████████████████████████████████| 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6
3
4.8
1.8
3
5.7
2.6
3.5
1
2
5.7
2.9
4.2
1.3
2
6.4
2.8
5.6
2.2
3
4.8
3
1.4
0.1
1
6.3
3.3
4.7
1.6
2
6.3
3.4
5.6
2.4
3
4.7
3.2
1.3
0.2
1
5.2
3.5
1.5
0.2
1
Out[7]:
Train KMeans Model.¶
In [8]:
# Import required libraries.
from h2o.estimators import H2OKMeansEstimator
In [9]:
# Add the code for training model. 
h2o_df_train["species"] = h2o_df_train["species"].asfactor()
predictors = h2o_df_train.columns
response = "species"
In [10]:
iris_kmeans = H2OKMeansEstimator(k=10, estimate_k=True, standardize=False, seed=1234)
In [11]:
iris_kmeans.train(x=predictors, training_frame=h2o_df_train, validation_frame=h2o_df_test)
kmeans Model Build progress: |████████████████████████████████████████████| 100%
Save the model in MOJO format.¶
In [12]:
# Saving H2O Model to a file.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = iris_kmeans.save_mojo(path=f"{temp_dir.name}", force=True)
Save the model in Vantage.¶
In [13]:
# Save the H2O Model in Vantage.
save_byom("h2o_kmeans_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [14]:
# List the models from "byom_models".
list_byom("byom_models")
                                    model
model_id                                 
h2o_kmeans_iris  b'504B03041400080808...'
Retrieve the model from Vantage.¶
In [15]:
# Retrieve the model from table "byom_models", using the model id 'h2o_kmeans_iris'.
modeldata = retrieve_byom("h2o_kmeans_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [16]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [17]:
result = H2OPredict(newdata=iris_test,
                    newdata_partition_column='id',
                    newdata_order_column='id',
                    modeldata=modeldata,
                    modeldata_order_column='model_id',
                    accumulate=['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models='*',
                    model_type='OpenSource'
                   )
In [18]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".H2OPredict(
ON "MLDB"."ml__select__16344484461915" AS InputTable
PARTITION BY "id"
ORDER BY "id" 
ON (select model_id,model from "MLDB"."ml__filter__16346196198222") AS ModelTable
DIMENSION
ORDER BY "model_id"
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [19]:
# Print the result.
result.result
Out[19]:
id sepal_length petal_length prediction json_report
21 5.4
1.7
0
{"cluster":0}
27 5.0
1.6
0
{"cluster":0}
35 4.9
1.5
0
{"cluster":0}
17 5.4
1.3
0
{"cluster":0}
89 5.6
4.1
1
{"cluster":1}
43 4.4
1.3
0
{"cluster":0}
56 5.7
4.5
1
{"cluster":1}
60 5.2
3.9
1
{"cluster":1}
64 6.1
4.7
1
{"cluster":1}
22 5.1
1.5
0
{"cluster":0}
Cleanup.¶
In [20]:
# Delete the model from table "byom_models", using the model id 'h2o_kmeans_iris'.
delete_byom("h2o_kmeans_iris", "byom_models")
Model is deleted.
In [21]:
# Drop models table.
db_drop_table("byom_models")
Out[21]:
True
In [22]:
# Drop input data table.
db_drop_table("iris_input")
Out[22]:
True
In [23]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[23]:
True
In [ ]:
PCA
H2OPredict() using PCA model.¶
Setup¶
In [1]:
import tempfile
import getpass
from teradataml import create_context, DataFrame, save_byom, retrieve_byom, \
delete_byom, list_byom, remove_context, load_example_data, db_drop_table
from teradataml.options.configure import configure
from teradataml.analytics.byom.H2OPredict import H2OPredict
import h2o
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
In [4]:
# Create teradataml DataFrames.
iris_input = DataFrame("iris_input")
In [5]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [6]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
17
5.4
3.9
1.3
0.4
1
38
4.9
3.6
1.4
0.1
1
78
6.7
3.0
5.0
1.7
2
122 5.6
2.8
4.9
2.0
3
59
6.6
2.9
4.6
1.3
2
40
5.1
3.4
1.5
0.2
1
120 6.0
2.2
5.0
1.5
3
57
6.3
3.3
4.7
1.6
2
19
5.7
3.8
1.7
0.3
1
61
5.0
2.0
3.5
1.0
2
In [7]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[7]:
id
sepal_length sepal_width petal_length petal_width species
87
6.7
3.1
4.7
1.5
2
76
6.6
3.0
4.4
1.4
2
116 6.4
3.2
5.3
2.3
3
99
5.1
2.5
3.0
1.1
2
114 5.7
2.5
5.0
2.0
3
80
5.7
2.6
3.5
1.0
2
118 7.7
3.8
6.7
2.2
3
55
6.5
2.8
4.6
1.5
2
15
5.8
4.0
1.2
0.2
1
61
5.0
2.0
3.5
1.0
2
Prepare dataset for creating PCA Model.¶
In [8]:
h2o.init()
Checking whether there is an H2O instance running at http://localhost:54321 . connected.
Warning: Your H2O cluster version is too old (5 months and 7 days)!Please download and install the latest version from http://h2o.ai/
H2O_cluster_uptime:
15 mins 54 secs
H2O_cluster_timezone:
Asia/Kolkata
H2O_data_parsing_timezone: UTC
H2O_cluster_version:
3.34.0.1
H2O_cluster_version_age:
5 months and 7 days !!!
H2O_cluster_name:
H2O_from_python_pg255042_ie998m
H2O_cluster_total_nodes:
1
H2O_cluster_free_memory:
6.507 Gb
H2O_cluster_total_cores:
16
H2O_cluster_allowed_cores:
16
H2O_cluster_status:
locked, healthy
H2O_connection_url:
http://localhost:54321
H2O_connection_proxy:
{"http": null, "https": null}
H2O_internal_security:
False
H2O_API_Extensions:
Amazon S3, Algos, AutoML, Core V3, TargetEncoder,
Core V4
Python_version:
3.6.12 ﬁnal
In [9]:
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_train_pd = iris_train.to_pandas()
h2o_df = h2o.H2OFrame(iris_train_pd)
h2o_df
Parse progress: |████████████████████████████████████████████████████████████████| (done) 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6.3
3.3
6
2.5
3
5.1
3.4
1.5
0.2
1
5.6
2.8
4.9
2
3
4.9
3.6
1.4
0.1
1
6.7
3.1
5.6
2.4
3
5.7
2.6
3.5
1
2
5.7
3.8
1.7
0.3
1
6.7
3
5
1.7
2
5.4
3.9
1.3
0.4
1
Out[9]:
Train PCA Model.¶
In [10]:
# Import required libraries.
from h2o.estimators import H2OPrincipalComponentAnalysisEstimator
In [11]:
# Add the code for training model. 
h2o_df["species"] = h2o_df["species"].asfactor()
predictors = h2o_df.columns
response = "species"
In [12]:
pca_model = H2OPrincipalComponentAnalysisEstimator(k = 4,
                                                   use_all_factor_levels = True,
                                                   pca_method = "glrm",
                                                   transform = "standardize",
                                                   impute_missing = True)
In [13]:
pca_model.train(x=predictors, y=response, training_frame=h2o_df)
pca Model Build progress: |██████████████████████████████████████████████████████| (done) 100%
Model Details
=============
H2OPrincipalComponentAnalysisEstimator :  Principal Components Analysis
Model Key:  PCA_model_python_1645512818794_3
Importance of components: 
pc1
pc2
pc3
pc4
0 Standard deviation
1.621899 1.320243 0.647021 0.464940
1 Proportion of Variance 0.525229 0.348023 0.083587 0.043161
2 Cumulative Proportion 0.525229 0.873252 0.956839 1.000000
ModelMetricsPCA: pca
** Reported on train data. **
MSE: NaN
RMSE: NaN
Scoring history from GLRM: 
timestamp
duration iterations step_size
objective
0
2022-02-22 12:39:50 0.062 sec 0.0
0.666667
894.252841
1
2022-02-22 12:39:50 0.062 sec 1.0
0.444444
894.252841
2
2022-02-22 12:39:50 0.062 sec 2.0
0.222222
894.252841
3
2022-02-22 12:39:50 0.078 sec 3.0
0.233333
712.328243
4
2022-02-22 12:39:50 0.078 sec 4.0
0.155556
712.328243
5
2022-02-22 12:39:50 0.078 sec 5.0
0.163333
588.726748
6
2022-02-22 12:39:50 0.078 sec 6.0
0.171500
588.159003
7
2022-02-22 12:39:50 0.078 sec 7.0
0.180075
244.333433
8
2022-02-22 12:39:50 0.078 sec 8.0
0.189079
113.292014
9
2022-02-22 12:39:50 0.078 sec 9.0
0.198533
48.716534
10
2022-02-22 12:39:50 0.078 sec 10.0
0.208459
27.685304
11
2022-02-22 12:39:50 0.078 sec 11.0
0.218882
20.933356
12
2022-02-22 12:39:50 0.078 sec 12.0
0.229826
19.800430
13
2022-02-22 12:39:50 0.084 sec 13.0
0.241318
19.242464
14
2022-02-22 12:39:50 0.084 sec 14.0
0.253384
18.975207
15
2022-02-22 12:39:50 0.084 sec 15.0
0.168922
18.975207
16
2022-02-22 12:39:50 0.084 sec 16.0
0.177369
18.671021
17
2022-02-22 12:39:50 0.084 sec 17.0
0.186237
18.653417
18
2022-02-22 12:39:50 0.084 sec 18.0
0.124158
18.653417
19
2022-02-22 12:39:50 0.084 sec 19.0
0.130366
17.888250
See the whole table with table.as_data_frame()
Out[13]:
Save the model in MOJO format.¶
In [14]:
# Saving H2O Model to a file.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = pca_model.save_mojo(path=f"{temp_dir.name}", force=True)
Save the model in Vantage.¶
In [15]:
# Save the H2O Model in Vantage.
save_byom(model_id="h2o_pca_iris", model_file=model_file_path, table_name="byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [16]:
# List the models from "byom_models".
list_byom("byom_models")
                                 model
model_id                              
h2o_pca_iris  b'504B03041400080808...'
Retrieve the model from Vantage.¶
In [17]:
# Retrieve the model from vantage using the model name 'h2o_pca_iris'.
model=retrieve_byom(model_id="h2o_pca_iris", table_name="byom_models")
In [18]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [19]:
# Score the model on 'iris_test' data.
result = H2OPredict(newdata=iris_test,
                    newdata_partition_column='id',
                    newdata_order_column='id',
                    modeldata=model,
                    modeldata_order_column='model_id',
                    accumulate=['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models='*',
                    model_type='OpenSource'
                   )
In [20]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".H2OPredict(
ON "MLDB"."ml__select__1645515297986767" AS InputTable
PARTITION BY "id"
ORDER BY "id" 
ON (select model_id,model from "MLDB"."ml__filter__1645516153481333") AS ModelTable
DIMENSION
ORDER BY "model_id"
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [21]:
# Print the result.
result.result
Out[21]:
id sepal_length petal_length prediction
json_report
52 6.4
4.5
{"dimensions":[-0.1441832230496968,-0.9194340730124243,-0.615501545531632,0.668914840466421]}
18 5.1
1.4
{"dimensions":[-1.3167810354416312,1.554559149811599,-0.29399982340185005,0.22075453102419862]}
24 5.1
1.7
{"dimensions":[-1.1831048786134577,1.3498913361132854,-0.3301298202908464,-0.1546627605928083]}
28 5.2
1.5
{"dimensions":[-1.3127184928032658,1.5368862516091986,-0.2880898531286974,0.28988895260585745]}
63 6.0
4.0
{"dimensions":[-0.4296527482753505,-1.2193116326341704,-0.4195427606020329,-1.4738343592564334]}
23 4.6
1.0
{"dimensions":[-1.5525177295466552,1.826668060039769,-0.23485386079713957,0.08217345112567695]}
36 5.0
1.2
{"dimensions":[-1.3859266051223962,1.454377300716858,-0.242108780864958,-0.4118185383859379]}
59 6.6
4.6
{"dimensions":[-0.12708038384252834,-1.118665879271585,-0.5793035674628677,0.23845396949628075]}
51 7.0
4.7
{"dimensions":[0.006838305319815413,-1.0866813881782158,-0.6429165658028986,1.0606461578918103]}
2
4.9
1.4
{"dimensions":[-1.3674041623918451,1.3364292790567744,-0.23401428509572839,-0.8484336521271371]}
Cleanup.¶
In [22]:
# Delete the saved Model from the table byom_models, using the model id h2o_pca_iris.
delete_byom("h2o_pca_iris", table_name="byom_models")
Model is deleted.
In [23]:
# Drop model table.
db_drop_table("byom_models")
Out[23]:
True
In [24]:
# Drop input data table.
db_drop_table("iris_input")
Out[24]:
True
In [25]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[25]:
True
XGBoost
H2OPredict() using XGBoost model.¶
Setup¶
In [1]:
import tempfile
import getpass
import teradataml as td
from teradataml import create_context, remove_context, load_example_data, DataFrame,\
db_drop_table, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
from teradataml.analytics.byom.H2OPredict import H2OPredict
import h2o
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [4]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[4]:
id
sepal_length sepal_width petal_length petal_width species
118 7.7
3.8
6.7
2.2
3
99
5.1
2.5
3.0
1.1
2
97
5.7
2.9
4.2
1.3
2
38
4.9
3.6
1.4
0.1
1
76
6.6
3.0
4.4
1.4
2
101 6.3
3.3
6.0
2.5
3
141 6.7
3.1
5.6
2.4
3
17
5.4
3.9
1.3
0.4
1
78
6.7
3.0
5.0
1.7
2
59
6.6
2.9
4.6
1.3
2
In [5]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
127 6.2
2.8
4.8
1.8
3
36
5.0
3.2
1.2
0.2
1
114 5.7
2.5
5.0
2.0
3
38
4.9
3.6
1.4
0.1
1
108 7.3
2.9
6.3
1.8
3
32
5.4
3.4
1.5
0.4
1
11
5.4
3.7
1.5
0.2
1
66
6.7
3.1
4.4
1.4
2
133 6.4
2.8
5.6
2.2
3
59
6.6
2.9
4.6
1.3
2
Prepare dataset for creating an XGBoost model.¶
In [6]:
h2o.init()
# Since H2OFrame accepts pandas DataFrame, converting teradataml DataFrame to pandas DataFrame.
iris_train_pd = iris_train.to_pandas()
h2o_df = h2o.H2OFrame(iris_train_pd)
h2o_df
Checking whether there is an H2O instance running at http://localhost:54321 . connected.
H2O_cluster_uptime:
10 mins 52 secs
H2O_cluster_timezone:
America/Los_Angeles
H2O_data_parsing_timezone: UTC
H2O_cluster_version:
3.32.1.6
H2O_cluster_version_age:
1 month and 21 days
H2O_cluster_name:
H2O_from_python_gp186005_ip5q0u
H2O_cluster_total_nodes:
1
H2O_cluster_free_memory:
3.998 Gb
H2O_cluster_total_cores:
12
H2O_cluster_allowed_cores:
12
H2O_cluster_status:
locked, healthy
H2O_connection_url:
http://localhost:54321
H2O_connection_proxy:
{"http": null, "https": null}
H2O_internal_security:
False
H2O_API_Extensions:
Amazon S3, XGBoost, Algos, AutoML, Core V3,
TargetEncoder, Core V4
Python_version:
3.7.3 ﬁnal
Parse progress: |█████████████████████████████████████████████████████████| 100%
sepal_length sepal_width petal_length petal_width species
5
2
3.5
1
2
6.3
3.3
6
2.5
3
5.1
3.4
1.5
0.2
1
5.7
3.8
1.7
0.3
1
4.9
3.6
1.4
0.1
1
6.7
3.1
5.6
2.4
3
5.7
2.6
3.5
1
2
5.1
2.5
3
1.1
2
6.7
3
5
1.7
2
5.4
3.9
1.3
0.4
1
Out[6]:
Train XGBoost Model.¶
In [7]:
# Import required libraries.
from h2o.estimators import H2OXGBoostEstimator
In [8]:
# Add the code for training model. 
h2o_df["species"] = h2o_df["species"].asfactor()
predictors = h2o_df.columns
response = "species"
In [9]:
iris_xgb = H2OXGBoostEstimator(booster='dart',
                               normalize_type="tree",
                               seed=1234)
In [10]:
iris_xgb.train(x=predictors, y=response, training_frame=h2o_df)
xgboost Model Build progress: |███████████████████████████████████████████| 100%
Save the model in MOJO format.¶
In [11]:
# Saving H2O Model to a file.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = iris_xgb.save_mojo(path=f"{temp_dir.name}", force=True)
Save the model in Vantage.¶
In [12]:
# Save the H2O Model in Vantage.
save_byom("h2o_xgb_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [13]:
# List the models from "byom_models".
list_byom("byom_models")
                                 model
model_id                              
h2o_xgb_iris  b'504B03041400080808...'
Retrieve the model from Vantage.¶
In [14]:
# Retrieve the model from vantage using the model name 'h2o_xgb_iris'.
modeldata = retrieve_byom("h2o_xgb_iris", table_name="byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [15]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [16]:
result = H2OPredict(newdata=iris_test,
                    newdata_partition_column='id',
                    newdata_order_column='id',
                    modeldata=modeldata,
                    modeldata_order_column='model_id',
                    model_output_fields=['label', 'classProbabilities'],
                    accumulate=['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models='*',
                    enable_options='stageProbabilities',
                    model_type='OpenSource'
                   )
In [17]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".H2OPredict(
ON "MLDB"."ml__select__16345201251589" AS InputTable
PARTITION BY "id"
ORDER BY "id" 
ON (select model_id,model from "MLDB"."ml__filter__16345179358491") AS ModelTable
DIMENSION
ORDER BY "model_id"
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('label','classProbabilities')
OverwriteCachedModel('*')
EnableOptions('stageProbabilities')
) as sqlmr
In [18]:
# Print the result.
result.result
Out[18]:
id sepal_length petal_length prediction label
classprobabilities
95 5.6
4.2
2
2
{"1": 0.002973866416141391,"2": 0.9956537485122681,"3": 0.0013723127776756883}
53 6.9
4.9
2
2
{"1": 0.0022306146565824747,"2": 0.9898779392242432,"3": 0.007891373708844185}
69 6.2
4.5
2
2
{"1": 0.0034840735606849194,"2": 0.9866116046905518,"3": 0.009904342703521252}
25 4.8
1.9
1
1
{"1": 0.9940190315246582,"2": 0.004053735174238682,"3": 0.0019271911587566137}
76 6.6
4.4
2
2
{"1": 0.0015327803557738662,"2": 0.9975995421409607,"3": 8.676660363562405E-4}
11 5.4
1.5
1
1
{"1": 0.9942521452903748,"2": 0.004054686054587364,"3": 0.001693196245469153}
28 5.2
1.5
1
1
{"1": 0.9942521452903748,"2": 0.004054686054587364,"3": 0.001693196245469153}
48 4.6
1.4
1
1
{"1": 0.9940190315246582,"2": 0.004053735174238682,"3": 0.0019271911587566137}
61 5.0
3.5
2
2
{"1": 0.017349962145090103,"2": 0.9489255547523499,"3": 0.03372446820139885}
21 5.4
1.7
1
1
{"1": 0.9942521452903748,"2": 0.004054686054587364,"3": 0.001693196245469153}
Cleanup.¶
In [19]:
# Delete the model from table "byom_models", using the model id 'h2o_xgb_iris'.
delete_byom("h2o_xgb_iris", "byom_models")
Model is deleted.
In [20]:
# Drop models table.
db_drop_table("byom_models")
Out[20]:
True
In [21]:
# Drop input data table.
db_drop_table("iris_input")
Out[21]:
True
In [22]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[22]:
True
In [ ]:
ONNXPredict
Decision Tree
ONNXPredict using Decision Tree model.¶
Setup¶
In [ ]:
# Import required libraries.
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [ ]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [ ]:
# Load example data.
load_example_data("byom", "iris_input")
In [ ]:
iris_input = DataFrame("iris_input")
In [ ]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species sampleid
59
6.6
2.9
4.6
1.3
2
1
80
5.7
2.6
3.5
1.0
2
2
120 6.0
2.2
5.0
1.5
3
2
101 6.3
3.3
6.0
2.5
3
1
17
5.4
3.9
1.3
0.4
1
2
61
5.0
2.0
3.5
1.0
2
1
38
4.9
3.6
1.4
0.1
1
1
78
6.7
3.0
5.0
1.7
2
1
141 6.7
3.1
5.6
2.4
3
1
40
5.1
3.4
1.5
0.2
1
1
In [ ]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
36
5.0
3.2
1.2
0.2
1
80
5.7
2.6
3.5
1.0
2
118 7.7
3.8
6.7
2.2
3
141 6.7
3.1
5.6
2.4
3
139 6.0
3.0
4.8
1.8
3
61
5.0
2.0
3.5
1.0
2
38
4.9
3.6
1.4
0.1
1
78
6.7
3.0
5.0
1.7
2
17
5.4
3.9
1.3
0.4
1
40
5.1
3.4
1.5
0.2
1
In [ ]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
146 6.7
3.0
5.2
2.3
3
17
5.4
3.9
1.3
0.4
1
51
7.0
3.2
4.7
1.4
2
38
4.9
3.6
1.4
0.1
1
131 7.4
2.8
6.1
1.9
3
118 7.7
3.8
6.7
2.2
3
127 6.2
2.8
4.8
1.8
3
62
5.9
3.0
4.2
1.5
2
76
6.6
3.0
4.4
1.4
2
101 6.3
3.3
6.0
2.5
3
Prepare dataset for creating a Decision Tree model.¶
In [ ]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
Train Model.¶
In [ ]:
# Import required libraries.
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.tree import DecisionTreeClassifier
from sklearn.preprocessing import StandardScaler
In [ ]:
# Generate the Decision Tree model.
dt_pipe_obj = Pipeline([
    ('scaler', StandardScaler()),
    ("dt", DecisionTreeClassifier(max_depth=5))
])
In [ ]:
dt_pipe_obj.fit(train_pd[features], train_pd[target])
Out[ ]:
Save the model in ONNX format.¶
In [ ]:
# Import required libraries.
from skl2onnx import to_onnx
In [ ]:
# Create temporary filepath to save model.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_dt_model.onnx"
In [ ]:
onx = to_onnx(dt_pipe_obj, train_pd.iloc[:,:4].astype(np.float32))
In [ ]:
with open(model_file_path, "wb") as f:
▸
Pipeline
▸StandardScaler
▸DecisionTreeClassifier
    f.write(onx.SerializeToString())
Save the model in Vantage.¶
In [ ]:
# Save the ONNX model in Vantage.
save_byom("onnx_dt_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [ ]:
# List the ONNX models in Vantage.
list_byom("byom_models")
                                model
model_id                             
onnx_dt_iris  b'8081208736B6C326F...'
Retrieve the model from Vantage.¶
In [ ]:
# Retrieve the model from table "byom_models", using the model id 'onnx_dt_iris'.
modeldata = retrieve_byom("onnx_dt_iris", "byom_models")
In [ ]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [ ]:
# Import required libraries
from teradataml import ONNXPredict
In [ ]:
# Perform prediction using ONNXPredict() and the ONNX model stored in Vantage.
predict_output = ONNXPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    model_output_fields = "output_label"
                    )
In [ ]:
# Print the query.
print(predict_output.show_query())
SELECT * FROM "mldb".ONNXPredict(
ON "MLDB"."ml__select__1666875028757503" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1666872325683112") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('output_label')
OverwriteCachedModel('*')
) as sqlmr
In [ ]:
# Print the result.
predict_output.result
Out[ ]:
id
sepal_length petal_length output_label
125 6.7
5.7
[3]
95
5.6
4.2
[2]
110 7.2
6.1
[3]
49
5.3
1.5
[1]
123 7.7
6.7
[3]
61
5.0
3.5
[2]
116 6.4
5.3
[3]
24
5.1
1.7
[1]
104 6.3
5.6
[3]
80
5.7
3.5
[2]
Cleanup.¶
In [ ]:
# Drop input data tables. 
# Delete the model from table "byom_models", using the model id 'onnx_dt_iris'.
delete_byom("onnx_dt_iris", "byom_models")
Model is deleted.
In [ ]:
db_drop_table("byom_models")
Out[ ]:
True
In [ ]:
db_drop_table("iris_input")
Out[ ]:
True
In [ ]:
remove_context()
Out[ ]:
True
GLM
ONNXPredict using GLM model.¶
Setup¶
In [ ]:
# Import required libraries.
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [ ]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [ ]:
# Load example data.
load_example_data("byom", "iris_input")
In [ ]:
iris_input = DataFrame("iris_input")
In [ ]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species sampleid
78
6.7
3.0
5.0
1.7
2
2
141 6.7
3.1
5.6
2.4
3
1
17
5.4
3.9
1.3
0.4
1
1
40
5.1
3.4
1.5
0.2
1
2
120 6.0
2.2
5.0
1.5
3
1
122 5.6
2.8
4.9
2.0
3
1
19
5.7
3.8
1.7
0.3
1
1
59
6.6
2.9
4.6
1.3
2
1
80
5.7
2.6
3.5
1.0
2
1
101 6.3
3.3
6.0
2.5
3
1
In [ ]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
76
6.6
3.0
4.4
1.4
2
141 6.7
3.1
5.6
2.4
3
17
5.4
3.9
1.3
0.4
1
40
5.1
3.4
1.5
0.2
1
120 6.0
2.2
5.0
1.5
3
19
5.7
3.8
1.7
0.3
1
99
5.1
2.5
3.0
1.1
2
36
5.0
3.2
1.2
0.2
1
80
5.7
2.6
3.5
1.0
2
101 6.3
3.3
6.0
2.5
3
In [ ]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
1
5.1
3.5
1.4
0.2
1
89
5.6
3.0
4.1
1.3
2
106 7.6
3.0
6.6
2.1
3
57
6.3
3.3
4.7
1.6
2
135 6.1
2.6
5.6
1.4
3
59
6.6
2.9
4.6
1.3
2
99
5.1
2.5
3.0
1.1
2
36
5.0
3.2
1.2
0.2
1
118 7.7
3.8
6.7
2.2
3
13
4.8
3.0
1.4
0.1
1
Prepare dataset for creating a GLM model.¶
In [ ]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
Train Model.¶
In [ ]:
# Import required libraries.
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler
In [ ]:
# Generate the GLM model.
glm_pipe_obj = Pipeline([
    ('scaler', StandardScaler()),
    ("glm", LogisticRegression(random_state=0, solver="newton-cg", C=2.0))
])
In [ ]:
glm_pipe_obj.fit(train_pd[features], train_pd[target])
Out[ ]:
Save the model in ONNX format.¶
In [ ]:
▸
Pipeline
▸StandardScaler
▸LogisticRegression
# Import required libraries.
from skl2onnx import to_onnx
In [ ]:
# Create temporary filepath to save model.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_glm_model.onnx"
In [ ]:
onx = to_onnx(glm_pipe_obj, train_pd.iloc[:,:4].astype(np.float32))
In [ ]:
with open(model_file_path, "wb") as f:
    f.write(onx.SerializeToString())
Save the model in Vantage.¶
In [ ]:
# Save the ONNX model in Vantage.
save_byom("onnx_glm_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [ ]:
# List the ONNX models in Vantage.
list_byom("byom_models")
                                 model
model_id                              
onnx_glm_iris  b'8081208736B6C326F...'
Retrieve the model from Vantage.¶
In [ ]:
# Retrieve the model from table "byom_models", using the model id 'onnx_glm_iris'.
modeldata = retrieve_byom("onnx_glm_iris", "byom_models")
In [ ]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [ ]:
# Import required libraries
from teradataml import ONNXPredict
In [ ]:
# Perform prediction using ONNXPredict() and the ONNX model stored in Vantage.
predict_output = ONNXPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    model_output_fields = "output_label"
                    )
In [ ]:
# Print the query.
print(predict_output.show_query())
SELECT * FROM "mldb".ONNXPredict(
ON "MLDB"."ml__select__1666872686152360" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1666872276184695") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('output_label')
OverwriteCachedModel('*')
) as sqlmr
In [ ]:
# Print the result.
predict_output.result
Out[ ]:
id
sepal_length petal_length output_label
18
5.1
1.4
[1]
17
5.4
1.3
[1]
34
5.5
1.4
[1]
80
5.7
3.5
[2]
35
4.9
1.5
[1]
99
5.1
3.0
[2]
97
5.7
4.2
[2]
125 6.7
5.7
[3]
112 6.4
5.3
[3]
141 6.7
5.6
[3]
Cleanup.¶
In [ ]:
# Drop input data tables. 
# Delete the model from table "byom_models", using the model id 'onnx_glm_iris'.
delete_byom("onnx_glm_iris", "byom_models")
Model is deleted.
In [ ]:
db_drop_table("byom_models")
Out[ ]:
True
In [ ]:
db_drop_table("iris_input")
Out[ ]:
True
In [ ]:
remove_context()
Out[ ]:
True
KMeans
ONNXPredict using KMeans model.¶
Setup¶
In [ ]:
# Import required libraries.
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [ ]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [ ]:
# Load example data.
load_example_data("kmeans", "computers_train1")
In [ ]:
computer_input = DataFrame("computers_train1")
computer_input
Out[ ]:
id
price speed
hd
ram screen
4894 1739.0 33.0
212.0
4.0
17.0
3487 1999.0 33.0
420.0
8.0
15.0
3956 2095.0 100.0 214.0
4.0
14.0
4425 2493.0 100.0 528.0
8.0
14.0
2345 1795.0 33.0
420.0
8.0
15.0
469
2599.0 50.0
405.0
8.0
14.0
5832 1490.0 66.0
340.0
4.0
15.0
4690 3440.0 33.0
1000.0 24.0 15.0
1876 2364.0 33.0
212.0
4.0
14.0
6036 1445.0 100.0 528.0
4.0
14.0
In [ ]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
computer_sample = computer_input.sample(frac=[0.8, 0.2])
computer_sample
Out[ ]:
id
price speed
hd
ram screen sampleid
4894 1739.0 33.0
212.0
4.0
17.0
1
3487 1999.0 33.0
420.0
8.0
15.0
1
3956 2095.0 100.0 214.0
4.0
14.0
1
4425 2493.0 100.0 528.0
8.0
14.0
2
2345 1795.0 33.0
420.0
8.0
15.0
1
469
2599.0 50.0
405.0
8.0
14.0
1
5832 1490.0 66.0
340.0
4.0
15.0
1
4690 3440.0 33.0
1000.0 24.0 15.0
1
1876 2364.0 33.0
212.0
4.0
14.0
1
6036 1445.0 100.0 528.0
4.0
14.0
1
In [ ]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
computer_train = computer_sample[computer_sample.sampleid == "1"].drop("sampleid", axis = 1)
computer_train
Out[ ]:
id
price speed
hd
ram screen
4894 1739.0 33.0
212.0
4.0
17.0
3487 1999.0 33.0
420.0
8.0
15.0
3956 2095.0 100.0 214.0
4.0
14.0
4425 2493.0 100.0 528.0
8.0
14.0
265
1899.0 50.0
120.0
4.0
14.0
469
2599.0 50.0
405.0
8.0
14.0
5832 1490.0 66.0
340.0
4.0
15.0
4690 3440.0 33.0
1000.0 24.0 15.0
2345 1795.0 33.0
420.0
8.0
15.0
6036 1445.0 100.0 528.0
4.0
14.0
In [ ]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
computer_test = computer_sample[computer_sample.sampleid == "2"].drop("sampleid", axis = 1)
computer_test
Out[ ]:
id
price speed
hd
ram screen
5525 1749.0 66.0
425.0
8.0
15.0
3670 1929.0 25.0
120.0
4.0
17.0
1039 3495.0 50.0
452.0
8.0
14.0
6097 1649.0 66.0
540.0
8.0
14.0
4751 1299.0 33.0
340.0
4.0
14.0
4690 3440.0 33.0
1000.0 24.0 15.0
2875 2799.0 50.0
340.0
8.0
15.0
6158 1923.0 100.0 1000.0 16.0 15.0
1468 2894.0 33.0
340.0
4.0
14.0
1407 2644.0 66.0
426.0
8.0
14.0
Prepare dataset for creating a KMeans model.¶
In [ ]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = computer_train.to_pandas()
features = train_pd.columns 
Train Model.¶
In [ ]:
# Import required libraries.
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
In [ ]:
# Generate the KMeans model.
k_means_pipe_obj = Pipeline([
    ('scaler', StandardScaler()),
    ("kmeans", KMeans(n_clusters=7, n_init=97))
])
In [ ]:
k_means_pipe_obj.fit(train_pd[features])
Out[ ]:
Save the model in ONNX format.¶
In [ ]:
# Import required libraries.
from skl2onnx import to_onnx
In [ ]:
# Create temporary filepath to save model.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/computer_db_k_means_model.onnx"
In [ ]:
onx = to_onnx(k_means_pipe_obj, train_pd.iloc[:,:5].astype(np.float32))
In [ ]:
with open(model_file_path, "wb") as f:
    f.write(onx.SerializeToString())
Save the model in Vantage.¶
In [ ]:
# Save the ONNX model in Vantage.
save_byom("onnx_k_means_computer", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [ ]:
# List the ONNX models in Vantage.
list_byom("byom_models")
                                         model
model_id                                      
onnx_k_means_computer  b'8081208736B6C326F...'
Retrieve the model from Vantage.¶
In [ ]:
# Retrieve the model from table "byom_models", using the model id 'onnx_k_means_computer'.
modeldata = retrieve_byom("onnx_k_means_computer", "byom_models")
In [ ]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [ ]:
# Import required libraries
from teradataml import ONNXPredict
In [ ]:
# Perform prediction using ONNXPredict() and the ONNX model stored in Vantage.
predict_output = ONNXPredict(
                    modeldata = modeldata,
                    newdata = computer_test,
▸
Pipeline
▸StandardScaler
▸KMeans
                    accumulate = ['id', 'speed', 'screen'],
                    overwrite_cached_models = '*',
                    model_output_fields = "label"
                    )
In [ ]:
# Print the query.
print(predict_output.show_query())
SELECT * FROM "mldb".ONNXPredict(
ON "MLDB"."ml__select__1666875823940419" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1666868989452469") AS ModelTable
DIMENSION
USING
Accumulate('id','speed','screen')
ModelOutputFields('label')
OverwriteCachedModel('*')
) as sqlmr
In [ ]:
# Print the result.
predict_output.result
Out[ ]:
id
speed screen label
3201 66.0
14.0
[5]
265
50.0
14.0
[1]
3283 33.0
14.0
[1]
1672 50.0
14.0
[1]
1264 66.0
14.0
[5]
1203 50.0
15.0
[6]
1937 33.0
14.0
[6]
6219 66.0
15.0
[4]
5893 100.0 15.0
[5]
2345 33.0
15.0
[6]
Cleanup.¶
In [ ]:
# Drop input data tables. 
# Delete the model from table "byom_models", using the model id 'onnx_k_means_computer'.
delete_byom("onnx_k_means_computer", "byom_models")
Model is deleted.
In [ ]:
db_drop_table("byom_models")
Out[ ]:
True
In [ ]:
db_drop_table("computers_train1")
Out[ ]:
True
In [ ]:
remove_context()
Out[ ]:
True
K-Nearest Neighbors (KNN)
ONNXPredict using KNN model.¶
Setup¶
In [ ]:
# Import required libraries.
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [ ]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [ ]:
# Load example data.
load_example_data("byom", "iris_input")
In [ ]:
iris_input = DataFrame("iris_input")
In [ ]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species sampleid
59
6.6
2.9
4.6
1.3
2
1
80
5.7
2.6
3.5
1.0
2
2
120 6.0
2.2
5.0
1.5
3
1
101 6.3
3.3
6.0
2.5
3
1
17
5.4
3.9
1.3
0.4
1
1
61
5.0
2.0
3.5
1.0
2
2
38
4.9
3.6
1.4
0.1
1
2
78
6.7
3.0
5.0
1.7
2
2
141 6.7
3.1
5.6
2.4
3
2
40
5.1
3.4
1.5
0.2
1
2
In [ ]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
59
6.6
2.9
4.6
1.3
2
80
5.7
2.6
3.5
1.0
2
120 6.0
2.2
5.0
1.5
3
101 6.3
3.3
6.0
2.5
3
17
5.4
3.9
1.3
0.4
1
38
4.9
3.6
1.4
0.1
1
76
6.6
3.0
4.4
1.4
2
116 6.4
3.2
5.3
2.3
3
141 6.7
3.1
5.6
2.4
3
40
5.1
3.4
1.5
0.2
1
In [ ]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
47
5.1
3.8
1.6
0.2
1
150 5.9
3.0
5.1
1.8
3
94
5.0
2.3
3.3
1.0
2
17
5.4
3.9
1.3
0.4
1
104 6.3
2.9
5.6
1.8
3
131 7.4
2.8
6.1
1.9
3
58
4.9
2.4
3.3
1.0
2
98
6.2
2.9
4.3
1.3
2
34
5.5
4.2
1.4
0.2
1
118 7.7
3.8
6.7
2.2
3
Prepare dataset for creating a KNN model.¶
In [ ]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
Train Model.¶
In [ ]:
# Import required libraries.
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.neighbors import KNeighborsClassifier
from sklearn.compose import ColumnTransformer
In [ ]:
# A column transformer must be used to be able to name the onnx inputs
# even if we are not actually transforming the columns. Use "passthrough"
column_trans_obj = ColumnTransformer(transformers=[], remainder="passthrough")
In [ ]:
# Generate the KNN model.
knn_pipe_obj = Pipeline(steps=[("column_transform", column_trans_obj),
                            ("knn_classifier", KNeighborsClassifier())
                            ])
In [ ]:
# Train the KNN model.
knn_pipe_obj.fit(train_pd[features], train_pd[target])
Out[ ]:
Save the model in ONNX format.¶
In [ ]:
# Import required libraries.
from skl2onnx import to_onnx
In [ ]:
# Create temporary filepath to save model.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_knn_model.onnx"
In [ ]:
onx = to_onnx(knn_pipe_obj, train_pd.iloc[:,:4].astype(np.float32), target_opset=15)
In [ ]:
with open(model_file_path, "wb") as f:
    f.write(onx.SerializeToString())
Save the model in Vantage.¶
In [ ]:
# Save the ONNX model in Vantage.
save_byom("onnx_knn_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [ ]:
# List the ONNX models in Vantage.
list_byom("byom_models")
                                 model
model_id                              
onnx_knn_iris  b'8081208736B6C326F...'
▸
Pipeline
▸column_transform: ColumnTransformer
▸
remainder
▸passthrough
▸KNeighborsClassifier
Retrieve the model from Vantage.¶
In [ ]:
# Retrieve the model from table "byom_models", using the model id 'onnx_knn_iris'.
modeldata = retrieve_byom("onnx_knn_iris", "byom_models")
In [ ]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [ ]:
# Import required libraries
from teradataml import ONNXPredict
In [ ]:
# Perform prediction using ONNXPredict() and the ONNX model stored in Vantage.
predict_output = ONNXPredict(
                    modeldata = modeldata,
                    newdata = iris_test.drop("species", axis=1),
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    model_output_fields = "output_label"
                    )
In [ ]:
# Print the query.
print(predict_output.show_query())
SELECT * FROM "mldb".ONNXPredict(
ON (select id,sepal_length,sepal_width,petal_length,petal_width from "MLDB"."ml__select__1664456905496109") AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1664456801770958") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('output_label')
OverwriteCachedModel('*')
) as sqlmr
In [ ]:
# Print the result.
predict_output.result
Out[ ]:
id
sepal_length petal_length output_label
22
5.1
1.5
[1]
15
5.8
1.2
[1]
146 6.7
5.2
[3]
26
5.0
1.6
[1]
24
5.1
1.7
[1]
13
4.8
1.4
[1]
51
7.0
4.7
[2]
87
6.7
4.7
[2]
5
5.0
1.4
[1]
59
6.6
4.6
[2]
Cleanup.¶
In [ ]:
# Drop input data tables. 
# Delete the model from table "byom_models", using the model id 'onnx_knn_iris'.
delete_byom("onnx_knn_iris", "byom_models")
Model is deleted.
In [ ]:
db_drop_table("byom_models")
Out[ ]:
True
In [ ]:
db_drop_table("iris_input")
Out[ ]:
True
In [ ]:
remove_context()
Out[ ]:
True
Linear Regression
ONNXPredict using Linear Regression model.¶
Setup¶
In [ ]:
# Import required libraries.
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [ ]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data¶
In [ ]:
# Load the example data.
load_example_data("dataframe", "insurance")
In [ ]:
# Create teradataml DataFrames.
insurance_input = DataFrame("insurance")
insurance_input
Out[ ]:
age
sex
bmi
children smoker
region
charges
19
female 28.6
5
no
southwest 4687.797
40
male
26.315 1
no
northwest 6389.37785
40
female 36.19
0
no
southeast 5920.1041
34
female 31.92
1
yes
northeast
37701.8768
34
female 27.5
1
no
southwest 5003.853
61
female 39.1
2
no
southwest 14235.072
61
female 29.92
3
yes
southeast 30942.1918
61
female 22.04
0
no
northeast
13616.3586
34
female 37.335 2
no
northwest 5989.52365
40
female 28.69
3
no
northwest 8059.6791
One Hot Encoder¶
In [ ]:
# Import required library and configure val install location.
from teradataml import  OneHotEncoder, valib, Retain, FillNa
configure.val_install_location = getpass.getpass("val_install_location: ")
In [ ]:
# Perform One Hot Encoding on the categorical features.   
dc1 = OneHotEncoder(values=["southwest", "southeast", "northwest", "northeast"], columns="region")
dc2 = OneHotEncoder(style="contrast", values="yes", reference_value=0, columns="smoker")
dc3 = OneHotEncoder(style="contrast", values="male", reference_value=0, columns="sex")
In [ ]:
# Perform FillNa on the children column.
fn1 = FillNa(style="median_wo_mean", columns="children", out_columns="mod_children")
In [ ]:
# Retaining unaltered columns.
retain = Retain(columns=["charges", "bmi"])
In [ ]:
# Execute Transform() function.
obj = valib.Transform(data=insurance_input, one_hot_encode=[dc1,dc2,dc3], fillna=fn1, key_columns="ID", retain=retain)
insurance_input = obj.result
insurance_input
Out[ ]:
age
charges
bmi
southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex mod_children
61
12950.0712
38.38
0
0
1
0
0
1
0
61
13041.921
28.2
1
0
0
0
0
0
0
61
30942.1918
29.92
0
1
0
0
1
0
3
61
24513.09126 25.08
0
1
0
0
0
0
0
61
13143.86485 33.915 0
0
0
1
0
1
0
61
27941.28758 36.1
1
0
0
0
0
1
3
61
46599.1084
35.86
0
1
0
0
1
1
0
61
13415.0381
21.09
0
0
1
0
0
0
0
61
14119.62
32.3
0
0
1
0
0
1
2
61
13635.6379
35.91
0
0
0
1
0
0
0
use sample() for splitting input data into testing and training dataset.¶
In [ ]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
insurance_sample = insurance_input.sample(frac=[0.8, 0.2])
insurance_sample
Out[ ]:
age
charges
bmi southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex mod_children sampleid
61
12950.0712
38.38 0
0
1
0
0
1
0
1
34
5385.3379
26.41 0
0
1
0
0
0
1
1
34
3935.1799
34.21 0
1
0
0
0
1
0
1
40
7196.867
35.3
1
0
0
0
0
1
3
1
40
22331.5668
28.12 0
0
0
1
1
0
1
2
19
13844.506
21.7
1
0
0
0
1
0
0
1
19
1639.5631
30.59 0
0
1
0
0
1
0
1
19
1743.214
28.9
1
0
0
0
0
0
0
1
40
17179.522
19.8
0
1
0
0
1
1
1
1
34
14358.36437 32.8
1
0
0
0
0
1
1
1
In [ ]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
insurance_train = insurance_sample[insurance_sample.sampleid == "1"].drop("sampleid", axis = 1)
insurance_train
Out[ ]:
age
charges
bmi southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex mod_children
61
13415.0381
21.09 0
0
1
0
0
0
0
34
5385.3379
26.41 0
0
1
0
0
0
1
34
3935.1799
34.21 0
1
0
0
0
1
0
40
7196.867
35.3
1
0
0
0
0
1
3
40
22331.5668
28.12 0
0
0
1
1
0
1
19
13844.506
21.7
1
0
0
0
1
0
0
19
2130.6759
32.11 0
0
1
0
0
0
0
19
16884.924
27.9
1
0
0
0
1
0
0
40
17179.522
19.8
0
1
0
0
1
1
1
34
14358.36437 32.8
1
0
0
0
0
1
1
In [ ]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
insurance_test = insurance_sample[insurance_sample.sampleid == "2"].drop("sampleid", axis = 1)
insurance_test
Out[ ]:
age
charges
bmi
southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex mod_children
38
6373.55735
40.565 0
0
1
0
0
0
1
32
4673.3922
37.18
0
1
0
0
0
1
2
32
19719.6947
28.93
0
1
0
0
1
1
1
40
6600.361
29.9
1
0
0
0
0
1
2
40
6600.20595
34.105 0
0
0
1
0
1
1
19
1261.442
34.1
1
0
0
0
0
1
0
19
1263.249
35.4
1
0
0
0
0
1
0
19
36219.40545 36.955 0
0
1
0
1
1
0
40
7173.35995
22.705 0
0
0
1
0
1
2
34
43943.8761
30.21
0
0
1
0
1
0
1
Prepare dataset for creating a linear regression model.¶
In [ ]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = insurance_train.to_pandas()
features = train_pd.columns.drop('charges')
target = 'charges'
Train Model.¶
In [ ]:
# Import required libraries.
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LinearRegression
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import MinMaxScaler
In [ ]:
# Defining column tranformation pipeline.
col_trans_obj = ColumnTransformer([
                                  ("num_preprocess", MinMaxScaler(), ["bmi"])
                                  ])
In [ ]:
# Generate the Linear Regression model.
lin_reg_pipe_obj = Pipeline(steps=[
                            ('col_trans', col_trans_obj),
                            ("LRM", LinearRegression())
                            ])
In [ ]:
lin_reg_pipe_obj.fit(train_pd[features], train_pd[target])
Out[ ]:
Save the model in ONNX format.¶
In [ ]:
# Import required libraries.
from skl2onnx import to_onnx
In [ ]:
# Create temporary filepath to save model.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/insurance_db_lin_reg_model.onnx"
In [ ]:
onx = to_onnx(lin_reg_pipe_obj, train_pd.iloc[:, :10].astype(np.float32))
In [ ]:
with open(model_file_path, "wb") as f:
    f.write(onx.SerializeToString())
Save the model in Vantage.¶
In [ ]:
# Save the ONNX model in Vantage.
save_byom("onnx_lin_reg_insurance", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [ ]:
▸
Pipeline
▸col_trans: ColumnTransformer
▸num_preprocess
▸MinMaxScaler
▸LinearRegression
# List the ONNX models in Vantage.
list_byom("byom_models")
                                          model
model_id                                       
onnx_lin_reg_insurance  b'8081208736B6C326F...'
Retrieve the model from Vantage.¶
In [ ]:
# Retrieve the model from table "byom_models", using the model id 'onnx_lin_reg_insurance'.
modeldata = retrieve_byom("onnx_lin_reg_insurance", "byom_models")
In [ ]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [ ]:
# Import required libraries
from teradataml import ONNXPredict
In [ ]:
# Perform prediction using ONNXPredict() and the ONNX model stored in Vantage.
predict_output = ONNXPredict(
                            modeldata = modeldata,
                            newdata = insurance_test,
                            accumulate = ['male_sex', 'bmi', 'mod_children'],
                            overwrite_cached_models = '*',
                            show_model_input_fields_map=True,
                            model_output_fields="variable"
                            )
In [ ]:
# Print the query.
print(predict_output.show_query())
SELECT * FROM "mldb".ONNXPredict(
ON "VAL"."ml__select__1666875162231665" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "VAL"."ml__filter__1666873236968949") AS ModelTable
DIMENSION
USING
Accumulate('male_sex','bmi','mod_children')
ModelOutputFields('variable')
OverwriteCachedModel('*')
ShowModelInputFieldsMap('True')
) as sqlmr
In [ ]:
# Print the result.
predict_output.result
Out[ ]:
ModelInputFieldsMap
ModelInputFieldsMap('male_sex=male_sex','mod_children=mod_children','charges=charges','southeast_region=southeast_region','yes_smoker=yes_smoker','northwest_region
ModelInputFieldsMap('male_sex=male_sex','mod_children=mod_children','charges=charges','southeast_region=southeast_region','yes_smoker=yes_smoker','northwest_region
ModelInputFieldsMap('male_sex=male_sex','mod_children=mod_children','charges=charges','southeast_region=southeast_region','yes_smoker=yes_smoker','northwest_region
ModelInputFieldsMap('male_sex=male_sex','mod_children=mod_children','charges=charges','southeast_region=southeast_region','yes_smoker=yes_smoker','northwest_region
Cleanup.¶
In [ ]:
# Drop input data tables. 
# Delete the model from table "byom_models", using the model id 'onnx_lin_reg_insurance'.
delete_byom("onnx_lin_reg_insurance", "byom_models")
Model is deleted.
In [ ]:
db_drop_table("byom_models")
Out[ ]:
True
In [ ]:
db_drop_table("insurance")
Out[ ]:
True
In [ ]:
remove_context()
Out[ ]:
True
Logistic Regression
ONNXPredict using Logistic Regression model.¶
Setup¶
In [ ]:
# Import required libraries.
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [ ]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [ ]:
# Load example data.
load_example_data("byom", "iris_input")
In [ ]:
iris_input = DataFrame("iris_input")
In [ ]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species sampleid
17
5.4
3.9
1.3
0.4
1
1
38
4.9
3.6
1.4
0.1
1
1
78
6.7
3.0
5.0
1.7
2
1
122 5.6
2.8
4.9
2.0
3
2
59
6.6
2.9
4.6
1.3
2
1
40
5.1
3.4
1.5
0.2
1
1
80
5.7
2.6
3.5
1.0
2
1
120 6.0
2.2
5.0
1.5
3
1
19
5.7
3.8
1.7
0.3
1
1
61
5.0
2.0
3.5
1.0
2
2
In [ ]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
17
5.4
3.9
1.3
0.4
1
76
6.6
3.0
4.4
1.4
2
116 6.4
3.2
5.3
2.3
3
19
5.7
3.8
1.7
0.3
1
99
5.1
2.5
3.0
1.1
2
40
5.1
3.4
1.5
0.2
1
80
5.7
2.6
3.5
1.0
2
120 6.0
2.2
5.0
1.5
3
59
6.6
2.9
4.6
1.3
2
78
6.7
3.0
5.0
1.7
2
In [ ]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
119 7.7
2.6
6.9
2.3
3
108 7.3
2.9
6.3
1.8
3
24
5.1
3.3
1.7
0.5
1
59
6.6
2.9
4.6
1.3
2
91
5.5
2.6
4.4
1.2
2
120 6.0
2.2
5.0
1.5
3
57
6.3
3.3
4.7
1.6
2
55
6.5
2.8
4.6
1.5
2
97
5.7
2.9
4.2
1.3
2
78
6.7
3.0
5.0
1.7
2
Prepare dataset for creating a Logistic Regression model.¶
In [ ]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
Train Model.¶
In [ ]:
# Import required libraries.
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler
In [ ]:
# Generate the Logistic Regression model.
log_reg_pipe_obj = Pipeline([
    ('scaler', StandardScaler()),
    ("LogReg", LogisticRegression())
])
In [ ]:
log_reg_pipe_obj.fit(train_pd[features], train_pd[target])
Out[ ]:
Save the model in ONNX format.¶
In [ ]:
# Import required libraries.
from skl2onnx import to_onnx
In [ ]:
# Create temporary filepath to save model.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_log_reg_model.onnx"
In [ ]:
onx = to_onnx(log_reg_pipe_obj, train_pd.iloc[:,:4].astype(np.float32))
In [ ]:
with open(model_file_path, "wb") as f:
    f.write(onx.SerializeToString())
Save the model in Vantage.¶
In [ ]:
▸
Pipeline
▸StandardScaler
▸LogisticRegression
# Save the ONNX model in Vantage.
save_byom("onnx_log_reg_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [ ]:
# List the ONNX models in Vantage.
list_byom("byom_models")
                                     model
model_id                                  
onnx_log_reg_iris  b'8081208736B6C326F...'
Retrieve the model from Vantage.¶
In [ ]:
# Retrieve the model from table "byom_models", using the model id 'onnx_log_reg_iris'.
modeldata = retrieve_byom("onnx_log_reg_iris", "byom_models")
In [ ]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [ ]:
# Import required libraries
from teradataml import ONNXPredict
In [ ]:
# Perform prediction using ONNXPredict() and the ONNX model stored in Vantage.
predict_output = ONNXPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    model_output_fields = "output_label"
                    )
In [ ]:
# Print the query.
print(predict_output.show_query())
SELECT * FROM "mldb".ONNXPredict(
ON "MLDB"."ml__select__1663138010926557" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1663142281856859") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('output_label')
) as sqlmr
In [ ]:
# Print the result.
predict_output.result
Out[ ]:
id
sepal_length petal_length output_label
137 6.3
5.6
[2]
9
4.4
1.4
[1]
104 6.3
5.6
[2]
78
6.7
5.0
[2]
148 6.5
5.2
[2]
135 6.1
5.6
[2]
150 5.9
5.1
[2]
94
5.0
3.3
[1]
76
6.6
4.4
[1]
139 6.0
4.8
[2]
Cleanup.¶
In [ ]:
# Drop input data tables. 
# Delete the model from table "byom_models", using the model id 'onnx_log_reg_iris'.
delete_byom("onnx_log_reg_iris", "byom_models")
Model is deleted.
In [ ]:
db_drop_table("byom_models")
Out[ ]:
True
In [ ]:
db_drop_table("iris_input")
Out[ ]:
True
In [ ]:
remove_context()
Out[ ]:
True
Naive Bayes
ONNXPredict using Naive Bayes model.¶
Setup¶
In [ ]:
# Import required libraries.
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [ ]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [ ]:
# Load example data.
load_example_data("byom", "iris_input")
In [ ]:
iris_input = DataFrame("iris_input")
In [ ]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species sampleid
120 6.0
2.2
5.0
1.5
3
1
19
5.7
3.8
1.7
0.3
1
2
59
6.6
2.9
4.6
1.3
2
2
61
5.0
2.0
3.5
1.0
2
1
78
6.7
3.0
5.0
1.7
2
1
101 6.3
3.3
6.0
2.5
3
1
141 6.7
3.1
5.6
2.4
3
1
17
5.4
3.9
1.3
0.4
1
1
38
4.9
3.6
1.4
0.1
1
1
122 5.6
2.8
4.9
2.0
3
1
In [ ]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
120 6.0
2.2
5.0
1.5
3
99
5.1
2.5
3.0
1.1
2
36
5.0
3.2
1.2
0.2
1
61
5.0
2.0
3.5
1.0
2
78
6.7
3.0
5.0
1.7
2
101 6.3
3.3
6.0
2.5
3
17
5.4
3.9
1.3
0.4
1
139 6.0
3.0
4.8
1.8
3
38
4.9
3.6
1.4
0.1
1
19
5.7
3.8
1.7
0.3
1
In [ ]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
22
5.1
3.7
1.5
0.4
1
114 5.7
2.5
5.0
2.0
3
85
5.4
3.0
4.5
1.5
2
116 6.4
3.2
5.3
2.3
3
60
5.2
2.7
3.9
1.4
2
141 6.7
3.1
5.6
2.4
3
11
5.4
3.7
1.5
0.2
1
106 7.6
3.0
6.6
2.1
3
148 6.5
3.0
5.2
2.0
3
74
6.1
2.8
4.7
1.2
2
Prepare dataset for creating a Naive Bayes model.¶
In [ ]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
Train Model.¶
In [ ]:
# Import required libraries.
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.naive_bayes import GaussianNB
In [ ]:
# Generate the naive bayes model.
nb_pipe_obj = Pipeline([
    ('scaler', StandardScaler()),
    ("nb", GaussianNB())
])
In [ ]:
nb_pipe_obj.fit(train_pd[features], train_pd[target])
Out[ ]:
Save the model in ONNX format.¶
In [ ]:
# Import required libraries.
from skl2onnx import to_onnx
In [ ]:
# Create temporary filepath to save model.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_nb_model.onnx"
▸
Pipeline
▸StandardScaler
▸GaussianNB
In [ ]:
# Convert Naive Bayes model to ONNX.
onx = to_onnx(nb_pipe_obj, train_pd.iloc[:,:4].astype(np.float32),
                            target_opset={'':12,  'ai.onnx.ml': 2})
In [ ]:
# Save the model in ONNX format.
with open(model_file_path, "wb") as f:
    f.write(onx.SerializeToString())
Save the model in Vantage.¶
In [ ]:
# Save the ONNX model in Vantage.
save_byom("onnx_nb_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [ ]:
# List the ONNX models in Vantage.
list_byom("byom_models")
                                model
model_id                             
onnx_nb_iris  b'8071208736B6C326F...'
Retrieve the model from Vantage.¶
In [ ]:
# Retrieve the model from table "byom_models", using the model id 'onnx_nb_iris'.
modeldata = retrieve_byom("onnx_nb_iris", "byom_models")
In [ ]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [ ]:
# Import required libraries
from teradataml import ONNXPredict
In [ ]:
# Perform prediction using ONNXPredict() and the ONNX model stored in Vantage.
predict_output = ONNXPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ["id","petal_length", "petal_width"],
                    overwrite_cached_models = '*',
                    model_output_fields = "output_label"
                    )
In [ ]:
# Print the query.
print(predict_output.show_query())
SELECT * FROM "mldb".ONNXPredict(
ON "MLDB"."ml__select__1666872685798551" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1666872923937459") AS ModelTable
DIMENSION
USING
Accumulate('id','petal_length','petal_width')
ModelOutputFields('output_label')
OverwriteCachedModel('*')
) as sqlmr
In [ ]:
# Print the predict_output.
predict_output.result
Out[ ]:
id
petal_length petal_width output_label
110 6.1
2.5
[3]
129 5.6
2.1
[3]
85
4.5
1.5
[2]
148 5.2
2.0
[3]
43
1.3
0.2
[1]
139 4.8
1.8
[3]
32
1.5
0.4
[1]
66
4.4
1.4
[2]
24
1.7
0.5
[1]
15
1.2
0.2
[1]
Cleanup.¶
In [ ]:
# Drop input data tables. 
# Delete the model from table "byom_models", using the model id 'onnx_nb_iris'.
delete_byom("onnx_nb_iris", "byom_models")
Model is deleted.
In [ ]:
db_drop_table("byom_models")
Out[ ]:
True
In [ ]:
db_drop_table("iris_input")
Out[ ]:
True
In [ ]:
remove_context()
Out[ ]:
True
Random Forest
ONNXPredict using Random Forest model.¶
Setup¶
In [ ]:
# Import required libraries.
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [ ]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [ ]:
# Load example data.
load_example_data("byom", "iris_input")
In [ ]:
iris_input = DataFrame("iris_input")
In [ ]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species sampleid
17
5.4
3.9
1.3
0.4
1
1
38
4.9
3.6
1.4
0.1
1
1
78
6.7
3.0
5.0
1.7
2
1
122 5.6
2.8
4.9
2.0
3
2
59
6.6
2.9
4.6
1.3
2
2
40
5.1
3.4
1.5
0.2
1
1
80
5.7
2.6
3.5
1.0
2
1
120 6.0
2.2
5.0
1.5
3
1
19
5.7
3.8
1.7
0.3
1
1
61
5.0
2.0
3.5
1.0
2
1
In [ ]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
17
5.4
3.9
1.3
0.4
1
78
6.7
3.0
5.0
1.7
2
76
6.6
3.0
4.4
1.4
2
122 5.6
2.8
4.9
2.0
3
59
6.6
2.9
4.6
1.3
2
80
5.7
2.6
3.5
1.0
2
120 6.0
2.2
5.0
1.5
3
118 7.7
3.8
6.7
2.2
3
19
5.7
3.8
1.7
0.3
1
61
5.0
2.0
3.5
1.0
2
In [ ]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
51
7.0
3.2
4.7
1.4
2
43
4.4
3.2
1.3
0.2
1
18
5.1
3.5
1.4
0.3
1
137 6.3
3.4
5.6
2.4
3
30
4.7
3.2
1.6
0.2
1
40
5.1
3.4
1.5
0.2
1
120 6.0
2.2
5.0
1.5
3
135 6.1
2.6
5.6
1.4
3
114 5.7
2.5
5.0
2.0
3
61
5.0
2.0
3.5
1.0
2
Prepare dataset for creating a Random Forest model.¶
In [ ]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
Train Model.¶
In [ ]:
# Import required libraries.
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import StandardScaler
In [ ]:
# Generate the Random Forest model.
rf_pipe_obj = Pipeline([
    ('scaler', StandardScaler()),
    ("rf", RandomForestClassifier(max_depth=5))
])
In [ ]:
rf_pipe_obj.fit(train_pd[features], train_pd[target])
Out[ ]:
Save the model in ONNX format.¶
In [ ]:
# Import required libraries.
from skl2onnx import to_onnx
In [ ]:
# Create temporary filepath to save model.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_rf_model.onnx"
In [ ]:
onx = to_onnx(rf_pipe_obj, train_pd.iloc[:,:4].astype(np.float32))
In [ ]:
with open(model_file_path, "wb") as f:
    f.write(onx.SerializeToString())
Save the model in Vantage.¶
In [ ]:
# Save the ONNX model in Vantage.
save_byom("onnx_rf_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [ ]:
# List the ONNX models in Vantage.
list_byom("byom_models")
                                model
model_id                             
onnx_rf_iris  b'8081208736B6C326F...'
Retrieve the model from Vantage.¶
In [ ]:
# Retrieve the model from table "byom_models", using the model id 'onnx_rf_iris'.
modeldata = retrieve_byom("onnx_rf_iris", "byom_models")
In [ ]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [ ]:
# Import required libraries
from teradataml import ONNXPredict
In [ ]:
# Perform prediction using ONNXPredict() and the ONNX model stored in Vantage.
predict_output = ONNXPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    model_output_fields = "output_label"
                    )
In [ ]:
# Print the query.
print(predict_output.show_query())
SELECT * FROM "mldb".ONNXPredict(
ON "MLDB"."ml__select__1666871043470589" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1666870566435686") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
ModelOutputFields('output_label')
OverwriteCachedModel('*')
) as sqlmr
▸
Pipeline
▸StandardScaler
▸RandomForestClassifier
In [ ]:
# Print the result.
predict_output.result
Out[ ]:
id
sepal_length petal_length output_label
22
5.1
1.5
[1]
129 6.4
5.6
[3]
45
5.1
1.9
[1]
61
5.0
3.5
[2]
78
6.7
5.0
[2]
11
5.4
1.5
[1]
9
4.4
1.4
[1]
28
5.2
1.5
[1]
38
4.9
1.4
[1]
137 6.3
5.6
[3]
Cleanup.¶
In [ ]:
# Drop input data tables. 
# Delete the model from table "byom_models", using the model id 'onnx_rf_iris'.
delete_byom("onnx_rf_iris", "byom_models")
Model is deleted.
In [ ]:
db_drop_table("byom_models")
Out[ ]:
True
In [ ]:
db_drop_table("iris_input")
Out[ ]:
True
In [ ]:
remove_context()
Out[ ]:
True
XGBoost
ONNXPredict using XGBoost Classiﬁer model.¶
Setup¶
In [ ]:
# Import required libraries.
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [ ]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [ ]:
# Load example data.
load_example_data("byom", "iris_input")
WARNING: Skipped loading table iris_input since it already exists in the database.
In [ ]:
iris_input = DataFrame("iris_input")
In [ ]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species sampleid
17
5.4
3.9
1.3
0.4
1
2
38
4.9
3.6
1.4
0.1
1
2
78
6.7
3.0
5.0
1.7
2
1
122 5.6
2.8
4.9
2.0
3
1
59
6.6
2.9
4.6
1.3
2
1
40
5.1
3.4
1.5
0.2
1
2
80
5.7
2.6
3.5
1.0
2
1
120 6.0
2.2
5.0
1.5
3
1
19
5.7
3.8
1.7
0.3
1
2
61
5.0
2.0
3.5
1.0
2
1
In [ ]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
139 6.0
3.0
4.8
1.8
3
38
4.9
3.6
1.4
0.1
1
78
6.7
3.0
5.0
1.7
2
122 5.6
2.8
4.9
2.0
3
99
5.1
2.5
3.0
1.1
2
40
5.1
3.4
1.5
0.2
1
120 6.0
2.2
5.0
1.5
3
55
6.5
2.8
4.6
1.5
2
59
6.6
2.9
4.6
1.3
2
61
5.0
2.0
3.5
1.0
2
In [ ]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[ ]:
id
sepal_length sepal_width petal_length petal_width species
83
5.8
2.7
3.9
1.2
2
24
5.1
3.3
1.7
0.5
1
1
5.1
3.5
1.4
0.2
1
59
6.6
2.9
4.6
1.3
2
85
5.4
3.0
4.5
1.5
2
118 7.7
3.8
6.7
2.2
3
55
6.5
2.8
4.6
1.5
2
95
5.6
2.7
4.2
1.3
2
97
5.7
2.9
4.2
1.3
2
26
5.0
3.0
1.6
0.2
1
Prepare dataset for creating a XGBoost Classiﬁer model.¶
In [ ]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
Train Model.¶
In [ ]:
# Import required libraries.
import numpy as np
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from xgboost import XGBClassifier
In [ ]:
# Generate the XGBoost Classifier model.
xgb_pipe_obj = Pipeline([
    ('scaler', StandardScaler()),
    ("xgbc", XGBClassifier(n_estimators=5))
])
In [ ]:
xgb_pipe_obj.fit(train_pd[features], train_pd[target])
Save the model in ONNX format.¶
In [ ]:
# Import required libraries.
from skl2onnx import to_onnx, convert_sklearn, update_registered_converter
from skl2onnx.common.shape_calculator import calculate_linear_classifier_output_shapes
from onnxmltools.convert.xgboost.operator_converters.XGBoost import convert_xgboost
In [ ]:
# Create temporary filepath to save model.
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_xgbc_model.onnx"
In [ ]:
# Update or register a new converter for the model pipeline
update_registered_converter(
    XGBClassifier, 'XGBoostXGBClassifier',
    calculate_linear_classifier_output_shapes, convert_xgboost,
    options={'nocl': [True, False], 'zipmap': [True]})
In [ ]:
# Convert XGBoost model to ONNX.
onx = to_onnx(xgb_pipe_obj, train_pd.iloc[:,:4].astype(np.float32),
                            target_opset={'':12,  'ai.onnx.ml': 2})
In [ ]:
# Save the model in ONNX format.
with open(model_file_path, "wb") as f:
    f.write(onx.SerializeToString())
Save the model in Vantage.¶
In [ ]:
# Save the ONNX model in Vantage.
save_byom("onnx_xgb_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
In [ ]:
# List the ONNX models in Vantage.
list_byom("byom_models")
                                 model
model_id                              
onnx_xgb_iris  b'8071208736B6C326F...'
Retrieve the model from Vantage.¶
In [ ]:
# Retrieve the model from table "byom_models", using the model id 'onnx_xgb_iris'.
modeldata = retrieve_byom("onnx_xgb_iris", "byom_models")
In [ ]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
Score the model.¶
In [ ]:
# Import required libraries
from teradataml import ONNXPredict
In [ ]:
# Perform prediction using ONNXPredict() and the ONNX model stored in Vantage.
predict_output = ONNXPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ["id","petal_length", "petal_width"],
                    overwrite_cached_models = '*',
                    model_output_fields = "output_label"
                    )
In [ ]:
# Print the query.
print(predict_output.show_query())
SELECT * FROM "mldb".ONNXPredict(
ON "MLDB"."ml__select__1666869336346812" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1666869978076131") AS ModelTable
DIMENSION
USING
Accumulate('id','petal_length','petal_width')
ModelOutputFields('output_label')
OverwriteCachedModel('*')
) as sqlmr
In [ ]:
# Print the predict_output.
predict_output.result
Out[ ]:
id
petal_length petal_width output_label
30
1.6
0.2
[1]
118 6.7
2.2
[3]
150 5.1
1.8
[3]
13
1.4
0.1
[1]
138 5.5
1.8
[3]
116 5.3
2.3
[3]
133 5.6
2.2
[3]
24
1.7
0.5
[1]
123 6.7
2.0
[3]
40
1.5
0.2
[1]
Cleanup.¶
In [ ]:
# Drop input data tables. 
# Delete the model from table "byom_models", using the model id 'onnx_xgb_iris'.
delete_byom("onnx_xgb_iris", "byom_models")
Model is deleted.
In [ ]:
db_drop_table("byom_models")
Out[ ]:
True
In [ ]:
db_drop_table("iris_input")
Out[ ]:
True
In [ ]:
remove_context()
Out[ ]:
True
PMMLPredict
Decision Tree
PMMLPredict() using Decision Tree model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, delete_byom, retrieve_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
con = create_context(host=getpass.getpass("Hostname: "), 
                     username=getpass.getpass("Username: "),
                     password=getpass.getpass("Password: "))
Hostname: ········
Username: ········
Password: ········
Load example data.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[3]:
id
sepal_length sepal_width petal_length petal_width species sampleid
78
6.7
3.0
5.0
1.7
2
1
141 6.7
3.1
5.6
2.4
3
2
17
5.4
3.9
1.3
0.4
1
1
40
5.1
3.4
1.5
0.2
1
1
120 6.0
2.2
5.0
1.5
3
2
122 5.6
2.8
4.9
2.0
3
1
19
5.7
3.8
1.7
0.3
1
1
59
6.6
2.9
4.6
1.3
2
1
80
5.7
2.6
3.5
1.0
2
1
101 6.3
3.3
6.0
2.5
3
1
In [4]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[4]:
id
sepal_length sepal_width petal_length petal_width species
78
6.7
3.0
5.0
1.7
2
141 6.7
3.1
5.6
2.4
3
17
5.4
3.9
1.3
0.4
1
80
5.7
2.6
3.5
1.0
2
57
6.3
3.3
4.7
1.6
2
122 5.6
2.8
4.9
2.0
3
19
5.7
3.8
1.7
0.3
1
59
6.6
2.9
4.6
1.3
2
120 6.0
2.2
5.0
1.5
3
101 6.3
3.3
6.0
2.5
3
In [5]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
133 6.4
2.8
5.6
2.2
3
13
4.8
3.0
1.4
0.1
1
28
5.2
3.5
1.5
0.2
1
118 7.7
3.8
6.7
2.2
3
150 5.9
3.0
5.1
1.8
3
91
5.5
2.6
4.4
1.2
2
45
5.1
3.8
1.9
0.4
1
85
5.4
3.0
4.5
1.5
2
112 6.4
2.7
5.3
1.9
3
17
5.4
3.9
1.3
0.4
1
Train Decision Tree Model.¶
In [6]:
# Import required libraries.
import numpy as np
from sklearn import tree
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
In [7]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = iris_train.to_pandas()
features = traid_pd.columns.drop('species')
target = 'species'
In [8]:
# Generate the Decision Tree model.
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
rf_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['sepal_length', 'sepal_width'], StandardScaler()) ,
    (['petal_length', 'petal_width'], imputer)
    ])),
    ("dt", tree.DecisionTreeClassifier())
])
In [9]:
rf_pipe_obj.fit(traid_pd[features], traid_pd[target])
Out[9]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['sepal_length', 'sepal_width'],
                                            StandardScaler()),
                                           (['petal_length', 'petal_width'],
                                            SimpleImputer())])),
                ('dt', DecisionTreeClassifier())])
Save the model in PMML format.¶
In [10]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_dt_model.pmml"
In [11]:
skl_to_pmml(rf_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [12]:
# Save the PMML Model in Vantage.
save_byom("pmml_decision_tree_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [13]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                            model
model_id                                         
pmml_decision_tree_iris  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [14]:
# Retrieve the model from table "byom_models", using the model id 'pmml_decision_tree_iris'.
modeldata = retrieve_byom("pmml_decision_tree_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [15]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [16]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    )
In [17]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__163422797257025" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__163428788546008") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [18]:
# Print the result.
result.result
Out[18]:
id
sepal_length petal_length prediction
json_report
24
5.1
1.7
1
{"probability_1":1.0,"predicted_species":1,"probability_2":0.0,"probability_3":0.0}
89
5.6
4.1
2
{"probability_1":0.0,"predicted_species":2,"probability_2":1.0,"probability_3":0.0}
66
6.7
4.4
2
{"probability_1":0.0,"predicted_species":2,"probability_2":1.0,"probability_3":0.0}
112 6.4
5.3
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
127 6.2
4.8
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
30
4.7
1.6
1
{"probability_1":1.0,"predicted_species":1,"probability_2":0.0,"probability_3":0.0}
129 6.4
5.6
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
146 6.7
5.2
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
7
4.6
1.4
1
{"probability_1":1.0,"predicted_species":1,"probability_2":0.0,"probability_3":0.0}
51
7.0
4.7
2
{"probability_1":0.0,"predicted_species":2,"probability_2":1.0,"probability_3":0.0}
Cleanup.¶
In [19]:
# Delete the model from table "byom_models", using the model id 'pmml_decision_tree_iris'.
delete_byom("pmml_decision_tree_iris", "byom_models")
Model is deleted.
In [20]:
# Drop models table.
db_drop_table("byom_models")
Out[20]:
True
In [21]:
# Drop input data tables. 
db_drop_table("iris_input")
Out[21]:
True
In [22]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[22]:
True
GLM
PMMLPredict() using Decision Tree model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, delete_byom, retrieve_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
con = create_context(host=getpass.getpass("Hostname: "), 
                     username=getpass.getpass("Username: "),
                     password=getpass.getpass("Password: "))
Hostname: ········
Username: ········
Password: ········
Load example data.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[3]:
id
sepal_length sepal_width petal_length petal_width species sampleid
78
6.7
3.0
5.0
1.7
2
1
141 6.7
3.1
5.6
2.4
3
2
17
5.4
3.9
1.3
0.4
1
1
40
5.1
3.4
1.5
0.2
1
1
120 6.0
2.2
5.0
1.5
3
2
122 5.6
2.8
4.9
2.0
3
1
19
5.7
3.8
1.7
0.3
1
1
59
6.6
2.9
4.6
1.3
2
1
80
5.7
2.6
3.5
1.0
2
1
101 6.3
3.3
6.0
2.5
3
1
In [4]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[4]:
id
sepal_length sepal_width petal_length petal_width species
78
6.7
3.0
5.0
1.7
2
141 6.7
3.1
5.6
2.4
3
17
5.4
3.9
1.3
0.4
1
80
5.7
2.6
3.5
1.0
2
57
6.3
3.3
4.7
1.6
2
122 5.6
2.8
4.9
2.0
3
19
5.7
3.8
1.7
0.3
1
59
6.6
2.9
4.6
1.3
2
120 6.0
2.2
5.0
1.5
3
101 6.3
3.3
6.0
2.5
3
In [5]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
133 6.4
2.8
5.6
2.2
3
13
4.8
3.0
1.4
0.1
1
28
5.2
3.5
1.5
0.2
1
118 7.7
3.8
6.7
2.2
3
150 5.9
3.0
5.1
1.8
3
91
5.5
2.6
4.4
1.2
2
45
5.1
3.8
1.9
0.4
1
85
5.4
3.0
4.5
1.5
2
112 6.4
2.7
5.3
1.9
3
17
5.4
3.9
1.3
0.4
1
Train Decision Tree Model.¶
In [6]:
# Import required libraries.
import numpy as np
from sklearn import tree
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
In [7]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = iris_train.to_pandas()
features = traid_pd.columns.drop('species')
target = 'species'
In [8]:
# Generate the Decision Tree model.
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
rf_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['sepal_length', 'sepal_width'], StandardScaler()) ,
    (['petal_length', 'petal_width'], imputer)
    ])),
    ("dt", tree.DecisionTreeClassifier())
])
In [9]:
rf_pipe_obj.fit(traid_pd[features], traid_pd[target])
Out[9]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['sepal_length', 'sepal_width'],
                                            StandardScaler()),
                                           (['petal_length', 'petal_width'],
                                            SimpleImputer())])),
                ('dt', DecisionTreeClassifier())])
Save the model in PMML format.¶
In [10]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_dt_model.pmml"
In [11]:
skl_to_pmml(rf_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [12]:
# Save the PMML Model in Vantage.
save_byom("pmml_decision_tree_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [13]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                            model
model_id                                         
pmml_decision_tree_iris  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [14]:
# Retrieve the model from table "byom_models", using the model id 'pmml_decision_tree_iris'.
modeldata = retrieve_byom("pmml_decision_tree_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [15]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [16]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    )
In [17]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__163422797257025" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__163428788546008") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [18]:
# Print the result.
result.result
Out[18]:
id
sepal_length petal_length prediction
json_report
24
5.1
1.7
1
{"probability_1":1.0,"predicted_species":1,"probability_2":0.0,"probability_3":0.0}
89
5.6
4.1
2
{"probability_1":0.0,"predicted_species":2,"probability_2":1.0,"probability_3":0.0}
66
6.7
4.4
2
{"probability_1":0.0,"predicted_species":2,"probability_2":1.0,"probability_3":0.0}
112 6.4
5.3
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
127 6.2
4.8
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
30
4.7
1.6
1
{"probability_1":1.0,"predicted_species":1,"probability_2":0.0,"probability_3":0.0}
129 6.4
5.6
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
146 6.7
5.2
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
7
4.6
1.4
1
{"probability_1":1.0,"predicted_species":1,"probability_2":0.0,"probability_3":0.0}
51
7.0
4.7
2
{"probability_1":0.0,"predicted_species":2,"probability_2":1.0,"probability_3":0.0}
Cleanup.¶
In [19]:
# Delete the model from table "byom_models", using the model id 'pmml_decision_tree_iris'.
delete_byom("pmml_decision_tree_iris", "byom_models")
Model is deleted.
In [20]:
# Drop models table.
db_drop_table("byom_models")
Out[20]:
True
In [21]:
# Drop input data tables. 
db_drop_table("iris_input")
Out[21]:
True
In [22]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[22]:
True
Isolation Forest
PMMLPredict() using Isolation Forest model¶
Setup¶
In [1]:
# Import required libraries
import getpass
import tempfile
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
In [4]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows.
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [5]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
139 6.0
3.0
4.8
1.8
3
38
4.9
3.6
1.4
0.1
1
78
6.7
3.0
5.0
1.7
2
122 5.6
2.8
4.9
2.0
3
59
6.6
2.9
4.6
1.3
2
40
5.1
3.4
1.5
0.2
1
80
5.7
2.6
3.5
1.0
2
120 6.0
2.2
5.0
1.5
3
19
5.7
3.8
1.7
0.3
1
61
5.0
2.0
3.5
1.0
2
In [6]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
142 6.9
3.1
5.1
2.3
3
108 7.3
2.9
6.3
1.8
3
43
4.4
3.2
1.3
0.2
1
59
6.6
2.9
4.6
1.3
2
100 5.7
2.8
4.1
1.3
2
118 7.7
3.8
6.7
2.2
3
55
6.5
2.8
4.6
1.5
2
150 5.9
3.0
5.1
1.8
3
74
6.1
2.8
4.7
1.2
2
131 7.4
2.8
6.1
1.9
3
Train Isolation Forest model¶
In [7]:
# Import required libraries.
import numpy as np
from sklearn import tree
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn.ensemble import IsolationForest
In [8]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = ""
In [9]:
#convert the data to a list to pass it to isolation forest pipeline.
data=list(train_pd[features].values.reshape(-1, 1))
In [10]:
# Generate the isolation forest model
if_pipe_obj = Pipeline([
    ("iforest", IsolationForest(n_estimators=30, max_samples=300, contamination=0, random_state=np.random.RandomState(42)))
])
In [11]:
if_pipe_obj.fit(data[1:])
Out[11]:
Pipeline(steps=[('iforest',
                 IsolationForest(contamination=0, max_samples=300,
                                 n_estimators=30,
                                 random_state=RandomState(MT19937) at 0x14151459258))])
Save the model in PMML format.¶
In [12]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iforest_pmml.pmml"
In [13]:
skl_to_pmml(if_pipe_obj, features, "",model_file_path)
Save the model in Vantage.¶
In [14]:
# Save the PMML Model in Vantage.
save_byom("iforest_pmml", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the model from Vantage.¶
In [15]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                 model
model_id                              
iforest_pmml  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [16]:
# Retrieve the model from table "byom_models", using the model id 'iforest_pmml'.
modeldata = retrieve_byom("iforest_pmml", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [17]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [18]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length','petal_width','sepal_width'],
                    overwrite_cached_models = '*',
                    )
In [19]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__164651977146640" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__164656379573423") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length','petal_width','sepal_width')
OverwriteCachedModel('*')
) as sqlmr
In [20]:
# Print the result.
result.result
Out[20]:
id
sepal_length petal_length petal_width sepal_width prediction
json_report
1
5.1
1.4
0.2
3.5
{"outlier":false,"anomalyScore":0.48314240193515484}
64
6.1
4.7
1.4
2.9
{"outlier":false,"anomalyScore":0.529516768216189}
83
5.8
3.9
1.2
2.7
{"outlier":false,"anomalyScore":0.5161471379987066}
55
6.5
4.6
1.5
2.8
{"outlier":false,"anomalyScore":0.5837145460178593}
112 6.4
5.3
1.9
2.7
{"outlier":false,"anomalyScore":0.5271424393254006}
19
5.7
1.7
0.3
3.8
{"outlier":false,"anomalyScore":0.5033194910283447}
36
5.0
1.2
0.2
3.2
{"outlier":false,"anomalyScore":0.48899373649243966}
137 6.3
5.6
2.4
3.4
{"outlier":false,"anomalyScore":0.5154114042761518}
135 6.1
5.6
1.4
2.6
{"outlier":false,"anomalyScore":0.529516768216189}
51
7.0
4.7
1.4
3.2
{"outlier":false,"anomalyScore":0.5946402582669383}
Cleanup.¶
In [21]:
# Delete the saved Model.
delete_byom("iforest_pmml", table_name="byom_models")
Model is deleted.
In [22]:
# Drop model table.
db_drop_table("byom_models")
Out[22]:
True
In [23]:
# Drop input data table.
db_drop_table("iris_input")
Out[23]:
True
In [24]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[24]:
True
In [ ]:
KMeansClustering
PMMLPredict() using KMeans Clustering¶
Setup¶
In [1]:
# Import required libraries
import getpass
import tempfile
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use DataFrame.sample() for splitting input data into testing and training dataset.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
WARNING: Skipped loading table iris_input since it already exists in the database.
In [4]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows.
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [5]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
99
5.1
2.5
3.0
1.1
2
120 6.0
2.2
5.0
1.5
3
57
6.3
3.3
4.7
1.6
2
101 6.3
3.3
6.0
2.5
3
17
5.4
3.9
1.3
0.4
1
61
5.0
2.0
3.5
1.0
2
38
4.9
3.6
1.4
0.1
1
78
6.7
3.0
5.0
1.7
2
141 6.7
3.1
5.6
2.4
3
40
5.1
3.4
1.5
0.2
1
In [6]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
74
6.1
2.8
4.7
1.2
2
135 6.1
2.6
5.6
1.4
3
110 7.2
3.6
6.1
2.5
3
141 6.7
3.1
5.6
2.4
3
13
4.8
3.0
1.4
0.1
1
133 6.4
2.8
5.6
2.2
3
108 7.3
2.9
6.3
1.8
3
1
5.1
3.5
1.4
0.2
1
139 6.0
3.0
4.8
1.8
3
55
6.5
2.8
4.6
1.5
2
Train Kmeans Clustering model¶
In [7]:
# Import required libraries.
import numpy as np
from sklearn import tree
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
In [8]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
In [9]:
# Generate Kmeans Clustering pipeline.
from sklearn.cluster import SpectralClustering
from sklearn.cluster import KMeans
km_pipe_obj = Pipeline([
    ("km",KMeans(n_clusters=8, random_state=0))
])
In [10]:
km_pipe_obj.fit(train_pd[features], train_pd[target])
Out[10]:
Pipeline(steps=[('km', KMeans(random_state=0))])
Save the model in PMML format.¶
In [11]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_kmeans_model.pmml"
In [12]:
skl_to_pmml(km_pipe_obj ,features,target,model_file_path)
Save the model in Vantage.¶
In [13]:
# Save the PMML Model in Vantage.
save_byom("pmml_kmeans_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the model from Vantage.¶
In [14]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                     model
model_id                                  
pmml_kmeans_iris  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [15]:
# Retrieve the model from table "byom_models", using the model id 'pmml_kmeans_iris'.
modeldata = retrieve_byom("pmml_kmeans_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [16]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [17]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    )
In [18]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__1646324588886704" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1646324705248551") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [19]:
# Print the result.
result.result
Out[19]:
id
sepal_length petal_length prediction
26
5.0
1.6
{"cluster":"5","afﬁnity(0)":0.751672360718738,"afﬁnity(1)":3.544763216406395,"afﬁnity(2)":3.9376134904279265,"afﬁnity(3)":5.6383281
9
4.4
1.4
{"cluster":"5","afﬁnity(0)":1.1611824452693957,"afﬁnity(1)":3.9874248314800367,"afﬁnity(2)":4.300248054860711,"afﬁnity(3)":6.091336
49
5.3
1.5
{"cluster":"0","afﬁnity(0)":0.1012394839308061,"afﬁnity(1)":3.5942320675110357,"afﬁnity(2)":4.070560976900686,"afﬁnity(3)":5.631148
118 7.7
6.7
{"cluster":"3","afﬁnity(0)":6.076673738059406,"afﬁnity(1)":2.6979678409710757,"afﬁnity(2)":2.6210939192126124,"afﬁnity(3)":0.859078
62
5.9
4.2
{"cluster":"6","afﬁnity(0)":3.126120660737127,"afﬁnity(1)":0.6744033990115543,"afﬁnity(2)":0.9434687770845057,"afﬁnity(3)":2.704174
70
5.6
3.9
{"cluster":"6","afﬁnity(0)":2.8388062388401933,"afﬁnity(1)":1.2373906958079262,"afﬁnity(2)":1.4147320123142286,"afﬁnity(3)":3.25922
129 6.4
5.6
{"cluster":"7","afﬁnity(0)":4.731129121725585,"afﬁnity(1)":1.221120687758594,"afﬁnity(2)":0.7412601882380211,"afﬁnity(3)":1.3466518
140 6.9
5.4
{"cluster":"7","afﬁnity(0)":4.663716268503754,"afﬁnity(1)":1.148719629284536,"afﬁnity(2)":1.053312236075641,"afﬁnity(3)":1.10075105
110 7.2
6.1
{"cluster":"3","afﬁnity(0)":5.483417591313434,"afﬁnity(1)":2.0898448563340066,"afﬁnity(2)":1.942026433050453,"afﬁnity(3)":0.8044298
34
5.5
1.4
{"cluster":"0","afﬁnity(0)":0.5771538581381218,"afﬁnity(1)":3.755217976344582,"afﬁnity(2)":4.274903507682951,"afﬁnity(3)":5.7194101
Cleanup.¶
In [20]:
# Delete the saved Model.
delete_byom("pmml_kmeans_iris", table_name="byom_models")
Model is deleted.
In [21]:
# Drop model table.
db_drop_table("byom_models")
Out[21]:
True
In [22]:
# Drop input data table.
db_drop_table("iris_input")
Out[22]:
True
In [23]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[23]:
True
In [ ]:
K-Nearest Neighbors (KNN)
PMMLPredict using K-Nearest Neighbors model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
WARNING: Skipped loading table iris_input since it already exists in the database.
In [4]:
# Create teradataml DataFrames.
iris_input = DataFrame("iris_input")
In [5]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[5]:
id
sepal_length sepal_width petal_length petal_width species sampleid
59
6.6
2.9
4.6
1.3
2
1
80
5.7
2.6
3.5
1.0
2
1
120 6.0
2.2
5.0
1.5
3
1
101 6.3
3.3
6.0
2.5
3
1
17
5.4
3.9
1.3
0.4
1
2
61
5.0
2.0
3.5
1.0
2
1
38
4.9
3.6
1.4
0.1
1
1
78
6.7
3.0
5.0
1.7
2
2
141 6.7
3.1
5.6
2.4
3
1
40
5.1
3.4
1.5
0.2
1
1
In [6]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
97
5.7
2.9
4.2
1.3
2
80
5.7
2.6
3.5
1.0
2
120 6.0
2.2
5.0
1.5
3
101 6.3
3.3
6.0
2.5
3
17
5.4
3.9
1.3
0.4
1
38
4.9
3.6
1.4
0.1
1
76
6.6
3.0
4.4
1.4
2
116 6.4
3.2
5.3
2.3
3
141 6.7
3.1
5.6
2.4
3
40
5.1
3.4
1.5
0.2
1
In [7]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[7]:
id
sepal_length sepal_width petal_length petal_width species
114 5.7
2.5
5.0
2.0
3
72
6.1
2.8
4.0
1.3
2
112 6.4
2.7
5.3
1.9
3
141 6.7
3.1
5.6
2.4
3
87
6.7
3.1
4.7
1.5
2
78
6.7
3.0
5.0
1.7
2
76
6.6
3.0
4.4
1.4
2
26
5.0
3.0
1.6
0.2
1
34
5.5
4.2
1.4
0.2
1
80
5.7
2.6
3.5
1.0
2
Train the KNN model.¶
In [8]:
# Import required libraries.
import numpy as np
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.neighbors import KNeighborsClassifier
from sklearn.preprocessing import StandardScaler
In [9]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = iris_train.to_pandas()
features = traid_pd.columns.drop('species')
target = 'species'
In [10]:
# Generate the k-nearest neighbors model.
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
KNN_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['sepal_length', 'sepal_width'], StandardScaler()) ,
    (['petal_length', 'petal_width'], imputer)
    ])),
    ("knn", KNeighborsClassifier(n_neighbors=5))
])
In [11]:
KNN_pipe_obj.fit(traid_pd[features], traid_pd[target])
Out[11]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['sepal_length', 'sepal_width'],
                                            StandardScaler()),
                                           (['petal_length', 'petal_width'],
                                            SimpleImputer())])),
                ('knn', KNeighborsClassifier())])
Save the model in PMML format.¶
In [12]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_KNN_model.pmml"
In [13]:
skl_to_pmml(KNN_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [14]:
# Save the PMML Model in Vantage.
save_byom("pmml_KNN_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage¶
In [15]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                  model
model_id                               
pmml_KNN_iris  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [16]:
# Retrieve the model from table "byom_models", using the model id 'pmml_KNN_iris'.
modeldata = retrieve_byom("pmml_KNN_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [17]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [18]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    )
In [19]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__1644918907764915" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1644919445800902") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [20]:
# Print the result.
result.result
Out[20]:
id
sepal_length petal_length prediction
json_report
52
6.4
4.5
2
{"predicted_species":2}
74
6.1
4.7
2
{"predicted_species":2}
70
5.6
3.9
2
{"predicted_species":2}
38
4.9
1.4
1
{"predicted_species":1}
93
5.8
4.0
2
{"predicted_species":2}
13
4.8
1.4
1
{"predicted_species":1}
32
5.4
1.5
1
{"predicted_species":1}
68
5.8
4.1
2
{"predicted_species":2}
116 6.4
5.3
3
{"predicted_species":3}
122 5.6
4.9
3
{"predicted_species":3}
Cleanup.¶
In [21]:
# Delete the model from table "byom_models", using the model id 'pmml_KNN_iris'.
delete_byom("pmml_KNN_iris", "byom_models")
Model is deleted.
In [22]:
# Drop models table.
db_drop_table("byom_models")
Out[22]:
True
In [23]:
# Drop input data tables. 
db_drop_table("iris_input")
Out[23]:
True
In [24]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[24]:
True
Linear Regression
PMMLPredict using Linear Regression model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data¶
In [3]:
# Load the example data.
load_example_data("dataframe", "insurance")
In [4]:
# Create teradataml DataFrames.
insurance_input = DataFrame("insurance")
In [5]:
insurance_input
Out[5]:
age
sex
bmi
children smoker
region
charges
19
female 28.6
5
no
southwest 4687.797
40
male
26.315 1
no
northwest 6389.37785
40
female 36.19
0
no
southeast 5920.1041
34
female 31.92
1
yes
northeast
37701.8768
34
female 27.5
1
no
southwest 5003.853
61
female 39.1
2
no
southwest 14235.072
61
female 29.92
3
yes
southeast 30942.1918
61
female 22.04
0
no
northeast
13616.3586
34
female 37.335 2
no
northwest 5989.52365
40
female 28.69
3
no
northwest 8059.6791
One Hot Encoder¶
In [6]:
# import required library.
from teradataml import  OneHotEncoder, valib, Retain
configure.val_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
In [7]:
# Perform One Hot Encoding on the categorical features.   
dc1 = OneHotEncoder(values=["southwest", "southeast", "northwest", "northeast" ], columns="region")
dc2 = OneHotEncoder(style="contrast", values="yes", reference_value=0, columns="smoker")
dc3 = OneHotEncoder(style="contrast", values="male", reference_value=0, columns="sex")
In [8]:
# Retaining Some columns unchanged.
retain = Retain(columns=["charges","bmi","children"])
In [9]:
# Execute Transform() function.
obj = valib.Transform(data=insurance_input, one_hot_encode=[dc1,dc2,dc3], key_columns="ID", retain=retain)
insurance_input = obj.result
insurance_input
Out[9]:
age
charges
bmi
children southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex
19
4687.797
28.6
5
1
0
0
0
0
0
19
1743.214
28.9
0
1
0
0
0
0
0
19
2331.519
28.4
1
1
0
0
0
0
0
19
17081.08
28.3
0
1
0
0
0
1
0
19
1261.442
34.1
0
1
0
0
0
0
1
19
1842.519
28.4
1
1
0
0
0
0
1
19
1632.56445 25.555 0
0
0
1
0
0
1
19
1625.43375 20.425 0
0
0
1
0
0
1
19
1837.237
24.6
1
1
0
0
0
0
1
19
16884.924
27.9
0
1
0
0
0
1
0
use DataFrame.sample() for splitting input data into testing and training dataset.¶
In [10]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
insurance_sample = insurance_input.sample(frac=[0.8, 0.2])
insurance_sample
Out[10]:
age
charges
bmi
children southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex sampleid
34
5003.853
27.5
1
1
0
0
0
0
0
2
61
30942.1918 29.92
3
0
1
0
0
1
0
1
61
13616.3586 22.04
0
0
0
0
1
0
0
1
19
16884.924
27.9
0
1
0
0
0
1
0
1
19
4687.797
28.6
5
1
0
0
0
0
0
2
40
8059.6791
28.69
3
0
0
1
0
0
0
2
40
6389.37785 26.315 1
0
0
1
0
0
1
1
40
5920.1041
36.19
0
0
1
0
0
0
0
2
19
1837.237
24.6
1
1
0
0
0
0
1
1
61
14235.072
39.1
2
1
0
0
0
0
0
1
In [11]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
insurance_train = insurance_sample[insurance_sample.sampleid == "1"].drop("sampleid", axis = 1)
insurance_train
Out[11]:
age
charges
bmi
children southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex
34
27375.90478 22.42
2
0
0
0
1
0
1
61
30942.1918
29.92
3
0
1
0
0
1
0
61
13616.3586
22.04
0
0
0
0
1
0
0
19
16884.924
27.9
0
1
0
0
0
1
0
19
4687.797
28.6
5
1
0
0
0
0
0
40
8059.6791
28.69
3
0
0
1
0
0
0
40
6389.37785
26.315 1
0
0
1
0
0
1
40
5920.1041
36.19
0
0
1
0
0
0
0
19
1837.237
24.6
1
1
0
0
0
0
1
61
14235.072
39.1
2
1
0
0
0
0
0
In [12]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
insurance_test = insurance_sample[insurance_sample.sampleid == "2"].drop("sampleid", axis = 1)
insurance_test
Out[12]:
age
charges
bmi
children southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex
34
3935.1799
34.21
0
0
1
0
0
0
1
61
48517.56315 36.385 1
0
0
0
1
1
0
61
24513.09126 25.08
0
0
1
0
0
0
0
19
1837.237
24.6
1
1
0
0
0
0
1
19
1261.442
34.1
0
1
0
0
0
0
1
40
7077.1894
25.46
1
0
0
0
1
0
0
40
6600.20595
34.105 1
0
0
0
1
0
1
40
7173.35995
22.705 2
0
0
0
1
0
1
19
2331.519
28.4
1
1
0
0
0
0
0
61
14235.072
39.1
2
1
0
0
0
0
0
Prepare dataset for creating a Linear Regression model.¶
In [13]:
# Import required libraries.
import numpy as np
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
In [14]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = insurance_train.to_pandas()
features = traid_pd.columns.drop('charges')
target = 'charges'
Train Linear Regression Model.¶
In [15]:
# Import required libraries.
from sklearn import linear_model
In [16]:
# Generate the Linear Regressor model.
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
LRM_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['bmi'], StandardScaler()) ,
    (['male_sex', 'children'], imputer)
    ])),
    ("LRM", linear_model.LinearRegression())
])
In [17]:
LRM_pipe_obj.fit(traid_pd[features], traid_pd[target])
Out[17]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['bmi'], StandardScaler()),
                                           (['male_sex', 'children'],
                                            SimpleImputer())])),
                ('LRM', LinearRegression())])
Save the model in PMML format.¶
In [18]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/insurance_db_LRM_model.pmml"
In [19]:
skl_to_pmml(LRM_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [20]:
# Save the PMML Model in Vantage.
save_byom("pmml_LRM_insurance", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage¶
In [21]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                       model
model_id                                    
pmml_LRM_insurance  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [22]:
# Retrieve the model from table "byom_models", using the model id 'pmml_LRM_insurance'.
modeldata = retrieve_byom("pmml_LRM_insurance", "byom_models")
In [23]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [24]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = insurance_test,
                    accumulate = ['male_sex', 'bmi', 'children'],
                    overwrite_cached_models = '*',
                    )
In [25]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__1646289308222356" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1646284961549474") AS ModelTable
DIMENSION
USING
Accumulate('male_sex','bmi','children')
OverwriteCachedModel('*')
) as sqlmr
In [26]:
# Print the result.
result.result
Out[26]:
male_sex
bmi
children
prediction
json_report
0
21.09
0
8496.226240089216
{"predicted_charges":8496.226240089216}
0
25.46
1
10814.194870937952 {"predicted_charges":10814.194870937952}
0
28.12
1
11905.440626739286 {"predicted_charges":11905.440626739286}
1
20.425 0
9079.054672356815
{"predicted_charges":9079.054672356815}
0
17.8
0
7146.527542124409
{"predicted_charges":7146.527542124409}
0
33.25
1
14009.986012927573 {"predicted_charges":14009.986012927573}
0
23.56
0
9509.525870476169
{"predicted_charges":9509.525870476169}
1
42.13
2
19033.78938667244
{"predicted_charges":19033.78938667244}
1
28.4
1
12875.948472252094 {"predicted_charges":12875.948472252094}
1
26.315 1
12020.592306520599 {"predicted_charges":12020.592306520599}
Cleanup.¶
In [27]:
# Delete the model from table "byom_models", using the model id 'pmml_LRM_insurance'.
delete_byom("pmml_LRM_insurance", "byom_models")
Model is deleted.
In [28]:
# Drop models table.
db_drop_table("byom_models")
Out[28]:
True
In [29]:
# Drop input data tables.
db_drop_table("insurance")
Out[29]:
True
In [30]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[30]:
True
Logistic Regression
PMMLPredict() using Logistic Regression Model¶
Setup¶
In [1]:
# Import required libraries
import getpass
import tempfile
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
In [4]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows.
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [5]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
17
5.4
3.9
1.3
0.4
1
59
6.6
2.9
4.6
1.3
2
99
5.1
2.5
3.0
1.1
2
40
5.1
3.4
1.5
0.2
1
57
6.3
3.3
4.7
1.6
2
61
5.0
2.0
3.5
1.0
2
78
6.7
3.0
5.0
1.7
2
76
6.6
3.0
4.4
1.4
2
120 6.0
2.2
5.0
1.5
3
122 5.6
2.8
4.9
2.0
3
In [6]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
106 7.6
3.0
6.6
2.1
3
38
4.9
3.6
1.4
0.1
1
148 6.5
3.0
5.2
2.0
3
19
5.7
3.8
1.7
0.3
1
137 6.3
3.4
5.6
2.4
3
55
6.5
2.8
4.6
1.5
2
95
5.6
2.7
4.2
1.3
2
110 7.2
3.6
6.1
2.5
3
36
5.0
3.2
1.2
0.2
1
61
5.0
2.0
3.5
1.0
2
Train Logistic Regression model¶
In [7]:
# Import required libraries.
import numpy as np
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression
In [8]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
In [9]:
# Generate the logistic regression model
LogReg_pipe_obj = Pipeline([
    ("LogReg", LogisticRegression(random_state=0))
])
In [10]:
LogReg_pipe_obj.fit(train_pd[features],train_pd[target])
C:\Users\pg255042\Anaconda3\envs\teraml\lib\site-packages\sklearn\linear_model\_logistic.py:765: ConvergenceWarning: lbfgs failed to 
STOP: TOTAL NO. of ITERATIONS REACHED LIMIT.
Increase the number of iterations (max_iter) or scale the data as shown in:
    https://scikit-learn.org/stable/modules/preprocessing.html
Please also refer to the documentation for alternative solver options:
    https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression
  extra_warning_msg=_LOGISTIC_SOLVER_CONVERGENCE_MSG)
Out[10]:
Pipeline(steps=[('LogReg', LogisticRegression(random_state=0))])
Save the model in PMML format.¶
In [11]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/LogReg_pmml.pmml"
In [12]:
skl_to_pmml(LogReg_pipe_obj, features, target,model_file_path)
Save the model in Vantage.¶
In [13]:
# Save the PMML Model in Vantage.
save_byom("LogReg_pmml", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the model from Vantage.¶
In [14]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                model
model_id                             
LogReg_pmml  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [15]:
# Retrieve the model from table "byom_models", using the model id 'LogReg_pmml'.
modeldata = retrieve_byom("LogReg_pmml", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [16]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [17]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length','petal_width','sepal_width'],
                    overwrite_cached_models = '*',
                    )
In [18]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__1646280875903936" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1646286065165265") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length','petal_width','sepal_width')
OverwriteCachedModel('*')
) as sqlmr
In [19]:
# Print the result.
result.result
Out[19]:
id
sepal_length petal_length petal_width sepal_width prediction
json_report
28
5.2
1.5
0.2
3.5
1
{"probability_1":0.5092043722052524,"predicted_species":1,"probability_2":0.4907717686590658,"probab
38
4.9
1.4
0.1
3.6
1
{"probability_1":0.5100130647653455,"predicted_species":1,"probability_2":0.4899721767209039,"probab
148 6.5
5.2
2.0
3.0
3
{"probability_1":0.004010147129431223,"predicted_species":3,"probability_2":0.46571406846300106,"pro
99
5.1
3.0
1.1
2.5
2
{"probability_1":0.453516154312164,"predicted_species":2,"probability_2":0.5349786491137434,"probabi
74
6.1
4.7
1.2
2.8
2
{"probability_1":0.031362355689500854,"predicted_species":2,"probability_2":0.5734551881733468,"prob
40
5.1
1.5
0.2
3.4
1
{"probability_1":0.5091500534660736,"predicted_species":1,"probability_2":0.49082480091847847,"proba
5}
80
5.7
3.5
1.0
2.6
2
{"probability_1":0.34394424099857585,"predicted_species":2,"probability_2":0.6200129236908061,"proba
118 7.7
6.7
2.2
3.8
3
{"probability_1":1.258246923313423E-4,"predicted_species":3,"probability_2":0.4415798744675827,"prob
15
5.8
1.2
0.2
4.0
1
{"probability_1":0.5085773809738329,"predicted_species":1,"probability_2":0.491414288701027,"probabi
61
5.0
3.5
1.0
2.0
2
{"probability_1":0.30610641895198143,"predicted_species":2,"probability_2":0.6433684908861783,"proba
Cleanup.¶
In [20]:
# Delete the saved Model.
delete_byom("LogReg_pmml", table_name="byom_models")
Model is deleted.
In [21]:
# Drop model table.
db_drop_table("byom_models")
Out[21]:
True
In [22]:
# Drop input data table.
db_drop_table("iris_input")
Out[22]:
True
In [23]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[23]:
True
In [ ]:
MLP
PMMLPredict using Nueral Networks MLP Classiﬁer model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
WARNING: Skipped loading table iris_input since it already exists in the database.
In [4]:
# Create teradataml DataFrames
iris_input = DataFrame("iris_input")
In [5]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[5]:
id
sepal_length sepal_width petal_length petal_width species sampleid
78
6.7
3.0
5.0
1.7
2
1
141 6.7
3.1
5.6
2.4
3
1
17
5.4
3.9
1.3
0.4
1
1
40
5.1
3.4
1.5
0.2
1
2
120 6.0
2.2
5.0
1.5
3
1
122 5.6
2.8
4.9
2.0
3
1
19
5.7
3.8
1.7
0.3
1
1
59
6.6
2.9
4.6
1.3
2
1
80
5.7
2.6
3.5
1.0
2
1
101 6.3
3.3
6.0
2.5
3
1
In [6]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
116 6.4
3.2
5.3
2.3
3
17
5.4
3.9
1.3
0.4
1
139 6.0
3.0
4.8
1.8
3
40
5.1
3.4
1.5
0.2
1
120 6.0
2.2
5.0
1.5
3
122 5.6
2.8
4.9
2.0
3
19
5.7
3.8
1.7
0.3
1
59
6.6
2.9
4.6
1.3
2
80
5.7
2.6
3.5
1.0
2
101 6.3
3.3
6.0
2.5
3
In [7]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[7]:
id
sepal_length sepal_width petal_length petal_width species
5
5.0
3.6
1.4
0.2
1
118 7.7
3.8
6.7
2.2
3
95
5.6
2.7
4.2
1.3
2
36
5.0
3.2
1.2
0.2
1
30
4.7
3.2
1.6
0.2
1
51
7.0
3.2
4.7
1.4
2
66
6.7
3.1
4.4
1.4
2
142 6.9
3.1
5.1
2.3
3
15
5.8
4.0
1.2
0.2
1
80
5.7
2.6
3.5
1.0
2
Train the MLP Model.¶
In [8]:
# Import required libraries.
import numpy as np
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.neural_network import MLPClassifier
from sklearn.preprocessing import StandardScaler
In [9]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = iris_train.to_pandas()
features = traid_pd.columns.drop('species')
target = 'species'
In [10]:
# Generate the Multi-layer Perceptron classifier model.
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
MLP_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['sepal_length', 'sepal_width'], StandardScaler()) ,
    (['petal_length', 'petal_width'], imputer)
    ])),
    ("mlp", MLPClassifier(random_state=7, max_iter=200))
])
In [11]:
MLP_pipe_obj.fit(traid_pd[features], traid_pd[target])
C:\Users\pg255042\Anaconda3\envs\teraml\lib\site-packages\sklearn\neural_network\_multilayer_perceptron.py:617: ConvergenceWarning: S
  % self.max_iter, ConvergenceWarning)
Out[11]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['sepal_length', 'sepal_width'],
                                            StandardScaler()),
                                           (['petal_length', 'petal_width'],
                                            SimpleImputer())])),
                ('mlp', MLPClassifier(random_state=7))])
Save the model in PMML format.¶
In [12]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_MLP_model.pmml"
In [13]:
skl_to_pmml(MLP_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [14]:
# Save the PMML Model in Vantage.
save_byom("pmml_MLP_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage¶
In [15]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                  model
model_id                               
pmml_MLP_iris  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [16]:
# Retrieve the model from table "byom_models", using the model id 'pmml_MLP_iris'.
modeldata = retrieve_byom("pmml_MLP_iris", "byom_models")
In [17]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [18]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    )
In [19]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__1644917801303812" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1644917506408693") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [20]:
# Print the result.
result.result
Out[20]:
id
sepal_length petal_length prediction
json_report
135 6.1
5.6
2
{"probability_1":0.0028729239874679523,"predicted_species":2,"probability_2":0.7423866117249545,"probability_3":0.25474046428
30
4.7
1.6
1
{"probability_1":0.9752530343409634,"predicted_species":1,"probability_2":0.02018545712712987,"probability_3":0.0045615085319
91
5.5
4.4
2
{"probability_1":0.016532572212896994,"predicted_species":2,"probability_2":0.8223711670195064,"probability_3":0.161096260767
93
5.8
4.0
2
{"probability_1":0.012263607385504423,"predicted_species":2,"probability_2":0.7630946810271045,"probability_3":0.224641711587
58
4.9
3.3
2
{"probability_1":0.12780047884946583,"predicted_species":2,"probability_2":0.7844663235360635,"probability_3":0.0877331976144
11
5.4
1.5
1
{"probability_1":0.9794910162772901,"predicted_species":1,"probability_2":0.013188707349543775,"probability_3":0.007320276373
49
5.3
1.5
1
{"probability_1":0.9848004762482929,"predicted_species":1,"probability_2":0.009852620035280505,"probability_3":0.005346903716
106 7.6
6.6
3
{"probability_1":2.719129314523456E-4,"predicted_species":3,"probability_2":0.14966921107492842,"probability_3":0.850058875993
108 7.3
6.3
3
{"probability_1":5.731525145961445E-4,"predicted_species":3,"probability_2":0.2686439360005586,"probability_3":0.7307829114848
97
5.7
4.2
2
{"probability_1":0.02599453802753972,"predicted_species":2,"probability_2":0.7005718844339104,"probability_3":0.2734335775385
Cleanup.¶
In [21]:
# Delete the model from table "byom_models", using the model id 'pmml_MLP_iris'.
delete_byom("pmml_MLP_iris", "byom_models")
Model is deleted.
In [22]:
# Drop models table.
db_drop_table("byom_models")
Out[22]:
True
In [23]:
# Drop input data tables.
db_drop_table("iris_input")
Out[23]:
True
In [24]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[24]:
True
MLPR
PMMLPredict using Multi-layer Perceptron model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data¶
In [3]:
# Load the example data.
load_example_data("dataframe", "insurance")
WARNING: Skipped loading table insurance since it already exists in the database.
In [4]:
# Create teradataml DataFrames.
insurance_input = DataFrame("insurance")
In [5]:
insurance_input
Out[5]:
age
sex
bmi
children smoker
region
charges
34
female 27.5
1
no
southwest 5003.853
34
male
25.3
2
yes
southeast 18972.495
34
female 26.73
1
no
southeast 5002.7827
34
female 33.7
1
no
southwest 5012.471
34
male
30.8
0
yes
southwest 35491.64
34
female 29.26
3
no
southeast 6184.2994
34
male
25.27
1
no
northwest 4894.7533
34
male
22.42
2
no
northeast
27375.90478
34
female 37.335 2
no
northwest 5989.52365
34
female 31.92
1
yes
northeast
37701.8768
Encode the data using One Hot Encoder and transform the data.¶
In [6]:
# import required library.
from teradataml import  OneHotEncoder, valib, Retain
configure.val_install_location = "MLDB"
In [7]:
# Perform One Hot Encoding on the categorical features.   
dc1 = OneHotEncoder(values=["southwest", "southeast", "northwest", "northeast" ], columns="region")
dc2 = OneHotEncoder(style="contrast", values="yes", reference_value=0, columns="smoker")
dc3 = OneHotEncoder(style="contrast", values="male", reference_value=0, columns="sex")
In [8]:
# Retaining Some columns unchanged.
retain = Retain(columns=["charges","bmi","children"])
In [9]:
# Execute Transform() function.
obj = valib.Transform(data=insurance_input, one_hot_encode=[dc1,dc2,dc3], key_columns="ID", retain=retain)
insurance_input = obj.result
insurance_input
Out[9]:
age
charges
bmi
children southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex
61
13616.3586
22.04
0
0
0
0
1
0
0
61
48517.56315 36.385 1
0
0
0
1
1
0
61
13429.0354
31.16
0
0
0
1
0
0
0
61
13415.0381
21.09
0
0
0
1
0
0
0
61
28868.6639
28.31
1
0
0
1
0
1
1
61
24513.09126 25.08
0
0
1
0
0
0
0
61
46599.1084
35.86
0
0
1
0
0
1
1
61
12557.6053
31.57
0
0
1
0
0
0
1
61
30942.1918
29.92
3
0
1
0
0
1
0
61
14235.072
39.1
2
1
0
0
0
0
0
use DataFrame.sample() for splitting input data into testing and training dataset.¶
In [10]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
insurance_sample = insurance_input.sample(frac=[0.8, 0.2])
insurance_sample
Out[10]:
age
charges
bmi
children southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex sampleid
34
5003.853
27.5
1
1
0
0
0
0
0
1
61
30942.1918 29.92
3
0
1
0
0
1
0
1
61
13616.3586 22.04
0
0
0
0
1
0
0
1
19
16884.924
27.9
0
1
0
0
0
1
0
2
19
4687.797
28.6
5
1
0
0
0
0
0
1
40
8059.6791
28.69
3
0
0
1
0
0
0
1
40
6389.37785 26.315 1
0
0
1
0
0
1
1
40
5920.1041
36.19
0
0
1
0
0
0
0
1
19
1837.237
24.6
1
1
0
0
0
0
1
2
61
14235.072
39.1
2
1
0
0
0
0
0
2
In [11]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
insurance_train = insurance_sample[insurance_sample.sampleid == "1"].drop("sampleid", axis = 1)
insurance_train
Out[11]:
age
charges
bmi
children southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex
34
5003.853
27.5
1
1
0
0
0
0
0
61
30942.1918 29.92
3
0
1
0
0
1
0
61
13616.3586 22.04
0
0
0
0
1
0
0
19
16884.924
27.9
0
1
0
0
0
1
0
19
1743.214
28.9
0
1
0
0
0
0
0
40
8059.6791
28.69
3
0
0
1
0
0
0
40
6389.37785 26.315 1
0
0
1
0
0
1
40
5920.1041
36.19
0
0
1
0
0
0
0
19
4687.797
28.6
5
1
0
0
0
0
0
61
14235.072
39.1
2
1
0
0
0
0
0
In [12]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
insurance_test = insurance_sample[insurance_sample.sampleid == "2"].drop("sampleid", axis = 1)
insurance_test
Out[12]:
age
charges
bmi children southwest_region southeast_region northwest_region northeast_region yes_smoker male_sex
34
5012.471
33.7
1
1
0
0
0
0
0
61
13415.0381
21.09 0
0
0
1
0
0
0
61
46599.1084
35.86 0
0
1
0
0
1
1
19
16884.924
27.9
0
1
0
0
0
1
0
19
1842.519
28.4
1
1
0
0
0
0
1
40
7682.67
33.0
3
0
1
0
0
0
0
40
6500.2359
29.81 1
0
1
0
0
0
0
40
15828.82173 29.3
4
1
0
0
0
0
0
19
4687.797
28.6
5
1
0
0
0
0
0
61
30942.1918
29.92 3
0
1
0
0
1
0
Prepare dataset for creating a Multi-layer Perceptron model.¶
In [13]:
# Import required libraries.
import numpy as np
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
In [14]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = insurance_train.to_pandas()
features = traid_pd.columns.drop('charges')
target = 'charges'
Train Multi-layer Perceptron model.¶
In [15]:
# Import required libraries.
from sklearn.neural_network import MLPRegressor
In [16]:
# Generate the Multi-layer Perceptron Regressor model.
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
MLP_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['bmi'], StandardScaler()) ,
    (['male_sex', 'children'], imputer)
    ])),
    ("mlp", MLPRegressor(random_state=7, max_iter=200))
])
In [17]:
MLP_pipe_obj.fit(traid_pd[features], traid_pd[target])
C:\Users\pg255042\Anaconda3\envs\teraml\lib\site-packages\sklearn\neural_network\_multilayer_perceptron.py:617: ConvergenceWarning: S
  % self.max_iter, ConvergenceWarning)
Out[17]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['bmi'], StandardScaler()),
                                           (['male_sex', 'children'],
                                            SimpleImputer())])),
                ('mlp', MLPRegressor(random_state=7))])
Save the model in PMML format.¶
In [18]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/insurance_db_MLP_model.pmml"
In [19]:
skl_to_pmml(MLP_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [20]:
# Save the PMML Model in Vantage.
save_byom("pmml_MLP_insurance", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage¶
In [21]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                       model
model_id                                    
pmml_MLP_insurance  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [22]:
# Retrieve the model from table "byom_models", using the model id 'pmml_MLP_insurance'.
modeldata = retrieve_byom("pmml_MLP_insurance", "byom_models")
In [23]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [24]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = insurance_test,
                    accumulate = ['male_sex', 'bmi', 'children'],
                    overwrite_cached_models = '*',
                    )
In [25]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__164656615949361" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__164661377060078") AS ModelTable
DIMENSION
USING
Accumulate('male_sex','bmi','children')
OverwriteCachedModel('*')
) as sqlmr
In [26]:
# Print the result.
result.result
Out[26]:
male_sex
bmi
children
prediction
json_report
1
25.3
2
866.2542248842337
{"predicted_charges":866.2542248842337}
1
33.915 0
643.1867081097179
{"predicted_charges":643.1867081097179}
1
33.535 0
629.8514774182736
{"predicted_charges":629.8514774182736}
1
20.425 0
169.78601856344287 {"predicted_charges":169.78601856344287}
0
32.11
0
319.8599458312356
{"predicted_charges":319.8599458312356}
0
28.69
3
987.9297200599833
{"predicted_charges":987.9297200599833}
1
26.315 1
639.1777110980797
{"predicted_charges":639.1777110980797}
1
29.355 1
745.8595566296347
{"predicted_charges":745.8595566296347}
0
28.4
1
452.3615998451724
{"predicted_charges":452.3615998451724}
1
43.4
0
976.0410847106386
{"predicted_charges":976.0410847106386}
Cleanup.¶
In [27]:
# Delete the model from table "byom_models", using the model id 'pmml_MLP_insurance'.
delete_byom("pmml_MLP_insurance", "byom_models")
Model is deleted.
In [28]:
# Drop models table.
db_drop_table("byom_models")
Out[28]:
True
In [29]:
# Drop input data tables.
db_drop_table("insurance")
Out[29]:
True
In [30]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[30]:
True
In [ ]:
Naive Bayes
PMMLPredict() using Naive Bayes model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, delete_byom, retrieve_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
con = create_context(host=getpass.getpass("Hostname: "), 
                     username=getpass.getpass("Username: "),
                     password=getpass.getpass("Password: "))
Hostname: ········
Username: ········
Password: ········
Load example data.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[3]:
id
sepal_length sepal_width petal_length petal_width species sampleid
120 6.0
2.2
5.0
1.5
3
2
19
5.7
3.8
1.7
0.3
1
1
59
6.6
2.9
4.6
1.3
2
1
61
5.0
2.0
3.5
1.0
2
1
78
6.7
3.0
5.0
1.7
2
2
101 6.3
3.3
6.0
2.5
3
1
141 6.7
3.1
5.6
2.4
3
1
17
5.4
3.9
1.3
0.4
1
1
38
4.9
3.6
1.4
0.1
1
1
122 5.6
2.8
4.9
2.0
3
1
In [4]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[4]:
id
sepal_length sepal_width petal_length petal_width species
120 6.0
2.2
5.0
1.5
3
99
5.1
2.5
3.0
1.1
2
137 6.3
3.4
5.6
2.4
3
38
4.9
3.6
1.4
0.1
1
93
5.8
2.6
4.0
1.2
2
141 6.7
3.1
5.6
2.4
3
17
5.4
3.9
1.3
0.4
1
34
5.5
4.2
1.4
0.2
1
78
6.7
3.0
5.0
1.7
2
19
5.7
3.8
1.7
0.3
1
In [5]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
150 5.9
3.0
5.1
1.8
3
53
6.9
3.1
4.9
1.5
2
129 6.4
2.8
5.6
2.1
3
93
5.8
2.6
4.0
1.2
2
41
5.0
3.5
1.3
0.3
1
34
5.5
4.2
1.4
0.2
1
89
5.6
3.0
4.1
1.3
2
66
6.7
3.1
4.4
1.4
2
133 6.4
2.8
5.6
2.2
3
59
6.6
2.9
4.6
1.3
2
Train Naive Bayes Model.¶
In [6]:
# Import required libraries.
import numpy as np
from sklearn import tree
from nyoka import skl_to_pmml
from sklearn.naive_bayes import GaussianNB
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
In [7]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = iris_train.to_pandas()
features = traid_pd.columns.drop('species')
target = 'species'
In [8]:
# Generate the NaiveBayes model.
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
rf_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['sepal_length', 'sepal_width'], StandardScaler()) ,
    (['petal_length', 'petal_width'], imputer)
    ])),
    ("nb", GaussianNB())
])
In [9]:
rf_pipe_obj.fit(traid_pd[features], traid_pd[target])
Out[9]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['sepal_length', 'sepal_width'],
                                            StandardScaler()),
                                           (['petal_length', 'petal_width'],
                                            SimpleImputer())])),
                ('nb', GaussianNB())])
Save the model in PMML format.¶
In [10]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_naive_bayes_model.pmml"
In [11]:
skl_to_pmml(rf_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [12]:
# Save the PMML Model in Vantage.
save_byom("pmml_naive_bayes_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [13]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                          model
model_id                                       
pmml_naive_bayes_iris  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [14]:
# Retrieve the model from table "byom_models", using the model id 'pmml_naive_bayes_iris'.
modeldata = retrieve_byom("pmml_naive_bayes_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [15]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [16]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    )
In [17]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__163422007125027" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__163423115194366") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [18]:
# Print the result.
result.result
Out[18]:
id
sepal_length petal_length prediction
json_report
9
4.4
1.4
1
{"probability_1":0.9999999721716369,"predicted_species":1,"probability_2":2.3209008397725517E-
8,"probability_3":4.619354729679975E-9}
70
5.6
3.9
2
{"probability_1":2.3814702218341936E-
8,"predicted_species":2,"probability_2":0.9999848619358067,"probability_3":1.5114249491108098E-5}
129 6.4
5.6
3
{"probability_1":4.04434847563354E-10,"predicted_species":3,"probability_2":1.837750035836196E-
5,"probability_3":0.9999816220952069}
120 6.0
5.0
2
{"probability_1":1.2477535746246755E-
9,"predicted_species":2,"probability_2":0.8929094896089551,"probability_3":0.10709050914329131}
118 7.7
6.7
3
{"probability_1":1.0696475582317612E-6,"predicted_species":3,"probability_2":2.1581490417633585E-
8,"probability_3":0.9999989087709513}
61
5.0
3.5
2
{"probability_1":1.5052909358122593E-
6,"predicted_species":2,"probability_2":0.9999976122771232,"probability_3":8.824319408796608E-7}
133 6.4
5.6
3
{"probability_1":4.99491491160099E-10,"predicted_species":3,"probability_2":2.2696869750709392E-
5,"probability_3":0.9999773026307578}
108 7.3
6.3
3
{"probability_1":3.0465249398372477E-9,"predicted_species":3,"probability_2":2.2475949876024456E-
5,"probability_3":0.999977521003599}
57
6.3
4.7
2
{"probability_1":2.2025345848104816E-
8,"predicted_species":2,"probability_2":0.5347366136162515,"probability_3":0.46526336435840276}
19
5.7
1.7
1
{"probability_1":0.9999999535947256,"predicted_species":1,"probability_2":8.396162719709246E-
9,"probability_3":3.800911159692161E-8}
Cleanup.¶
In [19]:
# Delete the model from table "byom_models", using the model id 'pmml_naive_bayes_iris'.
delete_byom("pmml_naive_bayes_iris", "byom_models")
Model is deleted.
In [20]:
# Drop models table.
db_drop_table("byom_models")
Out[20]:
True
In [21]:
# Drop input data tables.
db_drop_table("iris_input")
Out[21]:
True
In [22]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[22]:
True
Random Forest
PMMLPredict() using Random Forest model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, delete_byom, retrieve_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
con = create_context(host=getpass.getpass("Hostname: "), 
                     username=getpass.getpass("Username: "),
                     password=getpass.getpass("Password: "))
Hostname: ········
Username: ········
Password: ········
Load example data.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
Out[3]:
id
sepal_length sepal_width petal_length petal_width species sampleid
59
6.6
2.9
4.6
1.3
2
1
80
5.7
2.6
3.5
1.0
2
1
120 6.0
2.2
5.0
1.5
3
1
101 6.3
3.3
6.0
2.5
3
1
17
5.4
3.9
1.3
0.4
1
1
61
5.0
2.0
3.5
1.0
2
1
38
4.9
3.6
1.4
0.1
1
2
78
6.7
3.0
5.0
1.7
2
2
141 6.7
3.1
5.6
2.4
3
1
40
5.1
3.4
1.5
0.2
1
1
In [4]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[4]:
id
sepal_length sepal_width petal_length petal_width species
97
5.7
2.9
4.2
1.3
2
120 6.0
2.2
5.0
1.5
3
118 7.7
3.8
6.7
2.2
3
101 6.3
3.3
6.0
2.5
3
139 6.0
3.0
4.8
1.8
3
61
5.0
2.0
3.5
1.0
2
38
4.9
3.6
1.4
0.1
1
78
6.7
3.0
5.0
1.7
2
17
5.4
3.9
1.3
0.4
1
40
5.1
3.4
1.5
0.2
1
In [5]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
117 6.5
3.0
5.5
1.8
3
57
6.3
3.3
4.7
1.6
2
110 7.2
3.6
6.1
2.5
3
141 6.7
3.1
5.6
2.4
3
49
5.3
3.7
1.5
0.2
1
76
6.6
3.0
4.4
1.4
2
108 7.3
2.9
6.3
1.8
3
26
5.0
3.0
1.6
0.2
1
32
5.4
3.4
1.5
0.4
1
80
5.7
2.6
3.5
1.0
2
Train Random Forest Model.¶
In [6]:
# Import required libraries.
import numpy as np
from sklearn import tree
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
In [7]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = iris_train.to_pandas()
features = traid_pd.columns.drop('species')
target = 'species'
In [8]:
# Generate the Random forest model.
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
rf_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['sepal_length', 'sepal_width'], StandardScaler()) ,
    (['petal_length', 'petal_width'], imputer)
    ])),
    ("rfc", RandomForestClassifier(n_estimators = 100))
])
In [9]:
rf_pipe_obj.fit(traid_pd[features], traid_pd[target])
Out[9]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['sepal_length', 'sepal_width'],
                                            StandardScaler()),
                                           (['petal_length', 'petal_width'],
                                            SimpleImputer())])),
                ('rfc', RandomForestClassifier())])
Save the model in PMML format.¶
In [10]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_rf_class_model.pmml"
In [11]:
skl_to_pmml(rf_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [12]:
# Save the PMML Model in Vantage.
save_byom("pmml_random_forest_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [13]:
# List the PMML Model in Vantage.
list_byom("byom_models")
                                            model
model_id                                         
pmml_random_forest_iris  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [14]:
# Retrieve the model from table "byom_models", using the model id 'pmml_random_forest_iris'.
modeldata = retrieve_byom("pmml_random_forest_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [15]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [16]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    )
In [17]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__163423868255528" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__163423223025197") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [18]:
# Print the result.
result.result
Out[18]:
id
sepal_length petal_length prediction
json_report
30
4.7
1.6
1
{"probability_1":1.0,"predicted_species":1,"probability_2":0.0,"probability_3":0.0}
57
6.3
4.7
2
{"probability_1":0.0,"predicted_species":2,"probability_2":0.9,"probability_3":0.1}
118 7.7
6.7
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
34
5.5
1.4
1
{"probability_1":1.0,"predicted_species":1,"probability_2":0.0,"probability_3":0.0}
87
6.7
4.7
2
{"probability_1":0.0,"predicted_species":2,"probability_2":0.99,"probability_3":0.01}
148 6.5
5.2
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.0,"probability_3":1.0}
43
4.4
1.3
1
{"probability_1":1.0,"predicted_species":1,"probability_2":0.0,"probability_3":0.0}
56
5.7
4.5
2
{"probability_1":0.0,"predicted_species":2,"probability_2":1.0,"probability_3":0.0}
9
4.4
1.4
1
{"probability_1":0.99,"predicted_species":1,"probability_2":0.01,"probability_3":0.0}
80
5.7
3.5
2
{"probability_1":0.0,"predicted_species":2,"probability_2":1.0,"probability_3":0.0}
Cleanup.¶
In [19]:
# Delete the model from table "byom_models", using the model id 'pmml_random_forest_iris'.
delete_byom("pmml_random_forest_iris", "byom_models")
Model is deleted.
In [20]:
# Drop models table.
db_drop_table("byom_models")
Out[20]:
True
In [21]:
# Drop input data tables. 
db_drop_table("iris_input")
Out[21]:
True
In [22]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[22]:
True
SVC
PMMLPredict() with Vector Machine model using classiﬁcation¶
Setup¶
In [1]:
# Import required libraries
import getpass
import tempfile
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, retrieve_byom, delete_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load example data and use sample() for splitting input data into testing and training dataset.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
In [4]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows.
iris_sample = iris_input.sample(frac=[0.8, 0.2])
In [5]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid", axis = 1)
iris_train
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
139 6.0
3.0
4.8
1.8
3
38
4.9
3.6
1.4
0.1
1
78
6.7
3.0
5.0
1.7
2
19
5.7
3.8
1.7
0.3
1
36
5.0
3.2
1.2
0.2
1
40
5.1
3.4
1.5
0.2
1
80
5.7
2.6
3.5
1.0
2
118 7.7
3.8
6.7
2.2
3
99
5.1
2.5
3.0
1.1
2
61
5.0
2.0
3.5
1.0
2
In [6]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid", axis = 1)
iris_test
Out[6]:
id
sepal_length sepal_width petal_length petal_width species
87
6.7
3.1
4.7
1.5
2
18
5.1
3.5
1.4
0.3
1
98
6.2
2.9
4.3
1.3
2
122 5.6
2.8
4.9
2.0
3
70
5.6
2.5
3.9
1.1
2
40
5.1
3.4
1.5
0.2
1
35
4.9
3.1
1.5
0.2
1
54
5.5
2.3
4.0
1.3
2
30
4.7
3.2
1.6
0.2
1
133 6.4
2.8
5.6
2.2
3
Train SVC model¶
In [7]:
# Import required libraries.
import numpy as np
from sklearn import tree
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC
In [8]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = iris_train.to_pandas()
features = traid_pd.columns.drop('species')
target = 'species'
In [9]:
# Generate the SVC model
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
svc_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['sepal_length', 'sepal_width'], StandardScaler()) ,
    (['petal_length', 'petal_width'], imputer)
    ])),
    ("svc", SVC(gamma='auto',kernel='linear'))
])
In [10]:
svc_pipe_obj.fit(traid_pd[features], traid_pd[target])
Out[10]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['sepal_length', 'sepal_width'],
                                            StandardScaler()),
                                           (['petal_length', 'petal_width'],
                                            SimpleImputer())])),
                ('svc', SVC(gamma='auto', kernel='linear'))])
Save the model in PMML format.¶
In [11]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_svc_model.pmml"
In [12]:
skl_to_pmml(svc_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [13]:
# Save the PMML Model in Vantage.
save_byom("pmml_svc_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the model from Vantage.¶
In [14]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                  model
model_id                               
pmml_svc_iris  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [15]:
# Retrieve the model from table "byom_models", using the model id 'pmml_svc_iris'.
modeldata = retrieve_byom("pmml_svc_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [16]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [17]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = iris_test,
                    accumulate = ['id', 'sepal_length', 'petal_length'],
                    overwrite_cached_models = '*',
                    )
In [18]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__1644981761551165" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1644981845854836") AS ModelTable
DIMENSION
USING
Accumulate('id','sepal_length','petal_length')
OverwriteCachedModel('*')
) as sqlmr
In [19]:
# Print the result.
result.result
Out[19]:
id
sepal_length petal_length prediction
json_report
131 7.4
6.1
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.3333333333333333,"probability_3":0.6666666666666666}
34
5.5
1.4
1
{"probability_1":0.6666666666666666,"predicted_species":1,"probability_2":0.3333333333333333,"probability_3":0.0}
28
5.2
1.5
1
{"probability_1":0.6666666666666666,"predicted_species":1,"probability_2":0.3333333333333333,"probability_3":0.0}
40
5.1
1.5
1
{"probability_1":0.6666666666666666,"predicted_species":1,"probability_2":0.3333333333333333,"probability_3":0.0}
55
6.5
4.6
2
{"probability_1":0.0,"predicted_species":2,"probability_2":0.6666666666666666,"probability_3":0.3333333333333333}
53
6.9
4.9
2
{"probability_1":0.0,"predicted_species":2,"probability_2":0.6666666666666666,"probability_3":0.3333333333333333}
30
4.7
1.6
1
{"probability_1":0.6666666666666666,"predicted_species":1,"probability_2":0.3333333333333333,"probability_3":0.0}
70
5.6
3.9
2
{"probability_1":0.0,"predicted_species":2,"probability_2":0.6666666666666666,"probability_3":0.3333333333333333}
120 6.0
5.0
3
{"probability_1":0.0,"predicted_species":3,"probability_2":0.3333333333333333,"probability_3":0.6666666666666666}
17
5.4
1.3
1
{"probability_1":0.6666666666666666,"predicted_species":1,"probability_2":0.3333333333333333,"probability_3":0.0}
Cleanup.¶
In [20]:
# Delete the saved Model.
delete_byom("pmml_svc_iris", table_name="byom_models")
Model is deleted.
In [21]:
# Drop model table.
db_drop_table("byom_models")
Out[21]:
True
In [22]:
# Drop input data table.
db_drop_table("iris_input")
Out[22]:
True
In [23]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[23]:
True
In [ ]:
SVR
PMMLPredict() with Support Vector Machine model using Regression¶
Setup.¶
In [1]:
# Import required libraries
import getpass
import tempfile
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, delete_byom, retrieve_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
host = getpass.getpass("Host: ")
username = getpass.getpass("Username: ")
password = getpass.getpass("Password: ")
con = create_context(host=host, username=username, password=password)
Host: ········
Username: ········
Password: ········
Load Example Data.¶
In [3]:
load_example_data("decisionforest", "boston")
WARNING: Skipped loading table boston since it already exists in the database.
In [4]:
boston_input = DataFrame("boston")
In [5]:
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows.
boston_sample = boston_input.sample(frac=[0.8, 0.2])
In [6]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
boston_train = boston_sample[boston_sample.sampleid == "1"].drop("sampleid", axis = 1)
boston_train
Out[6]:
id
crim
zn indus chas nox
rm
age
dis
rad tax ptratio black
lstat medv
448 9.92485 0
18.1
0
0.74
6.251 96
2.198
24 666 20.2
388.52 16.44 12.6
223 0.62356 0
6.2
1
0.507 6.879 77
3.2721 8
307 17.4
390.39 9.93
27.5
488 4.83567 0
18.1
0
0.583 5.905 53
3.1523 24 666 20.2
388.22 11.45 20.6
305 0.05515 33 2.18
0
0.472 7.236 41
4.022
7
222 18.4
393.68 6.93
36.1
427 12.2472 0
18.1
0
0.584 5.837 59
1.9976 24 666 20.2
24.65
15.69 10.2
469 15.5757 0
18.1
0
0.58
5.926 71
2.9084 24 666 20.2
368.74 18.13 19.1
61
0.14932 25 5.13
0
0.453 5.741 66
7.2254 8
284 19.7
395.11 13.15 18.7
326 0.19186 0
7.38
0
0.493 6.431 14
5.4159 5
287 19.6
393.68 5.08
24.6
101 0.14866 0
8.56
0
0.52
6.727 79
2.7778 5
384 20.9
394.76 9.42
27.5
40
0.02763 75 2.95
0
0.428 6.595 21
5.4011 3
252 18.3
395.63 4.32
30.8
In [7]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
boston_test = boston_sample[boston_sample.sampleid == "2"].drop("sampleid", axis = 1)
boston_test
Out[7]:
id
crim
zn indus chas
nox
rm
age
dis
rad tax ptratio black
lstat medv
160 1.42502 0
19.58 0
0.871
6.51
100 1.7659 5
403 14.7
364.31 7.39
23.3
57
0.02055 85 0.74
0
0.41
6.383 35
9.1876 2
313 17.3
396.9
5.77
24.7
118 0.15098 0
10.01 0
0.547
6.021 82
2.7474 6
432 17.8
394.51 10.3
19.2
282 0.03705 20 3.33
0
0.4429 6.968 37
5.2447 5
216 14.9
392.23 4.59
35.4
299 0.06466 70 2.24
0
0.4
6.345 20
7.8278 5
358 14.8
368.24 4.97
22.5
467 3.77498 0
18.1
0
0.655
5.952 84
2.8715 24 666 20.2
22.01
17.15 19.0
221 0.35809 0
6.2
1
0.507
6.951 88
2.8617 8
307 17.4
391.7
9.71
26.7
116 0.17134 0
10.01 0
0.547
5.928 88
2.4631 6
432 17.8
344.91 15.76 18.3
34
1.15172 0
8.14
0
0.538
5.701 95
3.7872 4
307 21.0
358.77 18.35 13.1
162 1.46336 0
19.58 0
0.605
7.489 90
1.9709 5
403 14.7
374.43 1.73
50.0
Train SVM Regression Model¶
In [8]:
# Import required libraries.
import numpy as np
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn.svm import SVR
In [9]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
train_pd = boston_train.to_pandas()
features = train_pd.columns.drop('medv')
target = 'medv'
In [10]:
# Generate the SVM Regression model.
regr = Pipeline([
    ("svr", SVR(kernel= 'linear'))
                ])
In [11]:
regr.fit(train_pd[features], train_pd[target])
Out[11]:
Pipeline(steps=[('svr', SVR(kernel='linear'))])
Save the model in PMML format.¶
In [12]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/boston_db_svr_model.pmml"
In [13]:
skl_to_pmml(regr, features, target, model_file_path)
Save the model in Vantage.¶
In [14]:
# Save the PMML Model in Vantage.
save_byom("pmml_svr_boston", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vanatge¶
In [15]:
# List the PMML Models in Vantage.
list_byom("byom_models")
                                    model
model_id                                 
pmml_svr_boston  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [16]:
# Retrieve the model from table "byom_models", using the model id 'pmml_svr_boston'.
modeldata = retrieve_byom("pmml_svr_boston", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [17]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········
Score the model.¶
In [18]:
# Perform prediction using PMMLPredict() and the PMML model stored in Vantage.
result = PMMLPredict(
                    modeldata = modeldata,
                    newdata = boston_test,
                    accumulate = ['id', 'rm', 'lstat','ptratio'],
                    overwrite_cached_models = '*',
                    )
In [19]:
# Print the query.
print(result.show_query())
SELECT * FROM "mldb".PMMLPredict(
ON "MLDB"."ml__select__1645517679699217" AS InputTable
PARTITION BY ANY 
ON (select model_id,model from "MLDB"."ml__filter__1645521215241325") AS ModelTable
DIMENSION
USING
Accumulate('id','rm','lstat','ptratio')
OverwriteCachedModel('*')
) as sqlmr
In [20]:
# Print the result.
result.result
Out[20]:
id
rm
lstat ptratio
prediction
json_report
467 5.952 17.15 20.2
11.312941660878282 {"predicted_medv":11.312941660878282}
322 6.376 6.87
19.6
24.24203555205095
{"predicted_medv":24.24203555205095}
301 6.871 6.07
14.8
29.780764973146873 {"predicted_medv":29.780764973146873}
406 5.683 22.98 20.2
9.163877780148328
{"predicted_medv":9.163877780148328}
278 6.826 4.16
17.6
32.04837204324504
{"predicted_medv":32.04837204324504}
160 6.51
7.39
14.7
26.214879231825037 {"predicted_medv":26.214879231825037}
465 6.209 13.22 20.2
19.157576835196963 {"predicted_medv":19.157576835196963}
177 6.02
10.11 16.6
24.57108833740422
{"predicted_medv":24.57108833740422}
343 6.54
8.65
15.9
24.04016286752376
{"predicted_medv":24.04016286752376}
282 6.968 4.59
14.9
33.103048511903765 {"predicted_medv":33.103048511903765}
Cleanup.¶
In [21]:
# Delete the model from table "byom_models", using the model id 'pmml_svr_boston'.
delete_byom("pmml_svr_boston", "byom_models")
Model is deleted.
In [22]:
# Drop models table.
db_drop_table("byom_models")
Out[22]:
True
In [24]:
# Drop input data tables. 
db_drop_table("boston")
Out[24]:
True
In [25]:
# One must run remove_context() to close the connection and garbage collect internally generated objects.
remove_context()
Out[25]:
True
In [ ]:
XGBoost
PMMLPredict() using XGBoost model.¶
Setup¶
In [1]:
# Import required libraries
import tempfile
import getpass
from teradataml.dataframe.sql_functions import case
from teradataml import PMMLPredict, DataFrame, load_example_data, create_context, \
db_drop_table, remove_context, save_byom, delete_byom, retrieve_byom, list_byom
from teradataml.options.configure import configure
In [2]:
# Create the connection.
con = create_context(host=getpass.getpass("Hostname: "), 
                     username=getpass.getpass("Username: "),
                     password=getpass.getpass("Password: "))
Hostname: ········
Username: ········
Password: ········
Load example data.¶
In [3]:
# Load the example data.
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
# convert the "species" column to 0, 1, 2.
iris_input_df = iris_input.assign(species = case([(iris_input.species == 1, 0),
                                                (iris_input.species == 2, 1),
                                                (iris_input.species == 3, 2)],
                                                else_=0))
# Create 2 samples of input data - sample 1 will have 80% of total rows and sample 2 will have 20% of total rows. 
iris_sample = iris_input_df.sample(frac=[0.8, 0.2])
iris_sample
Out[3]:
id
sepal_length sepal_width petal_length petal_width species sampleid
120 6.0
2.2
5.0
1.5
2
1
19
5.7
3.8
1.7
0.3
0
1
59
6.6
2.9
4.6
1.3
1
1
61
5.0
2.0
3.5
1.0
1
1
78
6.7
3.0
5.0
1.7
1
1
101 6.3
3.3
6.0
2.5
2
1
141 6.7
3.1
5.6
2.4
2
1
17
5.4
3.9
1.3
0.4
0
1
38
4.9
3.6
1.4
0.1
0
1
122 5.6
2.8
4.9
2.0
2
1
In [4]:
# Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is not required for training model.
iris_train = iris_sample[iris_sample.sampleid == "1"].drop(["sampleid"], axis = 1)
iris_train
Out[4]:
id
sepal_length sepal_width petal_length petal_width species
118 7.7
3.8
6.7
2.2
2
19
5.7
3.8
1.7
0.3
0
59
6.6
2.9
4.6
1.3
1
61
5.0
2.0
3.5
1.0
1
78
6.7
3.0
5.0
1.7
1
101 6.3
3.3
6.0
2.5
2
141 6.7
3.1
5.6
2.4
2
17
5.4
3.9
1.3
0.4
0
38
4.9
3.6
1.4
0.1
0
122 5.6
2.8
4.9
2.0
2
In [5]:
# Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not required for scoring.
iris_test = iris_sample[iris_sample.sampleid == "2"].drop(["sampleid"], axis = 1)
iris_test
Out[5]:
id
sepal_length sepal_width petal_length petal_width species
7
4.6
3.4
1.4
0.3
0
99
5.1
2.5
3.0
1.1
1
15
5.8
4.0
1.2
0.2
0
78
6.7
3.0
5.0
1.7
1
60
5.2
2.7
3.9
1.4
1
34
5.5
4.2
1.4
0.2
0
13
4.8
3.0
1.4
0.1
0
11
5.4
3.7
1.5
0.2
0
131 7.4
2.8
6.1
1.9
2
122 5.6
2.8
4.9
2.0
2
Train XGBoost Model.¶
In [6]:
# Import required libraries.
import numpy as np
from sklearn import tree
from nyoka import xgboost_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from xgboost import XGBClassifier
In [7]:
# Convert teradataml dataframe to pandas dataframe.
# features : Training data.
# target : Training targets.
traid_pd = iris_train.to_pandas(coerce_float=True)
features = traid_pd.columns.drop('species')
target = 'species'
In [8]:
#Generate the XGBoost model.
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
rf_pipe_obj = Pipeline([
    ("mapping", DataFrameMapper([
    (['sepal_length', 'sepal_width'], StandardScaler()) ,
    (['petal_length', 'petal_width'], imputer)
    ])),
    ("xgb", XGBClassifier(use_label_encoder=False, eval_metric='mlogloss'))
])
In [9]:
rf_pipe_obj.fit(traid_pd[features], traid_pd[target])
Out[9]:
Pipeline(steps=[('mapping',
                 DataFrameMapper(drop_cols=[],
                                 features=[(['sepal_length', 'sepal_width'],
                                            StandardScaler()),
                                           (['petal_length', 'petal_width'],
                                            SimpleImputer())])),
                ('xgb',
                 XGBClassifier(base_score=0.5, booster='gbtree',
                               colsample_bylevel=1, colsample_bynode=1,
                               colsample_bytree=1, eval_metric='mlogloss',
                               gamma=0, gpu_id=-1, importance_type='gain',
                               interac...ts='',
                               learning_rate=0.300000012, max_delta_step=0,
                               max_depth=6, min_child_weight=1, missing=nan,
                               monotone_constraints='()', n_estimators=100,
                               n_jobs=16, num_parallel_tree=1,
                               objective='multi:softprob', random_state=0,
                               reg_alpha=0, reg_lambda=1, scale_pos_weight=None,
                               subsample=1, tree_method='exact',
                               use_label_encoder=False, validate_parameters=1,
                               verbosity=None))])
Save the model in PMML format.¶
In [10]:
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_xgb_model.pmml"
In [11]:
xgboost_to_pmml(rf_pipe_obj, features, target, model_file_path)
Save the model in Vantage.¶
In [12]:
# Save the PMML Model in Vantage.
save_byom("pmml_xgboost_iris", model_file_path, "byom_models")
Created the model table 'byom_models' as it does not exist.
Model is saved.
List the models from Vantage.¶
In [13]:
# List the PMML Model in Vantage.
list_byom("byom_models")
                                      model
model_id                                   
pmml_xgboost_iris  b'3C3F786D6C20766572...'
Retrieve the model from Vantage.¶
In [14]:
# Retrieve the model from table "byom_models", using the model id 'pmml_random_forest_iris'.
modeldata = retrieve_byom("pmml_xgboost_iris", "byom_models")
Set "conﬁgure.byom_install_location" to the database where BYOM functions are installed.¶
In [15]:
configure.byom_install_location = getpass.getpass("byom_install_location: ")
byom_install_location: ········