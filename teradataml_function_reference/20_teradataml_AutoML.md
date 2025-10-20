# teradataml AutoML

AutoML
Methods
__init__
teradataml.automl.AutoML.__init__ = __init__(self, task_type='Default', include=None, exclude=None, verbose=0, max_runtime_secs=None, stopping_metric=None,
stopping_tolerance=None, max_models=None, custom_conﬁg_ﬁle=None, **kwargs)
DESCRIPTION:
    AutoML (Automated Machine Learning) is an approach that automates the process 
    of building, training, and validating machine learning models. It involves 
    various algorithms to automate various aspects of the machine learning workflow, 
    such as data preparation, feature engineering, model selection, hyperparameter
    tuning, and model deployment. It aims to simplify the process of building 
    machine learning models, by automating some of the more time-consuming 
    and labor-intensive tasks involved in the process.
    AutoML is designed to handle both regression and classification (binary and 
    multiclass) tasks. User can specify the task type whether to apply
    regression OR classification algorithm on the provided dataset. By default, AutoML 
    decides the task type.
    AutoML by default, trains using all model algorithms applicable for the 
    task type problem. For example, "glm" and "svm" does not support multi-class 
    classification problem. Thus, only 3 models are available to train in case 
    of multi-class classification problem, by default. While for regression and 
    binary classification problem, all 5 models i.e., "glm", "svm", "knn", 
    "decision_forest", "xgboost" are available to train by default.
    AutoML provides functionality to use specific model algorithms for training.
    User can provide either include or exclude model. In case of include, 
    only specified models are trained while for exclude, all models except 
    specified model are trained.
    AutoML also provides an option to customize the processes within feature
    engineering, data preparation and model training phases. User can customize
    the processes by passing the JSON file path in case of custom run. It also 
    supports early stopping of model training based on stopping metrics,
    maximum running time and maximum models to be trained.
    Note:
        * configure.temp_object_type="VT" follows sequential execution.
9/14/2025, 16:50
Page 1,416 of 1,675
PARAMETERS:  
    task_type:
        Optional Argument.
        Specifies the task type for AutoML, whether to apply regression OR classification
        on the provided dataset. If user wants AutoML to decide the task type automatically, 
        then it should be set to "Default".
        Default Value: "Default"
        Permitted Values: "Regression", "Classification", "Default"
        Types: str
    include:
        Optional Argument.
        Specifies the model algorithms to be used for model training phase.
        By default, all 5 models are used for training for regression and binary
        classification problem, while only 3 models are used for multi-class.
        Permitted Values: "glm", "svm", "knn", "decision_forest", "xgboost"
        Types: str OR list of str
    exclude:
        Optional Argument.
        Specifies the model algorithms to be excluded from model training phase.
        No model is excluded by default. 
        Permitted Values: "glm", "svm", "knn", "decision_forest", "xgboost"
        Types: str OR list of str
    verbose:
        Optional Argument.
        Specifies the detailed execution steps based on verbose level.
        Default Value: 0
        Permitted Values: 
            * 0: prints the progress bar and leaderboard
            * 1: prints the execution steps of AutoML.
            * 2: prints the intermediate data between the execution of each step of AutoML.
        Types: int
    max_runtime_secs:
        Optional Argument.
        Specifies the time limit in seconds for model training.
        Types: int
    stopping_metric:
        Required, when "stopping_tolerance" is set, otherwise optional.
        Specifies the stopping metrics for stopping tolerance in model training.
        Permitted Values: 
            * For task_type "Regression": "R2", "MAE", "MSE", "MSLE",
                                          "MAPE", "MPE", "RMSE", "RMSLE",
                                          "ME", "EV", "MPD", "MGD"
            * For task_type "Classification": 'MICRO-F1','MACRO-F1',
                                              'MICRO-RECALL','MACRO-RECALL',
                                              'MICRO-PRECISION', 'MACRO-PRECISION',
                                              'WEIGHTED-PRECISION','WEIGHTED-RECALL',
                                              'WEIGHTED-F1', 'ACCURACY'
        Types: str
    stopping_tolerance:
        Required, when "stopping_metric" is set, otherwise optional.
        Specifies the stopping tolerance for stopping metrics in model training.
        Types: float
    max_models:
        Optional Argument.
        Specifies the maximum number of models to be trained.
        Types: int
    custom_config_file:
        Optional Argument.
        Specifies the path of JSON file in case of custom run.
        Types: str
    **kwargs:
        Specifies the additional arguments for AutoML. Below
        are the additional arguments:
            volatile:
                Optional Argument.
                Specifies whether to put the interim results of the
                functions in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
9/14/2025, 16:50
Page 1,417 of 1,675
            persist:
                Optional Argument.
                Specifies whether to persist the interim results of the
                functions in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Note:
                    * User is responsible for cleanup of the persisted tables. List of persisted tables
                      in current session can be viewed using get_persisted_tables() method.
                Default Value: False
                Types: bool
            seed:
                Optional Argument.
                Specifies the random seed for reproducibility.
                Default Value: 42
                Types: int
RETURNS:
    Instance of AutoML.
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function raises error if not supported on the Vantage
    #        user is connected to.
    # Load the example data.
    >>> load_example_data("GLMPredict", ["admissions_test", "admissions_train"])
    >>> load_example_data("decisionforestpredict", ["housing_train", "housing_test"])
    >>> load_example_data("teradataml", "iris_input")
    # Create teradataml DataFrames.
    >>> admissions_train = DataFrame.from_table("admissions_train")
    >>> admissions_test = DataFrame.from_table("admissions_test")
    >>> housing_train = DataFrame.from_table("housing_train")
    >>> housing_test = DataFrame.from_table("housing_test")
    >>> iris_input = DataFrame.from_table("iris_input")
    # Example 1: Run AutoML for classification problem.
    # Scenario: Predict whether a student will be admitted to a university
    #           based on different factors. Run AutoML to get the best 
    #           performing model out of available models.
    # Create an instance of AutoML.
    >>> automl_obj = AutoML(task_type="Classification")
    # Fit the data.
    >>> automl_obj.fit(admissions_train, "admitted")
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(admissions_test)
    >>> prediction
    # Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(admissions_test, rank=2)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(admissions_test)
    >>> performance_metrics
    # Run evaluate to get performance metrics using model rank 3.
    >>> performance_metrics = automl_obj.evaluate(admissions_test, rank=3)
    >>> performance_metrics
    # Example 2 : Run AutoML for regression problem.
    # Scenario : Predict the price of house based on different factors.
    #            Run AutoML to get the best performing model using custom
    #            configuration file to customize different processes of
    #            AutoML Run. Use include to specify "xgbooost" and
    #            "decision_forset" models to be used for training.  
9/14/2025, 16:50
Page 1,418 of 1,675
    # Generate custom JSON file
    >>> AutoML.generate_custom_config("custom_housing")           
    # Create instance of AutoML.
    >>> automl_obj = AutoML(task_type="Regression",
    >>>                     verbose=1,
    >>>                     include=["decision_forest", "xgboost"], 
    >>>                     custom_config_file="custom_housing.json")
    # Fit the data.
    >>> automl_obj.fit(housing_train, "price")
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(housing_test)
    >>> prediction
    # Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(housing_test, rank=2)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(housing_test)
    >>> performance_metrics
    # Run evaluate to get performance metrics using second best performing model.
    >>> performance_metrics = automl_obj.evaluate(housing_test, rank=2)
    >>> performance_metrics
    # Example 3 : Run AutoML for multiclass classification problem.
    # Scenario : Predict the species of iris flower based on different
    #            factors. Use custom configuration file to customize 
    #            different processes of AutoML Run to get the best
    #            performing model out of available models.
    # Split the data into train and test.
    >>> iris_sample = iris_input.sample(frac = [0.8, 0.2])
    >>> iris_train= iris_sample[iris_sample['sampleid'] == 1].drop('sampleid', axis=1)
    >>> iris_test = iris_sample[iris_sample['sampleid'] == 2].drop('sampleid', axis=1)
    # Generate custom JSON file
    >>> AutoML.generate_custom_config()
    # Create instance of AutoML.
    >>> automl_obj = AutoML(verbose=2, 
    >>>                     exclude="xgboost",
    >>>                     custom_config_file="custom.json")
    # Fit the data.
    >>> automl_obj.fit(iris_train, iris_train.species)
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()
    # Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(iris_test, rank=2)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(iris_test)
    >>> performance_metrics
    # Example 4 : Run AutoML for regression problem with early stopping metric and tolerance.
    # Scenario : Predict the price of house based on different factors. 
    #            Use custom configuration file to customize different 
    #            processes of AutoML Run. Define performance threshold
    #            to acquire for the available models, and terminate training 
    #            upon meeting the stipulated performance criteria.
    # Generate custom JSON file
    >>> AutoML.generate_custom_config("custom_housing")
    # Create instance of AutoML.
    >>> automl_obj = AutoML(verbose=2, 
    >>>                     exclude="xgboost",
    >>>                     stopping_metric="R2",
9/14/2025, 16:50
Page 1,419 of 1,675
    >>>                     stopping_tolerance=0.7,
    >>>                     max_models=10,
    >>>                     custom_config_file="custom_housing.json")
    # Fit the data.
    >>> automl_obj.fit(housing_train, "price")
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(housing_test)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(housing_test)
    >>> performance_metrics
    # Example 5 : Run AutoML for regression problem with maximum runtime.
    # Scenario : Predict the species of iris flower based on different factors.
    #            Run AutoML to get the best performing model in specified time.
    # Split the data into train and test.
    >>> iris_sample = iris_input.sample(frac = [0.8, 0.2])
    >>> iris_train= iris_sample[iris_sample['sampleid'] == 1].drop('sampleid', axis=1)
    >>> iris_test = iris_sample[iris_sample['sampleid'] == 2].drop('sampleid', axis=1)
    # Create instance of AutoML.
    >>> automl_obj = AutoML(verbose=2, 
    >>>                     exclude="xgboost",
    >>>                     max_runtime_secs=500,
    >>>                     max_models=3)
    # Fit the data.
    >>> automl_obj.fit(iris_train, iris_train.species)
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(iris_test)
    >>> prediction
    # Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(iris_test, rank=2)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(iris_test)
    >>> performance_metrics
    # Run evaluate to get performance metrics using model rank 4.
    >>> performance_metrics = automl_obj.evaluate(iris_test, 4)
    >>> performance_metrics
deploy
teradataml.automl.AutoML.deploy = deploy(self, table_name, top_n=3, ranks=None)
DESCRIPTION:
    Function saves models to the specified table name.
    Note:
        * If 'ranks' is provided, specified models in 'ranks' will be saved
          and ranks will be reassigned to specified models based
          on the order of the leaderboard, non-specified models will be ignored.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the table name to which models information is to be saved.
        Types: str
    top_n:
        Optional Argument.
        Specifies the top n models to be saved.
        Note:
            * If 'ranks' is not provided, the function saves the top 'top_n' models.
        Default Value: 3
        Types: int
    ranks:
        Optional Argument.
9/14/2025, 16:50
Page 1,420 of 1,675
        Specifies the ranks for the models to be saved.
        Note:
            * If 'ranks' is provided, then 'top_n' is ignored.
        Types: int or list of int or range object
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    >>> obj = AutoML(task_type="Classification")
    >>> obj.fit(data = data, target_column = target_column)
    # Save top 3 models to the specified table.
    >>> obj.deploy("model_table")
    # Save top n models to the specified table.
    >>> obj.deploy("model_table", top_n=5)
    # Save models based on specified ranks to the specified table.
    >>> obj.deploy("model_table", ranks=[1, 3, 5])
    # Save models based on specified rank range to the specified table.
    >>> obj.deploy("model_table", ranks=range(2,6))
evaluate
teradataml.automl.AutoML.evaluate = evaluate(self, data, rank=1, use_loaded_models=False)
DESCRIPTION:
    Function evaluates on data using model rank in leaderboard
    and generates performance metrics.
    Note:
        * If both fit and load method are called before predict, then fit method model will be used
          for prediction by default unless 'use_loaded_models' is set to True in predict.
PARAMETERS:
    data:
        Required Argument.
        Specifies the dataset on which performance metrics needs to be generated.
        Types: teradataml DataFrame
        Note:
            * Target column used for generating model is mandatory in "data" for evaluation.
    rank:
        Optional Argument.
        Specifies the rank of the model available in the leaderboard to be used for evaluation.
        Default Value: 1
        Types: int
    use_loaded_models:
        Optional Argument.
        Specifies whether to use loaded models from database for prediction or not.
        Default Value: False
        Types: bool
RETURNS:
    Pandas DataFrame with performance metrics.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Perform evaluate() operation on the "automl_obj".
    # Example 1: Run evaluate on test data using best performing model.
    >>> performance_metrics = automl_obj.evaluate(admissions_test)
    >>> performance_metrics
    # Example 2: Run evaluate on test data using second best performing model.
    >>> performance_metrics = automl_obj.evaluate(admissions_test, rank=2)
    >>> performance_metrics
    # Example 3: Run evaluate on test data using loaded model.
    >>> automl_obj.load("model_table")
    >>> evaluation = automl_obj.evaluate(admissions_test, rank=3)
    >>> evaluation
9/14/2025, 16:50
Page 1,421 of 1,675
    # Example 4: Run predict on test data using loaded model when fit is also called.
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.load("model_table")
    >>> evaluation = automl_obj.evaluate(admissions_test, rank=3, use_loaded_models=True)
    >>> evaluation
ﬁt
teradataml.automl.AutoML.ﬁt = ﬁt(self, data, target_column)
DESCRIPTION:
    Function triggers the AutoML run. It is designed to handle both 
    regression and classification tasks depending on the specified "task_type".
PARAMETERS:
    data:
        Required Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml Dataframe
    target_column:
        Required Argument.
        Specifies target column of dataset.
        Types: str or ColumnExpression
RETURNS:
    None
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Example 1: Passing column expression for target column.
    >>> automl_obj.fit(data = housing_train, target_col = housing_train.price)
    # Example 2: Passing name of target column.
    >>> automl_obj.fit(data = housing_train, target_col = "price")
generate_custom_conﬁg
teradataml.automl.AutoML.generate_custom_conﬁg = generate_custom_conﬁg(ﬁle_name='custom')
DESCRIPTION:
    Function generates custom JSON file containing user customized input under current 
    working directory which can be used for AutoML execution.
PARAMETERS:
    file_name:
        Optional Argument.
        Specifies the name of the file to be generated. Do not pass the file name
        with extension. Extension '.json' is automatically added to specified file name.
        Default Value: "custom"
        Types: str
RETURNS:
    None 
EXAMPLES:
    # Import either of AutoML or AutoClassifier or AutoRegressor from teradataml.
    # As per requirement, generate json file using generate_custom_config() method.
    # Generate a default file named "custom.json" file using either of below options.
    >>> AutoML.generate_custom_config()
    or
    >>> AutoClassifier.generate_custom_config()
    or 
    >>> AutoRegressor.generate_custom_config()
    # The above code will generate "custom.json" file under the current working directory.
    # Generate different file name using "file_name" argument.
    >>> AutoML.generate_custom_config("titanic_custom")
    or
    >>> AutoClassifier.generate_custom_config("titanic_custom")
    or
    >>> AutoRegressor.generate_custom_config("housing_custom")
    # The above code will generate "titanic_custom.json" file under the current working directory.
get_persisted_tables
teradataml.automl.AutoML.get_persisted_tables = get_persisted_tables(self)
DESCRIPTION:
9/14/2025, 16:50
Page 1,422 of 1,675
    Get the list of the tables that are persisted in the database.
    Note:
        * User is responsible for keeping track of the persistent tables 
          and cleanup of the same if required.
PARAMETERS:
    None
RETURNS:
    Dictionary, containing the list of table names that mapped to the stage
    at which it was generated.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # 'persist' argument must be set to True in the AutoML object.
    >>> obj = AutoML(verbose=2, max_models=10, persist=True)
    # Load and fit the data.
    >>> load_example_data("teradataml", "titanic")
    >>> titanic_data = DataFrame("titanic")
    >>> obj.fit(data = titanic_data, target_column = titanic.survived)
    # Get the list of tables that are persisted in the database.
    >>> obj.get_persisted_tables()
leader
teradataml.automl.AutoML.leader = leader(self)
DESCRIPTION:
    Function displays best performing model.
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Generate leaderboard using leaderboard() method on "automl_obj".
    # Display best performing model using leader() method on "automl_obj".
    >>> automl_obj.leader()
leaderboard
teradataml.automl.AutoML.leaderboard = leaderboard(self)
DESCRIPTION:
    Function displays leaderboard.
RETURNS:
    Pandas DataFrame with Leaderboard information.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Generate leaderboard using leaderboard() method on "automl_obj".
    >>> automl_obj.leaderboard()
load
teradataml.automl.AutoML.load = load(self, table_name)
DESCRIPTION:
    Function loads models information from the specified table.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the table name from which models are to be loaded.
        Types: str
RETURNS:
    Pandas DataFrame with loaded models information.
RAISES:
9/14/2025, 16:50
Page 1,423 of 1,675
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    >>> obj = AutoML()
    # Load models from the specified table.
    >>> tab = obj.load("model_table")
model_hyperparameters
teradataml.automl.AutoML.model_hyperparameters = model_hyperparameters(self, rank=1, use_loaded_models=False)
DESCRIPTION:
    Get hyperparameters of the model based on rank in leaderboard.
    Note:
        * If both the fit() and load() methods are invoked before calling model_hyperparameters(), 
          by default hyperparameters are retrieved from the fit leaderboard. 
          To retrieve hyperparameters from the loaded models, set "use_loaded_models" to True in the model_hyperparameters call
PARAMETERS:
    rank:
        Required Argument.
        Specifies the rank of the model in the leaderboard.
        Default Value: 1
        Types: int
    use_loaded_models:
        Optional Argument.
        Specifies whether to use loaded models from database to get hyperparameters or not.
        Default Value: False
        Types: bool
RETURNS:
    Dictionary, containing hyperparameters.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Example 1: Get hyperparameters of the model using fit models.
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Get hyperparameters of the model using model_hyperparameters() method on "automl_obj".
    >>> automl_obj = AutoML(task_type="Classification")
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.model_hyperparameters(rank=1)
    # Example 2: Get hyperparameters of the model using loaded models.
    # Create an instance of the AutoML called "automl_obj"
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Load models from the specified table.
    # Get hyperparameters of the model using model_hyperparameters() method on "automl_obj".
    >>> automl_obj = AutoML()
    >>> automl_obj.load("model_table")
    >>> automl_obj.model_hyperparameters(rank=1)
    # Example 3: Get hyperparameters of the model when both fit and load method are called.
    # Create an instance of the AutoML called "automl_obj"
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Fit the data.
    # Load models from the specified table.
    # Get hyperparameters of the model using model_hyperparameters() method on "automl_obj".
    >>> automl_obj = AutoML(task_type="Classification")
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.load("model_table")
    # Get hyperparameters of the model using loaded models.
    >>> automl_obj.model_hyperparameters(rank=1, use_loaded_models=True)
    # Get hyperparameters of the model using fit models.
    >>> automl_obj.model_hyperparameters(rank=1)
predict
teradataml.automl.AutoML.predict = predict(self, data, rank=1, use_loaded_models=False)
DESCRIPTION:
    Function generates prediction on data using model rank in 
    leaderboard.
    Note:
        * If both fit and load method are called before predict, then fit method model will be used
          for prediction by default unless 'use_loaded_models' is set to True in predict.
PARAMETERS:
    data:
9/14/2025, 16:50
Page 1,424 of 1,675
        Required Argument.
        Specifies the dataset on which prediction needs to be generated 
        using model rank in leaderboard.
        Types: teradataml DataFrame
    rank:
        Optional Argument.
        Specifies the rank of the model in the leaderboard to be used for prediction.
        Default Value: 1
        Types: int
    use_loaded_models:
        Optional Argument.
        Specifies whether to use loaded models from database for prediction or not.
        Default Value: False
        Types: bool
RETURNS:
    Pandas DataFrame with predictions.
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Perform predict() operation on the "automl_obj".
    # Example 1: Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(admissions_test)
    >>> prediction
    # Example 2: Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(admissions_test, rank=2)
    >>> prediction
    # Example 3: Run predict on test data using loaded model.
    >>> automl_obj.load("model_table")
    >>> prediction = automl_obj.predict(admissions_test, rank=3)
    >>> prediction
    # Example 4: Run predict on test data using loaded model when fit is also called.
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.load("model_table")
    >>> prediction = automl_obj.predict(admissions_test, rank=3, use_loaded_models=True)
    >>> prediction
remove_saved_models
teradataml.automl.AutoML.remove_saved_models = remove_saved_models(self, table_name)
DESCRIPTION:
    Function removes the specified table containing saved models.
    Note:
        * If any data table/ result table is not present inside the database, 
          then it will be skipped.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the table name containing saved models.
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    >>> obj = AutoML()
    # Remove saved models from the specified table.
    >>> obj.remove_saved_models("model_table")
visualize
teradataml.automl.AutoML.visualize = visualize(**kwargs)
DESCRIPTION:
    Function visualizes the data using various plots such as heatmap, 
    pair plot, histogram, univariate plot, count plot, box plot, and target distribution.
PARAMETERS:
9/14/2025, 16:50
Page 1,425 of 1,675
    data:
        Required Argument.
        Specifies the input teradataml DataFrame for plotting.
        Types: teradataml Dataframe
    target_column:
        Required Argument.
        Specifies the name of the target column in "data".
        Note:
            * "target_column" must be of numeric type.
        Types: str
    plot_type:
        Optional Argument.
        Specifies the type of plot to be displayed.
        Default Value: "target"
        Permitted Values: 
            * "heatmap": Displays a heatmap of feature correlations.
            * "pair": Displays a pair plot of features.
            * "density": Displays a density plot of features.
            * "count": Displays a count plot of categorical features.
            * "box": Displays a box plot of numerical features.
            * "target": Displays the distribution of the target variable.
            * "all": Displays all the plots.
        Types: str, list of str
    length:
        Optional Argument.
        Specifies the length of the plot.
        Default Value: 10
        Types: int
    breadth:
        Optional Argument.
        Specifies the breadth of the plot.
        Default Value: 8
        Types: int
    columns:
        Optional Argument.
        Specifies the column names to be used for plotting.
        Types: str or list of string
    max_features:
        Optional Argument.
        Specifies the maximum number of features to be used for plotting.
        Default Value: 10
        Note:
            * It applies separately to categorical and numerical features.
        Types: int
    problem_type:
        Optional Argument.
        Specifies the type of problem.
        Permitted Values:
            * 'regression'
            * 'classification'
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Import either of AutoML or AutoClassifier or AutoRegressor or Autodataprep
    # from teradataml.
    >>> from teradataml import AutoML
    >>> from teradataml import DataFrame
    >>> load_example_data("teradataml", "titanic")
    >>> titanic_data = DataFrame("titanic")
    # Example 1: Visualize the data using AutoML class.
    >>> AutoML.visualize(data = titanic_data,
    ...                  target_column = 'survived',
    ...                  plot_type = ['heatmap', 'pair', 'histogram', 'target'],
    ...                  length = 10,
    ...                  breadth = 8,
    ...                  max_features = 10,
    ...                  problem_type = 'classification')
    # Example 2: Visualize the data using AutoDataPrep class.
    >>> from teradataml import AutoDataPrep
    >>> obj = AutoDataPrep(task_type="classification")
9/14/2025, 16:50
Page 1,426 of 1,675
    >>> obj.fit(data = titanic_data, target_column = 'survived')
    # Retrieve the data from AutoDataPrep object.
    >>> datas = obj.get_data()
    >>> AutoDataPrep.visualize(data = datas['lasso_train'],
    ...                        target_column = 'survived',
    ...                        plot_type = 'all'
    ...                        length = 20,
    ...                        breadth = 15)
AutoRegressor
Methods
__init__
teradataml.automl.AutoRegressor.__init__ = __init__(self, include=None, exclude=None, verbose=0, max_runtime_secs=None, stopping_metric=None,
stopping_tolerance=None, max_models=None, custom_conﬁg_ﬁle=None, **kwargs)
DESCRIPTION:
    AutoRegressor is a special purpose AutoML feature to run regression specific tasks.
    Note:
        * configure.temp_object_type="VT" follows sequential execution.
PARAMETERS:
    include:
        Optional Argument.
        Specifies the model algorithms to be used for model training phase.
        By default, all 5 models are used for training for regression and binary
        classification problem, while only 3 models are used for multi-class.
        Permitted Values: "glm", "svm", "knn", "decision_forest", "xgboost"
        Types: str OR list of str
    exclude:
        Optional Argument.
        Specifies the model algorithms to be excluded from model training phase.
        No model is excluded by default.
        Permitted Values: "glm", "svm", "knn", "decision_forest", "xgboost"
        Types: str OR list of str
    verbose:
        Optional Argument.
        Specifies the detailed execution steps based on verbose level.
        Default Value: 0
        Permitted Values: 
            * 0: prints the progress bar and leaderboard
            * 1: prints the execution steps of AutoML.
            * 2: prints the intermediate data between the execution of each step of AutoML.
        Types: int
    max_runtime_secs:
        Optional Argument.
        Specifies the time limit in seconds for model training.
        Types: int
    stopping_metric:
        Required, when "stopping_tolerance" is set, otherwise optional.
        Specifies the stopping mertics for stopping tolerance in model training.
        Permitted Values: 
            * For task_type "Regression": "R2", "MAE", "MSE", "MSLE",
                                          "MAPE", "MPE", "RMSE", "RMSLE",
                                          "ME", "EV", "MPD", "MGD"
            * For task_type "Classification": 'MICRO-F1','MACRO-F1',
                                              'MICRO-RECALL','MACRO-RECALL',
                                              'MICRO-PRECISION', 'MACRO-PRECISION',
                                              'WEIGHTED-PRECISION','WEIGHTED-RECALL',
                                              'WEIGHTED-F1', 'ACCURACY'
        Types: str
    stopping_tolerance:
        Required, when "stopping_metric" is set, otherwise optional.
        Specifies the stopping tolerance for stopping metrics in model training.
        Types: float
    max_models:
        Optional Argument.
        Specifies the maximum number of models to be trained.
        Types: int
    custom_config_file:
        Optional Argument.
        Specifies the path of JSON file in case of custom run.
9/14/2025, 16:50
Page 1,427 of 1,675
        Types: str
    **kwargs:
        Specifies the additional arguments for AutoRegressor. Below
        are the additional arguments:
            volatile:
                    Optional Argument.
                    Specifies whether to put the interim results of the
                    functions in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            persist:
                Optional Argument.
                Specifies whether to persist the interim results of the
                functions in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Note:
                    * User is responsible for cleanup of the persisted tables. List of persisted tables
                      in current session can be viewed using get_persisted_tables() method.
                Default Value: False
                Types: bool
            seed:
                Optional Argument.
                Specifies the random seed for reproducibility.
                Default Value: 42
                Types: int
RETURNS:
    Instance of AutoRegressor.
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function will raise error if not supported on the Vantage
    #        user is connected to.
    # Load the example data.
    >>> load_example_data("decisionforestpredict", ["housing_train", "housing_test"])
    # Create teradataml DataFrame object.
    >>> housing_train = DataFrame.from_table("housing_train")
    # Example 1 : Run AutoRegressor using default options.
    # Scenario : Predict the price of house based on different factors.
    # Create instance of AutoRegressor.
    >>> automl_obj = AutoRegressor()
    # Fit the data.
    >>> automl_obj.fit(housing_train, "price")
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(housing_test)
    >>> prediction
    # Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(housing_test, rank=2)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(housing_test)
    >>> performance_metrics
    # Run evaluate to get performance metrics using second best performing model.
    >>> performance_metrics = automl_obj.evaluate(housing_test, 2)
    >>> performance_metrics
9/14/2025, 16:50
Page 1,428 of 1,675
    # Example 2 : Run AutoRegressor for regression problem with early stopping metric and tolerance.
    # Scenario : Predict the price of house based on different factors.
    #            Use custom configuration file to customize different 
    #            processes of AutoML Run. Define performance threshold
    #            to acquire for the available models, and terminate training 
    #            upon meeting the stipulated performance criteria.
    # Generate custom configuration file.
    >>> AutoRegressor.generate_custom_config("custom_housing")
    # Create instance of AutoRegressor.
    >>> automl_obj = AutoRegressor(verbose=2,
    >>>                            exclude="xgboost",
    >>>                            stopping_metric="R2",
    >>>                            stopping_tolerance=0.7,
    >>>                            max_models=10,
    >>>                            custom_config_file="custom_housing.json")
    # Fit the data.
    >>> automl_obj.fit(housing_train, "price")
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(housing_test)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(housing_test)
    >>> performance_metrics
    # Example 3 : Run AutoRegressor for regression problem with maximum runtime.
    # Scenario : Predict the price of house based on different factors.
    #            Run AutoML to get the best performing model in specified time.
    # Create instance of AutoRegressor.
    >>> automl_obj = AutoRegressor(verbose=2, 
    >>>                            exclude="xgboost",
    >>>                            max_runtime_secs=500)
    # Fit the data.
    >>> automl_obj.fit(housing_train, "price")
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()  
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(housing_test)
    >>> prediction
    # Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(housing_test, 2)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(housing_test)
    >>> performance_metrics
deploy
teradataml.automl.AutoRegressor.deploy = deploy(self, table_name, top_n=3, ranks=None)
DESCRIPTION:
    Function saves models to the specified table name.
    Note:
        * If 'ranks' is provided, specified models in 'ranks' will be saved
          and ranks will be reassigned to specified models based
          on the order of the leaderboard, non-specified models will be ignored.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the table name to which models information is to be saved.
        Types: str
    top_n:
        Optional Argument.
        Specifies the top n models to be saved.
        Note:
            * If 'ranks' is not provided, the function saves the top 'top_n' models.
        Default Value: 3
        Types: int
9/14/2025, 16:50
Page 1,429 of 1,675
    ranks:
        Optional Argument.
        Specifies the ranks for the models to be saved.
        Note:
            * If 'ranks' is provided, then 'top_n' is ignored.
        Types: int or list of int or range object
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    >>> obj = AutoML(task_type="Classification")
    >>> obj.fit(data = data, target_column = target_column)
    # Save top 3 models to the specified table.
    >>> obj.deploy("model_table")
    # Save top n models to the specified table.
    >>> obj.deploy("model_table", top_n=5)
    # Save models based on specified ranks to the specified table.
    >>> obj.deploy("model_table", ranks=[1, 3, 5])
    # Save models based on specified rank range to the specified table.
    >>> obj.deploy("model_table", ranks=range(2,6))
evaluate
teradataml.automl.AutoRegressor.evaluate = evaluate(self, data, rank=1, use_loaded_models=False)
DESCRIPTION:
    Function evaluates on data using model rank in leaderboard
    and generates performance metrics.
    Note:
        * If both fit and load method are called before predict, then fit method model will be used
          for prediction by default unless 'use_loaded_models' is set to True in predict.
PARAMETERS:
    data:
        Required Argument.
        Specifies the dataset on which performance metrics needs to be generated.
        Types: teradataml DataFrame
        Note:
            * Target column used for generating model is mandatory in "data" for evaluation.
    rank:
        Optional Argument.
        Specifies the rank of the model available in the leaderboard to be used for evaluation.
        Default Value: 1
        Types: int
    use_loaded_models:
        Optional Argument.
        Specifies whether to use loaded models from database for prediction or not.
        Default Value: False
        Types: bool
RETURNS:
    Pandas DataFrame with performance metrics.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Perform evaluate() operation on the "automl_obj".
    # Example 1: Run evaluate on test data using best performing model.
    >>> performance_metrics = automl_obj.evaluate(admissions_test)
    >>> performance_metrics
    # Example 2: Run evaluate on test data using second best performing model.
    >>> performance_metrics = automl_obj.evaluate(admissions_test, rank=2)
    >>> performance_metrics
    # Example 3: Run evaluate on test data using loaded model.
9/14/2025, 16:50
Page 1,430 of 1,675
    >>> automl_obj.load("model_table")
    >>> evaluation = automl_obj.evaluate(admissions_test, rank=3)
    >>> evaluation
    # Example 4: Run predict on test data using loaded model when fit is also called.
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.load("model_table")
    >>> evaluation = automl_obj.evaluate(admissions_test, rank=3, use_loaded_models=True)
    >>> evaluation
ﬁt
teradataml.automl.AutoRegressor.ﬁt = ﬁt(self, data, target_column)
DESCRIPTION:
    Function triggers the AutoML run. It is designed to handle both 
    regression and classification tasks depending on the specified "task_type".
PARAMETERS:
    data:
        Required Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml Dataframe
    target_column:
        Required Argument.
        Specifies target column of dataset.
        Types: str or ColumnExpression
RETURNS:
    None
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Example 1: Passing column expression for target column.
    >>> automl_obj.fit(data = housing_train, target_col = housing_train.price)
    # Example 2: Passing name of target column.
    >>> automl_obj.fit(data = housing_train, target_col = "price")
generate_custom_conﬁg
teradataml.automl.AutoRegressor.generate_custom_conﬁg = generate_custom_conﬁg(ﬁle_name='custom')
DESCRIPTION:
    Function generates custom JSON file containing user customized input under current 
    working directory which can be used for AutoML execution.
PARAMETERS:
    file_name:
        Optional Argument.
        Specifies the name of the file to be generated. Do not pass the file name
        with extension. Extension '.json' is automatically added to specified file name.
        Default Value: "custom"
        Types: str
RETURNS:
    None 
EXAMPLES:
    # Import either of AutoML or AutoClassifier or AutoRegressor from teradataml.
    # As per requirement, generate json file using generate_custom_config() method.
    # Generate a default file named "custom.json" file using either of below options.
    >>> AutoML.generate_custom_config()
    or
    >>> AutoClassifier.generate_custom_config()
    or 
    >>> AutoRegressor.generate_custom_config()
    # The above code will generate "custom.json" file under the current working directory.
    # Generate different file name using "file_name" argument.
    >>> AutoML.generate_custom_config("titanic_custom")
    or
    >>> AutoClassifier.generate_custom_config("titanic_custom")
    or
    >>> AutoRegressor.generate_custom_config("housing_custom")
    # The above code will generate "titanic_custom.json" file under the current working directory.
get_persisted_tables
9/14/2025, 16:50
Page 1,431 of 1,675
teradataml.automl.AutoRegressor.get_persisted_tables = get_persisted_tables(self)
DESCRIPTION:
    Get the list of the tables that are persisted in the database.
    Note:
        * User is responsible for keeping track of the persistent tables 
          and cleanup of the same if required.
PARAMETERS:
    None
RETURNS:
    Dictionary, containing the list of table names that mapped to the stage
    at which it was generated.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # 'persist' argument must be set to True in the AutoML object.
    >>> obj = AutoML(verbose=2, max_models=10, persist=True)
    # Load and fit the data.
    >>> load_example_data("teradataml", "titanic")
    >>> titanic_data = DataFrame("titanic")
    >>> obj.fit(data = titanic_data, target_column = titanic.survived)
    # Get the list of tables that are persisted in the database.
    >>> obj.get_persisted_tables()
leader
teradataml.automl.AutoRegressor.leader = leader(self)
DESCRIPTION:
    Function displays best performing model.
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Generate leaderboard using leaderboard() method on "automl_obj".
    # Display best performing model using leader() method on "automl_obj".
    >>> automl_obj.leader()
leaderboard
teradataml.automl.AutoRegressor.leaderboard = leaderboard(self)
DESCRIPTION:
    Function displays leaderboard.
RETURNS:
    Pandas DataFrame with Leaderboard information.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Generate leaderboard using leaderboard() method on "automl_obj".
    >>> automl_obj.leaderboard()
load
teradataml.automl.AutoRegressor.load = load(self, table_name)
DESCRIPTION:
    Function loads models information from the specified table.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the table name from which models are to be loaded.
        Types: str
RETURNS:
    Pandas DataFrame with loaded models information.
9/14/2025, 16:50
Page 1,432 of 1,675
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    >>> obj = AutoML()
    # Load models from the specified table.
    >>> tab = obj.load("model_table")
model_hyperparameters
teradataml.automl.AutoRegressor.model_hyperparameters = model_hyperparameters(self, rank=1, use_loaded_models=False)
DESCRIPTION:
    Get hyperparameters of the model based on rank in leaderboard.
    Note:
        * If both the fit() and load() methods are invoked before calling model_hyperparameters(), 
          by default hyperparameters are retrieved from the fit leaderboard. 
          To retrieve hyperparameters from the loaded models, set "use_loaded_models" to True in the model_hyperparameters call
PARAMETERS:
    rank:
        Required Argument.
        Specifies the rank of the model in the leaderboard.
        Default Value: 1
        Types: int
    use_loaded_models:
        Optional Argument.
        Specifies whether to use loaded models from database to get hyperparameters or not.
        Default Value: False
        Types: bool
RETURNS:
    Dictionary, containing hyperparameters.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Example 1: Get hyperparameters of the model using fit models.
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Get hyperparameters of the model using model_hyperparameters() method on "automl_obj".
    >>> automl_obj = AutoML(task_type="Classification")
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.model_hyperparameters(rank=1)
    # Example 2: Get hyperparameters of the model using loaded models.
    # Create an instance of the AutoML called "automl_obj"
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Load models from the specified table.
    # Get hyperparameters of the model using model_hyperparameters() method on "automl_obj".
    >>> automl_obj = AutoML()
    >>> automl_obj.load("model_table")
    >>> automl_obj.model_hyperparameters(rank=1)
    # Example 3: Get hyperparameters of the model when both fit and load method are called.
    # Create an instance of the AutoML called "automl_obj"
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Fit the data.
    # Load models from the specified table.
    # Get hyperparameters of the model using model_hyperparameters() method on "automl_obj".
    >>> automl_obj = AutoML(task_type="Classification")
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.load("model_table")
    # Get hyperparameters of the model using loaded models.
    >>> automl_obj.model_hyperparameters(rank=1, use_loaded_models=True)
    # Get hyperparameters of the model using fit models.
    >>> automl_obj.model_hyperparameters(rank=1)
predict
teradataml.automl.AutoRegressor.predict = predict(self, data, rank=1, use_loaded_models=False)
DESCRIPTION:
    Function generates prediction on data using model rank in 
    leaderboard.
    Note:
        * If both fit and load method are called before predict, then fit method model will be used
          for prediction by default unless 'use_loaded_models' is set to True in predict.
9/14/2025, 16:50
Page 1,433 of 1,675
PARAMETERS:
    data:
        Required Argument.
        Specifies the dataset on which prediction needs to be generated 
        using model rank in leaderboard.
        Types: teradataml DataFrame
    rank:
        Optional Argument.
        Specifies the rank of the model in the leaderboard to be used for prediction.
        Default Value: 1
        Types: int
    use_loaded_models:
        Optional Argument.
        Specifies whether to use loaded models from database for prediction or not.
        Default Value: False
        Types: bool
RETURNS:
    Pandas DataFrame with predictions.
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Perform predict() operation on the "automl_obj".
    # Example 1: Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(admissions_test)
    >>> prediction
    # Example 2: Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(admissions_test, rank=2)
    >>> prediction
    # Example 3: Run predict on test data using loaded model.
    >>> automl_obj.load("model_table")
    >>> prediction = automl_obj.predict(admissions_test, rank=3)
    >>> prediction
    # Example 4: Run predict on test data using loaded model when fit is also called.
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.load("model_table")
    >>> prediction = automl_obj.predict(admissions_test, rank=3, use_loaded_models=True)
    >>> prediction
remove_saved_models
teradataml.automl.AutoRegressor.remove_saved_models = remove_saved_models(self, table_name)
DESCRIPTION:
    Function removes the specified table containing saved models.
    Note:
        * If any data table/ result table is not present inside the database, 
          then it will be skipped.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the table name containing saved models.
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    >>> obj = AutoML()
    # Remove saved models from the specified table.
    >>> obj.remove_saved_models("model_table")
visualize
teradataml.automl.AutoRegressor.visualize = visualize(**kwargs)
DESCRIPTION:
    Function visualizes the data using various plots such as heatmap, 
    pair plot, histogram, univariate plot, count plot, box plot, and target distribution.
9/14/2025, 16:50
Page 1,434 of 1,675
PARAMETERS:
    data:
        Required Argument.
        Specifies the input teradataml DataFrame for plotting.
        Types: teradataml Dataframe
    target_column:
        Required Argument.
        Specifies the name of the target column in "data".
        Note:
            * "target_column" must be of numeric type.
        Types: str
    plot_type:
        Optional Argument.
        Specifies the type of plot to be displayed.
        Default Value: "target"
        Permitted Values: 
            * "heatmap": Displays a heatmap of feature correlations.
            * "pair": Displays a pair plot of features.
            * "density": Displays a density plot of features.
            * "count": Displays a count plot of categorical features.
            * "box": Displays a box plot of numerical features.
            * "target": Displays the distribution of the target variable.
            * "all": Displays all the plots.
        Types: str, list of str
    length:
        Optional Argument.
        Specifies the length of the plot.
        Default Value: 10
        Types: int
    breadth:
        Optional Argument.
        Specifies the breadth of the plot.
        Default Value: 8
        Types: int
    columns:
        Optional Argument.
        Specifies the column names to be used for plotting.
        Types: str or list of string
    max_features:
        Optional Argument.
        Specifies the maximum number of features to be used for plotting.
        Default Value: 10
        Note:
            * It applies separately to categorical and numerical features.
        Types: int
    problem_type:
        Optional Argument.
        Specifies the type of problem.
        Permitted Values:
            * 'regression'
            * 'classification'
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Import either of AutoML or AutoClassifier or AutoRegressor or Autodataprep
    # from teradataml.
    >>> from teradataml import AutoML
    >>> from teradataml import DataFrame
    >>> load_example_data("teradataml", "titanic")
    >>> titanic_data = DataFrame("titanic")
    # Example 1: Visualize the data using AutoML class.
    >>> AutoML.visualize(data = titanic_data,
    ...                  target_column = 'survived',
    ...                  plot_type = ['heatmap', 'pair', 'histogram', 'target'],
    ...                  length = 10,
    ...                  breadth = 8,
    ...                  max_features = 10,
    ...                  problem_type = 'classification')
    # Example 2: Visualize the data using AutoDataPrep class.
9/14/2025, 16:50
Page 1,435 of 1,675
    >>> from teradataml import AutoDataPrep
    >>> obj = AutoDataPrep(task_type="classification")
    >>> obj.fit(data = titanic_data, target_column = 'survived')
    # Retrieve the data from AutoDataPrep object.
    >>> datas = obj.get_data()
    >>> AutoDataPrep.visualize(data = datas['lasso_train'],
    ...                        target_column = 'survived',
    ...                        plot_type = 'all'
    ...                        length = 20,
    ...                        breadth = 15)
AutoClassifier
Methods
__init__
teradataml.automl.AutoClassiﬁer.__init__ = __init__(self, include=None, exclude=None, verbose=0, max_runtime_secs=None, stopping_metric=None,
stopping_tolerance=None, max_models=None, custom_conﬁg_ﬁle=None, **kwargs)
DESCRIPTION:
    AutoClassifier is a special purpose AutoML feature to run classification specific tasks.
    Note:
        * configure.temp_object_type="VT" follows sequential execution.
PARAMETERS:  
    include:
        Optional Argument.
        Specifies the model algorithms to be used for model training phase.
        By default, all 5 models are used for training for regression and binary
        classification problem, while only 3 models are used for multi-class.
        Permitted Values: "glm", "svm", "knn", "decision_forest", "xgboost"
        Types: str OR list of str  
    exclude:
        Optional Argument.
        Specifies the model algorithms to be excluded from model training phase.
        No model is excluded by default. 
        Permitted Values: "glm", "svm", "knn", "decision_forest", "xgboost"
        Types: str OR list of str
    verbose:
        Optional Argument.
        Specifies the detailed execution steps based on verbose level.
        Default Value: 0
        Permitted Values: 
            * 0: prints the progress bar and leaderboard
            * 1: prints the execution steps of AutoML.
            * 2: prints the intermediate data between the execution of each step of AutoML.
        Types: int
    max_runtime_secs:
        Optional Argument.
        Specifies the time limit in seconds for model training.
        Types: int
    stopping_metric:
        Required, when "stopping_tolerance" is set, otherwise optional.
        Specifies the stopping mertics for stopping tolerance in model training.
        Permitted Values: 
            * For task_type "Regression": "R2", "MAE", "MSE", "MSLE",
                                          "MAPE", "MPE", "RMSE", "RMSLE",
                                          "ME", "EV", "MPD", "MGD"
            * For task_type "Classification": 'MICRO-F1','MACRO-F1',
                                              'MICRO-RECALL','MACRO-RECALL',
                                              'MICRO-PRECISION', 'MACRO-PRECISION',
                                              'WEIGHTED-PRECISION','WEIGHTED-RECALL',
                                              'WEIGHTED-F1', 'ACCURACY'
        Types: str
    stopping_tolerance:
        Required, when "stopping_metric" is set, otherwise optional.
        Specifies the stopping tolerance for stopping metrics in model training.
        Types: float
    max_models:
        Optional Argument.
        Specifies the maximum number of models to be trained.
        Types: int
    custom_config_file:
9/14/2025, 16:50
Page 1,436 of 1,675
        Optional Argument.
        Specifies the path of json file in case of custom run.
        Types: str
    **kwargs:
        Specifies the additional arguments for AutoClassifier. Below
        are the additional arguments:
            volatile:
                Optional Argument.
                Specifies whether to put the interim results of the
                functions in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
            persist:
                Optional Argument.
                Specifies whether to persist the interim results of the
                functions in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Note:
                    * User is responsible for cleanup of the persisted tables. List of persisted tables
                      in current session can be viewed using get_persisted_tables() method.
                Default Value: False
                Types: bool
            seed:
                Optional Argument.
                Specifies the random seed for reproducibility.
                Default Value: 42
                Types: int
RETURNS:
    Instance of AutoClassifier.
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:    
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function will raise error if not supported on the Vantage
    #        user is connected to.
    # Load the example data.
    >>> load_example_data("teradataml", ["titanic", "iris_input"])
    >>> load_example_data("GLMPredict", ["admissions_test", "admissions_train"])
    # Create teradataml DataFrame object.
    >>> admissions_train = DataFrame.from_table("admissions_train")
    >>> titanic = DataFrame.from_table("titanic")
    >>> iris_input = DataFrame.from_table("iris_input")
    >>> admissions_test = DataFrame.from_table("admissions_test")
    # Example 1 : Run AutoClassifier for binary classification problem
    # Scenario : Predict whether a student will be admitted to a university
    #            based on different factors. Run AutoML to get the best performing model
    #            out of available models.
    # Create instance of AutoClassifier..
    >>> automl_obj = AutoClassifier()
    # Fit the data.
    >>> automl_obj.fit(admissions_train, "admitted")
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(admissions_test)
    >>> prediction
    # Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(admissions_test, rank=2)
    >>> prediction
9/14/2025, 16:50
Page 1,437 of 1,675
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(admissions_test)
    >>> performance_metrics
    # Run evaluate to get performance metrics using model rank 4.
    >>> performance_metrics = automl_obj.evaluate(admissions_test, 4)
    >>> performance_metrics
    # Example 2 : Run AutoClassifier for binary classification.
    # Scenario : Predict whether passenger aboard the RMS Titanic survived
    #            or not based on differect factors. Run AutoML to get the 
    #            best performing model out of available models. Use custom
    #            configuration file to customize different processes of
    #            AutoML Run. 
    # Split the data into train and test.
    >>> titanic_sample = titanic.sample(frac = [0.8, 0.2])
    >>> titanic_train= titanic_sample[titanic_sample['sampleid'] == 1].drop('sampleid', axis=1)
    >>> titanic_test = titanic_sample[titanic_sample['sampleid'] == 2].drop('sampleid', axis=1)
    # Generate custom configuration file.
    >>> AutoClassifier.generate_custom_config("custom_titanic")
    # Create instance of AutoClassifier.
    >>> automl_obj = AutoClassifier(verbose=2, 
    >>>                             custom_config_file="custom_titanic.json")
    # Fit the data.
    >>> automl_obj.fit(titanic_train, titanic_train.survived)
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(titanic_test)
    >>> prediction
    # Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(titanic_test, rank=2)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(titanic_test)
    >>> performance_metrics
    # Example 3 : Run AutoClassifier for multiclass classification problem.
    # Scenario : Predict the species of iris flower based on different factors.
    #            Run AutoML to get the best performing model out of available 
    #            models. Use custom configuration file to customize different 
    #            processes of AutoML Run.
    # Split the data into train and test.
    >>> iris_sample = iris_input.sample(frac = [0.8, 0.2])
    >>> iris_train= iris_sample[iris_sample['sampleid'] == 1].drop('sampleid', axis=1)
    >>> iris_test = iris_sample[iris_sample['sampleid'] == 2].drop('sampleid', axis=1)
    # Generate custom configuration file.
    >>> AutoClassifier.generate_custom_config("custom_iris")
    # Create instance of AutoClassifier.
    >>> automl_obj = AutoClassifier(verbose=1, 
    >>>                             custom_config_file="custom_iris.json")
    # Fit the data.
    >>> automl_obj.fit(iris_train, "species")
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()
    # Predict on test data using best performing model.
    >>> prediction = automl_obj.predict(iris_test)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(iris_test)
    >>> performance_metrics
    # Example 4 : Run AutoClassifier for classification problem with stopping metric and tolerance.
    # Scenario : Predict whether passenger aboard the RMS Titanic survived
    #            or not based on differect factors. Use custom configuration 
9/14/2025, 16:50
Page 1,438 of 1,675
    #            file to customize different processes of AutoML Run. Define
    #            performance threshold to acquire for the available models, and 
    #            terminate training upon meeting the stipulated performance criteria.
    # Split the data into train and test.
    >>> titanic_sample = titanic.sample(frac = [0.8, 0.2])
    >>> titanic_train= titanic_sample[titanic_sample['sampleid'] == 1].drop('sampleid', axis=1)
    >>> titanic_test = titanic_sample[titanic_sample['sampleid'] == 2].drop('sampleid', axis=1)
    # Generate custom configuration file.
    >>> AutoClassifier.generate_custom_config("custom_titanic")
    # Create instance of AutoClassifier.
    >>> automl_obj = AutoClassifier(verbose=2, 
    >>>                             exclude="xgboost",
    >>>                             stopping_metric="MICRO-F1",
    >>>                             stopping_tolerance=0.7,
    >>>                             max_models=8
    >>>                             custom_config_file="custom_titanic.json")
    # Fit the data.
    >>> automl_obj.fit(titanic_train, titanic_train.survived)
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(titanic_test)
    >>> prediction
    # Run evaluate to get performance metrics using best performing model.
    >>> performance_metrics = automl_obj.evaluate(titanic_test)
    >>> performance_metrics
    # Example 5 : Run AutoClassifier for classification problem with maximum runtime.
    # Scenario : Predict the species of iris flower based on different factors.
    #            Run AutoML to get the best performing model in specified time.
    # Split the data into train and test.
    >>> iris_sample = iris_input.sample(frac = [0.8, 0.2])
    >>> iris_train= iris_sample[iris_sample['sampleid'] == 1].drop('sampleid', axis=1)
    >>> iris_test = iris_sample[iris_sample['sampleid'] == 2].drop('sampleid', axis=1)
    # Create instance of AutoClassifier.
    >>> automl_obj = AutoClassifier(verbose=2, 
    >>>                             exclude="xgboost",
    >>>                             max_runtime_secs=500)
    >>>                             max_models=3)
    # Fit the data.
    >>> automl_obj.fit(iris_train, iris_train.species)
    # Display leaderboard.
    >>> automl_obj.leaderboard()
    # Display best performing model.
    >>> automl_obj.leader()
    # Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(iris_test)
    >>> prediction
    # Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(iris_test, rank=2)
    >>> prediction
    # Run evaluate to get performance metrics using model rank 3.
    >>> performance_metrics = automl_obj.evaluate(iris_test, 3)
    >>> performance_metrics
deploy
teradataml.automl.AutoClassiﬁer.deploy = deploy(self, table_name, top_n=3, ranks=None)
DESCRIPTION:
    Function saves models to the specified table name.
    Note:
        * If 'ranks' is provided, specified models in 'ranks' will be saved
          and ranks will be reassigned to specified models based
          on the order of the leaderboard, non-specified models will be ignored.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the table name to which models information is to be saved.
        Types: str
9/14/2025, 16:50
Page 1,439 of 1,675
    top_n:
        Optional Argument.
        Specifies the top n models to be saved.
        Note:
            * If 'ranks' is not provided, the function saves the top 'top_n' models.
        Default Value: 3
        Types: int
    ranks:
        Optional Argument.
        Specifies the ranks for the models to be saved.
        Note:
            * If 'ranks' is provided, then 'top_n' is ignored.
        Types: int or list of int or range object
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    >>> obj = AutoML(task_type="Classification")
    >>> obj.fit(data = data, target_column = target_column)
    # Save top 3 models to the specified table.
    >>> obj.deploy("model_table")
    # Save top n models to the specified table.
    >>> obj.deploy("model_table", top_n=5)
    # Save models based on specified ranks to the specified table.
    >>> obj.deploy("model_table", ranks=[1, 3, 5])
    # Save models based on specified rank range to the specified table.
    >>> obj.deploy("model_table", ranks=range(2,6))
evaluate
teradataml.automl.AutoClassiﬁer.evaluate = evaluate(self, data, rank=1, use_loaded_models=False)
DESCRIPTION:
    Function evaluates on data using model rank in leaderboard
    and generates performance metrics.
    Note:
        * If both fit and load method are called before predict, then fit method model will be used
          for prediction by default unless 'use_loaded_models' is set to True in predict.
PARAMETERS:
    data:
        Required Argument.
        Specifies the dataset on which performance metrics needs to be generated.
        Types: teradataml DataFrame
        Note:
            * Target column used for generating model is mandatory in "data" for evaluation.
    rank:
        Optional Argument.
        Specifies the rank of the model available in the leaderboard to be used for evaluation.
        Default Value: 1
        Types: int
    use_loaded_models:
        Optional Argument.
        Specifies whether to use loaded models from database for prediction or not.
        Default Value: False
        Types: bool
RETURNS:
    Pandas DataFrame with performance metrics.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Perform evaluate() operation on the "automl_obj".
    # Example 1: Run evaluate on test data using best performing model.
9/14/2025, 16:50
Page 1,440 of 1,675
    >>> performance_metrics = automl_obj.evaluate(admissions_test)
    >>> performance_metrics
    # Example 2: Run evaluate on test data using second best performing model.
    >>> performance_metrics = automl_obj.evaluate(admissions_test, rank=2)
    >>> performance_metrics
    # Example 3: Run evaluate on test data using loaded model.
    >>> automl_obj.load("model_table")
    >>> evaluation = automl_obj.evaluate(admissions_test, rank=3)
    >>> evaluation
    # Example 4: Run predict on test data using loaded model when fit is also called.
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.load("model_table")
    >>> evaluation = automl_obj.evaluate(admissions_test, rank=3, use_loaded_models=True)
    >>> evaluation
ﬁt
teradataml.automl.AutoClassiﬁer.ﬁt = ﬁt(self, data, target_column)
DESCRIPTION:
    Function triggers the AutoML run. It is designed to handle both 
    regression and classification tasks depending on the specified "task_type".
PARAMETERS:
    data:
        Required Argument.
        Specifies the input teradataml DataFrame.
        Types: teradataml Dataframe
    target_column:
        Required Argument.
        Specifies target column of dataset.
        Types: str or ColumnExpression
RETURNS:
    None
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Example 1: Passing column expression for target column.
    >>> automl_obj.fit(data = housing_train, target_col = housing_train.price)
    # Example 2: Passing name of target column.
    >>> automl_obj.fit(data = housing_train, target_col = "price")
generate_custom_conﬁg
teradataml.automl.AutoClassiﬁer.generate_custom_conﬁg = generate_custom_conﬁg(ﬁle_name='custom')
DESCRIPTION:
    Function generates custom JSON file containing user customized input under current 
    working directory which can be used for AutoML execution.
PARAMETERS:
    file_name:
        Optional Argument.
        Specifies the name of the file to be generated. Do not pass the file name
        with extension. Extension '.json' is automatically added to specified file name.
        Default Value: "custom"
        Types: str
RETURNS:
    None 
EXAMPLES:
    # Import either of AutoML or AutoClassifier or AutoRegressor from teradataml.
    # As per requirement, generate json file using generate_custom_config() method.
    # Generate a default file named "custom.json" file using either of below options.
    >>> AutoML.generate_custom_config()
    or
    >>> AutoClassifier.generate_custom_config()
    or 
    >>> AutoRegressor.generate_custom_config()
    # The above code will generate "custom.json" file under the current working directory.
    # Generate different file name using "file_name" argument.
9/14/2025, 16:50
Page 1,441 of 1,675
    >>> AutoML.generate_custom_config("titanic_custom")
    or
    >>> AutoClassifier.generate_custom_config("titanic_custom")
    or
    >>> AutoRegressor.generate_custom_config("housing_custom")
    # The above code will generate "titanic_custom.json" file under the current working directory.
get_persisted_tables
teradataml.automl.AutoClassiﬁer.get_persisted_tables = get_persisted_tables(self)
DESCRIPTION:
    Get the list of the tables that are persisted in the database.
    Note:
        * User is responsible for keeping track of the persistent tables 
          and cleanup of the same if required.
PARAMETERS:
    None
RETURNS:
    Dictionary, containing the list of table names that mapped to the stage
    at which it was generated.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # 'persist' argument must be set to True in the AutoML object.
    >>> obj = AutoML(verbose=2, max_models=10, persist=True)
    # Load and fit the data.
    >>> load_example_data("teradataml", "titanic")
    >>> titanic_data = DataFrame("titanic")
    >>> obj.fit(data = titanic_data, target_column = titanic.survived)
    # Get the list of tables that are persisted in the database.
    >>> obj.get_persisted_tables()
leader
teradataml.automl.AutoClassiﬁer.leader = leader(self)
DESCRIPTION:
    Function displays best performing model.
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Generate leaderboard using leaderboard() method on "automl_obj".
    # Display best performing model using leader() method on "automl_obj".
    >>> automl_obj.leader()
leaderboard
teradataml.automl.AutoClassiﬁer.leaderboard = leaderboard(self)
DESCRIPTION:
    Function displays leaderboard.
RETURNS:
    Pandas DataFrame with Leaderboard information.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Generate leaderboard using leaderboard() method on "automl_obj".
    >>> automl_obj.leaderboard()
load
teradataml.automl.AutoClassiﬁer.load = load(self, table_name)
DESCRIPTION:
    Function loads models information from the specified table.
9/14/2025, 16:50
Page 1,442 of 1,675
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the table name from which models are to be loaded.
        Types: str
RETURNS:
    Pandas DataFrame with loaded models information.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    >>> obj = AutoML()
    # Load models from the specified table.
    >>> tab = obj.load("model_table")
model_hyperparameters
teradataml.automl.AutoClassiﬁer.model_hyperparameters = model_hyperparameters(self, rank=1, use_loaded_models=False)
DESCRIPTION:
    Get hyperparameters of the model based on rank in leaderboard.
    Note:
        * If both the fit() and load() methods are invoked before calling model_hyperparameters(), 
          by default hyperparameters are retrieved from the fit leaderboard. 
          To retrieve hyperparameters from the loaded models, set "use_loaded_models" to True in the model_hyperparameters call
PARAMETERS:
    rank:
        Required Argument.
        Specifies the rank of the model in the leaderboard.
        Default Value: 1
        Types: int
    use_loaded_models:
        Optional Argument.
        Specifies whether to use loaded models from database to get hyperparameters or not.
        Default Value: False
        Types: bool
RETURNS:
    Dictionary, containing hyperparameters.
RAISES:
    TeradataMlException.
EXAMPLES:
    # Example 1: Get hyperparameters of the model using fit models.
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Get hyperparameters of the model using model_hyperparameters() method on "automl_obj".
    >>> automl_obj = AutoML(task_type="Classification")
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.model_hyperparameters(rank=1)
    # Example 2: Get hyperparameters of the model using loaded models.
    # Create an instance of the AutoML called "automl_obj"
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Load models from the specified table.
    # Get hyperparameters of the model using model_hyperparameters() method on "automl_obj".
    >>> automl_obj = AutoML()
    >>> automl_obj.load("model_table")
    >>> automl_obj.model_hyperparameters(rank=1)
    # Example 3: Get hyperparameters of the model when both fit and load method are called.
    # Create an instance of the AutoML called "automl_obj"
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Fit the data.
    # Load models from the specified table.
    # Get hyperparameters of the model using model_hyperparameters() method on "automl_obj".
    >>> automl_obj = AutoML(task_type="Classification")
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.load("model_table")
    # Get hyperparameters of the model using loaded models.
    >>> automl_obj.model_hyperparameters(rank=1, use_loaded_models=True)
    # Get hyperparameters of the model using fit models.
    >>> automl_obj.model_hyperparameters(rank=1)
predict
9/14/2025, 16:50
Page 1,443 of 1,675
teradataml.automl.AutoClassiﬁer.predict = predict(self, data, rank=1, use_loaded_models=False)
DESCRIPTION:
    Function generates prediction on data using model rank in 
    leaderboard.
    Note:
        * If both fit and load method are called before predict, then fit method model will be used
          for prediction by default unless 'use_loaded_models' is set to True in predict.
PARAMETERS:
    data:
        Required Argument.
        Specifies the dataset on which prediction needs to be generated 
        using model rank in leaderboard.
        Types: teradataml DataFrame
    rank:
        Optional Argument.
        Specifies the rank of the model in the leaderboard to be used for prediction.
        Default Value: 1
        Types: int
    use_loaded_models:
        Optional Argument.
        Specifies whether to use loaded models from database for prediction or not.
        Default Value: False
        Types: bool
RETURNS:
    Pandas DataFrame with predictions.
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    # Create an instance of the AutoML called "automl_obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    # Perform fit() operation on the "automl_obj".
    # Perform predict() operation on the "automl_obj".
    # Example 1: Run predict on test data using best performing model.
    >>> prediction = automl_obj.predict(admissions_test)
    >>> prediction
    # Example 2: Run predict on test data using second best performing model.
    >>> prediction = automl_obj.predict(admissions_test, rank=2)
    >>> prediction
    # Example 3: Run predict on test data using loaded model.
    >>> automl_obj.load("model_table")
    >>> prediction = automl_obj.predict(admissions_test, rank=3)
    >>> prediction
    # Example 4: Run predict on test data using loaded model when fit is also called.
    >>> automl_obj.fit(admissions_train, "admitted")
    >>> automl_obj.load("model_table")
    >>> prediction = automl_obj.predict(admissions_test, rank=3, use_loaded_models=True)
    >>> prediction
remove_saved_models
teradataml.automl.AutoClassiﬁer.remove_saved_models = remove_saved_models(self, table_name)
DESCRIPTION:
    Function removes the specified table containing saved models.
    Note:
        * If any data table/ result table is not present inside the database, 
          then it will be skipped.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the table name containing saved models.
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Create an instance of the AutoML called "obj" 
    # by referring "AutoML() or AutoRegressor() or AutoClassifier()" method.
    >>> obj = AutoML()
    # Remove saved models from the specified table.
9/14/2025, 16:50
Page 1,444 of 1,675
    >>> obj.remove_saved_models("model_table")
visualize
teradataml.automl.AutoClassiﬁer.visualize = visualize(**kwargs)
DESCRIPTION:
    Function visualizes the data using various plots such as heatmap, 
    pair plot, histogram, univariate plot, count plot, box plot, and target distribution.
PARAMETERS:
    data:
        Required Argument.
        Specifies the input teradataml DataFrame for plotting.
        Types: teradataml Dataframe
    target_column:
        Required Argument.
        Specifies the name of the target column in "data".
        Note:
            * "target_column" must be of numeric type.
        Types: str
    plot_type:
        Optional Argument.
        Specifies the type of plot to be displayed.
        Default Value: "target"
        Permitted Values: 
            * "heatmap": Displays a heatmap of feature correlations.
            * "pair": Displays a pair plot of features.
            * "density": Displays a density plot of features.
            * "count": Displays a count plot of categorical features.
            * "box": Displays a box plot of numerical features.
            * "target": Displays the distribution of the target variable.
            * "all": Displays all the plots.
        Types: str, list of str
    length:
        Optional Argument.
        Specifies the length of the plot.
        Default Value: 10
        Types: int
    breadth:
        Optional Argument.
        Specifies the breadth of the plot.
        Default Value: 8
        Types: int
    columns:
        Optional Argument.
        Specifies the column names to be used for plotting.
        Types: str or list of string
    max_features:
        Optional Argument.
        Specifies the maximum number of features to be used for plotting.
        Default Value: 10
        Note:
            * It applies separately to categorical and numerical features.
        Types: int
    problem_type:
        Optional Argument.
        Specifies the type of problem.
        Permitted Values:
            * 'regression'
            * 'classification'
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Import either of AutoML or AutoClassifier or AutoRegressor or Autodataprep
    # from teradataml.
    >>> from teradataml import AutoML
    >>> from teradataml import DataFrame
    >>> load_example_data("teradataml", "titanic")
    >>> titanic_data = DataFrame("titanic")
    # Example 1: Visualize the data using AutoML class.
    >>> AutoML.visualize(data = titanic_data,
9/14/2025, 16:50
Page 1,445 of 1,675
    ...                  target_column = 'survived',
    ...                  plot_type = ['heatmap', 'pair', 'histogram', 'target'],
    ...                  length = 10,
    ...                  breadth = 8,
    ...                  max_features = 10,
    ...                  problem_type = 'classification')
    # Example 2: Visualize the data using AutoDataPrep class.
    >>> from teradataml import AutoDataPrep
    >>> obj = AutoDataPrep(task_type="classification")
    >>> obj.fit(data = titanic_data, target_column = 'survived')
    # Retrieve the data from AutoDataPrep object.
    >>> datas = obj.get_data()
    >>> AutoDataPrep.visualize(data = datas['lasso_train'],
    ...                        target_column = 'survived',
    ...                        plot_type = 'all'
    ...                        length = 20,
    ...                        breadth = 15)
AutoDataPrep
Methods
__init__
teradataml.automl.autodataprep.AutoDataPrep.__init__ = __init__(self, task_type='Default', verbose=0, **kwargs)
DESCRIPTION:
    AutoDataPrep simplifies the data preparation process by automating the different aspects of 
    data cleaning and transformation, enabling seamless exploration, transformation, and optimization of datasets.
PARAMETERS:
    task_type:
        Optional Argument.
        Specifies the task type for AutoDataPrep, whether to apply regression OR classification
        on the provided dataset. If user wants AutoDataPrep() to decide the task type automatically, 
        then it should be set to "Default".
        Default Value: "Default"
        Permitted Values: "Regression", "Classification", "Default"
        Types: str
    verbose:
        Optional Argument.
        Specifies the detailed execution steps based on verbose level.
        Default Value: 0
        Permitted Values: 
            * 0: prints the progress bar.
            * 1: prints the execution steps.
            * 2: prints the intermediate data between the execution of each step.
        Types: int
    **kwargs:
        Specifies the additional arguments for AutoDataPrep. Below
        are the additional arguments:
            custom_config_file:
                Optional Argument.
                Specifies the path of JSON file in case of custom run.
                Types: str
            volatile:
                Optional Argument.
                Specifies whether to put the interim results of the
                functions in a volatile table or not. When set to
                True, results are stored in a volatile table,
                otherwise not.
                Default Value: False
                Types: bool
            persist:
                Optional Argument.
                Specifies whether to persist the interim results of the
                functions in a table or not. When set to True,
                results are persisted in a table; otherwise,
                results are garbage collected at the end of the
                session.
                Default Value: False
                Types: bool
RETURNS:
    Instance of AutoDataPrep.
RAISES:
    TeradataMlException, TypeError, ValueError
9/14/2025, 16:50
Page 1,446 of 1,675
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function raises error if not supported on the Vantage
    #        user is connected to.
    # Load the example data.
    >>> load_example_data("teradataml", "titanic")
    # Create teradataml DataFrames.
    >>> titanic = DataFrame.from_table("titanic")
    # Example 1: Run AutoDataPrep for classification problem.
    # Scenario: Titanic dataset is used to predict the survival of passengers.
    # Create an instance of AutoDataPrep.
    >>> aprep_obj = AutoDataPrep(task_type="Classification", verbose=2)
    # Fit the data.
    >>> aprep_obj.fit(titanic, titanic.survived)
    # Retrieve the data after Auto Data Preparation.
    >>> datas = aprep_obj.get_data()
delete
teradataml.automl.autodataprep.AutoDataPrep.delete_data = delete_data(self, table_name, fs_method=None)
DESCRIPTION:
    Deletes the deployed datasets from the database.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the name of the table containing the deployed datasets.
        Types: str
    fs_method:
        Optional Argument.
        Specifies the name of the feature selection method to delete from the
        deployed datasets.
        Default Value: None
        Permitted Values: "lasso", "rfe", "pca"
        Note:
            * If "fs_method" is None, then method deletes all the deployed datasets.
        Types: str or list of str
RETURNS:
    None
RAISES:
    TeradataMlException
EXAMPLES:
    # Create an instance of the AutoDataPrep.
    # Fit the data.
    # Deploy the data to the table.
    # Remove the deployed data from the table.
    # Example 1: Remove the deployed data from the table within the AutoDataPrep object.
    from teradataml import AutoDataPrep
    # Load the example data.
    >>> load_example_data("teradataml", "titanic")
    >>> titanic = DataFrame.from_table("titanic")
    # Create an instance of AutoDataPrep.
    >>> aprep_obj = AutoDataPrep(task_type="Classification", verbose=2)
    # fit the data.
    >>> aprep_obj.fit(titanic, titanic.survived)
    # Deploy the datas to the database.
    >>> aprep_obj.deploy("table_name")
    # Remove lasso deployed data from the table.
    >>> aprep_obj.delete_data("table_name", fs_method="lasso")
    # Example 2: Remove the deployed data from the table using different instance of AutoDataPrep object.
    # Create an instance of AutoDataPrep.
    >>> aprep_obj2 = AutoDataPrep()
9/14/2025, 16:50
Page 1,447 of 1,675
    # Remove lasso and pca deployed data from the table.
    >>> aprep_obj2.delete_data("table_name", fs_method=["lasso", "pca"])
deploy
teradataml.automl.autodataprep.AutoDataPrep.deploy = deploy(self, table_name)
DESCRIPTION:
    Deploy the AutoDataPrep generated data to the database,
    i.e., saves the data in the database.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the name of the table to store the information
        of deployed datasets in the database.
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException, ValueError
EXAMPLES:
    # Create an instance of the AutoDataPrep.
    # Perform fit() operation on the AutoDataPrep object.
    # Deploy the data to the table.
    From teradataml import AutoDataPrep
    # Load the example data.
    >>> load_example_data("teradataml", "titanic")
    >>> titanic = DataFrame.from_table("titanic")
    # Create an instance of AutoDataPrep.
    >>> aprep_obj = AutoDataPrep(task_type="Classification", verbose=2)
    # Fit the data.
    >>> aprep_obj.fit(titanic, titanic.survived)
    # Deploy the data to the table.
    >>> aprep_obj.deploy("table_name")
ﬁt
teradataml.automl.autodataprep.AutoDataPrep.ﬁt = ﬁt(self, data, target_column)
DESCRIPTION:
    Function to fit the data for Auto Data Preparation.
PARAMETERS:
    data:
        Required Argument.
        Specifies the input data to be used for Auto Data Preparation.
        Types: DataFrame
    target_column:
        Required Argument.
        Specifies the target column to be used for Auto Data Preparation.
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException, ValueError
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function raises error if not supported on the Vantage
    #        user is connected to.
    # Load the example data.
    >>> load_example_data("teradataml", "titanic")
    # Create teradataml DataFrames.
    >>> titanic = DataFrame.from_table("titanic")
    # Example 1: Run AutoDataPrep for classification problem.
    # Scenario: Titanic dataset is used to predict the survival of passengers.
    # Create an instance of AutoDataPrep.
    >>> aprep_obj = AutoDataPrep(task_type="Classification", verbose=2)
9/14/2025, 16:50
Page 1,448 of 1,675
    # Fit the data.
    >>> aprep_obj.fit(titanic, titanic.survived)
get_data
teradataml.automl.autodataprep.AutoDataPrep.get_data = get_data(self)
DESCRIPTION:
    Function to retrieve the data after Auto Data Preparation.
RETURNS:
     Dictionary of DataFrames containing the data after Auto Data Preparation.
RAISES:
    TeradataMlException
EXAMPLES:
    # Notes:
    #     1. Get the connection to Vantage to execute the function.
    #     2. One must import the required functions mentioned in
    #        the example from teradataml.
    #     3. Function raises error if not supported on the Vantage
    #        user is connected to.
    # Load the example data.
    >>> load_example_data("teradataml", "titanic")
    # Create teradataml DataFrames.
    >>> titanic = DataFrame.from_table("titanic")
    # Example 1: Run AutoDataPrep for classification problem.
    # Scenario: Titanic dataset is used to predict the survival of passengers.
    # Create an instance of AutoDataPrep.
    >>> aprep_obj = AutoDataPrep(task_type="Classification", verbose=2)
    # Fit the data.
    >>> aprep_obj.fit(titanic, titanic.survived)
    # Retrieve the data after Auto Data Preparation.
    >>> datas = aprep_obj.get_data()
load
teradataml.automl.autodataprep.AutoDataPrep.load = load(self, table_name)
DESCRIPTION:
    Loads the AutoDataPrep generated data from the database  
    in the session to use it for model training or scoring.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the name of the table containing the information
        of deployed datasets in the database.
        Types: str
RETURNS:
    Dictionary of DataFrames containing the datas generated from AutoDataPrep.
RAISES:
    TeradataMlException, ValueError
EXAMPLES:
    # Create an instance of the AutoDataPrep.
    # Load the data from the table.
    # Create an instance of AutoDataPrep.
    >>> aprep_obj = AutoDataPrep()
    # Load the data from the table.
    >>> data = aprep_obj.load("table_name")
    # Retrieve the data
    >>> print(data)
visualize
teradataml.automl.autodataprep.AutoDataPrep.visualize = visualize(**kwargs)
DESCRIPTION:
    Function visualizes the data using various plots such as heatmap, 
    pair plot, histogram, univariate plot, count plot, box plot, and target distribution.
PARAMETERS:
    data:
9/14/2025, 16:50
Page 1,449 of 1,675
        Required Argument.
        Specifies the input teradataml DataFrame for plotting.
        Types: teradataml Dataframe
    target_column:
        Required Argument.
        Specifies the name of the target column in "data".
        Note:
            * "target_column" must be of numeric type.
        Types: str
    plot_type:
        Optional Argument.
        Specifies the type of plot to be displayed.
        Default Value: "target"
        Permitted Values: 
            * "heatmap": Displays a heatmap of feature correlations.
            * "pair": Displays a pair plot of features.
            * "density": Displays a density plot of features.
            * "count": Displays a count plot of categorical features.
            * "box": Displays a box plot of numerical features.
            * "target": Displays the distribution of the target variable.
            * "all": Displays all the plots.
        Types: str, list of str
    length:
        Optional Argument.
        Specifies the length of the plot.
        Default Value: 10
        Types: int
    breadth:
        Optional Argument.
        Specifies the breadth of the plot.
        Default Value: 8
        Types: int
    columns:
        Optional Argument.
        Specifies the column names to be used for plotting.
        Types: str or list of string
    max_features:
        Optional Argument.
        Specifies the maximum number of features to be used for plotting.
        Default Value: 10
        Note:
            * It applies separately to categorical and numerical features.
        Types: int
    problem_type:
        Optional Argument.
        Specifies the type of problem.
        Permitted Values:
            * 'regression'
            * 'classification'
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException.
EXAMPLES:
    # Import either of AutoML or AutoClassifier or AutoRegressor or Autodataprep
    # from teradataml.
    >>> from teradataml import AutoML
    >>> from teradataml import DataFrame
    >>> load_example_data("teradataml", "titanic")
    >>> titanic_data = DataFrame("titanic")
    # Example 1: Visualize the data using AutoML class.
    >>> AutoML.visualize(data = titanic_data,
    ...                  target_column = 'survived',
    ...                  plot_type = ['heatmap', 'pair', 'histogram', 'target'],
    ...                  length = 10,
    ...                  breadth = 8,
    ...                  max_features = 10,
    ...                  problem_type = 'classification')
    # Example 2: Visualize the data using AutoDataPrep class.
    >>> from teradataml import AutoDataPrep
    >>> obj = AutoDataPrep(task_type="classification")
    >>> obj.fit(data = titanic_data, target_column = 'survived')