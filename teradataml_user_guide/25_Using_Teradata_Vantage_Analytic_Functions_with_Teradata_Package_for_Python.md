# Using Teradata Vantage Analytic Functions with Teradata Package for Python

Teradata Package for Python provides a Python interface to all of the analytic functions in Vantage: ML
Engine analytic functions and Database Engine 20 in-database analytic functions.
The teradataml analytics package includes two subpackages:
```python
teradataml.analytics.mle
teradataml.analytics.sqle
```
All teradataml analytic functions are in either of these two subpackages.
Teradata recommends importing the desired analytic function in one of the following two ways:
* For best practice, import from the teradataml package.
  For example:
```python
# import DecisionTreePredict from the teradataml package (recommended)
from teradataml import DecisionTreePredict
```
* To choose the engine where the analytic function is used, import from the desired subpackage.
  For example:
```python
# import DecisionTreePredict from the teradataml.analytics.sqle package
from teradataml.analytics.sqle import DecisionTreePredict
```
                                        21
Note:
Older versions of teradataml placed all analytic functions in a single package (teradataml.
```python
analytics), with each analytic function in it’s own module.
```
From release 16.20.00.01, the analytic functions are in either of the two subpackages, with each
analytic function in it’s own module.
Instead of using the old modules in the teradataml.analytics package, Teradata recommends
importing the desired analytic function in one of the ways listed in the previous examples.
Instantiating an analytic function from the old module in the teradataml.analytics package gives
a DeprecationWarning.
For example:
```python
>>> from teradataml.analytics.KMeans import KMeans
>>> KMeans(*args, **kw)
DeprecationWarning:
The "teradataml.analytics.KMeans.KMeans" class has moved to a new package in
version 16.20.00.02.
Import from the teradataml package, teradataml.analytics package, or directly
from the teradataml.analytics.mle.KMeans module.
See the teradataml 16.20.00.02 User Guide for more information.
```
This section shows examples of how the Teradata Package for Python enables users to run a selection of
these analytic functions through a Python interface.
In this section, assume you have the connection to the Database Engine 20 as user "tdapUser", where the
target database is "tdapUserDB".
See Test teradataml Connection to Vantage for details on how to set up a connection and import the
```python
teradataml module.
```
* Usage Notes
* load_example_data() Function
* Use Cases
## Usage Notes
Note:
* Database Engine 20 functions and UAF functions are available only if underlying Vantage, which
    user is connected to, supports those.
* Import these functions only after establishing the connection to Vantage.
    Importing these functions before context creation raises import error.
* You can check whether Database Engine 20 functions and UAF functions are available by running
    the function display_analytic_functions().
    * If these functions are available for the corresponding Vantage version, then
```python
display_analytic_functions() displays those functions.
```
    * Otherwise, these functions are not displayed in display_analytic_functions() output.
### Column Specification in Engine 20 In-Database
### Analytic Functions
#### Column Specification
Some analytic functions in the Database Engine 20 provide arguments for selecting or performing
operations on multiple columns.
teradataml allows user to specify columns in the following ways:
* Pass a single column name.
  For example, column_arg = "col1"
* Pass multiple columns (specific columns only).
  For example, column_arg = ["col1", "col3", "col8"]
* Pass multiple columns using DataFrame.columns and slice filtering.
  For example, column_arg = list(set(df.columns[2:10]) - set(df.columns[5:7]))
* Pass multiple column as a column range.
  For example, column_arg = "column_range"
#### Specifying Column Range column_range
Column range can be specified in the following ways:
* Without column exclusion.
  Syntax: "start_column:end_column"
Note:
    Must be passed as string.
* With column exclusion.
  Syntax: ["start_column:end_column", "-exclude_column1", ...]
  Note:
    Must be passed as a list of strings.
The start_column and end_column can be:
* Column names.
  For example, "column1:column2".
* Nonnegative integers that represent the indexes of columns in the table. The first column has index 0.
  For example, "0:4" specifies the first five columns in the table.
* Empty.
  For example,
```python
":4" or ":columnD" specifies all columns up to and including the column with index 4
```
    or columnD.
```python
"4:" or "columnD:" specifies the column with index 4 (or columnD) and all columns after it.
":" specifies all columns in the table.
```
The exclude_column is a column in the specified range, represented by either its name or its index.
For example, ["0:99", "-[50]", "-column10"] specifies the columns with indexes from 0 to 99, except
the column with index 50 and column10.
Note:
Column ranges cannot overlap, and cannot include any specified column.
When handling column name containing range separators:
* Column can be enclosed in double quotes.
  For example, if DataFrame has columns :columnA, columnB: and :column:C:, columns can be
  selected by enclosing in double quotes:
```python
"\":columnA\"" or '":columnA"'
"\"columnB:\"" or '"columnB:"'
"\":column:C:\"" or '":column:C:"'
```
* You can always use column range and exclusion by index instead of column names.
#### Column Range Examples
Assume "insect_sprays" DataFrame has columns groupA, :groupB, groupC:CC, groupD:,
```python
groupE:EE:EEE and :groupF:, the following examples show how to select range of columns for the
```
group_columns argument in the ANOVA function.
Example 1: Specify range index of columns.
```python
ANOVA(data=insect_sprays,
group_columns="2:5",
alpha = 0.025)
```
Example 2: Specify range index of columns with first column empty.
```python
ANOVA(data=insect_sprays,
group_columns=":5",
alpha=0.025)
```
Example 3: Specify range of columns by their names. Column has separator, hence enclose it in
double quotes.
```python
ANOVA(data=insect_sprays,
group_columns= '"groupC:CC":"groupE:EE:EEE"',
alpha = 0.05)
```
Example 4: Specify range of columns by their names with second column empty. Column has separator,
hence enclose it in double quotes.
```python
ANOVA(data=insect_sprays,
group_columns='"groupE:EE:EEE":',
alpha = 0.05)
```
Example 5: Specify range of columns for all columns. Column has separator, hence enclose it in
double quotes.
```python
ANOVA(data=insect_sprays,
group_columns=[':', "-\":groupB\""], # or [':', "-[2]"]
alpha = 0.05)
```
## load_example_data() Function
The load_example_data() is a helper function that loads the sample datasets.
Teradata Package for Python offers various APIs and each API provides some examples. To test these
examples, users need the sample datasets loaded in Vantage.
The load_example_data() function can only be used in a restricted way. Function arguments only accept
predetermined values as shown in the given examples in this User Guide and the Teradata Package for
Python Function Reference.
* function_name
  This required argument contains the prefix name of the example JSON file to be used to load data.
  Note:
    You must specify the function_name values as specified in the example sections of corresponding
    teradataml APIs. If any other string is passed as prefix input, an error will be raised
    as 'prefix_str_example.json' file not found. This *_example.json file contains the schema
    information for the tables that can be loaded using this JSON file.
* table_name
  This required argument specifies the name(s) of the table to be created in the database.
  Note:
    Table names provided here must have an equivalent datafile (CSV) present at teradataml/data.
    Schema information for the same must also be present in <function_name>_example.json as
    shown in 'function_name' argument description.
Note:
* The function creates a new table in Vantage with the name specified in the table_name argument.
    You must manually drop the table if required.
* If a table with the name provided for table_name argument already exists, this function skips
    creation and loading of the dataset.
* Database used for loading table depends on the following conditions:
    * If the configuration option temp_table_database is set, then the tables are loaded in the
    database specified in this option.
    * If the configuration option temp_table_database is not set and the temp_database_name
    argument is used while creating context, then the tables are loaded in the database specified
    in the temp_database_name argument.
    * If none of them are specified, then the tables are created in the connecting users' default
    database or the connecting database.
#### Example 1: When connection is created without temp_database_name, table is
#### loaded in users' default database
```python
>>> from teradataml import *
>>> con = create_context(host = 'tdhost', username='tduser', password
= 'tdpassword')
>>> load_example_data("pack", "ville_temperature")
# Create a teradataml DataFrame.
>>> df = DataFrame("ville_temperature")
```
#### Example 2: When connection is created with temp_database_name, table is
#### loaded in that database
This examples shows when connection is created with the temp_database_name argument, then tables are
loaded in the database specified in the temp_database_name argument.
This example also demonstrates loading multiple tables in a single function call.
```python
>>> con = create_context(host = 'tdhost', username='tduser', password =
'tdpassword', temp_database_name = "temp_db")
>>> load_example_data("GLM", ["admissions_train", "housing_train"])
# Create teradataml DataFrames.
>>> admission_train = DataFrame(in_schema("temp_db", "admissions_train"))
>>> housing_train = DataFrame(in_schema("temp_db", "housing_train"))
```
#### Example 3: When connection is created with temp_database_name and the
#### configuration option temp_table_database is set
This example shows when connection is created with temp_database_name and the configuration option
temp_table_database is also specified, the table is created in the database specified in the configuration
option temp_table_database.
```python
>>> from teradataml import *
>>> con = create_context(host = 'tdhost', username='tduser', password =
'tdpassword', temp_database_name = "temp_db")
>>> configure.temp_table_database = "temp_db1"
>>> load_example_data("pack", "ville_temperature")
# Create a teradataml DataFrame.
>>> df = DataFrame(in_schema("temp_db1", "ville_temperature"))
```
## Use Cases
* Text Analysis with teradataml Package
* Use ClassificationEvaluator to Evaluate the Classification
* Use GLM Model with teradataml Package
* Use Decision Forest Model with teradataml Package
* Use the KMeans Clustering Function with teradataml Package
* Churn Analysis Using Sessionize and NPath with Teradata Package for Python
* Use DataFrame.map_partition() Function for GLM Model Fitting and Scoring Functions
* Use PMMLPredict to Score using Externally Trained Models
* Use H2OPredict to Score using Externally Trained Models
* Use ONNXPredict to Score using Externally Trained Models
* Use Open Analytics to Score using Externally Trained Models using Apply
### Text Analysis with teradataml Package
This example investigates a log of vehicle complaints that have been categorized as crash-related or not
crash-related. Use this log to build a Naïve Bayes Text Classifier model, and then apply the model to a new
log data to predict if the complaint is associated with a crash.
This example shows the steps to build a Naïve Bayes Text Classifier model and then apply the model to
the new log data.
1. Import the required modules and load the example datasets.
```python
from teradataml import NaiveBayesTextClassifierTrainer
from teradataml import NaiveBayesTextClassifierPredict
from teradataml import TextParser
from teradataml import load_example_data
from teradataml.dataframe.dataframe import DataFrame
# Load the data to run the example.
load_example_data("TextTokenizer","complaints")
```
2. Create a teradataml DataFrame from the training dataset.
```python
complaints = DataFrame.from_table("complaints")
```
3. Create tokens from the training dataset.
```python
text_tokenizer_out = TextParser(data=complaints,
text_column="text_data",
remove_stopwords=True,
accumulate=["doc_id", "category"])
```
4. Create a teradataml DataFrame "tddf_nbayes_tokens" consisting of the tokens from the training
dataset, ignoring the case (lower()).
```python
tddf_nbayes_tokens = text_tokenizer_out.result.assign(drop_columns = True,
doc_id
= text_tokenizer_out.result.doc_id,
token
= text_tokenizer_out.result.token.str.lower(),
category
= text_tokenizer_out.result.category)
```
5. Train a new Naïve Bayes Text Classifier based on the teradataml DataFrame from the training dataset,
using the NaiveBayesTextClassifierTrainer function from teradataml package.
```python
nb_textclassifier_model = NaiveBayesTextClassifierTrainer(data
= tddf_nbayes_tokens,
data_partition_column = "category",
token_column
= "token",
doc_category_column
= "category")
```
6. Get the test data or creates a sample test data.
```python
import pandas as pd
nbayes_test = {"doc_id": range(1, 11),
"text_data":
["ELECTRICAL CONTROL MODULE IS SHORTENING OUT, CAUSING THE VEHICLE TO STALL.
ENGINE WILL BECOME TOTALLY INOPERATIVE. CONSUMER HAD TO CHANGE ALTERNATOR/
BATTERY AND STARTER, AND MODULE REPLACED 4 TIMES, BUT DEFECT STILL OCCURRING
CANNOT DETERMINE WHAT IS CAUSING THE PROBLEM.",
"ABS BRAKES FAIL TO OPERATE PROPERLY, AND AIR BAGS FAILED TO DEPLOY DURING A
CRASH AT APPROX. 28 MPH IMPACT. MANUFACTURER NOTIFIED.",
"WHILE DRIVING AT 60 MPH GAS PEDAL GOT STUCK DUE TO THE RUBBER THAT IS
AROUND THE GAS PEDAL.",
"THERE IS A KNOCKING NOISE COMING FROM THE CATALYTIC CONVERTER, AND THE
VEHICLE IS STALLING. ALSO, HAS PROBLEM WITH THE STEERING.",
"CONSUMER WAS MAKING A TURN, DRIVING AT APPROX 5-10 MPH WHEN CONSUMER HIT
ANOTHER VEHICLE. UPON IMPACT, DUAL AIRBAGS DID NOT DEPLOY. ALL DAMAGE WAS
DONE FROM ENGINE TO TRANSMISSION, TO THE FRONT OF VEHICLE, AND THE VEHICLE
CONSIDERED A TOTAL LOSS.",
"WHEEL BEARING AND HUBS CRACKED, CAUSING THE METAL TO GRIND WHEN MAKING A
RIGHT TURN. ALSO WHEN APPLYING THE BRAKES, PEDAL GOES TO THE FLOOR, CAUSE
UNKNOWN. WAS ADVISED BY MIDAS NOT TO DRIVE VEHICLE- WHEELE COULD COME OFF.",
"DRIVING ABOUT 5-10 MPH, THE VEHICLE HAD A LOW FRONTAL IMPACT IN WHICH THE
OTHER VEHICLE HAD NO DAMAGES. UPON IMPACT, DRIVER'S AND THE PASSENGER'S
AIR BAGS DID NOT DEPLOY, RESULTING IN INJURIES. PLEASE PROVIDE FURTHER
INFORMATION AND VIN#.",
"THE AIR BAG WARNING LIGHT HAS COME ON, INDICATING AIRBAGS ARE INOPERATIVE.
THEY WERE FIXED ONE AT THE TIME, BUT PROBLEM HAS REOCCURRED.",
"CONSUMER WAS DRIVING WEST WHEN THE OTHER CAR WAS GOING EAST. THE OTHER
CAR TURNED IN FRONT OF CONSUMER'S VEHICLE, CONSUMER HIT OTHER VEHICLE AND
STARTED TO SPIN AROUND, COULDN'T STOP, RESULTING IN A CRASH. UPON IMPACT,
AIRBAGS DIDN'T DEPLOY.",
"WHILE DRIVING ABOUT 65 MPH AND THE TRANSMISSION MADE A STRANGE NOISE, AND
THE LEFT FRONT AXLE LOCKED UP. THE DEALER HAS REPAIRED THE VEHICLE."]}
nbayes_test = pd.DataFrame(nbayes_test)
from teradataml import copy_to_sql
copy_to_sql(nbayes_test, table_name="nbayes_test")
```
Next, apply the model to the test data.
7. Create a teradataml DataFrame from the test dataset.
```python
nbayes_test = DataFrame.from_table("nbayes_test")
```
8. Create tokens from the test dataset.
```python
text_tokenizer_out_2 = TextParser(data=nbayes_test,
text_column="text_data",
remove_stopwords=True,
accumulate="doc_id")
```
9. Create a teradataml DataFrame "tddf_nbayes_tokens_test" with the tokens from the test dataset,
ignoring the case (lower()).
```python
tddf_nbayes_tokens_test = text_tokenizer_out_2.result.assign(drop_columns
= True,
doc_id
= text_tokenizer_out_2.result.doc_id,
token
= text_tokenizer_out_2.result.token.str.lower() )
```
10. Predict the categories ('crash' or 'no crash') by applying the Naïve Bayes Text
Classifier model to the teradataml DataFrame from the test dataset, using the
```python
NaiveBayesTextClassifierPredict function.
nb_textclassifier_pred = NaiveBayesTextClassifierPredict(newdata
= tddf_nbayes_tokens_test,
object
= nb_textclassifier_model.result,
newdata_partition_column = "doc_id",
input_token_column
= "token",
doc_id_columns
= "doc_id" )
```
11. Inspect the results.
```python
nb_textclassifier_pred
```
### Use ClassificationEvaluator to Evaluate the Classification
This example performs prediction on a model and evaluates the model, then generates the statistics for
the classification.
1. Set up the environment.
a. Import required libraries.
```python
import tempfile
import getpass
from teradataml import DataFrame, load_example_data, create_context
```
b. Create the connection to database.
```python
con = create_context(host=getpass.getpass("Hostname: "),
username=getpass.getpass("Username: "),
password=getpass.getpass("Password: "))
```
c. Load example data and create required dataframes.
```python
load_example_data("textparser", ["complaints", "stop_words"])
complaints = DataFrame.from_table("complaints")
stop_words = DataFrame.from_table("stop_words")
```
2. Train the model and create classifier for crash or nocrash.
a. Check the list of available analytic functions.
```python
display_analytic_functions()
```
b. Import functions TextParser, NaiveBayesTextClassifierTrainer, NaiveBayesTextClassifierPredict.
```python
from teradataml import TextParser,
NaiveBayesTextClassifierTrainer, NaiveBayesTextClassifierPredict
```
c. Tokenize the "text_column" and accumulate result by "doc_id" and "category".
```python
complaints_tokenized = TextParser(data=complaints,
text_column="text_data",
object=stop_words,
remove_stopwords=True,
accumulate=["doc_id", "category"])
```
d. Calculate the conditional probabilities for token-category pairs.
```python
NaiveBayesTextClassifierTrainer_out =
NaiveBayesTextClassifierTrainer(data=complaints_tokenized.result,
token_column="token",
doc_category_column="category")
```
e. Print the result DataFrames.
```python
print(NaiveBayesTextClassifierTrainer_out.result)
print(NaiveBayesTextClassifierTrainer_out.model_data)
```
f. Score the data using NaiveBayesTextClassifierPredict() on model generated by
    NaiveBayesTextClassifier() where model_type is "MULTINOMIAL".
```python
nbt_predict_out = NaiveBayesTextClassifierPredict(object
= NaiveBayesTextClassifierTrainer_out.model_data,
newdata
= complaints_tokenized.result,
input_token_column
= 'token',
accumulate="category",
doc_id_columns
= 'doc_id')
```
g. Print the result DataFrame.
```python
print(nbt_predict_out.result)
```
3. Convert prediction column and category column to same DataType.
```python
from teradataml import ConvertTo
predicted_data = ConvertTo(data = nbt_predict_out.result,
target_columns = ["category", "prediction"],
target_datatype
= ["VARCHAR(charlen=20,charset=UNICODE,casespecific=NO)"])
```
4. Evaluate classifier.
a. Import function ClassificationEvaluator.
```python
from teradataml import ClassificationEvaluator
```
b. Evaluate classification.
```python
ClassificationEvaluator_obj =
ClassificationEvaluator(data=predicted_data.result,
observation_column='category',
prediction_column='prediction',
labels=['no_crash','crash'])
```
c. Print the result DataFrames.
```python
print(ClassificationEvaluator_obj.result)
print(ClassificationEvaluator_obj.output_data)
```
### Use GLM Model with teradataml Package
This example shows the steps to build a Generalized Linear model (GLM) and then apply the model to
the new testing admissions data. The data set contains two classes, where one class represents the
successful admission while the other represents no admission.
Note:
This example uses Vantage Analytic Library (VAL) functions. You must make sure VAL functions are
installed in Vantage before using VAL functions.
1. Import required libraries.
```python
from teradataml import GLM
from teradataml import TDGLMPredict
from teradataml.dataframe.dataframe import DataFrame
```
2. Create training data.
a. If the input table (admissions_train) does not exist already, create the table and load the dataset
    into the table.
```python
load_example_data("dataframe", "admissions_train")
```
b. Create a teradataml DataFrame for the training dataset from "admissions_train" table.
```python
admissions_train = DataFrame.from_table("admissions_train")
```
3. Convert categorical columns in the input data table into numeric columns using OneHotEncoder() and
Transform() functions from VAL, as GLM() function supports only numeric columns.
a. Import required libraries.
```python
from teradataml import valib, OneHotEncoder, Retain
```
b. Configure VAL install location.
```python
configure.val_install_location = "VAL"
```
c. Define encoders for categorical columns.
```python
masters_code = OneHotEncoder(values=["yes", "no"],
columns="masters",
out_columns="masters")
stats_code = OneHotEncoder(values=["Advanced", "Novice"],
columns="stats",
out_columns="stats")
programming_code = OneHotEncoder(values=["Advanced",
"Novice", "Beginner"],
columns="programming",
out_columns="programming")
```
d. Retain numerical columns.
```python
retain = Retain(columns=["admitted", "gpa"])
```
e. Transform categorical columns to numeric columns.
```python
all_numeruc_admissions_train = valib.Transform(data=admissions_train,
one_hot_encode=[masters_code, stats_code, programming_code],
retain=retain)
```
4. Train a new Generalized Linear Model (GLM) based on the teradataml DataFrame from the training
dataset, using the train function - GLM() function from teradataml package.
```python
glm_train = GLM(formula="admitted ~ gpa + yes_masters + no_masters +
Advanced_stats + Novice_stats + Advanced_programming + Novice_programming
+ Beginner_programming",
data=all_numeruc_admissions_train.result,
learning_rate="INVTIME",
momentum=0.80)
```
Next, apply the model to the test data using TDGLMPredict() function.
5. Create test data.
a. If the input table (admissions_test) does not exist already, create the table and load the dataset into
    the table.
```python
load_example_data("GLMPredict", "admissions_test")
```
b. Create a teradataml DataFrame for the test dataset from "admissions_test" table.
```python
admissions_test = DataFrame.from_table("admissions_test")
```
6. Convert categorical columns in test input data table into numeric columns using OneHotEncoder()
and Transform() functions from VAL, as GLM() function has created the model on
"all_numeric_admissions_train" table having only numeric columns.
a. Import required libraries.
```python
from teradataml import valib, OneHotEncoder, Retain
```
b. Configure VAL install location.
```python
configure.val_install_location = "VAL"
```
c. Define encoders for categorical columns.
```python
masters_code = OneHotEncoder(values=["yes", "no"],
columns="masters",
out_columns="masters")
stats_code = OneHotEncoder(values=["Advanced", "Novice"],
columns="stats",
out_columns="stats")
programming_code = OneHotEncoder(values=["Advanced",
"Novice", "Beginner"],
columns="programming",
out_columns="programming")
```
d. Retain numerical columns.
```python
retain = Retain(columns=["admitted", "gpa"])
```
e. Transform categorical columns to numeric columns.
```python
all_numeric_admissions_test= valib.Transform(data=admissions_test,
one_hot_encode=[masters_code, stats_code, programming_code],
retain=retain)
```
7. Predict the admission status by applying the Generalized Linear Model (GLM) to the teradataml
DataFrame from the test dataset, using the TDGLMPredict() function and output of the train function.
```python
tdglmpredict_out = TDGLMPredict(object= glm_train.result,
newdata= all_numeric_admissions_test.result,
id_column="id")
```
8. Inspect the results.
```python
tdglmpredict_out.result
```
### Use Decision Forest Model with teradataml Package
This section uses iris data with three classes. The dataset contains 150 samples, each with four features
describing flower properties, and a fifth column indicates the flower species.
In this example, you build a Decision Forest model based on the training dataset and apply the model to
the test dataset to evaluate the performance of the model.
1. Import the required modules.
```python
from teradataml import DecisionForest
from teradataml import DecisionForestPredict
from teradataml import load_example_data
from teradataml.dataframe.dataframe import DataFrame
```
2. If the input table "iris_input" does not already exist, create it and load the dataset.
```python
load_example_data("byom", "iris_input")
```
3. Create a teradataml DataFrame from the loaded dataset.
a. Create a teradataml DataFrame "iris_input" consisting the tokens from the training dataset.
```python
iris_input = DataFrame("iris_input")
```
b. Create two samples of input data: sample 1 has 80% of the total rows for training the model
    ("iris_train"), and sample 2 has 20% of the total rows for testing the model ("iris_test").
    First, sample the "iris_input" dataframe.
```python
iris_sample = iris_input.sample(frac=[0.8, 0.2])
```
c. Create train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column as it is
    not required for training model.
```python
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid",
axis = 1)
```
d. Create test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column as it is not
    required for scoring.
```python
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid",
axis = 1)
```
4. Train a new Decision Forest model based on the teradataml DataFrame "iris_train" from the training
dataset, using the DecisionForest function from teradataml package.
This can be done with or without using the formula argument.
Example 1: Train the decision forest Classification model using input teradataml dataframe and
provided the formula argument.
```python
formula = "species ~ sepal_length + sepal_width + petal_length + petal_width"
# Train the Decision Forest model.
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
```
Example 2: Train the same decision forest Classification model (rft_model) without using the
formula argument.
```python
rft_model = DecisionForest(data=iris_train,
input_columns=["sepal_length", "sepal_width",
"petal_length", "petal_width"],
response_column="species",
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
```
Once the model is created, you can apply the model to the test dataset.
5. Predict the iris species by applying the Decision Forest model to the teradataml DataFrame "iris_test"
from the test dataset, using the DecisionForestPredict function.
```python
decision_forest_predict_out = DecisionForestPredict(object = rft_model,
newdata = iris_test,
id_column = "id",
detailed = False,
terms = ["species"]
```
6. Inspect the results.
```python
decision_forest_predict_out.result
```
### Use the KMeans Clustering Function with teradataml Package
This example uses the US Arrests data of 50 samples containing statistics for arrests made per 100,000
residents for assault, murder, and rape in each of the 50 US states in 1973, along with the percentage of
the population living in urban areas.
Example here shows how to use KMeans clustering function.
1. Import the required modules.
```python
from teradataml import KMeans, KMeansPredict
from teradataml.dataframe.dataframe import DataFrame
from teradataml.data.load_example_data import load_example_data
import matplotlib.pyplot as plt
```
2. If the input table "kmeans_us_arrests_data" does not already exist, create the table and load the
datasets into the table.
```python
load_example_data("kmeans", "kmeans_us_arrests_data")
```
3. Create a teradataml DataFrame from "kmeans_us_arrests_data".
```python
## Creating TeradataML dataframes
df_train = DataFrame('kmeans_us_arrests_data')
# Print train data to see how the sample train data looks like
print("\nHead(10) of Train data:")
print(df_train.head(10))
```
4. In the training dataset, the features are 'sno', 'state', 'murder', 'assault', 'urban_pop' and 'rape'. And 'sno'
to 'state' is a one-to-one mapping. For training, drop off the repeated feature 'state'.
```python
# A dictionary to get 'sno' to 'state' mapping. Required for plotting.
df1 = df_train.select(['sno', 'state'])
sno_to_state = dict(df1.to_pandas()['state'])
print(sno_to_state)
# No need of 'state' columns, instead we have 'sno' column for the same
df_train = df_train.drop(['state'], axis=1)
colnames = ["sno", "murder", "assault", "urban_pop", "rape"]
```
5. Apply KMeans algorithm to generate two clusters and inspect the outputs.
a. Apply KMeans algorithm.
```python
## Train the KMeans model with 2 clusters.
kMeans_model = KMeans(id_column="sno",
target_columns=['murder', 'assault',
'urban_pop', 'rape'],
data=df_train,
num_init=10,
num_clusters=2)
# Print the KMeans model training results
print(kMeans_model.result)
```
b. 'model_data' dataframe presents overall summary of the trained KMeans model.
```python
print(kMeans_model.model_data)
```
c. Use KMeansPredict function to perform predictions using trained KMeans model.
```python
# KMeansPredict function performs prediction on the dataset using trained
KMeans model.
kMeans_output =  KMeansPredict(object=kMeans_model.result,
data=df_train)
```
d. 'result' dataframe presents the 'sno' representing state and the corresponding cluster id for all
    the samples.
```python
kMeans_output.result.head(30)
```
6. Quick Analysis of clustering output by plotting clusters based on features.
```python
## Inner join of clustered_output to actual dataset df_train We shall use
the data from df1 to plot.
df1 = df_train.join(kMeans_output.clustered_output, how='inner', on=['sno'],
lsuffix='t1', rsuffix='t2')
print("\nInner join of clustered_output to actual dataset df_train:")
print(df1)
```
a. Plot clusters based on the two features 'urban_pop' and 'murder'.
```python
# Selecting only the necessary features for plot.
df3 = df1.select(['t1_sno', 'urban_pop', 'murder', 'td_clusterid_kmeans'])
# Since there is no plotting possible for teradataml DataFrame, we are
converting it to
# pandas dataframe and then to numpy_array 'numpy_df' to use matplotlib
library of python.
pandas_df = df3.to_pandas()
numpy_df = pandas_df.values
# Setting figure display size.
plt.rcParams['figure.figsize'] = [15, 10]
# Coloring based on 'td_clusterid_kmeans'.
plt.scatter(numpy_df[:,1], numpy_df[:,2], c=numpy_df[:,3],
cmap='winter_r', alpha=0.4)
for ind, value in enumerate(numpy_df[:, 0]):
# sno_to_state is used hear to get state names.
plt.text(numpy_df[ind,1], numpy_df[ind,2],
sno_to_state[int(value)], fontsize=14)
plt.xlabel('urban_pop')
plt.ylabel('murder')
plt.show()
```
    After running these commands, the following plot shows:
b. Plot the clusters based on the two features 'rape' and 'murder'.
```python
# Selecting only the necessary features for plot.
df3 = df1.select(['t1_sno', 'rape', 'murder', 'td_clusterid_kmeans'])
# Since there is no plotting possible for teradataml DataFrame, we are
converting it to
# pandas dataframe and then to numpy_array 'numpy_df' to use matplotlib
library of python.
pandas_df = df3.to_pandas()
numpy_df = pandas_df.values
# Coloring based on 'td_clusterid_kmeans'.
plt.scatter(numpy_df[:,1], numpy_df[:,2], c=numpy_df[:,3],
cmap='winter_r', marker='^', alpha=0.4)
for ind, value in enumerate(numpy_df[:, 0]):
# sno_to_state is used hear to get state names.
plt.text(numpy_df[ind,1], numpy_df[ind,2],
sno_to_state[int(value)], fontsize=14)
plt.xlabel('rape')
plt.ylabel('murder')
plt.show()
```
    After running these command, the following plot shows:
### Churn Analysis Using Sessionize and NPath with Teradata
### Package for Python
This example uses the retail dataset of product purchase and returns to perform churn analysis.
1. Import the required modules.
```python
from teradataml.analytics.sqle.Sessionize import Sessionize
from teradataml.analytics.sqle.NPath import NPath
from teradataml.dataframe.dataframe import DataFrame, in_schema
from teradataml.data.load_example_data import load_example_data
```
2. Load the retail churn data.
```python
load_example_data("sessionize", "retail_churn_table")
```
3. Create the DataFrame on the loaded dataset 'retail_churn_table'.
```python
df1 = DataFrame(in_schema('alice', 'retail_churn_table'))
# in_schema(databaseName, tableName) - Assuming table has been created in
'alice' database.
print("***** Let's take a peek at the data *****")
print(df1)
print("\n***** Retails Churn Table Column Types *****")
print(df1.dtypes)
print("\n***** Row Count and column count for the retail data *****")
print(df1.shape)
```
4. Sessionize.
The Sessionize function maps each click in a session to a unique session identifier. A session is defined
as a sequence of clicks by one user that are separated by at most n seconds.
a. Example 1: Call the Sessionize function – NO CHURN CUSTOMER.
    The non-churn group (denoted by the churn flag value of 'N'). Filter rows where churn_flag = 'N'.
```python
input_retail_data = df1[df1.churn_flag == 'N']
print("***** No churn data *****")
print(input_retail_data)
# Sessionize the non-churn data
daily_session_nochurn = Sessionize(data = input_retail_data,
time_out = 86400.0,
data_order_column = 'datestamp',
data_partition_column = 'customer_id',
time_column = 'datestamp')
# Data checking
# SELECT * FROM xxxx ORDER BY 2 ASC WHERE customer_id=1531;
# Here xxxx is the table/view created from daily_session_nochurn.result.
res = daily_session_nochurn.result
res = res[res.customer_id == 1531].sort(['datestamp'],ascending = True)
print(res)
```
b. Example 2: Call the Sessionize function – CHURN CUSTOMER.
    The churn group (denoted by the churn flag value of 'Y'). Filter rows where churn_flag = 'Y'.
```python
input_retail_data2 = df1[df1.churn_flag == 'Y']
print("***** Churn data *****")
print(input_retail_data2)
# Sessionize the churn data
daily_session_churn = Sessionize(data = input_retail_data2,
time_out = 86400.0,
data_order_column = 'datestamp',
data_partition_column = 'customer_id',
time_column = 'datestamp')
# Data checking
# SELECT * FROM  xxxx ORDER BY 2 ASC WHERE customer_id=64497;
# Here xxxx is the table/view created from daily_session_churn.result.
res2 = daily_session_churn.result
res2 = res2[res2.customer_id == 64497].sort(['datestamp'], ascending=True)
res2
print(res2)
```
5. Use NPath function on the outcome of Sessionize to perform Pathing.
Pathing is the process of discovering a sequence of antecedent actions that occur prior to a specific
event of interest. For example, a buyer could browse a site, call a company, read online reviews, test
out the product, and then purchase.
Pathing discovers the most salient patterns across a group of individuals or entities based on which
further actions are considered.
a. Call the NPath function – NO CHURN CUSTOMERS.
```python
npath_nochurn = NPath(data1 = daily_session_nochurn.result,
mode = "NONOVERLAPPING",
pattern ='E*.C',
symbols = ["EVENT = 'Purchase'  AS C", "EVENT <> 'Purchase'
AS E"],
result = ["FIRST(customer_id OF ANY(E,C)) AS customer_id",
"FIRST(datestamp OF ANY(E,C)) AS DS_START",
"LAST(datestamp OF ANY(E,C)) AS DS_END",
"COUNT(* OF E) AS EVENT_CNT",
"ACCUMULATE(EVENT OF ANY(E,C)) AS PATH"],
data1_partition_column = ['customer_id', 'SESSIONID'],
data1_order_column = 'datestamp')
# Shape of the resultant dataframe
npath_nochurn.result.shape
# Print result
npath_nochurn.result.to_pandas().head(10)
# Path for a particular customer, 1895
res3 = npath_nochurn.result
res3 = res3[res3.customer_id == 1895]
res3.to_pandas()
```
b. Call the NPath function – CHURN CUSTOMERS.
```python
npath_churn = NPath(data1 = daily_session_churn.result,
mode = "NONOVERLAPPING",
pattern ='E*.C',
symbols = ["EVENT = 'Product Return'  AS C",
"EVENT <> 'Product Return' AS E"],
result = ["FIRST(customer_id OF ANY(E,C)) AS customer_id",
"FIRST(datestamp OF ANY(E,C)) AS DS_START",
"LAST(datestamp OF ANY(E,C)) AS DS_END",
"COUNT(* OF E) AS EVENT_CNT",
"ACCUMULATE(EVENT OF ANY(E,C)) AS PATH"],
data1_partition_column = ['customer_id', 'SESSIONID'],
data1_order_column = 'datestamp')
# Shape of the resultant dataframe
npath_churn.result.shape
# Print result
npath_churn.result.to_pandas().head(10)
# Path for a particular customer, 64115
res4 = npath_churn.result
res4 = res4[res4.customer_id == 64115]
res4.to_pandas()
```
### Use DataFrame.map_partition() Function for GLM Model Fitting
### and Scoring Functions
This example shows how to fit one or multiple GLM models to the housing data, and then use the models
to price the houses in the test data using DataFrame.map_partition() function.
1. Set up
a. Load example data.
    The required training and test datasets are included the in the teradataml package, and you can
    use the load_example_data() Function to load the example data.
```python
>>> load_example_data("GLMPredict", ["housing_test","housing_train"])
```
b. Create input DataFrames.
```python
>>> train = DataFrame('housing_train')
>>> test = DataFrame('housing_test')
```
2. Model fitting
a. Define the user function to fit multiple models to the partitions in housing train dataset using the
```python
statsmodels functions.
```
    >>> # Define the function that we want to use to fit multiple GLM models, one
    for each home style.
    >>> def glm_fit(rows):
    """
    DESCRIPTION:
    Function that accepts a iterator on a pandas DataFrame
    (TextFileObject) created using
    'chunk_size' with pandas.read_csv(), and fits a GLM model to it.
The underlying data is the housing data with 12 independent
    variable (inluding the home style)
    and one dependent variable (price).
    RETURNS:
    A numpy.ndarray object with two elements:
    * The homestyle value (type: str)
    * The GLM model that was fit to the corresponding data, which is
    serialized using pickle
    and base64 encoded. We use decode() to make sure it is of type
    str, and not bytes.
    """
    # Read the entire partition/group of rows in a pandas DataFrame - pdf.
    data = rows.read()
    # Add the 'intercept' column along with the features.
    data['intercept'] = 1.0
    # Function does not process the partition if there are no rows here.
    if data.shape[0] > 0:
    # Fit the model using R-style formula to specify categorical
    variables as well.
    # Use 'disp=0' to prevent sterr output.
    model = smf.glm('price ~ C(recroom) + lotsize + stories + garagepl
    + C(gashw) +'
    ' bedrooms + C(driveway) + C(airco) + C(homestyle)
    + bathrms +'
    ' C(fullbase) + C(prefarea)',
    family=sm.families.Gaussian(), data=data).fit(disp=0)
    # Serialize and base64 encode the model in prepration to output it.
    modelSer = b64encode(dumps(model))
    # The user function can either return a value of supported type
    # (numpy array, Pandas Series, or Pandas DataFrame),
    # or just print it to find it's way to the output.
    # Here we return it as a numpy ndarray object.
    # Note that we use decode for the serialized model so that it is
    # represented in the ascii form (which is what base64
    encoding does),
    # instead of bytes.
    return asarray([data.loc[0]['homestyle'], modelSer.decode('ascii')])
b. Use the defined function "glm_fit()" to fit the model on group of the housing data where the grouping
    is done by the 'homestyle'.
    a. Apply the "glm_fit" function defined in the previous step to create a model for every 'homestyle'
    in the training dataset. Specify the output column names and their types with the returns
    argument since the output is not similar to the input.
```python
>>> model =
train.map_partition(glm_fit, data_partition_column='homestyle',
returns=OrderedDict([('homestyle',
train.homestyle.type),('model', CLOB())]))
```
    b. Print the model to show the model table has been created successfully.
```python
>>> print(model.head())
model
homestyle
Eclectic        gANjc3RhdHNtb2RlbHMuZ2VubW9kLmdlbmVyYWxpemVkX2...
Classic         gANjc3RhdHNtb2RlbHMuZ2VubW9kLmdlbmVyYWxpemVkX2...
bungalow        gANjc3RhdHNtb2RlbHMuZ2VubW9kLmdlbmVyYWxpemVkX2...
```
3. Scoring
a. Use window functions to assign row numbers to each subset of data corresponding to a particular
    'homestyle'. This is to extend the table to add the model corresponding to the 'homestyle' as the
    last column value for the first row in the partition, making it easier for the scoring function to read
    the model and then score the input records based on it.
    a. Create row number column ('row_id') in the 'test' DataFrame.
```python
>>> test_with_row_num = test.assign(row_id =
func.row_number().over(partition_by=test.homestyle.expression,
order_by=test.sn.expression.desc()))
```
    b. Join it with the model created based on the value of 'homestyle'.
```python
>>> temp = test_with_row_num.join(model, on
= [(test_with_row_num.homestyle == model.homestyle)],
rsuffix='r', lsuffix='l')
```
    c. Set the model column to NULL when row_id is not 1.
```python
>>> temp = temp.assign(modeldata = case([(temp.row_id == 1,
literal_column(temp.model.name))], else_ = None))
```
    d. Drop the extraneous columns created in the processing.
```python
>>> temp = temp.assign(homestyle
= temp.l_homestyle).drop('l_homestyle',
axis=1).drop('r_homestyle',axis=1).drop('model', axis=1)
```
    e. Reorder the columns to have the housing data columns positioned first, followed by the row_id
    and modeldata.
```python
>>> new_test = temp.select(test.columns + ['row_id', 'modeldata'])
```
b. Define the user function that will score the test data to predict the prices based on the features.
```python
>>> DELIMITER = '\t'
>>> QUOTECHAR = None
```
    >>> def glm_score(rows):
    """
    DESCRIPTION:
    Function that accepts a iterator on a pandas DataFrame
    (TextFileObject) created using
    'chunk_size' with pandas.read_csv(), and scores it based on the
    model found in the data.
    The underlying data is the housing data with 12 independent
    variable (inluding the home style)
    and one dependent variable (price).
    The function chooses to output the values itself, rather than
    returning objects of supported type.
    RETURNS:
    None.
    """
    model = None
    for chunk in rows:
    # Process data only if there is any, i.e. only when the chunk read
    has any rows.
    if chunk.shape[0] > 0:
    if model is None:
    # Read the model once (it is found only once)
    per partition.
    model = loads(b64decode(chunk.loc[0].iloc[-1]))
    # Exclude the row_id and modeldata columns from the scoring
    dataset as they are no longer required.
    chunk = chunk.iloc[:,:-2]
    # For prediction, exclude the first two column ('sn' - not
    relevant, and 'price' - the dependent variable).
    prediction = model.predict(chunk.iloc[:,2:])
    # Concat the chunk with the prediction column (Pandas Series)
    to form a DataFrame.
    outdf = concat([chunk,
    prediction], axis=1)
    # We just cannot return this DataFrame yet as more chunks need
    to be processed.
    # In such scenarios, there are two options:
    #   1. print the output here, or
    #   2. keep concatenating the results of each chunk to create
    a final resultant pandas DataFrame to return.
    # In this example, use option #1 here.
    for _, row in outdf.iterrows():
    if QUOTECHAR is not None:
    # A NULL value should not be enclosed in quotes.
    # The CSV module has no support for such output with
    writer, and hence the custom formatting.
    values = ['' if isna(s) else "{}{}
    {}".format(QUOTECHAR, str(s), QUOTECHAR) for s in row]
    else:
    values = ['' if isna(s) else str(s) for s in row]
    print(DELIMITER.join(values), file=sys.stdout)
c. Perform the actual scoring by calling the map_partition() method on the test data.
    a. Specify the output of the function. It has one more column than the input.
```python
>>> returns = OrderedDict([(col.name, col.type) for col in
test._metaexpr.c] + [('prediction', FLOAT())])
```
    b. Scoring using map_partition(), using the  data_order_column  argument to order by the 'row_id'
    column so that the model is read before any data that need to be scored.
```python
>>> prediction = new_test.map_partition(glm_score,
returns=returns,
data_partition_column='homestyle',
data_order_column='row_id')
```
    c. Print the scoring result.
    >>> print(prediction.head())
    price   lotsize bedrooms    bathrms stories driveway    recroom fullbase    gashw   airco
    garagepl    prefarea    homestyle   prediction
    sn
    469 55000.0  2176.0       2           1       2       yes       yes      no       no      no
    0           yes     Eclectic    64597.746106
    301 55000.0  4080.0       2           1       1       yes       no       no       no      no
    0           no      Eclectic    54979.762152
    463 49000.0  2610.0       3           1       2       yes       no       yes      no      no
    0           yes     Classic     46515.461314
    177 70000.0  5400.0       4           1       2       yes       no       no       no      no
    0           no      Eclectic    63607.229642
    38  67000.0  5170.0       3           1       4       yes       no       no       no      yes
    0           no      Eclectic    78029.766193
    13  27000.0  1700.0       3           1       2       yes       no       no       no      no
    0           no      Classic     39588.073581
    255 61000.0  4360.0       4           1       2       yes       no       no       no      no
    0           no      Eclectic    61320.393435
    53  68000.0  9166.0       2           1       1       yes       no      yes       no      yes
    2           no      Eclectic    76977.937496
    364 72000.0 10700.0       3           1       2       yes       yes     yes       no      no
    0           no      Eclectic    80761.658291
    459 44555.0  2398.0       3           1       1       yes       no       no       no      no
    0           yes     Classic     42921.671929
4. Model generated on client, and scored on Database Engine 20.
This step shows how a model generated locally on the client machine can be used to score data
on Vantage.
a. First, create a sklearn GLM model, and serialize and base64-encode it. Fit the model on the entire
    training housing data using the sklearn package on the client machine .
    a. Import the pandas module.
```python
>>> import pandas as pd
```
    b. Read the housing_train.csv file (shipped with the teradataml package) into a
    pandas DataFrame.
```python
>>> with open('path_on_local_machine_to_housing_data\\housing_train.csv',
'r') as f:
housing_train = pd.read_csv(f)
```
    c. Encode the categorical columns.
```python
>>> replace_dict = {'driveway': {'yes': 1, 'no': 0}, 'recroom':
{'yes': 1, 'no': 0}, 'fullbase': {'yes': 1, 'no': 0}, 'gashw': {'yes':
1, 'no': 0}, 'airco': {'yes': 1, 'no': 0}, 'prefarea': {'yes': 1,
'no': 0}, 'homestyle': {'Classic': 1, 'Eclectic': 2, 'bungalow': 3}}
```
    d. Replace the values  inplace .
```python
>>> housing_train.replace(replace_dict, inplace=True)
```
    e. Fit the GLM model.
```python
>>> model = LogisticRegression(max_iter=5000, solver='lbfgs',
multi_class='auto').fit(housing_train.iloc[:,2:], housing_train.price)
```
    f. Serialize and base64-encode the GLM model.
```python
>>> modelSer = b64encode(dumps(model)).decode('ascii')
```
b. Define a user function which accepts the model and the dataset to use it for scoring
    with map_partition().
    >>> def glm_score_local_model(rows, model):
    """
    DESCRIPTION:
    Function that accepts a iterator on a pandas DataFrame
    (TextFileObject) created using
    'chunk_size' with pandas.read_csv(), and scores it based on the
    model passed to the function as
    the second argument.
    The underlying data is the housing data with 12 independent
    variable (inluding the home style)
    and one dependent variable (price).
    The function concatenates the result of all chunk scoring
    operation into a final pandas DataFrame to return.
    RETURNS:
    pandas DataFrame.
    """
    # Decode and deserialize the model.
    model = loads(b64decode(model))
    result_df = None
    for chunk in rows:
    # Process data only when the chunk read has any rows.
    if chunk.shape[0] > 0:
    # Perform the encoding for the categorical columns.
    chunk.replace(replace_dict, inplace=True)
    # For prediction, exclude the first two column ('sn' - not
    relevant, and 'price' - the dependent variable).
    prediction = pd.Series(model.predict(chunk.iloc[:,2:]))
    # Concat the chunk with the prediction column (Pandas Series)
    to form a DataFrame.
    outdf = concat([chunk, prediction], axis=1)
    # We just cannot return this DataFrame yet as more chunks need
    to be processed.
    # In such scenarios, there are two options:
    #   1. print the output here, or
    #   2. keep concatenating the results of each chunk to create
    a final resultant Pandas DataFrame to return.
    # In this example, use option #2 here.
    if result_df is None:
    result_df = outdf
    else:
result_df = concat([result_df, outdf], axis=0)
    # Return the result pandas DataFrame.
    return result_df
c. Call the map_partition() method for the test data to score the data and predict the prices.
    a. Specify the output of the function. It has one more column than the input.
```python
>>> # Note that here the output of the function is going to have one
more column than the input,
>>> # and we must specify the same.
>>> returns = OrderedDict([(col.name, col.type) for col in
test._metaexpr.c] + [('prediction', FLOAT())])
```
    b. Scoring using map_partition(), using the  data_order_column  argument to order by the 'row_id'
    column so that the model is read before any data that need to be scored.
```python
>>> prediction = test.map_partition(lambda rows:
glm_score_local_model(rows, modelSer),
returns=returns,
data_partition_column='homestyle')
```
    c. Print the scoring result.
    >>> print(prediction.head())
    price  lotsize  bedrooms  bathrms  stories driveway recroom fullbase gashw airco  garagepl
    prefarea homestyle  prediction
    sn
    25   42000.0   4960.0         2        1        1        1       0        0     0     0
    0        0         1     50000.0
    53   68000.0   9166.0         2        1        1        1       0        1     0     1
    2        0         2     70000.0
    111  43000.0   5076.0         3        1        1        0       0        0     0     0
    0        0         1     50000.0
    117  93000.0   3760.0         3        1        2        1       0        0     1     0
    2        0         2     62000.0
    140  43000.0   3750.0         3        1        2        1       0        0     0     0
    0        0         1     50000.0
    142  40000.0   2650.0         3        1        2        1       0        1     0     0
    1        0         1     48000.0
    157  60000.0   2953.0         3        1        2        1       0        1     0     1
    0        0         2     52000.0
    161  63900.0   3162.0         3        1        2        1       0        0     0     1
    1        0         2     52000.0
    176  57500.0   3630.0         3        2        2        1       0        0     1     0
    2        0         2     60000.0
    177  70000.0   5400.0         4        1        2        1       0        0     0     0
    0        0         2     60000.0
### Use PMMLPredict to Score using Externally Trained Models
This example uses the iris_input dataset and performs a prediction on each row of the input table using a
model previously trained in PMML and then loaded into the database.
1. Set up the environment.
a. Import required libraries.
```python
import tempfile
import getpass
from teradataml import PMMLPredict, DataFrame, load_example_data,
create_context, db_drop_table, remove_context, save_byom, delete_byom,
retrieve_byom, list_byom
from teradataml.options.configure import configure
```
b. Create the connection to database.
```python
con = create_context(host=getpass.getpass("Hostname: "),
username=getpass.getpass("Username: "),
password=getpass.getpass("Password: "))
```
c. Load example data.
```python
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
```
2. Create train dataset and test dataset.
a. Create two samples of input data.
    This step creates two samples of input data: sample 1 has 80% of total rows and sample 2 has 20%
    of total rows.
```python
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
```
b. Create train dataset.
    This step creates train dataset from sample 1 by filtering on "sampleid" and drop "sampleid" column
    as it is not required for training model.
```python
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid",
axis = 1)
iris_train
```
c. Create test dataset.
    This step creates test dataset from sample 2 by filtering on "sampleid" and drop "sampleid" column
    as it is not required for scoring.
```python
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid",
axis = 1)
iris_test
```
3. Train the Random Forest model and perform the Prediction using PMMLPredict().
a. Import required libraries.
```python
import numpy as np
from sklearn import tree
from nyoka import skl_to_pmml
from sklearn.pipeline import Pipeline
from sklearn_pandas import DataFrameMapper
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
```
b. Prepare dataset to create a Random Forest model.
```python
traid_pd = iris_train.to_pandas()
features = traid_pd.columns.drop('species')
target = 'species'
```
c. Generate the Random Forest model.
```python
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
rf_pipe_obj = Pipeline([
("mapping", DataFrameMapper([
(['sepal_length', 'sepal_width'], StandardScaler()) ,
(['petal_length', 'petal_width'], imputer)
])),
("rfc", RandomForestClassifier(n_estimators = 100))
])
rf_pipe_obj.fit(traid_pd[features], traid_pd[target])
```
d. Save the model in PMML format.
```python
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_rf_class_model.pmml"
skl_to_pmml(rf_pipe_obj, features, target, model_file_path)
```
e. Save the model in Vantage.
```python
save_byom("pmml_random_forest_iris", model_file_path, "byom_models")
```
f. List the model from Vantage.
```python
list_byom("byom_models")
```
g. Retrieve the model from Vantage.
```python
modeldata = retrieve_byom("pmml_random_forest_iris", "byom_models")
```
h. Score the test data using PMMLPredict function with the retrieved model.
```python
result = PMMLPredict(
modeldata = modeldata,
newdata = iris_test,
accumulate = ['id', 'sepal_length', 'petal_length'],
overwrite_cached_models = '*',
```
i. Print the equivalent SQL query and Score result.
```python
print(result.show_query())
result.result
```
4. Clean up.
```python
# Delete the model from table "byom_models", using the model
id 'pmml_random_forest_iris'.
delete_byom("pmml_random_forest_iris", "byom_models")
# Drop models table.
db_drop_table("byom_models")
# Drop input data tables.
db_drop_table("iris_input")
# One must run remove_context() to close the connection and garbage collect
internally generated objects.
remove_context()
```
### Use H2OPredict to Score using Externally Trained Models
This example uses the iris_input dataset and performs a prediction on each row of the input table using a
model previously trained in H2O and then loaded into the database.
1. Set up the environment.
a. Import required libraries.
```python
import tempfile
import getpass
from teradataml import H2OPredict, DataFrame, load_example_data,
create_context, db_drop_table, remove_context, save_byom, delete_byom,
retrieve_byom, list_byom
from teradataml.options.configure import configure
```
b. Create the connection to database.
```python
con = create_context(host=getpass.getpass("Hostname: "),
username=getpass.getpass("Username: "),
password=getpass.getpass("Password: "))
```
c. Load example data.
```python
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
```
2. Create train dataset and test dataset.
a. Create two samples of input data.
    This step creates two samples of input data: sample 1 has 80% of total rows and sample 2 has 20%
    of total rows.
```python
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
```
b. Create train dataset.
    This step creates train dataset from sample 1 by filtering on "sampleid" and dropping "sampleid"
    column as it is not required for training model.
```python
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid",
axis = 1)
iris_train
```
c. Create test dataset.
    This step creates test dataset from sample 2 by filtering on "sampleid" and dropping "sampleid"
    column as it is not required for scoring.
```python
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid",
axis = 1)
iris_test
```
3. Train the Gradient Boosting Machine model and perform the Prediction using H2OPredict().
a. Import required libraries.
```python
import h2o
from h2o.estimators import H2OGradientBoostingEstimator
```
b. Prepare dataset to create a Gradient Boosting Machine model.
    Converting teradataml DataFrame to pandas DataFrame, since H2OFrame accepts
    pandas DataFrame.
```python
h2o.init()
iris_train_pd = iris_train.to_pandas()
h2o_df = h2o.H2OFrame(iris_train_pd)
h2o_df
```
c. Train the Gradient Boosting Machine model.
    Add the code for training model.
```python
h2o_df["species"] = h2o_df["species"].asfactor()
predictors = h2o_df.columns
response = "species"
gbm_model = H2OGradientBoostingEstimator(nfolds=5, seed=1111,
keep_cross_validation_predictions = True)
gbm_model.train(x=predictors, y=response, training_frame=h2o_df)
```
d. Save the model to a file in MOJO format.
```python
temp_dir = tempfile.TemporaryDirectory()
model_file_path = gbm_model.save_mojo(path=f"{temp_dir.name}", force=True)
```
e. Save the model in Vantage.
```python
save_byom(model_id="h2o_gbm_iris",
model_file=model_file_path, table_name="byom_models")
```
f. List the model in Vantage.
```python
list_byom("byom_models")
```
g. Retrieve the model from Vantage.
```python
model=retrieve_byom("h2o_gbm_iris", "byom_models")
```
h. Set "configure.byom_install_location" to the database where BYOM functions are installed.
```python
configure.byom_install_location =
getpass.getpass("byom_install_location: ")
```
i. Score the test data using H2OPredict function with the retrieved model.
```python
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
```
j. Print the equivalent SQL query and Score result.
```python
print(result.show_query())
result.result
```
4. Clean up.
```python
# Delete the saved Model.
delete_byom("h2o_gbm_iris", table_name="byom_models")
# Drop models table.
db_drop_table("byom_models")
# Drop input data tables.
db_drop_table("iris_input")
# One must run remove_context() to close the connection and garbage collect
internally generated objects.
remove_context()
```
### Use ONNXPredict to Score using Externally Trained Models
This example uses the iris_input dataset and performs a prediction on each row of the input table using a
model trained in ONNX format and then loaded into database.
1. Set up the environment.
a. Import required libraries.
```python
import tempfile
import getpass
from teradataml import DataFrame, load_example_data,
create_context, db_drop_table, remove_context, save_byom, delete_byom,
retrieve_byom, list_byom
from teradataml.options.configure import configure
```
b. Create the connection to database.
```python
con = create_context(host=getpass.getpass("Hostname: "),
username=getpass.getpass("Username: "),
password=getpass.getpass("Password: "))
```
c. Load example data.
```python
load_example_data("byom", "iris_input")
iris_input = DataFrame("iris_input")
```
2. Create train dataset and test dataset.
a. Create two samples of input data.
    This step creates two samples of input data: sample 1 has 80% of total rows and sample 2 has 20%
    of total rows.
```python
iris_sample = iris_input.sample(frac=[0.8, 0.2])
iris_sample
```
b. Create train dataset.
    This step creates train dataset from sample 1 by filtering on "sampleid" and dropping "sampleid"
    column as it is not required for training model.
```python
iris_train = iris_sample[iris_sample.sampleid == "1"].drop("sampleid",
axis = 1)
iris_train
```
c. Create test dataset.
    This step creates test dataset from sample 2 by filtering on "sampleid" and dropping "sampleid"
    column as it is not required for scoring.
```python
iris_test = iris_sample[iris_sample.sampleid == "2"].drop("sampleid",
axis = 1)
iris_test
```
3. Train the Random Forest model and perform the Prediction using ONNXPredict().
a. Import required libraries.
```python
from teradataml import ONNXPredict
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
```
b. Prepare dataset for training Random Forest model.
    Convert teradataml dataframe to pandas dataframe.
```python
train_pd = iris_train.to_pandas()
features = train_pd.columns.drop('species')
target = 'species'
```
c. Generate Random Forest model.
```python
rf_pipe_obj = Pipeline([
('scaler', StandardScaler()),
("rf", RandomForestClassifier(max_depth=5))
])
```
d. Train the Random Forest model.
```python
rf_pipe_obj.fit(train_pd[features], train_pd[target])
```
e. Save the model in ONNX format.
    a. Create temporary file path to save the model.
```python
temp_dir = tempfile.TemporaryDirectory()
model_file_path = f"{temp_dir.name}/iris_db_rf_model.onnx"
```
    b. Convert and save the Random Forest model in ONNX format.
```python
onx = to_onnx(rf_pipe_obj, train_pd.iloc[:,:4].astype(np.float32))
with open(model_file_path, "wb") as f:
f.write(onx.SerializeToString())
```
f. Save the model in Vantage.
```python
save_byom("onnx_rf_iris", model_file_path, "byom_models")
```
g. List the model in Vantage.
```python
list_byom("byom_models")
```
h. Retrieve the model from table "byom_models", using the model id 'onnx_rf_iris'.
```python
modeldata = retrieve_byom("onnx_rf_iris", "byom_models")
```
i. Set "configure.byom_install_location" to the database where BYOM functions are installed.
```python
configure.byom_install_location =
getpass.getpass("byom_install_location: ")
```
j. Perform prediction using ONNXPredict() function and the ONNX model stored in Vantage.
```python
predict_output = ONNXPredict(
modeldata = modeldata,
newdata = iris_test,
accumulate = ['id',
'sepal_length', 'petal_length'],
overwrite_cached_models = '*',
model_output_fields = "output_label"
```
k. Print the equivalent SQL query and Score result.
```python
print(result.show_query())
result.result
```
4. Clean up.
a. Delete the model from table "byom_models".
```python
delete_byom("onnx_rf_iris", "byom_models")
```
b. Delete models table.
```python
db_drop_table("byom_models")
```
c. Drop input data table.
```python
db_drop_table("iris_input")
```
d. Remove context.
```python
remove_context()
```
### Use Open Analytics to Score using Externally Trained Models
### using Apply
This example uses Open Analytics to score using externally trained models using Apply.
Note:
This example works only on VantageCloud Lake.
1. Set up the environment.
a. Import required libraries.
```python
from teradataml import create_context, remove_context, list_base_envs,
list_user_envs, create_env, remove_env, get_env, DataFrame, copy_to_sql,
Apply, configure, read_csv, set_config_params
from teradataml.options.display import display
import pandas as pd, getpass, os
from collections import OrderedDict
from teradatasqlalchemy.types import BIGINT, VARCHAR, INTEGER, FLOAT
```
b. Set Authentication token and UES URL.
```python
set_config_params(ues_url=getpass.getpass("UES URL: "),
auth_token=getpass.getpass("JWT Token: "))
```
c. Create the connection.
```python
con = create_context(host=getpass.getpass("Hostname: "),
username=getpass.getpass("Username: "),
password=getpass.getpass("Password: "))
```
    Note:
    You can use the same JWT token instead of password to create a context. See
    create_context for more details.
2. Generate model.
a. Import required libraries.
```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression
```
b. Read the data from the scikit-learn package.
```python
iris = load_iris()
X, y = iris.data, iris.target
```
c. Train a model with Random Forests.
```python
X_train, X_test, y_train, y_test = train_test_split(X, y)
clr = RandomForestClassifier()
clr.fit(X_train, y_train)
```
d. Convert the model into ONNX format. Generate ONNX model file "rf_iris.onnx".
```python
from skl2onnx import convert_sklearn
from skl2onnx.common.data_types import FloatTensorType
initial_type = [('float_input', FloatTensorType([None, 4]))]
onx = convert_sklearn(clr, initial_types = initial_type)
with open("rf_iris.onnx", "wb") as f:
f.write(onx.SerializeToString())
print("RF model trained and saved in 'rf_iris.onnx'.")
```
3. Load test data into VantageCloud Lake and create teradataml dataframe for the input table.
```python
dfIn = pd.DataFrame(X_test, columns=["sepal_length", "sepal_width",
"petal_length", "petal_width"])
copy_to_sql(dfIn, table_name = 'onnx_test_table_dataset', if_exists
= 'replace')
onnx_test_data = DataFrame.from_table("onnx_test_table_dataset")
onnx_test_data.head(n=5)
```
4. Create a python file to score the model.
Create a file with the name 'sklearn_onnx_scoring.py' in local client with following code.
```python
# Train a model.
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
import pandas as pd
import csv
import sys
# Read input data from stdin into a dataframe.
_reader = csv.DictReader(sys.stdin.readlines(), fieldnames
= ["sepal_length","sepal_width","petal_length","petal_width"])
data=pd.DataFrame(_reader, columns
= ["sepal_length","sepal_width","petal_length","petal_width"])
# For AMPs that receive no data, exit the script instance gracefully.
if data.empty:
sys.exit()
iris = load_iris()
X, y = iris.data, iris.target
X_train, X_test, y_train, y_test = train_test_split(X, y)
clr = RandomForestClassifier()
clr.fit(X_train, y_train)
# Compute the prediction with ONNX Runtime
import onnxruntime as rt
import numpy
sess = rt.InferenceSession("rf_iris.onnx")
input_name = sess.get_inputs()[0].name
label_name = sess.get_outputs()[0].name
pred_onx = sess.run([label_name], {input_name:
data.values.astype(numpy.float32)})[0]
listToStr = ' '.join([str(elem) for elem in pred_onx])
print(listToStr)
```
5. Create Environment and install the corresponding files in the environment.
a. List the base Python environments.
```python
list_base_envs()
```
    Assume a new Python environment is needed.
b. Create a new Python user environment for Python 3.8.13.
    Function create_env() will return an object of 'UserEnv'.
```python
demo_env = create_env(env_name = 'oaf_usecase_2c_env',
base_env = 'python_3.8.13',
desc = 'OAF Demo Use Case 2c Environment')
```
c. Verify the new environment has been created.
```python
list_user_envs()
```
d. Install necessary Python add-ons synchronously, for ues by the script in the user environment
    using an object 'demo_env' of class "UserEnv".
```python
demo_env.install_lib(["skl2onnx", "sklearn", "onnxruntime", "pandas"])
```
e. Verify the Python libraries have been installed correctly.
```python
demo_env.libs
```
f. Install the model file and Python file to score the data inside VantageCloud Lake.
```python
demo_env.install_file(file_path = 'rf_iris.onnx', replace = True)
demo_env.install_file(file_path = 'sklearn_onnx_scoring.py', replace
= True)
```
g. Verify the files have been installed correctly.
```python
demo_env.files
```
6. Score the data inside VantageCloud Lake.
a. Use Apply to create an object for the Random Forest based prediction.
```python
applyRF_obj = Apply(data = onnx_test_data,
apply_command = 'python3 sklearn_onnx_scoring.py',
returns = {"Predicted_Class_RF": VARCHAR(200)},
env_name = demo_env
```
b. Run the Python script inside the remote user environment.
```python
applyRF_obj.execute_script()
```
    Note:
    You can display the underlying SQL by setting 'display.print_sqlmr_query = True'.
7. Remove the environment and disconnect from VantageCloud Lake.
a. After scoring the data, remove the environment.
```python
remove_env('oaf_usecase_2c_env')
```
b. Verify the specified environment has been removed.
```python
list_user_envs()
```
c. Disconnect from VantageCloud Lake.
```python
remove_context()
```