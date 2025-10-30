# BYOM (Bring Your Own Model) Management

The Bring Your Own Model feature in Vantage provides flexibility to score data in Vantage using
external models.
To score data using external models, the model files must be stored in Vantage. Users can store the models
in any table in Vantage. The table must have at least the following two columns:
| Column Name | Column Type |
| ----------- | ----------- |
| model_id | VARCHAR(of any length) |
| model | BLOB |

Note:
* These functions are available based on Vantage version.
* Import these functions only after establishing the connection to Vantage.
    These functions may not work properly if these are imported before context creation.
* You can check whether these functions are available or not by running the
    function display_analytic_functions().
    * If these functions are available for the corresponding Vantage version, then
```python
display_analytic_functions() displays those functions.
```
    * Otherwise, these functions are not displayed in display_analytic_functions() output.
* Supported External Model Types
* teradataml APIs that enable end users to process BYOM data:
  * save_byom() saves a BYOM model in Vantage
  * list_byom() lists all available BYOM models from Vantage
  * retrieve_byom() retrieves a specific model from Vantage
  * delete_byom() deletes a BYOM model from Vantage
  * set_byom_catalog() sets the BYOM model catalog information
  * set_license() sets the License information
  * get_license() gets the License information
* BYOM Workflows
## Supported External Model Types
The BYOM feature in Vantage supports the following types of external models:
                                        14
* H2O
* PMML
* ONNX
* DataikuPredict
* DataRobotPredict
### PMMLPredict
PMML is the most popular standard serialization format for exchange of Machine Learning models. Most
customers train their models in tools external to Vantage, such as Scikit-learn. Vantage Analytics enables
customers to bring their models to Vantage (by inserting the model as a blob into a table) and apply them
to data stored in Database Engine 20 for scoring. Users can use these external models for scoring through
teradataml by using PMMLPredict() function.
The following are examples of PMMLPredict() function call.
#### Example Setup
* Import necessary modules.
```python
>>> import os, teradataml
>>> from teradataml.options.configure import configure
>>> from teradataml import DataFrame, load_example_data,
save_byom, retrieve_byom
```
* Load example data.
```python
>>> load_example_data("byom", "iris_input")
```
* Create teradataml DataFrame object.
```python
>>> iris_test = DataFrame("iris_input")
```
* Set install location of the BYOM functions.
```python
>>> configure.byom_install_location = "mldb"
```
#### Example 1: Run a query with GLM model and overwrite cached models
* Load model file into Vantage.
```python
>>> model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "iris_db_glm_model.pmml")
```
* Save the model.
```python
>>> save_byom("iris_db_glm_model", model_file, "byom_models")
```
* Retrieve the model.
```python
>>> modeldata = retrieve_byom("iris_db_glm_model", table_name="byom_models")
```
* Pass the output of the retrieve_model API as an input to the PMMLPredict function to score data.
```python
>>> result = PMMLPredict(modeldata = modeldata,
newdata = iris_test,
accumulate = ['id',
'sepal_length', 'petal_length'],
overwrite_cached_models = '*')
```
#### Example 2: Run a query with XGBoost model and overwrite cached models
* Load model file into Vantage.
```python
>>> model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "iris_db_xgb_model.pmml")
```
* Save the model.
```python
>>> save_byom("iris_db_xgb_model", model_file, "byom_models")
```
* Retrieve the model.
```python
>>> modeldata = retrieve_byom("iris_db_xgb_model", table_name="byom_models")
```
* Pass the output of the retrieve_model API as an input to the PMMLPredict function to score data.
```python
>>> result = PMMLPredict(modeldata = modeldata,
newdata = iris_test,
accumulate = ['id',
'sepal_length', 'petal_length'],
overwrite_cached_models = '*')
```
### H2OPredict
H2OPredict performs a prediction on each row of the input table using a model previously trained in H2O
and then loaded into the database. The model uses an interchange format called MOJO and it is loaded
as a blob to a table in the database by the user.
The following are examples of H2OPredict() function call.
#### Example Setup
* Import necessary modules.
```python
>>> import os, teradataml
>>> from teradataml.options.configure import configure
>>> from teradataml import DataFrame, load_example_data,
save_byom, retrieve_byom
```
* Load example data.
```python
>>> load_example_data("byom", "iris_input")
```
* Create teradataml DataFrame object.
```python
>>> iris_test = DataFrame("iris_input")
```
* Set install location of the BYOM functions.
```python
>>> configure.byom_install_location = "mldb"
```
#### Example 1: Run a query with GLM model and overwrite cached models
The query also includes arguments model_type, enable_options and model_output_fields.
* Load model file into Vantage.
```python
>>> model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "iris_mojo_glm_h2o_model")
```
* Save the model.
```python
>>> save_byom("iris_mojo_glm_h2o_model", model_file, "byom_models")
```
* Retrieve the model.
```python
>>> modeldata =
retrieve_byom("iris_mojo_glm_h2o_model", table_name="byom_models")
```
* Pass the output of the retrieve_model API as an input to the PMMLPredict function to score data.
```python
>>> result = H2OPredict(newdata=iris_test,
newdata_partition_column='id',
newdata_order_column='id',
modeldata=modeldata,
modeldata_order_column='model_id',
model_output_fields=['label', 'classProbabilities'],
accumulate=['id', 'sepal_length', 'petal_length'],
overwrite_cached_models='*',
enable_options='stageProbabilities',
model_type='OpenSource')
```
* Print the results.
```python
print(result.result)
```
#### Example 2: Run a query with XGBoost model and overwrite cached models
The query also includes arguments model_type, enable_options and model_output_fields.
* Load model file into Vantage.
```python
>>> model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "iris_mojo_xgb_h2o_model")
```
* Save the model.
```python
>>> save_byom("iris_mojo_xgb_h2o_model", model_file, "byom_models")
```
* Retrieve the model.
```python
>>> modeldata =
retrieve_byom("iris_mojo_xgb_h2o_model", table_name="byom_models")
```
* Pass the output of the retrieve_model API as an input to the PMMLPredict function to score data.
```python
>>> result = H2OPredict(newdata=iris_test,
newdata_partition_column='id',
newdata_order_column='id',
modeldata=modeldata,
modeldata_order_column='model_id',
model_output_fields=['label', 'classProbabilities'],
accumulate=['id', 'sepal_length', 'petal_length'],
overwrite_cached_models='*',
enable_options='stageProbabilities',
model_type='OpenSource')
```
* Print the results.
```python
print(result.result)
```
#### Example 3: Run a query with a licensed model
This example runs a query with a licensed model with id 'licensed_model1' from the table
'byom_licensed_models' and associated license key stored in column 'license_key' of the table 'license'
present in the schema 'mldb'.
* Retrieve model.
```python
modeldata = retrieve_byom('licensed_model1',
table_name='byom_licensed_models',
license='license_key',
is_license_column=True,
license_table_name='license',
license_schema_name='mldb')
```
* Pass the output of the retrieve_model API as an input to the H2OPredict function to score data.
```python
result = H2OPredict(newdata=iris_test,
newdata_partition_column='id',
newdata_order_column='id',
modeldata=modeldata,
modeldata_order_column='model_id',
model_output_fields=['label', 'classProbabilities'],
accumulate=['id', 'sepal_length', 'petal_length'],
overwrite_cached_models='*',
enable_options='stageProbabilities',
model_type='OpenSource')
```
* Print the results.
```python
print(result.result)
```
### ONNXPredict
ONNXPredict performs a prediction on each row of the input table using a model previously trained in
ONNX and then loaded into the database. The model uses an interchange format called as ONNX and it
is loaded to the database in Vantage in a table by the user as a blob.
The following are examples of ONNXPredict() function call.
#### Example Setup
* Import necessary modules.
```python
>>> import os, teradataml
>>> from teradataml.options.configure import configure
>>> from teradataml import DataFrame, load_example_data,
save_byom, retrieve_byom
```
* Load example data.
```python
>>> load_example_data("byom", "iris_test")
```
* Create teradataml DataFrame object.
```python
>>> iris_test = DataFrame("iris_test")
```
* Set install location of the BYOM functions.
```python
>>> configure.byom_install_location = "mldb"
```
#### Example 1: Predict the flower species using trained 'skl_model' model
* Load model file into Vantage.
```python
>>> model_file_path = os.path.join(os.path.dirname(teradataml.__file__),
"data", "models")
>>> skl_model_file = os.path.join(model_file_path,
"iris_db_dt_model_sklearn.onnx")
```
* Save the model.
```python
>>> save_byom("iris_db_dt_model_sklearn",
skl_model_file, "byom_models")
```
* Retrieve model.
```python
>>> skl_model = retrieve_byom("iris_db_dt_model_sklearn",
table_name="byom_models")
```
* Using ONNXPredict to score data.
```python
>>> ONNXPredict_out = ONNXPredict(accumulate="id",
newdata=iris_test,
modeldata=skl_model)
```
### DataikuPredict
Use the DataikuPredict() function to score data in Vantage with a model that has been created outside
Vantage and exported to Vantage using Dataiku format.
Required Arguments:
* modeldata: Specifies the model teradataml DataFrame to be used for scoring.
* newdata: Specifies the input teradataml DataFrame that contains the data to be scored.
* accumulate: Specifies the names of input teradataml DataFrame columns to copy to the output.
  By default, the function copies all input teradataml DataFrame columns to the output.
Optional Arguments:
* model_output_fields: Specifies the columns of the json output that the user wants to specify as
  individual columns instead of the entire json report.
* overwrite_cached_models: Specifies the model name that needs to be removed from the cache.
  If a model loaded into the memory of the node fits in the cache, it stays in the cache until being evicted
  to make space for another model that needs to be loaded. Therefore, a model can remain in the cache
  even after the completion of function call. Other functions that use the same model can use it, saving
  the cost of reloading it into memory.
  You may overwrite a cached model only when it has been updated, to make sure that the Predict
  function uses the updated model instead of the cached model.
  Note:
    Do not use the overwrite_cached_models argument except when trying to replace a previously
    cached model. This applies to any model type (PMML, H2O Open Source, DAI, ONNX, and
    Dataiku). Using this argument in other cases, including in concurrent queries or multiple times
    within a short period of time, may lead to an OOM error from garbage collection not being
    fast enough.
  Permitted values 'current_cached_model', '*', 'true', 't', 'yes', '1', 'false', 'f', 'no', 'n', or '0'.
  Default Values is "false", which means this function does not overwrite cached models.
* is_debug: Specifies whether debug statements are added to a trace table or not.
  When set to True, debug statements are added to a trace table that must be created beforehand.
  Default value is False.
  Note:
    * Only available with BYOM version 3.00.00.02 and later.
    * Teradata recommends using small data input sizes, since a database trace table is used to
    collect the debug information which does impact performance of the function.
  To generate this log:
  1. Create a global trace table with columns vproc_ID BYTE(2), Sequence INTEGER,
    Trace_Output VARCHAR(31000).
  2. Turn on session function tracing:
```python
SET SESSION FUNCTION TRACE USING '' FOR
TABLE <trace_table_name_created_in_step_1>;
```
  3. Execute function with  is_debug  set to 'True'.
  4. Debug information is logged to the table created in step 1.
  5. To turn off the logging, either disconnect from the session or run the following SQL:
```python
SET SESSION FUNCTION TRACE OFF;
```
  Note:
    The trace table is temporary and the information is deleted if you log off from the session. If
    long term persistence is necessary, you can copy the table to a permanent table before leaving
    the session.
This function returns an instance of DataikuPredict. Output teradataml DataFrame can be accessed using
attribute references, such as DataikuPredictObj.<attribute_name>. Output teradataml DataFrame
attribute name is: result.
#### Example Setup
Note:
* You need to get the connection to Vantage to run this function. And the function will raise error
    if it is not supported on the Vantage you are connected to.
* To run BYOM functions, you need to set configure.byom_install_location to the database name
    where BYOM functions are installed.
* Import required libraries and functions.
```python
import os, teradataml
from teradataml import get_connection, DataFrame
from teradataml import load_example_data, save_byom, retrieve_byom
from teradataml import configure, display_analytic_functions, execute_sql
```
* Load example data.
```python
load_example_data("byom", "iris_test")
```
* Create teradataml DataFrame object.
```python
iris_test = DataFrame.from_table("iris_test")
```
* Set install location of the BYOM functions.
```python
configure.byom_install_location = "mldb"
```
* Check available analytic functions.
```python
display_analytic_functions(type="BYOM")
```
* Load model file into Vantage.
```python
model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "dataiku_iris_data_ann_thin")
save_byom("dataiku_iris_data_ann_thin", model_file, "byom_models")
```
* Retrieve model.
```python
modeldata =
retrieve_byom("dataiku_iris_data_ann_thin", table_name="byom_models")
```
#### Example 1: Score data in Vantage with a model created outside Vantage
* Run the predict function to score data.
```python
DataikuPredict_out_1 = DataikuPredict(newdata=iris_test,
modeldata=modeldata,
accumulate=['id',
'sepal_length', 'petal_length'],
overwrite_cached_models="*")
```
* Print the result.
```python
print(DataikuPredict_out_1.result)
```
#### Example 2: View trace table information
* Create the trace table.
```python
crt_tbl_query = 'CREATE GLOBAL TEMPORARY TRACE TABLE BYOM_Trace \
(vproc_ID  BYTE(2) \
,Sequence  INTEGER \
,Trace_Output VARCHAR(31000) CHARACTER SET LATIN NOT
CASESPECIFIC) \
ON COMMIT PRESERVE ROWS;'
execute_sql(crt_tbl_query
```
* Turn on the session function.
```python
execute_sql("SET SESSION FUNCTION TRACE USING '' FOR TABLE BYOM_Trace;")
```
* Run the DataikuPredict() function using is_debug=True.
```python
DataikuPredict_out_2 = DataikuPredict(newdata=iris_test,
modeldata=modeldata,
accumulate=['id',
'sepal_length', 'petal_length'],
overwrite_cached_models="*",
is_debug=True)
```
* Print the result.
```python
print(DataikuPredict_out_2.result)
```
* View the trace table information.
```python
trace_df = DataFrame.from_table("BYOM_Trace")
print(trace_df)
```
* Turn off the session function.
```python
execute_sql("SET SESSION FUNCTION TRACE OFF;")
```
### DataRobotPredict
Use the DataRobotPredict() function to score data in Vantage with a model that has been created outside
Vantage and exported to Vantage using DataRobot format.
Required Arguments:
* modeldata: Specifies the model teradataml DataFrame to be used for scoring.
* newdata: Specifies the input teradataml DataFrame that contains the data to be scored.
  Note:
    The input columns containing Date or Timestamp types must be converted to character type
    before running this function.
* accumulate: Specifies the names of input teradataml DataFrame columns to copy to the output.
  By default, the function copies all input teradataml DataFrame columns to the output.
Optional Arguments:
* model_output_fields: Specifies the columns of the json output that the user wants to specify as
  individual columns instead of the entire json report.
* overwrite_cached_models: Specifies the model name that needs to be removed from the cache.
  If a model loaded into the memory of the node fits in the cache, it stays in the cache until being evicted
  to make space for another model that needs to be loaded. Therefore, a model can remain in the cache
  even after the completion of function call. Other functions that use the same model can use it, saving
  the cost of reloading it into memory.
You may overwrite a cached model only when it has been updated, to make sure that the Predict
  function uses the updated model instead of the cached model.
  Note:
    Do not use the overwrite_cached_models argument except when trying to replace a previously
    cached model. This applies to any model type (PMML, H2O Open Source, DAI, ONNX, and
    Dataiku). Using this argument in other cases, including in concurrent queries or multiple times
    within a short period of time, may lead to an OOM error from garbage collection not being
    fast enough.
  Permitted values 'current_cached_model', '*', 'true', 't', 'yes', '1', 'false', 'f', 'no', 'n', or '0'.
  Default Values is "false", which means this function does not overwrite cached models.
* is_debug: Specifies whether debug statements are added to a trace table or not.
  When set to True, debug statements are added to a trace table that must be created beforehand.
  Default value is False.
  Note:
    * Only available with BYOM version 3.00.00.02 and later.
    * Teradata recommends using small data input sizes, since a database trace table is used to
    collect the debug information which does impact performance of the function.
  To generate this log:
  1. Create a global trace table with columns vproc_ID BYTE(2), Sequence INTEGER,
    Trace_Output VARCHAR(31000).
  2. Turn on session function tracing:
```python
SET SESSION FUNCTION TRACE USING '' FOR
TABLE <trace_table_name_created_in_step_1>;
```
  3. Execute function with  is_debug  set to 'True'.
  4. Debug information is logged to the table created in step 1.
  5. To turn off the logging, either disconnect from the session or run the following SQL:
```python
SET SESSION FUNCTION TRACE OFF;
```
  Note:
    The trace table is temporary and the information is deleted if you log off from the session. If
    long term persistence is necessary, you can copy the table to a permanent table before leaving
    the session.
* **generic_arguments: Specifies the generic keyword arguments Database Engine 20 functions
  accept. Below are the generic keyword arguments:
  * persist: Specifies whether to persist the results of the function in a table or not.
    When set to True, results are persisted in a table; otherwise, results are garbage collected at the
    end of the session.
    Default value is False.
  * volatile: Specifies whether to put the results of the function in a volatile table or not.
    When set to True, results are stored in a volatile table, otherwise not.
    Default value is False.
  * This function allows you to partition, hash, order or local order the input data. These generic
    arguments are available for each argument that accepts teradataml DataFrame as input and can
    be accessed as:
    "<input_data_arg_name>_partition_column" accepts str or list of str (Strings)
    "<input_data_arg_name>_hash_column" accepts str or list of str (Strings)
    "<input_data_arg_name>_order_column" accepts str or list of str (Strings)
    "local_order_<input_data_arg_name>" accepts boolean
  Note:
    These generic arguments are supported by teradataml if the underlying Database Engine 20
    function supports, otherwise an exception is raised.
This function returns an instance of DataikuPredict. Output teradataml DataFrame can be accessed using
attribute references, such as DataikuPredictObj.<attribute_name>. Output teradataml DataFrame
attribute name is: result.
#### Example Setup
Note:
* You need to get the connection to Vantage to run this function. And the function will raise error
    if it is not supported on the Vantage you are connected to.
* To run BYOM functions, you need to set configure.byom_install_location to the database name
    where BYOM functions are installed.
* Import required libraries and functions.
```python
import os, teradataml
from teradataml import get_connection, DataFrame
from teradataml import load_example_data, save_byom, retrieve_byom
from teradataml import configure, display_analytic_functions, execute_sql
```
* Load example data.
```python
load_example_data("byom", "iris_test")
```
* Create teradataml DataFrame object.
```python
iris_test = DataFrame.from_table("iris_test")
```
* Set install location of the BYOM functions.
```python
configure.byom_install_location = "mldb"
```
* Check available analytic functions.
```python
display_analytic_functions(type="BYOM")
```
* Load model file into Vantage.
```python
model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "dr_iris_rf")
save_byom("dr_iris_rf", model_file, "byom_models")
```
* Retrieve model.
```python
modeldata = retrieve_byom("dr_iris_rf", table_name="byom_models")
```
#### Example 1: Score data in Vantage with a model created outside Vantage
* Run the predict function to score data.
```python
Datarobotpredict_1 = DataRobotPredict(newdata=iris_test,
modeldata=modeldata,
accumulate=['id',
'sepal_length', 'petal_length'],
overwrite_cached_models="*")
```
* Print the result.
```python
print(Datarobotpredict_1.result)
```
## set_byom_catalog()
The set_byom_catalog() function allows a user to set the BYOM model catalog information, such as a table
name and schema name, at the session level for the following BYOM model cataloging APIs:
* save_byom()
* list_byom()
* retrieve_byom()
* delete_byom()
* set_license()
Required Argument:
* table_name specifies the name of the table to be used for BYOM model cataloging. This table is used
  for saving, retrieving BYOM model information by BYOM model cataloging APIs.
Optional Argument:
* schema_name specifies the name of the schema/database in which the table specified in table_name
  is looked up.
  If not specified, then table is looked up in current schema/database.
#### Example Setup
```python
>>> from teradataml import set_byom_catalog
```
#### Example 1: Set global parameters table_name and schema_name
```python
>>>
set_byom_catalog(table_name='model_table_name', schema_name='model_schema_name')
The model cataloging parameters are set to table_name='model_table_name'
and schema_name='model_schema_name'
```
## save_byom()
The save_model() API allows a user to save externally trained models in Vantage in a specified table. This
function allows users to save various models stored in different formats such as PMML, MOJO, and so on.
If the specified model table exists in Vantage, model data is saved in that table. Otherwise, model table is
created first based on the user parameters and then model data is saved.
If the user specified table exists, then
* The table must have at least two columns with names and types as specified in the following:
  * 'model_id' column of type VARCHAR of any length
  * 'model' column of type BLOB
* User can choose to have additional columns to store additional information of the model. This
  information can be passed using additional_columns parameter. See the following argument
  description for details.
If the user specified table does not exist, then
* This function creates the table with the name specified in table_name.
* The table is created in the schema specified in schema_name.
  If schema_name is not specified, then current schema is used.
* The table is created with the following columns:
  * 'model_id' column with type specified in additional_columns_types.
    If type is not specified, table is created with 'model_id' column as VARCHAR(128).
  * 'model' column with type specified in "additional_columns_types".
    If type is not specified, table is created with 'model' column as BLOB.
  * Columns specified in additional_columns parameter.
    See additional_columns argument description for more details.
  * Data types of these additional columns are either taken from the values passed to
    additional_columns_types or inferred from the values passed to the additional_columns.
    See additional_columns_types argument description for more details.
Required Arguments:
* model_id specifies the unique model identifier for this model.
* model_file specifies the absolute path of the file which has model information.
Optional Arguments:
* table_name specifies the name of the table where model is saved.
  If a table with this table_name does not exist, this function creates the table according to
  additional_columns and additional_columns_types.
  Note:
    * You must either specify this argument, or set the byom model catalog table name
    using set_byom_catalog().
    * If none of them are set, exception is raised; If both of them are set, the settings in
    save_byom() take precedence and is used for function execution when saving an model.
* schema_name specifies the name of the schema/database in which the table specified in table_name
  is looked up.
  If this argument is not specified, then table is looked up in the schema associated with the
  current context.
Note:
    * You must either specify this argument and the table_name argument, or set the byom model
    catalog schema name using set_byom_catalog().
    * If none of them are set, exception is raised; If both of them are set, the settings in
    save_byom() take precedence and is used for function execution when saving an model.
    * If you specify schema_name, table_name has to be specified, else exception is raised.
  The following table shows the system behavior based on different input combinations.
| In save_byom() | In set_byom_catalog() |  |
| -------------- | --------------------- | - |
|  |  | System Behavior |
| table_ |  |  |
| name |  |  |

      schema_
      name
              table_
              name
                    schema_
                    name
    Set
        Set
                Set Set
                            schema_name and table_name
                            in save_byom() are used for
                            function execution.
              Not set Not set
                            schema_name and table_name
                            in save_byom() is used for
                            function execution.
        Not set Set Set
                            table_name from save_byom() is used
                            and schema name associated with the
                            current context is used.
    Not set
        Set Set Set Exception is raised.
        Not set
                Set Set
                            table_name and schema_name from
                            set_byom_catalog() are used for
                            function execution.
                Set Not set
                            table_name from set_byom_catalog()
                            is used and schema name associated
                            with the current context is used.
              Not set Not set Exception is raised.
* additional_columns specifies the additional information about the model to be saved in the model table.
  Additional information about the model is passed as key value pair, where key is the name of the column
  and value is data to be stored in that column for the model being saved. Allowed types for the values
  passed in dictionary are:
  * int
  * float
  * str
  * bool
  * datetime.datetime
  * datetime.date
* datetime.time
  Note:
    additional_columns does not accept keys model_id and model.
* additional_columns_types specifies the column type of additional columns. These column types are
  used while creating the table using the columns specified in the additional_columns argument.
  Additional column datatype information is passed as key value pair, where key is the column name and
  value is teradatasqlalchemy.types.
  Note:
    * If any of the column type for additional columns are not specified in
    additional_columns_types, then it derives the column types. To get more information on
    column type mapping, refer to parameters using help(save_byom).
    * For columns model_id and model, column type must be set to VARCHAR and BLOB,
    respectively, for the table. Thus, for the columns model_id and model, acceptable values for
    additional_columns_types are VARCHAR and BLOB respectively.
    * This argument is ignored if the table exists.
#### Example Setup
* Import necessary modules.
```python
>>> import teradataml, os, datetime
>>> # import save_byom from teradataml
>>> from teradataml import save_byom
```
* Get the model file path to use in the examples.
```python
>>> model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "iris_kmeans_model")
```
#### Example 1: Create a table with additional columns and column types specified
This example creates a table "byom_models" with additional columns by specifying the type of the columns
as listed in the following table and save the model in it.
| Column Name | Column Type |
| ----------- | ----------- |
| model_id | VARCHAR(128) |
| model | BLOB |
| Description | VARCHAR(2000) |
| Column Name | Column Type |
| ----------- | ----------- |
| UserId | NUMBER(5) |
| ProductionReady | BYTEINT |
| ModelEfficiency | NUMBER(11,10) |
| ModelSavedTime | TIMESTAMP |
| ModelGeneratedDate | DATE |
| ModelGeneratedTime | TIME |

```python
>>> save_byom("model1",
model_file,
"byom_models",
additional_columns={"Description": "KMeans model",
"UserId": "12345",
"ProductionReady": False,
"ModelEfficiency": 0.67412,
"ModelSavedTime": datetime.datetime.now(),
"ModelGeneratedDate":datetime.date.today(),
"ModelGeneratedTime": datetime.time(hour=0,minute=5,second=45,microsecond=110)
},
additional_columns_types={"Description": VARCHAR(2000),
"UserId": NUMBER(5),
"ProductionReady": BYTEINT,
"ModelEfficiency": NUMBER(11,10),
"ModelSavedTime": TIMESTAMP,
"ModelGeneratedDate": DATE,
"ModelGeneratedTime": TIME}
Created the table 'byom_models' as it does not exist.
Model is saved.
```
#### Example 2: Create a table with additional columns and column types
#### not specified
This example creates a table "byom_models1" in the "test" database, with additional columns by not
specifying the type of the columns. Once table is created, save the model in it.
```python
>>> save_byom("model1",
model_file,
"byom_models1",
additional_columns={"Description": "KMeans model",
"UserId": "12346",
"ProductionReady": False,
"ModelEfficiency": 0.67412,
"ModelSavedTime": datetime.datetime.now(),
"ModelGeneratedDate":datetime.date.today(),
"ModelGeneratedTime": datetime.time(hour=0,minute=5,second=45,microsecond=110)
},
schema_name="test"
Created the table 'byom_models1' as it does not exist.
Model is saved.
```
#### Example 3: Save a model in an existing table
This example saves a model in the existing table "byom_models".
```python
>>> save_byom("model2",
model_file,
"byom_models",
additional_columns={"Description": "KMeans model duplicated"}
Model is saved.
```
#### Example 4: Use the BYOM model catalog information set at session level
#### by set_byom_catalog()
This example shows when save_byom() function is called without arguments table_name and
schema_name, system uses the BYOM model catalog information set at session level by the
set_byom_catalog() function.
* The model cataloging parameters are set using the set_byom_catalog() function.
```python
>>> set_byom_catalog(table_name='byom_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_models'
and schema_name='alice'
```
* The model is saved in the existing table specified by the arguments in set_byom_catalog() function.
```python
>>> save_byom('model3', model_file=model_file)
Model is saved.
```
#### Example 5: Override session level parameters for BYOM model cataloging
This example shows that you can override the session level BYOM model catalog information set by the
set_byom_catalog(), by passing the table as well as schema arguments explicitly.
* The model cataloging table name is set to 'byom_models' using the set_byom_catalog() function.
```python
>>> set_byom_catalog(table_name='byom_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_models'
and schema_name='alice'.
```
* The model is saved in a different table 'byom_licensed_models' which is specified by the table_name
  argument in this save_byom() function.
```python
>>> save_byom('licensed_model2',
model_file=model_file, table_name='byom_licensed_models',
additional_columns={"license_data": "A5sUL9KU_kP35Vq"})
Created the model table 'byom_licensed_models' as it does not exist.
Model is saved.
```
## list_byom()
The list_byom() API allows a user to list saved models, filtering the results based on the optional arguments.
Optional Arguments:
* table_name specifies the name of the table to list models from.
  Note:
    * You must either specify this argument, or set the byom model catalog table name
    using set_byom_catalog().
    * If none of them are set, exception is raised; If both of them are set, the settings in list_byom()
    take precedence and is used for function execution.
* schema_name specifies the name of the schema in which the table specified in table_name is
  looked up.
  If this argument is not specified, then table is looked up in the schema associated with the
  current context.
Note:
    * You must either specify this argument and the table_name argument, or set the byom model
    catalog schema name using set_byom_catalog().
    * If none of them are set, exception is raised; If both of them are set, the settings in list_byom()
    take precedence and is used for function execution.
    * If you specify schema_name, table_name has to be specified, else exception is raised.
  See table in save_byom() that shows different system behaviors based on different input combinations.
* model_id specifies the unique model identifier of the model(s).
  If specified, the models with either exact match or a substring match, are listed.
#### Example Setup
* Import necessary modules.
```python
>>> import teradataml, os, datetime
>>> from teradataml import save_byom, list_byom
```
* Get the model file path.
```python
>>> model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "iris_kmeans_model")
```
* Save three models with different parameters.
```python
>>> save_byom("model7", model_file, "byom_models")
Model is saved.
>>> save_byom("iris_model1", model_file, "byom_models")
Model is saved.
>>> save_byom("model8", model_file, "byom_models", schema_name="test")
Model is saved.
```
#### Example 1: List all models in a table
This example lists all the models saved in the table "byom_models".
```python
>>> list_byom(table_name="byom_models")
model
model_id
model7       b'
B03041400080808...'
iris_model1  b'
B03041400080808...'
```
#### Example 2: List all models with model_id containing a specific string
This example lists all the models with model_id containing the string "iris", from the "byom_models" table.
```python
>>> list_byom(table_name="byom_models", model_id="iris")
model
model_id
iris_model1  b'
B03041400080808...'
```
#### Example 3: List all models with model_id containing one of the two strings
This example lists all the models with model_id containing either "iris" or "7" string, from the
"byom_models" table.
```python
>>> list_byom(table_name="byom_models", model_id=["iris", "7"])
model
model_id
model7       b'
B03041400080808...'
iris_model1  b'
B03041400080808...'
```
#### Example 4: List all models in a table in the specified database
This example lists all the models from the "byom_models" in the "test" database.
```python
>>> list_byom(table_name="byom_models", schema_name="test")
model
model_id
model8       b'
B03041400080808...'
```
#### Example 5: List all the models from the model cataloging table set
#### by set_byom_catalog()
* The model cataloging parameters are set using the set_byom_catalog() function.
```python
>>> set_byom_catalog(table_name='byom_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_models'
and schema_name='alice'
```
* List all the models.
```python
>>> list_byom()
model
model_id
model8 b'504B03041400080808...'
```
#### Example 6: Override session level parameters for BYOM model cataloging
This example shows that you can override the session level BYOM model catalog information set by the
set_byom_catalog(), by passing the table as well as schema arguments explicitly.
* The model cataloging table name is set to 'byom_models' using the set_byom_catalog() function.
```python
>>> set_byom_catalog(table_name='byom_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_models'
and schema_name='alice'.
```
* List all the models from the table specified by the table_name argument in this list_byom() function.
```python
>>> list_byom(table_name='byom_licensed_models')
model
model_id
iris_model1 b'504B03041400080808...'
```
## retrieve_byom()
The retrieve_byom() API allows a user to retrieve a saved model. Output of this function can be directly
passed as input to the PMMLPredict and H2OPredict functions.
Note:
Some models, such as H2O-DAI, have licenses associated with the models.
When these models are used for scoring, users must retrieve the model by passing relevant license
information. See the license argument for details.
Required Arguments:
* model_id specifies the unique model identifier of the model to be retrieved.
Optional Argument:
* table_name specifies the name of the table to retrieve external model from.
Note:
    * You must either specify this argument, or set the byom model catalog table name
    using set_byom_catalog().
    * If none of them are set, exception is raised; If both of them are set, the settings in
    retrieve_byom() take precedence and is used for function execution.
* schema_name specifies the name of the schema in which the table specified in table_name is
  looked up.
  If this argument is not specified, then table is looked up in the schema associated with the
  current context.
  Note:
    * You must either specify this argument and the table_name argument, or set the byom model
    catalog schema name using set_byom_catalog().
    * If none of them are set, exception is raised; If both of them are set, the settings in
    retrieve_byom() take precedence and is used for function execution.
    * If you specify schema_name, table_name has to be specified, else exception is raised.
  See table in save_byom() that shows different system behaviors based on different input combinations.
License related optional arguments:
* license specifies either the license key itself or name of the column that contains the license key, based
  on where the license key is stored:
  * If the license key is stored in a variable, users can pass it as string.
    license contains the license key itself.
  * If the license key is stored in a table, users pass the name of the column containing the license key.
    license contains the name of the column containing the license key. is_license_column must be set
    to True.
    Also, based on the table where the license information is stored,
    - If the license information is stored in the same model table as that of the model specified in
    the argument table_name, users only need to specify the name of the column containing the
    license key.
    - If the license information is stored in a table different from the one specified in the argument
    table_name, in addition to the column name, users can specify the table name and schema name
    using license_table_name and license_schema_name respectively.
* is_license_column specifies whether the argument license is a license key or column name.
  - When set to True, license contains the name of the column that contains the license key.
- Otherwise, license contains the actual license key.
* license_table_name specifies the name of the table that holds the license key, if the license key is stored
  in a table other than the table specified in table_name.
* license_schema_name specifies the name of the database associated with the license_table_name.
  If not specified, current database is used.
* require_license specifies whether the model to be retrieved is associated with a license.
  The default value is False. If set to True, license information set by set_license() is retrieved.
  Note:
    If license parameters are passed, then this argument is ignored.
#### Example Setup
* Import necessary modules.
```python
>>> import teradataml, os, datetime
>>> from teradataml import save_byom, retrieve_byom, get_context
```
* Get the model file path.
```python
>>> model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "iris_kmeans_model")
```
* Save four models with different parameters.
  * Save a model in the same database without license.
```python
>>> save_byom("model9", model_file, "byom_models")
Model is saved.
```
  * Save a model in another database without license.
```python
>>> save_byom("model6", model_file, "byom_models", schema_name="test")
Model is saved.
```
  * Save a model in the same database with associated license stored in a variable.
```python
>>> save_byom("model5", model_file, "byom_models")
Model is saved.
```
  * Save a model in the same database with license key stored in an additional column "license_data"
    in the same table, for example 4.
```python
>>> save_byom("licensed_model1", model_file, "byom_licensed_models",
additional_columns={"license_data": "A5sUL9KU_kP35Vq"})
Created the model table "byom_licensed_models" as it does not exist.
Model is saved.
```
* Store a license in a variable "lic_key", for "model5" in example 3.
```python
>>> lic_key = "eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI"
```
* Store a license in a table "license" in the column "license_key", for example 5.
```python
>>> # Store the license in a table.
>>> lic_table = "create table license (id integer between 1 and 1,license_key
varchar(2500)) unique primary index(id);"
>>> get_context().execute(lic_table)
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000014AAFF27080>
>>> get_context().execute("insert into license values (1, "peBVRtjA-ib")")
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x0000014AAFF27278>
```
  This table is also created in database ""mldb", for example 6.
#### Example 1: Retrieve a model from a specific table
This example retrieves a model with id "model9" from the table "byom_models".
```python
>>> df = retrieve_byom("model9", table_name="byom_models")
>>> df
model
model_id
model9    b"504B03041400080808..."
```
#### Example 2: Retrieve a model from a specific table in a specific database
This example retrieves a model with id "model6" from the table "byom_models" in the "test" database.
```python
>>> df = retrieve_byom("model6", table_name="byom_models", schema_name="test")
>>> df
model
model_id
model6    b"504B03041400080808..."
```
#### Example 3: Retrieve a model from a specific table, with license key stored in
#### a variable
This example retrieves a model with id "model5" from the table "byom_models" with license key stored in a
variable "lic_key".
```python
>>> df = retrieve_byom("model5", table_name="byom_models", license=lic_key)
>>> df
model                                                         license
model_id
model5    b"504B03041400080808..."  eZSy3peBVRtjA-
ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
```
#### Example 4: Retrieve a model from a specific table, with license key stored in
#### the same table
This example retrieves a model with id "licensed_model1" from the table "byom_licensed_models", and
associated license key stored in the same table in the column "license_data".
```python
>>> df = retrieve_byom("licensed_model1",
table_name="byom_licensed_models",
license="license_data",
is_license_column=True)
>>> df
model          license
model_id
licensed_model1  b"504B03041400080808..."  A5sUL9KU_kP35Vq
```
#### Example 5: Retrieve a model from a specific table, with license key stored in
#### another table in the same database
This example retrieves a model with id "licensed_model1" from the table "byom_licensed_models", and
associated license key is stored in the column "license_key" of another table "license".
```python
>>> df = retrieve_byom("licensed_model1",
table_name="byom_licensed_models",
license="license_key",
is_license_column=True,
license_table_name="license")
>>> df
model      license
model_id
licensed_model1  b"504B03041400080808..."  peBVRtjA-ib
```
#### Example 6: Retrieve a model from a specific table, with license key stored in a
#### table in another database
```python
>>> df = retrieve_byom('licensed_model1',
table_name='byom_licensed_models',
license='license_key',
is_license_column=True,
license_table_name='license',
license_schema_name='mldb')
>>> df
model      license
model_id
licensed_model1  b'504B03041400080808...'  peBVRtjA-ib
```
#### Example 7: Retrieve a model with license key set at session level by
#### set_license(), license is passed as a variable
In this example, retrieve a model with id 'model5' from the table 'byom_models', with license key stored by
set_license() in a variable 'license'.
The catalog information is set using set_byom_catalog() to table_name='byom_models',
schema_name='alice', and is used to retrieve the model.
```python
>>> set_byom_catalog(table_name='byom_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_models' and
schema_name='alice'
>>> set_license(license=license, source='string')
The license parameters are set.
The license is: eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
>>> df = retrieve_byom('model5', require_license=True)
>>> df
model license
model_id
model5 b'504B03041400080808...' eZSy3peBVRtjA-
ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
```
#### Example 8: Retrieve a model with license key set at session level by
#### set_license() where license is passed as a file
In this example, retrieve a model with id 'model5' from the table 'byom_models', with license key stored by
set_license() in a file.
Since the schema name is not provided, default schema is used.
```python
>>> license_file = os.path.join(os.path.dirname(teradataml.__file__),  'data',
'models', 'License_file.txt')
>>> set_license(license=license_file, source='file')
The license parameters are set.
The license is: license_string
>>> df = retrieve_byom('model5', table_name='byom_models', require_license=True)
>>> df
model license
model_id
model5 b'504B03041400080808...' license_string
```
#### Example 9: Retrieve a model with license key stored in a column of another
#### table set at session level by set_license()
In this example, retrieve a model with id 'licensed_model1' with associated license key stored in the column
'license_key' of the table 'license' present in the schema 'alice'.
The byom catalog and license information is set using set_byom_catalog() and set_license() respectively.
Function is executed with license parameters passed, which overrides the license information set at the
session level.
```python
>>> set_byom_catalog(table_name='byom_licensed_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_licensed_models'
and schema_name='alice'
>>> set_license(license=license, source='string')
The license parameters are set.
The license is: eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
>>> df = retrieve_byom('licensed_model1', license='license_key',
is_license_column=True, license_table_name='license')
>>> df
model license
model_id
licensed_model1 b'504B03041400080808...' peBVRtjA-ib
```
#### Example 10: Retrieve a model with license key stored in a column of same
#### table set at session level by set_license()
In this example, retrieve a model with id 'licensed_model1' from the table 'byom_licensed_models' with
associated license key stored in the column 'license_data' of the table 'byom_licensed_models'.
The byom catalog and license information is already set at the session level, passing the table_name to the
function call overrides the byom catalog information at the session level.
```python
>>> set_byom_catalog(table_name='byom_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_models' and
schema_name='alice'
>>> set_license(license='license_data', table_name='byom_licensed_models',
schema_name='alice', source='column')
The license parameters are set.
The license is present in the table='byom_licensed_models', schema='alice'
and column='license_data'.
>>> df = retrieve_byom('licensed_model1',
table_name='byom_licensed_models', require_license=True)
>>> df
model license
model_id
licensed_model1 b'504B03041400080808...' A5sUL9KU_kP35Vq
```
Note:
If require_license=False, which is the default value, the license information is not retrieved.
```python
>>> df = retrieve_byom('licensed_model1', table_name='byom_licensed_models')
>>> df
model
model_id
licensed_model1 b'504B03041400080808...'
```
## delete_byom()
The delete_byom() API allows a user to delete a model from the user specified table in Vantage.
Required Arguments:
* model_id specifies the unique model identifier of the model to be deleted.
Optional Argument:
* table_name specifies the name of the table to delete the model from.
  Note:
    * You must either specify this argument, or set the byom model catalog table name
    using set_byom_catalog().
    * If none of them are set, exception is raised; If both of them are set, the settings in
    delete_byom() take precedence and is used for function execution.
* schema_name specifies the name of the schema in which the table specified in table_name is
  looked up.
  If this argument is not specified, then table is looked up in the schema associated with the
  current context.
  Note:
    * You must either specify this argument and the table_name argument, or set the byom model
    catalog schema name using set_byom_catalog().
    * If none of them are set, exception is raised; If both of them are set, the settings in
    delete_byom() take precedence and is used for function execution.
    * If you specify schema_name, table_name has to be specified, else exception is raised.
See table in save_byom() that shows different system behaviors based on different input combinations.
#### Example Setup
* Import necessary modules.
```python
>>> import teradataml, os, datetime
>>> # importing delete_byom from teradataml
>>> from teradataml from teradataml import save_byom, delete_byom
```
* Get the model file path to use it in examples.
```python
>>> model_file = os.path.join(os.path.dirname(teradataml.__file__), "data",
"models", "iris_kmeans_model")
```
* Save some models with different parameters.
```python
>>> save_byom("model3", model_file, "byom_models")
Model is saved.
>>> save_byom("model4", model_file, "byom_models", schema_name="test")
Model is saved.
```
#### Example 1: Delete a model with specific model_id from a specific table
This example deletes a model with id "model3" from the table "byom_models".
```python
>>> delete_byom(model_id="model3", table_name="byom_models")
Model is deleted.
```
#### Example 2: Delete a model with specific model_id from a specific table in a
#### specific database
This example deletes a model with id "model4" from the table "byom_models" is in the "test" database.
```python
>>> delete_byom(model_id="model4", table_name="byom_models", schema_name="test")
Model is deleted.
```
#### Example 3: Delete a model with specific model_id from a table specified by
#### set_byom_catalog() function
* The model cataloging parameters are set using the set_byom_catalog() function.
```python
>>> set_byom_catalog(table_name='byom_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_models'
and schema_name='alice'
```
* Delete the a model with id 'model4' from the model catalog table specified by
  set_byom_catalog() function.
```python
>>> delete_byom(model_id='model4')
Model is deleted.
```
#### Example 4: Override session level parameters for BYOM model cataloging
This example shows that you can override the session level BYOM model catalog information set by the
set_byom_catalog(), by passing the table as well as schema arguments explicitly.
* The model cataloging table name is set to 'byom_models' using the set_byom_catalog() function.
```python
>>> set_byom_catalog(table_name='byom_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_models'
and schema_name='alice'.
```
* The model is saved in a different table 'byom_licensed_models' which is specified by the table_name
  argument in the save_byom() function.
```python
>>> save_byom('licensed_model2',
model_file=model_file, table_name='byom_licensed_models')
Created the model table 'byom_licensed_models' as it does not exist.
Model is saved.
```
* Delete the model from the table specified by the table_name argument in this delete_byom() function.
```python
>>> delete_byom(model_id='licensed_model2', table_name='byom_licensed_models')
Model is deleted.
```
## set_license()
The set_license() function allows a user to set the license information associated with the externally
generated model in a session level variable which is required by H2O DAI models. It is used by the
retrieve_byom() function to retrieve the license information while retrieving the specific model.
If specified table name does not exist and is not the same as BYOM catalog tables, then this function
creates the table and stores the license information; otherwise, this function just validates and sets the
license information.
The license can be set by passing the license in the following ways:
* Passing the license as a variable;
* Passing the column name in the model table itself;
* Passing the table and the column name containing the license;
* Passing the license in a file.
Required Arguments:
* license specifies the license key information that can be passed as:
  * a variable;
  * in a file;
  * name of the column containing license information in a table specified by the
    table_name argument.
  Note:
    Argument source must be set accordingly.
* source specifies whether the license key specified in license is a string, file or column name.
  The default value is string.
Optional Arguments:
* table_name specifies the name of the table containing the license information.
* schema_name specifies the name of the schema in which the table specified in table_name is
  looked up.
  If not specified, then table is looked up in current schema/database.
Note:
Specify either both or neither of the arguments table_name and schema_name.
#### Example Setup
```python
>>> import os, teradataml
>>> from teradataml import save_byom, retrieve_byom, get_context,
set_license, set_byom_catalog
```
#### Example 1: License is passed as a string
```python
>>> set_license(license='eZSy3peBVRtjA-
ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI',
table_name=None, schema_name=None, source='string')
The license parameters are set.
The license is : eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
```
#### Example 2: License is stored in a file
The file that contains the license is passed as input to license and source must be set to 'file'.
```python
>>> license_file = os.path.join(os.path.dirname(teradataml.__file__), 'data',
'models', 'License_file.txt')
>>> set_license(license=license_file, source='file')
The license parameters are set.
The license is: license_string
```
#### Example 3: License is present in the byom model catalog table itself
* Store a model with license information in the model table.
```python
>>> model_file = os.path.join(os.path.dirname(teradataml.__file__), 'data',
'models', 'iris_kmeans_model')
>>> save_byom('licensed_model1', model_file, 'byom_licensed_models',
additional_columns={"license_data": "A5sUL9KU_kP35Vq"})
Created the model table 'byom_licensed_models' as it does not exist.
Model is saved.
>>> set_byom_catalog(table_name='byom_licensed_models', schema_name='alice')
The model cataloging parameters are set to table_name='byom_licensed_models'
and schema_name='alice'.
```
* Set license.
```python
>>> set_license(license='license_data', source='column')
The license parameters are set.
The license is present in the table='byom_licensed_models',schema='alice'
and column='license_data'.
```
#### Example 4: License is stored in a column
The license information is stored in the column 'license_key' of a table 'license_table'.
* Create a table and insert the license information in the table.
```python
>>> license = 'eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI'
>>> lic_table = 'create table license (id integer between 1 and 1,
license_key varchar(2500)) unique primary index(id);'
>>> get_context().execute(lic_table)
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x000001DC4F2EE9A0>
>>> get_context().execute("insert into license values (1, 'peBVRtjA-ib')")
<sqlalchemy.engine.cursor.LegacyCursorResult object at 0x000001DC4F2EEF10>
```
* Set license.
```python
>>> set_license(license='license_key', table_name='license',
schema_name='alice', source='column')
The license parameters are set.
The license is present in the table='license', schema='alice'
and column='license_key'.
```
## get_license()
The get_license() function allows a user to get the license information set by the set_license() at the
session level.
#### Example Setup
```python
>>> import os, teradataml
>>> from teradataml import save_byom, get_license, set_license
```
#### Example 1: License is passed as a string
```python
>>> set_license(license='eZSy3peBVRtjA-
ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI',
source='string')
The license parameters are set.
The license is: eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
>>> get_license()
The license is: eZSy3peBVRtjA-ibVuvNw5A5sUL9KU_kP35Vq4ZNBQ3iGY6oVSpE6g97sFY2LI
```
#### Example 2: License is present in a column of a table
In this example, license is present in column 'license_data' of the table 'byom_licensed_models'.
```python
>>> set_license(license='license_data',
table_name='byom_licensed_models', schema_name='alice',
source='column')
The license parameters are set.
The license is present in the table='byom_licensed_models', schema='alice'
and column='license_data'.
>>> get_license()
The license is stored in:
table = 'byom_licensed_models'
schema = 'alice'
column = 'license_data'
```
## BYOM Workflows
Download the teradataml_workflows_byom.zip from the attachment  in the left pane. The zip file
includes Jupyter notebooks and HTML files for sample BYOM workflows in the byom folder.