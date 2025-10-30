# teradataml OpenSourceML

td_sklearn
An Interface Object for scikit-learn
teradataml.opensource.td_sklearn = <teradataml.opensource._class.Sklearn object>
DESCRIPTION:
    Interface object to access exposed classes and functions of scikit-learn 
    opensource package. All the classes and functions can be run and attributes 
    can be accessed using the object created by "td_sklearn" interface object.
    Refer Teradata Python Package User Guide for more information about OpenML 
    and exposed interface objects.
PARAMETERS:
    None
RETURNS:
    None
EXAMPLES:
    # Load example data.
    >>> load_example_data("openml", ["test_classification", "test_prediction"])
    >>> df = DataFrame("test_classification")
    >>> df.head(3)
                   col2      col3      col4  label
    col1
    -2.560430  0.402232 -1.100742 -2.959588      0
    -3.587546  0.291819 -1.850169 -4.331055      0
    -3.697436  1.576888 -0.461220 -3.598652      0
    >>> df_test = DataFrame("test_prediction")
    >>> df_test.head(3)
                   col2      col3      col4
    col1
    -2.560430  0.402232 -1.100742 -2.959588
    -3.587546  0.291819 -1.850169 -4.331055
    -3.697436  1.576888 -0.461220 -3.598652
    # Get the feature and label data.
    >>> df_x_clasif = df.select(df.columns[:-1])
    >>> df_y_clasif = df.select(df.columns[-1])
    >>> from teradataml import td_sklearn
    >>> dt_cl = td_sklearn.DecisionTreeClassifier(random_state=0)
    >>> dt_cl
    DecisionTreeClassifier(random_state=0)
    # Set the paramaters.
    >>> dt_cl.set_params(random_state=2, max_features="sqrt")
    DecisionTreeClassifier(max_features='sqrt', random_state=2)
    # Get the paramaters.
    >>> dt_cl.get_params()
    {'ccp_alpha': 0.0,
     'class_weight': None,
     'criterion': 'gini',
     'max_depth': None,
     'max_features': 'sqrt',
     'max_leaf_nodes': None,
     'min_impurity_decrease': 0.0,
     'min_impurity_split': None,
     'min_samples_leaf': 1,
     'min_samples_split': 2,
     'min_weight_fraction_leaf': 0.0,
     'random_state': 2,
     'splitter': 'best'}
    # Train the model using fit().
    >>> dt_cl.fit(df_x_clasif, df_y_clasif)
    DecisionTreeClassifier(max_features='sqrt', random_state=2)
9/14/2025, 16:50
Page 1,451 of 1,675
    # Perform prediction.
    >>> dt_cl.predict(df_test)
           col1      col2      col3      col4  decisiontreeclassifier_predict_1
    0  1.105026 -1.949894 -1.537164  0.073171                                 1
    1  1.878349  0.577289  1.795746  2.762539                                 1
    2 -1.130582 -0.020296 -0.710234 -1.440991                                 0
    3 -1.243781  0.280821 -0.437933 -1.379770                                 0
    4 -0.509793  0.492659  0.248207 -0.309591                                 1
    5 -0.345538 -2.296723 -2.811807 -1.993113                                 0
    6  0.709217 -1.481740 -1.247431 -0.109140                                 0
    7 -1.621842  1.713381  0.955084 -0.885921                                 1
    8  2.425481 -0.549892  0.851440  2.689135                                 1
    9  1.780375 -1.749949 -0.900142  1.061262                                 0
    # Perform scoring.
    >>> dt_cl.score(df_x_clasif, df_y_clasif)
       score
    0    1.0
    # Access few attributes.
    >>> dt_cl.classes_
    array([0., 1.])
    >>> dt_cl.feature_importances_
    array([0.06945187, 0.02      , 0.67786339, 0.23268474])
    >>> dt_cl.max_features_
    2
Methods
deploy: Deploy externally trained sklearn model
teradataml.opensource.td_sklearn.deploy = deploy(model_name, model, replace_if_exists=False)
DESCRIPTION:
    Deploys the model to Vantage.
PARAMETERS:
    model_name:
        Required Argument.
        Specifies the unique name of the model to be deployed.
        Types: str
    model:
        Required Argument.
        Specifies the teradataml supported opensource model object that is to be deployed.
        Currently supported models are:
            - sklearn
            - lightgbm
        Types: object
    replace_if_exists:
        Optional Argument.
        Specifies whether to replace the model if a model with the same name already
        exists in Vantage. If this argument is set to False and a model with the same
        name already exists, then the function raises an exception.
        Default Value: False
        Types: bool
RETURNS:
    The opensource object wrapper.
RAISES:
    TeradataMLException if model with "model_name" already exists and the argument
    "replace_if_exists" is set to False.
EXAMPLES:
    # Import required packages and create LinearRegression sklearn object.
    >>> from teradataml import td_sklearn
    >>> from sklearn.linear_model import LinearRegression
    >>> model = LinearRegression(normalize=True)
    # Example 1: Deploy the model to Vantage.
    >>> lin_reg = td_sklearn.deploy("linreg_model_ver_1", model)
    Model is saved.
    >>> lin_reg
    LinearRegression(normalize=True)
    # Example 2: Deploy the model to Vantage with the name same as that of model that
    #            already existed in Vantage.
    >>> lin_reg = td_sklearn.deploy("linreg_model_ver_1", model, replace_if_exists=True)
    Model is deleted.
9/14/2025, 16:50
Page 1,452 of 1,675
    Model is saved.
    >>> lin_reg
    LinearRegression(normalize=True)
load: Load saved models
teradataml.opensource.td_sklearn.load = load(model_name)
DESCRIPTION:
    Loads the model from Vantage based on the interface object on which this function
    is called.
    For example, if the model in "model_name" argument is statsmodel model, then this
    function raises exception if the interface object is `td_sklearn`.
PARAMETERS:
    model_name:
        Required Argument.
        Specifies the name of the model to be loaded.
        Types: str
RETURNS:
    The opensource object wrapper.
RAISES:
    TeradataMlException if model with name "model_name" does not exist.
EXAMPLE:
    >>> from teradataml import td_sklearn
    >>> # Load the model saved in Vantage. Note that the model is saved using
    >>> # `deploy()` of exposed interface object (like `td_sklearn`) or
    >>> # `_OpenSourceObjectWrapper` Object.
    >>> model = td_sklearn.load("linreg_model_ver_1")
    >>> model
    LinearRegression(normalize=True)
_SkLearnObjectWrapper.deploy: Deploy internally trained sklearn model
teradataml.opensource._sklearn._SkLearnObjectWrapper.deploy = deploy(self, model_name, replace_if_exists=False)
DESCRIPTION:
    Deploys the model held by interface object to Vantage.
PARAMETERS:
    model_name:
        Required Argument.
        Specifies the unique name of the model to be deployed.
        Types: str
    replace_if_exists:
        Optional Argument.
        Specifies whether to replace the model if a model with the same name already
        exists in Vantage. If this argument is set to False and a model with the same
        name already exists, then the function raises an exception.
        Default Value: False
        Types: bool
RETURNS:
    The opensource object wrapper.
RAISES:
    TeradataMLException if model with "model_name" already exists and the argument
    "replace_if_exists" is set to False.
EXAMPLES:
    # Import the required libraries and create LinearRegression Opensource object wrapper.
    >>> from teradataml import td_sklearn
    >>> model = td_sklearn.LinearRegression(normalize=True)
    >>> model
    LinearRegression(normalize=True)
    # Example 1: Deploy the model held by LinearRegression Opensource object to Vantage.
    >>> lin_reg = model.deploy("linreg_model_ver_2")
    Model is saved.
    >>> lin_reg
    LinearRegression(normalize=True)
    # Example 2: Deploy the model held by LinearRegression Opensource object to Vantage
    #            with the name same as that of model that already existed in Vantage.
    >>> lin_reg = model.deploy("linreg_model_ver_2", replace_if_exists=True)
    Model is deleted.
    Model is saved.
    >>> lin_reg
    LinearRegression(normalize=True)
td_lightgbm
9/14/2025, 16:50
Page 1,453 of 1,675
An Interface Object for LightGBM
teradataml.opensource.td_lightgbm = <teradataml.opensource._class.Lightgbm object>
DESCRIPTION:
    Interface object to access exposed classes and functions of lightgbm
    opensource package. All the classes and functions can be run and attributes
    can be accessed using the object created by "td_lightgbm" interface object.
    Refer Teradata Python Package User Guide for more information about OpenML
    and exposed interface objects.
PARAMETERS:
    None
RETURNS:
    None
EXAMPLES:
    # Load example data.
    >>> load_example_data("openml", ["test_classification"])
    >>> df = DataFrame("test_classification")
    >>> df.head(3)
                   col2      col3      col4  label
    col1
    -2.560430  0.402232 -1.100742 -2.959588      0
    -3.587546  0.291819 -1.850169 -4.331055      0
    -3.697436  1.576888 -0.461220 -3.598652      0
    # Get the feature and label data.
    >>> df_x = df.select(df.columns[:-1])
    >>> df_y = df.select(df.columns[-1])
    >>> from teradataml import td_lightgbm
    # Example 1: Train the model using train() function.
    # Create lightgbm Dataset object.
    >>> lgbm_data = td_lightgbm.Dataset(data=df_x, label=df_y, free_raw_data=False)
    >>> lgbm_data
    <lightgbm.basic.Dataset object at ...>
    # Train the model.
    >>> model = td_lightgbm.train(params={}, train_set=lgbm_data, num_boost_round=30, valid_sets=[lgbm_data])
    [LightGBM] [Warning] Auto-choosing row-wise multi-threading, the overhead of testing was 0.000043 seconds.
    You can set `force_row_wise=true` to remove the overhead.
    And if memory is not enough, you can set `force_col_wise=true`.
    [LightGBM] [Info] Total Bins 532
    [LightGBM] [Info] Number of data points in the train set: 400, number of used features: 4
    [1]     valid_0's l2: 0.215811
    [2]     valid_0's l2: 0.188138
    [3]     valid_0's l2: 0.166146
    ...
    ...
    [29]    valid_0's l2: 0.042255
    [30]    valid_0's l2: 0.0416953
    >>> model
    <lightgbm.basic.Booster object at ...>
    # Example 2: Train the model using LGBMClassifier sklearn object.
    # Create lightgbm sklearn object.
    >>> lgbm_cl = td_lightgbm.LGBMClassifier()
    >>> lgbm_cl
    LGBMClassifier()
    # Fit/train the model using fit() function.
    >>> lgbm_cl.fit(df_x, df_y)
    LGBMClassifier()
    # Perform prediction.
    >>> lgbm_cl.predict(df_x).head(3)
           col1      col2      col3      col4  lgbmclassifier_predict_1
    0  1.105026 -1.949894 -1.537164  0.073171                         1
    1  1.878349  0.577289  1.795746  2.762539                         1
    2 -1.130582 -0.020296 -0.710234 -1.440991                         0
    # Access attributes.
    >>> lgbm_cl.feature_importances_
    array([ 0, 20, 10, 10])
Methods
deploy: Deploy externally trained model - either Booster object or object from lightgbm.sklearn module
9/14/2025, 16:50
Page 1,454 of 1,675
teradataml.opensource.td_lightgbm.deploy = deploy(model_name, model, replace_if_exists=False)
DESCRIPTION:
    Deploys the model to Vantage.
PARAMETERS:
    model_name:
        Required Argument.
        Specifies the unique name of the model to be deployed.
        Types: str
    model:
        Required Argument.
        Specifies the teradataml supported opensource model object that is to be deployed.
        Currently supported models are:
            - sklearn
            - lightgbm
        Types: object
    replace_if_exists:
        Optional Argument.
        Specifies whether to replace the model if a model with the same name already
        exists in Vantage. If this argument is set to False and a model with the same
        name already exists, then the function raises an exception.
        Default Value: False
        Types: bool
RETURNS:
    The opensource object wrapper.
RAISES:
    TeradataMLException if model with "model_name" already exists and the argument
    "replace_if_exists" is set to False.
EXAMPLES:
    # Import required packages and create LGBMClassifier lightGBM object.
    >>> from teradataml import td_lightgbm
    >>> import lightgbm as lgb
    >>> model = lgb.LGBMClassifier()
    # Example 1: Deploy the LightGBM model to Vantage.
    >>> lgb_model = td_lightgbm.deploy("lgb_model_ver_1", model)
    Model is saved.
    >>> lgb_model
    LGBMClassifier()
    # Example 2: Deploy the LightGBM model to Vantage with the name same as that of model that
    #            already existed in Vantage.
    >>> lgb_model = td_lightgbm.deploy("lgb_model_ver_1", model, replace_if_exists=True)
    Model is deleted.
    Model is saved.
    >>> lgb_model
    LGBMClassifier()
    # Example 3: Deploy LightGBM model trained locally using train() function to Vantage.
    # Create Dataset object locally, assuming pdf_x and pdf_y are the feature and label pandas
    # DataFrames.
    >>> lgbm_data = lgb.Dataset(data=pdf_x, label=pdf_y, free_raw_data=False)
    >>> lgbm_data
    <lightgbm.basic.Dataset object at ....>
    # Train the model using train() function.
    >>> model = lgb.train(params={}, train_set=lgbm_data, num_boost_round=30, valid_sets=[lgbm_data])
    [LightGBM] [Warning] Auto-choosing row-wise multi-threading, the overhead of testing was 0.000043 seconds.
    You can set `force_row_wise=true` to remove the overhead.
    And if memory is not enough, you can set `force_col_wise=true`.
    [LightGBM] [Info] Total Bins 532
    [LightGBM] [Info] Number of data points in the train set: 400, number of used features: 4
    [1] valid_0's l2: 0.215811
    [2] valid_0's l2: 0.188138
    [3] valid_0's l2: 0.166146
    ...
    ...
    [29]        valid_0's l2: 0.042255
    [30]        valid_0's l2: 0.0416953
    # Deploy the model to Vantage.
    >>> lgb_model = td_lightgbm.deploy("lgb_model_ver_2", model)
    >>> lgb_model
    <lightgbm.basic.Booster object at ...>
load: Load saved models
teradataml.opensource.td_lightgbm.load = load(model_name)
DESCRIPTION:
9/14/2025, 16:50
Page 1,455 of 1,675
    Loads the model from Vantage based on the interface object on which this function
    is called.
    For example, if the model in "model_name" argument is statsmodel model, then this
    function raises exception if the interface object is `td_lightgbm`.
PARAMETERS:
    model_name:
        Required Argument.
        Specifies the name of the model to be loaded.
        Types: str
RETURNS:
    The opensource object wrapper.
RAISES:
    TeradataMlException if model with name "model_name" does not exist.
EXAMPLE:
    >>> from teradataml import td_lightgbm
    >>> # Load the model saved in Vantage. Note that the model is saved using
    >>> # `deploy()` of exposed interface object (like `td_lightgbm`) or
    >>> # `_OpenSourceObjectWrapper` Object.
    >>> model = td_lightgbm.load("lgb_model_ver_1")
    >>> model
    LGBMClassifier()
_LightgbmBoosterWrapper.deploy: Deploy internally trained Booster object
teradataml.opensource._lightgbm._LightgbmBoosterWrapper.deploy = deploy(self, model_name, replace_if_exists=False)
DESCRIPTION:
    Deploys the model held by interface object to Vantage.
PARAMETERS:
    model_name:
        Required Argument.
        Specifies the unique name of the model to be deployed.
        Types: str
    replace_if_exists:
        Optional Argument.
        Specifies whether to replace the model if a model with the same name already
        exists in Vantage. If this argument is set to False and a model with the same
        name already exists, then the function raises an exception.
        Default Value: False
        Types: bool
RETURNS:
    The opensource object wrapper.
RAISES:
    TeradataMLException if model with "model_name" already exists and the argument
    "replace_if_exists" is set to False.
EXAMPLES:
    # Import the required libraries and create LGBMClassifier Opensource object wrapper.
    >>> from teradataml import td_lightgbm
    # Example 1: Deploy the model trained using td_lightgbm.train() function to Vantage.
    # Create Dataset object, assuming df_x and df_y are the feature and label teradataml
    # DataFrames.
    >>> lgbm_data = td_lightgbm.Dataset(data=df_x, label=df_y, free_raw_data=False)
    >>> lgbm_data
    <lightgbm.basic.Dataset object at ....>
    # Train the model using `td_lightgbm` interface object.
    >>> model = td_lightgbm.train(params={}, train_set=lgbm_data, num_boost_round=30, valid_sets=[lgbm_data])
    [LightGBM] [Warning] Auto-choosing row-wise multi-threading, the overhead of testing was 0.000043 seconds.
    You can set `force_row_wise=true` to remove the overhead.
    And if memory is not enough, you can set `force_col_wise=true`.
    [LightGBM] [Info] Total Bins 532
    [LightGBM] [Info] Number of data points in the train set: 400, number of used features: 4
    [1] valid_0's l2: 0.215811
    [2] valid_0's l2: 0.188138
    [3] valid_0's l2: 0.166146
    ...
    ...
    [29]        valid_0's l2: 0.042255
    [30]        valid_0's l2: 0.0416953
    # Deploy the model to Vantage.
    >>> lgb_model = model.deploy("lgbm_train_model_ver_2")
    >>> lgb_model
    <lightgbm.basic.Booster object at ...>