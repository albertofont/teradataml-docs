# teradataml Database 16.20.xx, 17.00.xx, 17.05.xx Analytic Functions

Antiselect
teradataml.analytics.sqle.Antiselect = class Antiselect(builtins.object)
    Methods deﬁned here:
    __init__(self, data=None, exclude=None, data_order_column=None)
    DESCRIPTION:
        AntiSelect returns all columns except those specified in the  
        exclude argument.
        Note: This function is only available when teradataml is connected
              to Vantage 1.1 or later versions.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the name of the teradataml DataFrame.
        exclude:
            Required Argument.
            Specifies the names of the columns not to return.
            Types: str OR list of Strings (str)
        data_order_column:
            Optional Argument.
            Specifies Order By columns for data.
            Values to this argument can be provided as a list, if multiple 
            columns are used for ordering.
            Types: str OR list of Strings (str)
    RETURNS:
        Instance of Antiselect.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as AntiselectObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        load_example_data("antiselect", "antiselect_input")
        # Create teradataml DataFrame objects.
        antiselect_input = DataFrame.from_table("antiselect_input")
        # Example1 - Fetching all columns except the columns 'rowids' and
        # 'orderdate'.
        antiselect_out = Antiselect(data=antiselect_input,
                                    exclude=['rowids','orderdate'])
        # Print the results.
        print(antiselect_out.result)
    __repr__(self)
    Returns the string representation for a Antiselect class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    Attribution
    teradataml.analytics.sqle.Attribution = class Attribution(builtins.object)
    Methods deﬁned here:
    __init__(self, data=None, data_optional=None, conversion_data=None, excluding_data=None, optional_data=None, model1_type=None, model2_type=None,
    event_column=None, timestamp_column=None, window_size=None, data_partition_column=None, data_optional_partition_column=None, data_order_column=None,
    data_optional_order_column=None, conversion_data_order_column=None, excluding_data_order_column=None, optional_data_order_column=None,
    model1_type_order_column=None, model2_type_order_column=None)
    DESCRIPTION:
        The Attribution function is used in web page analysis, where it lets 
        companies assign weights to pages before certain events, such as 
        buying a product.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the teradataml DataFrame that contains the click stream data,
            which the function uses to compute attributions.
        data_partition_column:
            Required Argument.
            Specifies Partition By columns for data.
            Values to this argument can be provided as a list, if multiple
            columns are used for partition.
            Types: str OR list of Strings (str)
        data_order_column:
            Required Argument.
            Specifies Order By columns for data.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        data_optional:
            Optional Argument.
            Specifies the teradataml DataFrame that contains the click stream data,
            which the function uses to compute attributions.
        data_optional_partition_column:
            Optional Argument.
            Required if the data_optional teradataml DataFrame is used.
            Specifies Partition By columns for data_optional.
            Values to this argument can be provided as a list, if multiple
            columns are used for partition.
            Types: str OR list of Strings (str)
        data_optional_order_column:
            Optional Argument.
            Required if the data_optional teradataml DataFrame is used.
            Specifies Order By columns for data_optional.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        conversion_data:
            Required Argument.
            Specifies the teradataml DataFrame that contains one varchar column
            (conversion_events) containing conversion event values.
        conversion_data_order_column:
            Optional Argument.
            Specifies Order By columns for conversion_data.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        excluding_data:
            Optional Argument.
            Specifies the teradataml DataFrame that contains one varchar column
            (excluding_events) containing excluding cause event values.
        excluding_data_order_column:
            Optional Argument.
            Specifies Order By columns for excluding_data.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        optional_data:
            Optional Argument.
            Specifies the teradataml DataFrame that contains one varchar column
            (optional_events) containing optional cause event values.
        optional_data_order_column:
            Optional Argument.
            Specifies Order By columns for optional_data.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        model1_type:
            Required Argument.
            Specifies the teradataml DataFrame that defines the type and
            specification of the first model.
            For example:
                model1_data ("EVENT_REGULAR", "email:0.19:LAST_CLICK:NA",
                "impression:0.81:WEIGHTED:0.4,0.3,0.2,0.1")
        model1_type_order_column:
            Optional Argument.
            Specifies Order By columns for model1_type.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        model2_type:
            Optional Argument.
            Specifies the teradataml DataFrame that defines the type and
            distributions of the second model.
            For example:
                model2_data ("EVENT_OPTIONAL", "OrganicSearch:0.5:UNIFORM:NA",
                "Direct:0.3:UNIFORM:NA", "Referral:0.2:UNIFORM:NA")
        model2_type_order_column:
            Optional Argument.
            Specifies Order By columns for model2_type.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
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
            attribution calculation:
                rows:K :
                    Consider the maximum number of events to be attributed,
                    excluding events of types specified in excluding_data,
                    which means assigning attributions to at most K effective
                    events before the current impact event.
                seconds:K :
                    Consider the maximum time difference between the current
                    impact event and the earliest effective event to be attributed.
                rows:K&seconds:K2 :
                    Consider both constraints and comply with the stricter one.
            Types: str
    RETURNS:
        Instance of Attribution.
        Output teradataml DataFrames can be accessed using attribute
        references, such as AttributionObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
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
        # Execute function
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
                                      model2_type=model2_table
                                      )
        # Print the results
        print(attribution_out.result)
    __repr__(self)
    Returns the string representation for a Attribution class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    DecisionForestPredict
    teradataml.analytics.sqle.DecisionForestPredict = class DecisionForestPredict(builtins.object)
    Methods deﬁned here:
    __init__(self, object=None, newdata=None, id_column=None, detailed=False, terms=None, newdata_order_column=None, object_order_column=None)
    DESCRIPTION:
        The DecisionForestPredict function uses the model generated by
        the DecisionForest function to generate predictions on a response
        variable for a test set of data. The model can be stored in either
        a teradataml DataFrame or a DecisionForest object.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data
            or instance of DecisionForest.
        object_order_column:
            Optional Argument.
            Specifies Order By columns for object.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input test data.
        newdata_order_column:
            Optional Argument.
            Specifies Order By columns for newdata.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
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
    RETURNS:
        Instance of DecisionForestPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as DecisionForestPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example
        load_example_data("decisionforestpredict", ["housing_train","housing_test"])
        # Create teradataml DataFrame objects.
        housing_test = DataFrame.from_table("housing_test")
        housing_train = DataFrame.from_table("housing_train")
        # Example 1 -
        # First train the data, i.e., create a decision forest Model
        formula = "homestyle ~ driveway + recroom + fullbase + gashw + airco + prefarea + price + lotsize + bedrooms + bathrm
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
        # Run predict on the output of decision forest
        decision_forest_predict_out = DecisionForestPredict(object = rft_model,
                                                            newdata = housing_test,
                                                            id_column = "sn",
                                                            detailed = False,
                                                            terms = ["homestyle"]
                                                            )
        # Print the results
        print(decision_forest_predict_out.result)
    __repr__(self)
    Returns the string representation for a DecisionForestPredict class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    DecisionTreePredict
    teradataml.analytics.sqle.DecisionTreePredict = class DecisionTreePredict(builtins.object)
    Methods deﬁned here:
    __init__(self, object=None, newdata=None, attr_table_groupby_columns=None, attr_table_pid_columns=None, attr_table_val_column=None,
    output_response_probdist=False, accumulate=None, output_responses=None, newdata_partition_column=None, newdata_order_column=None,
    object_order_column=None)
    DESCRIPTION:
        The DecisionTreePredict function applies a tree model to a data
        input, outputting predicted labels for each data point.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the name of the teradataml DataFrame containing the output 
            model from DecisionTree or instance of DecisionTree.
        object_order_column:
            Optional Argument.
            Specifies Order By columns for object.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        newdata:
            Required Argument.
            Specifies the name of the teradataml DataFrame containing the 
            attribute names and the values.
        newdata_partition_column:
            Required Argument.
            Specifies Partition By columns for newdata.
            Values to this argument can be provided as a list, if multiple
            columns are used for partition.
            Types: str OR list of Strings (str)
        newdata_order_column:
            Optional Argument.
            Specifies Order By columns for newdata.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
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
            Note: 'output_response_probdist' argument can accept input value True
                  only when teradataml is connected to Vantage 1.0 Maintenance
                  Update 2 version or later.
            Default Value: False
            Types: bool
        accumulate:
            Optional Argument.
            Specifies the names of newdata columns to copy to the output
            teradataml DataFrame.
            Types: str OR list of Strings (str)
        output_responses:
            Optional Argument.
            Required if output_response_probdist is True.
            Specifies all responses in newdata.
            Types: str OR list of Strings (str)
    RETURNS:
        Instance of DecisionTreePredict.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DecisionTreePredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        load_example_data("DecisionTreePredict", ["iris_attribute_train",
                                                  "iris_response_train",
                                                  "iris_attribute_test"])
        # Create teradataml DataFrame.
        iris_attribute_test = DataFrame.from_table("iris_attribute_test")
        iris_attribute_train = DataFrame.from_table("iris_attribute_train")
        iris_response_train = DataFrame.from_table("iris_response_train")
        # Example 1 -
        # First train the data, i.e., create a decision tree Model
        td_decision_tree_out  = DecisionTree(attribute_name_columns = 'attribute',
                                             attribute_value_column = 'attrvalue',
                                             id_columns = 'pid',
                                             attribute_table = iris_attribute_train,
                                             response_table = iris_response_train,
                                             response_column = 'response',
                                             approx_splits = True,
                                             nodesize = 100,
                                             max_depth = 5,
                                             weighted = False,
                                             split_measure = "gini",
                                             output_response_probdist = False)
        # Run predict on the output of decision tree
        decision_tree_predict_out = DecisionTreePredict(newdata=iris_attribute_test,
                                                        newdata_partition_column='pid',
                                                        object=td_decision_tree_out,
                                                        attr_table_groupby_columns='attribute',
                                                        attr_table_pid_columns='pid',
                                                        attr_table_val_column='attrvalue',
                                                        accumulate='pid',
                                                        output_response_probdist=False,
                                                        output_responses=['pid','attribute'])
        # Print output dataframes
        print(decision_tree_predict_out.result)
    __repr__(self)
    Returns the string representation for a DecisionTreePredict class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    GLMPredict
    teradataml.analytics.sqle.GLMPredict = class GLMPredict(builtins.object)
    Methods deﬁned here:
    __init__(self, modeldata=None, newdata=None, terms=None, family=None, linkfunction='CANONICAL', newdata_order_column=None)
    DESCRIPTION:
        The GLMPredict function uses the model generated by the function GLM
        to perform generalized linear model prediction on new input data.
    PARAMETERS:
        modeldata:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data
            or an instance of GLM.
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input data.
        newdata_order_column:
            Optional Argument.
            Specifies Order By columns for newdata.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
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
            Note: Use the same value that you used for the Link argument of the
                  function  when you generated the model.
            Default Value: "CANONICAL"
            Permitted Values: CANONICAL, IDENTITY, INVERSE, LOG,
            COMPLEMENTARY_LOG_LOG, SQUARE_ROOT, INVERSE_MU_SQUARED, LOGIT,
            PROBIT, CAUCHIT
            Types: str
    RETURNS:
        Instance of GLMPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as GLMPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        load_example_data("GLMPredict", ["admissions_test","admissions_train","housing_test","housing_train"])
        # Create teradataml DataFrame objects.
        admissions_test = DataFrame.from_table("admissions_test")
        admissions_train = DataFrame.from_table("admissions_train")
        housing_test = DataFrame.from_table("housing_test")
        housing_train = DataFrame.from_table("housing_train")
        # Example 1 -
        # First train the data, i.e., create a GLM Model
        glm_out = GLM(formula = "admitted ~ stats + masters + gpa + programming",
                     family = "LOGISTIC",
                     linkfunction = "LOGIT",
                     data = admissions_train,
                     weights = "1",
                     threshold = 0.01,
                     maxit = 25,
                     step = False,
                     intercept = True
                     )
        # Run predict on the output of GLM
        glm_predict_out1 = GLMPredict(modeldata = glm_out,
                                      newdata = admissions_test,
                                      terms = ["id","masters","gpa","stats","programming","admitted"],
                                      family = "LOGISTIC",
                                      linkfunction = "LOGIT"
                                      )
        # Print the results.
        print(glm_predict_out1.result)
        # Example 2 -
        # First train the data, i.e., create a GLM Model
        glm_out_hs = GLM(formula = "price ~ recroom + lotsize + stories + garagepl + gashw + bedrooms + driveway + airco + ho
                          family = "GAUSSIAN",
                          linkfunction = "IDENTITY",
                          data = housing_train,
                          weights = "1",
                          threshold = 0.01,
                          maxit = 25,
                          step = False,
                          intercept = True
                          )
        # Run predict on the output of GLM by passing coefficients
        glm_predict_out2 = GLMPredict(modeldata = glm_out_hs.coefficients,
                                      newdata = housing_test,
                                      terms = ["sn", "price"],
                                      family = "GAUSSIAN",
                                      linkfunction = "CANONICAL"
                                      )
        # Print the results.
        print(glm_predict_out2)
    __repr__(self)
    Returns the string representation for a GLMPredict class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    MovingAverage
    teradataml.analytics.sqle.MovingAverage = class MovingAverage(builtins.object)
    Methods deﬁned here:
    __init__(self, data=None, target_columns=None, alpha=0.1, start_rows=2, window_size=10, include_ﬁrst=False, mavgtype='C', data_partition_column=None,
    data_order_column=None)
    DESCRIPTION:
        The MovingAverage function calculates the moving average of the 
        target columns based on the moving average types ("mvgtype").
        Possible moving average types:
            'C' Cumulative moving average.
            'E' Exponential moving average.
            'M' Modified moving average.
            'S' Simple moving average.
            'T' Triangular moving average.
            'W' Weighted moving average.
        Note: This function is only available when teradataml is connected
              to Vantage 1.1 or later versions.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the name of the teradataml DataFrame that contains the 
            columns.
        data_partition_column:
            Required Argument.
            Specifies Partition By columns for data.
            Values to this argument can be provided as a list, if multiple 
            columns are used for partition.
            Types: str OR list of Strings (str)
        data_order_column:
            Required Argument.
            Specifies Order By columns for data.
            Values to this argument can be provided as a list, if multiple 
            columns are used for ordering.
            Types: str OR list of Strings (str)
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
            older observations faster. Only used if "mavgtype" is E.
            For other moving average types this value will be ignored.
            Default Value: 0.1
            Types: float
        start_rows:
            Optional Argument.
            Specifies the number of rows at the beginning of the time series that 
            the function skips before it begins the calculation of the 
            exponential moving average. The function uses the arithmetic average 
            of these rows as the initial value of the exponential moving average.
            Only used if "mavgtype" is E. For other moving average types 
            this value will be ignored.
            Default Value: 2
            Types: int
        window_size:
            Optional Argument.
            Specifies the number of previous values to include in the computation 
            of the moving average if "mavgtype" is M, S, T, and W.
            For other moving average types this value will be ignored.
            Default Value: 10
            Types: int
        include_first:
            Optional Argument.
            Specifies whether the first START_ROWS rows should be included in the 
            output or not. Only used if "mavgtype" is S, M, W, E, T. 
            For cumulative moving average types this value will be ignored.
            Default Value: False
            Types: bool
        mavgtype:
            Optional Argument.
            Specify the moving average type that needs to be used for computing 
            moving averages of "target_columns".
            Following are the different type of averages calculated by MovingAverage function:
                S: The MovingAverage function computes the simple moving average of points in a
                   series.
                W: The MovingAverage function computes the weighted moving average the average of
                   points in a time series, applying weights to older values. The 
                   weights for the older values decrease arithmetically.
                E: The MovingAverage function computes the exponential moving average
                   of the points in a time series, exponentially decreasing the
                   weights of older values.
                C: The MovingAverage function computes the cumulative moving average of a value
                   from the beginning of a series.
                M: The MovingAverage function computes moving average of points in series.
                T: The MovingAverage function computes double-smoothed average of points in series.
            Default Value: "C"
            Permitted Values: C, S, M, W, E, T
            Types: str
    RETURNS:
        Instance of MovingAverage.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as MovingAverageObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        load_example_data("movavg", "ibm_stock")
        # Create teradataml DataFrame objects.
        ibm_stock = DataFrame.from_table("ibm_stock")
        # Example1 - Calculating the cumulative moving average for data in 
        # the stockprice column.
        movingaverage_cmavg =  MovingAverage(data=ibm_stock,
                                        data_order_column='name',
                                        data_partition_column='name',
                                        target_columns='stockprice',
                                        mavgtype='C'
                                        )
        # Print the results.
        print(movingaverage_cmavg.result)
        # Example2 - Calculating the exponential moving average for data in 
        # the stockprice column.
        movingaverage_emavg =  MovingAverage(data=ibm_stock,
                                        data_partition_column='name',
                                        data_order_column='name',
                                        target_columns='stockprice',
                                        include_first=False,
                                        alpha=0.1,
                                        start_rows=10,
                                        mavgtype='E'
                                        )
        # Print the results.
        print(movingaverage_emavg.result)
        # Example3 - Calculating the simple moving average for data in 
        # the stockprice column.
        movingaverage_smavg =  MovingAverage(data=ibm_stock,
                                        data_partition_column='name',
                                        data_order_column='name',
                                        target_columns='stockprice',
                                        include_first=False,
                                        window_size=6,
                                        mavgtype='S'
                                        )
        # Print the results.
        print(movingaverage_smavg.result)
    __repr__(self)
    Returns the string representation for a MovingAverage class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    NaiveBayesPredict
    teradataml.analytics.sqle.NaiveBayesPredict = class NaiveBayesPredict(builtins.object)
    Methods deﬁned here:
    __init__(self, modeldata=None, newdata=None, id_col=None, responses=None, formula=None, newdata_order_column=None, modeldata_order_column=None)
    DESCRIPTION:
        The NaiveBayesPredict function uses the model output generated by the
        NaiveBayes function to predict the outcomes for a test set of data.
    PARAMETERS:
        modeldata:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data
            or instance of NaiveBayes.
        modeldata_order_column:
            Optional Argument.
            Specifies Order By columns for modeldata.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input test data.
        newdata_order_column:
            Optional Argument.
            Specifies Order By columns for newdata.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
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
    RETURNS:
        Instance of NaiveBayesPredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as NaiveBayesPredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example
        load_example_data("NaiveBayesPredict",["nb_iris_input_test","nb_iris_input_train"])
        # Create teradataml DataFrame objects.
        nb_iris_input_train = DataFrame.from_table("nb_iris_input_train")
        nb_iris_input_test = DataFrame.from_table("nb_iris_input_test")
        # Example 1 -
        # Run the train function
        naivebayes_train = NaiveBayes(formula="species ~ petal_length + sepal_width + petal_width + sepal_length",
                                      data=nb_iris_input_train)
        # Generate prediction using output of train function
        naivebayes_predict_result = NaiveBayesPredict(newdata=nb_iris_input_test,
                                                      modeldata = naivebayes_train,
                                                      id_col = "id",
                                                      responses = ["virginica","setosa","versicolor"]
                                                      )
        # Print result dataframe
        print(naivebayes_predict_result.result)
    __repr__(self)
    Returns the string representation for a NaiveBayesPredict class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    NaiveBayesTextClassifierPredict
    teradataml.analytics.sqle.NaiveBayesTextClassiﬁerPredict = class NaiveBayesTextClassiﬁerPredict(builtins.object)
    Methods deﬁned here:
    __init__(self, object=None, newdata=None, input_token_column=None, doc_id_columns=None, model_type='MULTINOMIAL', top_k=None,
    model_token_column=None, model_category_column=None, model_prob_column=None, newdata_partition_column=None, newdata_order_column=None,
    object_order_column=None)
    DESCRIPTION:
        The NaiveBayesTextClassifierPredict function uses the model generated by the
        NaiveBayesTextClassifier function to predict the outcomes for a test set
        of data.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the model data
            or instance of NaiveBayesTextClassifier.
        object_order_column:
            Optional Argument.
            Specifies Order By columns for object.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input test
            data.
        newdata_partition_column:
            Required Argument.
            Specifies Partition By columns for newdata.
            Values to this argument can be provided as a list, if multiple
            columns are used for partition.
            Types: str OR list of Strings (str)
        newdata_order_column:
            Optional Argument.
            Specifies Order By columns for newdata.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
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
            Default Value: 'MULTINOMIAL'
            Permitted Values: 'MULTINOMIAL', 'BERNOULLI'
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
        # Load the data to run the example
        load_example_data("NaiveBayesTextClassifierPredict",["complaints_tokens_test","token_table"])
        # Create teradataml DataFrame.
        token_table = DataFrame("token_table")
        complaints_tokens_test = DataFrame("complaints_tokens_test")
        # Create a model which is output of  NaiveBayesTextClassifier
        nbt_out = NaiveBayesTextClassifier(data = token_table,
                                           token_column = 'token',
                                           doc_id_columns = 'doc_id',
                                           doc_category_column = 'category',
                                           model_type = "Bernoulli",
                                           data_partition_column = 'category')
        # Example 1 -
        nbt_predict_out = NaiveBayesTextClassifierPredict(object = nbt_out,
                                                          newdata = complaints_tokens_test,
                                                          input_token_column = 'token',
                                                          doc_id_columns = 'doc_id',
                                                          model_type = "Bernoulli",
                                                          model_token_column = 'token',
                                                          model_category_column = 'category',
                                                          model_prob_column = 'prob',
                                                          newdata_partition_column = 'doc_id')
        # Print the result DataFrame
        print(nbt_predict_out.result)
    __repr__(self)
    Returns the string representation for a NaiveBayesTextClassifierPredict class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    NGramSplitter
    teradataml.analytics.sqle.NGramSplitter = class NGramSplitter(builtins.object)
    Methods deﬁned here:
    __init__(self, data=None, text_column=None, delimiter='[\\s]+', grams=None, overlapping=True, to_lower_case=True, punctuation='`~#^&*()-', reset='.,?!',
    total_gram_count=False, total_count_column='totalcnt', accumulate=None, n_gram_column='ngram', num_grams_column='n', frequency_column='frequency',
    data_order_column=None)
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
        data_order_column:
            Optional Argument.
            Specifies Order By columns for data.
            Values to this argument can be provided as a list, if multiple columns are 
            used for ordering.
            Types: str OR list of Strings (str)
        text_column:
            Required Argument.
            Specifies the name of the column that contains the input text. The column 
            must have a SQL string data type.
            Types: str
        delimiter:
            Optional Argument.
            Specifies a character or string that separates words in the input text. The 
            default value is the set of all whitespace characters which includes 
            the characters for space, tab, newline, carriage return and some others.
            Default Value: "[\s]+"
            Types: str
        grams:
            Required Argument.
            Specifies a list of integers or ranges of integers that specify the length, in 
            words, of each n-gram (that is, the value of n). A range of values has 
            the syntax integer1 - integer2, where integer1 <= integer2. The values 
            of n, integer1, and integer2 must be positive.
            Types: str OR list of strs
        overlapping:
            Optional Argument.
            Specifies whether the function allows overlapping n-grams. 
            When this value is "True", each word in each sentence starts an n-gram, if 
            enough words follow it (in the same sentence) to form a whole n-gram of the 
            specified size. For information on sentences, see the description of the 
            reset argument.
            Default Value: True
            Types: bool
        to_lower_case:
            Optional Argument.
            Specifies whether the function converts all letters in the input text 
            to lowercase. 
            Default Value: True
            Types: bool
        punctuation:
            Optional Argument.
            Specifies the punctuation characters for the function to remove before 
            evaluating the input text.
            Default Value: "`~#^&*()-"
            Types: str
        reset:
            Optional Argument.
            Specifies the character or string that ends a sentence. 
            At the end of a sentence, the function discards any partial n-grams and 
            searches for the next n-gram at the beginning of the next sentence. 
            An n-gram cannot span two sentences.
            Default Value: ".,?!"
            Types: str
        total_gram_count:
            Optional Argument.
            Specifies whether the function returns the total number of n-grams in the 
            document (that is, in the row). If you specify "True", then the name of the 
            returned column is specified by the total_count_column argument. 
            Note: The total number of n-grams is not necessarily the number of unique n-grams.
            Default Value: False
            Types: bool
        total_count_column:
            Optional Argument.
            Specifies the name of the column to return if the value of the total_gram_count 
            argument is "True". 
            Default Value: "totalcnt"
            Types: str
        accumulate:
            Optional Argument.
            Specifies the names of the columns to return for each n-gram. These columns 
            cannot have the same names as those specified by the arguments n_gram_column, 
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
    RETURNS:
        Instance of NgramSplitter.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as NgramSplitterObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load example data.
        load_example_data("NGrams","paragraphs_input")
        # Create teradataml DataFrame
        paragraphs_input = DataFrame.from_table("paragraphs_input")
        # Example 1
        # Creates output for tokenized data on grams values
        NGramSplitter_out1 = NGramSplitter(data=paragraphs_input,
                            text_column='paratext',
                            delimiter = " ",
                            grams = "4-6",
                            overlapping=True,
                            to_lower_case=True,
                            total_gram_count=True,
                            accumulate=['paraid','paratopic']
                            )
        # Print the result DataFrame
        print(NGramSplitter_out1.result)
        # Example 2
        # Creates total count column with default column totalcnt if total_gram_count is specified as False
        NGramSplitter_out2 = NGramSplitter(data = paragraphs_input,
                            text_column='paratext',
                            delimiter = " ",
                            grams = "4-6",
                            overlapping=False,
                            to_lower_case=True,
                            total_gram_count=False,
                            accumulate=['paraid','paratopic']
                           )
        # Print the result DataFrame
        print(NGramSplitter_out2.result)
    __repr__(self)
    Returns the string representation for a NgramSplitter class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    NPath
    teradataml.analytics.sqle.NPath = class NPath(builtins.object)
    Methods deﬁned here:
    __init__(self, data1=None, mode=None, pattern=None, symbols=None, result=None, ﬁlter=None, data2=None, data3=None, data1_partition_column=None,
    data2_partition_column=None, data3_partition_column=None, data1_order_column=None, data2_order_column=None, data3_order_column=None)
    DESCRIPTION:
        The nPath function scans a set of rows, looking for patterns that you
        specify. For each set of input rows that matches the pattern, nPath
        produces a single output row. The function provides a flexible
        pattern-matching capability that lets you specify complex patterns in
        the input data and define the values that are output for each matched
        input set.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the input teradataml DataFrame containing the input data set.
        data1_partition_column:
            Required Argument.
            Specifies Partition By columns for data1.
            Values to this argument can be provided as a list, if multiple
            columns are used for partition.
            Types: str OR list of Strings (str)
        data1_order_column:
            Required Argument.
            Specifies Order By columns for data1.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        mode:
            Required Argument.
            Specifies the pattern-matching mode:
                OVERLAPPING: The function finds every occurrence of the pattern in
                             the partition, regardless of whether it is part of a previously
                             found match. Therefore, one row can match multiple symbols in a
                             given matched pattern.
                NONOVERLAPPING: The function begins the next pattern search at the
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
            Defines the symbols that appear in the values of the pattern and
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
            Defines the output columns. The col_expr is an expression whose value
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
            Optional Argument.
            Specifies the additional optional input teradataml DataFrame containing the input data set.
        data2_partition_column:
            Optional Argument.
            Specifies Partition By columns for data2.
            Values to this argument can be provided as a list, if multiple
            columns are used for partition.
            Types: str OR list of Strings (str)
        data2_order_column:
            Optional Argument.
            Required when data2 teradataml DataFrame is used.
            Specifies Order By columns for data2.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        data3:
            Optional Argument.
            Specifies the additional optional input teradataml DataFrame containing the input data set.
        data3_partition_column:
            Optional Argument.
            Specifies Partition By columns for data3.
            Values to this argument can be provided as a list, if multiple
            columns are used for partition.
            Types: str OR list of Strings (str)
        data3_order_column:
            Optional Argument.
            Required when data3 teradataml DataFrame is used.
            Specifies Order By columns for data3.
            Values to this argument can be provided as a list, if multiple 
            columns are used for ordering.
            Types: str OR list of Strings (str)
    RETURNS:
        Instance of NPath.
        Output teradataml DataFrames can be accessed using attribute
        references, such as NPathObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load example data.
        load_example_data("NPath",["impressions","clicks2", "tv_spots", "clickstream"])
        # Create input teradataml dataframes.
        impressions = DataFrame.from_table("impressions")
        clicks2 = DataFrame.from_table("clicks2")
        tv_spots = DataFrame.from_table("tv_spots")
        clickstream = DataFrame.from_table("clickstream")
        # Example1:
        # We will try to search for pattern '(imp|tv_imp)*.click'
        # in the provided data sets(imressions, clicks2, tv_spots).
        # Run NPath function with the required patterns to get the rows which
        # has specified pattern. rows that matches the pattern.
        result = NPath(data1=impressions,
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
        # Print the result dataframe.
        print(result.result)
        # Example2:
        # We will try to search for pattern 'home.clickview*.checkout'
        # in the provided data set clickstream.
        # Run NPath function with the required patterns to get the rows which
        # has specified pattern and filter the rows with the filter,
        # where filter and result have ML Engine nPath sequence aggregate functions
        # like 'FIRST', 'COUNT' and 'LAST'
        result = NPath(data1=clickstream,
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
                                "FIRST (clicktime of any(checkout))"
                       )
        # Print the result dataframe.
        print(result.result)
    __repr__(self)
    Returns the string representation for a NPath class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    Pack
    teradataml.analytics.sqle.Pack = class Pack(builtins.object)
    Methods deﬁned here:
    __init__(self, data=None, input_columns=None, output_column=None, delimiter=',', include_column_name=True, data_order_column=None)
    DESCRIPTION:
        The Pack function packs data from multiple input columns into a 
        single column. The packed column has a virtual column for each input 
        column. By default, virtual columns are separated by commas and each 
        virtual column value is labeled with its column name.
        Note: This function is only available when teradataml is connected
              to Vantage 1.1 or later versions.
    PARAMETERS:
         data:
            Required Argument.
            Specifies the teradataml DataFrame containing the input attributes.
        data_order_column:
            Optional Argument.
            Specifies Order By columns for data.
            Values to this argument can be provided as a list, if multiple 
            columns are used for ordering.
            Types: str OR list of Strings (str)
        input_columns:
            Optional Argument.
            Specifies the names of the input columns to pack into a single output 
            column. These names become the column names of the virtual columns. 
            By default, all input teradataml DataFrame columns are packed into a 
            single output column. If you specify this argument, but do not 
            specify all input teradataml DataFrame columns, the function copies 
            the unspecified input teradataml DataFrame columns to the output 
            teradataml DataFrame.
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
    RETURNS:
        Instance of Pack.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as PackObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        load_example_data("Pack", "ville_temperature")
        # Create teradataml DataFrame objects.
        # The input, ville_temperature contains temperature readings for the 
        # cities Nashville and Knoxville, in the state of Tennessee.
        ville_temperature = DataFrame.from_table("ville_temperature")
        # Example1 - Default Argument Values.
        # Default values used for arguments "delimiter" and "include_column_name".
        pack_out1 = Pack(data=ville_temperature,
                        input_columns=['city','state','period','temp_f'],
                        output_column='packed_data',
                        delimiter=',',
                        include_column_name=True)
        # Print the results
        print(pack_out1.result)
        # Example2 - Nondefault Argument Values.
        # This example uses nondefault values for arguments "delimiter" and "include_column_name".
        pack_out2 = Pack(data=ville_temperature,
                        input_columns=['city','state','period','temp_f'],
                        output_column='packed_data',
                        delimiter='|',
                        include_column_name=False)
        # Print the results
        print(pack_out2)
    __repr__(self)
    Returns the string representation for a Pack class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    Sessionize
    teradataml.analytics.sqle.Sessionize = class Sessionize(builtins.object)
    Methods deﬁned here:
    __init__(self, data=None, time_column=None, time_out=None, click_lag=None, emit_null=False, data_partition_column=None, data_order_column=None)
    DESCRIPTION:
        The Sessionize function maps each click in a session to a unique 
        session identifier. A session is defined as a sequence of clicks by 
        one user that are separated by at most n seconds.
        The function is useful both for sessionization and for detecting web 
        crawler (bot) activity. It is typically used to understand user browsing
        behavior on a web site.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
        data_partition_column:
            Required Argument.
            Specifies Partition By columns for data.
            Values to this argument can be provided as a list, if multiple columns 
            are used for partition.
            Types: str OR list of Strings (str)
        data_order_column:
            Required Argument.
            Specifies Order By columns for data.
            Values to this argument can be provided as a list, if multiple columns 
            are used for ordering.
            Types: str OR list of Strings (str)
        time_column:
            Required Argument.
            Specifies the name of the input column that contains the click
            times.
            Note: The time_column must also be an data_order_column.
            Types: str
        time_out:
            Required Argument.
            Specifies the number of seconds at which the session times out. If 
            time_out seconds elapse after a click, then the next click
            starts a new session.
            Types: float
        click_lag:
            Optional Argument.
            Specifies the minimum number of seconds between clicks for the 
            session user to be considered human. If clicks are more frequent, 
            indicating that the user is a "bot," the function ignores the 
            session. The click_lag must be less than time_out.
            Types: float
        emit_null:
            Optional Argument.
            Specifies whether to output rows that have None values in their 
            session id and rapid fire columns, even if their time_column has 
            a None value. 
            Default Value: False
            Types: bool
    RETURNS:
        Instance of Sessionize.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SessionizeObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        load_example_data("Sessionize","sessionize_table")
        # Create teradataml DataFram object.
        sessionize_table = DataFrame.from_table("sessionize_table")
        # Example 1 - This example maps each click in a session to a unique session identifer, 
        # which uses input table web clickstream data recorded as user navigates through a web site
        # based on events — view, click, and so on which are recorded with a timestamp.
        td_sessionize_out = Sessionize(data = sessionize_table,
                                       data_partition_column = ["partition_id"],
                                       data_order_column = ["clicktime"],
                                       time_column = "clicktime",
                                       time_out = 60.0,
                                       click_lag = 0.2
                                       )
        # Print the result DataFrame
        print(td_sessionize_out.result)
    __repr__(self)
    Returns the string representation for a Sessionize class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    StringSimilarity
    teradataml.analytics.sqle.StringSimilarity = class StringSimilarity(builtins.object)
    Methods deﬁned here:
    __init__(self, data=None, comparison_columns=None, case_sensitive=None, accumulate=None, data_order_column=None)
    DESCRIPTION:
        The StringSimilarity function calculates the similarity between two
        strings, using the specified comparison method.
        The similarity is a value in the range [0, 1].
        Note: This function is only available when teradataml is connected
              to Vantage 1.1 or later versions.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the teradataml DataFrame that contains the string pairs
            to be compared.
        data_order_column:
            Optional Argument.
            Specifies Order By columns for data.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
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
            Types: str OR list of strs
        case_sensitive:
            Optional Argument.
            Specifies whether string comparison is case-sensitive.
            You can specify either one value for all pairs or
            one value for each pair. If you specify one value for each pair, then 
            the ith value applies to the ith pair.
            Default value: "False".
            Types: bool OR list of bools
        accumulate:
            Optional Argument.
            Specifies the names of input teradataml DataFrame columns to be 
            copied to the output teradataml DataFrame.
            Types: str OR list of Strings (str)
    RETURNS:
        Instance of StringSimilarity.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as StringSimilarityObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load example data.
        load_example_data("stringsimilarity", "strsimilarity_input")
        # Create teradataml DataFrame objects.
        strsimilarity_input = DataFrame.from_table("strsimilarity_input")
        # Example -
        stringsimilarity_out = StringSimilarity(data = strsimilarity_input,
                                                comparison_columns=['jaro (src_text1, tar_text) AS jaro1_sim',
                                                                    'LD (src_text1, tar_text) AS ld1_sim',
                                                                    'n_gram (src_text1, tar_text, 2) AS ngram1_sim',
                                                                    'jaro_winkler (src_text1, tar_text, 0.1) AS jw1_sim'],
                                                case_sensitive = True,
                                                accumulate = ["id","src_text1","tar_text"]
                                               )
        # Print result DataFrame.
        print(stringsimilarity_out.result)
    __repr__(self)
    Returns the string representation for a StringSimilarity class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    SVMSparsePredict
    teradataml.analytics.sqle.SVMSparsePredict = class SVMSparsePredict(builtins.object)
    Methods deﬁned here:
    __init__(self, object=None, newdata=None, sample_id_column=None, attribute_column=None, value_column=None, accumulate_label=None, output_class_num=1,
    newdata_partition_column=None, newdata_order_column=None, object_order_column=None)
    DESCRIPTION:
        The SVMSparsePredict function takes the model generated by the
        SVMSparse trainer function and a set of test samples (in sparse
        format) and outputs a prediction for each sample.
    PARAMETERS:
        object:
            Required Argument.
            Specifies the teradataml DataFrame containing the model
            data generated by SVMSparse or instance of SVMSparse.
        object_order_column:
            Optional Argument.
            Specifies Order By columns for object.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
        newdata:
            Required Argument.
            Specifies the teradataml DataFrame containing the input test data.
        newdata_partition_column:
            Required Argument.
            Specifies Partition By columns for newdata.
            Values to this argument can be provided as a list, if multiple
            columns are used for partition.
            Types: str OR list of Strings (str)
        newdata_order_column:
            Optional Argument.
            Specifies Order By columns for newdata.
            Values to this argument can be provided as a list, if multiple
            columns are used for ordering.
            Types: str OR list of Strings (str)
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
            Valid only for multiple-class models. Specifies the number of class
            labels to appear in the output teradataml DataFrame, with its corresponding
            prediction confidence.
            Default Value: 1
            Types: int
    RETURNS:
        Instance of SVMSparsePredict.
        Output teradataml DataFrames can be accessed using attribute
        references, such as SVMSparsePredictObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        load_example_data("SVMSparsePredict",["svm_iris_input_train","svm_iris_input_test"])
        # Create teradataml DataFrame
        svm_iris_input_train = DataFrame.from_table("svm_iris_input_train")
        svm_iris_input_test = DataFrame.from_table("svm_iris_input_test")
        # Create SparseSVMTrainer object
        svm_train = SVMSparse(data=svm_iris_input_train,
                            sample_id_column='id',
                            attribute_column='attribute',
                            label_column='species',
                            value_column='value1',
                            max_step=150,
                            seed=0,
                            )
        # Example 1
        # Instance of SVMTrainer is passed as input to object argument
        svm_sparse_predict_result1 = SVMSparsePredict(newdata=svm_iris_input_test,
                                                     newdata_partition_column=['id'],
                                                     object=svm_train,
                                                     attribute_column='attribute',
                                                     sample_id_column='id',
                                                     value_column='value1',
                                                     accumulate_label='species'
                                                     )
        # Print the result DataFrame
        print(svm_sparse_predict_result1.result)
        # Example 2
        # teradataml DataFrame containing the model data generated by SVMSparse is passed as input to object argument
        svm_sparse_predict_result2 = SVMSparsePredict(newdata=svm_iris_input_test,
                                                     newdata_partition_column=['id'],
                                                     object=svm_train.model_table,
                                                     attribute_column='attribute',
                                                     sample_id_column='id',
                                                     value_column='value1',
                                                     accumulate_label='species'
                                                     )
        # Print the result DataFrame
        print(svm_sparse_predict_result2.result)
    __repr__(self)
    Returns the string representation for a SVMSparsePredict class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.
    Unpack
    teradataml.analytics.sqle.Unpack = class Unpack(builtins.object)
    Methods deﬁned here:
    __init__(self, data=None, input_column=None, output_columns=None, output_datatypes=None, delimiter=',', column_length=None, regex='(.*)', regex_set=1,
    exception=False, data_order_column=None)
    DESCRIPTION:
        The Unpack function unpacks data from a single packed column into 
        multiple columns. The packed column is composed of multiple virtual 
        columns, which become the output columns. To determine the virtual 
        columns, the function must have either the delimiter that separates 
        them in the packed column or their lengths.
        Note: This function is only available when teradataml is connected
              to Vantage 1.1 or later versions.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the teradataml DataFrame containing the input attributes.
        data_order_column:
            Optional Argument.
            Specifies Order By columns for data.
            Values to this argument can be provided as a list, if multiple 
            columns are used for ordering.
            Types: str OR list of Strings (str)
        input_column:
            Required Argument.
            Specifies the name of the input column that contains the packed 
            data.
            Types: str
        output_columns:
            Required Argument.
            Specifies the names to give to the output columns, in the order in 
            which the corresponding virtual columns appear in "input_column". If you 
            specify fewer output column names than there are in virtual input 
            columns, the function ignores the extra virtual input columns. That 
            is, if the packed data contains x+y virtual columns and the 
            output_columns argument specifies x output column names, the function 
            assigns the names to the first x virtual columns and ignores the 
            remaining y virtual columns.
            Types: str OR list of Strings (str)
        output_datatypes:
            Required Argument.
            Specifies the datatypes of the unpacked output columns. Supported 
            output_datatypes are VARCHAR, int, float, TIME, DATE, and 
            TIMESTAMP. If output_datatypes specifies only one value and 
            output_columns specifies multiple columns, then the specified value 
            applies to every output_column. If output_datatypes specifies 
            multiple values, then it must specify a value for each output_column. 
            The nth datatype corresponds to the nth output_column. The function 
            can output only 16 VARCHAR columns.
            Types: str OR list of Strings (str)
        delimiter:
            Optional Argument.
            Specifies the delimiter (a string) that separates the virtual 
            columns in the packed data. If the virtual columns are separated
            by a delimiter, then specify the delimiter with this argument; 
            otherwise, specify the column_length argument. Do not specify 
            both this argument and the column_length argument.
            Default Value: ","
            Types: str
        column_length:
            Optional Argument.
            Specifies the lengths of the virtual columns; therefore, to use 
            this argument, you must know the length of each virtual column.
            If column_length specifies only one value and output_columns specifies 
            multiple columns, then the specified value applies to every 
            output_column. 
            If column_length specifies multiple values, then it must specify 
            a value for each output_column. The nth datatype corresponds to 
            the nth output_column. However, the last value in column_length 
            can be an asterisk (*), which represents a single virtual column 
            that contains the remaining data. 
            For example, if the first three virtual columns have the lengths 
            2, 1, and 3, and all remaining data belongs to the fourth virtual 
            column, you can specify column_length ("2", "1", "3", *). 
            If you specify this argument, you must omit the delimiter argument.
            Types: str OR list of Strings (str)
        regex:
            Optional Argument.
            Specifies a regular expression that describes a row of packed data, 
            enabling the function to find the data values. 
            A row of packed data contains one data value for each virtual column,
            but the row might also contain other information (such as the 
            virtual column name). In the regex, each data value is enclosed 
            in parentheses. 
            For example, suppose that the packed data has two virtual columns, 
            age and sex, and that one row of packed data is: age:34,sex:male.
            The regex that describes the row is ".*:(.*)". The ".*:" matches 
            the virtual column names, age and sex, and the "(.*)" matches the 
            values, 34 and male. 
            To represent multiple data groups in regex, use multiple pairs
            of parentheses. By default, the last data group in regex represents 
            the data value (other data groups are assumed to be virtual column
            names or unwanted data). If a different data group represents the 
            data value, specify its group number with the regex_set argument.
            Default value matches the whole string (between delimiters,
            if any). When applied to the preceding sample row, the default 
            regex causes the function to return "age:34" and "sex:male" as 
            data values. 
            Default Value: "(.*)"
            Types: str
        regex_set:
            Optional Argument.
            Specifies the ordinal number of the data group in regex that
            represents the data value in a virtual column. By default, the 
            last data group in regex represents the data value. 
            For example, suppose that regex is: "([a-zA-Z]*):(.*)". If 
            group number is "1", then "([a-zA-Z]*)" represents the data value. 
            If group number is "2", then "(.*)" represents the data value.
            Default Value: 1
            Types: int
        exception:
            Optional Argument.
            Specifies whether the function ignores rows that contain invalid
            data. By default, the function fails if it encounters a row with 
            invalid data.
            Default Value: False
            Types: bool
    RETURNS:
        Instance of Unpack.
        Output teradataml DataFrames can be accessed using attribute
        references, such as UnpackObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Load the data to run the example.
        load_example_data("Unpack",["ville_tempdata","ville_tempdata1"])
        # Create teradataml DataFrame objects
        ville_tempdata1 = DataFrame.from_table("ville_tempdata1")
        ville_tempdata = DataFrame.from_table("ville_tempdata")
        # Example1 - Delimiter Separates Virtual Columns.
        # The input table, ville_tempdata, is a collection of temperature readings 
        # for two cities, Nashville and Knoxville, in the state of Tennessee. 
        # In the column of packed data, the delimiter comma (,) separates the virtual
        # columns. 
        unpack_out1 = Unpack(data=ville_tempdata,
                            input_column='packed_temp_data',
                            output_columns=['city','state','temp_f'],
                            output_datatypes=['varchar','varchar','real'],
                            delimiter=',',
                            regex='(.*)',
                            regex_set=1,
                            exception=True)
        # Print the results
        print(unpack_out1.result)
        # Example2 - No Delimiter Separates Virtual Columns.
        # The input, ville_tempdata1, contains same data as the previous example, 
        # except that no delimiter separates the virtual columns in the packed data.
        # To enable the function to determine the virtual columns, the function call 
        # specifes the column lengths.
        unpack_out2 = Unpack(data=ville_tempdata1,
                            input_column='packed_temp_data',
                            output_columns=['city','state','temp_f'],
                            output_datatypes=['varchar','varchar','real'],
                            column_length=['9','9','4'],
                            regex='(.*)',
                            regex_set=1,
                            exception=True)
        # Print the results
        print(unpack_out2.result)
    __repr__(self)
    Returns the string representation for a Unpack class instance.
    get_build_time(self)
    Function to return the build time of the algorithm in seconds.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_prediction_type(self)
    Function to return the Prediction type of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    get_target_column(self)
    Function to return the Target Column of the algorithm.
    When model object is created using retrieve_model(), then the value returned is
    as saved in the Model Catalog.
    show_query(self)
    Function to return the underlying SQL query.
    When model object is created using retrieve_model(), then None is returned.