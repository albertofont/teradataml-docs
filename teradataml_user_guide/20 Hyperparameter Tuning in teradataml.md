# Hyperparameter Tuning in teradataml

A hyperparameter is a parameter whose value is used to control the learning process of machine learning
algorithms. The procedure of identifying the right set of hyperparameters for a learning algorithm is known
as hyperparameter tuning. The approaches followed to identify optimal hyperparameters from combinations
of hyperparameters (hyperparameter space) vary based on search algorithms. The search algorithms are
approaches to effectively searching for hyperparameters in the hyperparameter space.
**The hyperparameter tuning consist of following key features:**
* Hyperparameter space
* Search algorithm
* Data sampling
* Model training
* Model evaluation
* Optimal hyperparameter identification
**Hyperparameter tuning in teradataml provides following generic search algorithms:**
GridSearch
    * An approach of hyperparameter tuning that involves training a learning model for all possible
    * set of hyperparameter combinations present in hyperparameter space.
RandomSearch
    * An approach of hyperparameter tuning that involves training a learning model for a randomly
    * selected set of hyperparameter combinations from the hyperparameter space.
In addition to identifying optimal hyperparameters for learning models (model trainer functions), teradataml
extends the hyperparameter search functionality to nonmodel trainer functions such as data cleansing
functions and feature engineering functions.
teradataml also offers additional features for hyperparameter tuning, such as parallel execution, early stop,
live log, and hyper-parameterization of input data.
Model trainer functions are evaluatable functions that use the complete features of hyperparameter
tuning. Likewise, nonmodel trainer functions are nonevaluatable functions that use features of
hyperparameter tuning, such as hyperparameterization of function arguments, search algorithms, and
model training functionalities.
## Overview of Hyperparameter Tuning
A hyperparameter is a parameter whose value is used to control the learning process of machine learning
algorithms. The procedure of identifying the right set of hyperparameters for a learning algorithm is known
as hyperparameter tuning. The approaches followed to identify optimal hyperparameters from combinations
18

of hyperparameters (hyperparameter space) vary based on search algorithms. The search algorithms are
approaches to effectively searching for hyperparameters in the hyperparameter space.
**The hyperparameter tuning consist of following key features:**
* Hyperparameter space.
* Search algorithm.
* Data sampling.
* Model training.
* Model evaluation.
* Optimal hyperparameter identification.
**Hyperparameter tuning in teradataml provides following generic search algorithms:**
* GridSearch: An approach of hyperparameter tuning that involves training a learning model for all
* possible set of hyperparameter combinations present in hyperparameter space.
* RandomSearch: An approach of hyperparameter tuning that involves training a learning model for a
* randomly selected set of hyperparameter combinations from the hyperparameter space.
In addition to identifying optimal hyperparameters for learning models (model trainer functions), teradataml
extends the hyperparameter search functionality to various non-model trainer functions such as data
cleansing functions, feature engineering functions, and so on.
teradataml also offers additional features for hyperparameter tuning, such as parallel execution, early stop,
live log, and hyper-parameterization of input data.
Model trainer functions are evaluable functions that utilize the complete features of hyperparameter
tuning. Likewise, non-model trainer functions are non-evaluable functions that utilize certain features of
hyperparameter tuning, such as hyper-parameterization of function arguments, search algorithms, and
model training functionalities.
## GridSearch
Use GridSearch to identify and transform distinct categorical values into numerical values from the input
using Ordinal Encoding.
* Methods of GridSearch
* Properties of GridSearch
* Examples for GridSearch
### Methods of GridSearch
* _init_
* fit
* evaluate
* predict
* get_error_log
18: Hyperparameter Tuning in teradataml

* get_input_data
* get_model
* get_parameter_grid
* is_running
* set_model
_init_
GridSearch is an exhaustive search algorithm that covers all possible parameter values to identify optimal
hyperparameters. It works for teradataml analytic functions from Database Engine 20, BYOM, VAL, and
UAF features.
teradataml GridSearch allows you to perform hyperparameter tuning for all model trainer and non-model
trainer functions.
* When used for model trainer functions:
    * Based on evaluation metrics, search determines best model.
    * All methods and properties can be used.
* When used for non-model trainer functions:
    * You can choose the best output as you see fit to use this.
    * Only  fit  method is supported.
teradataml GridSearch also allows you to use input data as the hyperparameter. This option can be
suitable when the you want to identify the best models for a set of input data. When you pass set of data as
hyperparameter for model trainer function, the search determines the best data along with the best model
based on the evaluation metrics.
**Note:**
configure.temp_object_type="VT" follows sequential execution.
### Required Argument
func
    * Specifies a teradataml analytic function from Database Engine 20, BYOM, VAL,
    * and UAF.
    * Use the  display_analytic_functions()  function for list of functions.
```python
params : Specifies the parameters of a teradataml analytic function.
```
    * The parameters must be in dictionary type:
    * Keys refer to the argument names;
    * Values refer to argument values for corresponding arguments.
18: Hyperparameter Tuning in teradataml

* Note:
    * You can specify the argument value in a tuple to run hyperparameter tunning
    * with different arguments.
    * Model trainer function arguments  id_column ,  input_columns , and
    * target_columns  must be passed in fit() method.
    * All required arguments of non-model trainer function must be passed during
    * GridSearch object creation.
fit
Use the fit() method to perform hyperparameter tuning using GridSearch algorithm.
* In model trainer function, the best parameters are selected based on training results.
* In non- model trainer function, first execution parameter set is selected as the best parameters.
**Optional Arguments:**
* data : Specifies the input teradataml DataFrame for model trainer function.
* Note:
    * This is a required argument for model trainer functions.
    * This argument is ignored for non-model trainer functions.
* This argument can be either a dictionary, or a tuple, or a teradataml DataFrame.
    * If it is a dictionary, then Key represents the label for DataFrame and Value represents
    * the DataFrame.
    * If it is a tuple, then teradataml converts it to dictionary by generating the labels internally.
    * If it is a teradataml DataFrame, then teradataml label it as "DF_0".
* Note:
    * When this argument is passed as a model hyperparameters ( params ), DataFrame need
    * not to be passed in fit() method.
    * This argument can contain a single DataFrame or multiple DataFrames.
    * You can pass multiple DataFrames to argument. Hyperparameter tuning is performed on
    * all the DataFrames for every model parameter.
* evaluation_metric : Specifies the evaluation metrics to be considered for model evaluation.
18: Hyperparameter Tuning in teradataml

* Note:
    * This argument is applicable for model trainer functions.
    * Best model is not selected when evaluation returns non-finite values.
* Permitted Values:
    * Classification: Accuracy, Micro-Precision, Micro-Recall, Micro-F1, Macro-Precision, Macro-
    * Recall, Macro-F1, Weighted-Precision, Weighted-Recall, Weighted-F1.
    * Regression: MAE, MSE, MSLE, MAPE, MPE, RMSE, RMSLE, ME, R2, EV, MPD, MGD.
* Default Value:
    * Classification: Accuracy
    * Regression: MAE
```python
early_stop : Specifies the early stop mechanism value for model trainer functions. Hyperparameter
```
* tuning ends model training when the training model evaluation metric attains the value of
* this argument.
* Note:
    * Early stopping supports only when evaluation returns finite value.
* frac : Specifies the split percentage of rows to be sampled for training dataset and testing dataset.
* The value of this argument is considered as train split percentage and the remaining percentage is
* for test splitting.
* The value must range between (0, 1). Default value is 0.8.
* Note:
    * This argument is not supported for non-model trainer function.
* run_parallel : Specifies the parallel execution functionality of hyperparameter tuning.
* When set to 'True', model functions are executed concurrently. Otherwise, model functions are
* executed sequentially. Default value is 'True'.
* wait : Specifies whether to wait for the completion of execution of hyperparameter tuning or not.
* When set to 'False', hyperparameter tuning is executed in the background and you can use
* is_running  method to check the status. Otherwise it waits until the execution is complete to return the
* control back to user. Default value is 'True'.
* verbose : Specifies whether to log the model training information and display the logs.
    * When set to 0, nothing is logged in the console.
    * When set to 1, progress bar alone is logged in the console.
18: Hyperparameter Tuning in teradataml

* When set to 2, along with progress bar, execution steps and execution time are logged in
    * the console.
* Default value is 0.
* Note:
    * This argument is not significant when the  wait  argument is set to 'False'.
* sample_seed : Specifies the seed value that controls the shuffling applied to the data before applying
* the train test split. Pass an integer for reproducible output across multiple function calls.
* When this argument is not specified, different runs of the query generate different outputs.
* Permitted value is an integer in the range [0, 2147483647].
* Note:
    * Seed is only supported when  stratify_column  is passed, otherwise it is optional.
* stratify_column : Specifies column name that contains the labels indicating which data needs to be
* stratified for train test split.
* sample_id_column : Specifies the input data column name that has the unique identifier for each row
* in the input.
* This argument is required when  sample_seed  argument is present.
* max_time : Specifies the maximum time for the completion of Hyperparameter tuning execution.
* kwargs : Specifies the keyword arguments. Accepts additional arguments required for the teradataml
* analytic function.
evaluate
Use the evaluate() method for evaluation using trained models from Database Engine 20, VAL, and
UAF features.
Evaluation are done using the default trained model.
**Note:**
    * Evaluation is supported for evaluable model-trainer functions.
    * Best model is set as default model by default.
    * Default model can be changed using  set_model  method.
**Optional Arguments:**
* kwargs : Specifies the keyword arguments. Accepts additional arguments required for the teradataml
* analytic function evaluations.
18: Hyperparameter Tuning in teradataml

* If this argument is empty, internal sampled test dataset and arguments are used for evaluation.
* Otherwise, all arguments required with validation data need to be passed for evaluation.
Output teradataml DataFrames can be accessed using attribute references, such as
HPTEvaluateObj.<attribute_name>. Output teradataml DataFrame attribute name is: result.
predict
Use the predict() method for prediction using models generated by model training function from Database
Engine 20, VAL, and UAF features.
Predictions are done using the best trained model.
Predict function is not supported for non-model trainer function.
**Optional Arguments:**
* kwargs : Specifies the keyword arguments. Accepts all merge model predict feature arguments
* required for the teradataml analytic function predictions.
Output teradataml DataFrames can be accessed using attribute references, such as
HPTEvaluateObj.<attribute_name>. Output teradataml DataFrame attribute name is: result.
get_error_log
Use the get_error_log() method to get the error logs of a failed model training in the fit method.
**Required Arguments:**
* model_id : Specifies the unique identifier for model.
* Note:
    * Only failed model training error log is returned.
get_input_data
Use the get_input_data() method to get the input data used by model trainer functions. Unique identifiers
( data_id ) is used to get the training data. In case of unlabeled data such as single dataframe or tuple of
dataframe, default unique identifiers are assigned. Thus, unlabeled training data is retrieved using default
unique identifiers.
18: Hyperparameter Tuning in teradataml

**Note:**
    * For model trainer functions, it only returns input data.
    * For supervised model trainer function (evaluatable functions), it returns both train and test
    * sampled data.
    * For unsupervised-model trainer function (non-evaluatable functions), it returns train data.
**Required Arguments:**
* data_id : Specifies the unique data identifier used for model training.
get_model
Use the get_model() method to get the model.
**Required Arguments:**
* model_id : Specifies the unique identifier for model.
* This function returns object of teradataml analytic functions:
    * For model trainer functions, it returns trained model results.
    * For non-model trainer functions, it returns executed function results.
**Note:**
Attribute references remain the same as that of the function attributes.
get_parameter_grid
Use the get_parameter_grid to get the value of the attribute _parameter_grid.
This function returns a dictionary of the value.
This method does not require parameters.
is_running
Use the is_running() method to check whether hyperparameter tuning is completed or not.
Function returns 'True' when execution is in progress. Otherwise it returns 'False'.
This method does not require parameters.
set_model
Use the set_model() method to set the model to be used for prediction.
18: Hyperparameter Tuning in teradataml

**Required Arguments:**
* model_id : Specifies the unique identifier of the model to be used for prediction.
* Note:
    * This argument is not significant for non-model trainer functions.
### Properties of GridSearch
* best_data_id
* best_model
* best_model_id
* best_params_
* best_sampled_data_
* best_score_
* model_stats
* models
best_data_id
Use the best_data_id property to return a string representing the "data_id" of a sampled data used for
training the best model.
**Note:**
This property is not supported for non-model trainer functions.
best_data_id
Use the best_data_id property to return a string representing the "data_id" of a sampled data used for
training the best model.
**Note:**
This property is not supported for non-model trainer functions.
best_model
Use the best_model property to return the best trained model obtained from hyperparameter tuning.
18: Hyperparameter Tuning in teradataml

**Note:**
This property is not supported for non-model trainer functions.
best_model_id
Use the best_model_id property to return a string representing the model id of the model with the
best score.
**Note:**
This property is not supported for non-model trainer functions.
best_params_
Use the best_params_ property to return a dictionary of the the parameters used for the model with
best score.
**Note:**
This property is not supported for non-model trainer functions.
best_sampled_data_
Use the best_sampled_data_ property to return a list of DataFrames representing the best sampled data
used for training the best model.
**Note:**
This property is not supported for non-model trainer functions.
best_score_
Use the best_score_ property to return a string representing the best score of the model out of all
generated models.
**Note:**
This property is not supported for non-model trainer functions.
18: Hyperparameter Tuning in teradataml

model_stats
Use the model_stats property to return a pandas DataFrame representing the model statistics of the
model with best score.
models
Use the models property to return pandas DataFrame representing metadata of the generated models.
### Examples for GridSearch
* Example 1: Using Hyperparameter Tuning for Model Trainer Function
* Example 2: Input Data Hyper-parameterization for Model Trainer Function Tuning
* Example 3: Hyperparameter Tuning Operations on Non-Model Trainer Function
Example 1: Using Hyperparameter Tuning for Model Trainer Function
This example creates an optimistic SVM regression model using GridSearch optimization algorithm. The
best model from GridSearch optimization is used to predict house value in California.
In this example, teradataml example California housing data is used to build the SVM regression model.
1.
* Example setup.
* a.
    * Load example data from "cal_housing_ex_raw" that contains California housing data.
```python
load_example_data("teradataml", ["cal_housing_ex_raw"])
```
* b.
    * Create teradataml DataFrame objects.
```python
data_input = DataFrame.from_table("cal_housing_ex_raw")
```
* c.
    * Scale "target_columns" with respect to 'STD' value of the column.
```python
fit_obj = ScaleFit(data=data_input,
target_columns=['MedInc', 'HouseAge', 'AveRooms',
'AveBedrms',
'Population', 'AveOccup',
'Latitude', 'Longitude'],
scale_method="USTD")
```
* d.
    * Transform the data.
18: Hyperparameter Tuning in teradataml

```python
transform_obj = ScaleTransform(data=data_input,
object=fit_obj.output,
accumulate=["id", "MedHouseVal"])
```
* e.
    * Sample train and validation dataframe, where 80% data used for model training and 20% used for
    * model validation.
```python
train_val_sample = transform_obj.result.sample(frac=[0.8, 0.2])
train_df = train_val_sample[train_val_sample.sampleid == 1].drop(\
"sampleid", axis = 1)
val_df = train_val_sample[train_val_sample.sampleid == 2].drop(\
"sampleid", axis = 1)
```
* f.
    * Create two training data samples for model optimization.
```python
train_df1 = train_df.iloc[:30]
train_df2 = train_df.iloc[30:]
```
2.
* Define a parameter space and use GridSearch for Hyperparameterization.
* a.
    * Define parameter space for model training.
```python
params = {"input_columns":['MedInc', 'HouseAge', 'AveRooms',
'AveBedrms', 'Population', 'AveOccup',
'Latitude', 'Longitude'],
"response_column":"MedHouseVal",
"model_type":"regression",
"batch_size":(11, 50, 75),
"iter_max":(100, 301),
"lambda1":0.1,
"alpha":0.5,
"iter_num_no_change":60,
"tolerance":0.01,
"intercept":False,
"learning_rate":"INVTIME",
"initial_data":0.5,
"decay_rate":0.5,
"momentum":0.6,
"nesterov_optimization":True,
"local_sgd_iterations":1}
```
* b.
    * Define required argument for model prediction and evaluation.
18: Hyperparameter Tuning in teradataml

```python
eval_params = {"id_column": "id",
"accumulate": "MedHouseVal"}
```
* c.
    * Import trainer function and optimizer.
```python
from teradataml import SVM, GridSearch
```
* d.
    * Initialize the GridSearch optimizer with model trainer function and parameter space required for
    * model training.
```python
gs_obj = GridSearch(func=SVM, params=params)
```
    * Note:
    * Model optimization is initiated using  fit  method.
3.
* Pass single DataFrame for model trainer function and hyperparameter tuning execution viewed using
* progress bar. Perform model optimization for SVM function. Evaluation and prediction arguments are
* passed along with.
```python
gs_obj.fit(data=train_df, verbose=1, **eval_params)
Completed: |████████████████████████████████████████████████████████████|
100% - 6/6
```
* Note:
* All model training has been passed. In case of failure, use  get_error_log  method to retrieve
* corresponding error logs.
4.
* View trained model metadata from hyperparameter tuning using  models  property. Retrieve the model
* metadata of "gs_obj" instance.
```python
gs_obj.models
MODEL_ID DATA_ID                                         PARAMETERS
STATUS       MAE
0    SVM_2    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.229113
1    SVM_3    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.229113
2    SVM_0    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.254733
3    SVM_1    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.254733
4    SVM_4    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
```
18: Hyperparameter Tuning in teradataml

```python
PASS  2.229113
5    SVM_5    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.229113
```
5.
* View the best model identified by GridSearch. Retrieve the best model id identified by
* "gs_obj" instance.
```python
gs_obj.best_model_id
'SVM_0'
```
* Note:
* Identified best model is stored as a default model for future prediction and evaluation operations.
6.
* Perform prediction on validation data using the identified best model.
```python
gs_obj.predict(newdata=val_df, **eval_params)
result Output
id  prediction  MedHouseVal
0  11246    0.350175        2.028
1   5328    0.760091        2.775
2  16736    1.236138        3.152
3   3687    1.026577        1.741
4   8783    0.031345        2.500
5  17768    1.022263        1.601
6   7114    0.224646        2.187
7  18164    0.383037        3.674
8  10966    0.253726        1.896
9  16199    0.468865        0.590
```
7.
* Perform evaluation of validation data using the best model.
```python
gs_obj.evaluate(newdata=val_df, **eval_params)
result Output
MAE      MSE      MSLE        MAPE         MPE      RMSE
RMSLE        ME        R2        EV  MPD  MGD
0  2.011638  4.95115  0.097629  117.909264  116.463401  2.225118  0.312457
3.208421 -1.312606  0.436544  NaN  NaN
```
8.
* View all trained model stats report. Retrieve the model stats of "gs_obj" instance.
18: Hyperparameter Tuning in teradataml

```python
gs_obj.model_stats
MODEL_ID       MAE       MSE      MSLE        MAPE         MPE
RMSE     RMSLE        ME        R2        EV  MPD  MGD
0     SVM_0  2.292239  6.188213  0.286058  110.899315  110.899315
2.487612  0.534843  3.325346 -1.788798  0.579146  NaN  NaN
1     SVM_1  2.018557  6.799822  0.000000   92.159608   92.159608
2.607647  0.000000  5.467911 -2.512063 -0.407573  NaN  NaN
2     SVM_2  2.018557  6.799822  0.000000   92.159608   92.159608
2.607647  0.000000  5.467911 -2.512063 -0.407573  NaN  NaN
3     SVM_3  2.292239  6.188213  0.286058  110.899315  110.899315
2.487612  0.534843  3.325346 -1.788798  0.579146  NaN  NaN
4     SVM_7  2.156224  5.709455  0.127423  101.757168  101.757168
2.389447  0.356964  3.440837 -1.573039  0.522228  NaN  NaN
5     SVM_4  1.960213  6.039734  0.000000   91.243341   91.243341
2.457587  0.000000  4.982941 -2.119482 -0.134890  NaN  NaN
```
9.
* Update default model with other trained model and perform predictions.
* a.
    * Find the best model which is considered as default model.
```python
gs_obj.best_model_id
'SVM_0'
```
* b.
    * Update the default trained model. Default model of GridSearch instance is updated using
    * set_model  method.
```python
gs_obj.set_model(model_id="SVM_1")
```
    * Note:
    * Though the default model is updated, known best model information will remain unchanged.
    * The best model and corresponding information can be retrieved using the  Properties of
    * GridSearch  starting with "best_".
* c.
    * Perform prediction using "SVM_1" model.
```python
gs_obj.predict(newdata=val_df.iloc[:5], **eval_params)
id  prediction  MedHouseVal
0   686    0.202843        1.578
1  2018    0.149868        0.578
2  1754    0.211870        1.651
3   670    0.192414        1.922
4   244    0.247545        1.117
```
18: Hyperparameter Tuning in teradataml

Example 2: Input Data Hyper-parameterization for Model Trainer
Function Tuning
teradataml offers hyper-parameterization of training data for hyperparameter tuning tasks. This example
builds a SVM regression model to predict house value in California.
In this example, teradataml example California housing data is sliced into multiple data and used to build
the SVM regression model.
1.
* Example setup.
* a.
    * Load example data from "cal_housing_ex_raw" that contains California housing data.
```python
load_example_data("teradataml", ["cal_housing_ex_raw"])
```
* b.
    * Create teradataml DataFrame objects.
```python
data_input = DataFrame.from_table("cal_housing_ex_raw")
```
* c.
    * Scale "target_columns" with respect to 'STD' value of the column.
```python
fit_obj = ScaleFit(data=data_input,
target_columns=['MedInc', 'HouseAge', 'AveRooms',
'AveBedrms',
'Population', 'AveOccup',
'Latitude', 'Longitude'],
scale_method="USTD")
```
* d.
    * Transform the data.
```python
transform_obj = ScaleTransform(data=data_input,
object=fit_obj.output,
accumulate=["id", "MedHouseVal"])
```
* e.
    * Sample train and validation dataframe, where 80% data used for model training and 20% used for
    * model validation.
```python
train_val_sample = transform_obj.result.sample(frac=[0.8, 0.2])
train_df = train_val_sample[train_val_sample.sampleid == 1].drop(\
"sampleid", axis = 1)
val_df = train_val_sample[train_val_sample.sampleid == 2].drop(\
"sampleid", axis = 1)
```
* f.
    * Create two training data samples for model optimization.
18: Hyperparameter Tuning in teradataml

```python
train_df1 = train_df.iloc[:30]
train_df2 = train_df.iloc[30:]
```
2.
* Define a parameter space and use GridSearch for Hyperparameterization.
* a.
    * Define parameter space for model training.
```python
params = {"input_columns":['MedInc', 'HouseAge', 'AveRooms',
'AveBedrms', 'Population', 'AveOccup',
'Latitude', 'Longitude'],
"response_column":"MedHouseVal",
"model_type":"regression",
"batch_size":(11, 50, 75),
"iter_max":(100, 301),
"lambda1":0.1,
"alpha":0.5,
"iter_num_no_change":60,
"tolerance":0.01,
"intercept":False,
"learning_rate":"INVTIME",
"initial_data":0.5,
"decay_rate":0.5,
"momentum":0.6,
"nesterov_optimization":True,
"local_sgd_iterations":1}
```
* b.
    * Define required argument for model prediction and evaluation.
```python
eval_params = {"id_column": "id",
"accumulate": "MedHouseVal"}
```
* c.
    * Import trainer function and optimizer.
```python
from teradataml import SVM, GridSearch
```
* d.
    * Initialize the GridSearch optimizer with model trainer function and parameter space required for
    * model training.
```python
gs_obj = GridSearch(func=SVM, params=params)
```
    * Note:
    * Model optimization is initiated using  fit  method.
3.
* Pass multiple training datasets as tuple of DataFrames for model trainer function.
18: Hyperparameter Tuning in teradataml

* DataFrames are passed as hyper-parameterized and hyperparameter tuning execution viewed using
* detailed training logs along with progress bar.
* Parallel execution mode is disabled, and early stop criteria is set.
```python
gs_obj.fit(data=(train_df1, train_df2),
run_parallel=False,
early_stop=0.85,
evaluation_metric="MAE",
verbose=2,
**eval_params)
Model_id:SVM_0 - Run time:11.608s - Status:PASS - MAE:2.2
Model_id:SVM_1 - Run time:11.624s - Status:PASS - MAE:3.11
Model_id:SVM_2 - Run time:13.068s - Status:PASS - MAE:2.2
Model_id:SVM_3 - Run time:11.549s - Status:PASS - MAE:3.11
Model_id:SVM_4 - Run time:11.965s - Status:PASS - MAE:2.187
Model_id:SVM_5 - Run time:11.586s - Status:PASS - MAE:3.11
Model_id:SVM_6 - Run time:12.506s - Status:PASS - MAE:2.187
Model_id:SVM_7 - Run time:11.695s - Status:PASS - MAE:3.11
Model_id:SVM_8 - Run time:11.491s - Status:PASS - MAE:2.187
Model_id:SVM_9 - Run time:14.294s - Status:PASS - MAE:3.11
Model_id:SVM_10 - Run time:11.422s - Status:PASS - MAE:2.187
Model_id:SVM_11 - Run time:15.748s - Status:PASS - MAE:3.11
Completed: |████████████████████████████████████████████████████████████|
100% - 12/12
```
* Note:
* All model training has been passed. In case of failure, use  get_error_log  method to retrieve
* corresponding error logs.
4.
* View hyperparameter tuning trained model metadata using  models  property. Retrieve the model
* metadata of "gs_obj" instance.
```python
gs_obj.models
MODEL_ID    DATA_ID                                       PARAMETERS
STATUS      MAE
0     SVM_0    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.199501
1     SVM_1    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  3.110119
2     SVM_2    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.199501
```
18: Hyperparameter Tuning in teradataml

```python
3     SVM_3    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  3.110119
4     SVM_4    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.187302
5     SVM_5    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  3.110119
6     SVM_6    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.187302
7     SVM_7    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  3.110119
8     SVM_8    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.187302
9     SVM_9    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  3.110119
10   SVM_10    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  2.187302
11   SVM_11    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR
PASS  3.110119
```
* Note:
* DATA_ID column contain unique data identifier used to train the corresponding model. Input
* data can be retrieved using  get_input_data  method.
5.
* View the best model and corresponding information identified by GridSearch.
* a.
    * Retrieve the best model id identified by "gs_obj" instance.
```python
gs_obj.best_model_id
'SVM_4'
```
* b.
    * Retrieve the best data id.
```python
gs_obj.best_data_id
'DF_0'
```
* c.
    * Retrieve the best model of "gs_obj1" instance.
```python
gs_obj.best_model
output_data Output
iterNum      loss       eta  bias
0        3  1.787430  0.028868   0.0
```
18: Hyperparameter Tuning in teradataml

```python
1        5  1.702996  0.022361   0.0
2        6  1.671471  0.020412   0.0
3        7  1.644866  0.018898   0.0
4        9  1.608033  0.016667   0.0
5       10  1.596019  0.015811   0.0
6        8  1.624216  0.017678   0.0
7        4  1.742452  0.025000   0.0
8        2  1.834916  0.035355   0.0
9        1  1.888336  0.050000   0.0
result Output
predictor
estimate                                   value
attribute
-5                             BIC
37.025676                                    None
-9          Learning Rate (Initial)
0.050000                                    None
-11                       Momentum
0.600000                                    None
-14                        Epsilon
0.100000                                    None
-1                   Loss Function         NaN  EPSILON_INSENSITIVE
-3          Number of Observations
24.000000                                    None
7                         Latitude
-0.538928                                    None
5                       Population
0.091047                                    None
-16                         Kernel
NaN                                  LINEAR
-7                           Alpha
0.500000                              Elasticnet
```
    * Note:
    * Identified best model is stored as a default model for future prediction and
    * evaluation operations.
6.
* Perform prediction on validation data using the identified best model.
```python
gs_obj.predict(newdata=val_df, **eval_params)
```
18: Hyperparameter Tuning in teradataml

```python
result Output
id  prediction  MedHouseVal
0  15749   -1.176686        3.500
1   2313   -0.130279        0.863
2   5611    0.736420        1.587
3   5300    1.108423        3.500
4   6558    0.766487        3.594
5   7114    0.762072        2.187
6    670   -1.094195        1.922
7   7581    1.010409        1.334
8  16102   -1.229020        2.841
9   8090    0.404590        1.607
```
7.
* Perform evaluation using internally sampled data using best model.
```python
gs_obj.evaluate()
result Output
MAE       MSE      MSLE       MAPE        MPE      RMSE
RMSLE        ME        R2        EV  MPD  MGD
0  2.187302  6.385763  0.119889  83.446339  83.446339  2.527007  0.346249
4.414041 -2.361604  0.156949  NaN  NaN
```
* Note:
* When validation data is not passed to evaluate() method, it will use internally sampled test data
* for evaluation.
8.
* View all trained models stats report. Retrieve the model stats of "gs_obj" instance.
```python
gs_obj.model_stats
MODEL_ID       MAE        MSE      MSLE        MAPE  ...
ME        R2        EV  MPD  MGD
0     SVM_0  2.199501   6.485457  0.127586   83.595306  ...  4.360486
-2.414085  0.132640  NaN  NaN
1     SVM_1  3.110119  10.385778  0.079247  101.796641  ...  4.151917
-5.607787  0.546405  NaN  NaN
2     SVM_2  2.199501   6.485457  0.127586   83.595306  ...  4.360486
-2.414085  0.132640  NaN  NaN
3     SVM_3  3.110119  10.385778  0.079247  101.796641  ...  4.151917
```
18: Hyperparameter Tuning in teradataml

```python
-5.607787  0.546405  NaN  NaN
4     SVM_4  2.187302   6.385763  0.119889   83.44
9  ...  4.414041
-2.361604  0.156949  NaN  NaN
5     SVM_5  3.110119  10.385778  0.079247  101.796641  ...  4.151917
-5.607787  0.546405  NaN  NaN
6     SVM_6  2.187302   6.385763  0.119889   83.44
9  ...  4.414041
-2.361604  0.156949  NaN  NaN
7     SVM_7  3.110119  10.385778  0.079247  101.796641  ...  4.151917
-5.607787  0.546405  NaN  NaN
8     SVM_8  2.187302   6.385763  0.119889   83.44
9  ...  4.414041
-2.361604  0.156949  NaN  NaN
9     SVM_9  3.110119  10.385778  0.079247  101.796641  ...  4.151917
-5.607787  0.546405  NaN  NaN
10   SVM_10  2.187302   6.385763  0.119889   83.44
9  ...  4.414041
-2.361604  0.156949  NaN  NaN
11   SVM_11  3.110119  10.385778  0.079247  101.796641  ...  4.151917
-5.607787  0.546405  NaN  NaN
[12 rows x 13 columns]
```
9.
* Update default model with other trained model and perform predictions.
* a.
    * Find the best model.
```python
gs_obj.best_model_id
'SVM_4'
```
    * Note:
    * GridSearch identifies 'SVM_4' as a best model and same is considered as default model.
* b.
    * Update the default trained model. Default model of GridSearch instance is updated using
    * set_model  method.
```python
gs_obj.set_model(model_id="SVM_1")
```
* c.
    * Perform prediction using "SVM_1" model.
```python
gs_obj.predict(newdata=val_df.iloc[:5], **eval_params)
result Output
id  prediction  MedHouseVal
0  3687   -1.067513        1.741
1  6044   -0.524250        1.109
```
18: Hyperparameter Tuning in teradataml

```python
2  5611   -1.042297        1.587
3  3593    0.300275        2.676
4   686    0.688285        1.578
```
10. Retrieve any trained model from the GridSearch instance.
```python
gs_obj.get_model("SVM_2")
output_data Output
iterNum      loss       eta  bias
0        3  1.749836  0.028868   0.0
1        5  1.649338  0.022361   0.0
2        6  1.616893  0.020412   0.0
3        7  1.590792  0.018898   0.0
4        9  1.554049  0.016667   0.0
5       10  1.541651  0.015811   0.0
6        8  1.569562  0.017678   0.0
7        4  1.693831  0.025000   0.0
8        2  1.808278  0.035355   0.0
9        1  1.932960  0.050000   0.0
result Output
predictor
estimate                                   value
attribute
-5                             BIC
36.383901                                    None
-9         Learning Rate (Initial)
0.050000                                    None
-11                       Momentum
0.600000                                    None
-14                        Epsilon
0.100000                                    None
-1                   Loss Function         NaN  EPSILON_INSENSITIVE
-3          Number of Observations
24.000000                                    None
7                         Latitude
-0.542485                                    None
5                       Population
0.133593                                    None
-16                         Kernel
```
18: Hyperparameter Tuning in teradataml

```python
NaN                                  LINEAR
-7                           Alpha
0.500000                              Elasticnet
```
Example 3: Hyperparameter Tuning Operations on Non-Model
Trainer Function
teradataml offers hyper-parameterization of parameters for non-model trainer functions using GridSearch
algorithm. This example makes use of hyper-parameterization feature to perform Antiselect function on
titanic data.
In this example, teradataml example titanic data is used to perform Antiselect function.
1.
* Example setup.
* a.
    * Load the example dataset.
```python
load_example_data("teradataml", "titanic")
```
* b.
    * Create teradataml DataFrame.
```python
titanic = DataFrame.from_table("titanic")
```
* c.
    * Slice input data for Hyper-parameterization of data.
```python
train_df1 = titanic.iloc[:200]
train_df2 = titanic.iloc[200:]
```
2.
* Define hyperparameter-tuning for Antiselect (non-model trainer function).
* a.
    * Define the non-model trainer function parameter space. Include input data in parameter space for
    * non-model trainer function.
```python
params = {"data":(train_df1, train_df2), "exclude":(
['survived', 'name', 'age'],
['survived', 'age'],
["ticket", "parch", "age"],
["ticket", "parch", "sex", "age"])}
```
    * Note:
    * Any argument in 'params' can be hyper-parameterized.
* b.
    * Import non-model trainer function and optimizer.
```python
from teradataml import Antiselect, GridSearch
```
* c.
    * Initialize the GridSearch optimizer with non-model trainer function and parameter space required
    * for non-model training.
18: Hyperparameter Tuning in teradataml

```python
gs_obj = GridSearch(func=Antiselect, params=params)
```
3.
* Perform execution of Antiselect function in sequential mode, and enable progress bar by setting
* verbose level to “1”.
```python
gs_obj.fit(run_parallel=True, verbose=1)
completed: |████████████████████████████████████████████████████████████|
100% - 8/8
```
* Note:
* Best model selection is not supported by hyperparameter tuning for non-model trainer function.
4.
* View the non-model trainer function execution metadata. Retrieve the model metadata of
* "gs_obj" instance.
```python
gs_obj.models
MODEL_ID                                         PARAMETERS STATUS
0  ANTISELECT_0  {'data': '"ALICE"."ml__select__169839646305843      PASS
1  ANTISELECT_3  {'data': '"ALICE"."ml__select__169839646305843      PASS
2  ANTISELECT_2  {'data': '"ALICE"."ml__select__169839646305843      PASS
3  ANTISELECT_1  {'data': '"ALICE"."ml__select__169839646305843      PASS
4  ANTISELECT_4  {'data': '"ALICE"."ml__select__169840161255008      PASS
5  ANTISELECT_5  {'data': '"ALICE"."ml__select__169840161255008      PASS
6  ANTISELECT_6  {'data': '"ALICE"."ml__select__169840161255008      PASS
7  ANTISELECT_7  {'data': '"ALICE"."ml__select__169840161255008      PASS
```
* Note:
* All model training has been passed. In case of failure, use  get_error_log  method to retrieve
* corresponding error logs.
5.
* Retrieve the parameter grid for non-model trainer function. Retrieve "gs_obj" object's parameter grid.
```python
pprint.pprint(gs_obj.get_parameter_grid())
[{'data': '"ALICE"."ml__select__169839646305843"',
'exclude': ['survived', 'name', 'age']},
{'data': '"ALICE"."ml__select__169839646305843"',
'exclude': ['survived', 'age']},
{'data': '"ALICE"."ml__select__169839646305843"',
'exclude': ['ticket', 'parch', 'age']},
{'data': '"ALICE"."ml__select__169839646305843"',
```
18: Hyperparameter Tuning in teradataml

```python
'exclude': ['ticket', 'parch', 'sex', 'age']},
{'data': '"ALICE"."ml__select__169840161255008"',
'exclude': ['survived', 'name', 'age']},
{'data': '"ALICE"."ml__select__169840161255008"',
'exclude': ['survived', 'age']},
{'data': '"ALICE"."ml__select__169840161255008"',
'exclude': ['ticket', 'parch', 'age']},
{'data': '"ALICE"."ml__select__169840161255008"',
'exclude': ['ticket', 'parch', 'sex', 'age']}]
```
6.
* Get the non-model function execution result from GridSearch instance "ANTISELECT_3".
```python
gs_obj.get_model("ANTISELECT_3")
result Output
passenger  survived  pclass                                                 name  sibsp     fare
cabin embarked
0          3         1       3                               Heikkinen, Miss. Laina      0   7.9250
None        S
1          5         0       3                             Allen, Mr. William Henry      0   8.0500
None        S
2          6         0       3                                     Moran, Mr. James      0   8.4583
None        Q
3          7         0       1                              McCarthy, Mr. Timothy J      0  51.8625
E46        S
4          9         1       3    Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)      0  11.1333
None        S
5         10         1       2                  Nasser, Mrs. Nicholas (Adele Achem)      1  30.0708
None        C
6          8         0       3                       Palsson, Master. Gosta Leonard      3  21.0750
None        S
7          4         1       1         Futrelle, Mrs. Jacques Heath (Lily May Peel)      1  53.1000
C123        S
8          2         1       1  Cumings, Mrs. John Bradley (Florence Briggs Thayer)      1  71.2833
C85        C
9          1         0       3                              Braund, Mr. Owen Harris      1   7.2500
None        S
```
Example 4: Parallelization in Hyperparameter Tuning for Model and Non-
Model Trainer Functions
teradataml provides the capability for parallel execution of hyperparameters for both model and non-
model trainer functions using GridSearch algorithm. This example executes DecisionForest (model
trainer function) and AntiSelect (non-model trainer function) on the admission dataset.
In this example, admission data is used to demonstrate the parallel capability.
1.
* Example setup.
* a.
    * Load the example dataset.
```python
load_example_data("teradataml", "admission_train")
```
* b.
    * Create teradataml DataFrame.
```python
train_df = DataFrame.from_table("admission_train")
```
2.
* Execute model trainer function DecisionForest.
18: Hyperparameter Tuning in teradataml

* a.
    * Define hyperparameter tuning for DecisionForest function.
```python
Model training parameters
model_params = {"input_columns":(['gpa', 'stats',
'programming', 'masters']),
"response_column":'admitted',
"max_depth":(1,15,25,20),
"num_trees":(5,15,50),
"tree_type":'CLASSIFICATION'}
Model evaluation parameters
eval_params = {"id_columnn": "id",
"accumulate": "admitted"
}
Import model trainer and optimizer
from teradataml import DecisionForest, GridSearch
Initialize the GridSearch optimizer with model trainer
function and parameter space required for model training.
gs_obj = GridSearch(func=DecisionForest, params=model_params)
```
* b.
    * Execute the hyperparameter  fit  function.
    * The default setting for  run_parallel  is True.
```python
gs_obj.fit(data=train_df, run_parallel=True, verbose=2,
evaluation_metric="Micro-f1", **eval_params)
Model_id:DECISIONFOREST_0 - Run time:27.721s - Status:PASS - MICRO-
F1:0.5
Model_id:DECISIONFOREST_3 - Run time:27.722s - Status:PASS - MICRO-
F1:0.5
Model_id:DECISIONFOREST_1 - Run time:27.722s - Status:PASS - MICRO-
F1:0.5
Model_id:DECISIONFOREST_2 - Run time:27.722s - Status:PASS - MICRO-
F1:0.5
Model_id:DECISIONFOREST_4 - Run time:28.249s - Status:PASS - MICRO-
F1:0.5
Model_id:DECISIONFOREST_5 - Run time:28.249s - Status:PASS - MICRO-
F1:0.167
Model_id:DECISIONFOREST_7 - Run time:28.239s - Status:PASS - MICRO-
F1:0.167
Model_id:DECISIONFOREST_6 - Run time:28.242s - Status:PASS - MICRO-
F1:0.5
```
18: Hyperparameter Tuning in teradataml

```python
Model_id:DECISIONFOREST_9 - Run time:27.638s - Status:PASS - MICRO-
F1:0.5
Model_id:DECISIONFOREST_11 - Run time:27.368s - Status:PASS - MICRO-
F1:0.167
Model_id:DECISIONFOREST_10 - Run time:27.624s - Status:PASS - MICRO-
F1:0.167
Model_id:DECISIONFOREST_8 - Run time:27.661s - Status:PASS - MICRO-
F1:0.167
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾
```
    * ｜  100% - 12/12
    * Note:
    * Different  evaluation_metric  can be used for training different models in
    * hyperparameter tuning.
* c.
    * View the results using  models  and  model_stats  properties.
```python
Trained models can be viewed using models property
gs_obj.models
MODEL_ID                DATA_ID
PARAMETERS                              STATUS        MICRO-F1
0        DECISIONFOREST_0        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.500000
1        DECISIONFOREST_3        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.500000
2        DECISIONFOREST_1        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.500000
3        DECISIONFOREST_2        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.500000
4        DECISIONFOREST_4        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.500000
5        DECISIONFOREST_5        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.166667
6        DECISIONFOREST_7        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.166667
7        DECISIONFOREST_6        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.500000
8        DECISIONFOREST_9        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.500000
9        DECISIONFOREST_11       DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.166667
10       DECISIONFOREST_10       DF_0        {'input_columns': ['gpa',
```
18: Hyperparameter Tuning in teradataml

```python
'stats', 'programmin       PASS        0.166667
11       DECISIONFOREST_8        DF_0        {'input_columns': ['gpa',
'stats', 'programmin       PASS        0.166667
Additional Performance metrics can be viewd using
model_stats property
gs_obj.model_stats
MODEL_ID           ACCURACY   MICRO-PRECISION  MICRO-RECALL  MICRO-F1
MACRO-PRECISION  MACRO-RECALL  MACRO-F1  WEIGHTED-PRECISION  WEIGHTED-
RECALL  WEIGHTED-F1
0   DECISIONFOREST_0  0.500000  0.500000          0.500000
0.500000  0.625000           0.7          0.485714  0.875000
0.500000         0.542857
1   DECISIONFOREST_3  0.500000  0.500000          0.500000
0.500000  0.625000           0.7          0.485714  0.875000
0.500000         0.542857
2   DECISIONFOREST_1  0.500000  0.500000          0.500000
0.500000  0.625000           0.7          0.485714  0.875000
0.500000         0.542857
3   DECISIONFOREST_2  0.500000  0.500000          0.500000
0.500000  0.625000           0.7          0.485714  0.875000
0.500000         0.542857
4   DECISIONFOREST_4  0.500000  0.500000          0.500000
0.500000  0.625000           0.7          0.485714  0.875000
0.500000         0.542857
5   DECISIONFOREST_5  0.166667  0.166667          0.166667
0.166667  0.083333           0.5          0.142857  0.027778
0.166667         0.047619
6   DECISIONFOREST_7  0.166667  0.166667          0.166667
0.166667  0.083333           0.5          0.142857  0.027778
0.166667         0.047619
7   DECISIONFOREST_6  0.500000  0.500000          0.500000
0.500000  0.625000           0.7          0.485714  0.875000
0.500000         0.542857
8   DECISIONFOREST_9  0.500000  0.500000          0.500000
0.500000  0.625000           0.7          0.485714  0.875000
0.500000         0.542857
9   DECISIONFOREST_11 0.166667  0.166667          0.166667
0.166667  0.083333           0.5          0.142857  0.027778
0.166667         0.047619
10  DECISIONFOREST_10 0.166667  0.166667          0.166667
0.166667  0.083333           0.5          0.142857  0.027778
```
18: Hyperparameter Tuning in teradataml

```python
0.166667         0.047619
11  DECISIONFOREST_8  0.166667  0.166667          0.166667
0.166667  0.083333           0.5          0.142857  0.027778
0.166667         0.047619
```
3.
* Execute non-model trainer function Antiselect.
* a.
    * Define the parameter space for Antiselect function.
```python
Define the non-model trainer function parameter space.
params = { "data":train_df,
"exclude":(['stats', 'programming', 'masters'],
['id', 'admitted'],
['admitted', 'gpa', 'stats'],
['masters'],
['admitted', 'gpa', 'stats', 'programming'])}
Import non-model trainer function and optimizer.
from teradataml import Antiselect, GridSearch
Initialize the GridSearch optimizer with non-model trainer
function and parameter space required for non-model training.
gs_obj = GridSearch(func=Antiselect, params=params)
```
* b.
    * Execute hyperparameter tunning with Antiselect in parallel.
```python
Execute Antiselect in parallel
gs_obj.fit(verbose=2)
Model_id:ANTISELECT_3 - Run time:5.878s
- Status:PASS
Model_id:ANTISELECT_2 - Run time:5.882s
- Status:PASS
Model_id:ANTISELECT_1 - Run time:5.882s
- Status:PASS
Model_id:ANTISELECT_0 - Run time:5.883s
- Status:PASS
Model_id:ANTISELECT_4 - Run time:4.402s
- Status:PASS
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾
```
    * ｜  100% - 5/5
* c.
    * View the non-model trainer function execution metadata.
```python
Retrieve the model metadata of "gs_obj" instance.
gs_obj.models
```
18: Hyperparameter Tuning in teradataml

```python
MODEL_ID
PARAMETERS                          STATUS
0      ANTISELECT_3      {'data':
'"ALICE"."ml__select__170973834455250         PASS
1      ANTISELECT_2      {'data':
'"ALICE"."ml__select__170973834455250         PASS
2      ANTISELECT_1      {'data':
'"ALICE"."ml__select__170973834455250         PASS
3      ANTISELECT_0      {'data':
'"ALICE"."ml__select__170973834455250         PASS
4      ANTISELECT_4      {'data':
'"ALICE"."ml__select__170973834455250         PASS
```
* Note:
* All the properties, arguments and functions in previous examples are also applicable here for
* model and non-model trainer functions.
Example 5: Early Stopping Methods in Hyperparameter Tunning
teradataml GridSearch provides the capability of early stopping hyperparameter tuning based on
**the following:**
* Time based: Hyperparameter tuning is stopped once the maximum time is reached, thereby ceasing
* the optimization of hyperparameters.
* Metrics based: Hyperparameter tuning is terminated once a trained model satisfies the specified
* minimum or maximum thresholds for the respective performance metrics, as dictated by the
* evaluation criteria.
* Note:
    * Metrics based method cannot be used for Non-Model Trainer function.
**Note:**
Both time and metrics methods can be used simultaneously, and hyperparameter tuning stops when
either of these two methods satisfies the stopping condition.
Example 5 Setup
The following setup steps apply to all examples for Example 5: Early Stopping Methods in
Hyperparameter Tunning.
18: Hyperparameter Tuning in teradataml

### Example Setup
* Load example dataset iris data from teradataml.
```python
load_example_data('teradataml','iris_input')
```
* Create teradataml DataFrame.
```python
df = DataFrame.from_table('iris_input')
```
* Scale "target_columns" with respect to 'Range' value of the column.
```python
scale_fit = ScaleFit(data=df,
target_columns=['sepal_length', 'sepal_width',
'petal_length', 'petal_width'],
scale_method="Range")
```
* Transform the data.
```python
scale_transform = ScaleTransform(data=df,
object=scale_fit.output,
accumulate=["id", "species"])
```
* Create data for training.
```python
data = scale_transform.result
```
Example 5.1: Time Based Early Stopping for Model Trainer Function
This example shows time based early stopping for model trainer function XGBoost.
1.
* Define hyperparameter space and GridSearch with XGBoost.
* a.
    * Define model training parameters.
```python
model_params = {"input_columns":['sepal_length', 'sepal_width',
'petal_length', 'petal_width'],
"response_column" :'species',
"max_depth":(5,10,15),
"lambda1" :(1000.0,0.001),
"model_type" :"Classification",
"seed":32,
"shrinkage_factor":0.1,
"iter_num":(5, 50)}
```
* b.
    * Define evaluation parameters.
18: Hyperparameter Tuning in teradataml

```python
eval_params = {"id_column": "id",
"accumulate":"species",
"model_type":'Classification',
"object_order_column":['task_index', 'tree_num',
'iter','class_num', 'tree_order']
}
```
* c.
    * Import model trainer function and optimizer.
```python
from teradataml import XGBoost, GridSearch
```
* d.
    * Initialize the GridSearch optimizer with model trainer function and parameter space required for
    * model training.
```python
gs_obj = GridSearch(func=XGBoost, params=model_params)
```
2.
* Execute hyperparameter tunning with max time.
* This step fits the hyperparameters in parallel with  max_time  argument set to 30 seconds.
```python
gs_obj.fit(data=data, max_time=30, verbose=2, **eval_params)
Model_id:XGBOOST_2 - Run time:33.277s - Status:PASS
- ACCURACY:0.933
Model_id:XGBOOST_3 - Run time:33.276s - Status:PASS
- ACCURACY:0.933
Model_id:XGBOOST_0 - Run time:33.279s - Status:PASS
- ACCURACY:0.967
Model_id:XGBOOST_1 - Run time:33.278s - Status:PASS
- ACCURACY:0.933
Computing:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
33% - 4/12
```
* As shown in the output, four models are trained, as the maximum number of trained models in
* parallel is set to 4.
* Any additional models are skipped due to reaching the maximum time allowed for running
* hyperparameter tuning.
3.
* View hyperparameter tuning model metadata using  models  and  model_stats  properties.
* a.
    * View trained model using models property.
```python
gs_obj.models
MODEL_ID     DATA_ID
PARAMETERS       STATUS    ACCURACY
```
18: Hyperparameter Tuning in teradataml

```python
0     XGBOOST_2     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     PASS     0.933333
1     XGBOOST_4     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     SKIP     NaN
2     XGBOOST_5     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     SKIP     NaN
3     XGBOOST_6     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     SKIP     NaN
4     XGBOOST_7     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     SKIP     NaN
5     XGBOOST_8     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     SKIP     NaN
6     XGBOOST_9     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     SKIP     NaN
7     XGBOOST_10    DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     SKIP     NaN
8     XGBOOST_11    DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     SKIP     NaN
9     XGBOOST_3     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     PASS     0.933333
10    XGBOOST_0     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     PASS     0.966667
11    XGBOOST_1     DF_0     {'input_columns': ['sepal_length',
'sepal_widt...     PASS     0.933333
```
    * Note:
    * The status "SKIP" indicates that the model was not trained due to reaching the maximum
    * time limit.
* b.
    * View additional performance metrics using model_stats property.
```python
gs_obj.model_stats
MODEL_ID   ACCURACY MICRO-PRECISION   MICRO-RECALL   MICRO-F1
MACRO-PRECISION   MACRO-RECALL   MACRO-F1   WEIGHTED-PRECISION
WEIGHTED-RECALL   WEIGHTED-F1
0   XGBOOST_3   1.000   1.000   1.000   1.000   1.000   1.000   1.000
1.000   1.000   1.000
1   XGBOOST_4   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
2   XGBOOST_5   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
```
18: Hyperparameter Tuning in teradataml

```python
3   XGBOOST_6   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
4   XGBOOST_7   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
5   XGBOOST_8   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
6   XGBOOST_9   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
7   XGBOOST_10  NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
8   XGBOOST_11  NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
9   XGBOOST_2   1.000   1.000   1.000   1.000   1.000   1.000   1.000
1.000   1.000   1.000
10  XGBOOST_1   0.967   0.967   0.967   0.967   0.972   0.933   0.948
0.969   0.967   0.966
11  XGBOOST_0   0.967   0.967   0.967   0.967   0.972   0.933   0.948
0.969   0.967   0.966
```
Example 5.2: Time Based Early Stopping for Non-Model Trainer Function
This example shows time based early stopping for non-model trainer function Antiselect.
1.
* Define hyperparameter tuning for Antiselect.
* a.
    * Define the non-model trainer function parameter space.
```python
non_model_trainer_params = {"data":data, "exclude":(
['petal_length', 'petal_width'],
['sepal_length',
'sepal_width', 'petal_length'],
['id', 'sepal_length', 'sepal_width'],
['petal_width'],
['petal_width', 'species'],
['sepal_width', 'petal_length',
'petal_width', 'species'],
['sepal_width', 'petal_length',
'petal_width', 'species', 'id'],
['sepal_length', 'sepal_width',])
}
```
* b.
    * Import non-model trainer function and optimizer.
```python
from teradataml import Antiselect, GridSearch
```
18: Hyperparameter Tuning in teradataml

* c.
    * Initialize the GridSearch optimizer with non-model trainer function and parameter space required
    * for non-model training.
```python
as_obj =
GridSearch(func=Antiselect, params=non_model_trainer_params)
```
2.
* Run Antiselect with  max_time  set to 5 seconds.
```python
as_obj.fit(max_time=5, verbose=1)
Computing:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
50% - 4/8
```
3.
* View the result of hyperparameter tunning using  models  property.
```python
as_obj.models
MODEL_ID
PARAMETERS      STATUS
0    ANTISELECT_3    {'data':
'"AUTOML_USER"."ml__td_sqlmr_out__170...    PASS
1    ANTISELECT_4    {'data':
'"AUTOML_USER"."ml__td_sqlmr_out__170...    SKIP
2    ANTISELECT_5    {'data':
'"AUTOML_USER"."ml__td_sqlmr_out__170...    SKIP
3    ANTISELECT_6    {'data':
'"AUTOML_USER"."ml__td_sqlmr_out__170...    SKIP
4    ANTISELECT_7    {'data':
'"AUTOML_USER"."ml__td_sqlmr_out__170...    SKIP
5    ANTISELECT_0    {'data':
'"AUTOML_USER"."ml__td_sqlmr_out__170...    PASS
6    ANTISELECT_2    {'data':
'"AUTOML_USER"."ml__td_sqlmr_out__170...    PASS
7    ANTISELECT_1    {'data':
'"AUTOML_USER"."ml__td_sqlmr_out__170...    PASS
```
Example 5.3: Metrics Based Early Stopping for Model Trainer Function
This example shows metrics based early stopping for model trainer function XGBoost.
1.
* Define hyperparameter space and GridSearch with XGBoost.
* a.
    * Define model training parameters.
```python
model_params = {"input_columns":['sepal_length', 'sepal_width',
'petal_length', 'petal_width'],
```
18: Hyperparameter Tuning in teradataml

```python
"response_column" :'species',
"max_depth":(5,10,15),
"lambda1" :(1000.0,0.001),
"model_type" :"Classification",
"seed":32,
"shrinkage_factor":0.1,
"iter_num":(5, 50)}
```
* b.
    * Define evaluation parameters.
```python
eval_params = {"id_column": "id",
"accumulate":"species",
"model_type":'Classification',
"object_order_column":['task_index', 'tree_num',
'iter','class_num', 'tree_order']
}
```
* c.
    * Import model trainer function and optimizer.
```python
from teradataml import XGBoost, GridSearch
```
* d.
    * Initialize the GridSearch optimizer with model trainer function and parameter space required for
    * model training.
```python
gs_obj = GridSearch(func=XGBoost, params=model_params)
```
2.
* Execute hyperparameter tunning with early stop.
* This step runs  fit  with  early_stop  and evaluation metric.
```python
gs_obj.fit(data=data, evaluation_metric = 'Micro-f1', early_stop=0.9,
verbose=2, **eval_params)
Model_id:XGBOOST_3 - Run time:32.816s - Status:PASS - MICRO-
F1:1.0
Model_id:XGBOOST_2 - Run time:32.816s - Status:PASS - MICRO-
F1:1.0
Model_id:XGBOOST_1 - Run time:32.816s - Status:PASS - MICRO-
F1:0.967
Model_id:XGBOOST_0 - Run time:32.818s - Status:PASS - MICRO-
F1:0.967
Computing:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
33% - 4/12
```
18: Hyperparameter Tuning in teradataml

* Note:
* Various evaluation metrics can be utilized with early stop to take advantage of the early
* stopping functionality.
3.
* View hyperparameter tuning trained model metadata using  models  and  model_stats  properties.
* a.
    * View trained model using models property.
```python
gs_obj.models
MODEL_ID    DATA_ID
PARAMETERS       STATUS    MICRO-F1
0    XGBOOST_3    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    PASS    1.000000
1    XGBOOST_4    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
2    XGBOOST_5    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
3    XGBOOST_6    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
4    XGBOOST_7    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
5    XGBOOST_8    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
6    XGBOOST_9    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
7    XGBOOST_10   DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
8    XGBOOST_11   DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
9    XGBOOST_2    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    PASS    1.000000
10   XGBOOST_1    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    PASS    0.966667
11   XGBOOST_0    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    PASS    0.966667
```
    * Note:
    * The status "SKIP" indicates that the model was not trained, as the previously trained model
    * before it was able to achieve the desired threshold or a better value of evaluation metrics..
* b.
    * View additional performance metrics using model_stats property.
18: Hyperparameter Tuning in teradataml

```python
gs_obj.model_stats
MODEL_ID   ACCURACY   MICRO-PRECISION   MICRO-RECALL   MICRO-F1   MACRO-
PRECISION   MACRO-RECALL   MACRO-F1   WEIGHTED-PRECISION   WEIGHTED-
RECALL   WEIGHTED-F1
0   XGBOOST_3   1.000   1.000   1.000   1.000   1.000   1.000   1.000
1.000   1.000   1.000
1   XGBOOST_4   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
2   XGBOOST_5   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
3   XGBOOST_6   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
4   XGBOOST_7   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
5   XGBOOST_8   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
6   XGBOOST_9   NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
7   XGBOOST_10  NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
8   XGBOOST_11  NaN     NaN     NaN     NaN     NaN     NaN     NaN
NaN     NaN     NaN
9   XGBOOST_2   1.000   1.000   1.000   1.000   1.000   1.000   1.000
1.000   1.000   1.000
10  XGBOOST_1   0.967   0.967   0.967   0.967   0.972   0.933   0.948
0.969   0.967   0.966
11  XGBOOST_0   0.967   0.967   0.967   0.967   0.972   0.933   0.948
0.969   0.967   0.966
```
## RandomSearch
* Methods of RandomSearch
* Properties of RandomSearch
* Examples for RandomSearch
### Methods of RandomSearch
* _init_
* fit
* evaluate
* predict
18: Hyperparameter Tuning in teradataml

* get_error_log
* get_input_data
* get_model
* get_parameter_grid
* is_running
* set_model
_init_
RandomSearch algorithm performs random sampling on hyperparameter space to identify optimal
hyperparameters. It works for teradataml analytic functions from Database Engine 20, BYOM, VAL, and
UAF features.
teradataml RandomSearch allows user to perform hyperparameter tuning for all model trainer and
non-model trainer functions.
* When used for model trainer functions:
    * Based on evaluation metrics, search determines best model.
    * All methods and properties can be used.
* When used for non-model trainer functions:
    * You can choose the best output as you see fit to use this.
    * Only  fit  method is supported.
teradataml RandomSearch also allows you to use input data as the hyperparameter. This option can be
suitable when you want to identify the best models for a set of input data. When you pass set of data as
hyperparameter for model trainer function, the search determines the best data along with the best model
based on the evaluation metrics.
**Note:**
configure.temp_object_type="VT" follows sequential execution.
### Required Parameters
func
    * Specifies a teradataml analytic function from Database Engine 20, BYOM, VAL, and UAF.
    * Use the  display_analytic_functions()  function for list of functions.
params
    * Specifies the parameters of a teradataml analytic function.
    * The parameters must be in dictionary type:
    * Keys refer to the argument names;
18: Hyperparameter Tuning in teradataml

* Values refer to argument values for corresponding arguments.
    * Note:
    * You can specify the argument value in a tuple to run hyperparameter tunning with
    * different arguments.
    * Model trainer function arguments  id_column ,  input_columns , and  target_columns
    * must be passed in fit() method.
    * All required arguments of non-model trainer function must be passed during
    * RandomSearch object creation.
### Optional Argument
n_iter
    * Specifies the number of iterations random search need to be performed.
    * Default value is 10.
    * Note:
    * The value of this argument must be less than the size of parameter populations.
fit
Use the fit() method to perform hyperparameter tuning using RandomSearch algorithm.
* In model trainer function, the best parameters are selected based on training results.
* In non- model trainer function, first execution parameter set is selected as the best parameters.
**Optional Arguments:**
* data : Specifies the input teradataml DataFrame for model trainer function.
* Note:
    * This is a required argument for model trainer functions.
    * This argument is ignored for non-model trainer functions.
* This argument can be either a dictionary, or a tuple, or a teradataml DataFrame.
    * If it is a dictionary, then Key represents the label for DataFrame and Value represents
    * the DataFrame.
    * If it is a tuple, then teradataml converts it to dictionary by generating the labels internally.
    * If it is a teradataml DataFrame, then teradataml label it as "DF_0".
18: Hyperparameter Tuning in teradataml

* Note:
    * When this argument is passed as a model hyperparameters ( params ), DataFrame need
    * not to be passed in fit() method.
    * This argument can contain a single DataFrame or multiple DataFrames.
    * You can pass multiple DataFrames to argument. Hyperparameter tuning is performed on
    * all the DataFrames for every model parameter.
* evaluation_metric : Specifies the evaluation metrics to be considered for model evaluation.
* Note:
    * This argument is applicable for model trainer functions.
    * Best model is not selected when evaluation returns non-finite values.
* Permitted Values:
    * Classification: Accuracy, Micro-Precision, Micro-Recall, Micro-F1, Macro-Precision, Macro-
    * Recall, Macro-F1, Weighted-Precision, Weighted-Recall, Weighted-F1.
    * Regression: MAE, MSE, MSLE, MAPE, MPE, RMSE, RMSLE, ME, R2, EV, MPD, MGD.
* Default Value:
    * Classification: Accuracy
    * Regression: MAE
```python
early_stop : Specifies the early stop mechanism value for model trainer functions. Hyperparameter
```
* tuning ends model training when the training model evaluation metric attains the value of
* this argument.
* Note:
    * Early stopping supports only when evaluation returns finite value.
* frac : Specifies the split percentage of rows to be sampled for training dataset and testing dataset.
* The value of this argument is considered as train split percentage and the remaining percentage is
* for test splitting.
* The value must range between (0, 1). Default value is 0.8.
* Note:
    * This argument is not supported for non-model trainer function.
* run_parallel : Specifies the parallel execution functionality of hyperparameter tuning.
* When set to 'True', model functions are executed concurrently. Otherwise, model functions are
* executed sequentially. Default value is 'True'.
18: Hyperparameter Tuning in teradataml

* wait : Specifies whether to wait for the completion of execution of hyperparameter tuning or not.
* When set to 'False', hyperparameter tuning is executed in the background and you can use
* is_running  method to check the status. Otherwise it waits until the execution is complete to return the
* control back to user. Default value is 'True'.
* verbose : Specifies whether to log the model training information and display the logs.
    * When set to 0, nothing is logged in the console.
    * When set to 1, progress bar alone is logged in the console.
    * When set to 2, along with progress bar, execution steps and execution time are logged in
    * the console.
* Default value is 0.
* Note:
    * This argument is not significant when the  wait  argument is set to 'False'.
* sample_seed : Specifies the seed value that controls the shuffling applied to the data before applying
* the train test split. Pass an integer for reproducible output across multiple function calls. When this
* argument is not specified, different runs of the query generate different outputs.
* Permitted value is an integer in the range [0, 2147483647].
* Note:
    * Seed is only supported when  stratify_column  is passed, otherwise it is optional.
* stratify_column : Specifies column name that contains the labels indicating which data needs to be
* stratified for train test split.
* sample_id_column : Specifies the input data column name that has the unique identifier for each row
* in the input.
* This argument is required when  sample_seed  argument is present.
* max_time : Specifies the maximum time for the completion of Hyperparameter tuning execution.
* kwargs : Specifies the keyword arguments. Accepts additional arguments required for the teradataml
* analytic function.
evaluate
Use the evaluate() method for evaluation using trained models from Database Engine 20, VAL, and
UAF features.
Evaluation are done using the default trained model.
18: Hyperparameter Tuning in teradataml

**Note:**
    * Evaluation is supported for evaluable model-trainer functions.
    * Best model is set as default model by default.
    * Default model can be changed using  set_model  method.
**Optional Arguments:**
* kwargs : Specifies the keyword arguments. Accepts additional arguments required for the teradataml
* analytic function evaluations.
* If this argument is empty, internal sampled test dataset and arguments are used for evaluation.
* Otherwise, all arguments required with validation data need to be passed for evaluation.
Output teradataml DataFrames can be accessed using attribute references, such as
HPTEvaluateObj.<attribute_name>. Output teradataml DataFrame attribute name is: result.
predict
Use the predict() method for prediction using models generated by model training function from Database
Engine 20, VAL, and UAF features.
Predictions are done using the best trained model.
Predict function is not supported for non-model trainer function.
**Optional Arguments:**
* kwargs : Specifies the keyword arguments. Accepts all merge model predict feature arguments
* required for the teradataml analytic function predictions.
Output teradataml DataFrames can be accessed using attribute references, such as
HPTEvaluateObj.<attribute_name>. Output teradataml DataFrame attribute name is: result.
get_error_log
Use the get_error_log() method to get the error logs of a failed model training in the fit method.
**Required Arguments:**
* model_id : Specifies the unique identifier for model.
* Note:
    * Only failed model training error log is returned.
18: Hyperparameter Tuning in teradataml

get_input_data
Use the get_input_data() method to get the input data used by model trainer functions. Unique identifiers
( data_id ) is used to get the training data. In case of unlabeled data such as single dataframe or tuple of
dataframe, default unique identifiers are assigned. Thus, unlabeled training data is retrieved using default
unique identifiers.
**Note:**
    * For model trainer functions, it only returns input data.
    * For supervised model trainer function (evaluatable functions), it returns both train and test
    * sampled data.
    * For unsupervised-model trainer function (non-evaluatable functions), it returns train data.
**Required Arguments:**
* data_id : Specifies the unique data identifier used for model training.
get_model
Use the get_model() method to get the model.
**Required Arguments:**
* model_id : Specifies the unique identifier for model.
* This function returns object of teradataml analytic functions:
    * For model trainer functions, it returns trained model results.
    * For non-model trainer functions, it returns executed function results.
**Note:**
Attribute references remain the same as that of the function attributes.
get_parameter_grid
Use the get_parameter_grid to get the value of the attribute _parameter_grid.
This function returns a dictionary of the value.
This method does not require parameters.
18: Hyperparameter Tuning in teradataml

is_running
Use the is_running() method to check whether hyperparameter tuning is completed or not.
Function returns 'True' when execution is in progress. Otherwise it returns 'False'.
This method does not require parameters.
set_model
Use the set_model() method to set the model to be used for prediction.
**Required Arguments:**
* model_id : Specifies the unique identifier of the model to be used for prediction.
* Note:
    * This argument is not significant for non-model trainer functions.
### Properties of RandomSearch
* best_data_id
* best_model
* best_model_id
* best_params_
* best_sampled_data_
* best_score_
* model_stats
* models
best_data_id
Use the best_data_id property to return a string representing the "data_id" of a sampled data used for
training the best model.
**Note:**
This property is not supported for non-model trainer functions.
best_model
Use the best_model property to return the best trained model obtained from hyperparameter tuning.
18: Hyperparameter Tuning in teradataml

**Note:**
This property is not supported for non-model trainer functions.
best_model_id
Use the best_model_id property to return a string representing the model id of the model with the
best score.
**Note:**
This property is not supported for non-model trainer functions.
best_params_
Use the best_params_ property to return a dictionary of the the parameters used for the model with
best score.
**Note:**
This property is not supported for non-model trainer functions.
best_sampled_data_
Use the best_sampled_data_ property to return a list of DataFrames representing the best sampled data
used for training the best model.
**Note:**
This property is not supported for non-model trainer functions.
best_score_
Use the best_score_ property to return a string representing the best score of the model out of all
generated models.
**Note:**
This property is not supported for non-model trainer functions.
18: Hyperparameter Tuning in teradataml

model_stats
Use the model_stats property to return a pandas DataFrame representing the model statistics of the
model with best score.
models
Use the models property to return pandas DataFrame representing metadata of the generated models.
### Examples for RandomSearch
* Example 1: Using Hyperparameter Tuning for Model Trainer Function
* Example 2: Input Data Hyper-parameterization for Model Trainer Function Tuning
* Example 3: Hyperparameter Tuning Operations on Non-Model Trainer Function
Example 1: Using Hyperparameter Tuning for Model Trainer Function
This example creates an optimistic DecisionForest classification model using RandomSearch
optimization algorithm. The best model from RandomSearch optimization is used to classify iris flower.
Perform hyperparameter-tuning on DecisionForest model trainer function for classification task.
In this example, teradataml example iris data is used to build the DecisionForest classification model.
1.
* Example Setup.
* a.
    * Load example data.
```python
load_example_data("byom", "iris_input")
```
* b.
    * Create two samples of input data: sample 1 has 90% of total rows and sample 2 has 10% of
    * total rows.
```python
iris_sample = iris_input.sample(frac=[0.9, 0.1])
```
* c.
    * Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is
    * not required for training model.
```python
iris_train = iris_sample[iris_sample.sampleid ==
"1"].drop("sampleid", axis = 1)
```
* d.
    * Create validation dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as
    * it is not required for scoring.
```python
iris_val = iris_sample[iris_sample.sampleid == "2"].drop("sampleid",
axis = 1)
```
18: Hyperparameter Tuning in teradataml

2.
* Define the parameter space and use RandomSearch for hyperparameterization.
* a.
    * Define parameter space for model training.
```python
params = {"input_columns":["sepal_length", "sepal_width",
"petal_length", "petal_width"],
"response_column":"species",
"tree_type":"classification",
"ntree":tuple(set(round(random.uniform(20, 500)) for i
in range(50))),
"tree_size":(100, 200),
"nodesize":10,
"variance":tuple(set(round(random.random(), 2) for i
in range(20))),
"max_depth":tuple(set(round(random.uniform(2, 20)) for i
in range(6))),
"maxnum_categorical":20,
"mtry":30,
"mtry_seed":100,
"seed":100}
```
* b.
    * Define required argument for model prediction and evaluation.
```python
eval_params = {"id_column": "id",
"accumulate": "species"}
```
* c.
    * Import trainer function and optimizer.
```python
from teradataml import DecisionForest, RandomSearch
```
* d.
    * Initialize the RandomSearch optimizer with model trainer function and parameter space required
    * for model training.
```python
rs_obj = RandomSearch(func=DecisionForest, params=params, n_iter=4)
```
    * Note:
    * Model optimization is initiated using  fit  method.
3.
* Pass a single DataFrame for model training. Perform model optimization for DecisionForest
* function.Evaluation and prediction arguments are passed along with training dataframe.
* Parallel execution mode is disabled, and early stop criteria is set.
```python
rs_obj.fit(data=iris_train, run_parallel=False,
early_stop=0.93, **eval_params)
```
18: Hyperparameter Tuning in teradataml

* Note:
* All model training has been passed. In case of failure, use  get_error_log  method to retrieve
* corresponding error logs.
* Note:
* Model optimization will be stopped once  early_stop  criteria is achieved. Otherwise,
* Hyperparameter tuning is performed for specified iterations.
4.
* View trained model metadata from hyperparameter tuning using  models  property. Retrieve the model
* metadata of "rs_obj" instance.
```python
rs_obj.models
MODEL_ID DATA_ID
PARAMETERS STATUS  ACCURACY
0  DECISIONFOREST_0    DF_0  {'input_columns': ['sepal_length',
'sepal_widt      PASS  0.925926
1  DECISIONFOREST_1    DF_0  {'input_columns': ['sepal_length',
'sepal_widt      PASS  0.925926
2  DECISIONFOREST_2    DF_0  {'input_columns': ['sepal_length',
'sepal_widt      PASS  0.925926
3  DECISIONFOREST_3    DF_0  {'input_columns': ['sepal_length',
'sepal_widt      PASS  0.925926
```
5.
* View the best model and corresponding information identified by RandomSearch.
* a.
    * Retrieve the best model id identified by "rs_obj" instance.
```python
rs_obj.best_model_id
'DECISIONFOREST_0'
```
* b.
    * Retrieve the best data id.
```python
rs_obj.best_data_id
'DF_0'
```
* c.
    * Retrieve the best model of "rs_obj" instance.
```python
rs_obj.best_model
result Output
task_index  tree_num
```
18: Hyperparameter Tuning in teradataml

```python
tree_order
classification_tree
0           1         0           0
{"id_":1,"size_":87,"maxDepth_":10,"responseCounts_":
{"1":28,"2":32,"3":27},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":2.600000,"attr_":"petal_length","type_":"CLASSIFICATION_NU
MERIC_SPLIT","score_":0.664817,"scoreImprove_":0.328172,"leftNodeSize_":2
8,"rightNodeSize_":59},"leftChild_":
{"id_":2,"size_":28,"maxDepth_":9,"label_":"1","responseCounts_":
{"1":28},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":3,"size_":59,"maxDepth_":9,"responseCounts_":
{"2":32,"3":27},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":1.700000,"attr_":"petal_width","type_":"CLASSIFICATION_NUM
ERIC_SPLIT","score_":0.496409,"scoreImprove_":0.336645,"leftNodeSize_":32
,"rightNodeSize_":27},"leftChild_":
{"id_":6,"size_":32,"maxDepth_":8,"label_":"2","responseCounts_":
{"2":32},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":7,"size_":27,"maxDepth_":8,"label_":"3","responseCounts_":
{"3":27},"nodeType_":"CLASSIFICATION_LEAF"}}}
1           0         0           0
{"id_":1,"size_":86,"maxDepth_":10,"responseCounts_":
{"1":32,"2":29,"3":25},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":2.600000,"attr_":"petal_length","type_":"CLASSIFICATION_NU
MERIC_SPLIT","score_":0.663332,"scoreImprove_":0.351101,"leftNodeSize_":3
2,"rightNodeSize_":54},"leftChild_":
{"id_":2,"size_":32,"maxDepth_":9,"label_":"1","responseCounts_":
{"1":32},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":3,"size_":54,"maxDepth_":9,"responseCounts_":
{"2":29,"3":25},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":1.750000,"attr_":"petal_width","type_":"CLASSIFICATION_NUM
ERIC_SPLIT","score_":0.497257,"scoreImprove_":0.312231,"leftNodeSize_":29
,"rightNodeSize_":25},"leftChild_":
{"id_":6,"size_":29,"maxDepth_":8,"label_":"2","responseCounts_":
```
18: Hyperparameter Tuning in teradataml

```python
{"2":29},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":7,"size_":25,"maxDepth_":8,"label_":"3","responseCounts_":
{"3":25},"nodeType_":"CLASSIFICATION_LEAF"}}}
```
* d.
    * Retrieve the best parameters.
```python
rs_obj.best_params_
{'input_columns': ['sepal_length', 'sepal_width', 'petal_length',
'petal_width'], 'response_column': 'species', 'tree_type':
'classification', 'ntree': 453, 'tree_size': 100, 'nodesize':
10, 'variance': 0.25, 'max_depth': 10, 'maxnum_categorical':
20, 'mtry': 30, 'mtry_seed': 100, 'seed': 100,
'data': '"ALICE"."ml__select__169836386746840"'}
```
* e.
    * Retrieve the best sampled data.
```python
rs_obj.best_sampled_data_
[{'data':
sepal_length sepal_width  petal_length petal_width species
id
141           6.7          3.1           5.6          2.4        3
139           6.0          3.0           4.8          1.8        3
118           7.7          3.8           6.7          2.2        3
61            5.0          2.0           3.5          1.0        2
59            6.6          2.9           4.6          1.3        2
38            4.9          3.6           1.4          0.1        1
78            6.7          3.0           5.0          1.7        2
36            5.0          3.2           1.2          0.2        1
40            5.1          3.4           1.5          0.2        1
17            5.4          3.9           1.3          0.4        1},
{'newdata':
sepal_length sepal_width  petal_length petal_width species
id
5             5.0          3.6           1.4          0.2        1
58            4.9          2.4           3.3          1.0        2
77            6.8          2.8           4.8          1.4        2
99            5.1          2.5           3.0          1.1        2
28            5.2          3.5           1.5          0.2        1
89            5.6          3.0           4.1          1.3        2
106           7.6          3.0           6.6          2.1        3
125           6.7          3.3           5.7          2.1        3
```
18: Hyperparameter Tuning in teradataml

```python
70            5.6          2.5           3.9          1.1        2
43            4.4          3.2           1.3          0.2        1}]
```
* Note:
* Identified best model is stored as a default model for future prediction and evaluation operations.
6.
* Perform prediction on validation data using the identified best model.
```python
rs_obj.predict(newdata=iris_val, **eval_params)
result Output
species   id  prediction  confidence_lower  confidence_upper
0        2   54           2               1.0               1.0
1        1    8           1               1.0               1.0
2        1   23           1               1.0               1.0
3        2   85           2               1.0               1.0
4        3  138           3               1.0               1.0
5        1   14           1               1.0               1.0
6        1   46           1               1.0               1.0
7        3  107           2               1.0               1.0
8        2   81           2               1.0               1.0
9        3  113           3               1.0               1.0
```
7.
* Perform evaluation using internally sampled data using the best model.
* Note:
* If validation data is not passed to  evaluate  method, it will use internally sampled test data
* for evaluation.
```python
rs_obj.evaluate(newdata=iris_val, **eval_params)
output_data Output
SeqNum                                              Metric  MetricValue
0       3  Micro-Recall                                                1.0
1       5  Macro-Precision                                             1.0
2       6  Macro-Recall                                                1.0
3       7  Macro-F1                                                    1.0
4       9  Weighted-Recall                                             1.0
5      10  Weighted-F1                                                 1.0
6       8  Weighted-Precision                                          1.0
7       4  Micro-F1                                                    1.0
```
18: Hyperparameter Tuning in teradataml

```python
8       2  Micro-Precision                                             1.0
9       1  Accuracy                                                    1.0
result Output
Prediction  Mapping  CLASS_1  CLASS_2  CLASS_3  Precision  Recall
F1  Support
SeqNum
2               3  CLASS_3        0        0        5        1.0     1.0
1.0        5
1               2  CLASS_2        0        5        0        1.0     1.0
1.0        5
0               1  CLASS_1        5        0        0        1.0     1.0
1.0        5
```
8.
* View all trained model stats report. Retrieve the model stats of "rs_obj" instance.
```python
rs_obj.model_stats
MODEL_ID  ACCURACY  MICRO-PRECISION       WEIGHTED-PRECISION
WEIGHTED-RECALL  WEIGHTED-F1
0  DECISIONFOREST_0  0.925926         0.925926
0.939394         0.925926        0.925
1  DECISIONFOREST_1  0.925926         0.925926
0.939394         0.925926        0.925
2  DECISIONFOREST_2  0.925926         0.925926
0.939394         0.925926        0.925
3  DECISIONFOREST_3  0.925926         0.925926
0.939394         0.925926        0.925
[4 rows x 11 columns]
```
9.
* Update default model with other trained model and perform predictions.
* a.
    * Find the best model which is considered as default model.
```python
rs_obj.best_model_id
'DECISIONFOREST_0'
```
* b.
    * Update the default trained model of RandomSearch instance using  set_model  method.
```python
rs_obj.set_model(model_id="DECISIONFOREST_1")
```
* c.
    * Perform prediction using "DECISIONFOREST_1" model.
18: Hyperparameter Tuning in teradataml

```python
rs_obj.predict(newdata=iris_val.iloc[:5], **eval_params)
result Output
species  id  prediction  confidence_lower  confidence_upper
0        1  17           1               1.0               1.0
1        1  26           1               1.0               1.0
2        1  19           1               1.0               1.0
3        1  15           1               1.0               1.0
4        1   6           1               1.0               1.0
```
10. Retrieve any trained model from the RandomSearch instance using  get_model .
```python
rs_obj.get_model("DECISIONFOREST_3")
result Output
task_index  tree_num
tree_order
classification_tree
0           1         0           0
{"id_":1,"size_":87,"maxDepth_":6,"responseCounts_":
{"1":28,"2":32,"3":27},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":2.600000,"attr_":"petal_length","type_":"CLASSIFICATION_NUMER
IC_SPLIT","score_":0.664817,"scoreImprove_":0.328172,"leftNodeSize_":28,"rig
htNodeSize_":59},"leftChild_":
{"id_":2,"size_":28,"maxDepth_":5,"label_":"1","responseCounts_":
{"1":28},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":3,"size_":59,"maxDepth_":5,"responseCounts_":
{"2":32,"3":27},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":1.700000,"attr_":"petal_width","type_":"CLASSIFICATION_NUMERI
C_SPLIT","score_":0.496409,"scoreImprove_":0.336645,"leftNodeSize_":32,"righ
tNodeSize_":27},"leftChild_":
```
18: Hyperparameter Tuning in teradataml

```python
{"id_":6,"size_":32,"maxDepth_":4,"label_":"2","responseCounts_":
{"2":32},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":7,"size_":27,"maxDepth_":4,"label_":"3","responseCounts_":
{"3":27},"nodeType_":"CLASSIFICATION_LEAF"}}}
1           0         0           0
{"id_":1,"size_":86,"maxDepth_":6,"responseCounts_":
{"1":32,"2":29,"3":25},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":2.600000,"attr_":"petal_length","type_":"CLASSIFICATION_NUMER
IC_SPLIT","score_":0.663332,"scoreImprove_":0.351101,"leftNodeSize_":32,"rig
htNodeSize_":54},"leftChild_":
{"id_":2,"size_":32,"maxDepth_":5,"label_":"1","responseCounts_":
{"1":32},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":3,"size_":54,"maxDepth_":5,"responseCounts_":
{"2":29,"3":25},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":1.750000,"attr_":"petal_width","type_":"CLASSIFICATION_NUMERI
C_SPLIT","score_":0.497257,"scoreImprove_":0.312231,"leftNodeSize_":29,"righ
tNodeSize_":25},"leftChild_":
{"id_":6,"size_":29,"maxDepth_":4,"label_":"2","responseCounts_":
{"2":29},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":7,"size_":25,"maxDepth_":4,"label_":"3","responseCounts_":
{"3":25},"nodeType_":"CLASSIFICATION_LEAF"}}}
```
Example 2: Input Data Hyper-parameterization for Model Trainer
Function Tuning
teradataml’s RandomSearch offers hyper-parameterization of training data for hyperparameter tuning
tasks. This example builds a DecisionForest classification model to classify iris flower. Perform
hyperparameter-tuning on DecisionForest model trainer function for classification task.
In this example, teradataml example iris data is used to build the DecisionForest classification model.
1.
* Example Setup.
* a.
    * Load example data.
```python
load_example_data("byom", "iris_input")
```
* b.
    * Create teradataml DataFrame.
```python
iris_input = DataFrame("iris_input")
```
* c.
    * Create two samples of input data: sample 1 has 90% of total rows and sample 2 has 10% of
    * total rows.
```python
iris_sample = iris_input.sample(frac=[0.9, 0.1])
```
18: Hyperparameter Tuning in teradataml

* d.
    * Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is
    * not required for training model.
```python
iris_train = iris_sample[iris_sample.sampleid ==
"1"].drop("sampleid", axis = 1)
```
* e.
    * Create validation dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as
    * it is not required for scoring.
```python
iris_val = iris_sample[iris_sample.sampleid == "2"].drop("sampleid",
axis = 1)
```
2.
* Define a parameter space and use RandomSearch for input data hyperparameterization.
* a.
    * Create two slices of training data for this use case.
```python
train_df1 = iris_train.iloc[:60]
train_df2 = iris_train.iloc[60:]
```
* b.
    * Define parameter space for model training.
```python
params = {"input_columns":["sepal_length", "sepal_width",
"petal_length", "petal_width"],
"response_column":"species",
"tree_type":"classification",
"ntree":tuple(set(round(random.uniform(20, 500)) for i
in range(50))),
"tree_size":(100, 200),
"nodesize":10,
"variance":tuple(set(round(random.random(), 2) for i
in range(20))),
"max_depth":tuple(set(round(random.uniform(2, 20)) for i
in range(6))),
"maxnum_categorical":20,
"mtry":30,
"mtry_seed":100,
"seed":100}
```
* c.
    * Define required argument for model prediction and evaluation.
```python
eval_params = {"id_column": "id",
"accumulate": "species"}
```
* d.
    * Import trainer function and optimizer.
```python
from teradataml import DecisionForest, RandomSearch
```
18: Hyperparameter Tuning in teradataml

* e.
    * Initialize the RandomSearch optimizer with model trainer function and parameter space required
    * for model training.
```python
rs_obj = RandomSearch(func=DecisionForest, params=params, n_iter=4)
```
    * Note:
    * Model optimization is initiated using  fit  method.
3.
* Perform model optimization for DecisionForest function.
* Pass single DataFrame for model trainer function and hyperparameter tuning execution viewed using
* progress bar.Evaluation and prediction arguments are passed along with training dataframe.
```python
rs_obj.fit(**eval_params)
```
* Note:
* data  argument is not required for fit() method. Labeled dataframes passed in  params  argument
* as a hyperparameter.
4.
* View hyperparameter tuning trained model metadata using  models  property. Retrieve the model
* metadata of "rs_obj" instance.
```python
rs_obj.models
MODEL_ID DATA_ID
PARAMETERS STATUS  ACCURACY
0  DECISIONFOREST_1  data-2  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       0.8
1  DECISIONFOREST_3  data-2  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       0.8
2  DECISIONFOREST_0  data-1  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       1.0
3  DECISIONFOREST_2  data-1  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       1.0
4  DECISIONFOREST_5  data-2  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       0.8
5  DECISIONFOREST_7  data-2  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       0.8
6  DECISIONFOREST_4  data-1  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       1.0
7  DECISIONFOREST_6  data-1  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       1.0
8  DECISIONFOREST_9  data-2  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       0.8
```
18: Hyperparameter Tuning in teradataml

```python
9  DECISIONFOREST_8  data-1  {'input_columns': ['sepal_length',
'sepal_widt...   PASS       0.8
```
* Collectively 10 models are built because 'n' iteration is performed on all the input data.
* Note:
* All model training has been passed. In case of failure, use  get_error_log  method to retrieve
* corresponding error logs.
5.
* View the best model identified by RandomSearch. Retrieve the best model id identified by
* "rs_obj" instance.
```python
rs_obj.best_model_id
'DECISIONFOREST_0'
```
* Note:
* Identified best model is stored as a default model for future prediction and evaluation operations.
6.
* Perform prediction on validation data using the identified best model.
```python
rs_obj.predict(newdata=iris_val, **eval_params)
result Output
species   id  prediction  confidence_lower  confidence_upper
0        3  106           2               1.0               1.0
1        3  136           2               1.0               1.0
2        2   71           2               1.0               1.0
3        2   61           2               1.0               1.0
4        2   91           2               1.0               1.0
5        1    1           1               1.0               1.0
6        3  149           2               1.0               1.0
7        3  128           2               1.0               1.0
8        2   95           2               1.0               1.0
9        2   85           2               1.0               1.0
```
7.
* Perform evaluation on validation data using the best model.
```python
rs_obj.evaluate()
output_data Output
SeqNum                                              Metric  MetricValue
```
18: Hyperparameter Tuning in teradataml

```python
0       3  Micro-Recall                                                1.0
1       5  Macro-Precision                                             1.0
2       6  Macro-Recall                                                1.0
3       7  Macro-F1                                                    1.0
4       9  Weighted-Recall                                             1.0
5      10  Weighted-F1                                                 1.0
6       8  Weighted-Precision                                          1.0
7       4  Micro-F1                                                    1.0
8       2  Micro-Precision                                             1.0
9       1  Accuracy                                                    1.0
result Output
Prediction  Mapping  CLASS_1  CLASS_2  Precision  Recall
F1  Support
SeqNum
1               2  CLASS_2        0        6        1.0     1.0
1.0        6
0               1  CLASS_1        6        0        1.0     1.0
1.0        6
```
* Note:
* When validation data is not passed to evaluate() method, it will use internally sampled test data
* for evaluation.
8.
* View all trained models stats report. Retrieve the model stats of "rs_obj" instance.
```python
rs_obj.model_stats
MODEL_ID  ACCURACY  MICRO-PRECISION  ...  WEIGHTED-PRECISION
WEIGHTED-RECALL  WEIGHTED-F1
0  DECISIONFOREST_1       0.8              0.8  ...
0.875              0.8      0.80543
1  DECISIONFOREST_3       0.8              0.8  ...
0.875              0.8      0.80543
2  DECISIONFOREST_0       1.0              1.0  ...
1.000              1.0      1.00000
3  DECISIONFOREST_2       1.0              1.0  ...
1.000              1.0      1.00000
4  DECISIONFOREST_5       0.8              0.8  ...
0.875              0.8      0.80543
5  DECISIONFOREST_7       0.8              0.8  ...
```
18: Hyperparameter Tuning in teradataml

```python
0.875              0.8      0.80543
6  DECISIONFOREST_4       1.0              1.0  ...
1.000              1.0      1.00000
7  DECISIONFOREST_6       1.0              1.0  ...
1.000              1.0      1.00000
8  DECISIONFOREST_9       0.8              0.8  ...
0.875              0.8      0.80543
9  DECISIONFOREST_8       0.8              0.8  ...
0.875              0.8      0.80543
[10 rows x 11 columns]
```
* Note:
* Model stats provide additional evaluation metrics report.
9.
* Update default model with other trained model and perform predictions.
* a.
    * Find the best model.
```python
rs_obj.best_model_id
'DECISIONFOREST_0'
```
    * Note:
    * RandomSearch identifies 'DECISIONFOREST_0' as a best model and same is considered
    * as default model.
* b.
    * Update the default trained model. Default model of RandomSearch instance is updated using
    * set_model  method.
```python
rs_obj.set_model(model_id="DECISIONFOREST_4")
```
* c.
    * Perform prediction using "DECISIONFOREST_4" model.
```python
rs_obj.predict(newdata=iris_val.iloc[:5], **eval_params)
result Output
species  id  prediction  confidence_lower  confidence_upper
0        1  26           1               1.0               1.0
1        1  29           1               1.0               1.0
2        1  28           1               1.0               1.0
3        1  13           1               1.0               1.0
4        1   6           1               1.0               1.0
```
18: Hyperparameter Tuning in teradataml

* Note:
    * Though the default model is updated, known best model information will remain unchanged.
    * The best model and corresponding information can be retrieved using the  Properties of
    * RandomSearch  starting with "best_".
10. Retrieve the identified best training data.
```python
rs_obj.get_input_data(data_id=rs_obj.best_data_id)
[{'data':     sepal_length  sepal_width  petal_length  petal_width
species  id
36           5.0          3.2           1.2          0.2        1
26           5.0          3.0           1.6          0.2        1
5            5.0          3.6           1.4          0.2        1
17           5.4          3.9           1.3          0.4        1
34           5.5          4.2           1.4          0.2        1
13           4.8          3.0           1.4          0.1        1
53           6.9          3.1           4.9          1.5        2
11           5.4          3.7           1.5          0.2        1
15           5.8          4.0           1.2          0.2        1
7            4.6          3.4           1.4          0.3        1},
{'newdata':     sepal_length  sepal_width  petal_length  petal_width
species  id
38              4.9          3.6           1.4          0.1        1
62              5.9          3.0           4.2          1.5        2
25              4.8          3.4           1.9          0.2        1
51              7.0          3.2           4.7          1.4        2
31              4.8          3.1           1.6          0.2        1
29              5.2          3.4           1.4          0.2        1
48              4.6          3.2           1.4          0.2        1
27              5.0          3.4           1.6          0.4        1
52              6.4          3.2           4.5          1.5        2
57              6.3          3.3           4.7          1.6        2}]
```
11. Retrieve any trained model using RandomSearch instance.
```python
rs_obj.get_model("DECISIONFOREST_1")
result Output
task_index  tree_num
tree_order
```
18: Hyperparameter Tuning in teradataml

```python
classification_tree
0           0         0
0
{"id_":1,"size_":89,"maxDepth_":5,"responseCounts_":
{"2":37,"3":52},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":1.750000,"attr_":"petal_width","type_":"CLASSIFICATION_NUMERI
C_SPLIT","score_":0.485797,"scoreImprove_":0.485797,"leftNodeSize_":37,"righ
tNodeSize_":52},"leftChild_":
{"id_":2,"size_":37,"maxDepth_":4,"label_":"2","responseCounts_":
{"2":37},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":3,"size_":52,"maxDepth_":4,"label_":"3","responseCounts_":
{"3":52},"nodeType_":"CLASSIFICATION_LEAF"}}
1           1         0           0
{"id_":1,"size_":87,"maxDepth_":5,"responseCounts_":
{"2":36,"3":51},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":4.650000,"attr_":"petal_length","type_":"CLASSIFICATION_NUMER
IC_SPLIT","score_":0.485137,"scoreImprove_":0.399870,"leftNodeSize_":32,"rig
htNodeSize_":55},"leftChild_":
{"id_":2,"size_":32,"maxDepth_":4,"label_":"2","responseCounts_":
{"2":32},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
```
18: Hyperparameter Tuning in teradataml

```python
{"id_":3,"size_":55,"maxDepth_":4,"responseCounts_":
{"3":51,"2":4},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":6.050000,"attr_":"sepal_length","type_":"CLASSIFICATION_NUMER
IC_SPLIT","score_":0.134876,"scoreImprove_":0.030094,"leftNodeSize_":10,"rig
htNodeSize_":45},"leftChild_":
{"id_":6,"size_":10,"maxDepth_":3,"responseCounts_":
{"2":4,"3":6},"nodeType_":"CLASSIFICATION_NODE","split_":
{"splitValue_":5.050000,"attr_":"petal_length","type_":"CLASSIFICATION_NUMER
IC_SPLIT","score_":0.480000,"scoreImprove_":0.055172,"leftNodeSize_":6,"righ
tNodeSize_":4},"leftChild_":
{"id_":12,"size_":6,"maxDepth_":2,"label_":"3","responseCounts_":
{"3":6},"nodeType_":"CLASSIFICATION_LEAF"},"rightChild_":
{"id_":13,"size_":4,"maxDepth_":2,"label_":"2","responseCounts_":
{"2":4},"nodeType_":"CLASSIFICATION_LEAF"}},"rightChild_":
{"id_":7,"size_":45,"maxDepth_":3,"label_":"3","responseCounts_":
{"3":45},"nodeType_":"CLASSIFICATION_LEAF"}}}
```
* Note:
* Any trained model is retrieved using  get_model  method. Best model can retrieved using
* best_model  property.
12. Retrieve the parameter grid of "rs_obj" object.
```python
rs_obj.get_parameter_grid()
[{'data_id': 'data-1',
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
'petal_length',
'petal_width'],
'max_depth': 5,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
'ntree': 23,
'response_column': 'species',
'seed': 100,
'tree_size': 100,
'tree_type': 'classification',
'variance': 0.81}},
{'data_id': 'data-2',
```
18: Hyperparameter Tuning in teradataml

```python
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
'petal_length',
'petal_width'],
'max_depth': 5,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
'ntree': 23,
'response_column': 'species',
'seed': 100,
'tree_size': 100,
'tree_type': 'classification',
'variance': 0.81}},
{'data_id': 'data-1',
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
'petal_length',
'petal_width'],
'max_depth': 15,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
'ntree': 99,
'response_column': 'species',
'seed': 100,
'tree_size': 200,
'tree_type': 'classification',
'variance': 0.13}},
{'data_id': 'data-2',
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
'petal_length',
'petal_width'],
'max_depth': 15,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
```
18: Hyperparameter Tuning in teradataml

```python
'ntree': 99,
'response_column': 'species',
'seed': 100,
'tree_size': 200,
'tree_type': 'classification',
'variance': 0.13}},
{'data_id': 'data-1',
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
'petal_length',
'petal_width'],
'max_depth': 12,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
'ntree': 89,
'response_column': 'species',
'seed': 100,
'tree_size': 100,
'tree_type': 'classification',
'variance': 0.13}},
{'data_id': 'data-2',
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
'petal_length',
'petal_width'],
'max_depth': 12,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
'ntree': 89,
'response_column': 'species',
'seed': 100,
'tree_size': 100,
'tree_type': 'classification',
'variance': 0.13}},
{'data_id': 'data-1',
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
```
18: Hyperparameter Tuning in teradataml

```python
'petal_length',
'petal_width'],
'max_depth': 13,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
'ntree': 107,
'response_column': 'species',
'seed': 100,
'tree_size': 200,
'tree_type': 'classification',
'variance': 0.73}},
{'data_id': 'data-2',
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
'petal_length',
'petal_width'],
'max_depth': 13,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
'ntree': 107,
'response_column': 'species',
'seed': 100,
'tree_size': 200,
'tree_type': 'classification',
'variance': 0.73}},
{'data_id': 'data-1',
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
'petal_length',
'petal_width'],
'max_depth': 9,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
'ntree': 22,
'response_column': 'species',
'seed': 100,
```
18: Hyperparameter Tuning in teradataml

```python
'tree_size': 200,
'tree_type': 'classification',
'variance': 0.69}},
{'data_id': 'data-2',
'param': {'data': '"ALICE"."ml__select__169836486331463"',
'input_columns': ['sepal_length',
'sepal_width',
'petal_length',
'petal_width'],
'max_depth': 9,
'maxnum_categorical': 20,
'mtry': 30,
'mtry_seed': 100,
'nodesize': 10,
'ntree': 22,
'response_column': 'species',
'seed': 100,
'tree_size': 200,
'tree_type': 'classification',
'variance': 0.69}}]
```
Example 3: Hyperparameter Tuning Operations on Non-Model
Trainer Function
teradataml offers hyper-parameterization of parameters for non-model trainer functions using
RandomSearch algorithm. This example makes use of hyper-parameterization feature to perform
Antiselect function on titanic data.
In this example, teradataml example titanic data is used to perform Antiselect function.
1.
* Example setup.
* a.
    * Load the example dataset.
```python
load_example_data("teradataml", "titanic")
```
* b.
    * Create teradataml DataFrame.
```python
titanic = DataFrame.from_table("titanic")
```
* c.
    * Slice input data for Hyper-parameterization of data.
```python
train_df1 = titanic.iloc[:200]
train_df2 = titanic.iloc[200:]
```
2.
* Define hyperparameter-tuning for Antiselect (non-model trainer function).
18: Hyperparameter Tuning in teradataml

* a.
    * Define the non-model trainer function parameter space. Include input data in parameter space for
    * non-model trainer function.
```python
params = {"data":(train_df1, train_df2), "exclude":(
['survived', 'name', 'age'],
['survived', 'age'],
["ticket", "parch", "age"],
["ticket", "parch", "sex", "age"])}
```
    * Note:
    * Any argument in 'params' can be hyper-parameterized.
* b.
    * Import non-model trainer function and optimizer.
```python
from teradataml import Antiselect, RandomSearch
```
* c.
    * Initialize the RandomSearch optimizer with non-model trainer function and parameter space
    * required for non-model training.
```python
rs_obj = RandomSearch(func=Antiselect, params=params, n_iter=3)
```
3.
* Perform execution of Antiselect function and execute parallel run in the background.
* a.
    * Perform execution of Antiselect function in background.
```python
rs_obj.fit(wait=False)
```
    * Note:
    * Best model selection is not supported by hyperparameter tuning for non-model
    * trainer function.
* b.
    * Check hyperparameter tuning execution status.
```python
rs_obj.is_running()
True
```
* c.
    * Check execution status after some interval.
```python
rs_obj.is_running()
False
```
    * Note:
    * is_running() method works for both sequential and parallel execution.
18: Hyperparameter Tuning in teradataml

4.
* View the non-model trainer function execution metadata. Retrieve the model metadata of
* "rs_obj" instance.
```python
rs_obj.models
MODEL_ID                                         PARAMETERS STATUS
0  ANTISELECT_0  {'data': '"ALICE"."ml__select__169839553065344...   PASS
1  ANTISELECT_1  {'data': '"ALICE"."ml__select__169839553065344...   PASS
2  ANTISELECT_2     {'data': None, 'exclude': ['survived', 'age']}   PASS
```
* Note:
* All model training has been passed. In case of failure, use  get_error_log  method to retrieve
* corresponding error logs.
5.
* Retrieve the parameter grid for non-model trainer function. Retrieve "rs_obj" object's parameter grid.
```python
rs_obj.get_parameter_grid()
[{'data': '"ALICE"."ml__select__169839553065344"',
'exclude': ['ticket', 'parch', 'sex', 'age']},
{'data': '"ALICE"."ml__select__169839553065344"',
'exclude': ['ticket', 'parch', 'age']},
{'data': None, 'exclude': ['survived', 'age']}]
```
6.
* Get the non-model function execution result from RandomSearch instance "ANTISELECT_2".
```python
rs_obj.get_model("ANTISELECT_2")
result Output
passenger  pclass                                                 name     sex  sibsp  parch
ticket     fare cabin embarked
0          3       3                               Heikkinen, Miss. Laina  female      0      0  STON/O2.
3101282   7.9250  None        S
1          5       3                             Allen, Mr. William Henry    male      0      0
373450   8.0500  None        S
2          6       3                                     Moran, Mr. James    male      0      0
330877   8.4583  None        Q
3          7       1                              McCarthy, Mr. Timothy J    male      0      0
17463  51.8625   E46        S
4          9       3    Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)  female      0      2
347742  11.1333  None        S
5         10       2                  Nasser, Mrs. Nicholas (Adele Achem)  female      1      0
237736  30.0708  None        C
6          8       3                       Palsson, Master. Gosta Leonard    male      3      1
349909  21.0750  None        S
7          4       1         Futrelle, Mrs. Jacques Heath (Lily May Peel)  female      1      0
113803  53.1000  C123        S
8          2       1  Cumings, Mrs. John Bradley (Florence Briggs Thayer)  female      1      0          PC
17599  71.2833   C85        C
9          1       3                              Braund, Mr. Owen Harris    male      1      0         A/5
21171   7.2500  None        S
```
18: Hyperparameter Tuning in teradataml

Example 4: Parallelization in Hyperparameter Tuning for Model and Non-
Model Trainer Functions
teradataml provides the capability for parallel execution of hyperparameters for both model non-model
trainer functions using RandomSearch algorithm. This example execute DecisionForest (model trainer
function) and AntiSelect (non-model trainer function) on the admission dataset.
In this example, teradataml example admission data is used to demonstrate the parallel capability.
1.
* Example setup.
* a.
    * Load the example dataset.
```python
load_example_data("teradataml", "admission_train")
```
* b.
    * Create teradataml DataFrame.
```python
train_df = DataFrame.from_table("admission_train")
```
* c.
    * Identify and transform distinct categorical values into numerical values from the input using
    * Ordinal Encoding.
```python
ordinal_fit = OrdinalEncodingFit(data=df,
target_column=['stats','programming','masters'])
ordinal_transform = OrdinalEncodingTransform(data=df,
object=ordinal_fit,
accumulate=['id','admitted','gpa'])
df = ordinal_transform.result
target_col='admitted'
columns =['gpa', 'stats', 'programming', 'masters']
```
* d.
    * Scale the data.
```python
scale_transform = ScaleTransform(data=df,
object=scale_fit.output,
accumulate=["id", "admitted"])
```
* e.
    * Sample the data.
```python
train_val_sample = scale_transform.result.sample(frac=[0.8, 0.2])
```
18: Hyperparameter Tuning in teradataml

* f.
    * Create train and test data.
```python
train_df = train_val_sample[train_val_sample.sampleid ==
1].drop("sampleid", axis = 1)
test_df = train_val_sample[train_val_sample.sampleid ==
2].drop("sampleid", axis = 1)
```
2.
* Execute model trainer function DecisionForest.
* a.
    * Define hyperparameter tuning for DecisionForest function.
```python
Model training parameters
model_params = {"input_columns":(['gpa', 'stats',
'programming', 'masters']),
"response_column":'admitted',
"max_depth":(1,15,25,20),
"num_trees":(5,15,50),
"tree_type":'CLASSIFICATION'}
Model evaluation parameters
eval_params = {"id_columnn": "id",
"accumulate": "admitted"
}
Import model trainer and optimizer
from teradataml import DecisionForest, RandomSearch
Initialize the RandomSearch optimizer with model trainer
function and parameter space required for model training.
rs_obj = RandomSearch(func=DecisionForest,
params=model_params, n_iter=5)
```
* b.
    * Execute the hyperparameter  fit  function.
    * The default setting for  run_parallel  is True. That is, by default, hyperparameter runs in parallel.
```python
rs_obj.fit(data=train_df, verbose=2,
run_parallel=True, **eval_params)
Model_id:DECISIONFOREST_2 - Run time:29.327s - Status:PASS
- ACCURACY:0.833
Model_id:DECISIONFOREST_3 - Run time:29.451s - Status:PASS
- ACCURACY:0.833
Model_id:DECISIONFOREST_0 - Run time:29.454s - Status:PASS
- ACCURACY:0.833
Model_id:DECISIONFOREST_1 - Run time:29.453s - Status:PASS
```
18: Hyperparameter Tuning in teradataml

```python
- ACCURACY:0.833
Model_id:DECISIONFOREST_4 - Run time:16.397s - Status:PASS
- ACCURACY:0.667
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾
```
    * ｜  100% - 5/5
    * Note:
    * Different  evaluation_metric  can be used for training different models in
    * hyperparameter tuning.
* c.
    * View the results using  models  and  model_stats  properties.
```python
Trained models can be viewed using models property
rs_obj.models
MODEL_ID        DATA_ID
PARAMETERS                   STATUS   ACCURACY
0    DECISIONFOREST_2    DF_0    {'input_columns': ['gpa', 'stats',
'programmin...    PASS    0.833333
1    DECISIONFOREST_3    DF_0    {'input_columns': ['gpa', 'stats',
'programmin...    PASS    0.833333
2    DECISIONFOREST_0    DF_0    {'input_columns': ['gpa', 'stats',
'programmin...    PASS    0.833333
3    DECISIONFOREST_1    DF_0    {'input_columns': ['gpa', 'stats',
'programmin...    PASS    0.833333
4    DECISIONFOREST_4    DF_0    {'input_columns': ['gpa', 'stats',
'programmin...    PASS    0.666667
Additional Performance metrics can be viewd using
model_stats property
rs_obj.model_stats
MODEL_ID    ACCURACY    MICRO-PRECISION    MICRO-RECALL
MICRO-F1    MACRO-PRECISION    MACRO-RECALL    MACRO-F1    WEIGHTED-
PRECISION    WEIGHTED-RECALL    WEIGHTED-F1
0    DECISIONFOREST_2    0.833333    0.833333    0.833333    0.833333
0.833333    0.875    0.828571    0.888889    0.833333    0.838095
1    DECISIONFOREST_3    0.833333    0.833333    0.833333    0.833333
0.833333    0.875    0.828571    0.888889    0.833333    0.838095
2    DECISIONFOREST_0    0.833333    0.833333    0.833333    0.833333
0.833333    0.875    0.828571    0.888889    0.833333    0.838095
3    DECISIONFOREST_1    0.833333    0.833333    0.833333    0.833333
0.833333    0.875    0.828571    0.888889    0.833333    0.838095
```
18: Hyperparameter Tuning in teradataml

```python
4    DECISIONFOREST_4    0.666667    0.666667    0.666667    0.666667
0.625000    0.625    0.625000    0.666667    0.666667    0.666667
```
3.
* Execute non-model trainer function Antiselect.
* a.
    * Define the parameter space for Antiselect function.
```python
Define the non-model trainer function parameter space.
params = { "data":train_df,
"exclude":(['stats', 'programming', 'masters'],
['id', 'admitted'],
['admitted', 'gpa', 'stats'],
['masters'],
['admitted', 'gpa', 'stats', 'programming'])}
Import non-model trainer function and optimizer.
from teradataml import Antiselect, RandomSearch
Initialize the GridSearch optimizer with non-model trainer
function and parameter space required for non-model training.
rs_obj = RandomSearch(func=Antiselect, params=params, n_iter=3)
```
* b.
    * Execute hyperparameter tunning with Antiselect in parallel.
    * The default setting for  run_parallel  is True.
```python
Fitting Antiselect in parallel
rs_obj.fit(verbose=2)
MODEL_ID
PARAMETERS    STATUS
0    ANTISELECT_1    {'data':
'"ALICE"."ml__select__170983718572642...    PASS
1    ANTISELECT_2    {'data':
'"ALICE"."ml__select__170983718572642...    PASS
2    ANTISELECT_0    {'data':
'"ALICE"."ml__select__170983718572642...    PASS
```
* c.
    * View the non-model trainer function execution metadata.
```python
Retrieve the model metadata of "rs_obj" instance.
rs_obj.models
MODEL_ID
PARAMETERS    STATUS
0    ANTISELECT_1    {'data':
'"ALICE"."ml__select__170983718572642...    PASS
```
18: Hyperparameter Tuning in teradataml

```python
1    ANTISELECT_2    {'data':
'"ALICE"."ml__select__170983718572642...    PASS
2    ANTISELECT_0    {'data':
'"ALICE"."ml__select__170983718572642...    PASS
```
* Note:
* All the properties, arguments and functions in previous examples are also applicable here for
* model and non-model trainer functions.
Example 5: Early stopping methods in hyperparameter tunning
teradataml RandomSearch provides the capability of early stopping hyperparameter tuning based on
**the following:**
* Time based: Hyperparameter tuning is stopped once the maximum time is reached, thereby ceasing
* the optimization of hyperparameters.
* Metrics based: Hyperparameter tuning is terminated once a trained model satisfies the specified
* minimum or maximum thresholds for the respective performance metrics, as dictated by the
* evaluation criteria.
* Note:
    * Metrics based method cannot be used for Non-Model Trainer function.
**Note:**
Both time and metrics methods can be used simultaneously, and hyperparameter tuning stops when
either of these two methods satisfies the stopping condition.
Example 5 Setup
The following setup steps apply to all examples for Example 5: Early Stopping Methods in
Hyperparameter Tunning.
### Example Setup
* Load example dataset iris data from teradataml.
```python
load_example_data('teradataml','iris_input')
```
* Create teradataml DataFrame.
```python
df = DataFrame.from_table('iris_input')
```
18: Hyperparameter Tuning in teradataml

* Scale "target_columns" with respect to 'Range' value of the column.
```python
scale_fit = ScaleFit(data=df,
target_columns=['sepal_length', 'sepal_width',
'petal_length', 'petal_width'],
scale_method="Range")
```
* Transform the data.
```python
scale_transform = ScaleTransform(data=df,
object=scale_fit.output,
accumulate=["id", "species"])
```
* Create data for training.
```python
data = scale_transform.result
```
Example 5.1: Time Based Early Stopping for Model Trainer Function
This example shows time based early stopping for model trainer function XGBoost.
1.
* Define hyperparameter space and RandomSearch with XGBoost.
* a.
    * Define model training parameters.
```python
model_params = {"input_columns":['sepal_length', 'sepal_width',
'petal_length', 'petal_width'],
"response_column" : 'species',
"max_depth":(5,10,15),
"lambda1" : (1000.0,0.001),
"model_type" :"Classification",
"seed":32,
"shrinkage_factor":0.1,
"iter_num":(5, 50)}
```
* b.
    * Define evaluation parameters.
```python
eval_params = {"id_column": "id",
"accumulate": "species",
"model_type":'Classification',
"object_order_column":['task_index', 'tree_num',
'iter','class_num', 'tree_order']
}
```
* c.
    * Import model trainer function and optimizer.
```python
from teradataml import XGBoost, RandomSearch
```
18: Hyperparameter Tuning in teradataml

* d.
    * Initialize the RandomSearch optimizer with model trainer function and parameter space required
    * for model training.
```python
rs_obj = RandomSearch(func=XGBoost, params=model_params, n_iter=5)
```
2.
* Execute hyperparameter tunning with max time.
* This step fits the hyperparameters in parallel with  max_time  argument set to 30 seconds.
* Note:
* max_time  argument can be used with sequential hyperparameter tunning.
```python
rs_obj.fit(data=data, max_time=30, verbose=2, **eval_params)
Model_id:XGBOOST_3 - Run time:28.292s - Status:PASS
- ACCURACY:0.8
Model_id:XGBOOST_0 - Run time:28.291s - Status:PASS
- ACCURACY:0.867
Model_id:XGBOOST_2 - Run time:28.289s - Status:PASS
- ACCURACY:0.867
Model_id:XGBOOST_1 - Run time:28.291s - Status:PASS
- ACCURACY:0.867
Computing:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
80% - 4/5
```
* As shown in the output, four models are trained, as the maximum number of trained models in
* parallel is set to 4.
* Any additional models are skipped due to reaching the maximum time allowed for running
* hyperparameter tuning.
3.
* View hyperparameter tuning model metadata using  models  and  model_stats  properties.
* a.
    * View trained model using models property.
```python
rs_obj.models
MODEL_ID    DATA_ID
PARAMETERS    STATUS   ACCURACY
0    XGBOOST_3    DF_0    {'input_columns': ['sepal_length',
'sepal_widt       PASS    0.800000
1    XGBOOST_4    DF_0    {'input_columns': ['sepal_length',
'sepal_widt       SKIP    NaN
2    XGBOOST_0    DF_0    {'input_columns': ['sepal_length',
'sepal_widt       PASS    0.866667
3    XGBOOST_2    DF_0    {'input_columns': ['sepal_length',
'sepal_widt       PASS    0.866667
```
18: Hyperparameter Tuning in teradataml

```python
4    XGBOOST_1    DF_0    {'input_columns': ['sepal_length',
'sepal_widt       PASS    0.866667
```
    * Note:
    * The status "SKIP" indicates that the model was not trained due to reaching the maximum
    * time limit.
* b.
    * View additional performance metrics using model_stats property.
```python
rs_obj.model_stats
MODEL_ID    ACCURACY    MICRO-PRECISION    MICRO-RECALL
MICRO-F1    MACRO-PRECISION    MACRO-RECALL    MACRO-F1    WEIGHTED-
PRECISION    WEIGHTED-RECALL    WEIGHTED-F1
0    XGBOOST_3    0.800000    0.800000    0.800000    0.800000
0.841270    0.766667    0.772339    0.831746    0.800000    0.789433
1    XGBOOST_4    NaN    NaN    NaN    NaN    NaN    NaN    NaN
NaN    NaN    NaN
2    XGBOOST_0    0.866667    0.866667    0.866667    0.866667
0.904762    0.833333    0.833333    0.904762    0.866667    0.855556
3    XGBOOST_2    0.866667    0.866667    0.866667    0.866667
0.904762    0.833333    0.833333    0.904762    0.866667    0.855556
4    XGBOOST_1    0.866667    0.866667    0.866667    0.866667
0.904762    0.833333    0.833333    0.904762    0.866667    0.855556
```
Example 5.2: Time Based Early Stopping for Non-Model Trainer Function
This example shows time based early stopping for non-model trainer function Antiselect.
1.
* Define hyperparameter tuning for Antiselect.
* a.
    * Define the non-model trainer function parameter space.
```python
non_model_trainer_params = {"data":data, "exclude":(
['petal_length', 'petal_width'],
['sepal_length',
'sepal_width', 'petal_length'],
['id', 'sepal_length', 'sepal_width'],
['petal_width'],
['petal_width', 'species'],
['sepal_width', 'petal_length',
'petal_width', 'species'],
['sepal_width', 'petal_length',
```
18: Hyperparameter Tuning in teradataml

```python
'petal_width', 'species', 'id'],
['sepal_length', 'sepal_width'])}
```
* b.
    * Import non-model trainer function and optimizer.
```python
from teradataml import Antiselect, RandomSearch
```
* c.
    * Initialize the GridSearch optimizer with non-model trainer function and parameter space required
    * for non-model training.
```python
rs_obj = RandomSearch(func=Antiselect,
params=non_model_trainer_params, n_iter=5)
```
2.
* Run Antiselect with  max_time  set to 4 seconds.
```python
rs_obj.fit(max_time=4, verbose=2)
Model_id:ANTISELECT_3 - Run time:4.588s
- Status:PASS
Model_id:ANTISELECT_2 - Run time:4.619s
- Status:PASS
Model_id:ANTISELECT_0 - Run time:4.621s
- Status:PASS
Model_id:ANTISELECT_1 - Run time:4.621s
- Status:PASS
Computing:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
80% - 4/5
```
3.
* View the result of hyperparameter tunning using  models  property.
```python
rs_obj.models
MODEL_ID
PARAMETERS    STATUS
0    ANTISELECT_3    {'data':
'"ALICE"."ml__td_sqlmr_out__171014558...    PASS
1    ANTISELECT_4    {'data':
'"ALICE"."ml__td_sqlmr_out__171014558...    SKIP
2    ANTISELECT_2    {'data':
'"ALICE"."ml__td_sqlmr_out__171014558...    PASS
3    ANTISELECT_0    {'data':
'"ALICE"."ml__td_sqlmr_out__171014558...    PASS
4    ANTISELECT_1    {'data':
'"ALICE"."ml__td_sqlmr_out__171014558...    PASS
```
18: Hyperparameter Tuning in teradataml

Example 5.3: Metrics Based Early Stopping for Model Trainer Function
This example shows metrics based early stopping for model trainer function XGBoost.
1.
* Define hyperparameter space and GridSearch with XGBoost.
* a.
    * Define model training parameters.
```python
model_params = {"input_columns":['sepal_length', 'sepal_width',
'petal_length', 'petal_width'],
"response_column" :'species',
"max_depth":(5,10,15),
"lambda1" :(1000.0,0.001),
"model_type" :"Classification",
"seed":32,
"shrinkage_factor":0.1,
"iter_num":(5, 50)}
```
* b.
    * Define evaluation parameters.
```python
eval_params = {"id_column": "id",
"accumulate": "species",
"model_type":'Classification',
"object_order_column":['task_index', 'tree_num',
'iter','class_num', 'tree_order']
}
```
* c.
    * Import model trainer function and optimizer.
```python
from teradataml import XGBoost,  RandomSearch
```
* d.
    * Initialize the GridSearch optimizer with model trainer function and parameter space required for
    * model training.
```python
rs_obj = RandomSearch(func=XGBoost, params=model_params, n_iter=10)
```
2.
* Run hyperparameter tunning with early stop.
* This step runs  fit  with  early_stop  and  evalation_metric .
* Note:
* early_stop  can be used without specifying an  evaluation_metric .
* By default,
    * In classification, the  evaluation_metric  is set to Accuracy.
    * In regression, the  evaluation_metric  is set to MAE.
18: Hyperparameter Tuning in teradataml

```python
rs_obj.fit(data=data, evaluation_metric = 'WEIGHTED-F1',
early_stop=0.87, verbose=2, **eval_params)
Model_id:XGBOOST_0 - Run time:28.722s - Status:PASS - WEIGHTED-
F1:0.901
Model_id:XGBOOST_3 - Run time:28.783s - Status:PASS - WEIGHTED-
F1:0.935
Model_id:XGBOOST_1 - Run time:28.804s - Status:PASS - WEIGHTED-
F1:0.901
Model_id:XGBOOST_2 - Run time:28.824s - Status:PASS - WEIGHTED-
F1:0.901
Computing:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
40% - 4/10
```
* Note:
* Various evaluation metrics can be utilized with early stop to take advantage of the early
* stopping functionality.
3.
* View hyperparameter tuning trained model metadata using  models  and  model_stats  properties.
* a.
    * View trained model using models property.
```python
rs_obj.models
MODEL_ID    DATA_ID
PARAMETERS    STATUS   WEIGHTED-F1
0    XGBOOST_0    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    PASS    0.901483
1    XGBOOST_4    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
2    XGBOOST_5    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
3    XGBOOST_6    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
4    XGBOOST_7    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
5    XGBOOST_8    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
6    XGBOOST_9    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    SKIP    NaN
7    XGBOOST_3    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    PASS    0.935065
8    XGBOOST_1    DF_0    {'input_columns': ['sepal_length',
```
18: Hyperparameter Tuning in teradataml

```python
'sepal_widt...    PASS    0.901483
9    XGBOOST_2    DF_0    {'input_columns': ['sepal_length',
'sepal_widt...    PASS    0.901483
```
    * Note:
    * The status "SKIP" indicates that the model was not trained, as the previously trained model
    * before it was able to achieve the desired threshold or a better value of evaluation metrics..
* b.
    * View additional performance metrics using model_stats property.
```python
rs_obj.model_stats
MODEL_ID    ACCURACY    MICRO-PRECISION    MICRO-RECALL
MICRO-F1    MACRO-PRECISION    MACRO-RECALL    MACRO-F1    WEIGHTED-
PRECISION    WEIGHTED-RECALL    WEIGHTED-F1
0    XGBOOST_0    0.900000    0.900000    0.900000    0.900000
0.879121    0.888889    0.879441    0.912088    0.900000    0.901483
1    XGBOOST_4    NaN    NaN    NaN    NaN    NaN    NaN    NaN
NaN    NaN    NaN
2    XGBOOST_5    NaN    NaN    NaN    NaN    NaN    NaN    NaN
NaN    NaN    NaN
3    XGBOOST_6    NaN    NaN    NaN    NaN    NaN    NaN    NaN
NaN    NaN    NaN
4    XGBOOST_7    NaN    NaN    NaN    NaN    NaN    NaN    NaN
NaN    NaN    NaN
5    XGBOOST_8    NaN    NaN    NaN    NaN    NaN    NaN    NaN
NaN    NaN    NaN
6    XGBOOST_9    NaN    NaN    NaN    NaN    NaN    NaN    NaN
NaN    NaN    NaN
7    XGBOOST_3    0.933333    0.933333    0.933333    0.933333
0.916667    0.944444    0.922078    0.950000    0.933333    0.935065
8    XGBOOST_1    0.900000    0.900000    0.900000    0.900000
0.879121    0.888889    0.879441    0.912088    0.900000    0.901483
9    XGBOOST_2    0.900000    0.900000    0.900000    0.900000
0.879121    0.888889    0.879441    0.912088    0.900000    0.901483
```
18: Hyperparameter Tuning in teradataml