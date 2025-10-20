# teradataml Open-Source Machine Learning Functions

The Teradata Package for Python introduces teradataml open-source machine learning functions, referred
as teradataml OpenSourceML, which exposes most of the functionality of open-source packages like
scikit-learn, and so on. With teradataml open-source machine learning functions, you can run these
open-source packages without needing to pull the data to your client. It offers a simple interface object for
the open-source packages, allowing them to be used with the same syntax and arguments as the actual
open-source packages' functions and classes.
Functions/classes from open-source packages generates a single model that is trained on all the data. Unlike
traditional open-source packages, you can use teradataml OpenSourceML to generate distributed models,
also known as multiple models or micro models.
In combination with the MPP architecture that Vantage provides, teradataml OpenSourceML can tap,
process and solve a large set of use cases where distributed models are needed. To enable this support,
teradataml OpenSourceML introduces the  partition_columns  argument, which can be used in all functions;
partition_columns  accepts the column to be used to partition the data and generate the models for
partitioned data.
**Supported open-source packages:**
* scikit-learn
* lightGBM
**The following topics provide setup, module, and usage information:**
* Setup and Requirements
* Support for Model Operations
* td_sklearn
* td_lightgbm
* Limitations and Considerations
## Setup and Requirements
**teradataml open-source machine learning functions support the following Vantage platforms:**
* VantageCloud Enterprise
* VantageCloud Lake
Setup and requirement information common to both platforms follows.
### Use of Script and Apply Table Operators
When both table operators (Script and Apply) are available, you can execute feature functions using
**either operator:**
* For systems with Teradata in-nodes Python packages (STO), use the Script table operator:
11

```python
from teradataml import configure
configure.table_operator = "script"
```
* For systems with the Apply table operator:
```python
from teradataml import configure
configure.table_operator = "apply"
```
If no configuration option is specified, teradataml checks the database version. If the version is 20 or higher,
it will execute using the Apply table operator; otherwise, it will default to the Script table operator.
### Version requirement
teradataml OpenSourceML is supported when the Python interpreter major version in Vantage and the client
are same.
teradataml open-source machine learning functions support the classes, methods, and attributes of the
classes in the underlying open source machine learning package if the underlying package version in
Vantage and the version on the client are the same.
For example, in  scikit-learn , for the class  OneVsRestClassifier , an attribute  intercept_  is supported
on  scikit-learn  v0.24.2, whereas it is not supported on v1.1.3.
**So, when you run the following:**
```python
obj = OneVsRestClassifier(...)
obj.fit(X, y)
obj.intercept_
```
This works fine with  scikit-learn  v0.24.2, but fails on  scikit-learn  v1.1.3 with  AttributeError .
Same behavior will be seen with teradataml open-source machine learning functions.
### When Client is Connected to VantageCloud Enterprise
Prerequisites to run teradataml open-source machine learning functions when your local machine is
**connected to VantageCloud Enterprise:**
* Execution of the teradataml open-source machine learning functions depends on the Script
* table operator.
* The Script table operator setup is done on Vantage.
* Administrator provides the standard permissions required by Script table operator to the
* database user.
* Administrator sets the following standard permissions for the database <user>:
11: teradataml Open-Source Machine Learning Functions

```python
GRANT EXECUTE FUNCTION ON td_sysfnlib.script TO <user>;
GRANT CREATE EXTERNAL PROCEDURE ON <user> TO <user>;
GRANT DROP PROCEDURE ON <user> TO <user>;
GRANT EXECUTE PROCEDURE ON SYSUIF.INSTALL_FILE TO <user>;
GRANT EXECUTE PROCEDURE ON SYSUIF.REPLACE_FILE TO <user>;
GRANT EXECUTE PROCEDURE ON SYSUIF.REMOVE_FILE TO <user>;
GRANT EXECUTE ON SYSUIF.DEFAULT_AUTH TO <user>;
```
* Administrator sets the following permission for OpensourceML:
```python
GRANT CREATE EXTERNAL PROCEDURE ON SYSUIF TO <user>;
```
* If the default user database <userDB> is different from the <user> database, set the
* following permissions:
```python
GRANT CREATE EXTERNAL PROCEDURE ON <userDB> TO <user>;
GRANT DROP PROCEDURE ON <userDB> TO <user>;
GRANT EXECUTE FUNCTION ON td_sysfnlib.script TO <userDB> WITH GRANT OPTION;
GRANT EXECUTE ON SYSUIF.DEFAULT_AUTH TO <userDB> WITH GRANT OPTION;
```
* Version of the open-source package on your local machine should be the same as the one installed
* in Vantage as part of the Script table operator setup.
* You can use  db_python_package_details()  to check the version of installed packages.
If Teradata In-nodes Python Packages (STO) is installed on Vantage, there are additional requirements
based on different versions of the Teradata In-nodes Python Packages.
* For Teradata In-nodes Python Packages v1.0.x:
    * The Python executable is  /opt/teradata/languages/Python/ .
    * Set the following to make sure teradataml open-source machine learning functions could invoke
    * the interpreter.
```python
from teradataml import configure
configure.indb_install_location = "/opt/teradata/languages/Python/"
scikit-learn  version on your client is expected to be v0.24.2 as STO has scikit-learn
```
    * version 0.24.2.
* For Teradata In-nodes Python Packages v2.0.0:
    * The Python executable is present in the directory  /var/opt/teradata . teradataml open-source
    * machine learning functions use this location by default.
    * If Vantage raises "python interpreter file not found" exception, set the following to make sure
    * teradataml open-source machine learning functions could invoke the interpreter.
11: teradataml Open-Source Machine Learning Functions

```python
from teradataml import configure
configure.indb_install_location = "/var/opt/teradata/languages/
sles12sp3/Python/"
```
    * Upgrade or downgrade the  scikit-learn  library on your client to v1.1.3 to keep it similar to the
```python
scikit-learn  version on STO v2.0.0.
```
### When Client is Connected to VantageCloud Lake
Prerequisites to run teradataml open-source machine learning functions when your local machine is
**connected to VantageCloud Lake:**
* Execution of the teradataml open-source machine learning functions depends on APPLY
* table operator.
* Permissions are preconfigured during database installation.
* You can set up a user environment with required packages using teradataml Open Analytics APIs.
* Once the user environment is created, you can set the following to make sure teradataml open-source
* machine learning functions could use the environment.
```python
from teradataml import configure
confgiure.openml_user_env = <Object of UserEnv class>
```
* If no configuration option is provided by the user, teradataml open-source machine learning functions
* use the default  openml_env  user environment.
* This default environment has latest Python and scikit-learn versions that are supported by Open
* Analytics Framework at the time of creating environment.
* Version of the Python package installed must be same on both local system and VantageCloud Lake
* user environment.
## Support for Model Operations
teradataml OpenSourceML offers ways to deploy and load the models that are trained inside or outside
of Vantage.
**Unlike traditional open-source packages, teradataml OpenSourceML provides you the following options:**
* Deploy and save the trained model to a table. See  Model Deployment .
* This table (opensourceml_models) is internally created by teradataml OpenSourceML in connected
* database, and should not be deleted.
* Load the saved model from a Vantage table in Python as an object. See  Model Loader .
11: teradataml Open-Source Machine Learning Functions

### Model Deployment
The model deployment component saves and persists the model to the Vantage table so that you can use
it for prediction or scoring or other class methods that require model in other sessions.
This model can be a model trained either by teradataml OpenSourceML framework or outside of
teradataml OpenSourceML.
### Model Loader
The model loader component loads the model, based on the provided  model_name .
The loaded model can be used for any operation that requires model. For example: accessing attributes,
or running other class methods.
## td_sklearn
Currently, teradataml open-source machine learning module exposes scikit-learn package dynamically
through an interface object  td_sklearn . Current implementation exposes around 94 percent of classes and
around 91 percent of class methods supported by scikit-learn.
Use this module to execute any scikit-learn function with same syntax and arguments using the interface
**object. teradataml open-source machine learning functions can be used to achieve:**
* Load and deploy scikit-learn models (model generated by teradataml OpenSourceML as well as
* external models)
* Support classification and regression metrics
With  td_sklearn , you can easily run any scikit-learn function inside Vantage where data reside, that is,
without any data transfer, using the Massively Parallel Processing (MPP) capabilities. While doing so, you
do not have to worry about usage and function syntaxes. To ease the usage, teradataml  td_sklearn
**supports multiple syntaxes as follows:**
* Syntax 1: Using well known scikit-learn function syntax where arguments,  X  and  y  are passed.
* Syntax 2: Alternative to the legacy arguments  X  and  y , Teradata introduces another set of arguments
* data ,  feature_columns ,  label_columns ,  group_columns .
The following sections discuss about how to use teradataml's  td_sklearn  to run scikit-learn using different
syntaxes, generating classification and regression metrics, generating single model and distributed-model
(multi-model) support through  partition_columns  argument, additional support for load and deploy scikit-
learn models, the supportability information and limitations and considerations.
* Run scikit-learn Functions using Legacy Arguments
* Run scikit-learn Functions using Teradata Introduced Arguments
* Support for Distributed Modeling
* Support for Classification and Regression Metrics
* Support for Model Operations using td_sklearn
11: teradataml Open-Source Machine Learning Functions

* td_sklearn Supportability Matrix
**Note:**
The examples use only specific scikit-learn function, but the same logic is applicable for all other
scikit-learn functions.
### Run scikit-learn Functions using Legacy Arguments
If you are familiar with scikit-learn, you can use the data argument  X ,  y  and  groups  like the way you use
them in scikit-learn.
**One minor difference in the usage:**
* In scikit-learn, these arguments are pandas DataFrames, or numpy arrays, or list of lists, and so on.
* With td_sklearn, these arguments are teradataml DataFrames which are created from the same
* teradataml DataFrame using  select()  API.
* Note:
    * If there is only X argument, then it does not need to be derived using  select()  API.
### scikit-learn Example
* Generate data.
```python
X : {array-like, sparse matrix} of shape (n_samples, n_features)
y : array-like of shape (n_samples,)
from sklearn.datasets import make_classification
X, y = make_classification(n_features=4, random_state=0)
```
* Instantiate scikit-learn LinearSVC object.
```python
from sklearn.svm import LinearSVC
clf = LinearSVC(random_state=0, tol=1e-5)
clf
LinearSVC(random_state=0, tol=1e-05)
```
* Train the model.
```python
clf.fit(X=x, y=y)
LinearSVC(random_state=0, tol=1e-05)
```
11: teradataml Open-Source Machine Learning Functions

* Generate predictions on test data.
```python
clf.predict([[0, 0, 0, 0]])
[1]
```
* Access attributes.
```python
linear_svc.intercept_
array([0.55058172])
```
### teradataml Open-Source Machine Learning Functions Example
* Generate data.
```python
df_train = DataFrame("test_classification")
df_train
col1                    col2
col3                   col4    label
-1.1305820619922704     -0.0202959251414216   -0.7102336334648424
-1.4409910829920618        0
-0.2869200001717422     -0.7169529842687833   -0.9865850877151031
-0.848214734984639        0
-2.5604297516143286      0.4022323367243113   -1.1007419820939435
-2.9595882598466674        0
0.4223414
917685     -2.0391144030275625   -2.053215806414584
-0.8491230457662061        0
0.7216694959200303     -1.1215566442946217   -0.8318398647044646
0.1507420965953343        0
-0.9861325665504175      1.7105310292848412    1.3382818041204743
-0.0853410902974293        1
-0.5097927128625588      0.4926589443964751    0.2482067293662461
-0.3095907315896897        1
0.1833246820582146      -0.774610353732039    -0.766054694735782
-0.2936686329125327        0
-0.4032571038523639      2.0061840569850093    2.0275124771199318
0.8508919440196763        1
-0.0715602561938739      0.2295539000122874    0.21654344712218576
0.0652739792167357        1
feature_columns = ["col1", "col2", "col3", "col4"]
```
11: teradataml Open-Source Machine Learning Functions

```python
label_columns = "label"
```
* Note:
    * Input teradataml DataFrames must be created using select() on the same parent DataFrame.
```python
df_x_clasif = df.select(feature_columns)
df_y_clasif = df.select(label_columns)
```
* Create an instance of scikit-learn LinearSVC object through 'td_sklearn'.
```python
from teradataml import td_sklearn as osml
linear_svc = osml.LinearSVC(loss="hinge", tol=0.01)
linear_svc
LinearSVC(loss='hinge', tol=0.01)
```
* Train the model.
```python
linear_svc.fit(X=df_x_clasif, y=df_y_clasif)
LinearSVC(loss='hinge', tol=0.01)
```
* Get predictions on test data.
* Note:
    * Compared to the predicted values in previous scikit-learn example, teradataml OpenSourceML
    * returns teradataml DataFrame with both features and labels.
```python
linear_svc.predict(df_x_clasif)
col1                  col2
col3                 col4   linearsvc_predict_1
1.23195055037206     -1.53949525926716    -0.99510531686895
0.511600970144431                   0.0
1.26780439921386     -1.80170792990881    -1.27034986297172
0.379112827728592                   0.0
-0.869536951900537      1.99896877100815     1.73590334857413
0.257374908024379                   1.0
1.43370121321312     -1.75423983622451    -1.11573423222268
```
11: teradataml Open-Source Machine Learning Functions

```python
0.620716743476382                   0.0
-1.05286597780779    -0.641515112432539    -1.36672011108273
-1.76399738946526                   0.0
-0.345538051487565     -2.29672333669221    -2.81180710379968
-1.9931134219738                   0.0
-1.2573206891836     -2.14861012008993    -3.19826339415065
-3.04373306805433                   0.0
-0.205721671526727      1.75895320535307     1.86752027575658
0.932664558487293                   1.0
-3.58754622394712      0.29181935785016    -1.85016852734401
-4.33105451025007                   0.0
-2.52159550020822      2.47822554412282     1.27458363813847
-1.50328319686837                   1.0
```
* Access attributes.
```python
linear_svc.intercept_
array([0.55058172])
```
### Run scikit-learn Functions using Teradata
### Introduced Arguments
An alternative to the legacy scikit-learn arguments, teradataml OpenSourceML introduces new arguments
data ,  feature_columns ,  label_columns  and  group_columns  to run functions of scikit-learn classes. The
functionality is similar to what was described in the previous example, but avoids additional step of creating
X and y DataFrames from  select()  API.
**Note:**
Teradata recommends using  data ,  feature_columns ,  label_columns  and  group_columns  compared
to the legacy arguments of  X ,  y  and  groups , for its clear and simple usage.
### Description of Teradata Introduced Arguments
* data : teradataml DataFrame containing columns specified in the following three arguments
* feature_columns : Column name or list of column names (as str) containing features to be trained on
* or tested for
* label_columns : Column name or list of column names (as str) containing labels which are used
* for training
* group_columns : Column name or list of column names (as str) containing group columns needed for
* classes in model selection module
The column names provided in the  feature_columns ,  label_columns  and  group_columns  arguments
should be present in the teradataml DataFrame specified by  data .
11: teradataml Open-Source Machine Learning Functions

### Example using Teradata introduced arguments
* Generate data.
```python
df_train = DataFrame("test_classification")
df_train
col1                    col2
col3                   col4    label
-1.1305820619922704     -0.0202959251414216   -0.7102336334648424
-1.4
910829920618        0
-0.2869200001717422     -0.7169529842687833   -0.9865850877151031
-0.848214734984639        0
-2.5604297516143286      0.4022323367243113   -1.1007419820939435
-2.9595882598466674        0
0.4223414406917685     -2.0391144030275625    -2.053215806414584
-0.8491230457662061        0
0.7216694959200303     -1.1215566442946217   -0.8318398647044646
0.1507420965953343        0
-0.9861325665504175      1.7105310292848412    1.3382818041204743
-0.0853410902974293        1
-0.5097927128625588      0.4926589443964751    0.2482067293662461
-0.3095907315896897        1
0.1833246820582146      -0.774610353732039    -0.766054694735782
-0.2936686329125327        0
-0.4032571038523639      2.0061840569850093    2.0275124771199318
0.8508919440196763        1
-0.0715602561938739      0.2295539000122874   0.21654344712218576
0.0652739792167357        1
feature_columns = ["col1", "col2", "col3", "col4"]
label_columns = "label"
```
* Create an instance of scikit-learn LinearSVC object through teradataml open-source machine learning
* function 'td_sklearn'.
```python
from teradataml import td_sklearn as osml
linear_svc = osml.LinearSVC(loss="hinge", tol=0.01)
linear_svc
```
11: teradataml Open-Source Machine Learning Functions

```python
LinearSVC(loss='hinge', tol=0.01)
```
* Train the model.
```python
linear_svc.fit(data=df_train,
feature_columns=feature_columns, label_columns=label_columns)
LinearSVC(loss='hinge', tol=0.01)
```
* Get predictions on test data.
* teradataml OpenSourceML returns teradataml DataFrame with both features and labels for predict
* and similar functions.
```python
linear_svc.predict(data=df_train, feature_columns=feature_columns)
col1                  col2
col3                 col4    linearsvc_predict_1
1.23195055037206     -1.53949525926716    -0.99510531686895
0.511600970144431                    0.0
1.26780439921386     -1.80170792990881    -1.27034986297172
0.379112827728592                    0.0
-0.869536951900537      1.99896877100815     1.73590334857413
0.257374908024379                    1.0
1.43370121321312     -1.75423983622451    -1.11573423222268
0.620716743476382                    0.0
-1.05286597780779    -0.641515112432539    -1.36672011108273
-1.76399738946526                    0.0
-0.345538051487565     -2.29672333669221    -2.81180710379968
-1.9931134219738                    0.0
-1.2573206891836     -2.14861012008993    -3.19826339415065
-3.04373306805433                    0.0
-0.205721671526727      1.75895320535307     1.86752027575658
0.932664558487293                    1.0
-3.58754622394712      0.29181935785016    -1.85016852734401
-4.33105451025007                    0.0
-2.52159550020822      2.47822554412282     1.27458363813847
-1.50328319686837                    1.0
```
* Access attributes.
```python
linear_svc.intercept_
array([0.55058172])
```
11: teradataml Open-Source Machine Learning Functions

### Support for Distributed Modeling
While traditional scikit-learn functions generates single model,that is, model is trained on all the data;
teradataml OpenSourceML allow you to generate distributed models, also known as multiple models, or
micro models.
With the MPP architecture that Vantage provides, teradataml OpenSourceML can tap, process and solve
large set of use cases where distributed models are needed.
To enable this support, teradataml OpenSourceML introduces another argument  partition_columns .
This argument is applicable for all functions, thus can be used with any SkLearn function.
The  partition_columns  accepts the name of the columns that are used to partition, and functions generate
**model for each unique partition. These columns should be present:**
* In teradataml DataFrame  X , when legacy arguments are used;
* In teradataml DataFrame  data , when Teradata introduced arguments are used.
If data related to  partition_columns  argument is not passed either in  X  or  data  argument, Teradata raises
an exception.
**Note:**
When fit() method generates distributed models based on unique partition, you may or may not
provide  partition_columns  in predict() or other functions, as teradataml OpenSourceML internally
picks  partition_columns  from trained model if this argument is not provided.
### Example Setup
* Generate data.
```python
df_train = DataFrame("multi_model_classification")
df_train
col1                    col2
col3                    col4    label  group_column
partition_column_1    partition_column_2
-1.9087921658084848      -1.160262700727636    -0.2736454485734128
-0.8276602780534795        1            10
0                    10
-1.1704705376390987    0.022123819493562014    -2.1737679735754902
-0.13421975547018156        0            11
1                    11
0.7901003669294754      0.6853062352887638   -0.44740487360308157
0.4469295901427309        0            12
```
11: teradataml Open-Source Machine Learning Functions

```python
1                    10
1.686169889935727      1.6329131018946743    -1.4207265350272436
1.040505566804641        0            11
0                    11
-1.2426815806615432     -1.1471527921467466     0.8931618813249708
-0.7381982270343821        1             9
1                    10
0.426749100345748     0.05289280597364859     0.6258591691181341
0.07995591661425976        1             8
1                    11
-0.9391289258328815     -1.0227083782811874      1.100938269732546
-0.6371443231582048        1             9
1                    10
-0.7769469454662005      0.3143429885076965     -2.262318506243238
0.06339125339933988        0            11
0                    10
1.1494603494659446      0.6225459371796891      0.373029393994218
0.45965795
125965        0            11
0                    10
-0.7724413877578084     0.36075760239525223     -2.381101325504745
0.08756999856023923        0             9
1                    11
feature_columns = ["col1", "col2", "col3", "col4"]
label_columns = "label"
part_cols = ["partition_column_1", "partition_column_2"]
```
* Create sklearn object.
```python
from teradataml import td_sklearn as osml
kmeans = osml.KMeans(n_clusters=4, algorithm="elkan", init="random")
kmeans.set_params(n_clusters=6, tol=0.1)
```
### Example 1: Partition columns passed for both fit() and predict()
* Pass partition columns for fit().
* Case 1: Using legacy arguments: in the output of fit(), there are 4 different models for 4 different unique
* partition values.
11: teradataml Open-Source Machine Learning Functions

```python
kmeans.fit(X=df_train.select(feature_columns +
part_cols), partition_columns=part_cols)
partition_column_1
partition_column_2
model
0                   1                  11  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
1                   0                  11  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
2                   1                  10  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
3                   0                  10  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
```
* Case 2: Using Teradata introduced arguments: same in this case, output with 4 different models for 4
* different unique partition values.
```python
kmeans.fit(data=df_train, feature_columns=feature_columns,
label_columns=label_columns, partition_columns=part_cols)
partition_column_1
partition_column_2
model
0                   1                  11  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
1                   0                  11  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
2                   1                  10  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
3                   0                  10  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
```
* Pass partition columns for predict().
* predict() returns output from the model generated on that particular partition.
```python
kmeans.predict(X=df_train.select(feature_columns +
part_cols), partition_columns=part_cols)
partition_column_1    partition_column_2
```
11: teradataml Open-Source Machine Learning Functions

```python
col1                  col2                 col3
col4     kmeans_predict_1
1                    10
3.03238599297047      3.03481436440056    -2.82355004664948
1.92120455311016                    3
1                    10
1.01738017000636      1.33916140701086    -1.82497543774135
0.807901883197634                    3
1                    10
1.31804370433225     0.178246937722197     1.89230973992192
0.254523189858355                    2
1                    10
-0.651461127752867     -0.63774973705703    0.567676564082086
-0.405498305447537                    4
1                    10
-1.17935408605987   -0.0635371417057811    -1.95557314394007
-0.178913714086315                    0
1                    10
-0.428384248092839    -0.234809682957427   -0.131372278595596
-0.172730190203367                    1
0                    10
-0.919743003749025    -0.202104661295527    -1.10794482877913
-0.217158744843899                    1
0                    10
-1.08210379782304     -1.03594281152977    0.878987325514909
-0.661649203133496                    3
0                    10
-1.1228679082477     -0.30415325948158    -1.19563980905683
-0.29433404813862                    1
0                    10    -
1.60877127718123    -0.986713721609351   -0.206518667132905
-0.702057767233714                    0
```
### Example 2: Partition columns passed for fit(), but not for predict()
* predict() returns similar output because partition columns are taken from fit() and the DataFrame 'X'
* has both feature_columns and partition columns.
```python
kmeans.predict(X=df_train.select(feature_columns + part_cols))
partition_column_1    partition_column_2
col1                  col2                 col3
col4     kmeans_predict_1
```
11: teradataml Open-Source Machine Learning Functions

```python
1                    10
3.03238599297047      3.03481436440056    -2.82355004664948
1.92120455311016                    3
1                    10
1.01738017000636      1.33916140701086    -1.82497543774135
0.807901883197634                    3
1                    10
1.31804370433225     0.178246937722197     1.89230973992192
0.254523189858355                    2
1                    10
-0.651461127752867     -0.63774973705703    0.567676564082086
-0.405498305447537                    4
1                    10
-1.17935408605987   -0.0635371417057811    -1.95557314394007
-0.178913714086315                    0
1                    10
-0.428384248092839    -0.234809682957427   -0.131372278595596
-0.172730190203367                    1
0                    10
-0.919743003749025    -0.202104661295527    -1.10794482877913
-0.217158744843899                    1
0                    10
-1.08210379782304     -1.03594281152977    0.878987325514909
-0.661649203133496                    3
0                    10
-1.1228679082477     -0.30
325948158    -1.19563980905683
-0.29433404813862                    1
0                    10    -
1.60877127718123    -0.986713721609351   -0.206518667132905
-0.702057767233714                    0
```
* If DataFrame `X` does not contain partition columns as trained in fit, teradataml OpenSourceML
* raises exception.
```python
kmeans.predict(df_train.select(feature_columns))
[Teradata][teradataml](TDML_2536) Model is fitted using 'partition_columns'
but 'partition_columns' are not passed.
Partition columns should be same as fit()'s 'partition_columns' or pass
'partition_columns' to the function. In either case they should be present
in 'X' DataFrame.
```
11: teradataml Open-Source Machine Learning Functions

### Support for Classification and Regression Metrics
teradataml open-source machine learning supports classification and regression functions in metrics
module for both single model and distributed model cases. Support for other functions will be added in
upcoming releases. See the supportability matrix for more details.
For classification and regression metrics,  scikit-learn  requires  y_true  (true labels) and  y_pred
(predicted labels), and can be in two different datasets (numpy arrays, or pandas DataFrames, and
so on).
teradataml OpenSourceML's support for classification and regression metrics requires these two be in the
same teradataml DataFrame. The values that you pass to  y_true  and  y_pred  should be from the same
parent teradataml DataFrame, using select() API.
To have both arguments in the same teradataml DataFrame, Teradata introduced an argument to include
labels along with predicted values in  predict  or  decision_function  by allowing the user to pass the
argument  y  along with  X  (Or  label_columns  along with  feature_columns  in additional arguments support).
**Note:**
y  is not needed along with  X  for  predict  or  decision_function  when running through actual
```python
scikit-lear n.
```
### Example
* Import the module.
```python
from teradataml import td_sklearn as osml
```
* Create sklearn object.
```python
linear_svc = osml.LinearSVC(loss="hinge", tol=0.001, C=0.5)
linear_svc
```
* Fit the data.
```python
linear_svc.fit(df_x_clasif, df_y_clasif)
LinearSVC(C=0.5, loss='hinge', tol=0.001)
LinearSVC(C=0.5, loss='hinge', tol=0.001)
```
* Generate prediction.
* If using legacy arguments:
11: teradataml Open-Source Machine Learning Functions

* Note:
    * The argument  y  is also passed to the predict() function.
```python
opt = linear_svc.predict(df_x_clasif, df_y_clasif)
opt
col1                  col2
col3                   col4    label    linearsvc_predict_1
-0.986132566550
1.71053102928484      1.33828180412047
-0.0853410902974293        1                      1
0.393906075573426      0.39024734554033      0.68152067710274
0.761804327884656        1                      1
2.03196825439669     0.840398654629947      2.18718139755992
3.13482383282103        1                      1
0.0458931008257339    -0.261403391676136    -0.268225264366388
-0.11926611814336        1                      1
-1.30819171461515     -0.43265955878063     -1.28532882978911
-1.94473774435271        0                      0
0.55626974301926    -0.584264226130486    -0.323726921967906
0.306165066460928        1                      1
-1.20114434929225     0.117241060941286    -0.597321844774845
-1.43683401202797        0                      0
-0.072316803817605     -0.77366833358392    -0.920383252944682
-0.615748703538104        0                      0
0.72166949592003     -1.12155664429462    -0.831839864704465
0.150742096595334        0                      0
-2.56042975161433     0.402232336724311     -1.10074198209394
-2.95958825984667        0                      0
```
* If using Teradata introduced arguments:
* Note:
    * The argument  label_columns  is also passed to predict() along with argument  feature_columns .
```python
opt = linear_svc.predict(data=df_train,
feature_columns=feature_columns, label_columns=label_columns)
opt
```
11: teradataml Open-Source Machine Learning Functions

```python
col1                  col2
col3                   col4    label    linearsvc_predict_1
-0.986132566550417      1.71053102928484      1.33828180412047
-0.0853410902974293        1                      1
0.393906075573426      0.39024734554033      0.68152067710274
0.761804327884656        1                      1
2.03196825439669     0.840398654629947      2.18718139755992
3.13482383282103        1                      1
0.0458931008257339    -0.261403391676136    -0.268225264366388
-0.11926611814336        1                      1
-1.30819171461515     -0.43265955878063     -1.28532882978911
-1.94473774435271        0                      0
0.55626974301926    -0.584264226130486    -0.323726921967906
0.306165066460928        1                      1
-1.20114434929225     0.117241060941286    -0.597321844774845
-1.43683401202797        0                      0
-0.072316803817605     -0.77366833358392    -0.920383252944682
-0.615748703538104        0                      0
0.72166949592003     -1.12155664429462    -0.831839864704465
0.150742096595334        0                      0
-2.56042975161433     0.402232336724311     -1.10074198209394
-2.95958825984667        0                      0
```
* Create  y_true  and  y_pred  teradataml DataFrames that are both from select() API on the same
* teradataml DataFrame.
```python
y_true_df = opt.select(["label"])
y_pred_df = opt.select("linearsvc_predict_1")
```
* Run classification_report() function from classification metrics.
```python
opt = osml.classification_report(y_true=y_true_df,
y_pred=y_pred_df, digits=4)
print(opt)
precision    recall  f1-score   support
0     0.9231    0.9600    0.9412        50
1     0.9583    0.9200    0.9388        50
accuracy                         0.9400       100
```
11: teradataml Open-Source Machine Learning Functions

```python
macro avg     0.9407    0.9400    0.9400       100
weighted avg     0.9407    0.9400    0.9400       100
```
* Run ridge regression.
```python
df_reg = DataFrame("train_regression")
df_reg.head(5)
col1                   col2
col3                  col4    label
-1.8430695501566485    -0.4779740040404867    -0.47965581400794766
0.6203582983435125      -31
-1.6981058194322545     0.3872804753950634     -2.2555642294021894
-1.0225068436356035     -263
-1.7558905834377194    0.45093446180591484     -0.6840108977372166
1.6595507961898721       79
-1.936279805846507    0.18877859679382855      0.5238910238342056
0.08842208704466141        9
-2.5529898158340787     0.6536185954403606      0.8644361988595057
-0.7421650204064
-36
```
* Get data needed for Ridge Regression fitting, for cells df_x_reg and df_y_reg.
```python
df_x_reg = df_reg.select(feature_columns)
LinearSVC(C=0.5, loss='hinge', tol=0.001)>>> df_y_reg
= df_reg.select(label_columns)
```
* Create Ridge object.
```python
ridge = osml.Ridge(max_iter=400, alpha=3)
```
* Fit the data.
```python
ridge.fit(X=df_x_reg, y=df_y_reg)
```
* Predict the output.
```python
opt_r = ridge.predict(X=df_x_reg, y=df_y_reg)
opt_r
col1                  col2
col3                 col4    label    ridge_predict_1
1.49407907315761    -0.205158263765801    0.313067701650901
```
11: teradataml Open-Source Machine Learning Functions

```python
-0.85409573930172      -30                -28
1.88315069705625     -1.34775906114245    -1.27048499848573
0.969396708158011       -8                 -7
-0.039282818227956      -1.1680934977412    0.523276660531754
-0.17154633122224      -20                -20
-0.672460447775951    -0.359553161540541    -0.81314628204445
-1.72628260233168     -232               -225
-0.887785747630113     -1.98079646822393    -0.34791214932615
0.15634896910398      -95                -93
2.38314477486394     0.944479486990414    -0.91282222544415
1.11701628809585      117                115
-0.907298364383242     0.051945395796139    0.729090562177537
0.128982910757411       43                 41
0.786327962108976    -0.466419096735943    -0.94444625591825
-0.41004969320254      -99                -96
0.28634368889228     0.608843834475451    -1.04525336614695
1.2111452896827       62                 61
1.53277921435846      1.46935876990029    0.154947425696916
0.378162519602174      125                122
```
* Get data for running mean_squared_error() function.
```python
y_true_reg = opt_r.select("label")
y_pred_reg = opt_r.select("ridge_predict_1")
```
* Run mean_squared_error() regression metrics function.
```python
opt = osml.mean_squared_error(y_true=y_true_reg,
y_pred=y_pred_reg, squared=False)
opt
3.8249182997810554
```
* Distributed model case for classification functions.
```python
df = DataFrame("metrics_data_table")
df.head(5)
y_true    y_pred       sample_weights
partition_column_1    partition_column_2
0         1    4.939110860968824
0                    10
```
11: teradataml Open-Source Machine Learning Functions

```python
0         1     3.93185833024791
0                    10
0         0    3.710826071080717
1                    10
0         0    4.897359655357496
0                    11
0         1    4.939374388961727
0                    10
```
* Note:
    * In the following command,  partition_columns  argument is added to run this function in distributed
    * model case.
    * However, these columns data need not be present in any of the teradataml DataFrames, but
    * should be present in parent DataFrame from which  y_true ,  y_pred  and  sample_weight  is derived
    * using select() API.
```python
opt = osml.accuracy_score(y_true = df.select(["y_true"]), y_pred
= df.select("y_pred"),
sample_weight =
df.select("sample_weights"), normalize=False,
partition_columns=["partition_column_1", "partition_column_2"])
opt
partition_column_1    partition_column_2         accuracy_score
1                    11    [4922.452566554788]
0                    11     [5021.77976372099]
1                    10    [4998.349672611376]
0                    10    [4996.775947412637]
```
### Support for Model Operations using td_sklearn
**See the following examples of deploying and loading models using td_sklearn:**
* Example: Deploy and Load Distributed Model
* Example: Deploy and Load Single Model
* Example: Deploy and Load Model Trained outside Vantage
11: teradataml Open-Source Machine Learning Functions

Example: Deploy and Load Distributed Model
1.
* Train the model.
* a.
    * Import the module.
```python
from teradataml import td_sklearn as osml
```
* b.
    * Create an object.
```python
kmeans = osml.KMeans(n_clusters=4, algorithm="elkan", init="random")
kmeans.set_params(n_clusters=6, tol=0.1)
kmeans.fit(X=df_x_clasif, partition_columns=part_cols)
partition_column_1
partition_column_2
model
0                   1                  11  KMeans(algorithm='elkan',
init='random', n_clusters=6, tol=0.1)
1                   0                  11  KMeans(algorithm='elkan',
init='random', n_clusters=6, tol=0.1)
2                   1                  10  KMeans(algorithm='elkan',
init='random', n_clusters=6, tol=0.1)
3                   0                  10  KMeans(algorithm='elkan',
init='random', n_clusters=6, tol=0.1)
```
2.
* Deploy the model.
```python
kmeans.deploy(model_name="test_kmeans", replace_if_exists=True)
Model is deleted.
Model is saved.
partition_column_1
partition_column_2
model
0                   1                  11  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
1                   0                  11  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
2                   1                  10  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
```
11: teradataml Open-Source Machine Learning Functions

```python
3                   0                  10  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
```
* Note:
* This step replace the model if a model with the same name already exists.
```python
try:
kmeans.deploy(model_name="test_kmeans")
except Exception as ex:
print(str(ex))
[Teradata][teradataml](TDML_2200) Model with name 'test_kmeans'
already exists.
```
3.
* Load the model back in Python session to be used further for prediction.
```python
loaded = osml.load(model_name="test_kmeans")
loaded
partition_column_1
partition_column_2
model
0                   1                  11  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
1                   0                  11  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
2                   1                  10  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
3                   0                  10  KMeans(algorithm='elkan', init='random',
n_clusters=6, tol=0.1)
```
4.
* Predict on loaded model.
* Predict on loaded model with  partition_columns  argument.
```python
loaded.predict(df_x_clasif, partition_columns=part_cols)
partition_column_1    partition_column_2
col1                 col2                 col3
col4     kmeans_predict_1
1                    10
3.03238599297047     3.03481436440056    -2.82355004664948
1.92120455311016                    3
1                    10
```
11: teradataml Open-Source Machine Learning Functions

```python
1.01738017000636     1.33916140701086    -1.82497543774135
0.807901883197634                    3
1                    10
1.31804370433225    0.178246937722197     1.89230973992192
0.254523189858355                    1
1                    10
-0.651461127752867    -0.63774973705703    0.567676564082086
-0.40549830544753                    0
1                    10
-1.17935408605987    -0.06353714170578    -1.95557314394007
-0.17891371408631                    4
1                    10
-0.42838
8092839    -0.23480968295742    -0.13137227859559
-0.17273019020336                    0
1                    10
-0.695735879594567    -1.07084898019651     1.67201893676997
-0.63139004513942                    5
1                    10
-1.23995029412835    -1.07952124763549    0.713159403625714
-0.70344346319249                    0
1                    10
2.48384809580349     2.37973754066001    -2.02266697958209
1.51968143653911                    3
1                    10
1.55458501178875     1.85365285731526    -2.26189922281813
1.1364775082343                    3
```
* Predict on loaded model without  partition_columns .
* df_x_clasif  contains both feature columns, and partition columns data picked from fitted model.
```python
loaded.predict(df_x_clasif)
partition_column_1    partition_column_2
col1                 col2                 col3
col4     kmeans_predict_1
1                    10
3.03238599297047     3.03481436440056    -2.82355004664948
1.92120455311016                    3
1                    10
1.01738017000636     1.33916140701086    -1.82497543774135
0.807901883197634                    3
1                    10
1.31804370433225    0.178246937722197     1.89230973992192
0.254523189858355                    1
```
11: teradataml Open-Source Machine Learning Functions

```python
1                    10
-0.651461127752867    -0.63774973705703    0.567676564082086
-0.40549830544753                    0
1                    10
-1.17935408605987    -0.06353714170578    -1.95557314394007
-0.17891371408631                    4
1                    10
-0.428384248092839    -0.23480968295742    -0.13137227859559
-0.17273019020336                    0
1                    10
-0.695735879594567    -1.07084898019651     1.67201893676997
-0.63139004513942                    5
1                    10
-1.23995029412835    -1.07952124763549    0.713159403625714
-0.70344346319249                    0
1                    10
2.48384809580349     2.37973754066001    -2.02266697958209
1.51968143653911                    3
1                    10
1.55458501178875     1.85365285731526    -2.26189922281813
1.1364775082343                    3
```
Example: Deploy and Load Single Model
1.
* Train the model.
* a.
    * Import the module.
```python
from teradataml import td_sklearn as osml
```
* b.
    * Create an object.
```python
kmeans1 = osml.KMeans(n_clusters=4, algorithm="elkan", init="random")
kmeans1.set_params(n_clusters=6, tol=0.1)
kmeans1.fit(X=df_train.select(feature_columns))
KMeans(algorithm='elkan', init='random', n_clusters=6, tol=0.1)
```
2.
* Deploy the model.
```python
kmeans1.deploy(model_name="test_kmeans_no_partition",
replace_if_exists=True)
```
11: teradataml Open-Source Machine Learning Functions

```python
Model is deleted.
Model is saved.
KMeans(algorithm='elkan', init='random', n_clusters=6, tol=0.1)
```
3.
* Load the model.
```python
loaded1 = osml.load(model_name="test_kmeans_no_partition")
loaded1
KMeans(algorithm='elkan', init='random', n_clusters=6, tol=0.1)
```
4.
* Predict on loaded model.
```python
loaded1.predict(df_train.select(feature_columns))
col1                 col2
col3                 col4     kmeans_predict_1
3.03238599297047     3.03481436440056    -2.82355004664948
1.92120455311016                    3
1.01738017000636     1.33916140701086    -1.82497543774135
0.807901883197634                    3
1.31804370433225    0.178246937722197     1.89230973992192
0.254523189858355                    1
-0.651461127752867    -0.63774973705703    0.567676564082086
-0.40549830544753                    0
-1.17935408605987    -0.06353714170578    -1.95557314394007
-0.17891371408631                    4
-0.428384248092839    -0.23480968295742    -0.13137227859559
-0.17273019020336                    0
-0.695735879594567    -1.07084898019651     1.67201893676997
-0.63139004513942                    5
-1.23995029412835    -1.07952124763549    0.713159403625714
-0.70344346319249                    0
2.48384809580349     2.37973754066001    -2.02266697958209
1.51968143653911                    3
1.55458501178875     1.85365285731526    -2.26189922281813
1.1364775082343                    3
```
Example: Deploy and Load Model Trained outside Vantage
1.
* Train scikit-learn model.
11: teradataml Open-Source Machine Learning Functions

```python
import numpy as np
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
X = np.array([[-1, -1], [-2, -1], [1, 1], [2, 1]])
y = np.array([1, 1, 2, 2])
from sklearn.svm import SVC
clf = make_pipeline(StandardScaler(), SVC(gamma='auto'))
clf.fit(X, y)
Pipeline(steps=[('standardscaler', StandardScaler()),
('svc', SVC(gamma='auto'))])
```
2.
* Deploy model trained outside Vantage.
```python
Import the module.
from teradataml import td_sklearn as osml
svc_obj = osml.deploy(model_name="svc_outside_tdml",
model=clf, replace_if_exists=True)
Model is deleted.
Model is saved.
```
3.
* Load the model that is deployed from outside Vantage.
```python
loaded2 = osml.load(model_name="svc_outside_tdml")
loaded2
Pipeline(steps=[('standardscaler', StandardScaler()),
('svc', SVC(gamma='auto'))])
```
4.
* Predict on loaded model.
```python
loaded2.predict(df_x_clasif.select(["col1", "col2"]))
col1                 col2    pipeline_predict_1
0.426749100345748    0.052892805973648                     2
0.061358610926176    -0.32391973812397                     1
1.06516942678559     1.37093616727654                     2
1.34487963860504    0.006631689510300                     2
```
11: teradataml Open-Source Machine Learning Functions

```python
1.13904878664119     1.05078416796634                     2
0.615820493677217    0.351753443920848                     2
-0.58236154328365    -0.95815122115123                     1
1.36289521790798    0.273506135473117                     2
-1.24268158066154    -1.14715279214675                     1
0.790100366929475    0.685306235288764                     2
```
### td_sklearn Supportability Matrix
Download the  td_sklearn Supportability Matrix.pdf  from the  attachment   in the left pane. This
document provides the supportability matrix for each function in all the modules that are supported
in scikit-learn.
## td_lightgbm
teradataml OpenSourceML exposes lightGBM package through an interface object  td_lightgbm . Use this
interface object to execute all supported lightGBM’s functions with the same syntax and arguments without
pulling the data to client using MPP capabilities.
teradataml OpenSourceML’s td_ lightgbm  trains and scores models in both single model approach and
distributed/multi-model approach. However, there are few things to note when working with lightGBM in
**distributed model training:**
* teradataml OpenSourceML has introduced an argument  partition_columns  that can be used with any
* lightGBM function.
* partition_columns  argument accepts the names of the columns used for partitioning.
* Generates model for each unique partition.
* Column names specified should be present in the parent teradataml DataFrame from which input
* teradataml DataFrames are derived.
* If parent DataFrame does not contain the columns, then teradataml raises an exception.
* When distributed models are generated per unique partition by  fit()  or  train()  methods, you may
* or may not provide  partition_columns  in  predict  or other functions as teradataml OpenSourceML
* internally picks  partition_columns  from trained model if this argument is not provided.
The following sections detail how to use teradataml’s  td_lightgbm  to run supported lightGBM functions -
```python
Dataset ,  Booster ,  train ,  cv , and all scikit-learn functions - to generate single model and distributed-model
```
(multi-model) through  partition_columns  argument and supportability information.
* Execute lightGBM functions using standard supported arguments
* Using Booster for prediction
* Running scikit-learn API
* Support for Model Operations using td_lightgbm
* td_lightgbm Supportability Matrix
* td_lightgbm Usage Notes
11: teradataml Open-Source Machine Learning Functions

### Execute lightGBM functions using standard
### supported arguments
### Using training APIs for training
To run training APIs  train  and  cv , create a Dataset object then pass the object as input to these functions.
The following subsections cover both single model and multi model training and highlight the differences
between training with actual lightGBM and  td_lightgbm .
Whether it is single-model training or multi-model training, you must first create the required teradataml
DataFrames. These DataFrames should be created from the same parent teradataml DataFrame using the
```python
select()  API.
Load the example dataset.
load_example_data("openml", ["multi_model_classification",
"multi_model_regression"])
Classification data.
Create DataFrames.
df_train_classif = DataFrame('multi_model_classification')
df_train_classif
col1
col2
col3
col4  label
group_column
partition_column_1
partition_column_2
-2.86976401
-1.45650077  -0.096175758
-1.831834743
0
11
1                 11
-2.69651383
-1.244829178  -0.110505514
-1.670524168
0
9
0                 10
0.939206169
-0.618203487   0.209642108
0.150728526
0
11
0                 10
1.413963995
0.329342266   0.110572078
0.743405742
0
10
0                 11
0.599510805
1.298906549  -0.141761534
0.790378168
1
8
1                 10
-1.17273888
0.05006965  -0.144305532
-0.484090357
1
12
1                 11
1.087219107
-1.006162386   0.289957882
0.055393647
0
9
1                 10
0.669401117
0.965276402  -0.079356706
0.683697322
1
8
1                 10
1.441799942
-0.007863088    0.16867607
0.617164007
0
9
1                 10
1.619580063
0.479070866   0.110079835
0.893252734
1
8
0                 10
```
11: teradataml Open-Source Machine Learning Functions

```python
Create required classification DataFrames.
df_x_classif = df_train_classif.select(["col1", "col2", "col3", "col4"])
df_y_classif = df_train_classif.select("label")
Regression data.
Create DataFrames.
df_train_reg = DataFrame('multi_model_regression')
df_train_reg.head(4)
col1
col2
col3
col4  label
group_column
partition_column_1
partition_column_2
-2.86976401
-1.45650077  -0.096175758
-1.831834743   -141
11
1                 11
-2.69651383
-1.244829178  -0.110505514
-1.670524168    -89
9
0                 10
0.939206169
-0.618203487   0.209642108
0.150728526   -137
11
0                 10
1.413963995
0.329342266   0.110572078
0.743405742
8
10
0                 11
Create required regression DataFrames.
df_x_reg = df_train_reg.select(["col1", "col2", "col3", "col4"])
df_y_reg = df_train_reg.select("label")
```
**See the following model examples:**
* Single model training
* Single model cross validation
* Multi model training
* Multi model cross validation
Single model training
Once the required teradataml DataFrames are created, you need to create Dataset objects.
```python
obj_s1 = td_lightgbm.Dataset(df_x_classif, df_y_classif,
silent=True, free_raw_data=False)
obj_s1
<lightgbm.basic.Dataset object at 0x7f5553c28c40>
```
After creating the Dataset object, run training function with  record_evaluation  and  early_
```python
stopping  callbacks:
```
11: teradataml Open-Source Machine Learning Functions

```python
rec = {}To pass this empty dictionary to record_evaluation callback.
Training With valid_sets and callbacks argument.
opt_tr_s = td_lightgbm.train(params={}, train_set=obj_s1,
num_boost_round=30,
callbacks=[td_lightgbm.record_evaluation(rec),
td_lightgbm.early_stopping(3)],
valid_sets=[obj_s1])
opt_tr_s
[LightGBM] [Warning] Auto-choosing col-wise multi-threading, the overhead of
testing was 0.000065 seconds.
You can set `force_col_wise=true` to remove the overhead.
[LightGBM] [Info] Total Bins 532
[LightGBM] [Info] Number of data points in the train set: 400, number of used
features: 4
Training until validation scores don't improve for 3 rounds
Did not meet early stopping. Best iteration is:
[30]
valid_0's l2: 0.0416953
<lightgbm.basic.Booster object at 0x7f5553d235b0>
```
Similar to the  train()  function of  lightgbm , OpensourceML’s lightGBM also displays console output
and returns Booster object. However, this Booster object is not lightGBM’s Booster object but it is
OpensourceML’s internal Booster wrapper object.
**Differences between training functionality of  lightgbm  and  td_lightgbm  follow:**
```python
lightgbm  populates the record evaluation results in the variable that was passed to the  record_
evaluation  function.
td_lightgbm  provides an attribute  record_evaluation_result  for the object returned by  train()
```
* method, which can be accessed as shown in the following example:
```python
opt_tr_s.record_evaluation_result
{'valid_0': OrderedDict([('l2',
[0.21581071275252517,
0.18813848372931546,
0.16614597654803748,
0.04169529314351532])])}
```
11: teradataml Open-Source Machine Learning Functions

Single model cross validation
The following example shows a single model cross validation using  td_lightgbm .
**Note:**
```python
return_cvbooster  argument is not yet supported for  cv() .
opt_cv_s = td_lightgbm.cv(params={}, train_set=obj_s1,
callbacks=[td_lightgbm.early_stopping(5)])
opt_cv_s
[LightGBM] [Warning] Auto-choosing col-wise multi-threading, the overhead of
testing was 0.000058 seconds.
You can set `force_col_wise=true` to remove the overhead.
[LightGBM] [Info] Total Bins 532
[LightGBM] [Info] Number of data points in the train set: 320, number of used
features: 4
[LightGBM] [Warning] Auto-choosing col-wise multi-threading, the overhead of
testing was 0.000054 seconds.
You can set `force_col_wise=true` to remove the overhead.
[LightGBM] [Info] Total Bins 532
[LightGBM] [Info] Number of data points in the train set: 320, number of used
features: 4
[LightGBM] [Warning] Auto-choosing col-wise multi-threading, the overhead of
testing was 0.000051 seconds.
You can set `force_col_wise=true` to remove the overhead.
[LightGBM] [Info] Total Bins 532
[LightGBM] [Info] Number of data points in the train set: 320, number of used
features: 4
[LightGBM] [Warning] Auto-choosing col-wise multi-threading, the overhead of
testing was 0.000045 seconds.
You can set `force_col_wise=true` to remove the overhead.
[LightGBM] [Info] Total Bins 532
[LightGBM] [Info] Number of data points in the train set: 320, number of used
features: 4
[LightGBM] [Warning] Auto-choosing col-wise multi-threading, the overhead of
testing was 0.000047 seconds.
You can set `force_col_wise=true` to remove the overhead.
[LightGBM] [Info] Total Bins 532
[LightGBM] [Info] Number of data points in the train set: 320, number of used
features: 4
Training until validation scores don't improve for 5 rounds
Early stopping, best iteration is:
```
11: teradataml Open-Source Machine Learning Functions

```python
[57]
cv_agg's l2: 0.067099 + 0.0151336
{'l2-mean': [0.22023353349592326,
0.19595424631042407,
0.06726610385135459,
0.06709902214997468],
'l2-stdv': [0.0015084316502001842,
0.0029052033405121175,
0.015130340427949404,
0.015133573776865996]}
```
Multi model training
After teradataml DataFrames are created, create the required Dataset objects. The following example
creates two Dataset objects, one for training and another for validation. Note that, in these examples,
teradataml introduces a new argument  partition_columns  for distributed/multi model training.
**Note:**
    * The teradataml DataFrames that are passed should be created from the same parent
    * teradataml DataFrame using the  select()  API.
    * The partition columns should be present in the parent DataFrame from which the required
    * DataFrames are derived from.
```python
Training Dataset.
obj_m = td_lightgbm.Dataset(df_x_classif, df_y_classif, silent=True,
partition_columns=["partition_column_1", "partition_column_2"],
free_raw_data=False)
obj_m
partition_column_1
partition_column_2                                              model
0                   1                  11  <lightgbm.basic.Dataset object
at 0x7f555198e640>
1                   0                  11  <lightgbm.basic.Dataset object
at 0x7f55519523a0>
2                   1                  10  <lightgbm.basic.Dataset object
at 0x7f5551968eb0>
```
11: teradataml Open-Source Machine Learning Functions

```python
3                   0                  10  <lightgbm.basic.Dataset object
at 0x7f55518f6430>
Validation dataset.
obj_m_v = td_lightgbm.Dataset(df_x_classif,
df_y_classif, free_raw_data=False,
partition_columns=["partition_column_1", "partition_column_2"])
obj_m_v
partition_column_1
partition_column_2                                              model
0                   1                  11  <lightgbm.basic.Dataset object
at 0x7f555198e9d0>
1                   0                  11  <lightgbm.basic.Dataset object
at 0x7f55684c6d90>
2                   1                  10  <lightgbm.basic.Dataset object
at 0x7f55518de640>
3                   0                  10  <lightgbm.basic.Dataset object
at 0x7f55518dec40>
```
**After creating the Dataset objects, run the training function with  record_evaluation  callback:**
```python
Training with valid_sets and callbacks argument.
rec = {}
opt_tr_m = td_lightgbm.train(params={},
train_set=obj_m_v, num_boost_round=30,
callbacks=[td_lightgbm.record_evaluation(rec)],
valid_sets=[obj_m_v, obj_m_v])
opt_tr_m
partition_column_1  partition_column_2  \
0                   1                  11
1                   0                  11
2                   1                  10
3                   0                  10
model  \
0  <lightgbm.basic.Booster object at 0x7f55518bd040>
1  <lightgbm.basic.Booster object at 0x7f55500a6be0>
2  <lightgbm.basic.Booster object at 0x7f55500a61c0>
3  <lightgbm.basic.Booster object at 0x7f55500a6670>
console_output  \
0  [LightGBM] [Warning] Auto-choosing col-wise mu...
1  [LightGBM] [Warning] Auto-choosing col-wise mu...
```
11: teradataml Open-Source Machine Learning Functions

```python
2  [LightGBM] [Warning] Auto-choosing col-wise mu...
3  [LightGBM] [Warning] Auto-choosing row-wise mu...
record_evaluation_result
0  {'valid_0': {'l2': [0.21963737683498566, 0.196...
1  {'valid_0': {'l2': [0.2229904865477988, 0.2008...
2  {'valid_0': {'l2': [0.2151413809523807, 0.1919...
3  {'valid_0': {'l2': [0.2195184911242605, 0.1948...
```
Since there are multiple models involved, each trained model has a different console output and
record_evaluation result. These are provided in pandas Dataframe and can be accessed using  model_
```python
info  attribute.
```
**Access pandas Dataframe and the individual record evaluation results or console outputs:**
```python
opt_tr_m.model_info
partition_column_1
partition_column_2
model
console_output
record_evaluation_result
0
1
11
<lightgbm.basic.Booster
object at ...>
[LightGBM] [Warning] Auto-choosing col-wise mu...
{'valid_0': {'l2': [0.21963737683498566, 0.196...
1
0
11
<lightgbm.basic.Booster
object at ...>
[LightGBM] [Warning] Auto-choosing col-wise mu...
{'valid_0': {'l2': [0.2229904865477988, 0.2008...
2
1
10
<lightgbm.basic.Booster
object at ...>
[LightGBM] [Warning] Auto-choosing col-wise mu...
{'valid_0': {'l2': [0.2151413809523807, 0.1919...
3
0
10
<lightgbm.basic.Booster
object at ...>
[LightGBM] [Warning] Auto-choosing row-wise mu...
{'valid_0': {'l2': [0.2195184911242605, 0.1948...
console output.
print(opt_tr_m.model_info.iloc[0]["console_output"])
[LightGBM] [Warning] Auto-choosing col-wise multi-threading, the overhead of
testing was 0.000037 seconds.
You can set `force_col_wise=true` to remove the overhead.
[LightGBM] [Info] Total Bins 136
[LightGBM] [Info] Number of data points in the train set: 97, number of used
features: 4
[LightGBM] [Info] Start training from score 0.556701
[LightGBM] [Warning] No further splits with positive gain, best gain: -inf
[LightGBM] [Warning] No further splits with positive gain, best gain: -inf
```
11: teradataml Open-Source Machine Learning Functions

```python
[LightGBM] [Warning] No further splits with positive gain, best gain: -inf
[LightGBM] [Warning] No further splits with positive gain, best gain: -inf
record_evaluation result.
print(opt_tr_m.model_info.iloc[0]["record_evaluation_result"])
{'valid_0': OrderedDict([('l2', [0.21963737683498566, 0.1965251124298607, ...,
0.06526645509697025])]), 'valid_1': OrderedDict([('l2', [0.21963737683498566,
0.1965251124298607, ..., 0.06526645509697025])])}
```
Multi model cross validation
The following example shows multi model cross validation using  td_lightgbm .
**Note:**
The argument  return_cvbooster  is not supported yet for  cv() .
```python
opt_cv_m = td_lightgbm.cv(params={}, train_set=obj_m_v, num_boost_round=30)
opt_cv_m
partition_column_1
partition_column_2
model
console_output
0
1
11
{'l2-mean':
[0.2228372778767334, 0.20378581597...
[LightGBM] [Warning] Auto-choosing col-
wise mu...
1
0
11
{'l2-mean':
[0.22670871234512563, 0.2076397899...
[LightGBM] [Warning] Auto-choosing col-
wise mu...
2
1
10
{'l2-mean':
[0.2254886102590233, 0.20486831815...
[LightGBM] [Warning] Auto-choosing col-
wise mu...
3
0
10
{'l2-mean':
[0.22325269845549206, 0.2001155354...
[LightGBM] [Warning] Auto-choosing col-
wise mu...
```
### Using Booster for prediction
teradataml’s Booster Wrapper object is returned by train(), which can be used to predict values on
new data. The  label  argument for predict() is introduced by teradataml for the function to return label
column in output DataFrame so that both true values and predicted values are in same teradataml
11: teradataml Open-Source Machine Learning Functions

DataFrame. The resultant DataFrame can be used to get some evaluation metrics like  accuracy_score
using OpenSourceML’s scikit-learn metrics functions.
* Single model prediction
* Multi model prediction
Single model prediction
To get predicted values,  predict  function should be called on the trained Booster object.
```python
opt_pred_s = opt_tr_s.predict(data=df_x_classif, label=df_y_classif,
num_iteration=20)
opt_pred_s.head(3)
col1
col2
col3
col4
label
booster_predict_1
0.191240730419773
1.97100133015377
-0.298530178457708
0.890194110648821
1
0.9609170779776948
-1.16999138897268
0.354732459083604
-0.193563184945544
-0.35802804454994
1
0.9358935125840876
1.61885814289279
0.530672471352164
0.101599080962697
0.914093399898154
0
0.4503489139975592
```
The following examples show how other supported functions can be accessed through Booster object.
### model_to_string
```python
str_opt = opt_tr_s.model_to_string(-1)
print(str_opt)
tree
version=v3
num_class=1
num_tree_per_iteration=1
label_index=0
max_feature_idx=3
objective=regression
[gpu_use_dp: 0]
[num_gpu: 1]
end of parameters
```
11: teradataml Open-Source Machine Learning Functions

```python
pandas_categorical:[]
End of model string output.
```
### model_from_string
```python
opt2 = opt_tr_s.model_from_string(str_opt)
opt2
Finished loading model, total used 30 iterations
<lightgbm.basic.Booster object at 0x7f5553d235b0>
```
Multi model prediction
For prediction in multi model case,  partition_columns  argument may or may not be provided. If this
argument is not provided, the partition columns and values are taken from training.
```python
opt_pred_m = opt_tr_m.predict(data=df_x_classif, label=df_y_classif,
num_iteration=20)
opt_pred_m.head(3)
partition_column_1
partition_column_2
col1
col2
col3
col4
label
booster_predict_1
1                  10  0.191240730419773
1.97100133015377
-0.298530178457708
0.890194110648821
1
0.9609170779776948
0                  10  -1.16999138897268
0.354732459083604
-0.193563184945544
-0.35802804454994
1
0.9358935125840876
1                  11   1.61885814289279
0.530672471352164
0.101599080962697
0.914093399898154
0
0.4503489139975592
```
**Booster Wrapper object can be instantiated by passing arguments to the class as follows:**
### Passing  model_file  argument
The existing model needs to be saved to a file.
```python
opt_tr_s.save_model("model_file_single_partition")
<lightgbm.basic.Booster object at 0x7f5553bbe250>
```
11: teradataml Open-Source Machine Learning Functions

### Instantiate Booster Wrapper object through  model_file  argument
```python
direct_obj = td_lightgbm.Booster(model_file="model_file_single_partition")
direct_obj
<lightgbm.basic.Booster object at 0x7f5553c28580>
```
### Use  model_str  argument if model is already present as string
```python
direct_obj1 = td_lightgbm.Booster(model_str = str_opt)
direct_obj1
<lightgbm.basic.Booster object at 0x7f5553b6c220>
```
### Running scikit-learn API
LightGBM offers the following scikit-learn APIs whose syntax and usage are similar to scikit-learn classes.
All classes are supported for single and multi model cases.
* Single model case - LGBMRegressor
* Multi model case - LGBMClassifier
Single model case - LGBMRegressor
The following example shows how to create the LGBRegressor object, set parameters, train the model,
predict the values, and access attributes.
### Create LGBMRegressor object
```python
obj = td_lightgbm.LGBMRegressor(num_leaves=5, n_estimators=15,
learning_rate=0.01)
obj
LGBMRegressor(learning_rate=0.01, n_estimators=15, num_leaves=5)
```
### Set/update parameters
```python
obj.set_params(n_estimators=10)
LGBMRegressor(learning_rate=0.01, n_estimators=10, num_leaves=5)
```
### Train the model
```python
obj.fit(df_x_reg, df_y_reg, callbacks=[td_lightgbm.log_evaluation()])
LGBMRegressor(learning_rate=0.01, n_estimators=10, num_leaves=5)
```
11: teradataml Open-Source Machine Learning Functions

### Predict the values
```python
obj.predict(X=df_x_reg)
col1
col2
col3
col4  lgbmregressor_predict_1
0.994394391315499
-0.27567053456055
-0.70972796584688
1.73887267745451
-12.0
0.978567297446043
0.02538560
6459
0.610391764305413
0.28601252697811
0.0
-1.18468659041155
-0.85172919725359
1.822723600123796
-0.5215796779933
7.0
1.5363770542458
-0.11054065723247
1.020172711715805
-0.6920498477843
7.0
-2.73967716718956
-0.13010695419370
0.093953229385568
0.94304608732251
-3.0
0.378162519602174
1.532779214358461
1.469358769900291
0.15494742569691
7.0
-1.82691137830452
0.917221542115856
-0.05704286766218
0.87672677369045
0.0
-0.37522240076067
0.434807957731158
0.540094460524806
0.73242400975487
0.0
-0.52118931230111
1.364531848102473
-0.68944918454993
-0.6522935999350
-12.0
-0.76157338825655
-2.36417381714118
0.020334181705243
-1.3479254226291
-7.0
```
### Access attributes
```python
obj.feature_importances_
array([ 0, 10, 30,  0], dtype=int32)
obj.objective_
'regression'
```
Multi model case - LGBMClassifier
For multi model training, fit() method should take  partition_columns  argument. These columns should be
present in the parent DataFrame from which X and y teradataml DataFrames are derived from.
### Create LGBMClassifier object
```python
obj = td_lightgbm.LGBMClassifier(num_leaves=5,
n_estimators=15, learning_rate=0.01)
```
11: teradataml Open-Source Machine Learning Functions

```python
obj
LGBMClassifier(learning_rate=0.01, n_estimators=15, num_leaves=5)
```
### Set/update parameters
```python
obj.set_params(n_estimators=10)
LGBMClassifier(learning_rate=0.01, n_estimators=10, num_leaves=5)
```
### Train the model
```python
obj.fit(df_x_classif, df_y_classif,
callbacks=[td_lightgbm.log_evaluation()],
partition_columns=["partition_column_1", "partition_column_2"])
partition_column_1
partition_column_2
model
0                   1                  11  LGBMClassifier(learning_rate=0.01,
n_estimators=10, num_leaves=5)
1                   0                  11  LGBMClassifier(learning_rate=0.01,
n_estimators=10, num_leaves=5)
2                   1                  10  LGBMClassifier(learning_rate=0.01,
n_estimators=10, num_leaves=5)
3                   0                  10  LGBMClassifier(learning_rate=0.01,
n_estimators=10, num_leaves=5)
```
### Predict the values
```python
obj.predict(X=df_x_classif)
partition_column_1
partition_column_2                  col1
col2
col3
col4 lgbmclassifier_predict_1
1
10
0.99439439131549
-0.27567053456055
-0.70972796584688
1.73887267745451
0
0
10     0.978567297446043
0.025385604406459
0.610391764305413
0.28601252697811
0
0
10
-1.18468659041155
-0.85172919725359
1.822723600123796
-0.5215796779933
1
1
10
1.5363770542458 -0.11054065723247
1.020172711715805
-0.6920498477843
0
1
11
-2.73967716718956
-0.13010695419370
0.093953229385568
```
11: teradataml Open-Source Machine Learning Functions

```python
0.94304608732251
0
1
11     0.378162519602174
1.532779214358461
1.469358769900291
0.15494742569691
0
1
10     -1.82691137830452
0.917221542115856
-0.05704286766218
0.87672677369045
1
1
10     -0.37522240076067
0.434807957731158
0.540094460524806
0.73242400975487
1
1
11     -0.52118931230111
1.364531848102473
-0.68944918454993
-0.6522935999350
0
1
11
-0.76157338825655
-2.36417381714118
0.020334181705243
-1.3479254226291
0
```
### Score per partition
```python
obj.score(df_x_classif, df_y_classif,
sample_weight=df_train_classif.select("group_column"))
partition_column_1
partition_column_2
score
0
11
0.8322981366459627
0
10
0.8705994291151284
1
10
0.5585585585585585
1
11
0.5515088449531738
```
### Access attributes
```python
obj.feature_importances_
partition_column_1
partition_column_2
feature_importances_
0
1
11
[10, 10, 10, 0]
1
0
11
[3, 7, 10, 3]
2
1
10
[7, 18, 1, 2]
3
0
10
[10, 20, 0, 0]
obj.objective_
partition_column_1
partition_column_2
objective_
0
1
11
binary
1
0
11
binary
2
1
10
binary
3
0
10
binary
```
11: teradataml Open-Source Machine Learning Functions

### Support for Model Operations using td_lightgbm
See the following prerequisites (data preparation) and examples of deploying and loading models using
**lightgbm opensourceML class objects:**
* Data preparation
* Example: Single model deployment and loading of td_lightgbm Booster trained in Vantage
* Example: Distributed model deployment and loading of td_lightgbm Booster trained in Vantage
* Example: Single model deployment and loading of td_lightgbm sklearn model trained in Vantage
* Example: Distributed model deployment and loading of td_lightgbm sklearn model trained in Vantage
* Example: Deploy and load the Booster model trained outside Vantage using train()
* Example: Deploy and load the sklearn model trained outside Vantage
Data preparation
Use the relevant single or multi model case statement to prepare your data, then validate the dataset and
get the pandas DataFrame of data.
### Single model case
```python
obj_s = td_lightgbm.Dataset(df_x_classif, df_y_classif,
silent=True, free_raw_data=False)
```
### Multi model case
```python
obj_m = td_lightgbm.Dataset(df_x_classif, df_y_classif, free_raw_data=False,
partition_columns=["partition_column_1", "partition_column_2"])
```
### Validation dataset
```python
obj_m_v = td_lightgbm.Dataset(df_x_classif,
df_y_classif, free_raw_data=False,
partition_columns=["partition_column_1", "partition_column_2"])
```
### Get pandas DataFrame of data for training locally
```python
pdf_x = df_x_classif.to_pandas().reset_index()
pdf_y = df_y_classif.to_pandas()
```
11: teradataml Open-Source Machine Learning Functions

Example: Single model deployment and loading of td_lightgbm Booster
trained in Vantage
1.
* Train the data without the callbacks argument.
```python
opt_s = td_lightgbm.train(params={}, train_set=obj_s,
num_boost_round=30, valid_sets=[obj_s])
type(opt_s)
teradataml.opensource._lightgbm._LightgbmBoosterWrapper
```
2.
* Deploy the model.
```python
opt_s.deploy(model_name="lightgbm_deploy_train_single_model")
Model is saved.
<lightgbm.basic.Booster object at 0x000...00>
opt_s.record_evaluation_resultEmpty as no record evaluation
callback used.
```
3.
* Load the deployed model.
```python
opt_load
= td_lightgbm.load(model_name="lightgbm_deploy_train_single_model")
opt_load
<lightgbm.basic.Booster object at 0x000...E0>
```
4.
* Predict using the loaded model.
```python
opt_load.predict(data=df_x_classif, label=df_y_classif)
col1
col2
col3
col4
label
booster_predict_1
1.0823357639576
0.84635733604494
-0.012062715650015
0.812633063515458
1
0.9346760906166353
-0.7745167656447
1.03844942569731
-0.258906316647375
0.092392283225207
1
0.9704807283586632
-0.9709790567905
0.29023691666418
-0.159962241726072
-0.298832196718898
1
0.9747562930550571
-1.1673562332519
0.10485969688830
-0.152596373567538
-0.459316049285644
1
0.9527103200682584
```
11: teradataml Open-Source Machine Learning Functions

```python
-1.4168228235536
-1.10436212447853
0.015212098341070
-1.062313493751677
0
0.1671869702680136
1.0246109854249
-1.42517183237691
0.350872770060749
-0.143296130974637
0
0.0307264091286578
0.7595463230046
0.0453714
9357
0.080802080624812
0.345420111262436
0
0.0507321960512050
0.6469851931442
-0.58122848711195    0.169697726509673
0.040145692994821
0
0.0341349264570978
-1.4719082500811
-0.02919489258849
-0.166141412269092
-0.6
45309128519277
1
0.8729191084916251
-1.1750967374648
-0.95074511349926
0.018279571156030
-0.895335003873629
1
0.5546785905584524
```
5.
* Create a single model with record_evaluation callback
```python
rec = {}
```
6.
* Train with valid_sets and callbacks argument.
```python
opt1 = td_lightgbm.train(params={}, train_set = obj_s,
num_boost_round=30,
callbacks=[td_lightgbm.record_evaluation(rec),
td_lightgbm.early_stopping(3)],
valid_sets=[obj_s])
opt1.record_evaluation_result
{'valid_0': OrderedDict([('l2',
[0.21581071275252509,
0.18813848372931546,
0.04169529314351532])])}
```
7.
* Deploy the model.
```python

opt1.deploy(model_name="lightgbm_deploy_train_single_model_with_callback")
Model is saved.
<lightgbm.basic.Booster object at 0x00..8B0>
```
8.
* Load the deployed model.
```python
opt1_load =
td_lightgbm.load(model_name="lightgbm_deploy_train_single_model_with_callbac
k")
```
11: teradataml Open-Source Machine Learning Functions

```python
opt1_load.record_evaluation_resultDeploy will not save wrapper
attributes. It just saves underlying model object.
opt1_load.predict(df_x_classif)
col1
col2              col3
col4
booster_predict_1
-0.697767009551012
2.3918078347398 -0.47022244995057
0.680153033261556
0.9798849376620596
-0.733263307298235
1.9773705102473 -0.40690381676384
0.495003202510811
0.9699154086912621
0.9577016978304661
-1.410567531062
0.340727932762679
-0.16610004792191
0.0295051822937243
-1.083977873075923
1.8792459682678 -0.43165520428563
0.303874588671142
0.9832371207930688
-0.655697611869257
-0.708487381682
0.039161396505109
-0.57254406467090
0.3199544221250283
-0.652532094184103
1.3294307289508 -0.29209389759953
0.264152749130696
0.9732308238382781
-1.537164191873611
-0.750650223194 -0.05631805152509
-0.969109623
12450
0.6395031001506465
0.9082253663109264
-1.169232609988
0.295712076085351
-0.08846679694776
0.0159457535568849
-0.741761812488457
-1.319910570598
0.128663746621739
-0.86019632384497
0.1768105684264068
1.4588391821477944
0.6278710266435 0.067203725213036
0.885080711754702
0.6711517783816373
```
Example: Distributed model deployment and loading of td_lightgbm
Booster trained in Vantage
1.
* Train with valid_sets argument.
```python
opt_m = td_lightgbm.train(params={}, train_set=obj_m,
num_boost_round=30, early_stopping_rounds=50,
valid_sets=[obj_m_v, obj_m_v])
```
2.
* Deploy distributed models.
```python
opt_m_deploy
= opt_m.deploy(model_name="lightgbm_deploy_train_multi_model")
opt_m_deploy
partition_column_1  partition_column_2
```
11: teradataml Open-Source Machine Learning Functions

```python
model  \
0                   1                  11   <lightgbm.basic.Booster object
at 0x00..18...
1                   0                  11   <lightgbm.basic.Booster object
at 0x00..18...
2                   1                  10   <lightgbm.basic.Booster object
at 0x00..18...
3                   0                  10   <lightgbm.basic.Booster object
at 0x00..18...
console_output
0  [LightGBM] [Warning] Auto-choosing col-wise mu...
1  [LightGBM] [Warning] Auto-choosing col-wise mu...
2  [LightGBM] [Warning] Auto-choosing row-wise mu...
3  [LightGBM] [Warning] Auto-choosing col-wise mu...
```
3.
* Load the deployed distributed models.
```python
opt_m_load = td_lightgbm.load("lightgbm_deploy_train_multi_model")
```
4.
* Predict using loaded models.
```python
opt_m_load.predict(df_x_classif)
partition_column_1
partition_column_2
col1
col2
col3
col4 booster_predict_1
1
10
0.32696596570584
1.47330535418833
-0.20178393176355  0.74459119624978
1.023338334370605
1
10
-1.0004943664752
-0.5036412703804
-0.03420420369756  -0.6369398278106
0.139315093910313
1
10
0.52297759126479
1.06716836387948
-0.11293733853424  0.66245837528695
0.938397513812559
1
10
1.05259185573158
1.19821169185417
-0.07277200487147  0.94405839783240
0.921848136707542
1
10
0.31320927089271
1.40774148153391  -0.19271215342466  0.71179749886599
0.925780938479566
1
10
-0.8238860682069
2.14052731749492
-0.44397524257905  0.52288679440173
0.925780938479566
0
10
0.73106437158004
```
11: teradataml Open-Source Machine Learning Functions

```python
-1.7427860510320
0.368475460169321
-0.3997941143268
-0.01629951110533
0
10
1.77127535720407
0.47018240258731
0.129138419955093  0.95488182548344
0.640713746422475
0
10
-1.0974686702160
1.97082483622747
-0.
12380705639  0.33560750373566
1.003188524949226
0
10
-3.0355858113854
-1.6588988201468
-0.08249259860607  -1.9861478027571
0.066485139499413
```
5.
* Create a multi model with record_evaluation callback.
```python
rec = {}
```
6.
* Train with valid_sets and callbacks argument.
```python
opt_m_r = td_lightgbm.train(params={}, train_set=obj_m,
num_boost_round=30, callbacks=[td_lightgbm.record_evaluation(rec)],
valid_sets=[obj_m_v, obj_m_v])
opt_m_r
partition_column_1  partition_column_2  \
0                   1                  11
1                   0                  11
2                   1                  10
3                   0                  10
model  \
0  <lightgbm.basic.Booster object at 0x0000029D18...
1  <lightgbm.basic.Booster object at 0x0000029D18...
2  <lightgbm.basic.Booster object at 0x0000029D18...
3  <lightgbm.basic.Booster object at 0x0000029D18...
console_output  \
0  [LightGBM] [Warning] Auto-choosing col-wise mu...
1  [LightGBM] [Warning] Auto-choosing col-wise mu...
2  [LightGBM] [Warning] Auto-choosing col-wise mu...
3  [LightGBM] [Warning] Auto-choosing row-wise mu...
record_evaluation_result
0  {'valid_0': {'l2': [0.2196373768349857, 0.1965...
1  {'valid_0': {'l2': [0.22299048654779868, 0.200...
2  {'valid_0': {'l2': [0.21514138095238086, 0.191...
3  {'valid_0': {'l2': [0.2195184911242605, 0.1948...
```
11: teradataml Open-Source Machine Learning Functions

7.
* Deploy trained distributed models.
```python
opt_m_r_deploy
= opt_m_r.deploy("lightgbm_deploy_train_multi_model_with_record_eval")
```
8.
* Load the deployed models.
```python
opt_m_r_load
= td_lightgbm.load("lightgbm_deploy_train_multi_model_with_record_eval")
```
9.
* Predict using loaded models.
```python
opt_m_r_load.predict(df_x_classif, label=df_y_classif)
partition_column_1
partition_column_2
col1
col2
col3
col4 label
booster_predict_1
1
10
1.05259185573158  1.19821169185417  -0.0727720048714  0.944058397832
1
0.921848136707542
1
10
-1.0004943664752  -0.5036412703804  -0.0342042036975  -0.63693982781
0
0.139315093910313
1
10
0.52297759126479  1.06716836387948  -0.1129373385342  0.662458375286
1
0.938397513812559
1
10
-0.5066609936524  2.31850732028455  -0.4361066493234  0.732337775994
1
1.023338334370605
1
10
-1.5819640423908  -0.9365785479499  -0.0312640459629  -1.06459785797
0
0.078450464059613
1
10
-0.8238860682069  2.14052731749492  -0.4439752425790  0.522886794401
1
0.925780938479566
0
10
1.09867539212952  -0.7489871189513  0.24943881268310  0.165738276697
0
0.142214969318460
0
10
1.77127535720407  0.47018240258731  0.12913841995509  0.954881825483
1
0.640713746422523
0
10
-1.0257898611087  -0.2476906693605  -0.0787909605284  -0.54291097851
1
0.380117443587507
0
```
11: teradataml Open-Source Machine Learning Functions

```python
10
-1.2011699609085  -1.3113290698053  0.07392888868973  -1.05435592604
0
0.001931702920989
```
Example: Single model deployment and loading of td_lightgbm sklearn
model trained in Vantage
1.
* Create an object of LGBMModel using 'td_lightgbm'.
```python
obj_skl_s = td_lightgbm.LGBMModel(num_leaves=15,
objective="binary", n_estimators=10)
```
2.
* Train the model.
```python
obj_skl_s.fit(df_x_classif,
df_y_classif, callbacks=[td_lightgbm.log_evaluation()])
```
3.
* Deploy the model.
```python
obj_skl_deploy_s
= obj_skl_s.deploy(model_name="lightgbm_sklearn_single_model")
Model is saved.
```
4.
* Load the trained model.
```python
obj_skl_load_model_s = td_lightgbm.load("lightgbm_sklearn_single_model")
```
5.
* Predict using the loaded model.
```python
obj_skl_load_model_s.predict(df_x_classif, pred_leaf=True)
col1
col2
col3
col4 lgbmmodel_predict_1 lgbmmodel_predict_2  ... lgbmmodel_predict_10
0.19124073041977  1.971001330153  -0.29853017845  0.890194110648
8
13  ...
13
-1.1699913889726  0.354732459083  -0.19356318494  -0.35802804454
3
1  ...
1
-0.8449702490257  1.453101117091  -0.33456086466  0.232041220971
8
13  ...
13
1.61885814289279  0.530672471352  0.101599080962  0.914093399898
7
7  ...
7
```
11: teradataml Open-Source Machine Learning Functions

```python
-1.0093122519523  1.498390036418  -0.36101107471  0.179890827548
8
12  ...
12
1.49413777147502  0.172801814430  0.145353695535  0.713738112186
2
2  ...
2
-0.8103072656328  -0.95301356540  0.061001667799  -0.73930084696
4
4  ...
4
-0.8156136865920  -1.23797225905  0.106755845337  -0.85838759227
2
9  ...
3
-1.0993936064538  0.871762881447  -0.26950104137  -0.11572200547
4
8  ...
8
-1.2173132357157  -0.75099738722  -0.01912607171  5
-0.831624374
7
4  ...
4
```
Example: Distributed model deployment and loading of td_lightgbm
sklearn model trained in Vantage
1.
* Create an object of LGBMModel using 'td_lightgbm'.
```python
obj_skl_m = td_lightgbm.LGBMModel(num_leaves=15,
objective="binary", n_estimators=5)
```
2.
* Train the model.
```python
obj_skl_m.fit(df_x_classif,
df_y_classif, sample_weight=df_train_classif.select("group_column"),
partition_columns=["partition_column_1", "partition_column_2"],
callbacks=[td_lightgbm.log_evaluation()])
partition_column_1
partition_column_2
model
0                   1                  11  LGBMModel(n_estimators=5, num_leaves=15,
objective='binary')
1                   0                  11  LGBMModel(n_estimators=5, num_leaves=15,
objective='binary')
2                   1                  10  LGBMModel(n_estimators=5, num_leaves=15,
objective='binary')
```
11: teradataml Open-Source Machine Learning Functions

```python
3                   0                  10  LGBMModel(n_estimators=5, num_leaves=15,
objective='binary')
```
3.
* Deploy multiple models created on multiple partitions.
```python
obj_deploy_skl_m
= obj_skl_m.deploy(model_name="lightgbm_sklearn_multi_model")
Model is saved.
```
4.
* Load the deployed models.
```python
obj_skl_load_model_m = td_lightgbm.load("lightgbm_sklearn_multi_model")
```
5.
* Predict using the loaded models.
```python
obj_skl_load_model_m.predict(df_x_classif,
raw_score=True, pred_leaf=True)
partition_column_1
partition_column_2
col1  ...
col4  lgbmmodel_predict_1
lgbmmodel_predict_2  ...   lgbmmodel_predict_5
1
10
0.05870288650510  ...  -1.17785382356
0
0  ...
1
1
10
-1.1615345341513  ...  -0.86423207141
3
2  ...
1
1
10
0.78433767392278  ...  0.720943051314
1
1  ...
0
1
10
-0.2464757122084  ...  -0.40166606892
8
3  ...
2
1
10
0.23164855319438  ...  -1.26023579265
0
0  ...
1
1
10
-0.7267611459507  ...  -0.32066543120
7
1  ...
2
0
10
2.082631343
29  ...  1.258519751162
2
```
11: teradataml Open-Source Machine Learning Functions

```python
2  ...
2
0
10
1.43975694528171  ...  0.656882820481
3
3  ...
3
0
10
-0.6124690179513  ...  -0.48785374115
5
3  ...
3
0
10
1.30373436256498  ...  0.933048019921
2
2  ...
2
```
6.
* Run attributes on the loaded models.
```python
obj_skl_load_model_m.feature_importances_
partition_column_1
partition_column_2
feature_importances_
0
1
11
[3, 11, 1, 0]
1
0
11
[0, 5, 7, 0]
2
1
10
[2, 7, 2, 2]
3
0
10
[4, 10, 0, 1]
```
Example: Deploy and load the Booster model trained outside Vantage
using train()
1.
* Import the lightgbm module and create a Booster object.
```python
from lightgbm import Dataset, train
pdf_dataset = Dataset(params={},data=pdf_x, label=pdf_y)
pdf_dataset
<lightgbm.basic.Dataset at 0x29d...a0>
local_train = train(train_set=pdf_dataset, params={})
local_train
[LightGBM] [Warning] Auto-choosing col-wise multi-threading, the overhead
of testing was 0.000096 seconds.
You can set `force_col_wise=true` to remove the overhead.
[LightGBM] [Info] Total Bins 532
```
11: teradataml Open-Source Machine Learning Functions

```python
[LightGBM] [Info] Number of data points in the train set: 400, number of
used features: 4
[LightGBM] [Info] Start training from score 0.507500
[LightGBM] [Warning] No further splits with positive gain, best gain: -inf
[LightGBM] [Warning] No further splits with positive gain, best gain: -inf
[LightGBM] [Warning] No further splits with positive gain, best gain: -inf
<lightgbm.basic.Booster at 0x29d19f446d0>
type(local_train)
lightgbm.basic.Booster
```
2.
* Deploy the trained lightgbm Booster model in Vantage.
```python
opt_outside =
td_lightgbm.deploy(model_name="model_trained_outside_vantage_using_train",
model=local_train)
Model is saved.
type(opt_outside)
teradataml.opensource._lightgbm._LightgbmBoosterWrapper
```
3.
* Load the deployed model.
```python
opt_load_outside
= td_lightgbm.load("model_trained_outside_vantage_using_train")
type(opt_load_outside)
teradataml.opensource._lightgbm._LightgbmBoosterWrapper
```
4.
* Predict on data residing in Vantagee using the loaded model.
```python
opt_load_outside.predict(data=df_x_classif,
label=df_y_classif, pred_contrib=True)
col1
col2
col3
col4 label
booster_predict_1  ...
booster_predict_5
0.19124073041977  1.971001330153  -0.29853017845  0.890194110648
1
0.031404602776514  ...  0.2040997952197996
-1.1699913889726  0.354732459083  -0.19356318494  -0.35802804
1
0.092682932810633  ...  0.2532883087965086
-0.8449702490257  1.453101117091  -0.33456086466  0.232041220971
1
```
11: teradataml Open-Source Machine Learning Functions

```python
0.046136151499138  ...  0.2164065785284724
1.61885814289279  0.530672471352  0.101599080962  0.914093399898
0
-0.10520322255265  ...  0.10279002
23291
-1.0093122519523  1.498390036418  -0.36101107471  0.179890827548
1
0.062801639713840  ...  0.2107812341392544
1.49413777147502  0.172801814430  0.145353695535  0.713738112186
0
-0.16550849715057  ...  0.1549907235100593
-0.8103072656328  -0.95301356540  0.061001667799  -0.73930084696
0
-0.00069855457829  ...  -0.171764014056180
-0.8156136865920  -1.23797225905  0.106755845337  -0.85838759227
0
0.001558571995085  ...  -0.195691235260052
-1.0993936064538  0.871762881447  -0.26950104137  -0.11572200547
1
0.065010292043574  ...  0.2115849205082161
-1.2173132357157  -0.75099738722  -0.01912607171  -0.83162437457
0
0.021267893372609  ...  -0.244580217753899
```
Example: Deploy and load the sklearn model trained outside Vantage
1.
* Import the lightgbm module and create an LGBMClassifier model.
```python
from lightgbm import LGBMClassifier
local_obj = LGBMClassifier(num_leaves=5, objective="binary",
n_estimators=10, learning_rate=0.01)
local_obj.fit(pdf_x, pdf_y)
LGBMClassifier(learning_rate=0.01, n_estimators=10,
num_leaves=5, objective='binary')
type(local_obj)
lightgbm.sklearn.LGBMClassifier
```
2.
* Deploy the trained sklearn LGBMClassifier model in Vantage.
```python
skl_deploy =
td_lightgbm.deploy(model_name="skl_model_trained_outside_vantage",
model=local_obj)
Model is saved.
type(skl_deploy)
teradataml.opensource._lightgbm._LighgbmSklearnWrapper
```
3.
* Predict on data residing in Vantage using the loaded model.
11: teradataml Open-Source Machine Learning Functions

```python
opt_load_outside.predict(data=df_x_classif,
label=df_y_classif, pred_contrib=True)
col1
col2
col3
col4  lgbmclassifier_predict_1
1.08233576395768  0.846357336044  -0.012062715650015  0.812633063515
1
-0.7745167650864  1.038449425697  -0.258906316647375  0.092392283225
1
-0.9709790567988  0.290236916664  -0.159962241726072  -0.29883219671
1
-1.1673562332519  0.104859696888  -0.152596373567532  -0.45931604928
1
-1.4168228235533  -1.10436212447  0.0152120983410702  -1.06231349375
0
1.02461098542497  -1.42517183237  0.3508727700607491  -0.14329613097
0
0.75954632300467  0.045371444593  0.0808020806248121  0.345420111262
1
0.64698519314421  -0.58122848711  0.1696977265096732  0.0401
92994
0
-1.4719082500819  -0.02919489258  -0.166141412269092  -0.64530912851
1
-1.1750967374649  -0.95074511349  0.0182795711560301
-0.89533500387
1
```
### td_lightgbm Supportability Matrix
Download the  td_lightgbm Supportability Matrix.pdf  from the  attachment   in the left pane.
This document provides the supportability matrix for each function in all the modules that are supported
in lightGBM.
### td_lightgbm Usage Notes
* predict(), which return only labels/predictions in lightGBM, will return the features data along with
* labels/predictions.
* Any functions marked as not supported will be supported in a future release.
* Support for classes/functions taking generators, iterators and functions or callable objects will be
* added in a future release.
See the attached Supportability Matrix for teradataml OpenSourceML - td_lightgbm classes and
functions support.
11: teradataml Open-Source Machine Learning Functions

## Limitations and Considerations
* Single model generation in VantageCloud Enterprise and VantageCloud Lake might reach memory
* limits when trained on very large data.
```python
split()  function of  model_selection  module returns generator of train and test data when used
```
* locally with  scikit-learn , but with teradataml OpenSourceML module, it returns generator of train
* and test as teradataml DataFrames.
* Functions like  predict ,  transform , and so on, return only labels/predictions in scikit-learn. They return
* the features data along with labels/predictions in teradataml OpenSourceML.
```python
list  and  delete  model in Model Operations are not supported.
```
    * To list the deployed models, run  DataFrame(“opensourceml_models”)  or  SELECT *
```python
FROM opensourceml_models .
```
    * To delete the models, run SQL query to delete the rows from the table “opensourceml_models”.
* Classes that take generators, iterators, functions, and callable objects as arguments are not supported.
```python
feature_extraction  module is not supported.
```
* Performance issues can result during single model generation when trained on very large data.
* On an AI on-prem system, if the Apply table operator is not available and you attempts to execute
* OpenML functions, it will raise a runtime error.
* You can resolve the issue by setting the table_operator configuration option to "script":
```python
from teradataml import configure
configure.table_operator = "script"
```
See attached Supportability Matrix for teradataml OpenSourceML - scikit-learn classes and
functions support.
11: teradataml Open-Source Machine Learning Functions