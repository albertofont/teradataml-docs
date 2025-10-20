# AutoML Examples

```python
load_example_data("teradataml", "titanic")
titanic_data = DataFrame("titanic")
```
### Example 1: Visualize the data using AutoML class
```python
AutoML.visualize(data = titanic_data,
target_column = 'survived',
plot_type = ['heatmap', 'pair', 'histogram', 'target'],
length = 10,
breadth = 8,
max_features = 10,
problem_type = 'classification')
```
### Example 2: Visualize the data using AutoDataPrep class
```python
from teradataml import AutoDataPrep
obj = AutoDataPrep(task_type="classification")
obj.fit(data = titanic_data, target_column = 'survived')
```
Retrieve the data from AutoDataPrep object
```python
datas = obj.get_data()
AutoDataPrep.visualize(data = datas['lasso_train'],
target_column = 'survived',
plot_type = 'all'
length = 20,
breadth = 15)
```
## AutoML Examples
* Example 1: Run AutoML for Regression Problem with Early Stopping Timer and Metrics Threshold
* Example 2: Run AutoRegressor for Regression Problem with Early Stopping Condition
* and Customization
* Example 3: Run AutoClassifier for Classification Problem using Early Stopping Timer
* Example 4: Run AutoML for classification problem using early stopping timer, max_models
* and customization
* Example 4: Run AutoML for Classification Problem using Early Stopping Timer and Customization
* Example 5: Run AutoML for multiclass classification problem using early stopping timer
* and max_models
* Example 5: Run AutoML for Multiclass Classification Problem using Early Stopping Timer
* Example 6: Run AutoClassifier for Multiclass Classification Problem using Early Stopping Timer
* Example 7: Run Trained AutoML Models across Multiple Sessions using deploy and load APIs
19: AutoML in teradataml

* Example 7: Run AutoML for Classification Problem Using Early Stopping Metrics and max_models
### Example 1: Run AutoML for Regression Problem with Early
### Stopping Timer and Metrics Threshold
This example predicts the price of house based on different factors.
**Run AutoML to get the best performing model with the following specifications:**
* Set early stopping criteria, that is, time limit to 100300 sec and performance metrics R2 threshold value
* to 0.7.
* Exclude 'knn' model from default model training list.
* Exclude 'knn', 'glm', and 'svm' models from default model training list.
* Opt for verbose level 2 to get detailed logging.
1.
* Load the example dataset.
```python
load_example_data("decisionforestpredict",
["housing_train", "housing_test"])
housing_train = DataFrame.from_table("housing_train")
housing_test = DataFrame.from_table("housing_test")
```
2.
* Create an AutoML instance.
```python
aml = AutoML(task_type="Regression",
exclude=['knn', 'glm', 'svm'],
verbose=2,
max_runtime_secs=100,
stopping_metric='R2',
stopping_tolerance=0.7
seed=42)
aml = AutoML(task_type="Regression",
exclude=['knn'],
verbose=2,
max_runtime_secs=300,
stopping_metric='R2',
stopping_tolerance=0.7
seed=42)
```
3.
* Fit the data.
```python
aml.fit(housing_train,housing_train.price)
```
19: AutoML in teradataml

```python
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 492
Total Columns in the data: 14
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
prefarea
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
lotsize FLOAT
492
0
None
0
492
0
0.0
100.0
stories INTEGER 492
0
None
0
492
0
0.0
100.0
fullbase
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
gashw
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
garagepl
INTEGER 492
0
None
270
222
0
0.0
100.0
homestyle
VARCHAR(20) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
sn
INTEGER 492
0
None
0
492
0
0.0
100.0
airco
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
bedrooms
INTEGER 492
0
None
0
492
0
0.0
100.0
driveway
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
price
FLOAT
492
0
None
0
492
0
0.0
100.0
recroom VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
bathrms INTEGER 492
0
None
0
492
0
0.0
100.0
sn       price    lotsize  bedrooms  bathrms  stories  garagepl
func
50%    274.000   62000.000   4616.000     3.000    1.000    2.000     0.000
count  492.000     492.000    492.000   492.000  492.000  492.000   492.000
mean   272.943   68100.396   5181.795     2.965    1.293    1.803     0.685
min      1.000   25000.000   1650.000     1.000    1.000    1.000     0.000
max    546.000  190000.000  16200.000     6.000    4.000    4.000     3.000
75%    413.250   82000.000   6370.000     3.000    2.000    2.000     1.000
```
19: AutoML in teradataml

```python
25%    132.500   49975.000   3600.000     2.000    1.000    1.000     0.000
std    159.501   26472.496   2182.443     0.731    0.510    0.861     0.854
Statistics of Data:
func
sn
price
lotsize bedrooms
bathrms stories garagepl
50%
274
62000
4616
3
1
2
0
count
492
492
492
492
492
492
492
mean
272.943 68100.396
5181.795
2.965
1.293
1.803
0.685
min
1
25000
1650
1
1
1
0
max
546
190000
16200
6
4
4
3
75%
413.25
82000
6370
3
2
2
1
25%
132.5
49975
3600
2
1
1
0
std
159.501 26472.496
2182.443
0.731
0.51
0.861
0.854
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
driveway                  2
recroom                   2
fullbase                  2
gashw                     2
airco                     2
prefarea                  2
homestyle                 3
No Futile columns found.
Target Column Distribution:
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0      price           2.439024
1    stories           7.113821
2   bedrooms           2.235772
3    lotsize           2.235772
4   garagepl           2.235772
5    bathrms           0.203252
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
```
19: AutoML in teradataml

```python
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.56 sec
Handling less significant features from data ...
Analysis indicates all categorical columns are significant. No action
Needed.
Total time to handle less significant features: 15.24 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
Checking Missing values in dataset ...
Analysis Completed. No Missing Values
Detected.
Total time to find missing values in data: 7.52 sec
Imputing Missing Values ...
Analysis completed. No imputation
required.
Time taken to perform imputation: 0.00 sec
Performing encoding for categorical columns ...
ONE HOT Encoding these Columns:
['driveway', 'recroom', 'fullbase', 'gashw', 'airco', 'prefarea',
'homestyle']
Sample of dataset after performing one hot encoding:
sn
price
lotsize bedrooms
bathrms stories driveway_0
driveway_1
recroom_0
recroom_1
fullbase_0
fullbase_1
gashw_0 gashw_1 airco_0 airco_1 garagepl
prefarea_0
prefarea_1
homestyle_0
homestyle_1
homestyle_2
id
202
53900.0 2520.0
5
2
1
1
0
1
0
0
1
1
0
0
1
1
1
0
0
0
1
31
282
74500.0 4500.0
4
2
1
1
0
1
0
0
1
1
0
0
1
2
1
0
0
0
1
47
78
47000.0 3180.0
4
1
2
0
1
1
0
```
19: AutoML in teradataml

```python
0
1
1
0
0
1
0
1
0
0
1
0
55
139
26500.0 2990.0
2
1
1
1
0
1
0
1
0
1
0
1
0
1
1
0
0
1
0
63
179
74500.0 3180.0
3
2
2
0
1
1
0
1
0
1
0
1
0
2
1
0
0
0
1
79
97
53900.0 8250.0
3
1
1
0
1
1
0
1
0
1
0
1
0
2
1
0
0
0
1
87
322
88500.0 6350.0
3
2
3
0
1
0
1
1
0
1
0
0
1
0
1
0
0
0
1
71
528
106000.0
6325.0
3
1
4
0
1
1
0
1
0
1
0
0
1
1
1
0
1
0
0
39
223
70100.0 4200.0
3
1
2
0
1
1
0
1
0
1
0
1
0
1
1
0
0
0
1
23
427
49500.0 5320.0
2
1
1
0
1
1
0
1
0
1
0
1
0
1
0
1
0
1
0
15
492 rows X 23 columns
Time taken to encode the columns: 14.70 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
Outlier preprocessing ...
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0    lotsize           2.235772
1   garagepl           2.235772
2    bathrms           0.203252
3    stories           7.113821
4      price           2.439024
5   bedrooms           2.235772
```
19: AutoML in teradataml

```python
Deleting rows of these columns:
['price', 'bathrms', 'garagepl', 'bedrooms', 'lotsize', 'stories']
Sample of dataset after removing outlier rows:
sn
price
lotsize bedrooms
bathrms stories driveway_0
driveway_1
recroom_0
recroom_1
fullbase_0
fullbase_1
gashw_0 gashw_1 airco_0 airco_1 garagepl
prefarea_0
prefarea_1
homestyle_0
homestyle_1
homestyle_2
id
282
74500.0 4500.0
4
2
1
1
0
1
0
0
1
1
0
0
1
2
1
0
0
0
1
47
139
26500.0 2990.0
2
1
1
1
0
1
0
1
0
1
0
1
0
1
1
0
0
1
0
63
322
88500.0 6350.0
3
2
3
0
1
0
1
1
0
1
0
0
1
0
1
0
0
0
1
71
179
74500.0 3180.0
3
2
2
0
1
1
0
1
0
1
0
1
0
2
1
0
0
0
1
79
15
37000.0 3600.0
2
1
1
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
95
280
52000.0 5960.0
3
1
2
0
1
0
1
0
1
1
0
1
0
0
1
0
0
0
1
103
97
53900.0 8250.0
3
1
1
0
1
1
0
1
0
1
0
1
0
2
1
0
0
0
1
87
78
47000.0 3180.0
4
1
2
0
1
1
0
0
1
1
0
0
1
0
1
0
0
1
0
55
223
70100.0 4200.0
3
1
2
0
1
1
0
1
0
1
0
1
0
1
1
0
0
0
1
23
427
49500.0 5320.0
2
1
1
0
1
1
0
1
0
1
0
1
0
1
0
1
0
1
0
15
421 rows X 23 columns
Time Taken by Outlier processing: 32.93 sec
Feature selection using lasso ...
```
19: AutoML in teradataml

```python
feature selected by lasso:
['airco_0', 'homestyle_1', 'homestyle_2', 'prefarea_0', 'recroom_0', 'sn',
'bathrms', 'gashw_0', 'airco_1', 'driveway_0', 'homestyle_0', 'garagepl',
'bedrooms', 'gashw_1', 'stories', 'fullbase_0', 'fullbase_1', 'lotsize']
Total time taken by feature selection: 1.36 sec
scaling Features of lasso data ...
columns that will be scaled:
['sn', 'bathrms', 'garagepl', 'bedrooms', 'stories', 'lotsize']
Dataset sample after scaling:
airco_0 homestyle_1
gashw_1 homestyle_2
prefarea_0
id
homestyle_0
recroom_0
price
gashw_0 fullbase_0
airco_1
driveway_0
fullbase_1
sn
bathrms garagepl
bedrooms
stories lotsize
1
0
0
1
1
10
0
1
54500.0 1
0
0
1
1
-1.4054622327473343
1.7271115861320958
-0.7575814075373921
-1.3175958030479806
-0.99
13469784128766
-0.9180289424247704
0
0
0
1
1
12
0
1
63900.0 1
0
1
0
1
-1.146424878727206
-0.5101621915959424
0.5442217458227795
-1.3175958030479806
-0.9913469784128766
0.8787510949440291
0
0
0
1
1
13
0
1
99000.0 1
0
1
0
1
0.4466548484965834
1.7271115861320958
0.5442217458227795
0.2063268153124581
0.5895510439803847
2.289307385962526
1
1
0
0
1
14
0
1
48000.0 1
1
0
0
0
-1.2694676218867669
-0.5101621915959424
-0.7575814075373921
-1.3175958030479806
0.5895510439803847
-0.3750767192946347
0
0
0
1
1
16
0
1
87000.0 1
1
1
0
0
1.6317507431386706
-0.5101621915959424
1.8460248991829513
0.2063268153124581
2.170449066373646
2.004957149519115
0
0
0
1
1
17
0
1
57000.0 1
0
1
1
1
-1.0104302678666386
1.7271115861320958
-0.7575814075373921
0.2063268153124581
0.5895510439803847
-0.1623737865220042
1
1
0
0
0
15
0
1
49500.0 1
1
0
0
0
1.1007241673974075
-0.5101621915959424
```
19: AutoML in teradataml

```python
0.5442217458227795
-1.3175958030479806
-0.9913469784128766
0.2966167526189352
1
0
0
1
1
11
0
1
80000.0 1
1
0
0
0
-0.8744356570060712
1.
1115861320958
0.5442217458227795
1.7302494336728969
0.5895510439803847
3.1960935730458453
1
1
0
0
1
9
0
1
50000.0 1
1
0
0
0
0.051622883615887676
-0.5101621915959424
0.5442217458227795
-1.3175958030479806
-0.9913469784128766
-0.64
37541080600627
1
0
0
1
1
8
0
1
58000.0 1
1
0
0
0
-0.4794036921253754
-0.5101621915959424
-0.7575814075373921
0.2063268153124581
-0.9913469784128766
-0.2519329161104802
421 rows X 20 columns
Total time taken by feature scaling: 39.71 sec
Feature selection using rfe ...
feature selected by RFE:
['homestyle_1', 'homestyle_2', 'sn', 'bathrms', 'airco_1', 'homestyle_0',
'garagepl', 'bedrooms', 'stories', 'lotsize']
Total time taken by feature selection: 69.74 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_sn', 'r_bathrms', 'r_garagepl', 'r_bedrooms', 'r_stories', 'r_lotsize']
Dataset sample after scaling:
r_homestyle_2
id
price
r_homestyle_0
r_airco_1
r_homestyle_1
r_sn
r_bathrms
r_garagepl
r_bedrooms
r_stories
r_lotsize
1
10
54500.0 0
0
0
-1.4054622327473343
1.
1115861320958
-0.7575814075373921
-1.3175958030479806
-0.99
13469784128766
-0.9180289424247704
1
12
63900.0 0
1
0
-1.146424878
206
-0.5101621915959424
0.5442217458227795
-1.3175958030479806
-0.9913469784128766
0.8787510949440291
1
13
99000.0 0
1
0
0.4466548484965834
```
19: AutoML in teradataml

```python
1.7271115861320958
0.5442217458227795
0.2063268153124581
0.5895510439803847
2.289307385962526
0
14
48000.0 0
0
1
-1.2694676218867669
-0.5101621915959424
-0.7575814075373921
-1.3175958030479806
0.5895510439803847
-0.3750767192946347
1
16
87000.0 0
1
0
1.6317507431386706
-0.5101621915959424
1.8460248991829513
0.2063268153124581
2.170449066373646
2.004957149519115
1
17
57000.0 0
1
0
-1.0104302678666386
1.7271115861320958
-0.7575814075373921
0.2063268153124581
0.5895510439803847
-0.1623737865220042
0
15
49500.0 0
0
1
1.1007241673974075
-0.5101621915959424
0.5442217458227795
-1.3175958030479806
-0.9913469784128766
0.2966167526189352
1
11
80000.0 0
0
0
-0.8744356570060712
1.7271115861320958
0.5442217458227795
1.7302494336
969
0.5895510439803847
3.1960935730458453
0
9
50000.0 0
0
1
0.051622883615887676
-0.5101621915959424
0.5442217458227795
-1.3175958030479806
-0.9913469784128766
-0.64
37541080600627
1
8
58000.0 0
0
0
-0.4794036921253754
-0.5101621915959424
-0.7575814075373921
0.2063268153124581
-0.9913469784128766
-0.2519329161104802
421 rows X 12 columns
Total time taken by feature scaling: 29.92 sec
scaling Features of pca data ...
columns that will be scaled:
['sn', 'lotsize', 'bedrooms', 'bathrms', 'stories', 'garagepl']
Dataset sample after scaling:
recroom_1
airco_0 homestyle_1
gashw_1 homestyle_2
driveway_1
prefarea_0
prefarea_1
id
homestyle_0
recroom_0
price
gashw_0 fullbase_0
airco_1 driveway_0
fullbase_1
sn
lotsize bedrooms
bathrms stories garagepl
0
1
1
0
0
1
1
0
9
0
1
50000.0 1
1
0
0
0
0.051622883615887676
-0.6437541080600625
-1.317595803047977
-0.51
01621915959411
-0.9913469784128749
0.5442217458227792
```
19: AutoML in teradataml

```python
0
1
0
0
1
0
1
0
10
0
1
54500.0 1
0
0
1
1
-1.4054622327473343
-0.9180289424247702
-1.317595803047977
1.7271115861320911
-0.9913469784128749
-0.7575814075373916
0
1
0
0
1
1
1
0
26
0
1
52000.0 1
0
0
0
1
-0.36931281666682086
-0.6829362272550208
0.20632681531245756
-0.5101621915959411
0.5895510439803837
-0.7575814075373916
0
1
1
0
0
1
0
1
15
0
1
49500.0 1
1
0
0
0
1.1007241673974075
0.29661675261893516
-1.317595803047977
-0.5101621915959411
-0.99
13469784128749
0.5442217458227792
0
1
0
0
1
1
1
0
11
0
1
80000.0 1
1
0
0
0
-0.8744356570060712
3.196093573045845
1.7302494336728922
1.7271115861320911
0.5895510439803837
0.5442217458227792
0
0
0
0
0
1
0
1
27
1
1
120000.0
1
0
1
0
1
1.236718778257975
0.39737077340597066
1.7302494336728922
1.7271115861320911
0.5895510439803837
0.5442217458227792
0
1
0
0
1
1
1
0
8
0
1
58000.0 1
1
0
0
0
-0.4794036921253754
-0.25193291611048013
0.20632681531245756
-0.5101621915959411
-0.9913469784128749
-0.75
75814075373916
0
0
0
0
1
1
1
0
16
0
1
87000.0 1
1
1
0
0
1.6317507431386706
2.004957149519114
0.20632681531245756
-0.5101621915959411
2.170449066373642
1.84602489918295
0
1
1
0
0
1
1
0
14
0
1
48000.0 1
1
0
0
0
-1.2694676218867669
-0.3750767192946346
-1.317595803047977
-0.5101621915959411
0.5895510439803837
-0.7575814075373916
0
1
1
0
0
1
1
0
22
0
1
27000.0 1
1
0
0
0
-0.0843717272446797
-0.6387164070207108
-1.317595803047977
-0.5101621915959411
-0.9913469784128749
-0.7575814075373916
421 rows X 23 columns
Total time taken by feature scaling: 40.80 sec
Dimension Reduction using pca ...
```
19: AutoML in teradataml

```python
PCA columns:
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5', 'col_6', 'col_7',
'col_8', 'col_9', 'col_10']
Total time taken by PCA: 5.85 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Hyperparameters used for model training:
response_column :
price
name : xgboost
model_type : Regression
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1, 0.2, 0.3)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.01, 0.05, 0.1)
max_depth : (5, 3, 4, 7, 8)
min_node_size : (1, 2, 3, 4)
iter_num : (10, 20, 30, 40)
seed : 42
Total number of models for xgboost : 10240
response_column : price
name : decision_forest
tree_type : Regression
min_impurity : (0.0, 0.1, 0.2, 0.3)
max_depth : (5, 3, 4, 7, 8)
min_node_size : (1, 2, 3, 4)
num_trees : (-1, 20, 30, 40)
seed : 42
Total number of models for decision_forest : 320
```
19: AutoML in teradataml

```python
Performing hyperparameter tuning ...
xgboost
decision_forest
Leaderboard
RANK
MODEL_ID
FEATURE_SELECTION
MAE
MSE
MSLE
MAPE
MPE
RMSE
RMSLE
ME
R2
EV
MPD
MGD
ADJUSTED_R2
0
1
XGBOOST_0
lasso
6640.729539
8.381567e+07
0.024194
11.699538
-2.303463
9155.089728
0.155546
32011.316254
0.767821
0.768248
1350.223253
0.023587
0.757425
1
2
XGBOOST_3
lasso
6640.729539
8.381567e+07
0.024194
11.699538
-2.303463
9155.089728
0.155546
32011.316254
0.767821
0.768248
1350.223253
0.023587
0.757425
2
3
DECISIONFOREST_0
lasso
7217.464057
8.961653e+07
0.023486
12.169350
-1.641086
9466.600977
0.153251
27811.538462
0.751752
0.751758
1382.746065
0.023525
0.740636
3
4
DECISIONFOREST_1
rfe
7172.322420
9.241116e+07
0.024042
12.126098
-1.475958
9613.072566
0.155054
27811.538462
0.744010
0.744033
1417.341141
0.023941
0.737767
4
5
XGBOOST_1
rfe
6930.716300
9.267134e+07
0.025747
11.955202
-1.639779
9626.595436
0.160457
34372.991548
0.743290
0.743305
1471.137222
0.025598
0.737028
5
6
DECISIONFOREST_3
lasso
8444.172549
1.271166e+08
0.030063
14.331596
-4.661477
11274.601114
0.173388
36115.000000
0.647872
0.649860
1857.379849
0.029634
0.632105
6
7
DECISIONFOREST_2
pca
8539.230818
1.507301e+08
0.033673
14.426075
-5.668495
12277.218445
0.183501
42170.000000
0.582460
0.592381
2143.056974
0.032648
```
19: AutoML in teradataml

```python
0.571230
7
8
XGBOOST_2
pca
8343.664629
1.559985e+08
0.037137
14.679852
-5.005181
12489.934338
0.192711
51690.267244
0.567866
0.574629
2232.426199
0.034528
0.556244
8 rows X 16 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 15/15
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 492
Total Columns in the data: 14
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
sn
INTEGER 492
0
None
0
492
0
0.0
100.0
recroom VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
garagepl
INTEGER 492
0
None
270
222
0
0.0
100.0
fullbase
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
gashw
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
price
FLOAT
492
0
None
0
492
0
0.0
100.0
bathrms INTEGER 492
0
None
0
492
0
0.0
100.0
prefarea
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
airco
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
stories INTEGER 492
0
None
0
492
0
0.0
100.0
bedrooms
INTEGER 492
0
None
0
492
0
0.0
```
19: AutoML in teradataml

```python
100.0
lotsize FLOAT
492
0
None
0
492
0
0.0
100.0
homestyle
VARCHAR(20) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
driveway
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
Statistics of Data:
func
sn
price
lotsize bedrooms
bathrms stories garagepl
min
1
25000
1650
1
1
1
0
std
159.501 26472.496
2182.443
0.731
0.51
0.861
0.854
25%
132.5
49975
3600
2
1
1
0
50%
274
62000
4616
3
1
2
0
75%
413.25
82000
6370
3
2
2
1
max
546
190000
16200
6
4
4
3
mean
272.943 68100.396
5181.795
2.965
1.293
1.803
0.685
count
492
492
492
492
492
492
492
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
driveway                  2
recroom                   2
fullbase                  2
gashw                     2
airco                     2
prefarea                  2
homestyle                 3
No Futile columns found.
Target Column Distribution:
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0    stories           7.113821
1   bedrooms           2.235772
2    bathrms           0.203252
3   garagepl           2.235772
4    lotsize           2.235772
5      price           2.439024
```
19: AutoML in teradataml

```python
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.55 sec
Handling less significant features from data ...
Analysis indicates all categorical columns are significant. No action
Needed.
Total time to handle less significant features: 19.75 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
Checking Missing values in dataset ...
Analysis Completed. No Missing Values
Detected.
Total time to find missing values in data: 8.82 sec
Imputing Missing Values ...
Analysis completed. No imputation
required.
Time taken to perform imputation: 0.00 sec
Performing encoding for categorical columns ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713814119199335"'8
ONE HOT Encoding these Columns:
['driveway', 'recroom', 'fullbase', 'gashw', 'airco', 'prefarea',
'homestyle']
Sample of dataset after performing one hot encoding:
```
19: AutoML in teradataml

```python
sn
price
lotsize bedrooms
bathrms stories driveway_0
driveway_1
recroom_0
recroom_1
fullbase_0
fullbase_1
gashw_0 gashw_1 airco_0 airco_1 garagepl
prefarea_0
prefarea_1
homestyle_0
homestyle_1
homestyle_2
id
242
52000.0 3000.0
2
1
2
0
1
1
0
1
0
1
0
0
1
0
1
0
0
0
1
28
425
65500.0 3840.0
3
1
2
0
1
1
0
1
0
1
0
1
0
1
0
1
0
0
1
44
118
94500.0 4000.0
3
2
2
0
1
1
0
0
1
1
0
0
1
1
1
0
0
0
1
52
240
30000.0 3000.0
4
1
2
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
60
362
145000.0
8580.0
4
3
4
0
1
1
0
1
0
1
0
0
1
2
0
1
1
0
0
76
259
33500.0 3640.0
2
1
1
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
84
505
71500.0 8150.0
3
2
1
0
1
0
1
0
1
1
0
1
0
0
1
0
0
0
1
68
507
75000.0 9800.0
4
2
2
0
1
0
1
1
0
1
0
1
0
2
1
0
0
0
1
36
345
88000.0 4500.0
3
1
4
0
1
1
0
1
0
1
0
0
1
0
1
0
0
0
1
20
80
63900.0 6360.0
2
1
1
0
1
1
0
0
1
1
0
0
1
1
1
0
0
0
1
12
492 rows X 23 columns
Time taken to encode the columns: 14.51 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
```
19: AutoML in teradataml

```python
Spliting of dataset into training and testing ...
Training size :
0.8
Testing size  :
0.2
Training data sample
sn
price
lotsize bedrooms
bathrms stories driveway_0
driveway_1
recroom_0
recroom_1
fullbase_0
fullbase_1
gashw_0 gashw_1 airco_0 airco_1 garagepl
prefarea_0
prefarea_1
homestyle_0
homestyle_1
homestyle_2
id
40
54500.0 3150.0
2
2
1
1
0
1
0
0
1
1
0
1
0
0
1
0
0
0
1
10
122
80000.0 10500.0 4
2
2
0
1
1
0
1
0
1
0
1
0
1
1
0
0
0
1
11
387
83900.0 11460.0 3
1
3
0
1
1
0
1
0
1
0
1
0
2
0
1
0
0
1
19
326
99000.0 8880.0
3
2
2
0
1
1
0
0
1
1
0
0
1
1
1
0
0
0
1
13
61
48000.0 4120.0
2
1
2
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
14
244
27000.0 3649.0
2
1
1
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
22
427
49500.0 5320.0
2
1
1
0
1
1
0
1
0
1
0
1
0
1
0
1
0
1
0
15
223
70100.0 4200.0
3
1
2
0
1
1
0
1
0
1
0
1
0
1
1
0
0
0
1
23
265
50000.0 3640.0
2
1
1
0
1
1
0
1
0
1
0
1
0
1
1
0
0
1
0
9
101
57000.0 4500.0
3
2
2
1
0
1
0
0
1
1
0
0
1
0
1
0
0
0
1
17
393 rows X 23 columns
```
19: AutoML in teradataml

```python
Testing data sample
sn
price
lotsize bedrooms
bathrms stories driveway_0
driveway_1
recroom_0
recroom_1
fullbase_0
fullbase_1
gashw_0 gashw_1 airco_0 airco_1 garagepl
prefarea_0
prefarea_1
homestyle_0
homestyle_1
homestyle_2
id
385
78000.0 6600.0
4
2
2
0
1
0
1
0
1
1
0
1
0
0
0
1
0
0
1
29
284
45000.0 6750.0
2
1
1
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
25
354
86000.0 6800.0
2
1
1
0
1
0
1
0
1
1
0
1
0
2
1
0
0
0
1
121
488
44100.0 8100.0
2
1
1
0
1
1
0
1
0
1
0
1
0
1
1
0
0
1
0
30
202
53900.0 2520.0
5
2
1
1
0
1
0
0
1
1
0
0
1
1
1
0
0
0
1
31
32
48000.0 3500.0
4
1
2
0
1
1
0
1
0
1
0
0
1
2
1
0
0
1
0
127
448
120000.0
5500.0
4
2
2
0
1
1
0
0
1
1
0
0
1
1
0
1
1
0
0
27
91
47000.0 6060.0
3
1
1
0
1
0
1
0
1
1
0
1
0
0
1
0
0
1
0
123
242
52000.0 3000.0
2
1
2
0
1
1
0
1
0
1
0
0
1
0
1
0
0
0
1
28
379
84000.0 7160.0
3
1
1
0
1
1
0
0
1
1
0
1
0
2
0
1
0
0
1
124
99 rows X 23 columns
Time taken for spliting of data: 10.83 sec
Outlier preprocessing ...
Columns with outlier
percentage :-
```
19: AutoML in teradataml

```python
ColumnName  OutlierPercentage
0    stories           7.113821
1   bedrooms           2.235772
2   garagepl           2.235772
3    bathrms           0.203252
4    lotsize           2.235772
5      price           2.439024
Deleting rows of these columns:
['price', 'bathrms', 'garagepl', 'bedrooms', 'lotsize', 'stories']
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713813731333800"'8
Sample of training dataset after removing outlier rows:
sn
price
lotsize bedrooms
bathrms stories driveway_0
driveway_1
recroom_0
recroom_1
fullbase_0
fullbase_1
gashw_0 gashw_1 airco_0 airco_1 garagepl
prefarea_0
prefarea_1
homestyle_0
homestyle_1
homestyle_2
id
278
65500.0 4000.0
3
1
2
0
1
1
0
1
0
1
0
1
0
1
1
0
0
0
1
74
175
50000.0 3036.0
3
1
2
0
1
1
0
0
1
1
0
1
0
0
1
0
0
1
0
98
478
88500.0 5500.0
3
2
1
0
1
0
1
0
1
1
0
1
0
2
0
1
0
0
1
130
415
52000.0 2850.0
3
2
2
1
0
1
0
0
1
1
0
1
0
0
0
1
0
0
1
154
455
74900.0 6050.0
3
1
1
0
1
1
0
0
1
1
0
1
0
0
0
1
0
0
1
178
127
117000.0
5960.0
3
3
2
0
1
0
1
0
1
1
0
1
0
1
1
0
1
0
0
194
7
66000.0 3880.0
3
2
2
0
1
1
0
0
1
1
0
1
0
2
1
0
0
0
1
162
461
47600.0 2145.0
3
1
2
0
1
1
0
0
1
1
0
1
0
0
0
1
0
1
0
90
423
61100.0 3400.0
3
1
2
0
1
1
0
0
1
1
0
1
0
2
0
1
0
```
19: AutoML in teradataml

```python
0
1
58
57
25245.0 2400.0
3
1
1
1
0
1
0
1
0
1
0
1
0
0
1
0
0
1
0
34
192 rows X 23 columns
Time Taken by Outlier processing: 33.67 sec
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713814511698109"'8
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713818540523806"'
Feature selection using lasso ...
feature selected by lasso:
['stories', 'prefarea_1', 'sn', 'bathrms', 'fullbase_0', 'recroom_0',
'homestyle_1', 'recroom_1', 'garagepl', 'driveway_0', 'prefarea_0',
'fullbase_1', 'airco_1', 'driveway_1', 'homestyle_0', 'lotsize']
Total time taken by feature selection: 0.97 sec
scaling Features of lasso data ...
columns that will be scaled:
['stories', 'sn', 'bathrms', 'garagepl', 'lotsize']
Training dataset sample after scaling:
fullbase_0
recroom_1
prefarea_0
driveway_0
fullbase_1
prefarea_1
airco_1 driveway_1
price
homestyle_0
recroom_0
homestyle_1
id
stories sn
bathrms garagepl
lotsize
0
0
1
0
1
0
0
1
44700.0 0
1
1
426
-1.1754677333050345
-0.6087119947083474
-0.5026028286234501
-0.7628533219155937
0.26705222388917843
1
0
1
1
0
0
0
0
38000.0 0
1
1
133
-1.1754677333050345
-0.6986239543619377
-0.5026028286234501
-0.7628533219155937
-1.3291743663387832
1
0
1
0
0
0
1
1
84000.0 0
1
0
136
1.8337296639558538
1.6519315622962105
1.6905731508243313
-0.7628533219155937
0.7904478921368461
1
0
1
0
0
0
0
1
65000.0 0
1
0
```
19: AutoML in teradataml

```python
108
-1.1754677333050345
-0.9426707019931115
-0.5026028286234501
1.8067578676948275
-0.6365213924388846
0
0
0
0
1
1
1
1
73000.0 0
1
0
182
-1.1754677333050345
1.0996152387098697
-0.5026028286234501
-0.7628533219155937
0.5821312082571773
1
0
0
0
0
1
0
1
60000.0 0
1
0
512
1.8337296639558538
1.2152163296930574
-0.5026028286234501
0.5219522728896168
-1.477600003603047
1
0
1
0
0
0
0
1
62000.0 0
1
0
192
1.8337296639558538
-0.8591810251719205
1.6905731508243313
0.5219522728896168
-0.2511355272614975
1
0
1
0
0
0
0
1
70000.0 0
1
0
72
1.8337296639558538
0.38031956148114676
-0.5026028286234501
-0.7628533219155937
-1.04
79468431012304
1
1
0
0
0
1
1
1
78000.0 0
0
0
285
1.8337296639558538
0.4830760867995358
-0.5026028286234501
-0.7628533219155937
0.5821312082571773
0
1
0
0
1
1
0
1
88500.0 0
0
0
130
-1.1754677333050345
1.2601723095198525
1.6905731508243313
1.8067578676948275
0.2696561824376743
192 rows X 18 columns
Testing dataset sample after scaling:
fullbase_0
recroom_1
prefarea_0
driveway_0
fullbase_1
prefarea_1
airco_1 driveway_1
price
homestyle_0
recroom_0
homestyle_1
id
stories sn
bathrms garagepl
lotsize
1
0
1
0
0
0
0
1
87250.0 0
1
0
492
1.8337296639558538
-0.36466524707717346
-0.5026028286234501
0.5219522728896168
-0.907333081482454
1
1
1
0
0
0
1
1
103000.0
1
0
0
368
3.338328362586298
1.536330471313023
1.6905731508243313
0.5219522728896168
1.4049821095818689
0
0
0
0
1
1
0
1
93000.0 0
1
0
454
1.8337296639558538
1.2409054610226546
-0.5026028286234501
-0.7628533219155937
0.8789824827857053
1
0
1
0
0
0
0
1
30000.0 0
1
1
247
-1.1754677333050345
```
19: AutoML in teradataml

```python
0.046360854196382535
-0.5026028286234501
0.5219522728896168
-0.8448380763185533
1
0
0
0
0
1
1
1
100500.0
1
1
0
205
1.8337296639558538
0.5858326121179247
1.6905731508243313
1.8067578676948275
0.7175370527789621
0
1
1
1
1
0
0
0
72000.0 0
0
0
211
-1.1754677333050345
-0.46742177239556243
-0.5026028286234501
-0.7628533219155937
-0.7510955685727024
1
0
0
0
0
1
1
1
112000.0
1
1
0
200
3.338328362586298
0.8427239254138973
1.6905731508243313
-0.7628533219155937
0.7175370527789621
0
1
1
0
1
0
1
1
70000.0 0
0
0
451
-1.1754677333050345
0.17480651084436877
-0.5026028286234501
-0.7628533219155937
0.4258936953474258
1
0
1
0
0
0
1
1
120000.0
1
1
0
417
3.338328362586298
1.6069755824694154
-0.5026028286234501
1.8067578676948275
1.050843746986432
1
0
1
1
0
0
0
0
41000.0 0
1
1
187
-1.1754677333050345
-1.507831591244251
-0.5026028286234501
-0.7628533219155937
-1.0114914234222883
99 rows X 18 columns
Total time taken by feature scaling: 48.99 sec
Feature selection using rfe ...
feature selected by RFE:
['sn', 'bathrms', 'homestyle_1', 'garagepl', 'homestyle_2', 'airco_0',
'homestyle_0', 'lotsize']
Total time taken by feature selection: 41.78 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_sn', 'r_bathrms', 'r_garagepl', 'r_lotsize']
Training dataset sample after scaling:
r_homestyle_1
r_airco_0
r_homestyle_2
r_homestyle_0
price
id
```
19: AutoML in teradataml

```python
r_sn
r_bathrms
r_garagepl
r_lotsize
1
1
0
0
44700.0
426
-0.6087119947083474
-0.5026028286234501
-0.7628533219155937
0.26705222388917843
1
1
0
0
38000.0
133
-0.6986239543619377
-0.5026028286234501
-0.7628533219155937
-1.3291743663387832
0
0
1
0
84000.0 136
1.6519315622962105
1.6905731508243313
-0.7628533219155937
0.7904478921368461
0
1
1
0
65000.0
108
-0.9426707019931115
-0.5026028286234501
1.8067578676948275
-0.6365213924388846
0
0
1
0
73000.0 182
1.0996152387098697
-0.5026028286234501
-0.7628533219155937
0.5821312082571773
0
1
1
0
60000.0 512
1.2152163296930574
-0.5026028286234501
0.5219522728896168
-1.477600003603047
0
1
1
0
62000.0 192
-0.8591810251719205
1.6905731508243313
0.5219522728896168
-0.2511355272614975
0
1
1
0
70000.0 72
0.38031956148114676
-0.5026028286234501
-0.7628533219155937
-1.04
79468431012304
0
0
1
0
78000.0 285
0.4830760867995358
-0.5026028286234501
-0.7628533219155937
0.5821312082571773
0
1
1
0
88500.0 130
1.2601723095198525
1.6905731508243313
1.8067578676948275
0.2696561824376743
192 rows X 10 columns
Testing dataset sample after scaling:
r_homestyle_1
r_airco_0
r_homestyle_2
r_homestyle_0
price
id
r_sn
r_bathrms
r_garagepl
r_lotsize
0
1
1
0
87250.0
492
-0.36466524707717346
-0.5026028286234501
0.5219522728896168
-0.907333081482454
0
0
0
1
103000.0
368
1.536330471313023
1.6905731508243313
0.5219522728896168
1.4049821095818689
0
1
1
0
93000.0 454
1.2409054610226546
-0.5026028286234501
-0.7628533219155937
0.8789824827857053
1
1
0
0
30000.0 247
0.046360854196382535
-0.5026028286234501
```
19: AutoML in teradataml

```python
0.5219522728896168
-0.8448380763185533
0
0
0
1
100500.0
205
0.5858326121179247
1.6905731508243313
1.8067578676948275
0.7175370527789621
0
1
1
0
72000.0
211
-0.46742177239556243
-0.5026028286234501
-0.7628533219155937
-0.7510955685727024
0
0
0
1
112000.0
200
0.8427239254138973
1.6905731508243313
-0.7628533219155937
0.7175370527789621
0
0
1
0
70000.0 451
0.17480651084436877
-0.5026028286234501
-0.7628533219155937
0.4258936953474258
0
0
0
1
120000.0
417
1.6069755824694154
-0.5026028286234501
1.8067578676948275
1.050843746986432
1
1
0
0
41000.0
187
-1.507831591244251
-0.5026028286234501
-0.7628533219155937
-1.0114914234222883
99 rows X 10 columns
Total time taken by feature scaling: 39.84 sec
scaling Features of pca data ...
columns that will be scaled:
['sn', 'lotsize', 'bathrms', 'stories', 'garagepl']
Training dataset sample after scaling:
recroom_0
recroom_1
prefarea_0
driveway_0
bedrooms
prefarea_1
fullbase_1
airco_1 homestyle_2
gashw_0 driveway_1
gashw_1 airco_0 homestyle_0
fullbase_0
price
homestyle_1
id
sn
lotsize bathrms stories garagepl
1
0
1
1
3
0
1
1
1
1
0
0
0
0
0
57000.0 0
17
-1.1610283182946874
-0.25113552726149785
1.690573150824329
0.32913096532540864
-0.7628533219155942
1
0
0
0
3
1
0
0
1
1
1
0
1
0
1
65500.0 0
44
0.9197913194026884
-0.5948580556629517
-0.5026028286234494
0.32913096532540864
0.5219522728896171
1
0
1
0
3
0
1
1
1
1
1
0
0
0
0
94500.0 0
52
-1.0518495101438992
-0.5115313821110842
1.690573150824329
0.32913096532540864
0.5219522728896171
```
19: AutoML in teradataml

```python
1
0
1
0
3
0
0
0
1
1
1
0
1
0
1
70100.0 0
23
-0.3775098127419719
-0.40737304017124965
-0.5026028286234494
0.32913096532540864
0.5219522728896171
1
0
1
0
3
0
0
0
0
1
1
0
1
0
1
35500.0 1
51
-1.4307641972554586
-0.3032146982314151
-0.5026028286234494
0.32913096532540864
-0.7628533219155942
1
0
1
0
3
0
0
1
1
1
1
0
0
0
1
98000.0 0
59
0.2711407533303583
0.5300520372872609
-0.5026028286234494
-1.175467733305031
0.5219522728896171
1
0
1
0
3
0
0
0
1
1
1
0
1
0
1
56000.0 0
38
-0.9041370049987152
-1.0323230918102566
-0.5026028286234494
0.32913096532540864
-0.7628533219155942
0
1
0
0
3
1
1
0
1
1
1
0
1
0
0
86000.0 0
46
0.7977679455871016
0.9987645760165162
1.690573150824329
-1.175467733305031
-0.7628533219155942
1
0
1
0
3
0
1
1
1
1
1
0
0
0
0
99000.0 0
13
0.28398531899515694
2.029932161220878
1.690573150824329
0.32913096532540864
0.5219522728896171
1
0
1
0
3
0
0
0
1
0
1
1
1
0
1
60000.0 0
21
0.14911737951477144
0.42589369534742644
-0.5026028286234494
-1.175467733305031
1.8067578676948284
192 rows X 23 columns
Testing dataset sample after scaling:
recroom_0
recroom_1
prefarea_0
driveway_0
bedrooms
prefarea_1
fullbase_1
airco_1 homestyle_2
gashw_0 driveway_1
gashw_1 airco_0 homestyle_0
fullbase_0
price
homestyle_1
id
sn
lotsize bathrms stories garagepl
1
0
1
0
3
0
1
0
1
1
1
0
1
0
0
52000.0 0
26
-0.525222317887156
-0.7354718172817283
-0.5026028286234494
0.32913096532540864
-0.7628533219155942
1
0
0
0
4
1
1
1
0
1
1
0
0
1
0
120000.0
0
27
```
19: AutoML in teradataml

```python
1.0675038245478725
0.2696561824376747
1.690573150824329
0.32913096532540864
0.5219522728896171
0
1
1
0
3
0
1
0
0
1
1
0
1
0
0
47000.0 1
123
-1.2252511466186806
0.5612995398692113
-0.5026028286234494
-1.175467733305031
-0.76
28533219155942
1
0
1
0
2
0
1
0
1
0
1
1
1
0
0
99000.0 0
24
0.5408766322911293
4.2797523471213035
-0.5026028286234494
-1.175467733305031
0.5219522728896171
1
0
1
1
5
0
1
1
1
1
0
0
0
0
0
53900.0 0
31
-0.5123777522223574
-1.2823031124658595
1.690573150824329
-1.175467733305031
0.5219522728896171
1
0
1
0
4
0
0
1
0
1
1
0
0
0
1
48000.0 1
127
-1.6041658337302398
-0.7719272369606704
-0.5026028286234494
0.32913096532540864
1.8067578676948284
1
0
1
0
2
0
0
0
0
1
1
0
1
0
1
44100.0 1
30
1.324395137843845
1.6237146276555232
-0.5026028286234494
-1.175467733305031
0.5219522728896171
1
0
1
1
3
0
0
0
0
1
0
0
1
0
1
42000.0 1
126
-0.8206473281775242
-0.7198480659907531
-0.5026028286234494
0.32913096532540864
0.5219522728896171
1
0
1
0
2
0
0
1
1
1
1
0
0
0
1
52000.0 0
28
-0.255486438926385
-1.0323230918102566
-0.5026028286234494
0.32913096532540864
-0.7628533219155942
1
0
0
0
3
1
1
0
1
1
1
0
1
0
0
84000.0 0
124
0.6243663091123203
1.1341704205383012
-0.5026028286234494
-1.175467733305031
1.8067578676948284
99 rows X 23 columns
Total time taken by feature scaling: 49.69 sec
Dimension Reduction using pca ...
```
19: AutoML in teradataml

```python
PCA columns:
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5', 'col_6', 'col_7',
'col_8', 'col_9']
Total time taken by PCA: 11.21 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Hyperparameters used for model training:
response_column :
price
name : glm
family : GAUSSIAN
lambda1 : (0.001, 0.02, 0.1)
alpha : (0.15, 0.85)
learning_rate : ('invtime', 'constant', 'adaptive')
initial_eta : (0.05, 0.1)
momentum : (0.65, 0.8, 0.95)
iter_num_no_change : (5, 10, 50)
iter_max : (300, 200, 400, 500)
batch_size : (10, 80, 100, 150)
Total number of models for glm : 5184
response_column : price
name : xgboost
model_type : Regression
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1, 0.2, 0.3)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.01, 0.05, 0.1)
max_depth : (5, 3, 4, 7, 8)
min_node_size : (1, 2, 3, 4)
iter_num : (10, 20, 30, 40)
Total number of models for xgboost : 10240
```
19: AutoML in teradataml

```python
response_column : price
name : decision_forest
tree_type : Regression
min_impurity : (0.0, 0.1, 0.2, 0.3)
max_depth : (5, 3, 4, 7, 8)
min_node_size : (1, 2, 3, 4)
num_trees : (-1, 20, 30, 40)
Total number of models for decision_forest : 320
response_column : price
name : svm
model_type : regression
lambda1 : (0.001, 0.02, 0.1)
alpha : (0.15, 0.85)
tolerance : (0.001, 0.01)
learning_rate : ('Invtime', 'Adaptive', 'constant')
initial_eta : (0.05, 0.1)
momentum : (0.65, 0.8, 0.95)
nesterov : True
intercept : True
iter_num_no_change : (5, 10, 50)
local_sgd_iterations  : (10, 20)
iter_max : (300, 200, 400, 500)
batch_size : (10, 80, 100, 150)
Total number of models for svm : 20736
Performing hyperParameter tuning ...
glm
xgboost
```
19: AutoML in teradataml

```python
decision_forest
svm
Evaluating models performance ...
Evaluation completed.
Leaderboard
Rank
Model-ID
Feature-Selection
MAE
MSE
MSLE
RMSE
RMSLE
R2-score
Adjusted R2-score
0
1
XGBOOST_3
lasso
9628.455479
1.483976e+08
0.034632
12181.855382
0.186097
0.783807
0.741623
1
2
XGBOOST_0
lasso
9628.455479
1.483976e+08
0.034632
12181.855382
0.186097
0.783807
0.741623
2
3
GLM_3
lasso
10200.663327
1.775318e+08
0.037768
13324.104150
0.194340
0.741363
0.690898
3
4
DECISIONFOREST_0
lasso
11490.006156
2.148976e+08
0.046107
14659.386825
0.214725
0.686927
0.625840
4
5
GLM_1
rfe
13133.449531
3.101527e+08
0.061828
17611.153178
0.248652
0.548155
0.507991
5
6
GLM_2
pca
12825.056171
3.332267e+08
0.058128
18254.497063
0.241098
0.514540
0.459374
6
7
GLM_0
lasso
14148.183643
3.375269e+08
0.069698
18371.906254
0.264003
0.508275
0.412328
7
8
XGBOOST_2
pca
13922.629788
3.710428e+08
0.064012
19262.470688
0.253006
0.459447
0.398021
8
9
DECISIONFOREST_2
pca
12586.068723
3.734912e+08
0.059352
19325.919884
0.243623
0.455880
0.394049
9
10
DECISIONFOREST_3
lasso
16432.237374
4.326218e+08
0.085381
20799.562257
0.292201
0.369736
0.246758
10
11
XGBOOST_1
rfe
21225.126287
6.860673e+08
0.153932
26192.885820
0.392342
0.000505
-0.088339
11
12
DECISIONFOREST_1
rfe
22215.932941
7.373789e+08
```
19: AutoML in teradataml

```python
0.161533
27154.721120
0.401911
-0.074248
-0.169737
12
13
SVM_0
lasso
66220.929293
5.071762e+09
51.035352
71216.300842
7.143903
-6.388782
-7.830496
13
14
SVM_1
rfe
66243.121212
5.075075e+09
57.353084
71239.561004
7.573182
-6.393609
-7.050819
14
15
SVM_3
lasso
66269.585859
5.078071e+09
61.891356
71260.586170
7.867106
-6.397974
-7.841481
15
16
SVM_2
pca
66271.212121
5.078287e+09
0.000000
71262.102818
0.000000
-6.398289
-7.239004
16 rows X 10 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 18/18
```
4.
* Display model leaderboard.
```python
aml.leaderboard()
RANK
MODEL_ID
FEATURE_SELECTION
MAE
MSE
MSLE
MAPE
MPE
RMSE
RMSLE
ME
R2
EV
MPD
MGD
ADJUSTED_R2
0
1
XGBOOST_0
lasso
6640.729539
8.381567e+07
0.024194
11.699538
-2.303463
9155.089728
0.155546
32011.316254
0.767821
0.768248
1350.223253
0.023587
0.757425
1
2
XGBOOST_3
lasso
6640.729539
8.381567e+07
0.024194
11.699538
-2.303463
9155.089728
0.155546
32011.316254
0.767821
0.768248
1350.223253
0.023587
0.757425
2
3
DECISIONFOREST_0
lasso
7217.464057
8.961653e+07
0.023486
12.169350
-1.641086
9466.600977
0.153251
27811.538462
0.751752
0.751758
1382.746065
0.023525
0.740636
3
4
DECISIONFOREST_1
rfe
7172.322420
9.241116e+07
0.024042
12.126098
-1.475958
9613.072566
0.155054
27811.538462
0.744010
0.744033
1417.341141
0.023941
0.737767
4
5
XGBOOST_1
rfe
6930.716300
9.267134e+07
0.025747
11.955202
-1.639779
9626.595436
0.160457
34372.991548
0.743290
0.743305
1471.137222
0.025598
0.737028
```
19: AutoML in teradataml

```python
5
6
DECISIONFOREST_3
lasso
8444.172549
1.271166e+08
0.030063
14.331596
-4.661477
11274.601114
0.173388
36115.000000
0.647872
0.649860
1857.379849
0.029634
0.632105
6
7
DECISIONFOREST_2
pca
8539.230818
1.507301e+08
0.033673
14.426075
-5.668495
12277.218445
0.183501
42170.000000
0.582460
0.592381
2143.056974
0.032648
0.571230
7
8
XGBOOST_2
pca
8343.664629
1.559985e+08
0.037137
14.679852
-5.005181
12489.934338
0.192711
51690.267244
0.567866
0.574629
2232.426199
0.034528
0.556244
Rank
Model-ID
Feature-Selection
MAE
MSE
MSLE
RMSE
RMSLE
R2-score
Adjusted R2-score
0
1
XGBOOST_3
lasso
9628.455479
1.483976e+08
0.034632
12181.855382
0.186097
0.783807
0.741623
1
2
XGBOOST_0
lasso
9628.455479
1.483976e+08
0.034632
12181.855382
0.186097
0.783807
0.741623
2
3
GLM_3
lasso
10200.663327
1.775318e+08
0.037768
13324.104150
0.194340
0.741363
0.690898
3
4
DECISIONFOREST_0
lasso
11490.006156
2.148976e+08
0.046107
14659.386825
0.214725
0.686927
0.625840
4
5
GLM_1
rfe
13133.449531
3.101527e+08
0.061828
17611.153178
0.248652
0.548155
0.507991
5
6
GLM_2
pca
12825.056171
3.332267e+08
0.058128
18254.497063
0.241098
0.514540
0.459374
6
7
GLM_0
lasso
14148.183643
3.375269e+08
0.069698
18371.906254
0.264003
0.508275
0.412328
7
8
XGBOOST_2
pca
13922.629788
3.710428e+08
0.064012
19262.470688
0.253006
0.459447
0.398021
8
9
DECISIONFOREST_2
pca
12586.068723
3.734912e+08
0.059352
19325.919884
0.243623
0.455880
0.394049
9
10
DECISIONFOREST_3
lasso
16432.237374
4.326218e+08
0.085381
20799.562257
0.292201
0.369736
0.246758
10
11
XGBOOST_1
rfe
21225.126287
6.860673e+08
0.153932
26192.885820
0.392342
0.000505
-0.088339
11
12
DECISIONFOREST_1
rfe
22215.932941
7.373789e+08
0.161533
27154.721120
0.401911
-0.074248
-0.169737
12
13
SVM_0
lasso
66220.929293
5.071762e+09
51.035352
71216.300842
7.143903
-6.388782
-7.830496
13
14
SVM_1
rfe
66243.121212
5.0
75e+09
57.353084
71239.561004
7.573182
-6.393609
-7.050819
14
15
SVM_3
lasso
66269.585859
5.078071e+09
61.891356
```
19: AutoML in teradataml

```python
71260.586170
7.867106
-6.397974
-7.841481
15
16
SVM_2
pca
66271.212121
5.078287e+09
0.000000
71262.102818
0.000000
-6.398289
-7.239004
```
5.
* Display the best performing model.
```python
aml.leader()
RANK  MODEL_ID    FEATURE_SELECTION  MAE        MSE         MSLE
MAPE      MPE       RMSE      RMSLE    ME         R2        EV
MPD       MGD      ADJUSTED_R2
1     XGBOOST_0   lasso              6640.73    8.38e+07    0.0242
11.6995   -2.3035   9155.09   0.1555   32011.32   0.7678    0.7682
1350.22   0.0236   0.7574
Rank
Model-ID
Feature-Selection
MAE
MSE
MSLE
RMSE
RMSLE
R2-score
Adjusted R2-score
0
1
XGBOOST_3
lasso
9628.455479
1.483976e+08
0.034632
12181.855382
0.186097
0.783807
0.741623
```
6.
* Display hyperparameters for trained model.
* a.
    * Display model hyperparameters for rank 2.
```python
aml.model_hyperparameters(rank=2)
{'response_column': 'price',
'name': 'xgboost',
'model_type': 'Regression',
'column_sampling': 1,
'min_impurity': 0.0,
'lambda1': 0.01,
'shrinkage_factor': 0.5,
'max_depth': 5,
'min_node_size': 1,
'iter_num': 20,
'seed': 42,
'persist': False}
```
* b.
    * Display model hyperparameters for rank 6.
```python
aml.model_hyperparameters(rank=6)
{'response_column': 'price',
'name': 'decision_forest',
'tree_type': 'Regression',
```
19: AutoML in teradataml

```python
'min_impurity': 0.0,
'max_depth': 5,
'min_node_size': 1,
'num_trees': 20,
'seed': 42,
'persist': False}
```
7.
* Generate prediction on validation dataset using best performing model.
* Note:
* In the data preparation phase, AutoML generates the validation dataset by splitting the data
* provided during fitting into training and testing sets. AutoML's model training utilizes the training
* data, with the testing data acting as the validation dataset for model evaluation.
```python
prediction = aml.predict()
Following model is being used for generating prediction :
Model ID : XGBOOST_3
Feature Selection Method : lasso
Prediction :
id     Prediction  Confidence_Lower  Confidence_upper     price
0  492   63523.430206      -7990.015251     135036.875663   87250.0
1  368  102759.944778     -17
.673004     223272.562559  103000.0
2  454   77782.022661     -14282.526959     169846.572282   93000.0
3  247   45022.847275      -6566.519982      96612.214533   30000.0
4  205  117354.767915     -17115.856482     251825.392313  100500.0
5  211   56755.264163      -8626.057137     122136.585463   72000.0
6  200  116854.017302     -17656.674664     251364.709267  112000.0
7  451   66682.350190      -8409.325274     141774.025654   70000.0
8  417  107685.061593     -13027.683164     228397.806351  120000.0
9  187   37198.515964      -7152.411
81549.443681   41000.0
Performance Metrics :
MAE           MSE      MSLE       MAPE       MPE
RMSE     RMSLE            ME        R2        EV          MPD       MGD
0  9628.455479  1.483976e+08  0.034632  15.784078 -5.485578  12181.855382
0.186097  52320.270431  0.783807  0.785125  2084.728019  0.033508
prediction
id
Prediction
Confidence_Lower
Confidence_upper
price
26
55320.3275035
-14932.158036819601
125572.8130438196
52000.0
28
68406.69997650001
-3935.3562509474723
140748.75620394747
```
19: AutoML in teradataml

```python
52000.0
29
86185.303965
-12767.850251373035
185138.45818137302
78000.0
30
54504.629284999995
-7394.386681102078
116403.64525110208
44100.0
120
110679.72956949998
-18432.318021989675
239791.77716098964
163000.0
121
71314.912363
-14548.560484102243
157178.38521010225
86000.0
31
61366.627566999996
-10317.129140241195
133050.3842742412
53900.0
27
114092.16751700001
-20268.7281787322
248453.0632127322
120000.0
25
52893.839117999996
-6981.666775837832
112769.34501183782
45000.0
24
80870.8686425
-11200.480495712312
172942.21778071232
99000.0
```
8.
* Generate prediction on validation dataset using third best performing model.
```python
prediction = aml.predict(rank=3)
Following model is being used for generating prediction :
Model ID : GLM_3
Feature Selection Method : lasso
Prediction :
id     prediction     price
0  492   66186.826718   87250.0
1  368  119
.381829  103000.0
2  454   78665.886884   93000.0
3  247   45251.710670   30000.0
4  205  113365.000670  100500.0
5  211   51491.354759   72000.0
6  200  111707.987096  112000.0
7  451   74287.740695   70000.0
8  417  112877.191516  120000.0
9  187   30706.236368   41000.0
Performance Metrics :
MAE           MSE      MSLE       MAPE       MPE         RMSE
RMSLE            ME        R2        EV          MPD       MGD
0  10200.663327  1.7
18e+08  0.037768  16.300732 -5.466382  13324.10415
0.19434  54055.219486  0.741363  0.742799  2391.983615  0.037205
prediction.head()
```
19: AutoML in teradataml

```python
id
prediction
price
26
63880.087955143106
52000.0
28
64887.03364189595
52000.0
29
81838.00634272449
78000.0
30
59020.9083389725
44100.0
120
108944.78051355484
163000.0
121
76648.23791163946
86000.0
31
64051.73639523083
53900.0
27
108332.60522274605
120000.0
25
51235.20813818371
45000.0
24
89398.59783781157
99000.0
```
9.
* Generate prediction on test dataset using best performing model.
```python
prediction = aml.predict(housing_test)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after performing categorical encoding :
sn
price
lotsize bedrooms
bathrms stories driveway_0
driveway_1
recroom_0
recroom_1
fullbase_0
fullbase_1
gashw_0 gashw_1 airco_0 airco_1 garagepl
prefarea_0
prefarea_1
homestyle_0
homestyle_1
homestyle_2
id
53
68000.0 9166.0
2
1
1
0
1
1
0
0
1
1
0
0
1
2
1
0
0
0
1
11
38
67000.0 5170.0
3
1
4
0
1
1
0
1
0
1
0
0
1
0
1
0
0
0
1
12
198
40500.0 4350.0
3
1
2
1
0
1
0
1
0
0
1
1
0
1
1
0
0
1
0
20
301
55000.0 4080.0
2
1
1
0
1
1
0
1
0
1
0
1
0
0
1
0
0
0
1
9
251
48500.0 3450.0
3
1
1
0
1
1
0
0
1
1
0
1
0
2
1
0
0
1
0
14
408
87500.0 6420.0
3
1
3
0
1
1
0
0
1
1
0
1
0
0
0
1
0
0
1
22
255
61000.0 4360.0
4
1
2
0
1
1
0
```
19: AutoML in teradataml

```python
1
0
1
0
1
0
0
1
0
0
0
1
15
16
37900.0 3185.0
2
1
1
0
1
1
0
1
0
1
0
0
1
0
1
0
0
1
0
23
260
41000.0 6000.0
2
1
1
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
10
540
85000.0 7320.0
4
2
2
0
1
1
0
1
0
1
0
1
0
0
1
0
0
0
1
18
46 rows X 23 columns
Performing transformation carried out in data preparation phase ...
Updated dataset after performing Lasso feature selection:
id
airco_0 homestyle_1
homestyle_2
prefarea_0
recroom_0
sn
bathrms gashw_0 airco_1 driveway_0
homestyle_0
garagepl
bedrooms
gashw_1 stories fullbase_0
fullbase_1
lotsize price
18
1
0
1
1
1
540
2
1
0
0
0
0
4
0
2
1
0
7320.0
85000.0
39
1
0
1
0
1
441
1
1
0
0
0
2
3
0
1
1
0
3520.0
51900.0
47
1
1
0
1
1
132
1
1
0
0
0
0
3
0
2
1
0
3850.0
44500.0
9
1
0
1
1
1
301
1
1
0
0
0
0
2
0
1
1
0
4080.0
55000.0
25
1
1
0
1
1
249
1
1
0
0
0
0
2
0
1
1
0
3500.0
44500.0
33
1
0
1
0
1
411
1
1
0
0
0
1
3
0
1
0
1
9000.0
90000.0
23
0
1
0
1
1
16
1
1
1
0
0
0
2
0
1
1
0
3185.0
37900.0
49
0
0
1
1
1
161
1
1
1
0
0
1
3
0
2
1
0
3162.0
63900.0
```
19: AutoML in teradataml

```python
11
0
0
1
1
1
53
1
1
1
0
0
2
2
0
1
0
1
9166.0
68000.0
19
0
0
1
0
1
440
1
1
1
0
0
2
3
0
2
1
0
6862.0
69000.0
46 rows X 20 columns
Updated dataset after performing scaling on Lasso selected features :
airco_0 homestyle_1
gashw_1 homestyle_2
prefarea_0
id
homestyle_0
recroom_0
price
gashw_0 fullbase_0
airco_1
driveway_0
fullbase_1
sn
bathrms garagepl
bedrooms
stories lotsize
1
0
0
1
1
18
0
1
85000.0 1
1
0
0
0
1.83250469250427
1.7271115861320958
-0.7575814075373921
1.7302494336728969
0.5895510439803847
1.4161058724748852
1
0
0
1
0
39
0
1
51900.0 1
1
0
0
0
1.1913872413044524
-0.5101621915959424
1.8460248991829513
0.2063268153124581
-0.9913469784128766
-0.7109234552514196
1
1
0
0
1
47
0
1
44500.0 1
1
0
0
0
-0.809676318501039
-0.5101621915959424
-0.7575814075373921
0.2063268153124581
0.5895510439803847
-0.526207750475188
1
0
0
1
1
9
0
1
55000.0 1
1
0
0
0
0.284
5022340032
-0.5101621915959424
-0.7575814075373921
-1.31
75958030479806
-0.9913469784128766
-0.3974665016917537
1
1
0
0
1
25
0
1
44500.0 1
1
0
0
0
-0.05199205799216366
-0.5101621915959424
-0.7575814075373921
-1.3175958030479806
-0.9913469784128766
-0.7221183464499792
1
0
0
1
0
33
0
1
90000.0 1
0
0
0
1
0.9971092257893561
-0.5101621915959424
0.5442217458227795
0.2063268153124581
-0.9913469784128766
2.356476733153883
0
1
0
0
1
23
0
1
37900.0 1
1
1
0
0
-1.5608846451594112
-0.5101621915959424
-0.7575814075373921
-1.3175958030479806
-0.9913469784128766
-0.8984378828272913
0
0
0
1
1
49
0
1
63900.0 1
1
1
0
0
-0.621874236836446
-0.5101621915959424
```
19: AutoML in teradataml

```python
0.5442217458227795
0.2063268153124581
0.5895510439803847
-0.9113120077056347
0
0
0
1
1
11
0
1
68000.0 1
0
1
0
1
-1.3212750926907926
-0.5101621915959424
1.8460248991829513
-1.3175958030479806
-0.9913469784128766
2.449394330101927
0
0
0
1
0
19
0
1
69000.0 1
1
1
0
0
1.1849113074539492
-0.5101621915959424
1.8460248991829513
0.2063268153124581
0.5895510439803847
1.1597428640278726
46 rows X 20 columns
Updated dataset after performing RFE feature selection:
id
homestyle_1
homestyle_2
sn
bathrms airco_1 homestyle_0
garagepl
bedrooms
stories lotsize price
18
0
1
540
2
0
0
0
4
2
7320.0
85000.0
39
0
1
441
1
0
0
2
3
1
3520.0
51900.0
47
1
0
132
1
0
0
0
3
2
3850.0
44500.0
9
0
1
301
1
0
0
0
2
1
4080.0
55000.0
25
1
0
249
1
0
0
0
2
1
3500.0
44500.0
33
0
1
411
1
0
0
1
3
1
9000.0
90000.0
23
1
0
16
1
1
0
0
2
1
3185.0
37900.0
49
0
1
161
1
1
0
1
3
2
3162.0
63900.0
11
0
1
53
1
1
0
2
2
1
9166.0
68000.0
19
0
1
440
1
1
0
2
3
2
6862.0
69000.0
46 rows X 12 columns
Updated dataset after performing scaling on RFE selected features :
r_homestyle_2
id
price
r_homestyle_0
r_airco_1
r_homestyle_1
r_sn
r_bathrms
r_garagepl
r_bedrooms
r_stories
r_lotsize
1
18
85000.0 0
0
0
1.83250469250427
```
19: AutoML in teradataml

```python
1.7271115861320958
-0.75
14075373921
1.7302494336728969
0.5895510439803847
1.4161058724748852
1
39
51900.0 0
0
0
1.1913872413044524
-0.5101621915959424
1.8460248991829513
0.2063268153124581
-0.9913469784128766
-0.7109234552514196
0
47
44500.0 0
0
1
-0.809676318501039
-0.5101621915959424
-0.75
14075373921
0.2063268153124581
0.5895510439803847
-0.526207750475188
1
9
55000.0 0
0
0
0.2847565022340032
-0.5101621915959424
-0.75
14075373921
-1.31
75958030479806
-0.9913469784128766
-0.3974665016917537
0
25
44500.0 0
0
1
-0.05199205799216366
-0.5101621915959424
-0.75
14075373921
-1.3175958030479806
-0.9913469784128766
-0.7221183464499792
1
33
90000.0 0
0
0
0.9971092257893561
-0.5101621915959424
0.5442217458227795
0.2063268153124581
-0.9913469784128766
2.356476733153883
0
23
37900.0 0
1
1
-1.5608846451594112
-0.5101621915959424
-0.75
14075373921
-1.3175958030479806
-0.9913469784128766
-0.8984378828272913
1
49
63900.0 0
1
0
-0.621874236836446
-0.5101621915959424
0.5442217458227795
0.2063268153124581
0.5895510439803847
-0.9113120077056347
1
11
68000.0 0
1
0
-1.3212750926907926
-0.5101621915959424
1.8460248991829513
-1.3175958030479806
-0.9913469784128766
2.449394330101927
1
19
69000.0 0
1
0
1.1849113074539492
-0.5101621915959424
1.8460248991829513
0.2063268153124581
0.5895510439803847
1.1597428640278726
46 rows X 12 columns
Updated dataset after performing scaling for PCA feature selection :
recroom_1
airco_0 homestyle_1
gashw_1 homestyle_2
driveway_1
prefarea_0
prefarea_1
id
homestyle_0
recroom_0
price
gashw_0 fullbase_0
airco_1 driveway_0
fullbase_1
sn
lotsize bedrooms
bathrms stories garagepl
0
0
0
0
1
1
1
0
11
0
1
68000.0 1
0
1
0
1
-1.3212750926907926
2.449394330101926
-1.317595803047977
-0.5101621915959411
-0.99
13469784128749
1.84602489918295
0
0
0
0
1
1
1
0
29
0
1
64000.0 1
1
1
0
0
0.31713617148651924
```
19: AutoML in teradataml

```python
0.6128724289782409
-1.31
5803047977
-0.5101621915959411
-0.99
13469784128749
0.5442217458227792
0
0
0
0
1
1
1
0
61
0
1
60000.0 1
1
1
0
0
-0.01961238873964762
-0.10639933052920669
0.20632681531245756
1.7271115861320911
0.5895510439803837
0.5442217458227792
0
0
0
0
1
1
1
0
12
0
1
67000.0 1
1
1
0
0
-1.4184141004483406
0.21265506862973893
0.20632681531245756
-0.5101621915959411
3.7513470887669005
-0.7575814075373916
1
0
0
0
1
1
0
1
40
0
0
92500.0 1
0
1
0
1
0.932349887284324
1.4664828828684024
0.20632681531245756
-0.5101621915959411
-0.9913469784128749
1.84602489918295
1
0
0
0
1
1
0
1
32
0
0
77500.0 1
0
1
0
1
0.9453017549853304
1.1390323153105373
0.20632681531245756
-0.5101621915959411
-0.9913469784128749
-0.75
75814075373916
1
1
0
0
1
1
1
0
24
0
0
64900.0 1
0
0
0
1
0.10990628827041656
-0.3862716104931941
-1.31
5803047977
1.7271115861320911
-0.9913469784128749
-0.7575814075373916
0
1
1
0
0
1
1
0
10
0
1
41000.0 1
1
0
0
0
0.019243214363371633
0.677243053369958
-1.31
5803047977
-0.5101621915959411
-0.99
13469784128749
-0.7575814075373916
0
1
0
0
1
1
1
0
18
0
1
85000.0 1
1
0
0
0
1.83250469250427
1.4161058724748847
1.7302494336728922
1.7271115861320911
0.5895510439803837
-0.7575814075373916
0
1
0
0
1
1
1
0
26
0
1
86900.0 1
1
0
0
0
0.6344569301611764
-0.27432269850
916
4.778094670393761
1.7271115861320911
0.5895510439803837
-0.7575814075373916
46 rows X 23 columns
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
col_6
col_7
col_8
col_9
col_10
price
0
24
-0.254411
0.449359
0.798747
2.408292
-0.778557
0.263065
```
19: AutoML in teradataml

```python
0.921262
-0.455394
-0.176837
-0.010335
0.811939
64900.0
1
23
-2.471745
0.174682
0.523237
0.189115
0.430485
0.301048
0.469823
1.219421
0.263411
-0.709902
-0.028935
37900.0
2
10
-1.351342
1.320579
-0.380456
0.467459
0.662415
0.830990
-0.301989
-0.242817
0.332596
-0.356174
-0.008707
41000.0
3
49
-0.277884
-0.718613
0.174583
-1.226129
-0.165300
-0.146566
0.581603
0.980761
-0.570473
-0.229230
0.205543
63900.0
4
18
2.592898
-0.714658
-0.799964
1.026582
0.610593
1.022254
-1.293902
-0.472070
-0.680221
-0.088954
0.382031
85000.0
5
11
0.373379
2.526476
1.940257
-0.817936
1.550564
-0.234555
1.406837
0.256339
-0.101670
0.027882
-0.8
90
68000.0
6
26
2.650478
-3.196239
-0.041318
0.4
76
0.495893
-0.757674
-2.765423
-0.134092
-1.129081
-0.653021
0.625218
86900.0
7
19
2.040151
1.210922
-0.428903
-1.629863
-0.180097
0.230269
0.175858
0.784173
0.172874
0.215821
-0.476107
69000.0
8
39
0.635020
1.350810
0.002427
-1.041711
-1.824893
-0.396504
-0.992050
0.237966
-0.405037
0.017674
-0.105572
51900.0
9
29
-0.279745
1.813835
0.104148
-0.310959
0.225222
0.529495
0.527447
1.031482
-0.570448
-0.039936
0.194111
64000.0
10 rows X 13 columns
Data Transformation completed.⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 10/10
Following model is being picked for evaluation:
Model ID : XGBOOST_0
Feature Selection Method : lasso
Prediction :
id    Prediction  Confidence_Lower  Confidence_upper    price
0  18  69345.452
69345.452
69345.452
85000.0
1  39  57590.606963      57590.606963      57590.606963  51900.0
```
19: AutoML in teradataml

```python
2  47  39957.102008      39957.102008      39957.102008  44500.0
3   9  53556.262102      53556.262102      53556.262102  55000.0
4  25  41374.084969      41374.084969      41374.084969  44500.0
5  33  79590.545472      79590.545472      79590.545472  90000.0
6  23  41494.207470      41494.207470      41494.207470  37900.0
7  49  62129.986607      62129.986607      62129.986607  63900.0
8  11  58577.423542      58577.423542      58577.423542  68000.0
9  19  82470.069541      82470.069541      82470.069541  69000.0
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713818093862300"'
Updated dataset after performing categorical encoding :
sn
price
lotsize bedrooms
bathrms stories driveway_0
driveway_1
recroom_0
recroom_1
fullbase_0
fullbase_1
gashw_0 gashw_1 airco_0 airco_1 garagepl
prefarea_0
prefarea_1
homestyle_0
homestyle_1
homestyle_2
id
260
41000.0 6000.0
2
1
1
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
10
469
55000.0 2176.0
2
1
2
0
1
0
1
1
0
1
0
1
0
0
0
1
0
0
1
8
364
72000.0 10700.0 3
1
2
0
1
0
1
0
1
1
0
1
0
0
1
0
0
0
1
16
53
68000.0 9166.0
2
1
1
0
1
1
0
0
1
1
0
0
1
2
1
0
0
0
1
11
255
61000.0 4360.0
4
1
2
0
1
1
0
1
0
1
0
1
0
0
1
0
0
0
1
15
16
37900.0 3185.0
2
1
1
0
1
1
0
1
0
1
0
0
1
0
1
0
0
1
0
23
251
48500.0 3450.0
3
1
1
0
1
1
0
0
1
1
0
1
0
2
1
0
0
1
0
14
408
87500.0 6420.0
3
1
3
0
1
1
0
0
1
1
0
1
0
0
0
1
0
0
1
22
301
55000.0 4080.0
2
1
1
0
1
1
0
```
19: AutoML in teradataml

```python
1
0
1
0
1
0
0
1
0
0
0
1
9
13
27000.0 1700.0
3
1
2
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
17
Performing transformation carried out in data preparation phase ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713815079036517"'
Updated dataset after performing Lasso feature selection:
id
stories prefarea_1
sn
bathrms fullbase_0
recroom_0
homestyle_1
recroom_1
garagepl
driveway_0
prefarea_0
fullbase_1
airco_1 driveway_1
homestyle_0
lotsize price
48
1
0
25
1
1
1
1
0
0
0
1
0
0
1
0
4960.0
42000.0
44
1
0
239
1
0
1
1
0
2
0
1
1
0
1
0
3000.0
26000.0
39
1
1
441
1
1
1
0
0
2
0
0
0
0
1
0
3520.0
51900.0
51
1
1
443
1
1
1
0
0
0
0
0
0
0
1
0
3520.0
65000.0
33
1
1
411
1
0
1
0
0
1
0
0
1
0
1
0
9000.0
90000.0
25
1
0
249
1
1
1
1
0
0
0
1
0
0
1
0
3500.0
44500.0
12
4
0
38
1
1
1
0
0
0
0
1
0
1
1
0
5170.0
67000.0
67
4
0
317
1
1
1
0
0
0
0
1
0
0
1
0
5000.0
80000.0
22
3
1
408
1
0
1
0
0
0
0
0
1
0
1
0
6420.0
87500.0
75
1
0
294
1
1
1
1
0
0
0
1
0
0
1
0
4040.0
47000.0
Updated dataset after performing scaling on Lasso selected features :
fullbase_0
recroom_1
prefarea_0
driveway_0
fullbase_1
prefarea_1
airco_1 driveway_1
price
homestyle_0
recroom_0
homestyle_1
id
stories sn
bathrms garagepl
lotsize
0
0
0
0
1
1
0
1
87500.0 0
1
0
22
1.8337296639558538
0.8106125112519007
-0.5026028286234501
-0.
8533219155937
0.7487845553609124
0
1
0
0
1
1
1
1
92500.0 0
0
0
40
-1.1754677333050345
```
19: AutoML in teradataml

```python
0.7656565314251055
-0.5026028286234501
1.8067578676948275
1.2643683479630925
1
0
1
0
0
0
0
1
42000.0 0
1
1
48
-1.1754677333050345
-1.6491218135570358
-0.5026028286234501
-0.7628533219155937
-0.011571340799878474
1
0
1
0
0
0
1
1
64000.0 0
1
0
29
-1.1754677333050345
0.15553966234717084
-0.5026028286234501
0.5219522728896168
0.47016099067185546
1
0
0
0
0
1
0
1
51900.0 0
1
0
39
-1.1754677333050345
1.022547844721078
-0.5026028286234501
1.8067578676948275
-0.7615114027666858
1
0
0
0
0
1
0
1
65000.0 0
1
0
51
-1.1754677333050345
1.0353924103858767
-0.5026028286234501
-0.7628533219155937
-0.76
15114027666858
1
0
1
0
0
0
0
1
47000.0 0
1
1
75
-1.1754677333050345
0.0784722683583791
-0.5026028286234501
-0.7628533219155937
-0.49
069971372311655
0
0
0
0
1
1
0
1
90000.0 0
1
0
33
-1.1754677333050345
0.8298793597490987
-0.5026028286234501
0.5219522728896168
2.0924271663847755
1
0
1
0
0
0
0
1
44500.0 0
1
1
25
-1.1754677333050345
-0.21053045909958995
-0.5026028286234501
-0.7628533219155937
-0.7719272369606693
0
0
1
0
1
0
0
1
26000.0 0
1
1
44
-1.1754677333050345
-0.27475328742358307
-0.5026028286234501
1.8067578676948275
-1.0323230918102553
Updated dataset after performing RFE feature selection:
id
sn
bathrms homestyle_1
garagepl
homestyle_2
airco_0 homestyle_0
lotsize price
48
25
1
1
0
0
1
0
4960.0
42000.0
44
239
1
1
2
0
1
0
3000.0
26000.0
39
441
1
0
2
1
1
0
3520.0
51900.0
```
19: AutoML in teradataml

```python
51
443
1
0
0
1
1
0
3520.0
65000.0
33
411
1
0
1
1
1
0
9000.0
90000.0
25
249
1
1
0
0
1
0
3500.0
44500.0
12
38
1
0
0
1
0
0
5170.0
67000.0
67
317
1
0
0
1
1
0
5000.0
80000.0
22
408
1
0
0
1
1
0
6420.0
87500.0
75
294
1
1
0
0
1
0
4040.0
47000.0
Updated dataset after performing scaling on RFE selected features :
r_homestyle_1
r_homestyle_2
r_airco_0
r_homestyle_0
price
id
r_sn
r_bathrms
r_garagepl
r_lotsize
1
0
1
0
42000.0
48
-1.6491218135570358
-0.5026028286234501
-0.7628533219155937
-0.011571340799878474
1
0
1
0
26000.0
44
-0.27475328742358307
-0.5026028286234501
1.8067578676948275
-1.0323230918102553
0
1
1
0
51900.0 39
1.022547844721078
-0.5026028286234501
1.8067578676948275
-0.7615114027666858
0
1
1
0
65000.0 51
1.0353924103858767
-0.5026028286234501
-0.7628533219155937
-0.76
15114027666858
0
1
1
0
90000.0 33
0.8298793597490987
-0.5026028286234501
0.5219522728896168
2.0924271663847755
1
0
1
0
44500.0
25
-0.21053045909958995
-0.5026028286234501
-0.7628533219155937
-0.7719272369606693
0
1
0
0
67000.0
12
-1.5656321367358448
-0.5026028286234501
-0.7628533219155937
0.09779491823694761
0
1
1
0
80000.0 67
0.22618477350356328
-0.5026028286234501
-0.7628533219155937
0.009260327588088398
0
1
1
0
87500.0 22
0.8106125112519007
-0.5026028286234501
-0.7628533219155937
```
19: AutoML in teradataml

```python
0.7487845553609124
1
0
1
0
47000.0 75
0.0784722683583791
-0.5026028286234501
-0.7628533219155937
-0.49
069971372311655
Updated dataset after performing scaling for PCA feature selection :
recroom_0
recroom_1
prefarea_0
driveway_0
bedrooms
prefarea_1
fullbase_1
airco_1 homestyle_2
gashw_0 driveway_1
gashw_1 airco_0 homestyle_0
fullbase_0
price
homestyle_1
id
sn
lotsize bathrms stories garagepl
1
0
0
0
3
1
1
0
1
1
1
0
1
0
0
87500.0 0
22
0.8106125112519003
0.7487845553609134
-0.5026028286234494
1.8337296639558482
-0.7628533219155942
0
1
0
0
3
1
1
1
1
1
1
0
0
0
0
92500.0 0
40
0.
656531425105
1.2643683479630943
-0.5026028286234494
-1.175467733305031
1.8067578676948284
1
0
1
0
2
0
0
0
0
1
1
0
1
0
1
42000.0 1
48
-1.649121813557035
-0.01157134079987849
-0.5026028286234494
-1.175467733305031
-0.7628533219155942
1
0
1
0
2
0
0
1
1
1
1
0
0
0
1
64000.0 0
29
0.15553966234717076
0.4701609906718561
-0.5026028286234494
-1.175467733305031
0.5219522728896171
1
0
0
0
3
1
0
0
1
1
1
0
1
0
1
51900.0 0
39
1.0225478447210774
-0.7615114027666869
-0.5026028286234494
-1.17
5467733305031
1.8067578676948284
1
0
0
0
3
1
0
0
1
1
1
0
1
0
1
65000.0 0
51
1.035392410385876
-0.7615114027666869
-0.5026028286234494
-1.17
5467733305031
-0.7628533219155942
1
0
1
0
2
0
0
0
0
1
1
0
1
0
1
47000.0 1
75
0.07847226835837905
-0.4906997137231172
-0.5026028286234494
-1.17
5467733305031
-0.7628533219155942
1
0
0
0
3
1
1
0
1
1
1
0
1
0
0
90000.0 0
33
0.8298793597490982
2.0924271663847787
-0.5026028286234494
-1.175467733305031
```
19: AutoML in teradataml

```python
0.5219522728896171
1
0
1
0
2
0
0
0
0
1
1
0
1
0
1
44500.0 1
25
-0.21053045909958984
-0.7719272369606704
-0.5026028286234494
-1.175467733305031
-0.7628533219155942
1
0
1
0
2
0
1
0
0
1
1
0
1
0
0
26000.0 1
44
-0.2747532874235829
-1.0323230918102566
-0.5026028286234494
-1.175467733305031
1.8067578676948284
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
col_6
col_7
col_8
col_9
price
0
24
0.121058
-0.088188
1.823276
-1.499059
0.807124
-0.001108
-0.486189
-0.327831
0.999693
0.041307
64900.0
1
12
-1.702851
2.244144
-0.163064
1.531449
-1.726684
1.435539
-0.336577
0.312267
-0.721428
-0.019429
67000.0
2
22
0.436066
1.674569
-1.184835
-0.517098
-0.149681
0.581587
-1.015373
0.346597
-1.125296
-0.054246
87500.0
3
40
2.732152
-1.196533
-0.243570
-0.130603
0.061535
0.992846
0.669891
0.796020
0.182694
0.034230
92500.0
4
67
-1.071889
2.634593
-1.143026
1.133870
-0.666936
0.448109
-1.183424
0.156071
-0.190941
-0.026246
80000.0
5
48
-1.466384
-1.594199
0.440659
-0.192916
-0.727838
-0.698315
-0.370444
0.078220
-0.266739
-0.400643
42000.0
6
29
0.713664
-1.049937
-0.245372
0.286599
-0.760120
-0.026496
0.770101
-0.793006
0.435955
0.026876
64000.0
7
44
-0.084548
-1.910265
0.033862
0.687256
1.538283
0.149991
0.443854
0.136846
0.043304
-1.148533
26000.0
8
39
1.145001
-1.114998
-1.120372
0.694225
1.703701
-0.350166
0.571300
-0.292433
0.199957
0.403042
51900.0
9
51
0.035124
-0.359414
-1.219128
-1.145653
0.772859
-0.721654
0.162539
-0.630366
0.185628
0.517291
65000.0
```
19: AutoML in teradataml

```python
Data Transformation completed.
Following model is being used for generating prediction :
Model ID : XGBOOST_3
Feature Selection Method : lasso
Prediction :
id    Prediction  Confidence_Lower  Confidence_upper    price
0  48  52832.052623      -1130.634446     106794.739693  42000.0
1  44  48048.913899      -1342.559233      97440.387030  26000.0
2  39  59237.625097     -11283.510389     129758.760583  51900.0
3  51  59341.776847     -11013.321662     129696.875356  65000.0
4  33  81820.074617     -14655.044080     178295.193314  90000.0
5  25  47145.112130      -5338.347479      99628.571740  44500.0
6  22  83148.275354     -13682.206803     179978.757511  87500.0
7  12  82823.957607      -8536.222191     174184.137404  67000.0
8  67  76352.427511     -13523.627091     166228.482114  80000.0
9  75  41305.158083      -8937.031699      91547.347865  47000.0
Performance Metrics :
MAE           MSE      MSLE       MAPE       MPE         RMSE
RMSLE            ME        R2        EV          MPD       MGD
0  7530.032113  9.803829e+07  0.032812  14.340231 -6.394165  9901.428629
0.18114  31376.260153  0.695521  0.702174  1675.413575  0.030915
prediction.head()
id
Prediction
Confidence_Lower
Confidence_upper
price
10
43992.446503
43992.446503
43992.446503
41000.0
12
72565.284652
72565.284652
72565.284652
67000.0
13
47280.89498099999
47280.89498099999
47280.89498099999
49000.0
14
44128.888049999994
44128.888049999994
44128.888049999994
48500.0
16
76661.89054699999
76661.89054699999
76661.89054699999
72000.0
17
31961.054944
31961.054944
31961.054944
27000.0
15
61180.928922
61180.928922
61180.928922
61000.0
11
58577.423542
58577.423542
58577.423542
68000.0
9
53556.26210199999
53556.26210199999
53556.26210199999
55000.0
8
63626.498983
63626.498983
63626.498983
55000.0
```
19: AutoML in teradataml

```python
id
Prediction
Confidence_Lower
Confidence_upper
price
10
50847.987966999994
-2114.7423931209414
103810.71832712094
41000.0
12
82823.9576065
-8536.222191293302
174184.1374042933
67000.0
13
42366.405568999995
-7506.190055977691
92239.00119397769
49000.0
14
50464.071062999996
-2300.8744488123702
103229.01657481235
48500.0
16
72076.95612349999
-17375.200759916668
161529.11300691665
72000.0
17
30842.594055999998
-5288.270194834851
66973.45830683484
27000.0
15
62003.993956999984
-9860.1547885412
133868.14270254117
61000.0
11
74293.7388875
-8882.318131336462
157469.79590633645
68000.0
9
60351.362888
-11434.725754912419
132137.45153091243
55000.0
8
59123.682122
-16171.248326522546
134418.61257052253
55000.0
```
10. Generate evaluation metrics on test dataset using best performing model.
```python
performance_metrics = aml.evaluate(housing_test)
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : XGBOOST_0
Feature Selection Method : lasso
Performance Metrics :
MAE           MSE      MSLE      MAPE       MPE         RMSE
RMSLE            ME        R2        EV         MPD       MGD
0  5653.340405  6.496131e+07  0.016298  9.579258 -0.579437  8059.857835
0.127663  25532.231052  0.798248  0.800673  991.174089  0.016417
performance_metrics
MAE            MSE                 MSLE              MAPE
MPE                RMSE            RMSLE          ME
R2                EV                MPD               MGD
5653.3404      64961308.3216       0.01629774        9.5792575
```
19: AutoML in teradataml

```python
-0.5794372         8059.8578       0.1276626      25532.2311
0.7982484         0.8006725         991.1741         0.0164169
```
11. Generate prediction on test dataset using second best performing model.
```python
prediction = aml.predict(housing_test,2)
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : XGBOOST_3
Feature Selection Method : lasso
Prediction :
id    Prediction  Confidence_Lower  Confidence_upper    price
0  18  69345.452760      69345.452760      69345.452760  85000.0
1  39  57590.606963      57590.606963      57590.606963  51900.0
2  47  39957.102008      39957.102008      39957.102008  44500.0
3   9  53556.262102      53556.262102      53556.262102  55000.0
4  25  41374.084969      41374.084969      41374.084969  44500.0
5  33  79590.545472      79590.545472      79590.545472  90000.0
6  23  41494.207470      41494.207470      41494.207470  37900.0
7  49  62129.986607      62129.986607      62129.986607  63900.0
8  11  58577.423542      58577.423542      58577.423542  68000.0
9  19  82470.069541      82470.069541      82470.069541  69000.0
prediction.head()
id
Prediction
Confidence_Lower
Confidence_upper
price
10
43992.446503
43992.446503
43992.446503
41000.0
12
72565.284652
72565.284652
72565.284652
67000.0
13
47280.89498099999
47280.89498099999
47280.89498099999
49000.0
14
44128.888049999994
44128.888049999994
44128.888049999994
48500.0
16
76661.89054699999
76661.89054699999
76661.89054699999
72000.0
17
31961.054944
31961.054944
31961.054944
27000.0
15
61180.928922
61180.928922
61180.928922
61000.0
11
58577.423542
58577.423542
58577.423542
68000.0
9
53556.26210199999
53556.26210199999
53556.26210199999
55000.0
8
63626.498983
63626.498983
63626.498983
55000.0
```
19: AutoML in teradataml

```python
prediction = aml.predict(housing_test,2)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713815255642391"'
Updated dataset after performing categorical encoding :
sn
price
lotsize bedrooms
bathrms stories driveway_0
driveway_1
recroom_0
recroom_1
fullbase_0
fullbase_1
gashw_0 gashw_1 airco_0 airco_1 garagepl
prefarea_0
prefarea_1
homestyle_0
homestyle_1
homestyle_2
id
53
68000.0 9166.0
2
1
1
0
1
1
0
0
1
1
0
0
1
2
1
0
0
0
1
11
463
49000.0 2610.0
3
1
2
0
1
1
0
0
1
1
0
1
0
0
0
1
0
1
0
13
459
44555.0 2398.0
3
1
1
0
1
1
0
1
0
1
0
1
0
0
0
1
0
1
0
21
38
67000.0 5170.0
3
1
4
0
1
1
0
1
0
1
0
0
1
0
1
0
0
0
1
12
251
48500.0 3450.0
3
1
1
0
1
1
0
0
1
1
0
1
0
2
1
0
0
1
0
14
408
87500.0 6420.0
3
1
3
0
1
1
0
0
1
1
0
1
0
0
0
1
0
0
1
22
255
61000.0 4360.0
4
1
2
0
1
1
0
1
0
1
0
1
0
0
1
0
0
0
1
15
16
37900.0 3185.0
2
1
1
0
1
1
0
1
0
1
0
0
1
0
1
0
0
1
0
23
301
55000.0 4080.0
2
1
1
0
1
1
0
1
0
1
0
1
0
0
1
0
0
0
1
9
13
27000.0 1700.0
3
1
2
0
1
1
0
1
0
1
0
1
0
0
1
0
0
1
0
17
Performing transformation carried out in data preparation phase ...
```
19: AutoML in teradataml

```python
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713814881895584"'
Updated dataset after performing Lasso feature selection:
id
stories prefarea_1
sn
bathrms fullbase_0
recroom_0
homestyle_1
recroom_1
garagepl
driveway_0
prefarea_0
fullbase_1
airco_1 driveway_1
homestyle_0
lotsize price
23
1
0
16
1
1
1
1
0
0
0
1
0
1
1
0
3185.0
37900.0
32
1
1
403
1
0
0
0
1
0
0
0
1
1
1
0
6825.0
77500.0
24
1
0
274
2
0
0
0
1
0
0
1
1
0
1
0
4100.0
64900.0
75
1
0
294
1
1
1
1
0
0
0
1
0
0
1
0
4040.0
47000.0
52
1
0
111
1
1
1
1
0
0
1
1
0
0
0
0
5076.0
43000.0
11
1
0
53
1
0
1
0
0
2
0
1
1
1
1
0
9166.0
68000.0
39
1
1
441
1
1
1
0
0
2
0
0
0
0
1
0
3520.0
51900.0
29
1
0
306
1
1
1
0
0
1
0
1
0
1
1
0
5885.0
64000.0
22
3
1
408
1
0
1
0
0
0
0
0
1
0
1
0
6420.0
87500.0
51
1
1
443
1
1
1
0
0
0
0
0
0
0
1
0
3520.0
65000.0
Updated dataset after performing scaling on Lasso selected features :
fullbase_0
recroom_1
prefarea_0
driveway_0
fullbase_1
prefarea_1
airco_1 driveway_1
price
homestyle_0
recroom_0
homestyle_1
id
stories sn
bathrms garagepl
lotsize
0
1
0
0
1
1
1
1
77500.0 0
0
0
32
-1.1754677333050345
0.7785010970899041
-0.5026028286234501
-0.7628533219155937
0.9597051977890769
1
0
1
0
0
0
0
1
47000.0 0
1
1
75
-1.1754677333050345
0.0784722683583791
-0.5026028286234501
-0.7628533219155937
-0.49
069971372311655
1
0
0
0
0
1
0
1
65000.0 0
1
0
51
-1.1754677333050345
1.0353924103858767
-0.5026028286234501
-0.7628533219155937
-0.76
15114027666858
```
19: AutoML in teradataml

```python
1
0
1
1
0
0
0
0
43000.0 0
1
1
52
-1.1754677333050345
-1.096805489970695
-0.5026028286234501
-0.7628533219155937
0.04884049752522546
1
0
0
0
0
1
0
1
51900.0 0
1
0
39
-1.1754677333050345
1.022547844721078
-0.5026028286234501
1.8067578676948275
-0.7615114027666858
1
0
1
0
0
0
1
1
64000.0 0
1
0
29
-1.1754677333050345
0.15553966234717084
-0.5026028286234501
0.5219522728896168
0.47016099067185546
1
0
1
0
0
0
0
1
80000.0 0
1
0
67
3.338328362586298
0.22618477350356328
-0.5026028286234501
-0.7628533219155937
0.009260327588088398
1
0
1
0
0
0
1
1
67000.0 0
1
0
12
3.338328362586298
-1.5656321367358448
-0.5026028286234501
-0.76
28533219155937
0.09779491823694761
0
0
0
0
1
1
0
1
87500.0 0
1
0
22
1.8337296639558538
0.8106125112519007
-0.5026028286234501
-0.7628533219155937
0.7487845553609124
0
0
1
0
1
0
1
1
68000.0 0
1
0
11
-1.1754677333050345
-1.4692978942498551
-0.5026028286234501
1.8067578676948275
2.178878590194838
Updated dataset after performing RFE feature selection:
id
sn
bathrms homestyle_1
garagepl
homestyle_2
airco_0 homestyle_0
lotsize price
23
16
1
1
0
0
0
0
3185.0
37900.0
32
403
1
0
0
1
0
0
6825.0
77500.0
24
274
2
0
0
1
1
0
4100.0
64900.0
75
294
1
1
0
0
1
0
4040.0
47000.0
52
111
1
1
0
0
1
0
5076.0
43000.0
11
53
1
0
2
1
0
0
9166.0
68000.0
```
19: AutoML in teradataml

```python
39
441
1
0
2
1
1
0
3520.0
51900.0
29
306
1
0
1
1
0
0
5885.0
64000.0
22
408
1
0
0
1
1
0
6420.0
87500.0
51
443
1
0
0
1
1
0
3520.0
65000.0
Updated dataset after performing scaling on RFE selected features :
r_homestyle_1
r_homestyle_2
r_airco_0
r_homestyle_0
price
id
r_sn
r_bathrms
r_garagepl
r_lotsize
0
1
0
0
77500.0 32
0.7785010970899041
-0.5026028286234501
-0.7628533219155937
0.9597051977890769
1
0
1
0
47000.0 75
0.0784722683583791
-0.5026028286234501
-0.7628533219155937
-0.49
069971372311655
0
1
1
0
65000.0 51
1.0353924103858767
-0.5026028286234501
-0.7628533219155937
-0.76
15114027666858
1
0
1
0
43000.0
52
-1.096805489970695
-0.5026028286234501
-0.7628533219155937
0.04884049752522546
0
1
1
0
51900.0 39
1.022547844721078
-0.5026028286234501
1.8067578676948275
-0.7615114027666858
0
1
0
0
64000.0 29
0.15553966234717084
-0.5026028286234501
0.5219522728896168
0.47016099067185546
0
1
1
0
87500.0 22
0.8106125112519007
-0.5026028286234501
-0.7628533219155937
0.7487845553609124
0
1
1
0
80000.0 67
0.226184
50356328
-0.5026028286234501
-0.7628533219155937
0.009260327588088398
0
1
0
0
67000.0
12
-1.5656321367358448
-0.5026028286234501
-0.7628533219155937
0.09779491823694761
0
1
0
0
68000.0
11
-1.4692978942498551
-0.5026028286234501
1.8067578676948275
2.178878590194838
Updated dataset after performing scaling for PCA feature selection :
```
19: AutoML in teradataml

```python
recroom_0
recroom_1
prefarea_0
driveway_0
bedrooms
prefarea_1
fullbase_1
airco_1 homestyle_2
gashw_0 driveway_1
gashw_1 airco_0 homestyle_0
fullbase_0
price
homestyle_1
id
sn
lotsize bathrms stories garagepl
0
1
0
0
3
1
1
1
1
1
1
0
0
0
0
77500.0 0
32
0.7785010970899037
0.9597051977890783
-0.5026028286234494
-1.175467733305031
-0.76
28533219155942
1
0
1
0
2
0
0
0
0
1
1
0
1
0
1
47000.0 1
75
0.07847226835837905
-0.4906997137231172
-0.5026028286234494
-1.17
5467733305031
-0.7628533219155942
1
0
0
0
3
1
0
0
1
1
1
0
1
0
1
65000.0 0
51
1.035392410385876
-0.7615114027666869
-0.5026028286234494
-1.17
5467733305031
-0.7628533219155942
1
0
1
1
3
0
0
0
0
1
0
0
1
0
1
43000.0 1
52
-1.0968054899706945
0.048840497525225526
-0.5026028286234494
-1.175467733305031
-0.76
28533219155942
1
0
0
0
3
1
0
0
1
1
1
0
1
0
1
51900.0 0
39
1.0225478447210
-0.7615114027666869
-0.5026028286234494
-1.17
5467733305031
1.8067578676948284
1
0
1
0
2
0
0
1
1
1
1
0
0
0
1
64000.0 0
29
0.15553966234717076
0.4701609906718561
-0.5026028286234494
-1.175467733305031
0.5219522728896171
1
0
1
0
3
0
0
0
1
1
1
0
1
0
1
80000.0 0
67
0.22618477350356314
0.009260327588088412
-0.5026028286234494
3.338328362586288
-0.7628533219155942
1
0
1
0
3
0
0
1
1
1
1
0
0
0
1
67000.0 0
12
-1.565632136735844
0.09779491823694775
-0.5026028286234494
3.338328362586288
-0.7628533219155942
1
0
0
0
3
1
1
0
1
1
1
0
1
0
0
87500.0 0
22
0.8106125112519003
0.7487845553609134
-0.5026028286234494
1.8337296639558482
-0.7628533219155942
1
0
1
0
2
0
1
1
1
1
```
19: AutoML in teradataml

```python
1
0
0
0
0
68000.0 0
11
-1.4692978942498542
2.178878590194841
-0.5026028286234494
-1.175467733305031
1.8067578676948284
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
col_6
col_7
col_8
col_9
price
0
23
-1.846762
-1.338399
0.503782
-0.227864
-0.735116
-0.101448
1.053769
-0.133609
0.006323
-0.570542
37900.0
1
67
-1.071889
2.634593
-1.143026
1.133870
-0.666936
0.448109
-1.183424
0.156071
-0.190941
-0.026246
80000.0
2
22
0.436066
1.674569
-1.184835
-0.517098
-0.149681
0.581587
-1.015373
0.346597
-1.125296
-0.054246
87500.0
3
21
-0.556103
-0.430944
-1.299009
-1.208186
1.167272
-1.350354
0.655622
0.022532
0.184275
-0.272029
44555.0
4
12
-1.702851
2.244144
-0.163064
1.531449
-1.726684
1.435539
-0.336577
0.312267
-0.721428
-0.019429
67000.0
5
32
1.447936
-0.419323
-0.367653
-1.974154
-0.669749
0.666961
0.361696
0.420576
0.244402
0.138265
00.0
6
24
0.121058
-0.088188
1.823276
-1.499059
0.807124
-0.001108
-0.486189
-0.327831
0.999693
0.041307
64900.0
7
75
-0.913410
-0.987968
-0.428719
-0.617452
-0.02
6463
-1.166772
0.029816
-0.223178
0.401544
-0.558936
47000.0
8
51
0.035124
-0.359414
-1.219128
-1.145653
0.772859
-0.721654
0.162539
-0.630366
0.185628
0.517291
65000.0
9
52
-1.417779
-1.546909
0.258422
-0.403733
-0.676443
-1.060856
0.087088
0.370494
-0.220855
0.444359
43000.0
Data Transformation completed.
Following model is being used for generating prediction :
```
19: AutoML in teradataml

```python
Model ID : XGBOOST_0
Feature Selection Method : lasso
Prediction :
id    Prediction  Confidence_Lower  Confidence_upper    price
0  22  83148.275354     -13682.206803     179978.757511  87500.0
1  21  40304.804437      -4991.597547      85601.206422  44555.0
2  32  80038.998447     -15350.979605     175428.976498  77500.0
3  24  75236.641121     -15797.504638     166270.786879  64900.0
4  51  59341.
847     -11013.321662     129696.875356  65000.0
5  52  49527.065554      -1879.863957     100933.995065  43000.0
6  11  74293.738888      -8882.318131     157469.795906  68000.0
7  39  59237.625097     -11283.510389     129758.760583  51900.0
8  29  65
.624865      -9231.694949     140784.944679  64000.0
9  75  41305.158083      -8937.031699      91547.347865  47000.0
Performance Metrics :
MAE           MSE      MSLE       MAPE       MPE         RMSE
RMSLE            ME        R2        EV          MPD       MGD
0  7530.032113  9.803829e+07  0.032812  14.340231 -6.394165  9901.428629
0.18114  31376.260153  0.695521  0.702174  1675.413575  0.030915
prediction.head()
id
Prediction
Confidence_Lower
Confidence_upper
price
10
50847.987966999994
-2114.7423931209414
103810.71832712094
41000.0
12
82823.9576065
-8536.222191293302
174184.1374042933
67000.0
13
42366.405568999995
-7506.1900559
91
92239.0011939
9
49000.0
14
50464.071062999996
-2300.8744488123702
103229.01657481235
48500.0
16
72076.95612349999
-17375.200759916668
161529.11300691665
72000.0
17
30842.594055999998
-5288.270194834851
66973.45830683484
27000.0
15
62003.993956999984
-9860.1547885412
133868.14270254117
61000.0
11
74293.7388875
-8882.318131336462
157469.79590633645
68000.0
9
60351.362888
-11434.725754912419
132137.45153091243
55000.0
```
19: AutoML in teradataml

```python
8
59123.682122
-16171.248326522546
134418.61257052253
55000.0
```
12. Generate evaluation metrics on test dataset using second best performing model.
```python
performance_metrics = aml.evaluate(housing_test, 2)
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : XGBOOST_3
Feature Selection Method : lasso
Performance Metrics :
MAE           MSE      MSLE      MAPE       MPE         RMSE
RMSLE            ME        R2        EV         MPD       MGD
0  5653.340405  6.496131e+07  0.016298  9.579258 -0.579437  8059.857835
0.127663  25532.231052  0.798248  0.800673  991.174089  0.016417
performance_metrics
MAE                MSE                  MSLE
MAPE               MPE                  RMSE
RMSLE              ME                  R2                  EV
MPD                MGD
5653.340404608697  64961308.32164452    0.01629774142321875
9.579257502786861  -0.5794371580105192  8059.857835076529
0.12766260
2279  25532.231052000003  0.7982484477510167  0.80067251696863
991.1740893951925  0.01641689990934832
```
### Example 2: Run AutoRegressor for Regression Problem with
### Early Stopping Condition and Customization
This example predicts the price of houses based on different factors.
**Run AutoRegressor to get the best performing model with following specifications:**
* Set early stopping criteria, that is, time limit to 200300 sec and performance metrics R2 threshold value
* to 0.60.7.
* Exclude 'glm', 'svm', and 'knn' model from default model training list.
* Opt for verbose level 2 to get detailed logging.
* Use  custom_config_file  to customize some specific processes in AutoML flow.
1.
* Load the example dataset.
19: AutoML in teradataml

```python
load_example_data("decisionforestpredict",
["housing_train", "housing_test"])
housing_train = DataFrame.from_table("housing_train")
housing_test = DataFrame.from_table("housing_test")
```
2.
* Generate custom config JSON file.
```python
AutoRegressor.generate_custom_config("custom_housing")
Generating custom config JSON for AutoML ...
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  1
Customizing Feature Engineering Phase ...
Available options for customization of feature engineering phase with
corresponding indices:
Index 1: Customize Missing Value Handling
Index 2: Customize Bincode Encoding
Index 3: Customize String Manipulation
Index 4: Customize Categorical Encoding
```
19: AutoML in teradataml

```python
Index 5: Customize Mathematical Transformation
Index 6: Customize Nonlinear Transformation
Index 7: Customize Antiselect Features
Index 8: Back to main menu
Index 9: Generate custom json and exit
Enter the list of indices you want to customize in feature engineering phase:
2,4,7,8
Customizing Bincode Encoding ...
Provide the following details to customize binning and coding encoding:
Available binning methods with corresponding indices:
Index 1: Equal-Width
Index 2: Variable-Width
Enter the feature or list of features for binning:  bedrooms
Enter the index of corresponding binning method for feature bedrooms:  2
Enter the number of bins for feature bedrooms:  2
Available value type of feature for variable binning with corresponding
indices:
Index 1: int
Index 2: float
Provide the range for bin 1 of feature bedrooms:
Enter the index of corresponding value type of feature bedrooms:  1
Enter the minimum value for bin 1 of feature bedrooms:  0
Enter the maximum value for bin 1 of feature bedrooms:  2
Enter the label for bin 1 of feature bedrooms:  small_house
```
19: AutoML in teradataml

```python
Provide the range for bin 2 of feature bedrooms:
Enter the index of corresponding value type of feature bedrooms:  1
Enter the minimum value for bin 2 of feature bedrooms:  3
Enter the maximum value for bin 2 of feature bedrooms:  6
Enter the label for bin 2 of feature bedrooms:  big_house
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  0
Customization of bincode encoding has been completed successfully.
Customizing Categorical Encoding ...
Provide the following details to customize categorical encoding:
Available categorical encoding methods with corresponding indices:
Index 1: OneHotEncoding
Index 2: OrdinalEncoding
Index 3: TargetEncoding
Enter the list of corresponding index categorical encoding methods you want to
use:  2,3
Enter the feature or list of features for OrdinalEncoding:  homestyle
Enter the feature or list of features for TargetEncoding:  prefarea
Available target encoding methods with corresponding indices:
Index 1: CBM_BETA
Index 2: CBM_DIRICHLET
Index 3: CBM_GAUSSIAN_INVERSE_GAMMA
Enter the index of target encoding method for feature prefarea:  3
Enter the response column for target encoding method for feature prefarea:
price
```
19: AutoML in teradataml

```python
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  0
Customization of categorical encoding has been completed successfully.
Customizing Antiselect Features ...
Enter the feature or list of features for antiselect:  sn
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  0
Customization of antiselect features has been completed successfully.
Customization of feature engineering phase has been completed successfully.
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  2
Customizing Data Preparation Phase ...
Available options for customization of data preparation phase with
```
19: AutoML in teradataml

```python
corresponding indices:
Index 1: Customize Data Imbalance Handling
Index 2: Customize Outlier Handling
Index 3: Customize Feature Scaling
Index 4: Back to main menu
Index 5: Generate custom json and exit
Enter the list of indices you want to customize in data preparation phase:
1,2
Customizing Data Imbalance Handling ...
Available data sampling methods with corresponding indices:
Index 1: SMOTE
Index 2: NearMiss
Enter the corresponding index data imbalance handling method:  1
Customization of data imbalance handling has been completed successfully.
Customizing Outlier Handling ...
Available outlier detection methods with corresponding indices:
Index 1: percentile
Index 2: tukey
Index 3: carling
Enter the corresponding index oulier handling method:  1
Enter the lower percentile value for outlier handling:  0.15
Enter the upper percentile value for outlier handling:  0.85
Enter the feature or list of features for outlier handling:  bathrms
```
19: AutoML in teradataml

```python
Available outlier replacement methods with corresponding indices:
Index 1: delete
Index 2: median
Index 3: Any Numeric Value
Enter the index of corresponding replacement method for feature bathrms:  1
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  0
Customization of outlier handling has been completed successfully.
Available options for customization of data preparation phase with
corresponding indices:
Index 1: Customize Data Imbalance Handling
Index 2: Customize Outlier Handling
Index 3: Customize Feature Scaling
Index 4: Back to main menu
Index 5: Generate custom json and exit
Enter the list of indices you want to customize in data preparation phase:  5
Customization of data preparation phase has been completed successfully.
Process of generating custom config file for AutoML has been completed
successfully.
'custom_housing.json' file is generated successfully under the current working
directory.
```
19: AutoML in teradataml

```python
Generating custom config JSON for AutoML ...
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  1
Customizing Feature Engineering Phase ...
Available options for customization of feature engineering phase with
corresponding indices:
Index 1: Customize Missing Value Handling
Index 2: Customize Bincode Encoding
Index 3: Customize String Manipulation
Index 4: Customize Categorical Encoding
Index 5: Customize Mathematical Transformation
Index 6: Customize Nonlinear Transformation
Index 7: Customize Antiselect Features
Index 8: Back to main menu
Index 9: Generate custom json and exit
```
19: AutoML in teradataml

```python
Enter the list of indices you want to customize in feature engineering phase:
2,4,7,8
Customizing Bincode Encoding ...
Provide the following details to customize binning and coding encoding:
Available binning methods with corresponding indices:
Index 1: Equal-Width
Index 2: Variable-Width
Enter the feature or list of features for binning:  bedrooms
Enter the index of corresponding binning method for feature bedrooms:  2
Enter the number of bins for feature bedrooms:  2
Available value type of feature for variable binning with corresponding
indices:
Index 1: int
Index 2: float
Provide the range for bin 1 of feature bedrooms:
Enter the index of corresponding value type of feature bedrooms:  1
Enter the minimum value for bin 1 of feature bedrooms:  0
Enter the maximum value for bin 1 of feature bedrooms:  2
Enter the label for bin 1 of feature bedrooms:  small_house
Provide the range for bin 2 of feature bedrooms:
Enter the index of corresponding value type of feature bedrooms:  1
Enter the minimum value for bin 2 of feature bedrooms:  3
Enter the maximum value for bin 2 of feature bedrooms:  5
Enter the label for bin 2 of feature bedrooms:  big_house
```
19: AutoML in teradataml

```python
Customization of bincode encoding has been completed successfully.
Customizing Categorical Encoding ...
Provide the following details to customize categorical encoding:
Available categorical encoding methods with corresponding indices:
Index 1: OneHotEncoding
Index 2: OrdinalEncoding
Index 3: TargetEncoding
Enter the list of corresponding index categorical encoding methods you want to
use:  2,3
Enter the feature or list of features for OrdinalEncoding:  homestyle
Enter the feature or list of features for TargetEncoding:  prefarea
Available target encoding methods with corresponding indices:
Index 1: CBM_BETA
Index 2: CBM_DIRICHLET
Index 3: CBM_GAUSSIAN_INVERSE_GAMMA
Enter the index of target encoding method for feature prefarea:  3
Enter the response column for target encoding method for feature prefarea:
price
Customization of categorical encoding has been completed successfully.
Customizing Antiselect Features ...
Enter the feature or list of features for antiselect:  sn
Customization of antiselect features has been completed successfully.
Customization of feature engineering phase has been completed successfully.
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
```
19: AutoML in teradataml

```python
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  2
Customizing Data Preparation Phase ...
Available options for customization of data preparation phase with
corresponding indices:
Index 1: Customize Train Test Split
Index 2: Customize Data Imbalance Handling
Index 3: Customize Outlier Handling
Index 4: Customize Feature Scaling
Index 5: Back to main menu
Index 6: Generate custom json and exit
Enter the list of indices you want to customize in data preparation phase:
1,2,3,4,5
Customizing Train Test Split ...
Enter the train size for train test split:  0.75
Customization of train test split has been completed successfully.
Customizing Data Imbalance Handling ...
Available data sampling methods with corresponding indices:
Index 1: SMOTE
```
19: AutoML in teradataml

```python
Index 2: NearMiss
Enter the corresponding index data imbalance handling method:  1
Customization of data imbalance handling has been completed successfully.
Customizing Outlier Handling ...
Available outlier detection methods with corresponding indices:
Index 1: percentile
Index 2: tukey
Index 3: carling
Enter the corresponding index oulier handling method:  1
Enter the lower percentile value for outlier handling:  0.1
Enter the upper percentile value for outlier handling:  0.9
Enter the feature or list of features for outlier handling:  bathrms
Available outlier replacement methods with corresponding indices:
Index 1: delete
Index 2: median
Index 3: Any Numeric Value
Enter the index of corresponding replacement method for feature bathrms:  1
Customization of outlier handling has been completed successfully.
Available feature scaling methods with corresponding indices:
Index 1: maxabs
Index 2: mean
Index 3: midrange
Index 4: range
Index 5: rescale
Index 6: std
Index 7: sum
Index 8: ustd
Enter the corresponding index feature scaling method:  6
Customization of feature scaling has been completed successfully.
```
19: AutoML in teradataml

```python
Customization of data preparation phase has been completed successfully.
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  3
Customizing Model Training Phase ...
Available options for customization of model training phase with corresponding
indices:
Index 1: Customize Model Hyperparameter
Index 2: Back to main menu
Index 3: Generate custom json and exit
Enter the list of indices you want to customize in model training phase:  1
Customizing Model Hyperparameter ...
Available models for hyperparameter tuning with corresponding indices:
Index 1: decision_forest
Index 2: xgboost
Index 3: knn
Index 4: glm
Index 5: svm
```
19: AutoML in teradataml

```python
Available hyperparamters update methods with corresponding indices:
Index 1: ADD
Index 2: REPLACE
Enter the list of model indices for performing hyperparameter tuning:  2
Available hyperparameters for model 'xgboost' with corresponding indices:
Index 1: min_impurity
Index 2: max_depth
Index 3: min_node_size
Index 4: shrinkage_factor
Index 5: iter_num
Enter the list of hyperparameter indices for model 'xgboost':  3
Enter the index of corresponding update method for hyperparameters
'min_node_size' for model 'xgboost':  1
Enter the list of value for hyperparameter 'min_node_size' for model
'xgboost':  1,2
Customization of model hyperparameter has been completed successfully.
Available options for customization of model training phase with corresponding
indices:
Index 1: Customize Model Hyperparameter
Index 2: Back to main menu
Index 3: Generate custom json and exit
Enter the list of indices you want to customize in model training phase:  2
Customization of model training phase has been completed successfully.
Available main options for customization with corresponding indices:
```
19: AutoML in teradataml

```python
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  4
Generating custom json and exiting ...
Process of generating custom config file for AutoML has been completed
successfully.
'custom_housing.json' file is generated successfully under the current working
directory.
```
3.
* Create an AutoRegressor instance.
```python
aml = AutoRegressor(exclude=['glm','svm','knn'],
verbose=2,
max_runtime_secs=200,
stopping_metric='R2',
stopping_tolerance=0.6,
custom_config_file='custom_housing.json')
aml = AutoRegressor(exclude=['glm','svm','knn'],
verbose=2,
max_runtime_secs=300,
stopping_metric='R2',
stopping_tolerance=0.7,
custom_config_file='custom_housing.json')
```
4.
* Fit the data.
```python
aml.fit(housing_train,housing_train.price)
Received below input for customization :
{
"BincodeIndicator": true,
"BincodeParam": {
"bedrooms": {
```
19: AutoML in teradataml

```python
"Type": "Variable-Width",
"NumOfBins": 2,
"Bin_1": {
"min_value": 0,
"max_value": 2,
"label": "small_house"
},
"Bin_2": {
"min_value": 3,
"max_value": 6,
"label": "big_house"
}
}
},
"CategoricalEncodingIndicator": true,
"CategoricalEncodingParam": {
"OrdinalEncodingIndicator": true,
"OrdinalEncodingList": [
"homestyle"
],
"TargetEncodingIndicator": true,
"TargetEncodingList": {
"prefarea": {
"encoder_method": "CBM_GAUSSIAN_INVERSE_GAMMA",
"response_column": "price"
}
}
},
"AntiselectIndicator": true,
"AntiselectParam": {
"excluded_columns": [
"sn"
]
},
"DataImbalanceIndicator": true,
"DataImbalanceMethod": "SMOTE",
"OutlierFilterIndicator": true,
"OutlierFilterMethod": "percentile",
"OutlierLowerPercentile": 0.15,
"OutlierUpperPercentile": 0.85,
"OutlierFilterParam": {
"bathrms": {
"replacement_value": "delete"
}
```
19: AutoML in teradataml

```python
}
}
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 492
Total Columns in the data: 14
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
homestyle
VARCHAR(20) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
airco
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
price
FLOAT
492
0
None
0
492
0
0.0
100.0
lotsize FLOAT
492
0
None
0
492
0
0.0
100.0
fullbase
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
bathrms INTEGER 492
0
None
0
492
0
0.0
100.0
recroom VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
bedrooms
INTEGER 492
0
None
0
492
0
0.0
100.0
driveway
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
prefarea
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
stories INTEGER 492
0
None
0
492
0
0.0
100.0
sn
INTEGER 492
0
None
0
492
0
0.0
100.0
garagepl
INTEGER 492
0
None
270
222
0
0.0
100.0
gashw
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
sn       price    lotsize  bedrooms  bathrms  stories  garagepl
func
50%    274.000   62000.000   4616.000     3.000    1.000    2.000     0.000
count  492.000     492.000    492.000   492.000  492.000  492.000   492.000
mean   272.943   68100.396   5181.795     2.965    1.293    1.803     0.685
min      1.000   25000.000   1650.000     1.000    1.000    1.000     0.000
```
19: AutoML in teradataml

```python
max    546.000  190000.000  16200.000     6.000    4.000    4.000     3.000
75%    413.250   82000.000   6370.000     3.000    2.000    2.000     1.000
25%    132.500   49975.000   3600.000     2.000    1.000    1.000     0.000
std    159.501   26472.496   2182.443     0.731    0.510    0.861     0.854
Statistics of Data:
func
sn
price
lotsize bedrooms
bathrms stories garagepl
50%
274
62000
4616
3
1
2
0
count
492
492
492
492
492
492
492
mean
272.943 68100.396
5181.795
2.965
1.293
1.803
0.685
min
1
25000
1650
1
1
1
0
max
546
190000
16200
6
4
4
3
75%
413.25
82000
6370
3
2
2
1
25%
132.5
49975
3600
2
1
1
0
std
159.501 26472.496
2182.443
0.731
0.51
0.861
0.854
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
driveway                  2
recroom                   2
fullbase                  2
gashw                     2
airco                     2
prefarea                  2
homestyle                 3
No Futile columns found.
Target Column Distribution:
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0    stories           7.113821
1    bathrms           0.203252
2   bedrooms           2.235772
3   garagepl           2.235772
4    lotsize           2.235772
5      price           2.439024
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
```
19: AutoML in teradataml

```python
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.71 sec
Starting customized anti-select columns ...
Updated dataset sample after performing anti-select columns:
price
lotsize bedrooms
bathrms stories driveway
recroom
fullbase
gashw
airco
garagepl
prefarea
homestyle
99000.0 8880.0
3
2
2
yes
no
yes
no
yes
1
no
Eclectic
63900.0 6360.0
2
1
1
yes
no
yes
no
yes
1
no
Eclectic
88000.0 4500.0
3
1
4
yes
no
no
no
yes
0
no
Eclectic
50000.0 3640.0
2
1
1
yes
no
no
no
no
1
no
Classic
48000.0 4120.0
2
1
2
yes
no
no
no
no
0
no
Classic
27000.0 3649.0
2
1
1
yes
no
no
no
no
0
no
Classic
58000.0 4340.0
3
1
1
yes
no
no
no
no
0
no
Eclectic
87000.0 8372.0
3
1
3
yes
no
no
no
yes
2
no
Eclectic
49500.0 5320.0
2
1
1
yes
no
no
no
no
1
yes
Classic
70100.0 4200.0
3
1
2
yes
no
no
no
no
1
no
Eclectic
492 rows X 13 columns
Handling less significant features from data ...
Analysis indicates all categorical columns are significant. No action
Needed.
Total time to handle less significant features: 18.82 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
```
19: AutoML in teradataml

```python
Total time to handle date features: 0.00 sec
Proceeding with default option for missing value imputation.
Proceeding with default option for handling remaining missing
values.
Checking Missing values in dataset ...
Analysis Completed. No Missing Values
Detected.
Total time to find missing values in data: 8.68 sec
Imputing Missing Values ...
Analysis completed. No imputation
required.
Time taken to perform imputation: 0.01 sec
No information provided for Equal-Width
Transformation.
Variable-Width binning information:-
ColumnName
MinValue
MaxValue
Label
0
bedrooms
0
2
small_house
1
bedrooms
3
6
big_house
2 rows X 4 columns
Updated dataset sample after performing Variable-Width binning:
bathrms stories gashw
airco
fullbase
id
homestyle
lotsize garagepl
prefarea
driveway
recroom price
bedrooms
3
2
no
no
yes
194
bungalow
5960.0
1
no
yes
yes
117000.0
big_house
3
2
no
no
yes
291
Eclectic
3300.0
0
no
yes
no
79000.0 big_house
3
4
no
yes
no
76
bungalow
8580.0
2
yes
yes
no
145000.0
big_house
3
2
no
no
no
92
Classic 3630.0
0
no
no
yes
38000.0 big_house
3
2
yes
no
yes
66
bungalow
6000.0
2
no
yes
yes
138300.0
big_house
3
2
no
no
no
222
bungalow
16200.0 0
no
yes
no
145000.0
big_house
3
2
no
no
yes
118
Eclectic
4410.0
2
no
yes
no
71000.0 big_house
1
1
no
yes
no
195
Classic 2684.0
1
no
yes
no
46000.0 small_house
1
2
no
no
no
83
Classic 4370.0
0
no
yes
no
46000.0 big_house
```
19: AutoML in teradataml

```python
1
2
no
no
yes
242
Classic 3970.0
0
no
yes
no
32500.0 big_house
492 rows X 14 columns
Skipping customized string
manipulation.
Starting Customized Categorical Feature Encoding ...
Updated dataset sample after performing ordinal encoding:
bathrms stories gashw
airco
fullbase
id
lotsize bedrooms
garagepl
prefarea
driveway
recroom price
homestyle
3
4
no
yes
no
76
8580.0
big_house
2
yes
yes
no
145000.0
0
3
2
no
no
no
73
2610.0
big_house
0
no
no
no
60000.0 2
3
2
no
no
no
222
16200.0 big_house
0
no
yes
no
145000.0
0
3
2
no
no
yes
118
4410.0
big_house
2
no
yes
no
71000.0 2
3
2
no
no
yes
291
3300.0
big_house
0
no
yes
no
79000.0 2
3
2
no
no
yes
194
5960.0
big_house
1
no
yes
yes
117000.0
0
3
2
no
no
no
510
8580.0
big_house
2
no
yes
no
92000.0 2
1
1
no
no
no
206
4320.0
big_house
0
yes
yes
no
67000.0 2
1
2
no
no
yes
169
5010.0
big_house
0
no
yes
no
66000.0 2
1
2
no
yes
no
53
4400.0
big_house
2
yes
yes
no
79500.0 2
492 rows X 14 columns
Updated dataset sample after performing target encoding:
prefarea
bathrms stories gashw
airco
fullbase
id
homestyle
lotsize bedrooms
garagepl
driveway
recroom price
83851.72413793103
1
1
no
yes
no
96
2
10240.0 small_house
2
yes
no
68000.0
83851.72413793103
1
1
no
no
yes
233
2
6900.0
big_house
0
yes
yes
78900.0
83851.72413793103
1
1
no
yes
yes
185
2
6360.0
big_house
2
yes
yes
82000.0
```
19: AutoML in teradataml

```python
83851.72413793103
1
2
no
yes
yes
375
2
5136.0
big_house
0
yes
yes
80000.0
83851.72413793103
1
2
no
yes
yes
378
2
8400.0
big_house
2
yes
yes
75000.0
83851.72413793103
1
3
no
yes
no
285
2
6100.0
big_house
0
yes
yes
78000.0
62906.33597883598
1
1
no
no
yes
211
2
3540.0
small_house
0
no
yes
72000.0
62906.33597883598
1
1
no
no
no
247
1
3360.0
small_house
1
yes
no
30000.0
62906.33597883598
1
1
no
no
no
395
1
2700.0
small_house
0
no
no
42000.0
62906.33597883598
1
2
no
yes
yes
55
1
3180.0
big_house
0
yes
no
47000.0
492 rows X 14 columns
Performing encoding for categorical columns ...
ONE HOT Encoding these Columns:
['gashw', 'airco', 'fullbase', 'bedrooms', 'driveway', 'recroom']
Sample of dataset after performing one hot encoding:
prefarea
bathrms stories gashw_0 gashw_1 airco_0 airco_1 fullbase_0
fullbase_1
id
homestyle
lotsize bedrooms_0
bedrooms_1
garagepl
driveway_0
driveway_1
recroom_0
recroom_1
price
83851.72413793103
1
1
1
0
0
1
0
1
185
2
6360.0
1
0
2
0
1
0
1
82000.0
83851.72413793103
1
2
1
0
0
1
0
1
378
2
8400.0
1
0
2
0
1
0
1
75000.0
83851.72413793103
1
3
1
0
0
1
1
0
285
2
6100.0
1
0
0
0
1
0
1
78000.0
83851.72413793103
1
1
1
0
0
1
0
1
139
2
11175.0 1
0
1
0
1
1
0
100000.0
83851.72413793103
1
2
1
0
1
0
0
1
58
2
3400.0
1
0
2
0
1
1
0
61100.0
83851.72413793103
1
1
1
0
1
0
1
0
464
2
6040.0
1
0
2
0
1
1
```
19: AutoML in teradataml

```python
0
69000.0
62906.33597883598
1
1
1
0
1
0
1
0
395
1
2700.0
0
1
0
1
0
1
0
42000.0
62906.33597883598
1
1
1
0
1
0
1
0
119
2
6720.0
1
0
0
0
1
1
0
70000.0
62906.33597883598
1
2
1
0
1
0
1
0
144
1
4840.0
0
1
0
0
1
1
0
35000.0
62906.33597883598
1
1
1
0
0
1
1
0
234
0
8880.0
0
1
1
0
1
1
0
101000.0
492 rows X 20 columns
Time taken to encode the columns: 14.42 sec
Starting customized mathematical transformation ...
Skipping customized mathematical
transformation.
Starting customized non-linear transformation ...
Skipping customized non-linear
transformation.
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
No information provided for performing customized feature scaling. Proceeding
with default option.
Starting customized outlier processing ...
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0         id           9.756098
1    lotsize           9.552846
2    bathrms           2.235772
3   garagepl           2.235772
4      price           8.739837
Feature selection using lasso ...
feature selected by lasso:
```
19: AutoML in teradataml

```python
['recroom_1', 'stories', 'gashw_0', 'bedrooms_0', 'garagepl', 'fullbase_1',
'bathrms', 'airco_0', 'recroom_0', 'airco_1', 'driveway_1', 'gashw_1',
'bedrooms_1', 'homestyle', 'fullbase_0', 'driveway_0', 'prefarea', 'lotsize']
Total time taken by feature selection: 0.96 sec
scaling Features of lasso data ...
columns that will be scaled:
['stories', 'garagepl', 'bathrms', 'homestyle', 'prefarea', 'lotsize']
Dataset sample after scaling:
recroom_1
airco_0 recroom_0
gashw_0 airco_1 id
driveway_1
gashw_1 bedrooms_1
fullbase_0
bedrooms_0
fullbase_1
price
driveway_0
stories garagepl
bathrms homestyle
prefarea
lotsize
0
1
1
1
0
10
0
0
1
0
0
1
54500.0 1
-0.9243489557048288
-0.7962575788492099
1.7248787237282124
0.7342974433930187
-0.5541346563708683
-0.9405595540664473
0
0
1
1
1
12
1
0
1
0
0
1
63900.0 0
-0.9243489557048288
0.38950061132561975
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
0.5744513081034206
0
0
1
1
1
13
1
0
0
0
1
1
99000.0 0
0.24261127446320968
0.38950061132561975
1.7248787237282124
0.7342974433930187
-0.5541346563708683
1.7638056298068683
0
1
1
1
0
14
1
0
1
1
0
0
4
0.0 0
0.24261127446320968
-0.7962575788492099
-0.5797509043642046
-0.74
35145661134329
-0.5541346563708683
-0.48275253341075514
0
0
1
1
1
16
1
0
0
1
1
0
87000.0 0
1.4095715046312483
1.5752588015004494
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
1.5240469014634748
0
0
1
1
1
17
0
0
0
0
1
1
57000.0 1
0.24261127446320968
-0.7962575788492099
1.7248787237282124
0.7342974433930187
-0.5541346563708683
-0.303405453153886
0
1
1
1
0
15
1
0
1
1
0
0
49500.0 0
-0.9243489557048288
0.38950061132561975
-0.5797509043642046
-0.7435145661134329
1.8046155180928998
0.08360666740041044
```
19: AutoML in teradataml

```python
0
1
1
1
0
11
1
0
0
1
1
0
80000.0 0
0.24261127446320968
0.38950061132561975
1.7248787237282124
0.7342974433930187
-0.5541346563708683
2.5283905509019418
0
1
1
1
0
9
1
0
1
1
0
0
50000.0 0
-0.9243489557048288
0.38950061132561975
-0.5797509043642046
-0.7435145661134329
-0.55
41346563708683
-0.7092962137352213
0
1
1
1
0
8
1
0
0
1
1
0
58000.0
0
-0.9243489557048288
-0.7962575788492099
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
-0.37892001326204144
481 rows X 20 columns
Total time taken by feature scaling: 44.45 sec
Feature selection using rfe ...
feature selected by RFE:
['stories', 'garagepl', 'bathrms', 'airco_0', 'bedrooms_1', 'homestyle',
'fullbase_0', 'prefarea', 'lotsize']
Total time taken by feature selection: 38.33 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_stories', 'r_garagepl', 'r_bathrms', 'r_homestyle', 'r_prefarea',
'r_lotsize']
Dataset sample after scaling:
r_bedrooms_1
r_fullbase_0
id
price
r_airco_0
r_stories
r_garagepl
r_bathrms
r_homestyle
r_prefarea
r_lotsize
1
0
10
54500.0
1
-0.9243489557048288
-0.7962575788492099
1.7248787237282124
0.7342974433930187
-0.5541346563708683
-0.9405595540664473
1
0
12
63900.0 0
-0.9243489557048288
0.38950061132561975
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
0.5744513081034206
0
0
13
99000.0 0
0.24261127446320968
0.38950061132561975
1.7248787237282124
0.7342974433930187
-0.5541346563708683
1.7638056298068683
1
1
14
48000.0 1
```
19: AutoML in teradataml

```python
0.24261127446320968
-0.7962575788492099
-0.5797509043642046
-0.74
35145661134329
-0.5541346563708683
-0.48275253341075514
0
1
16
87000.0 0
1.4095715046312483
1.5752588015004494
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
1.5240469014634748
0
0
17
57000.0 0
0.24261127446320968
-0.7962575788492099
1.7248787237282124
0.7342974433930187
-0.5541346563708683
-0.303405453153886
1
1
15
49500.0 1
-0.9243489557048288
0.38950061132561975
-0.5797509043642046
-0.7435145661134329
1.8046155180928998
0.08360666740041044
0
1
11
80000.0 1
0.24261127446320968
0.38950061132561975
1.7248787237282124
0.7342974433930187
-0.5541346563708683
2.5283905509019418
1
1
9
50000.0 1
-0.9243489557048288
0.38950061132561975
-0.5797509043642046
-0.7435145661134329
-0.55
41346563708683
-0.7092962137352213
0
1
8
58000.0
1
-0.9243489557048288
-0.7962575788492099
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
-0.37892001326204144
481 rows X 11 columns
Total time taken by feature scaling: 36.07 sec
scaling Features of pca data ...
columns that will be scaled:
['prefarea', 'bathrms', 'stories', 'homestyle', 'lotsize', 'garagepl']
Dataset sample after scaling:
recroom_1
airco_0 recroom_0
gashw_0 airco_1 id
driveway_1
gashw_1 bedrooms_1
fullbase_0
bedrooms_0
fullbase_1
price
driveway_0
prefarea
bathrms stories homestyle
lotsize
garagepl
1
0
0
1
1
285
1
0
0
1
1
0
78000.0 0
1.8046155180928782
-0.5797509043642026
1.409571504631246
0.7342974433930172
0.4517401479276683
-0.7962575788492109
0
1
1
1
0
58
1
0
0
0
1
1
61100.0 0
1.8046155180928782
-0.5797509043642026
0.24261127446320932
0.7342974433930172
-0.822568053897455
1.5752588015004514
0
1
1
1
0
464
1
0
0
1
```
19: AutoML in teradataml

```python
1
0
69000.0 0
1.8046155180928782
-0.5797509043642026
-0.9243489557048274
0.7342974433930172
0.42342218788711006
1.5752588015004514
0
1
1
1
0
393
1
0
0
0
1
1
86900.0 0
1.8046155180928782
-0.5797509043642026
-0.9243489557048274
0.7342974433930172
2.1130604703070883
1.5752588015004514
0
1
1
1
0
283
1
0
0
1
1
0
58000.0 0
1.8046155180928782
-0.5797509043642026
0.24261127446320932
0.7342974433930172
-0.3883593332755611
1.5752588015004514
0
1
1
1
0
387
1
0
0
0
1
1
47000.0 0
1.8046155180928782
-0.5797509043642026
0.24261127446320932
-0.7435145661134314
-1.4148853847457994
-0.79
62575788492109
0
1
1
1
0
144
1
0
1
1
0
0
35000.0 0
-0.5541346563708951
-0.5797509043642026
0.24261127446320932
-0.7435145661134314
-0.1429370129240559
-0.79
62575788492109
0
0
1
1
1
411
1
0
0
1
1
0
83000.0 0
-0.5541346563708951
-0.5797509043642026
1.409571504631246
0.7342974433930172
-0.16181565295109474
-0.7962575788492109
0
1
1
1
0
487
1
0
0
1
1
0
38000.0
0
-0.5541346563708951
-0.5797509043642026
-0.9243489557048274
-0.7435145661134314
-1.105747654303038
-0.7962575788492109
0
1
1
1
0
35
1
0
1
1
0
0
45000.0
0
-0.5541346563708951
-0.5797509043642026
-0.9243489557048274
-0.7435145661134314
-0.7989697538636564
-0.7962575788492109
481 rows X 20 columns
Total time taken by feature scaling: 44.18 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5', 'col_6', 'col_7',
'col_8', 'col_9']
Total time taken by PCA: 6.88 sec
```
19: AutoML in teradataml

```python
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Starting customized hyperparameter update ...
Skipping customized hyperparameter tuning
Hyperparameters used for model training:
response_column : price
name : xgboost
model_type : Regression
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1, 0.2, 0.3)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.01, 0.05, 0.1)
max_depth : (5, 3, 4, 7, 8)
min_node_size : (1, 2, 3, 4)
iter_num : (10, 20, 30, 40)
seed : 42
Total number of models for xgboost : 10240
response_column : price
name : decision_forest
tree_type : Regression
min_impurity : (0.0, 0.1, 0.2, 0.3)
max_depth : (5, 3, 4, 7, 8)
min_node_size : (1, 2, 3, 4)
num_trees : (-1, 20, 30, 40)
seed : 42
Total number of models for decision_forest : 320
Performing hyperparameter tuning ...
xgboost
```
19: AutoML in teradataml

```python
decision_forest
Leaderboard
RANK
MODEL_ID
FEATURE_SELECTION
MAE
MSE
MSLE
MAPE
MPE
RMSE
RMSLE
ME
R2
EV
MPD
MGD
ADJUSTED_R2
0
1
XGBOOST_1
rfe
8427.249621
1.394711e+08
0.024186
12.256896
-0.226853
11809.788825
0.155517
44860.075880
0.831416
0.835116
1668.854740
0.024415
0.828195
1
2
XGBOOST_0
lasso
8233.900404
1.412864e+08
0.024208
12.021986
0.890327
11886.393576
0.155589
49616.394501
0.829222
0.836897
1676.436224
0.024553
0.822569
2
3
XGBOOST_3
lasso
8233.900404
1.412864e+08
0.024208
12.021986
0.890327
11886.393576
0.155589
49616.394501
0.829222
0.836897
1676.436224
0.024553
0.822569
3
4
XGBOOST_2
pca
8762.454560
1.721874e+08
0.030721
13.421626
-2.003179
13122.018442
0.175275
60568.021287
0.791871
0.794088
2107.466538
0.030929
0.787443
4
5
DECISIONFOREST_1
rfe
9683.028747
2.273851e+08
0.032219
13.522362
0.032294
15079.293192
0.179496
71500.000000
0.725152
0.732519
2515.412163
0.032992
0.719900
5
6
DECISIONFOREST_0
lasso
9844.712596
2.469670e+08
0.033416
13.614288
0.124220
15715.184984
0.182801
71500.000000
0.701482
0.709846
2686.399136
0.034488
0.689852
6
7
DECISIONFOREST_3
lasso
10262.567624
2.878648e+08
0.036579
13.965556
-0.144662
16966.577888
0.191257
71945.000000
0.652048
0.668895
3111.956207
0.038856
0.638491
7
8
DECISIONFOREST_2
pca
11664.438993
3.793982e+08
0.056060
16.069311
1.031014
19478.147399
0.236771
84146.052632
0.541408
0.563668
4628.203132
0.064916
0.531651
```
19: AutoML in teradataml

```python
8 rows X 16 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 19/19
Received below input for customization :
{
"BincodeIndicator": true,
"BincodeParam": {
"bedrooms": {
"Type": "Variable-Width",
"NumOfBins": 2,
"Bin_1": {
"min_value": 0,
"max_value": 2,
"label": "small_house"
},
"Bin_2": {
"min_value": 3,
"max_value": 5,
"label": "big_house"
}
}
},
"CategoricalEncodingIndicator": true,
"CategoricalEncodingParam": {
"OrdinalEncodingIndicator": true,
"OrdinalEncodingList": [
"homestyle"
],
"TargetEncodingIndicator": true,
"TargetEncodingList": {
"prefarea": {
"encoder_method": "CBM_GAUSSIAN_INVERSE_GAMMA",
"response_column": "price"
}
}
},
"AntiselectIndicator": true,
"AntiselectParam": [
"sn"
```
19: AutoML in teradataml

```python
],
"TrainTestSplitIndicator": true,
"TrainingSize": 0.75,
"DataImbalanceIndicator": true,
"DataImbalanceMethod": "SMOTE",
"OutlierFilterIndicator": true,
"OutlierFilterMethod": "percentile",
"OutlierLowerPercentile": 0.1,
"OutlierUpperPercentile": 0.9,
"OutlierFilterParam": {
"bathrms": {
"replacement_value": "delete"
}
},
"FeatureScalingIndicator": true,
"FeatureScalingMethod": "std",
"HyperparameterTuningIndicator": true,
"HyperparameterTuningParam": {
"xgboost": {
"min_node_size": {
"Method": "ADD",
"Value": [
1,
2
]
}
}
}
}
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 492
Total Columns in the data: 14
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
recroom VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
```
19: AutoML in teradataml

```python
homestyle
VARCHAR(20) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
sn
INTEGER 492
0
None
0
492
0
0.0
100.0
price
FLOAT
492
0
None
0
492
0
0.0
100.0
prefarea
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
airco
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
stories INTEGER 492
0
None
0
492
0
0.0
100.0
fullbase
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
bedrooms
INTEGER 492
0
None
0
492
0
0.0
100.0
gashw
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
bathrms INTEGER 492
0
None
0
492
0
0.0
100.0
garagepl
INTEGER 492
0
None
270
222
0
0.0
100.0
lotsize FLOAT
492
0
None
0
492
0
0.0
100.0
driveway
VARCHAR(10) CHARACTER SET LATIN 492
0
0
None
None
None
0.0
100.0
Statistics of Data:
func
sn
price
lotsize bedrooms
bathrms stories garagepl
min
1
25000
1650
1
1
1
0
std
159.501 26472.496
2182.443
0.731
0.51
0.861
0.854
25%
132.5
49975
3600
2
1
1
0
50%
274
62000
4616
3
1
2
0
75%
413.25
82000
6370
3
2
2
1
max
546
190000
16200
6
4
4
3
mean
272.943 68100.396
5181.795
2.965
1.293
1.803
0.685
count
492
492
492
492
492
492
492
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
driveway                  2
recroom                   2
fullbase                  2
gashw                     2
airco                     2
prefarea                  2
homestyle                 3
No Futile columns found.
```
19: AutoML in teradataml

```python
Target Column Distribution:
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0    lotsize           2.235772
1   bedrooms           2.235772
2   garagepl           2.235772
3    stories           7.113821
4      price           2.439024
5    bathrms           0.203252
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.49 sec
Handling less significant features from data ...
Analysis indicates all categorical columns are significant. No action
Needed.
Total time to handle less significant features: 15.14 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
Proceeding with default option for missing value
imputation.
Proceeding with default option for handling remaining missing
values.
Checking Missing values in dataset ...
Analysis Completed. No Missing Values
Detected.
```
19: AutoML in teradataml

```python
Total time to find missing values in data: 7.37 sec
Imputing Missing Values ...
Analysis completed. No imputation
required.
Time taken to perform imputation: 0.01 sec
No information provided for Equal-Width
Transformation.
Variable-Width binning information:-
ColumnName
MinValue
MaxValue
Label
0
bedrooms
0
2
small_house
1
bedrooms
3
5
big_house
2 rows X 4 columns
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713816329745055"'0
Updated dataset sample after performing Variable-Width binning:
bathrms lotsize airco
gashw
garagepl
id
recroom sn
driveway
stories prefarea
fullbase
homestyle
price
bedrooms
3
4410.0
no
no
2
118
no
257
yes
2
no
yes
Eclectic
71000.0 big_house
3
8580.0
no
no
2
510
no
44
yes
2
no
no
Eclectic
92000.0 big_house
3
3630.0
no
no
0
92
yes
55
no
2
no
no
Classic 38000.0 big_house
3
2610.0
no
no
0
73
no
156
no
2
no
no
Eclectic
60000.0 big_house
3
7500.0
yes
no
2
338
no
338
yes
1
yes
yes
bungalow
155000.0
big_house
3
6000.0
no
yes
2
66
yes
217
yes
2
no
yes
bungalow
138300.0
big_house
3
3300.0
no
no
0
291
no
102
yes
2
no
yes
Eclectic
79000.0 big_house
1
10240.0 yes
no
2
96
no
421
yes
1
yes
no
Eclectic
68000.0 small_house
1
6000.0
yes
no
1
59
no
324
yes
1
no
no
Eclectic
98000.0 big_house
1
6060.0
no
no
0
123
yes
91
yes
1
no
yes
Classic 47000.0 big_house
```
19: AutoML in teradataml

```python
492 rows X 15 columns
Skipping customized string manipulation.⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾
```
* ｜  25% - 5/20
```python
Starting Customized Categorical Feature Encoding ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713816245888910"'0
Updated dataset sample after performing ordinal encoding:
bathrms bedrooms
lotsize gashw
garagepl
id
recroom sn
driveway
stories prefarea
airco
fullbase
price
homestyle
3
big_house
3630.0
no
0
92
yes
55
no
2
no
no
no
38000.0 1
3
big_house
8580.0
no
2
76
no
362
yes
4
yes
yes
no
145000.0
0
3
big_house
6000.0
yes
2
66
yes
217
yes
2
no
no
yes
138300.0
0
3
big_house
3300.0
no
0
291
no
102
yes
2
no
no
yes
79000.0 2
3
big_house
8580.0
no
2
510
no
44
yes
2
no
no
no
92000.0 2
3
big_house
4410.0
no
2
118
no
257
yes
2
no
no
yes
71000.0 2
3
big_house
5960.0
no
1
194
yes
127
yes
2
no
no
yes
117000.0
0
1
big_house
7020.0
no
2
262
no
388
yes
1
yes
yes
yes
85000.0 2
1
small_house
6800.0
no
2
121
yes
354
yes
1
no
no
yes
86000.0 2
1
small_house
3640.0
no
1
9
no
265
yes
1
no
no
no
50000.0 1
492 rows X 15 columns
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713814972077644"'0
Updated dataset sample after performing target encoding:
prefarea
bedrooms
bathrms lotsize gashw
garagepl
id
recroom sn
driveway
stories airco
homestyle
fullbase
price
62906.33597883598
big_house
1
3300.0
no
1
33
no
17
no
2
no
1
no
40500.0
```
19: AutoML in teradataml

```python
62906.33597883598
small_house
1
8400.0
no
1
495
no
494
yes
1
no
2
no
54000.0
62906.33597883598
big_house
1
4500.0
no
0
488
no
145
no
2
yes
2
yes
57250.0
62906.33597883598
big_house
1
4046.0
no
1
228
no
348
yes
2
no
2
yes
59500.0
62906.33597883598
small_house
1
2640.0
no
1
173
no
211
no
1
no
1
no
40500.0
62906.33597883598
big_house
1
5200.0
no
0
111
no
320
yes
3
yes
2
no
83000.0
83851.72413793103
big_house
1
2145.0
no
0
387
no
460
yes
2
no
1
yes
47000.0
83851.72413793103
small_house
1
10360.0 no
1
353
no
477
yes
1
no
2
no
61500.0
83851.72413793103
big_house
1
7000.0
no
2
360
no
399
yes
1
no
2
yes
82900.0
83851.72413793103
big_house
1
6600.0
no
3
93
no
360
yes
4
yes
0
no
107000.0
492 rows X 15 columns
Performing encoding for categorical columns ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713815195827284"'0
ONE HOT Encoding these Columns:
['bedrooms', 'gashw', 'recroom', 'driveway', 'airco', 'fullbase']
Sample of dataset after performing one hot encoding:
prefarea
bedrooms_0
bedrooms_1
bedrooms_2
bathrms
lotsize gashw_0 gashw_1 garagepl
id
recroom_0
recroom_1
sn
driveway_0
driveway_1
stories airco_0 airco_1 homestyle
fullbase_0
fullbase_1
price
83851.72413793103
1
0
0
1
7160.0
1
0
2
124
1
0
379
0
1
1
1
0
2
0
1
84000.0
83851.72413793103
1
0
0
1
5020.0
1
0
0
444
1
0
393
0
1
4
0
1
2
1
0
96000.0
83851.72413793103
1
0
0
1
3800.0
1
0
1
404
0
1
456
0
1
2
1
0
2
0
1
75000.0
83851.72413793103
1
0
0
1
3520.0
1
0
0
183
1
0
438
0
1
2
1
0
```
19: AutoML in teradataml

```python
2
1
0
60000.0
83851.72413793103
1
0
0
1
2880.0
1
0
0
257
1
0
424
0
1
2
1
0
2
1
0
62900.0
83851.72413793103
1
0
0
1
9620.0
1
0
2
393
1
0
391
0
1
1
1
0
2
0
1
86900.0
62906.33597883598
1
0
0
1
3300.0
1
0
1
33
1
0
17
1
0
2
1
0
1
1
0
40500.0
62906.33597883598
0
0
1
1
8400.0
1
0
1
495
1
0
494
0
1
1
1
0
2
1
0
54000.0
62906.33597883598
1
0
0
1
4500.0
1
0
0
488
1
0
145
1
0
2
0
1
2
0
1
57250.0
62906.33597883598
1
0
0
1
4046.0
1
0
1
228
1
0
348
0
1
2
1
0
2
0
1
59500.0
492 rows X 22 columns
Time taken to encode the columns: 13.96 sec
Starting customized mathematical transformation ...
Skipping customized mathematical
transformation.
Starting customized non-linear transformation ...
Skipping customized non-linear
transformation.
Starting customized anti-select columns ...
Updated dataset sample after performing anti-select columns:
prefarea
bedrooms_0
bedrooms_1
bedrooms_2
bathrms
lotsize gashw_0 gashw_1 garagepl
id
recroom_0
recroom_1
driveway_0
driveway_1
stories airco_0 airco_1 homestyle
fullbase_0
fullbase_1
price
83851.72413793103
1
0
0
1
7160.0
1
0
2
124
1
0
0
1
1
1
0
2
0
1
84000.0
83851.72413793103
1
0
0
1
5020.0
1
0
0
444
1
0
0
1
4
0
1
2
```
19: AutoML in teradataml

```python
1
0
96000.0
83851.72413793103
1
0
0
1
3800.0
1
0
1
404
0
1
0
1
2
1
0
2
0
1
75000.0
83851.72413793103
1
0
0
1
3520.0
1
0
0
183
1
0
0
1
2
1
0
2
1
0
60000.0
83851.72413793103
1
0
0
1
2880.0
1
0
0
257
1
0
0
1
2
1
0
2
1
0
62900.0
83851.72413793103
1
0
0
1
9620.0
1
0
2
393
1
0
0
1
1
1
0
2
0
1
86900.0
62906.33597883598
1
0
0
1
3300.0
1
0
1
33
1
0
1
0
2
1
0
1
1
0
40500.0
62906.33597883598
0
0
1
1
8400.0
1
0
1
495
1
0
0
1
1
1
0
2
1
0
54000.0
62906.33597883598
1
0
0
1
4500.0
1
0
0
488
1
0
1
0
2
0
1
2
0
1
57250.0
62906.33597883598
1
0
0
1
4046.0
1
0
1
228
1
0
0
1
2
1
0
2
0
1
59500.0
492 rows X 21 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
Spliting of dataset into training and testing ...
Training size :
0.75
Testing size  :
0.25
Training data sample
prefarea
bedrooms_0
bedrooms_1
bedrooms_2
bathrms
lotsize gashw_0 gashw_1 garagepl
id
recroom_0
recroom_1
driveway_0
driveway_1
stories airco_0 airco_1 homestyle
```
19: AutoML in teradataml

```python
fullbase_0
fullbase_1
price
62906.33597883598
0
0
1
1
6360.0
1
0
1
12
1
0
0
1
1
0
1
2
0
1
63900.0
62906.33597883598
1
0
0
1
8372.0
1
0
2
16
1
0
0
1
3
0
1
2
1
0
87000.0
62906.33597883598
1
0
0
2
4500.0
1
0
0
17
1
0
1
0
2
0
1
2
0
1
57000.0
62906.33597883598
1
0
0
1
6840.0
1
0
1
18
0
1
0
1
2
0
1
0
0
1
116000.0
62906.33597883598
1
0
0
1
5800.0
0
1
2
21
1
0
0
1
1
1
0
2
1
0
60000.0
62906.33597883598
0
0
1
1
3649.0
1
0
0
22
1
0
0
1
1
1
0
1
1
0
27000.0
83851.72413793103
0
0
1
1
5320.0
1
0
1
15
1
0
0
1
1
1
0
1
1
0
49500.0
83851.72413793103
1
0
0
2
6600.0
1
0
0
29
0
1
0
1
2
1
0
2
0
1
78000.0
83851.72413793103
1
0
0
1
11440.0 1
0
1
37
1
0
0
1
2
1
0
0
0
1
104900.0
83851.72413793103
1
0
0
1
6360.0
1
0
0
41
1
0
0
1
3
1
0
2
1
0
80000.0
369 rows X 21 columns
Testing data sample
prefarea
bedrooms_0
bedrooms_1
bedrooms_2
bathrms
lotsize gashw_0 gashw_1 garagepl
id
recroom_0
recroom_1
driveway_0
driveway_1
stories airco_0 airco_1 homestyle
fullbase_0
fullbase_1
price
62906.33597883598
1
0
0
2
8880.0
1
0
1
13
1
0
0
1
2
0
1
2
0
1
99000.0
62906.33597883598
1
0
0
1
2400.0
1
0
0
34
1
0
1
0
1
1
0
1
1
0
25245.0
```
19: AutoML in teradataml

```python
62906.33597883598
1
0
0
2
9800.0
1
0
2
36
0
1
0
1
2
1
0
2
1
0
75000.0
62906.33597883598
1
0
0
1
3000.0
1
0
0
38
1
0
0
1
2
1
0
2
1
0
56000.0
62906.33597883598
0
0
1
1
4040.0
1
0
1
67
1
0
0
1
2
1
0
2
1
0
58500.0
62906.33597883598
1
0
0
1
2970.0
1
0
0
72
1
0
0
1
3
1
0
2
1
0
70000.0
83851.72413793103
1
0
0
1
11460.0 1
0
2
19
1
0
0
1
3
1
0
2
1
0
83900.0
83851.72413793103
1
0
0
2
5500.0
1
0
1
27
1
0
0
1
2
0
1
0
0
1
120000.0
83851.72413793103
1
0
0
2
4880.0
1
0
1
40
1
0
0
1
2
0
1
0
1
0
118500.0
83851.72413793103
1
0
0
1
2145.0
1
0
0
75
1
0
0
1
3
1
0
1
1
0
49500.0
123 rows X 21 columns
Time taken for spliting of data: 14.48 sec
Starting customized outlier processing ...
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0   garagepl           2.235772
1      price           8.739837
2         id           9.756098
3    lotsize           9.552846
4    bathrms           2.235772
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713815695090274"'
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713
723333520"'20
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713
253699382"'
```
19: AutoML in teradataml

```python
Feature selection using lasso ...
feature selected by lasso:
['bathrms', 'fullbase_1', 'gashw_0', 'driveway_0', 'stories', 'airco_1',
'gashw_1', 'bedrooms_0', 'bedrooms_2', 'driveway_1', 'garagepl', 'recroom_0',
'fullbase_0', 'homestyle', 'airco_0', 'prefarea', 'lotsize']
Total time taken by feature selection: 1.43 sec
scaling Features of lasso data ...
columns that will be scaled:
['bathrms', 'stories', 'garagepl', 'homestyle', 'prefarea', 'lotsize']
Training dataset sample after scaling:
driveway_1
fullbase_1
price
bedrooms_0
gashw_0 recroom_0
id
driveway_0
fullbase_0
airco_1 airco_0 gashw_1 bedrooms_2
bathrms stories garagepl
homestyle
prefarea
lotsize
1
0
47000.0 1
1
1
56
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-0.6389834756925418
1
1
52000.0 1
1
1
26
0
0
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.7437609001391254
1
0
64000.0 1
1
1
245
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
0.3943732959927247
0.7474571831400929
-0.552679423766173
-0.521934821355
6
1
0
78000.0 1
1
0
175
0
1
1
0
0
0
-0.5698449326198071
2.5617608810097847
-0.7757094582336237
0.7474571831400929
-0.552679423766173
0.5022409040905186
0
0
40500.0 1
1
1
33
1
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
0.3943732959927247
-0.7516329215933895
-0.552679423766173
-0.87
119290284443
1
1
57000.0 0
1
0
100
0
0
0
1
0
1
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
1.8093671611394 -0.5172151175519175
1
0
57500.0 1
1
1
427
0
1
0
```
19: AutoML in teradataml

```python
1
0
0
-0.5698449326198071
-0.8999912756370636
2.734538804445422
0.7474571831400929
-0.552679423766173
0.11994489597460505
1
1
75000.0 1
1
1
116
0
0
1
0
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.41810133767001395
1
0
52000.0 0
1
1
28
0
1
1
0
0
1
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-1.012784016961435
0
0
46000.0 1
1
1
32
1
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-0.956147571314633
359 rows X 19 columns
Testing dataset sample after scaling:
driveway_1
fullbase_1
price
bedrooms_0
gashw_0 recroom_0
id
driveway_0
fullbase_0
airco_1 airco_0 gashw_1 bedrooms_2
bathrms stories garagepl
homestyle
prefarea
lotsize
0
0
25245.0 1
1
1
34
1
1
0
1
0
0
-0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-1.295966245195445
1
0
56000.0 1
1
1
38
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-1.012784016961435
0
0
47900.0 1
1
1
122
1
1
0
1
0
0
-0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-1.15437513107844
1
1
51000.0 1
1
1
140
0
0
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.9419884599029325
0
1
44000.0 1
1
1
452
1
0
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-1.409239136489049
1
1
60000.0 0
1
0
153
0
0
0
1
0
1
-0.5698449326198071
-0.8999912756370636
```
19: AutoML in teradataml

```python
1.5644560502190732
0.7474571831400929
-0.552679423766173
0.33233156715011253
1
0
47500.0 0
1
1
142
0
1
0
1
0
1
-0.5698449326198071
-0.8999912756370636
0.3943732959927247
-0.7516329215933895
-0.552679423766173
-0.52
19348213558176
1
0
59900.0 1
1
1
400
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
0.3943732959927247
0.7474571831400929
-0.552679423766173
-0.8003973457859275
1
0
92000.0 1
1
1
510
0
1
0
1
0
0
4.079571676709981
0.2539261099118858
1.5644560502190732
0.7474571831400929
-0.552679423766173
1.6208107056148582
0
1
70000.0 1
1
0
147
1
0
1
0
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.4959764504343667
123 rows X 19 columns
Total time taken by feature scaling: 54.15 sec
Feature selection using rfe ...
feature selected by RFE:
['bathrms', 'fullbase_1', 'gashw_0', 'driveway_0', 'stories', 'airco_1',
'gashw_1', 'bedrooms_0', 'bedrooms_2', 'driveway_1', 'garagepl', 'recroom_0',
'fullbase_0', 'homestyle', 'airco_0', 'recroom_1', 'prefarea', 'lotsize']
Total time taken by feature selection: 55.81 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_bathrms', 'r_stories', 'r_garagepl', 'r_homestyle', 'r_prefarea',
'r_lotsize']
Training dataset sample after scaling:
r_gashw_0
r_fullbase_1
r_driveway_1
r_recroom_1
r_bedrooms_0
r_bedrooms_2
id
r_recroom_0
r_gashw_1
r_driveway_0
r_airco_0
r_airco_1
r_fullbase_0
price
r_bathrms
r_stories
r_garagepl
r_homestyle
r_prefarea
r_lotsize
1
0
1
0
1
0
56
1
0
0
```
19: AutoML in teradataml

```python
1
0
1
47000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-0.6389834756925418
1
1
1
0
1
0
26
1
0
0
1
0
0
52000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.7437609001391254
1
0
1
0
1
0
245
1
0
0
1
0
1
64000.0 -0.5698449326198071
0.2539261099118858
0.3943732959927247
0.7474571831400929
-0.552679423766173
-0.5219348213558176
1
0
1
1
1
0
175
0
0
0
0
1
1
78000.0 -0.5698449326198071
2.5617608810097847
-0.7757094582336237
0.7474571831400929
-0.552679423766173
0.5022409040905186
1
0
0
0
1
0
33
1
0
1
1
0
1
40500.0 -0.5698449326198071
0.2539261099118858
0.3943732959927247
-0.7516329215933895
-0.552679423766173
-0.87
119290284443
1
1
1
1
0
1
100
0
0
0
1
0
0
57000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
1.8093671611394 -0.5172151175519175
1
0
1
0
1
0
427
1
0
0
1
0
1
57500.0 -0.5698449326198071
-0.8999912756370636
2.734538804445422
0.7474571831400929
-0.552679423766173
0.11994489597460505
1
1
1
0
1
0
116
1
0
0
0
1
0
75000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.41810133767001395
1
0
1
0
0
1
28
1
0
0
0
1
1
52000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-1.012784016961435
1
0
0
0
1
0
32
1
0
1
1
0
1
46000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-0.956147571314633
359 rows X 20 columns
Testing dataset sample after scaling:
r_gashw_0
r_fullbase_1
r_driveway_1
r_recroom_1
r_bedrooms_0
```
19: AutoML in teradataml

```python
r_bedrooms_2
id
r_recroom_0
r_gashw_1
r_driveway_0
r_airco_0
r_airco_1
r_fullbase_0
price
r_bathrms
r_stories
r_garagepl
r_homestyle
r_prefarea
r_lotsize
1
0
0
0
1
0
34
1
0
1
1
0
1
25245.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-1.295966245195445
1
0
1
0
1
0
38
1
0
0
1
0
1
56000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-1.012784016961435
1
0
0
0
1
0
122
1
0
1
1
0
1
47900.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-1.15437513107844
1
1
1
0
1
0
140
1
0
0
1
0
0
51000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.9419884599029325
1
1
0
0
1
0
452
1
0
1
1
0
0
44000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-1.409239136489049
1
1
1
1
0
1
153
0
0
0
1
0
0
60000.0 -0.5698449326198071
-0.8999912756370636
1.5644560502190732
0.7474571831400929
-0.552679423766173
0.33233156715011253
1
0
1
0
0
1
142
1
0
0
1
0
1
47500.0 -0.5698449326198071
-0.8999912756370636
0.3943732959927247
-0.7516329215933895
-0.552679423766173
-0.52
1934
3558176
1
0
1
0
1
0
400
1
0
0
1
0
1
59900.0 -0.5698449326198071
0.2539261099118858
0.3943732959927247
0.7474571831400929
-0.552679423766173
-0.8003973457859275
1
0
1
0
1
0
510
1
0
0
1
0
1
92000.0 4.079571676709981
0.2539261099118858
1.5644560502190732
0.7474571831400929
-0.552679423766173
1.6208107056148582
1
1
0
1
1
0
147
0
0
1
0
1
0
70000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.4959764504343667
```
19: AutoML in teradataml

```python
123 rows X 20 columns
Total time taken by feature scaling: 51.49 sec
scaling Features of pca data ...
columns that will be scaled:
['prefarea', 'bathrms', 'lotsize', 'garagepl', 'stories', 'homestyle']
Training dataset sample after scaling:
bedrooms_1
driveway_1
fullbase_1
price
bedrooms_0
gashw_0 recroom_0
id
driveway_0
recroom_1
fullbase_0
airco_1 airco_0 gashw_1 bedrooms_2
prefarea
bathrms lotsize
garagepl
stories homestyle
0
1
0
65500.0 1
1
1
44
0
0
1
0
1
0
0
1.8093671611393762
-0.5698449326198075
-0.6163288974338209
0.39437329599272386
0.2539261099118857
0.7474571831400939
0
1
0
54000.0 1
1
1
54
0
0
1
0
1
0
0
1.8093671611393762
-0.5698449326198075
-1.0807477517375974
-0.77
57094582336221
1.4078434954608345
0.7474571831400939
0
1
1
61100.0 1
1
1
58
0
0
0
0
1
0
0
1.8093671611393762
-0.5698449326198075
-0.8239958648054283
1.5644560502190699
0.2539261099118857
0.7474571831400939
0
1
0
51500.0 0
1
1
61
0
0
1
0
1
0
1
1.8093671611393762
-0.5698449326198075
-0.5408136365714183
-0.77
57094582336221
-0.8999912756370632
0.7474571831400939
0
1
1
95000.0 1
1
0
81
0
1
0
1
0
0
0
1.8093671611393762
-0.5698449326198075
0.26153601009161004
1.5644560502190699
-0.8999912756370632
0.7474571831400939
0
1
0
103500.0
1
1
0
88
0
1
1
1
0
0
0
1.8093671611393762
1.7548633720450884
1.8190382653786652
0.39437329599272386
2.5617608810097834
-2.2507230263268747
0
1
1
63900.0 0
1
1
12
0
0
0
1
0
0
1
-0.5526794237661955
-0.5698449326198075
0.5730364611490211
0.39437329599272386
-0.8999912756370632
0.7474571831400939
0
1
0
87000.0 1
1
1
16
0
0
1
1
0
0
```
19: AutoML in teradataml

```python
0
-0.5526794237661955
-0.5698449326198075
1.5226408664937345
1.5644560502190699
1.4078434954608345
0.7474571831400939
0
0
1
57000.0 1
1
1
17
1
0
0
1
0
0
0
-0.5526794237661955
1.7548633720450884
-0.30482844637640993
-0.77570945
36221
0.2539261099118857
0.7474571831400939
0
1
1
116000.0
1
1
0
18
0
1
0
1
0
0
0
-0.5526794237661955
-0.5698449326198075
0.7995822437362291
0.39437329599272386
0.2539261099118857
-2.2507230263268747
359 rows X 21 columns
Testing dataset sample after scaling:
bedrooms_1
driveway_1
fullbase_1
price
bedrooms_0
gashw_0 recroom_0
id
driveway_0
recroom_1
fullbase_0
airco_1 airco_0 gashw_1 bedrooms_2
prefarea
bathrms lotsize
garagepl
stories homestyle
0
1
1
99000.0 1
1
1
13
0
0
0
1
0
0
0
-0.5526794237661955
1.7548633720450884
1.7624018197318632
0.39437329599272386
0.2539261099118857
0.7474571831400939
0
0
0
25245.0 1
1
1
34
1
0
1
0
1
0
0
-0.5526794237661955
-0.5698449326198075
-1.295966245195445
-0.77570945
36221
-0.8999912756370632
-0.7516329215933905
0
1
0
75000.0 1
1
0
36
0
1
1
0
1
0
0
-0.5526794237661955
1.7548633720450884
2.1966145696906785
1.5644560502190699
0.2539261099118857
0.7474571831400939
0
1
0
56000.0 1
1
1
38
0
0
1
0
1
0
0
-0.5526794237661955
-0.5698449326198075
-1.012784016961435
-0.77570945
36221
0.2539261099118857
0.7474571831400939
0
1
0
58500.0 0
1
1
67
0
0
1
0
1
0
1
-0.5526794237661955
-0.5698449326198075
-0.5219348213558176
0.39437329599272386
0.2539261099118857
0.7474571831400939
0
1
0
70000.0 1
1
1
72
0
0
1
0
1
0
0
-0.5526794237661955
-0.5698449326198075
-1.0269431283731354
-0.77570945
36221
1.4078434954608345
0.7474571831400939
0
1
0
83900.0 1
1
1
19
0
0
1
0
1
0
0
```
19: AutoML in teradataml

```python
1.8093671611393762
-0.5698449326198075
2.980085401138106
1.5644560502190699
1.4078434954608345
0.7474571831400939
0
1
1
120000.0
1
1
1
27
0
0
0
1
0
0
0
1.8093671611393762
1.7548633720450884
0.16714193401360672
0.39437329599272386
0.2539261099118857
-2.2507230263268747
0
1
0
118500.0
1
1
1
40
0
0
1
1
0
0
0
1.8093671611393762
1.7548633720450884
-0.12547970182820362
0.39437329599272386
0.2539261099118857
-2.2507230263268747
0
1
0
49500.0 1
1
1
75
0
0
1
0
1
0
0
1.8093671611393762
-0.5698449326198075
-1.4163186921948991
-0.77
57094582336221
1.4078434954608345
-0.7516329215933905
123 rows X 21 columns
Total time taken by feature scaling: 50.44 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5', 'col_6', 'col_7',
'col_8', 'col_9']
Total time taken by PCA: 10.63 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Starting customized hyperparameter update ...
Completed customized hyperparameter update.
Hyperparameters used for model training:
response_column :
price
name : xgboost
model_type : Regression
column_sampling : (1, 0.6)
```
19: AutoML in teradataml

```python
min_impurity : (0.0, 0.1, 0.2, 0.3)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.01, 0.05, 0.1)
max_depth : (5, 3, 4, 7, 8)
min_node_size : (1, 2, 3, 4)
iter_num : (10, 20, 30, 40)
Total number of models for xgboost : 10240
response_column : price
name : decision_forest
tree_type : Regression
min_impurity : (0.0, 0.1, 0.2, 0.3)
max_depth : (5, 3, 4, 7, 8)
min_node_size : (1, 2, 3, 4)
num_trees : (-1, 20, 30, 40)
Total number of models for decision_forest : 320
Performing hyperParameter tuning ...
xgboost
decision_forest
Evaluating models performance ...
Evaluation completed.
Leaderboard
Rank
Model-ID
Feature-Selection
MAE
MSE
MSLE
RMSE
RMSLE
R2-score
Adjusted R2-score
0
1
XGBOOST_2
pca
9742.422518
2.064480e+08
0.035378
14368.297209
0.188091
0.690732
0.663119
```
19: AutoML in teradataml

```python
1
2
DECISIONFOREST_1
rfe
11097.622077
2.093546e+08
0.041378
14469.090168
0.203416
0.686378
0.632097
2
3
XGBOOST_1
rfe
11162.817285
2.219267e+08
0.050646
14897.203438
0.225047
0.667544
0.610004
3
4
DECISIONFOREST_2
pca
10892.666297
2.371269e+08
0.041082
15398.924619
0.202686
0.644773
0.613057
4
5
DECISIONFOREST_0
lasso
12588.499123
3.056303e+08
0.048797
17482.284634
0.220900
0.542152
0.468025
5
6
XGBOOST_3
lasso
12186.337469
3.168533e+08
0.047248
17800.374075
0.217365
0.525340
0.448490
6
7
XGBOOST_0
lasso
12186.337469
3.168533e+08
0.047248
17800.374075
0.217365
0.525340
0.448490
7
8
DECISIONFOREST_3
lasso
14983.007904
4.247282e+08
0.066943
20608.935846
0.258733
0.363738
0.260724
8 rows X 10 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 20/20
```
5.
* Display model leaderboard.
```python
aml.leaderboard()
RANK
MODEL_ID
FEATURE_SELECTION
MAE
MSE
MSLE
MAPE
MPE
RMSE
RMSLE
ME
R2
EV
MPD
MGD
ADJUSTED_R2
0
1
XGBOOST_1
rfe
8427.249621
1.394711e+08
0.024186
12.256896
-0.226853
11809.788825
0.155517
44860.075880
0.831416
0.835116
1668.854740
0.024415
0.828195
1
2
XGBOOST_0
lasso
8233.900404
1.412864e+08
0.024208
12.021986
0.890327
11886.393576
0.155589
49616.394501
0.829222
0.836897
1676.436224
0.024553
0.822569
2
3
XGBOOST_3
lasso
8233.900404
1.412864e+08
0.024208
12.021986
0.890327
11886.393576
0.155589
49616.394501
0.829222
0.836897
1676.436224
0.024553
0.822569
3
4
XGBOOST_2
pca
8762.454560
1.721874e+08
0.030721
13.421626
-2.003179
13122.018442
0.175275
60568.021287
0.791871
0.794088
2107.466538
0.030929
```
19: AutoML in teradataml

```python
0.787443
4
5
DECISIONFOREST_1
rfe
9683.028747
2.273851e+08
0.032219
13.522362
0.032294
15079.293192
0.179496
71500.000000
0.725152
0.732519
2515.412163
0.032992
0.719900
5
6
DECISIONFOREST_0
lasso
9844.712596
2.469670e+08
0.033416
13.614288
0.124220
15715.184984
0.182801
71500.000000
0.701482
0.709846
2686.399136
0.034488
0.689852
6
7
DECISIONFOREST_3
lasso
10262.567624
2.878648e+08
0.036579
13.965556
-0.144662
16966.577888
0.191257
71945.000000
0.652048
0.668895
3111.956207
0.038856
0.638491
7
8
DECISIONFOREST_2
pca
11664.438993
3.793982e+08
0.056060
16.069311
1.031014
19478.147399
0.236771
84146.052632
0.541408
0.563668
4628.203132
0.064916
0.531651
Rank
Model-ID
Feature-Selection
MAE
MSE
MSLE
RMSE
RMSLE
R2-score
Adjusted R2-score
0
1
XGBOOST_2
pca
9742.422518
2.064480e+08
0.035378
14368.297209
0.188091
0.690732
0.663119
1
2
DECISIONFOREST_1
rfe
11097.622077
2.093546e+08
0.041378
14469.090168
0.203416
0.686378
0.632097
2
3
XGBOOST_1
rfe
11162.817285
2.219267e+08
0.050646
14897.203438
0.225047
0.667544
0.610004
3
4
DECISIONFOREST_2
pca
10892.666297
2.371269e+08
0.041082
15398.924619
0.202686
0.644773
0.613057
4
5
DECISIONFOREST_0
lasso
12588.499123
3.056303e+08
0.048797
17482.284634
0.220900
0.542152
0.468025
5
6
XGBOOST_3
lasso
12186.337469
3.168533e+08
0.047248
17800.374075
0.217365
0.525340
0.448490
6
7
XGBOOST_0
lasso
12186.337469
3.168533e+08
0.047248
17800.374075
0.217365
0.525340
0.448490
7
8
DECISIONFOREST_3
lasso
14983.007904
4.247282e+08
0.066943
20608.935846
0.258733
0.363738
0.260724
```
6.
* Display the best performing model.
```python
aml.leader()
RANK   MODEL_ID    FEATURE_SELECTION  MAE          MSE            MSLE
MAPE        MPE         RMSE         RMSLE      ME           R2
EV        MPD         MGD       ADJUSTED_R2
```
19: AutoML in teradataml

```python
0     1      XGBOOST_1   rfe                8427.249621  1.394711e+08
0.024186  12.256896   -0.226853   11809.788825 0.155517   44860.07588
0.831416  0.835116  1668.85474  0.024415  0.
195
Rank
Model-ID
Feature-Selection
MAE
MSE
MSLE
RMSE
RMSLE
R2-score
Adjusted R2-score
0
1
XGBOOST_2
pca
9742.422518
2.064480e+08
0.035378
14368.297209
0.188091
0.690732
0.663119
```
7.
* Generate prediction on validation dataset using best performing model.
* Note:
* In the data preparation phase, AutoML generates the validation dataset by splitting the data
* provided during fitting into training and testing sets. AutoML's model training utilizes the training
* data, with the testing data acting as the validation dataset for model evaluation.
```python
prediction = aml.predict()
Following model is being used for generating prediction :
Model ID : XGBOOST_2
Feature Selection Method : pca
Prediction :
id     Prediction  Confidence_Lower  Confidence_upper     price
0  10   63809.865056      33717.239917      93902.490195   54500.0
1  13   92935.646586      46824.448096     139046.845076   99000.0
2  40  105986.892125      59432.832713     152540.951537  118500.0
3  24   79861.408889      40881.522757     118841.295022   99000.0
4  34   38466.813258      21251.107246      55682.519270   25245.0
5  97   98560.011426      47662.676958     149457.345895  106000.0
6  75   58383.595048      29686.945507      87080.244588   49500.0
7  27   96430.203680      51016.647630     141843.759730  120000.0
8  19   86654.297469      44430.989001     128877.605937   83900.0
9   9   41575.195612      22618.003199      60532.388024   50000.0
Performance Metrics :
MAE           MSE      MSLE       MAPE       MPE
RMSE     RMSLE            ME        R2        EV          MPD       MGD
0  9742.422518  2.064480e+08  0.035378  13.954839  0.188855  14368.297209
0.188091  63470.842062  0.690732  0.704693  2592.517245  0.036744
prediction.head()
```
19: AutoML in teradataml

```python
id
Prediction
Confidence_Lower
Confidence_upper
price
13
92935.64658575002
46824.44809589475
139046.84507560526
99000.0
24
79861.40888912501
40881.52275672913
118841.2950215209
99000.0
27
96430.20368
51016.64762957162
141843.7597304284
120000.0
34
38466.813258
21251.10724581299
55682.51927018701
25245.0
38
59323.84226600001
34078.64692764714
84569.03760435287
56000.0
40
105986.892125125
59432.832712888114
152540.9515373619
118500.0
36
89466.34093637503
45426.43042440514
133506.2514483449
75000.0
19
86654.29746912501
44430.989000780464
128877.60593746956
83900.0
10
63809.86505600001
33717.23991664394
93902.49019535608
54500.0
9
41575.195611500014
22618.00319854991
60532.38802445012
50000.0
```
8.
* Generate prediction on validation dataset using third best performing model.
```python
prediction = aml.predict(rank=3)
Following model is being used for generating prediction :
Model ID : XGBOOST_1
Feature Selection Method : rfe
Prediction :
id    Prediction  Confidence_Lower  Confidence_upper    price
0   34  35023.283882      -4837.186404      74883.754169  25245.0
1   38  55898.738253      -8963.818351     120761.294857  56000.0
2  122  35023.283882      -4837.186404      74883.754169  47900.0
3  140  55049.909953      -9742.813129     119842.633036  51000.0
4  452  37547.897706      -4255.979615      79351.775028  44000.0
5  153  67706.129535      -7797.877417     143210.136487  60000.0
6  142  53574.225636     -10035.903135     117184.354408  47500.0
7  400  58698.929647      -7337.989775     124735.849070  59900.0
8  510  74609.789819     -17587.700400     166807.280037  92000.0
9  147  76221.930629     -11523.433164     163967.294423  70000.0
Performance Metrics :
MAE           MSE      MSLE      MAPE       MPE
RMSE     RMSLE            ME        R2        EV          MPD       MGD
```
19: AutoML in teradataml

```python
0  11162.817285  2.219267e+08  0.050646  18.09273 -3.630758  14897.203438
0.225047  71234.841896  0.667544  0.668922  3045.942002  0.047478
prediction.head()
id
Prediction
Confidence_Lower
Confidence_upper
price
13
88394.47405500001
-13505.516260407865
190294.4643704079
99000.0
24
70578.8232925
-5948.346164785151
147105.99274978516
99000.0
27
104401.63842549999
-12692.073419267515
221495.3502702675
120000.0
34
35023.2838825
-4837.186403614331
74883.75416861434
25245.0
38
55898.738252999996
-8963.81835091576
120761.29485691575
56000.0
40
107643.55437249999
-17549.808679008158
232836.91742400813
118500.0
36
72512.602488
-14861.542217641778
159886.7471936418
75000.0
19
89922.82769049998
-10263.101185138774
190108.75656613873
83900.0
10
51887.02052199999
-10237.521547671604
114011.5625916716
54500.0
9
36789.518904
-6212.072251334146
79791.11005933414
50000.0
```
9.
* Display hyperparameters for trained model.
* a.
    * Display model hyperparameters for rank 2.
```python
aml.model_hyperparameters(rank=2)
{'response_column': 'price',
'name': 'xgboost',
'model_type': 'Regression',
'column_sampling': 1,
'min_impurity': 0.0,
'lambda1': 0.01,
'shrinkage_factor': 0.5,
'max_depth': 5,
'min_node_size': 1,
'iter_num': 10,
'seed': 42,
'persist': False}
```
19: AutoML in teradataml

* b.
    * Display model hyperparameters for rank 3.
```python
aml.model_hyperparameters(rank=3)
{'response_column': 'price',
'name': 'xgboost',
'model_type': 'Regression',
'column_sampling': 1,
'min_impurity': 0.0,
'lambda1': 0.01,
'shrinkage_factor': 0.5,
'max_depth': 5,
'min_node_size': 1,
'iter_num': 20,
'seed': 42,
'persist': False}
```
10. Generate prediction on test dataset using best performing model.
```python
prediction = aml.predict(housing_test)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after performing customized variable width bin-code
transformation :
bathrms stories gashw
airco
sn
fullbase
id
homestyle
lotsize garagepl
prefarea
driveway
recroom price
bedrooms
1
1
no
no
443
no
51
Eclectic
3520.0
0
yes
yes
no
65000.0 big_house
1
2
no
no
469
no
8
Eclectic
2176.0
0
yes
yes
yes
55000.0 small_house
1
2
no
yes
440
no
19
Eclectic
6862.0
2
yes
yes
no
69000.0 big_house
1
1
no
yes
401
yes
40
Eclectic
7410.0
2
yes
yes
yes
92500.0 big_house
1
1
no
no
25
no
48
Classic 4960.0
0
no
yes
no
42000.0 small_house
1
1
no
no
251
yes
14
Classic 3450.0
2
no
yes
no
48500.0 big_house
1
1
no
yes
16
no
23
Classic 3185.0
0
no
yes
no
37900.0 small_house
```
19: AutoML in teradataml

```python
1
1
no
no
301
no
9
Eclectic
4080.0
0
no
yes
no
55000.0 small_house
1
1
no
no
411
yes
33
Eclectic
9000.0
1
yes
yes
no
90000.0 big_house
1
2
no
yes
161
no
49
Eclectic
3162.0
1
no
yes
no
63900.0 big_house
46 rows X 15 columns
Updated dataset after performing customized categorical encoding :
prefarea
bathrms stories gashw
airco
sn
fullbase
id
homestyle
lotsize bedrooms
garagepl
driveway
recroom price
62906.33597883598
1
1
no
yes
53
yes
11
2
9166.0
small_house
2
yes
no
68000.0
62906.33597883598
1
2
no
no
140
no
43
1
3750.0
big_house
0
yes
no
43000.0
62906.33597883598
1
2
no
no
255
no
15
2
4360.0
big_house
0
yes
no
61000.0
62906.33597883598
1
2
no
no
364
yes
16
2
10700.0 big_house
0
yes
yes
72000.0
62906.33597883598
1
2
yes
no
117
no
37
2
3760.0
big_house
2
yes
no
93000.0
62906.33597883598
1
2
no
no
237
no
53
1
3630.0
big_house
3
yes
no
43000.0
83851.72413793103
1
1
no
yes
401
yes
40
2
7410.0
big_house
2
yes
yes
92500.0
83851.72413793103
1
1
no
no
411
yes
33
2
9000.0
big_house
1
yes
no
90000.0
83851.72413793103
1
2
no
no
463
yes
13
1
2610.0
big_house
0
yes
no
49000.0
83851.72413793103
1
3
no
no
408
yes
22
2
6420.0
big_house
0
yes
no
87500.0
46 rows X 15 columns
Updated dataset after performing categorical encoding :
prefarea
bathrms stories gashw_0 gashw_1 airco_0 airco_1 sn
fullbase_0
fullbase_1
id
homestyle
lotsize bedrooms_0
bedrooms_1
garagepl
driveway_0
driveway_1
recroom_0
recroom_1
price
62906.33597883598
1
2
1
0
1
0
255
1
0
15
2
4360.0
1
0
0
0
1
1
0
61000.0
```
19: AutoML in teradataml

```python
62906.33597883598
1
2
0
1
1
0
117
1
0
37
2
3760.0
1
0
2
0
1
1
0
93000.0
62906.33597883598
1
2
1
0
1
0
237
1
0
53
1
3630.0
1
0
3
0
1
1
0
43000.0
62906.33597883598
1
2
1
0
1
0
13
1
0
17
1
1700.0
1
0
0
0
1
1
0
27000.0
62906.33597883598
1
2
0
1
1
0
198
1
0
20
1
4350.0
1
0
1
1
0
1
0
40500.0
62906.33597883598
1
1
1
0
1
0
234
1
0
36
1
3970.0
0
1
0
1
0
1
0
32500.0
83851.72413793103
1
2
1
0
1
0
463
0
1
13
1
2610.0
1
0
0
0
1
1
0
49000.0
83851.72413793103
1
1
1
0
1
0
441
1
0
39
2
3520.0
1
0
2
0
1
1
0
51900.0
83851.72413793103
1
2
1
0
1
0
469
1
0
8
2
2176.0
0
1
0
0
1
0
1
55000.0
83851.72413793103
1
1
1
0
1
0
472
0
1
27
2
2787.0
1
0
0
0
1
1
0
60500.0
46 rows X 21 columns
Updated dataset after performing customized anti-selection :
prefarea
bathrms stories gashw_0 gashw_1 airco_0 airco_1 fullbase_0
fullbase_1
id
homestyle
lotsize bedrooms_0
bedrooms_1
garagepl
driveway_0
driveway_1
recroom_0
recroom_1
price
62906.33597883598
1
2
1
0
1
0
1
0
15
2
4360.0
1
0
0
0
1
1
0
61000.0
62906.33597883598
1
2
0
1
1
0
1
0
37
2
3760.0
1
0
2
0
1
1
0
93000.0
62906.33597883598
1
2
1
0
1
0
1
0
53
1
3630.0
1
0
3
0
1
1
0
43000.0
```
19: AutoML in teradataml

```python
62906.33597883598
1
2
1
0
1
0
1
0
17
1
1700.0
1
0
0
0
1
1
0
27000.0
62906.33597883598
1
2
0
1
1
0
1
0
20
1
4350.0
1
0
1
1
0
1
0
40500.0
62906.33597883598
1
1
1
0
1
0
1
0
36
1
3970.0
0
1
0
1
0
1
0
32500.0
83851.72413793103
1
2
1
0
1
0
0
1
13
1
2610.0
1
0
0
0
1
1
0
49000.0
83851.72413793103
1
1
1
0
1
0
1
0
39
2
3520.0
1
0
2
0
1
1
0
51900.0
83851.72413793103
1
2
1
0
1
0
1
0
8
2
2176.0
0
1
0
0
1
0
1
55000.0
83851.72413793103
1
1
1
0
1
0
0
1
27
2
2787.0
1
0
0
0
1
1
0
60500.0
46 rows X 20 columns
Performing transformation carried out in data preparation phase ...
Updated dataset after performing Lasso feature selection:
id
recroom_1
stories gashw_0 bedrooms_0
garagepl
fullbase_1
bathrms airco_0 recroom_0
airco_1 driveway_1
gashw_1 bedrooms_1
homestyle
fullbase_0
driveway_0
prefarea
lotsize price
24
1
1
1
0
0
1
2
1
0
0
1
0
1
2
0
0
62906.336
4100.0
64900.0
32
1
1
1
1
0
1
1
0
0
1
1
0
0
2
0
0
83851.7241
6825.0
77500.0
8
1
2
1
0
0
0
1
1
0
0
1
0
1
2
1
0
83851.7241
2176.0
55000.0
27
0
1
1
1
0
1
1
1
1
0
1
0
0
2
0
0
83851.7241
2787.0
60500.0
21
0
1
1
1
0
0
1
1
1
```
19: AutoML in teradataml

```python
0
1
0
0
1
1
0
83851.7241
2398.0
44555.0
22
0
3
1
1
0
1
1
1
1
0
1
0
0
2
0
0
83851.7241
6420.0
87500.0
11
0
1
1
0
2
1
1
0
1
1
1
0
1
2
0
0
62906.336
9166.0
68000.0
43
0
2
1
1
0
0
1
1
1
0
1
0
0
1
1
0
62906.336
3750.0
43000.0
15
0
2
1
1
0
0
1
1
1
0
1
0
0
2
1
0
62906.336
4360.0
61000.0
37
0
2
0
1
2
0
1
1
1
0
1
1
0
2
1
0
62906.336
3760.0
93000.0
46 rows X 20 columns
Updated dataset after performing scaling on Lasso selected features :
recroom_1
airco_0 recroom_0
gashw_0 airco_1 id
driveway_1
gashw_1 bedrooms_1
fullbase_0
bedrooms_0
fullbase_1
price
driveway_0
stories garagepl
bathrms homestyle
prefarea
lotsize
0
1
1
1
0
21
1
0
0
1
1
0
44555.0
0
-0.9243489557048288
-0.7962575788492099
-0.5797509043642046
-0.7435145661134329
1.8046155180928998
-1.2954779865747776
0
0
1
1
1
11
1
0
1
0
0
1
68000.0 0
-0.9243489557048288
1.5752588015004494
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
1.898787906000196
0
1
1
1
0
43
1
0
0
1
1
0
43000.0 0
0.24261127446320968
-0.7962575788492099
-0.5797509043642046
-0.74
35145661134329
-0.5541346563708683
-0.6573799536608644
0
1
1
1
0
15
1
0
0
1
1
0
61000.0 0
0.24261127446320968
-0.7962575788492099
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
-0.369480693248522
0
1
1
1
0
53
1
0
0
1
1
0
43000.0 0
0.24261127446320968
2.761016991675279
-0.5797509043642046
-0.7435145661134329
-0.55
```
19: AutoML in teradataml

```python
41346563708683
-0.714015873741981
0
1
1
1
0
17
1
0
0
1
1
0
27000.0 0
0.24261127446320968
-0.7962575788492099
-0.5797509043642046
-0.74
35145661134329
-0.5541346563708683
-1.6249102550466057
1
0
0
1
1
40
1
0
0
0
1
1
92500.0 0
-0.9243489557048288
1.5752588015004494
-0.5797509043642046
0.7342974433930187
1.8046155180928998
1.0700156088131905
1
1
0
1
0
35
0
0
0
0
1
1
78500.0 1
0.24261127446320968
0.38950061132561975
1.7248787237282124
0.7342974433930187
-0.5541346563708683
-1.0977242322915457
1
1
0
1
0
24
1
0
1
0
0
1
64900.0 0
-0.9243489557048288
-0.7962575788492099
1.7248787237282124
0.7342974433930187
-0.5541346563708683
-0.4921918534242745
1
1
0
1
0
16
1
0
0
0
1
1
72000.0 0
0.24261127446320968
-0.7962575788492099
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
2.622783751037136
46 rows X 20 columns
Updated dataset after performing RFE feature selection:
id
stories garagepl
bathrms airco_0 bedrooms_1
homestyle
fullbase_0
prefarea
lotsize price
24
1
0
2
1
1
2
0
62906.336
4100.0
64900.0
32
1
0
1
0
0
2
0
83851.7241
6825.0
77500.0
8
2
0
1
1
1
2
1
83851.7241
2176.0
55000.0
27
1
0
1
1
0
2
0
83851.7241
2787.0
60500.0
21
1
0
1
1
0
1
1
83851.7241
2398.0
44555.0
22
3
0
1
1
0
2
0
83851.7241
6420.0
87500.0
11
1
2
1
0
1
2
0
62906.336
9166.0
68000.0
43
2
0
1
1
0
1
1
62906.336
3750.0
43000.0
15
2
0
1
1
0
2
1
62906.336
```
19: AutoML in teradataml

```python
4360.0
61000.0
37
2
2
1
1
0
2
1
62906.336
3760.0
93000.0
46 rows X 11 columns
Updated dataset after performing scaling on RFE selected features :
r_bedrooms_1
r_fullbase_0
id
price
r_airco_0
r_stories
r_garagepl
r_bathrms
r_homestyle
r_prefarea
r_lotsize
1
0
24
64900.0
1
-0.9243489557048288
-0.7962575788492099
1.7248787237282124
0.7342974433930187
-0.5541346563708683
-0.4921918534242745
0
0
32
77500.0
0
-0.9243489557048288
-0.7962575788492099
-0.5797509043642046
0.7342974433930187
1.8046155180928998
0.7939154984177472
1
1
8
55000.0 1
0.24261127446320968
-0.7962575788492099
-0.5797509043642046
0.7342974433930187
1.8046155180928998
-1.4002544387248432
0
0
27
60500.0
1
-0.9243489557048288
-0.7962575788492099
-0.5797509043642046
0.7342974433930187
1.8046155180928998
-1.1118832123118247
0
1
21
44555.0
1
-0.9243489557048288
-0.7962575788492099
-0.5797509043642046
-0.7435145661134329
1.8046155180928998
-1.2954779865747776
0
0
22
87500.0 1
1.4095715046312483
-0.7962575788492099
-0.5797509043642046
0.7342974433930187
1.8046155180928998
0.6027692681439788
1
0
11
68000.0 0
-0.9243489557048288
1.5752588015004494
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
1.898787906000196
0
1
43
43000.0 1
0.24261127446320968
-0.7962575788492099
-0.5797509043642046
-0.74
35145661134329
-0.5541346563708683
-0.6573799536608644
0
1
15
61000.0 1
0.24261127446320968
-0.7962575788492099
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
-0.369480693248522
0
1
37
93000.0 1
0.24261127446320968
1.5752588015004494
-0.5797509043642046
0.7342974433930187
-0.5541346563708683
-0.6526602936541048
46 rows X 11 columns
Updated dataset after performing scaling for PCA feature selection :
recroom_1
airco_0 recroom_0
gashw_0 airco_1 id
driveway_1
```
19: AutoML in teradataml

```python
gashw_1 bedrooms_1
fullbase_0
bedrooms_0
fullbase_1
price
driveway_0
prefarea
bathrms stories homestyle
lotsize
garagepl
1
1
0
1
0
24
1
0
1
0
0
1
64900.0 0
-0.5541346539875237
1.7248787237282066
-0.9243489557048274
0.7342974433930172
-0.49219185342427485
-0.7962575788492109
1
0
0
1
1
32
1
0
0
0
1
1
77500.0 0
1.804615513821303
-0.5797509043642026
-0.9243489557048274
0.7342974433930172
0.7939154984177478
-0.7962575788492109
1
1
0
1
0
8
1
0
1
1
0
0
55000.0 0
1.804615513821303
-0.5797509043642026
0.24261127446320932
0.7342974433930172
-1.4002544387248441
-0.7962575788492109
0
1
1
1
0
27
1
0
0
0
1
1
60500.0 0
1.804615513821303
-0.5797509043642026
-0.9243489557048274
0.7342974433930172
-1.1118832123118256
-0.7962575788492109
0
1
1
1
0
21
1
0
0
1
1
0
44555.0 0
1.804615513821303
-0.5797509043642026
-0.9243489557048274
-0.74
35145661134314
-1.2954779865747785
-0.7962575788492109
0
1
1
1
0
22
1
0
0
0
1
1
87500.0 0
1.804615513821303
-0.5797509043642026
1.409571504631246
0.7342974433930172
0.6027692681439792
-0.7962575788492109
0
0
1
1
1
11
1
0
1
0
0
1
68000.0
0
-0.5541346539875237
-0.5797509043642026
-0.9243489557048274
0.7342974433930172
1.8987879060001973
1.5752588015004514
0
1
1
1
0
43
1
0
0
1
1
0
43000.0 0
-0.5541346539875237
-0.5797509043642026
0.24261127446320932
-0.7435145661134314
-0.6573799536608649
-0.79
62575788492109
0
1
1
1
0
15
1
0
0
1
1
0
61000.0 0
-0.5541346539875237
-0.5797509043642026
0.24261127446320932
0.7342974433930172
-0.36948069324852223
-0.7962575788492109
0
1
1
0
0
37
1
1
0
1
1
0
93000.0 0
-0.5541346539875237
-0.5797509043642026
0.24261127446320932
0.7342974433930172
-0.6526602936541052
1.5752588015004514
```
19: AutoML in teradataml

```python
46 rows X 20 columns
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
col_6
col_7
col_8
col_9
price
0
40
1.517005
2.635827
-0.254247
0.423657
-0.071790
-0.699173
1.123907
0.539062
-0.023292
0.589272
92500.0
1
27
-0.907076
1.115731
-1.753948
1.240695
0.341903
-0.906526
-0.000200
-0.165580
-0.384095
-0.464056
60500.0
2
35
0.199440
-0.655693
-0.973420
-1.287750
1.822717
-0.705320
0.523943
-0.663558
-0.058868
1.033697
78500.0
3
51
-0.831862
0.983331
-1.367625
1.188594
-0.213138
-0.572543
-0.901387
0.190808
-0.489312
0.225768
65000.0
4
24
-0.537790
0.121912
-0.965847
-0.607395
2.236753
0.806179
0.160764
-0.209506
1.265151
0.373994
64900.0
5
21
-0.851571
0.134084
-0.639709
2.122880
0.145022
-0.935130
-0.694137
0.133201
-0.479635
0.151287
44555.0
6
16
0.634760
1.141951
-0.301314
-0.451965
-0.887044
2.253530
1.215956
-1.351005
0.170636
0.298380
72000.0
7
22
0.788204
0.450834
-1.910249
0.856887
-1.306560
-0.058988
0.067773
-0.863751
0.045674
-0.688628
87500.0
8
32
0.441422
1.783878
-1.511197
1.200858
-0.102227
0.551105
1.190928
0.583395
-0.100689
0.490966
77500.0
9
11
0.665806
2.229285
1.326236
-0.984332
-0.103284
0.660512
0.754063
0.745793
0.443762
-0.770841
68000.0
10 rows X 12 columns
Data Transformation completed.⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 15/15
Following model is being picked for evaluation:
Model ID : XGBOOST_1
Feature Selection Method : rfe
Prediction :
```
19: AutoML in teradataml

```python
id    Prediction  Confidence_Lower  Confidence_upper    price
0  24  60493.951359      60493.951359      60493.951359  64900.0
1  32  79532.256218      79532.256218      79532.256218  77500.0
2   8  55241.854455      55241.854455      55241.854455  55000.0
3  27  60479.372933      60479.372933      60479.372933  60500.0
4  21  37397.395472      37397.395472      37397.395472  44555.0
5  22  88903.303042      88903.303042      88903.303042  87500.0
6  11  71834.288947      71834.288947      71834.288947  68000.0
7  43  42734.457781      42734.457781      42734.457781  43000.0
8  15  61650.265369      61650.265369      61650.265369  61000.0
9  37  64685.898547      64685.898547      64685.898547  93000.0
prediction.head()
id
Prediction
Confidence_Lower
Confidence_upper
price
10
53895.959449
53895.959449
53895.959449
41000.0
12
77403.76960199999
77403.76960199999
77403.76960199999
67000.0
13
45378.98779
45378.98779
45378.98779
49000.0
14
45164.636739999994
45164.636739999994
45164.636739999994
48500.0
16
72975.583423
72975.583423
72975.583423
72000.0
17
39109.471668
39109.471668
39109.471668
27000.0
15
61650.265369
61650.265369
61650.265369
61000.0
11
71834.288947
71834.288947
71834.288947
68000.0
9
55931.963123
55931.963123
55931.963123
55000.0
8
55241.854455
55241.854455
55241.854455
55000.0
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713817224640943"'
Updated dataset after performing customized variable width bin-code
transformation :
bathrms lotsize airco
gashw
garagepl
id
recroom sn
driveway
stories prefarea
fullbase
homestyle
price
bedrooms
1
3520.0
no
no
0
51
no
443
yes
1
yes
no
Eclectic
65000.0 big_house
1
4350.0
no
yes
1
20
no
198
no
2
no
no
Classic 40500.0 big_house
1
3162.0
yes
no
1
49
no
161
yes
2
no
no
Eclectic
63900.0 big_house
```
19: AutoML in teradataml

```python
1
3750.0
no
no
0
43
no
140
yes
2
no
no
Classic 43000.0 big_house
1
5076.0
no
no
0
52
no
111
no
1
no
no
Classic 43000.0 big_house
1
7980.0
no
no
2
69
no
353
yes
1
no
no
Eclectic
78500.0 big_house
1
3760.0
no
yes
2
37
no
117
yes
2
no
no
Eclectic
93000.0 big_house
1
5000.0
no
no
0
67
no
317
yes
4
no
no
Eclectic
80000.0 big_house
1
3000.0
no
no
2
44
no
239
yes
1
no
yes
Classic 26000.0 small_house
1
5400.0
no
no
0
28
no
177
yes
2
no
no
Eclectic
70000.0 big_house
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713815770435510"'
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713815975550667"'
Updated dataset after performing customized categorical encoding :
prefarea
bedrooms
bathrms lotsize gashw
garagepl
id
recroom sn
driveway
stories airco
homestyle
fullbase
price
62906.33597883598
small_house
1
3180.0
no
0
59
no
195
yes
1
no
1
no
33000.0
62906.33597883598
small_house
1
5885.0
no
1
29
no
306
yes
1
yes
2
no
64000.0
62906.33597883598
big_house
1
4360.0
no
0
15
no
255
yes
2
no
2
no
61000.0
62906.33597883598
big_house
1
5170.0
no
0
12
no
38
yes
4
yes
2
no
67000.0
62906.33597883598
small_house
1
3185.0
no
0
23
no
16
yes
1
yes
1
no
37900.0
62906.33597883598
small_house
1
9166.0
no
2
11
no
53
yes
1
yes
2
yes
68000.0
83851.72413793103
big_house
1
2787.0
no
0
27
no
472
yes
1
no
2
yes
60500.0
83851.72413793103
small_house
1
2176.0
no
0
8
yes
469
yes
2
no
2
no
55000.0
83851.72413793103
big_house
1
7410.0
no
2
40
yes
401
yes
1
yes
2
yes
92500.0
83851.72413793103
big_house
1
3520.0
no
2
39
no
441
yes
1
no
2
no
51900.0
result data stored in table
```
19: AutoML in teradataml

```python
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713816198052184"'
Updated dataset after performing categorical encoding :
prefarea
bedrooms_0
bedrooms_1
bedrooms_2
bathrms
lotsize gashw_0 gashw_1 garagepl
id
recroom_0
recroom_1
sn
driveway_0
driveway_1
stories airco_0 airco_1 homestyle
fullbase_0
fullbase_1
price
83851.72413793103
1
0
0
1
9000.0
1
0
1
33
1
0
411
0
1
1
1
0
2
0
1
90000.0
83851.72413793103
1
0
0
1
2398.0
1
0
0
21
1
0
459
0
1
1
1
0
1
1
0
44555.0
83851.72413793103
1
0
0
1
6862.0
1
0
2
19
1
0
440
0
1
2
0
1
2
1
0
69000.0
83851.72413793103
1
0
0
1
3520.0
1
0
0
51
1
0
443
0
1
1
1
0
2
1
0
65000.0
83851.72413793103
1
0
0
1
3520.0
1
0
2
39
1
0
441
0
1
1
1
0
2
1
0
51900.0
83851.72413793103
0
0
1
1
2176.0
1
0
0
8
0
1
469
0
1
2
1
0
2
1
0
55000.0
62906.33597883598
0
0
1
1
3180.0
1
0
0
59
1
0
195
0
1
1
1
0
1
1
0
33000.0
62906.33597883598
0
0
1
1
5885.0
1
0
1
29
1
0
306
0
1
1
0
1
2
1
0
64000.0
62906.33597883598
1
0
0
1
4360.0
1
0
0
15
1
0
255
0
1
2
1
0
2
1
0
61000.0
62906.33597883598
1
0
0
1
5170.0
1
0
0
12
1
0
38
0
1
4
0
1
2
1
0
67000.0
Updated dataset after performing customized anti-selection :
prefarea
bedrooms_0
bedrooms_1
bedrooms_2
bathrms
lotsize gashw_0 gashw_1 garagepl
id
recroom_0
recroom_1
driveway_0
driveway_1
stories airco_0 airco_1 homestyle
fullbase_0
fullbase_1
price
62906.33597883598
1
0
0
1
4360.0
1
0
```
19: AutoML in teradataml

```python
0
15
1
0
0
1
2
1
0
2
1
0
61000.0
62906.33597883598
0
0
1
1
3185.0
1
0
0
23
1
0
0
1
1
0
1
1
1
0
37900.0
62906.33597883598
0
0
1
1
9166.0
1
0
2
11
1
0
0
1
1
0
1
2
0
1
68000.0
62906.33597883598
1
0
0
1
10700.0 1
0
0
16
0
1
0
1
2
1
0
2
0
1
72000.0
62906.33597883598
0
0
1
1
4080.0
1
0
0
9
1
0
0
1
1
1
0
2
1
0
55000.0
62906.33597883598
1
0
0
1
1700.0
1
0
0
17
1
0
0
1
2
1
0
1
1
0
27000.0
83851.72413793103
1
0
0
1
7410.0
1
0
2
40
0
1
0
1
1
0
1
2
0
1
92500.0
83851.72413793103
1
0
0
1
6825.0
1
0
0
32
0
1
0
1
1
0
1
2
0
1
77500.0
83851.72413793103
1
0
0
1
9000.0
1
0
1
33
1
0
0
1
1
1
0
2
0
1
90000.0
83851.72413793103
1
0
0
1
2610.0
1
0
0
13
1
0
0
1
2
1
0
1
0
1
49000.0
Performing transformation carried out in data preparation phase ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713818406018603"'
Updated dataset after performing Lasso feature selection:
id
bathrms fullbase_1
gashw_0 driveway_0
stories airco_1
gashw_1 bedrooms_0
bedrooms_2
driveway_1
garagepl
recroom_0
fullbase_0
homestyle
airco_0 prefarea
lotsize price
31
1
1
1
0
2
1
0
1
0
1
0
1
0
2
0
62906.336
2953.0
60000.0
9
1
0
1
0
1
0
0
0
1
1
0
1
1
2
1
62906.336
4080.0
55000.0
```
19: AutoML in teradataml

```python
17
1
0
1
0
2
0
0
1
0
1
0
1
1
1
1
62906.336
1700.0
27000.0
25
1
0
1
0
1
0
0
0
1
1
0
1
1
1
1
62906.336
3500.0
44500.0
10
1
0
1
0
1
0
0
0
1
1
0
1
1
1
1
62906.336
6000.0
41000.0
36
1
0
1
1
1
0
0
0
1
0
0
1
1
1
1
62906.336
3970.0
32500.0
8
1
0
1
0
2
0
0
0
1
1
0
0
1
2
1
83851.7241
2176.0
55000.0
16
1
1
1
0
2
0
0
1
0
1
0
0
0
2
1
62906.336
10700.0
72000.0
29
1
0
1
0
1
1
0
0
1
1
1
1
1
2
0
62906.336
5885.0
64000.0
15
1
0
1
0
2
0
0
1
0
1
0
1
1
2
1
62906.336
4360.0
61000.0
Updated dataset after performing scaling on Lasso selected features :
driveway_1
fullbase_1
price
bedrooms_0
gashw_0 recroom_0
id
driveway_0
fullbase_0
airco_1 airco_0 gashw_1 bedrooms_2
bathrms stories garagepl
homestyle
prefarea
lotsize
1
1
60000.0 1
1
1
31
0
0
1
0
0
0
-0.569
9326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-1.0349666248397658
1
0
55000.0 0
1
1
9
0
1
0
1
0
1
-0.569
9326198071
-0.8999912756370636
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.503056006140217
1
0
27000.0 1
1
1
17
0
1
0
1
0
0
-0.569
9326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-1.6263455114684566
1
0
44500.0 0
1
1
25
0
1
0
1
0
1
-0.569
9326198071
-0.8999912756370636
-0.7757094582336237
```
19: AutoML in teradataml

```python
-0.7516329215933895
-0.552679423766173
-0.7767988267664266
1
0
41000.0 0
1
1
10
0
1
0
1
0
1
-0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
0.4031271242086151
0
0
32500.0 0
1
1
36
1
1
0
1
0
1
-0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-0.5549727479831188
1
0
55000.0 0
1
0
8
0
1
0
1
0
1
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
1.8093671611394 -1.4016876104028086
1
1
72000.0 1
1
0
16
0
0
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
2.6213879120416936
1
0
64000.0 0
1
1
29
0
1
1
0
0
1
-0.5698449326198071
-0.8999912756370636
0.3943732959927247
0.7474571831400929
-0.552679423766173
0.34885053046376313
1
0
61000.0 1
1
1
15
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.3709042996310123
Updated dataset after performing RFE feature selection:
id
bathrms fullbase_1
gashw_0 driveway_0
stories airco_1
gashw_1 bedrooms_0
bedrooms_2
driveway_1
garagepl
recroom_0
fullbase_0
homestyle
airco_0 recroom_1
prefarea
lotsize price
31
1
1
1
0
2
1
0
1
0
1
0
1
0
2
0
0
62906.336
2953.0
60000.0
9
1
0
1
0
1
0
0
0
1
1
0
1
1
2
1
0
62906.336
4080.0
55000.0
17
1
0
1
0
2
0
0
1
0
1
0
1
1
1
1
0
62906.336
1700.0
27000.0
25
1
0
1
0
1
0
0
0
1
1
0
1
1
1
1
0
62906.336
3500.0
44500.0
10
1
0
1
0
1
0
0
0
1
```
19: AutoML in teradataml

```python
1
0
1
1
1
1
0
62906.336
6000.0
41000.0
36
1
0
1
1
1
0
0
0
1
0
0
1
1
1
1
0
62906.336
3970.0
32500.0
8
1
0
1
0
2
0
0
0
1
1
0
0
1
2
1
1
83851.7241
2176.0
55000.0
16
1
1
1
0
2
0
0
1
0
1
0
0
0
2
1
1
62906.336
10700.0 72000.0
29
1
0
1
0
1
1
0
0
1
1
1
1
1
2
0
0
62906.336
5885.0
64000.0
15
1
0
1
0
2
0
0
1
0
1
0
1
1
2
1
0
62906.336
4360.0
61000.0
Updated dataset after performing scaling on RFE selected features :
r_gashw_0
r_fullbase_1
r_driveway_1
r_recroom_1
r_bedrooms_0
r_bedrooms_2
id
r_recroom_0
r_gashw_1
r_driveway_0
r_airco_0
r_airco_1
r_fullbase_0
price
r_bathrms
r_stories
r_garagepl
r_homestyle
r_prefarea
r_lotsize
1
1
1
0
1
0
31
1
0
0
0
1
0
60000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-1.0349666248397658
1
0
1
0
0
1
9
1
0
0
1
0
1
55000.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.503056006140217
1
0
1
0
1
0
17
1
0
0
1
0
1
27000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-1.6263455114684566
1
0
1
0
0
1
25
1
0
0
1
0
1
44500.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-0.7767988267664266
1
0
1
0
0
1
10
1
0
0
1
0
1
41000.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
0.4031271242086151
1
0
0
0
0
1
36
1
0
1
```
19: AutoML in teradataml

```python
1
0
1
32500.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-0.5549727479831188
1
0
1
1
0
1
8
0
0
0
1
0
1
55000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
1.8093671611394 -1.4016876104028086
1
1
1
1
1
0
16
0
0
0
1
0
0
72000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
2.6213879120416936
1
0
1
0
0
1
29
1
0
0
0
1
1
64000.0 -0.5698449326198071
-0.8999912756370636
0.3943732959927247
0.7474571831400929
-0.552679423766173
0.34885053046376313
1
0
1
0
1
0
15
1
0
0
1
0
1
61000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.3709042996310123
Updated dataset after performing scaling for PCA feature selection :
bedrooms_1
driveway_1
fullbase_1
price
bedrooms_0
gashw_0 recroom_0
id
driveway_0
recroom_1
fullbase_0
airco_1 airco_0 gashw_1 bedrooms_2
prefarea
bathrms lotsize
garagepl
stories homestyle
0
1
1
60000.0 1
1
1
31
0
0
0
1
0
0
0
-0.5526794213794932
-0.5698449326198075
-1.0349666248397658
-0.7757094582336221
0.2539261099118857
0.7474571831400939
0
1
0
55000.0 0
1
1
9
0
0
1
0
1
0
1
-0.5526794213794932
-0.5698449326198075
-0.503056006140217
-0.7757094582336221
-0.8999912756370632
0.7474571831400939
0
1
0
27000.0 1
1
1
17
0
0
1
0
1
0
0
-0.5526794213794932
-0.5698449326198075
-1.6263455114684566
-0.7757094582336221
0.2539261099118857
-0.7516329215933905
0
1
0
44500.0 0
1
1
25
0
0
1
0
1
0
1
-0.5526794213794932
-0.5698449326198075
-0.7767988267664266
-0.7757094582336221
-0.8999912756370632
-0.7516329215933905
0
1
0
41000.0 0
1
1
10
0
0
1
0
1
0
1
-0.5526794213794932
-0.5698449326198075
```
19: AutoML in teradataml

```python
0.4031271242086151
-0.7757094582336221
-0.8999912756370632
-0.75
16329215933905
0
0
0
32500.0 0
1
1
36
1
0
1
0
1
0
1
-0.5526794213794932
-0.5698449326198075
-0.5549727479831188
-0.7757094582336221
-0.8999912756370632
-0.7516329215933905
0
1
0
55000.0 0
1
0
8
0
1
1
0
1
0
1
1.809367156861831
-0.5698449326198075
-1.4016876104028086
-0.77
57094582336221
0.2539261099118857
0.7474571831400939
0
1
1
72000.0 1
1
0
16
0
1
0
0
1
0
0
-0.5526794213794932
-0.5698449326198075
2.6213879120416936
-0.7757094582336221
0.2539261099118857
0.7474571831400939
0
1
0
64000.0 0
1
1
29
0
0
1
1
0
0
1
-0.5526794213794932
-0.5698449326198075
0.34885053046376313
0.39437329599272386
-0.8999912756370632
0.7474571831400939
0
1
0
61000.0 1
1
1
15
0
0
1
0
1
0
0
-0.5526794213794932
-0.5698449326198075
-0.3709042996310123
-0.7757094582336221
0.2539261099118857
0.7474571831400939
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
col_6
col_7
col_8
col_9
price
0
15
-1.003669
0.239619
-0.544437
-0.446579
-0.873745
0.132942
-0.304909
-0.283428
-0.156519
-0.393635
61000.0
1
29
-0.570314
-0.909223
0.891253
-0.739743
-0.298341
0.315286
-0.009998
1.337645
0.357075
0.066034
64000.0
2
31
-0.938230
0.181104
-0.994173
-0.401154
-0.410828
-0.391002
1.254502
0.614167
-0.346502
-0.214179
60000.0
3
16
0.737217
-1.057533
-0.249024
-0.476448
-1.107181
2.240093
1.110594
-1.325931
0.298640
0.170796
72000.0
4
9
-1.786104
-0.439574
0.030216
-0.344894
-0.025365
0.425588
-0.550912
0.337141
0.623026
-0.124383
55000.0
```
19: AutoML in teradataml

```python
5
17
-1.414625
1.385440
-0.124862
0.557650
-0.185774
-0.603600
-0.094402
-0.227540
-0.150107
-0.692651
27000.0
6
25
-1.701373
0.405676
0.680010
0.610730
0.412048
0.370640
-0.388688
0.263257
0.585903
-0.277915
44500.0
7
8
-0.880471
-0.664464
-1.511189
1.245302
-0.187289
-0.943057
-0.515748
0.269059
1.517742
-0.028053
55000.0
8
10
-1.105595
0.044414
0.956715
0.552309
0.110896
1.189863
-0.446720
0.107281
0.533577
-0.103056
41000.0
9
36
-1.772019
0.459011
0.740512
0.646621
0.516132
0.508674
-0.404914
0.170345
0.001238
0.920050
32500.0
Data Transformation completed.
Following model is being used for generating prediction :
Model ID : XGBOOST_2
Feature Selection Method : pca
Prediction :
id    Prediction  Confidence_Lower  Confidence_upper    price
0  31  62660.364670      34660.085646      90660.643694  60000.0
1   9  55986.209180      30785.420530      81186.997830  55000.0
2  17  44426.444076      24229.803083      64623.085068  27000.0
3  25  38398.516388      21188.376911      55608.655865  44500.0
4  10  45683.429188      24617.332667      66749.525709  41000.0
5  36  40066.436509      21095.208542      59037.664475  32500.0
6   8  56863.357233      32185.946248      81540.768217  55000.0
7  16  67979.252919      37332.178823      98626.327014  72000.0
8  29  58572.951390      33177.335198      83968.567583  64000.0
9  15  59827.259623      33990.028359      85664.490888  61000.0
Performance Metrics :
MAE           MSE      MSLE       MAPE    MPE          RMSE
RMSLE           ME        R2        EV          MPD       MGD
0  8002.145716  1.167270e+08  0.038814  15.298458 -5.345  10804.026128
0.197013  34490.55843  0.637479  0.637927  1995.655639  0.036785
prediction.head()
id
Prediction
Confidence_Lower
Confidence_upper
price
10
45683.42918800001
24617.332667
66749.52570900002
```
19: AutoML in teradataml

```python
41000.0
12
78811.21095774998
41047.641704317764
116574.7802111822
67000.0
13
57454.380859375015
30640.814398146318
84267.94732060371
49000.0
14
52876.934924625 28793.137666832026
76960.73218241797
4
0.0
16
67979.2529185
37332.178823254886
98626.32701374512
72000.0
17
44426.44407562501
24229.80308276226
64623.0
68487766
27000.0
15
59827.259623375016
33990.02835867039
85664.49088807964
61000.0
11
71539.44990912499
38962.82825323011
104116.07156501987
68000.0
9
55986.209179749996
30785.420529747764
81186.99782975223
55000.0
8
56863.35723262501
32185.946247916516
81540.7682173335
55000.0
```
11. Generate prediction on test dataset using second best performing model.
```python
prediction = aml.predict(housing_test,2)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713815691874863"'
Updated dataset after performing customized variable width bin-code
transformation :
bathrms lotsize airco
gashw
garagepl
id
recroom sn
driveway
stories prefarea
fullbase
homestyle
price
bedrooms
1
3750.0
no
no
0
43
no
140
yes
2
no
no
Classic 43000.0 big_house
1
9000.0
no
no
1
33
no
411
yes
1
yes
yes
Eclectic
90000.0 big_house
1
10700.0 no
no
0
16
yes
364
yes
2
no
yes
Eclectic
72000.0 big_house
1
4080.0
no
no
0
9
no
301
yes
1
no
no
Eclectic
55000.0 small_house
1
3520.0
no
no
0
51
no
443
yes
1
yes
no
Eclectic
65000.0 big_house
```
19: AutoML in teradataml

```python
1
3180.0
no
no
0
59
no
195
yes
1
no
no
Classic 33000.0 small_house
1
4960.0
no
no
0
48
no
25
yes
1
no
no
Classic 42000.0 small_house
1
3500.0
no
no
0
25
no
249
yes
1
no
no
Classic 44500.0 small_house
1
2650.0
no
no
1
41
no
142
yes
2
no
yes
Classic 40000.0 big_house
1
3162.0
yes
no
1
49
no
161
yes
2
no
no
Eclectic
63900.0 big_house
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713815686869295"'
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713816248740571"'
Updated dataset after performing customized categorical encoding :
prefarea
bedrooms
bathrms lotsize gashw
garagepl
id
recroom sn
driveway
stories airco
homestyle
fullbase
price
62906.33597883598
big_house
1
5400.0
no
0
28
no
177
yes
2
no
2
no
70000.0
62906.33597883598
small_house
1
6000.0
no
0
10
no
260
yes
1
no
1
no
41000.0
62906.33597883598
big_house
1
1700.0
no
0
17
no
13
yes
2
no
1
no
27000.0
62906.33597883598
big_house
1
2650.0
no
1
41
no
142
yes
2
no
1
yes
40000.0
62906.33597883598
big_house
1
5000.0
no
0
67
no
317
yes
4
no
2
no
80000.0
62906.33597883598
small_house
1
3000.0
no
2
44
no
239
yes
1
no
1
yes
26000.0
83
.72413793103
small_house
1
2176.0
no
0
8
yes
469
yes
2
no
2
no
55000.0
83
.72413793103
big_house
1
3520.0
no
2
39
no
441
yes
1
no
2
no
51900.0
83
.72413793103
big_house
1
2610.0
no
0
13
no
463
yes
2
no
1
yes
49000.0
83
.72413793103
big_house
1
6420.0
no
0
22
no
408
yes
3
no
2
yes
87500.0
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713815690312918"'
Updated dataset after performing categorical encoding :
prefarea
bedrooms_0
bedrooms_1
bedrooms_2
bathrms
```
19: AutoML in teradataml

```python
lotsize gashw_0 gashw_1 garagepl
id
recroom_0
recroom_1
sn
driveway_0
driveway_1
stories airco_0 airco_1 homestyle
fullbase_0
fullbase_1
price
62906.33597883598
1
0
0
1
1700.0
1
0
0
17
1
0
13
0
1
2
1
0
1
1
0
27000.0
62906.33597883598
1
0
0
1
5000.0
1
0
0
67
1
0
317
0
1
4
1
0
2
1
0
80000.0
62906.33597883598
0
0
1
1
3000.0
1
0
2
44
1
0
239
0
1
1
1
0
1
0
1
26000.0
62906.33597883598
1
0
0
1
5076.0
1
0
0
52
1
0
111
1
0
1
1
0
1
1
0
43000.0
62906.33597883598
1
0
0
1
3900.0
1
0
0
45
1
0
340
0
1
2
1
0
2
1
0
62500.0
62906.33597883598
1
0
0
1
3630.0
1
0
3
53
1
0
237
0
1
2
1
0
1
1
0
43000.0
83851.72413793103
1
0
0
1
2610.0
1
0
0
13
1
0
463
0
1
2
1
0
1
0
1
49000.0
83851.72413793103
1
0
0
1
2398.0
1
0
0
21
1
0
459
0
1
1
1
0
1
1
0
44555.0
83851.72413793103
1
0
0
1
9000.0
1
0
1
33
1
0
411
0
1
1
1
0
2
0
1
90000.0
83851.72413793103
1
0
0
1
2787.0
1
0
0
27
1
0
472
0
1
1
1
0
2
0
1
60500.0
Updated dataset after performing customized anti-selection :
prefarea
bedrooms_0
bedrooms_1
bedrooms_2
bathrms
lotsize gashw_0 gashw_1 garagepl
id
recroom_0
recroom_1
driveway_0
driveway_1
stories airco_0 airco_1 homestyle
fullbase_0
fullbase_1
price
62906.33597883598
1
0
0
1
1700.0
1
0
0
17
1
0
0
1
2
1
0
1
1
0
27000.0
62906.33597883598
1
0
0
1
5000.0
1
0
0
67
1
0
0
1
4
1
0
2
```
19: AutoML in teradataml

```python
1
0
80000.0
62906.33597883598
0
0
1
1
3000.0
1
0
2
44
1
0
0
1
1
1
0
1
0
1
26000.0
62906.33597883598
1
0
0
1
5076.0
1
0
0
52
1
0
1
0
1
1
0
1
1
0
43000.0
62906.33597883598
1
0
0
1
3900.0
1
0
0
45
1
0
0
1
2
1
0
2
1
0
62500.0
62906.33597883598
1
0
0
1
3630.0
1
0
3
53
1
0
0
1
2
1
0
1
1
0
43000.0
83851.72413793103
1
0
0
1
2610.0
1
0
0
13
1
0
0
1
2
1
0
1
0
1
49000.0
83851.72413793103
1
0
0
1
2398.0
1
0
0
21
1
0
0
1
1
1
0
1
1
0
44555.0
83851.72413793103
1
0
0
1
9000.0
1
0
1
33
1
0
0
1
1
1
0
2
0
1
90000.0
83851.72413793103
1
0
0
1
2787.0
1
0
0
27
1
0
0
1
1
1
0
2
0
1
60500.0
Performing transformation carried out in data preparation phase ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713823026627552"'
Updated dataset after performing Lasso feature selection:
id
bathrms fullbase_1
gashw_0 driveway_0
stories airco_1
gashw_1 bedrooms_0
bedrooms_2
driveway_1
garagepl
recroom_0
fullbase_0
homestyle
airco_0 prefarea
lotsize price
47
1
0
1
0
2
0
0
1
0
1
0
1
1
1
1
62906.336
3850.0
44500.0
45
1
0
1
0
2
0
0
1
0
1
0
1
1
2
1
62906.336
3900.0
62500.0
53
1
0
1
0
2
0
0
1
0
1
3
1
1
1
1
62906.336
3630.0
43000.0
15
1
0
1
0
2
0
0
1
0
```
19: AutoML in teradataml

```python
1
0
1
1
2
1
62906.336
4360.0
61000.0
75
1
0
1
0
1
0
0
0
1
1
0
1
1
1
1
62906.336
4040.0
47000.0
11
1
1
1
0
1
1
0
0
1
1
2
1
0
2
0
62906.336
9166.0
68000.0
32
1
1
1
0
1
1
0
1
0
1
0
0
0
2
0
83851.7241
6825.0
77500.0
52
1
0
1
1
1
0
0
1
0
0
0
1
1
1
1
62906.336
5076.0
43000.0
10
1
0
1
0
1
0
0
0
1
1
0
1
1
1
1
62906.336
6000.0
41000.0
17
1
0
1
0
2
0
0
1
0
1
0
1
1
1
1
62906.336
1700.0
27000.0
Updated dataset after performing scaling on Lasso selected features :
driveway_1
fullbase_1
price
bedrooms_0
gashw_0 recroom_0
id
driveway_0
fullbase_0
airco_1 airco_0 gashw_1 bedrooms_2
bathrms stories garagepl
homestyle
prefarea
lotsize
1
0
44500.0 1
1
1
47
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-0.6116091936299208
1
0
62500.0 1
1
1
45
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.58801067461042
1
0
43000.0 1
1
1
53
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
2.734538804445422
-0.7516329215933895
-0.552679423766173
-0.71
54426773157244
1
0
61000.0 1
1
1
15
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.3709042996310123
1
0
47000.0 0
1
1
75
0
1
0
1
0
1
-0.5698449326198071
-0.8999912756370636
-0.7757094582336237
```
19: AutoML in teradataml

```python
-0.7516329215933895
-0.552679423766173
-0.5219348213558176
1
1
68000.0 0
1
1
11
0
0
1
0
0
1
-0.5698449326198071
-0.8999912756370636
1.5644560502190732
0.7474571831400929
-0.552679423766173
1.8973853485234078
1
1
77500.0 1
1
0
32
0
0
1
0
0
0
-0.5698449326198071
-0.8999912756370636
-0.7757094582336237
0.7474571831400929
1.8093671611394 0.7925026880303788
0
0
43000.0 1
1
1
52
1
1
0
1
0
0
-0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-0.03297350727176035
1
0
41000.0 0
1
1
10
0
1
0
1
0
1
-0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
0.4031271242086151
1
0
27000.0 1
1
1
17
0
1
0
1
0
0
-0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-1.6263455114684566
Updated dataset after performing RFE feature selection:
id
bathrms fullbase_1
gashw_0 driveway_0
stories airco_1
gashw_1 bedrooms_0
bedrooms_2
driveway_1
garagepl
recroom_0
fullbase_0
homestyle
airco_0 recroom_1
prefarea
lotsize price
47
1
0
1
0
2
0
0
1
0
1
0
1
1
1
1
0
62906.336
3850.0
44500.0
45
1
0
1
0
2
0
0
1
0
1
0
1
1
2
1
0
62906.336
3900.0
62500.0
53
1
0
1
0
2
0
0
1
0
1
3
1
1
1
1
0
62906.336
3630.0
43000.0
15
1
0
1
0
2
0
0
1
0
1
0
1
1
2
1
0
62906.336
4360.0
61000.0
75
1
0
1
0
1
0
0
0
1
1
0
1
1
1
1
0
62906.336
4040.0
47000.0
11
1
1
1
0
1
1
0
0
1
1
2
1
0
2
0
0
62906.336
```
19: AutoML in teradataml

```python
9166.0
68000.0
32
1
1
1
0
1
1
0
1
0
1
0
0
0
2
0
1
83851.7241
6825.0
77500.0
52
1
0
1
1
1
0
0
1
0
0
0
1
1
1
1
0
62906.336
5076.0
43000.0
10
1
0
1
0
1
0
0
0
1
1
0
1
1
1
1
0
62906.336
6000.0
41000.0
17
1
0
1
0
2
0
0
1
0
1
0
1
1
1
1
0
62906.336
1700.0
27000.0
Updated dataset after performing scaling on RFE selected features :
r_gashw_0
r_fullbase_1
r_driveway_1
r_recroom_1
r_bedrooms_0
r_bedrooms_2
id
r_recroom_0
r_gashw_1
r_driveway_0
r_airco_0
r_airco_1
r_fullbase_0
price
r_bathrms
r_stories
r_garagepl
r_homestyle
r_prefarea
r_lotsize
1
0
1
0
1
0
47
1
0
0
1
0
1
44500.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-0.6116091936299208
1
0
1
0
1
0
45
1
0
0
1
0
1
62500.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.58801067461042
1
0
1
0
1
0
53
1
0
0
1
0
1
43000.0 -0.5698449326198071
0.2539261099118858
2.734538804445422
-0.7516329215933895
-0.552679423766173
-0.71
54426773157244
1
0
1
0
1
0
15
1
0
0
1
0
1
61000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
0.7474571831400929
-0.552679423766173
-0.3709042996310123
1
0
1
0
0
1
75
1
0
0
1
0
1
47000.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-0.5219348213558176
1
1
1
0
0
1
11
1
0
0
0
1
0
68000.0 -0.5698449326198071
-0.8999912756370636
1.5644560502190732
0.7474571831400929
-0.552679423766173
1.8973853485234078
1
1
1
1
1
0
32
0
0
0
```
19: AutoML in teradataml

```python
0
1
0
77500.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
0.7474571831400929
1.8093671611394 0.7925026880303788
1
0
0
0
1
0
52
1
0
1
1
0
1
43000.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
-0.03297350727176035
1
0
1
0
0
1
10
1
0
0
1
0
1
41000.0 -0.5698449326198071
-0.8999912756370636
-0.7757094582336237
-0.7516329215933895
-0.552679423766173
0.4031271242086151
1
0
1
0
1
0
17
1
0
0
1
0
1
27000.0 -0.5698449326198071
0.2539261099118858
-0.7757094582336237
-0.7516329215933895
-0.55
2679423766173
-1.6263455114684566
Updated dataset after performing scaling for PCA feature selection :
bedrooms_1
driveway_1
fullbase_1
price
bedrooms_0
gashw_0 recroom_0
id
driveway_0
recroom_1
fullbase_0
airco_1 airco_0 gashw_1 bedrooms_2
prefarea
bathrms lotsize
garagepl
stories homestyle
0
1
0
44500.0 1
1
1
47
0
0
1
0
1
0
0
-0.5526794213794932
-0.5698449326198075
-0.6116091936299208
-0.7757094582336221
0.2539261099118
-0.7516329215933905
0
1
0
62500.0 1
1
1
45
0
0
1
0
1
0
0
-0.5526794213794932
-0.5698449326198075
-0.58801067461042
-0.7757094582336221
0.2539261099118
0.7474571831400939
0
1
0
43000.0 1
1
1
53
0
0
1
0
1
0
0
-0.5526794213794932
-0.5698449326198075
-0.7154426773157244
2.734538804445416
0.2539261099118
-0.7516329215933905
0
1
0
61000.0 1
1
1
15
0
0
1
0
1
0
0
-0.5526794213794932
-0.5698449326198075
-0.3709042996310123
-0.7757094582336221
0.2539261099118
0.7474571831400939
0
1
0
47000.0 0
1
1
75
0
0
1
0
1
0
1
-0.5526794213794932
-0.5698449326198075
-0.5219348213558176
-0.7757094582336221
-0.8999912756370632
-0.7516329215933905
0
1
1
68000.0 0
1
1
11
0
0
0
1
0
0
1
-0.5526794213794932
-0.5698449326198075
1.8973853485234078
```
19: AutoML in teradataml

```python
1.5644560502190699
-0.8999912756370632
0.7474571831400939
0
1
1
77500.0 1
1
0
32
0
1
0
1
0
0
0
1.809367156861831
-0.5698449326198075
0.7925026880303788
-0.7757094582336221
-0.8999912756370632
0.7474571831400939
0
0
0
43000.0 1
1
1
52
1
0
1
0
1
0
0
-0.5526794213794932
-0.5698449326198075
-0.03297350727176035
-0.7757094582336221
-0.8999912756370632
-0.7516329215933905
0
1
0
41000.0 0
1
1
10
0
0
1
0
1
0
1
-0.5526794213794932
-0.5698449326198075
0.4031271242086151
-0.7757094582336221
-0.8999912756370632
-0.75
16329215933905
0
1
0
27000.0 1
1
1
17
0
0
1
0
1
0
0
-0.5526794213794932
-0.5698449326198075
-1.6263455114684566
-0.7757094582336221
0.2539261099118857
-0.7516329215933905
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
col_6
col_7
col_8
col_9
price
0
17
-1.414625
1.385440
-0.124862
0.557650
-0.185774
-0.603600
-0.094402
-0.227540
-0.150107
-0.692651
27000.0
1
10
-1.105595
0.044414
0.956715
0.552309
0.110896
1.189863
-0.446720
0.107281
0.533577
-0.103056
41000.0
2
47
-0.902256
1.074754
0.113105
0.507409
-0.444764
0.100932
-0.144309
-0.361679
-0.195108
-0.542272
44500.0
3
52
-1.229283
0.416949
0.607011
0.564425
0.187417
0.726843
-0.220520
-0.356044
-1.097318
0.421967
43000.0
4
45
-1.113292
0.306091
-0.595351
-0.435829
-0.818333
-0.017795
-0.294231
-0.254729
-0.146891
-0.425810
62500.0
5
53
0.449859
0.338874
2.106509
-0.463153
-0.233032
-2.098387
-0.168963
-0.619831
-0.075069
-0.317378
43000.0
6
15
-1.003669
0.239619
-0.544437
-0.446579
-0.873745
```
19: AutoML in teradataml

```python
0.132942
-0.304909
-0.283428
-0.156519
-0.393635
61000.0
7
32
0.470801
-2.082235
-1.196821
1.131651
-0.030202
0.613375
1.070719
0.477148
-0.176126
-0.287062
77500.0
8
75
-1.572685
0.327644
0.739779
0.598111
0.346999
0.547592
-0.401223
0.229566
0.574601
-0.240146
47000.0
9
11
0.763052
-1.948140
1.644308
-1.101667
-0.221112
0.611181
0.837938
0.648384
0.374097
0.669405
68000.0
Data Transformation completed.
Following model is being used for generating prediction :
Model ID : DECISIONFOREST_1
Feature Selection Method : rfe
Prediction :
id    prediction  confidence_lower  confidence_upper    price
0  47  44000.694444      43999.333333      44002.055556  44500.0
1  45  58427.906977      55346.604651      61509.209302  62500.0
2  53  41336.956522      36117.391304      46556.521739  43000.0
3  15  58427.906977      55346.604651      61509.209302  61000.0
4  75  44000.694444      43999.333333      44002.055556  47000.0
5  11  80174.038462      51703.153846     108644.923077  68000.0
6  32  87175.757576      72428.242424     101923.272727  77500.0
7  52  38000.694444      26239.333333      49762.055556  43000.0
8  10  69350.694444      19666.055556     119035.333333  41000.0
9  17  41000.000000      35120.000000      46880.000000  27000.0
Performance Metrics :
MAE           MSE      MSLE       MAPE       MPE          RMSE
RMSLE            ME        R2        EV          MPD       MGD
0  7402.911675  1.081247e+08  0.032827  13.675299 -2.398854  10398.301013
0.181182  31113.636364  0.664195  0.664562  1791.747597  0.032045
prediction.head()
id
prediction
confidence_lower
confidence_upper
price
10
69350.69444444444
19666.055555555547
119035.33333333333
41000.0
12
71718.75
48750.0 94687.5 67000.0
13
41000.0 35120.0 46880.0 49000.0
14
41336.956521739135
36117.39130434795
46556.52173913032
```
19: AutoML in teradataml

```python
48500.0
16
80174.03846153847
51703.1538461539
108644.92307692303
72000.0
17
41000.0 35120.0 46880.0 27000.0
15
58427.90697674418
55346.60465116245
61509.209302325915
61000.0
11
80174.03846153847
51703.1538461539
108644.92307692303
68000.0
9
56427.90697674418
55589.20930232445
57266.604651163914
55000.0
8
56427.90697674418
55589.20930232445
57266.604651163914
55000.0
```
12. Generate evaluation metrics on test dataset using best performing model.
```python
performance_metrics = aml.evaluate(housing_test)
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : XGBOOST_1
Feature Selection Method : rfe
Performance Metrics :
MAE           MSE      MSLE       MAPE       MPE         RMSE
RMSLE            ME        R2        EV          MPD       MGD
0  6612.872179  7.447203e+07  0.027196  12.880711 -4.603534  8629.718038
0.164913  28314.101453  0.768711  0.771862  1331.165781  0.026174
performance_metrics
MAE                MSE                  MSLE
MAPE               MPE                 RMSE
RMSLE               ME                  R2
EV                  MPD                 MGD
6612.872178891304  74472033.41870987    0.02719635873394214
12.88071111950302  -4.603533619606152  8629.718038192781
0.1649131854459859  28314.101452999996  0.768710810641775
0.7718620121604588  1331.1657809502685  0.0261739261346348
```
### Example 3: Run AutoClassifier for Classification Problem using
### Early Stopping Timer
This example predict whether passenger aboard the RMS Titanic survived or not based on different factors.
19: AutoML in teradataml

**Run AutoClassifier to get the best performing model out of available models with following specifications:**
* Use all default models except 'knn' and 'svm'.
* Set early stopping timer to 100300 sec.
* Opt for verbose level 2 to get detailed log.
1.
* Load data and split it to train and test datasets.
* a.
    * Load the example data and create teradataml DataFrame.
```python
load_example_data("teradataml", "titanic")
titanic = DataFrame.from_table("titanic")
```
* b.
    * Perform sampling to get 80% for training and 20% for testing.
```python
titanic_sample = titanic.sample(frac = [0.8, 0.2])
```
* c.
    * Fetch train and test data.
```python
titanic_train= titanic_sample[titanic_sample['sampleid'] ==
1].drop('sampleid', axis=1)
titanic_test = titanic_sample[titanic_sample['sampleid'] ==
2].drop('sampleid', axis=1)
```
2.
* Create an AutoClassifier instance.
```python
aml = AutoClassifier(exclude='knn' 'svm',
verbose=2,
max_runtime_secs=100)
aml = AutoClassifier(exclude='knn',
verbose=2,
max_runtime_secs=300)
```
3.
* Fit the data.
```python
aml.fit(titanic_train, 'survived')
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 713
Total Columns in the data: 12
```
19: AutoML in teradataml

```python
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
age
INTEGER 578
135
None
6
572
0
18.93408134642356
81.06591865357643
fare
FLOAT
713
0
None
11
702
0
0.0
100.0
embarked
VARCHAR(20) CHARACTER SET LATIN 711
2
0
None
None
None
0.2805049088359046
99.71949509116409
name
VARCHAR(1000) CHARACTER SET LATIN
713
0
0
None
None
None
0.0
100.0
parch
INTEGER 713
0
None
545
168
0
0.0
100.0
sibsp
INTEGER 713
0
None
489
224
0
0.0
100.0
passenger
INTEGER 713
0
None
0
713
0
0.0
100.0
cabin
VARCHAR(20) CHARACTER SET LATIN 171
542
0
None
None
None
76.01683029453015
23.983169705469845
sex
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
ticket
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
pclass
INTEGER 713
0
None
0
713
0
0.0
100.0
survived
INTEGER 713
0
None
442
271
0
0.0
100.0
passenger  survived   pclass      age    sibsp    parch     fare
func
min        1.000     0.000    1.000    0.000    0.000    0.000    0.000
std      259.051     0.486    0.841   14.582    1.108    0.751   52.441
25%      232.000     0.000    1.000   21.000    0.000    0.000    7.925
50%      460.000     0.000    3.000   29.000    0.000    0.000   14.454
75%      675.000     1.000    3.000   38.750    1.000    0.000   31.275
max      891.000     1.000    3.000   80.000    8.000    5.000  512.329
mean     453.097     0.380    2.288   30.104    0.527    0.365   33.255
count    713.000   713.000  713.000  578.000  713.000  713.000  713.000
Statistics of Data:
func
passenger
survived
pclass
age
sibsp
parch
fare
50%
460
0
3
29
0
0
14.454
count
713
713
713
578
713
713
713
mean
453.097 0.38
2.288
30.104
0.527
0.365
33.255
min
1
0
1
0
0
0
0
max
891
1
3
80
8
5
512.329
75%
675
1
3
38.75
1
0
31.275
25%
232
0
1
21
0
0
7.925
```
19: AutoML in teradataml

```python
std
259.051 0.486
0.841
14.582
1.108
0.751
52.441
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
name                      713
sex                       2
ticket                    570
cabin                     128
embarked                  3
Futile columns in dataset:
ColumnName
ticket
name
Target Column Distribution:
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0      sibsp           5.750351
1       fare          13.744741
2        age          19.775596
3      parch          23.562412
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.75 sec
Handling less significant features from data ...
Removing Futile columns:
['ticket', 'name']
Sample of Data after removing Futile columns:
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
530
0
2
male
23
2
1
11.5
None
S
```
19: AutoML in teradataml

```python
9
122
0
3
male
None
0
0
8.05
None
S
11
591
0
3
male
35
0
0
7.125
None
S
19
40
1
3
female
14
1
0
11.2417 None
C
10
305
0
3
male
None
0
0
8.05
None
S
13
732
0
3
male
11
0
0
18.7875 None
C
21
734
0
2
male
23
0
0
13.0
None
S
14
61
0
3
male
22
0
0
7.2292
None
C
22
469
0
3
male
None
0
0
7.725
None
Q
8
774
0
3
male
None
0
0
7.225
None
C
16
713 rows X 11 columns
Total time to handle less significant features: 19.91 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
Checking Missing values in dataset ...
Columns with their missing values:
embarked: 2
age: 135
cabin: 542
Deleting rows of these columns for handling missing values:
['embarked']
Sample of dataset after removing 2 rows:
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
40
1
3
female
14
1
0
11.2417 None
C
```
19: AutoML in teradataml

```python
10
530
0
2
male
23
2
1
11.5
None
S
9
101
0
3
female
28
0
0
7.8958
None
S
17
122
0
3
male
None
0
0
8.05
None
S
11
570
1
3
male
32
0
0
7.8542
None
S
15
835
0
3
male
18
0
0
8.3
None
S
23
734
0
2
male
23
0
0
13.0
None
S
14
61
0
3
male
22
0
0
7.2292
None
C
22
80
1
3
female
30
0
0
12.475
None
S
12
345
0
2
male
36
0
0
13.0
None
S
20
711 rows X 11 columns
Dropping these columns for handling missing values:
['cabin']
Sample of dataset after removing 1 columns:
passenger
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
530
0
2
male
23
2
1
11.5
S
9
122
0
3
male
None
0
0
8.05
S
11
591
0
3
male
35
0
0
7.125
S
19
734
0
2
male
23
0
0
13.0
S
14
40
1
3
female
14
1
0
11.2417 C
10
631
1
1
male
80
0
0
30.0
S
18
80
1
3
female
30
0
0
12.475
S
12
345
0
2
male
36
0
0
13.0
S
20
305
0
3
male
None
0
0
8.05
S
13
732
0
3
male
11
0
0
18.7875 C
21
711 rows X 10 columns
Total time to find missing values in data: 14.97 sec
Imputing Missing Values ...
```
19: AutoML in teradataml

```python
Columns with their imputation method:
age: mean
Sample of dataset after Imputation:
passenger
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
305
0
3
male
30
0
0
8.05
S
13
80
1
3
female
30
0
0
12.475
S
12
345
0
2
male
36
0
0
13.0
S
20
734
0
2
male
23
0
0
13.0
S
14
122
0
3
male
30
0
0
8.05
S
11
591
0
3
male
35
0
0
7.125
S
19
570
1
3
male
32
0
0
7.8542
S
15
835
0
3
male
18
0
0
8.3
S
23
469
0
3
male
30
0
0
7.725
Q
8
774
0
3
male
30
0
0
7.225
C
16
711 rows X 10 columns
Time taken to perform imputation: 15.47 sec
Performing encoding for categorical columns ...
ONE HOT Encoding these Columns:
['sex', 'embarked']
Sample of dataset after performing one hot encoding:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
242
1
3
1
0
30
1
0
15.5
0
1
0
28
507
1
2
1
0
33
0
2
26.0
0
0
1
44
772
0
3
0
1
48
0
0
7.8542
0
0
1
52
587
0
2
0
1
47
0
0
15.0
0
0
1
60
240
0
2
0
1
33
0
0
12.275
0
0
1
76
505
1
1
1
0
16
0
0
86.5
0
0
1
84
852
0
3
0
1
74
0
0
7.775
0
0
1
68
```
19: AutoML in teradataml

```python
38
0
3
0
1
21
0
0
8.05
0
0
1
36
345
0
2
0
1
36
0
0
13.0
0
0
1
20
80
1
3
1
0
30
0
0
12.475
0
0
1
12
711 rows X 13 columns
Time taken to encode the columns: 17.96 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
Outlier preprocessing ...
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0      sibsp           5.766526
1        age           7.313643
2      parch          23.628692
3       fare          13.502110
Deleting rows of these columns:
['sibsp', 'age']
Sample of dataset after removing outlier rows:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
856
1
3
1
0
18
0
1
9.35
0
0
1
35
713
1
1
0
1
48
1
0
52.0
0
0
1
51
19
0
3
1
0
31
1
0
18.0
0
0
1
59
263
0
1
0
1
52
1
1
79.65
0
0
1
67
324
1
2
1
0
22
1
1
29.0
0
0
1
83
854
1
1
1
0
16
0
1
39.4
0
0
1
91
59
1
2
1
0
5
1
2
27.75
0
```
19: AutoML in teradataml

```python
0
1
75
448
1
1
0
1
34
0
0
26.55
0
0
1
43
591
0
3
0
1
35
0
0
7.125
0
0
1
19
122
0
3
0
1
30
0
0
8.05
0
0
1
11
623 rows X 13 columns
median inplace of outliers:
['fare', 'parch']
Sample of dataset after performing MEDIAN inplace:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
856
1
3
1
0
18
0
0
9.35
0
0
1
35
713
1
1
0
1
48
1
0
52.0
0
0
1
51
19
0
3
1
0
31
1
0
18.0
0
0
1
59
263
0
1
0
1
52
1
0
13.0
0
0
1
67
324
1
2
1
0
22
1
0
29.0
0
0
1
83
854
1
1
1
0
16
0
0
39.4
0
0
1
91
59
1
2
1
0
5
1
0
27.75
0
0
1
75
448
1
1
0
1
34
0
0
26.55
0
0
1
43
591
0
3
0
1
35
0
0
7.125
0
0
1
19
122
0
3
0
1
30
0
0
8.05
0
0
1
11
623 rows X 13 columns
Time Taken by Outlier processing: 46.84 sec
Checking imbalance data ...
Imbalance Not Found.
```
19: AutoML in teradataml

```python
Feature selection using lasso ...
feature selected by lasso:
['sex_0', 'embarked_1', 'sex_1', 'embarked_0', 'age', 'embarked_2',
'passenger', 'sibsp', 'fare', 'pclass']
Total time taken by feature selection: 3.89 sec
scaling Features of lasso data ...
columns that will be scaled:
['age', 'passenger', 'sibsp', 'fare', 'pclass']
Dataset sample after scaling:
sex_0
embarked_1
sex_1
embarked_0
embarked_2
survived
id
age
passenger
sibsp
fare
pclass
1
0
0
1
0
1
10
0.21568627450980393
0.043820224719101124
0.5
0.19722280701754386
1.0
1
0
0
0
1
1
12
0.5294117647058824
0.08876404494382023
0.0
0.218859649122807
1.0
0
0
1
0
1
0
13
0.5294117647058824
0.3415730337078652
0.0
0.14122807017543862
1.0
0
0
1
0
1
0
14
0.39215686274509803
0.8235955056179776
0.0
0.22807017543859648
0.5
0
0
1
1
0
0
16
0.5294117647058824
0.8685393258426967
0.0
0.1267543859649123
1.0
1
0
0
0
1
0
17
0.49019607843137253
0.11235955056179775
0.0
0.13852280701754385
1.0
0
0
1
0
1
1
15
0.5686274509803921
0.6393258426966292
0.0
0.13779298245614036
1.0
0
0
1
0
1
0
11
0.5294117647058824
0.13595505617977527
0.0
0.14122807017543862
1.0
0
0
1
0
1
0
9
0.39215686274509803
0.5943820224719101
1.0
0.20175438596491227
0.5
0
1
1
0
0
0
8
0.5294117647058824
0.5258426966292135
0.0
0.1355263157894737
1.0
623 rows X 12 columns
Total time taken by feature scaling: 33.36 sec
Feature selection using rfe ...
```
19: AutoML in teradataml

```python
feature selected by RFE:
['sex_0', 'embarked_1', 'sex_1', 'age', 'embarked_0', 'embarked_2',
'passenger', 'sibsp', 'parch', 'pclass', 'fare']
Total time taken by feature selection: 20.40 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_age', 'r_passenger', 'r_sibsp', 'r_pclass', 'r_fare']
Dataset sample after scaling:
id
r_sex_0 r_sex_1 r_embarked_0
r_parch r_embarked_1
survived
r_embarked_2
r_age
r_passenger
r_sibsp r_pclass
r_fare
10
1
0
1
0
0
1
0
0.21568627450980393
0.043820224719101124
0.5
1.0
0.19722280701754386
12
1
0
0
0
0
1
1
0.5294117647058824
0.08876404494382023
0.0
1.0
0.218859649122807
13
0
1
0
0
0
0
1
0.5294117647058824
0.3415730337078652
0.0
1.0
0.14122807017543862
14
0
1
0
0
0
0
1
0.39215686274509803
0.8235955056179776
0.0
0.5
0.22807017543859648
16
0
1
1
0
0
0
0
0.5294117647058824
0.8685393258426967
0.0
1.0
0.1267543859649123
17
1
0
0
0
0
0
1
0.49019607843137253
0.11235955056179775
0.0
1.0
0.13852280701754385
15
0
1
0
0
0
1
1
0.5686274509803921
0.6393258426966292
0.0
1.0
0.13779298245614036
11
0
1
0
0
0
0
1
0.5294117647058824
0.13595505617977527
0.0
1.0
0.14122807017543862
9
0
1
0
0
0
0
1
0.39215686274509803
0.5943820224719101
1.0
0.5
0.20175438596491227
8
0
1
0
0
1
0
0
0.5294117647058824
0.5258426966292135
0.0
1.0
0.1355263157894737
```
19: AutoML in teradataml

```python
623 rows X 13 columns
Total time taken by feature scaling: 34.77 sec
scaling Features of pca data ...
columns that will be scaled:
['passenger', 'pclass', 'age', 'sibsp', 'fare']
Dataset sample after scaling:
sex_0
embarked_1
sex_1
embarked_0
embarked_2
parch
survived
id
passenger
pclass
age
sibsp
fare
0
1
1
0
0
0
0
8
0.5258426966292135
1.0
0.5294117647058824
0.0
0.1355263157894737
0
0
1
0
1
0
0
13
0.3415730337078652
1.0
0.5294117647058824
0.0
0.14122807017543862
0
0
1
1
0
0
0
21
0.8213483146067416
1.0
0.1568627450980392
0.0
0.32960526315789473
0
0
1
0
1
0
0
14
0.8235955056179776
0.5
0.39215686274509803
0.0
0.22807017543859648
0
0
1
0
1
0
0
9
0.5943820224719101
0.5
0.39215686274509803
1.0
0.20175438596491227
1
0
0
0
1
0
0
17
0.11235955056179775
1.0
0.49019607843137253
0.0
0.13852280701754385
0
0
1
0
1
0
1
15
0.6393258426966292
1.0
0.5686274509803921
0.0
0.13779298245614036
0
0
1
0
1
0
0
23
0.9370786516853933
1.0
0.29411764705882354
0.0
0.1456140350877193
1
0
0
0
1
0
1
12
0.08876404494382023
1.0
0.5294117647058824
0.0
0.218859649122807
0
0
1
0
1
0
0
20
0.3865168539325843
0.5
0.6470588235294118
0.0
0.22807017543859648
```
19: AutoML in teradataml

```python
623 rows X 13 columns
Total time taken by feature scaling: 30.18 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5']
Total time taken by PCA: 6.86 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Hyperparameters used for model training:
response_column : survived
name : glm
family : BINOMIAL
lambda1 : (0.001, 0.02, 0.1)
alpha : (0.15, 0.85)
learning_rate : OPTIMAL
initial_eta : (0.05, 0.1)
momentum : (0.65, 0.8, 0.95)
iter_num_no_change : (5, 10, 50)
iter_max : (300, 200, 400)
batch_size : (10, 50, 60, 80)
Total number of models for glm : 1296
response_column : survived
name : xgboost
model_type : Classification
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1, 0.2)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.1, 0.3)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
iter_num : (10, 20, 30)
```
19: AutoML in teradataml

```python
seed : 42
Total number of models for xgboost : 2592
response_column : survived
name : decision_forest
tree_type : Classification
min_impurity : (0.0, 0.1, 0.2)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
num_trees : (-1, 20, 30)
seed : 42
Total number of models for decision_forest : 108
Performing hyperparameter tuning ...
glm
xgboost
decision_forest
Leaderboard
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
DECISIONFOREST_3
lasso
0.816
0.816
0.816
0.816
0.806990
0.816058
0.810118
0.821300
0.816
0.817337
```
19: AutoML in teradataml

```python
1
2
DECISIONFOREST_0
lasso
0.808
0.808
0.808
0.808
0.803885
0.787728
0.793616
0.806734
0.808
0.805385
2
3
DECISIONFOREST_1
rfe
0.808
0.808
0.808
0.808
0.803885
0.787728
0.793616
0.806734
0.808
0.805385
3
4
XGBOOST_2
pca
0.792
0.792
0.792
0.792
0.781821
0.781821
0.781821
0.792000
0.792
0.792000
4
5
XGBOOST_0
lasso
0.784
0.784
0.784
0.784
0.775751
0.786117
0.778325
0.792884
0.784
0.785986
5
6
XGBOOST_3
lasso
0.784
0.784
0.784
0.784
0.775751
0.786117
0.778325
0.792884
0.784
0.785986
6
7
XGBOOST_1
rfe
0.784
0.784
0.784
0.784
0.775751
0.786117
0.778325
0.792884
0.784
0.785986
7
8
DECISIONFOREST_2
pca
0.728
0.728
0.728
0.728
0.714403
0.711063
0.712527
0.726245
0.728
0.726933
8
9
GLM_2
pca
0.712
0.712
0.712
0.712
0.711222
0.665279
0.669118
0.711555
0.712
0.694847
9
10
GLM_0
lasso
0.704
0.704
0.704
0.704
0.702632
0.655075
0.657636
0.703200
0.704
0.684850
10
11
GLM_1
rfe
0.704
0.704
0.704
0.704
0.702632
0.655075
0.657636
0.703200
0.704
0.684850
11
12
GLM_3
lasso
0.632
0.632
0.632
0.632
0.610228
0.559613
0.535391
0.616985
0.632
0.581153
12 rows X 13 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 16/16
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 713
```
19: AutoML in teradataml

```python
Total Columns in the data: 12
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
survived
INTEGER 713
0
None
444
269
0
0.0
100.0
passenger
INTEGER 713
0
None
0
713
0
0.0
100.0
embarked
VARCHAR(20) CHARACTER SET LATIN 712
1
0
None
None
None
0.1402524544179523
99.85974754558205
fare
FLOAT
713
0
None
13
700
0
0.0
100.0
sibsp
INTEGER 713
0
None
481
232
0
0.0
100.0
name
VARCHAR(1000) CHARACTER SET LATIN
713
0
0
None
None
None
0.0
100.0
parch
INTEGER 713
0
None
535
178
0
0.0
100.0
age
INTEGER 564
149
None
7
557
0
20.897615708274895
79.1023842917251
sex
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
pclass
INTEGER 713
0
None
0
713
0
0.0
100.0
cabin
VARCHAR(20) CHARACTER SET LATIN 159
554
0
None
None
None
77.69985974754559
22.30014025245442
ticket
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
Statistics of Data:
func
passenger
survived
pclass
age
sibsp
parch
fare
min
1
0
1
0
0
0
0
std
256.46
0.485
0.825
14.656
1.119
0.811
51.196
25%
226
0
2
20
0
0
7.896
50%
451
0
3
28
0
0
14.454
75%
667
1
3
38
1
0
30.5
max
891
1
3
80
8
6
512.329
mean
446.952 0.377
2.325
29.246
0.54
0.393
31.973
count
713
713
713
564
713
713
713
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
name                      713
sex                       2
ticket                    563
cabin                     124
```
19: AutoML in teradataml

```python
embarked                  3
Futile columns in dataset:
ColumnName
name
ticket
Target Column Distribution:
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0       fare          12.342216
1      parch          24.964937
2      sibsp           5.189341
3        age          21.739130
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.61 sec
Handling less significant features from data ...
Removing Futile columns:
['ticket', 'name']
Sample of Data after removing Futile columns:
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
61
0
3
male
22
0
0
7.2292
None
C
14
469
0
3
male
None
0
0
7.725
None
Q
8
183
0
3
male
9
4
2
31.3875 None
S
16
80
1
3
female
30
0
0
12.475
None
S
```
19: AutoML in teradataml

```python
12
591
0
3
male
35
0
0
7.125
None
S
11
387
0
3
male
1
5
2
46.9
None
S
19
570
1
3
male
32
0
0
7.8542
None
S
15
162
1
2
female
40
0
0
15.75
None
S
23
40
1
3
female
14
1
0
11.2417 None
C
10
631
1
1
male
80
0
0
30.0
A23
S
18
713 rows X 11 columns
Total time to handle less significant features: 21.47 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.02 sec
Checking Missing values in dataset ...
Columns with their missing values:
age: 149
cabin: 554
embarked: 1
Deleting rows of these columns for handling missing values:
['embarked']
Sample of dataset after removing 1 rows:
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
40
1
3
female
14
1
0
11.2417 None
C
10
591
0
3
male
35
0
0
7.125
None
S
11
387
0
3
male
1
5
2
46.9
None
S
19
570
1
3
male
32
0
0
7.8542
None
S
```
19: AutoML in teradataml

```python
15
61
0
3
male
22
0
0
7.2292
None
C
14
652
1
2
female
18
0
1
23.0
None
S
22
469
0
3
male
None
0
0
7.725
None
Q
8
183
0
3
male
9
4
2
31.3875 None
S
16
80
1
3
female
30
0
0
12.475
None
S
12
345
0
2
male
36
0
0
13.0
None
S
20
712 rows X 11 columns
Dropping these columns for handling missing values:
['cabin']
Sample of dataset after removing 1 columns:
passenger
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
469
0
3
male
None
0
0
7.725
Q
8
80
1
3
female
30
0
0
12.475
S
12
345
0
2
male
36
0
0
13.0
S
20
61
0
3
male
22
0
0
7.2292
C
14
305
0
3
male
None
0
0
8.05
S
13
446
1
1
male
4
0
2
81.8583 S
21
570
1
3
male
32
0
0
7.8542
S
15
162
1
2
female
40
0
0
15.75
S
23
591
0
3
male
35
0
0
7.125
S
11
387
0
3
male
1
5
2
46.9
S
19
712 rows X 10 columns
Total time to find missing values in data: 17.32 sec
Imputing Missing Values ...
Columns with their imputation method:
age: mean
Sample of dataset after Imputation:
passenger
survived
pclass
sex
age
sibsp
parch
fare
```
19: AutoML in teradataml

```python
embarked
id
711
1
1
female
24
0
0
49.5042 C
29
709
1
1
female
22
0
0
151.55
S
45
484
1
3
female
63
0
0
9.5875
S
53
545
0
1
male
50
1
0
106.425 C
61
667
0
2
male
25
0
0
13.0
S
77
463
0
1
male
47
0
0
38.5
S
85
402
0
3
male
26
0
0
8.05
S
69
444
1
2
female
28
0
0
13.0
S
37
446
1
1
male
4
0
2
81.8583 S
21
305
0
3
male
29
0
0
8.05
S
13
712 rows X 10 columns
Time taken to perform imputation: 16.40 sec
Performing encoding for categorical columns ...
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713847448878735"'18
ONE HOT Encoding these Columns:
['sex', 'embarked']
Sample of dataset after performing one hot encoding:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
774
0
3
0
1
29
0
0
7.225
1
0
0
24
814
0
3
1
0
6
4
2
31.275
0
0
1
40
364
0
3
0
1
35
0
0
7.05
0
0
1
48
221
1
3
0
1
16
0
0
8.05
0
0
1
56
812
0
3
0
1
39
0
0
24.15
0
0
1
72
669
0
3
0
1
43
0
0
8.05
0
0
1
80
547
1
2
1
0
19
1
0
26.0
0
0
1
64
366
0
3
0
1
30
0
0
7.25
0
0
1
32
183
0
3
0
1
9
4
2
31.3875 0
0
1
16
```
19: AutoML in teradataml

```python
469
0
3
0
1
29
0
0
7.725
0
1
0
8
712 rows X 13 columns
Time taken to encode the columns: 14.11 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
Spliting of dataset into training and testing ...
Training size :
0.8
Testing size  :
0.2
Training data sample
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
40
1
3
1
0
14
1
0
11.2417 1
0
0
10
591
0
3
0
1
35
0
0
7.125
0
0
1
11
387
0
3
0
1
1
5
2
46.9
0
0
1
19
80
1
3
1
0
30
0
0
12.475
0
0
1
12
530
0
2
0
1
23
2
1
11.5
0
0
1
9
101
0
3
1
0
28
0
0
7.8958
0
0
1
17
305
0
3
0
1
29
0
0
8.05
0
0
1
13
446
1
1
0
1
4
0
2
81.8583 0
0
1
21
570
1
3
0
1
32
0
0
7.8542
0
0
1
15
162
1
2
1
0
40
0
0
15.75
0
0
1
23
569 rows X 13 columns
```
19: AutoML in teradataml

```python
Testing data sample
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
774
0
3
0
1
29
0
0
7.225
1
0
0
24
38
0
3
0
1
21
0
0
8.05
0
0
1
28
339
1
3
0
1
45
0
0
8.05
0
0
1
124
244
0
3
0
1
22
0
0
7.125
0
0
1
30
711
1
1
1
0
24
0
0
49.5042 1
0
0
29
194
1
2
0
1
3
1
1
26.0
0
0
1
125
427
1
2
1
0
28
1
0
26.0
0
0
1
31
97
0
1
0
1
71
0
0
34.6542 1
0
0
127
448
1
1
0
1
34
0
0
26.55
0
0
1
27
137
1
1
1
0
19
0
2
26.2833 0
0
1
123
143 rows X 13 columns
Time taken for spliting of data: 11.05 sec
Outlier preprocessing ...
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0       fare          12.219101
1        age           7.162921
2      sibsp           5.196629
3      parch          25.000000
Deleting rows of these columns:
['sibsp', 'age']
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713849531417344"'18
Sample of training dataset after removing outlier rows:
```
19: AutoML in teradataml

```python
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
141
0
3
1
0
29
0
2
15.2458 1
0
0
46
406
0
2
0
1
34
1
0
21.0
0
0
1
62
875
1
2
1
0
28
1
0
24.0
1
0
0
70
467
0
2
0
1
29
0
0
0.0
0
0
1
78
343
0
2
0
1
28
0
0
13.0
0
0
1
110
36
0
1
0
1
42
1
0
52.0
0
0
1
118
629
0
3
0
1
26
0
0
7.8958
0
0
1
102
610
1
1
1
0
40
0
0
153.4625
0
0
1
54
652
1
2
1
0
18
0
1
23.0
0
0
1
22
61
0
3
0
1
22
0
0
7.2292
1
0
0
14
500 rows X 13 columns
median inplace of outliers:
['fare', 'parch']
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713843547408813"'18
Sample of training dataset after performing MEDIAN inplace:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
141
0
3
1
0
29
0
0
15.2458 1
0
0
46
406
0
2
0
1
34
1
0
21.0
0
0
1
62
875
1
2
1
0
28
1
0
24.0
1
0
0
70
467
0
2
0
1
29
0
0
0.0
0
0
1
78
343
0
2
0
1
28
0
0
13.0
0
0
1
110
36
0
1
0
1
42
1
0
52.0
0
```
19: AutoML in teradataml

```python
0
1
118
629
0
3
0
1
26
0
0
7.8958
0
0
1
102
610
1
1
1
0
40
0
0
13.0
0
0
1
54
652
1
2
1
0
18
0
0
23.0
0
0
1
22
61
0
3
0
1
22
0
0
7.2292
1
0
0
14
500 rows X 13 columns
Time Taken by Outlier processing: 55.03 sec
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713843671280482"'18
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713843382441478"'
Checking imbalance data ...
Imbalance Not Found.
Feature selection using lasso ...
feature selected by lasso:
['embarked_2', 'sex_0', 'sibsp', 'embarked_0', 'age', 'sex_1', 'pclass',
'embarked_1', 'passenger', 'fare']
Total time taken by feature selection: 2.92 sec
scaling Features of lasso data ...
columns that will be scaled:
['sibsp', 'age', 'pclass', 'passenger', 'fare']
Training dataset sample after scaling:
embarked_2
id
sex_0
embarked_0
survived
sex_1
embarked_1
sibsp
age
pclass
passenger
fare
1
59
1
0
1
0
0
0.5
0.37254901960784315
0.5
0.36292134831460676
0.5087719298245614
1
67
1
0
1
0
0
0.0
0.2549019607843137
0.0
0.9584269662921349
0.6912280701754385
0
218
0
1
0
1
0
0.0
0.5098039215686274
0.0
0.6258426966292134
0.22807017543859648
```
19: AutoML in teradataml

```python
1
75
0
0
0
1
0
0.0
0.3137254901960784
1.0
0.3393258426966292
0.0
1
91
1
0
1
0
0
0.0
0.23529411764705882
0.0
0.7741573033707865
0.22807017543859648
0
338
0
1
0
1
0
0.0
0.5294117647058824
1.0
0.27415730337078653
0.1267543859649123
0
274
0
0
0
1
1
0.0
0.5098039215686274
1.0
0.14157303370786517
0.13596491228070176
0
138
0
1
0
1
0
0.0
0.6274509803921569
1.0
0.9516853932584269
0.13852280701754385
0
66
1
1
1
0
0
0.5
0.23529411764705882
1.0
0.9325842696629213
0.25358245614035085
1
43
0
0
0
1
0
0.5
0.9607843137254902
0.0
0.2943820224719101
0.22807017543859648
500 rows X 12 columns
Testing dataset sample after scaling:
embarked_2
id
sex_0
embarked_0
survived
sex_1
embarked_1
sibsp
age
pclass
passenger
fare
1
369
0
0
0
1
0
0.0
0.29411764705882354
0.5
0.43258426966292135
1.2894736842105263
1
449
0
0
0
1
0
0.5
0.29411764705882354
1.0
0.19662921348314608
0.13779298245614036
0
373
0
1
1
1
0
0.5
0.43137254901960786
0.0
0.5438202247191011
1.597880701754386
1
537
1
0
1
0
0
0.5
0.6470588235294118
0.5
0.5820224719101124
0.45614035087719296
1
553
1
0
0
0
0
0.0
0.7450980392156863
1.0
0.7168539325842697
0.6962719298245614
0
24
0
1
0
1
0
0.0
0.5098039215686274
1.0
0.8685393258426967
0.1267543859649123
0
541
0
1
0
1
0
0.5
0.23529411764705882
1.0
0.3955056179775281
0.1268280701754386
0
29
1
1
1
0
0
0.0
0.4117647058823529
0.0
0.797752808988764
0.8684947368421052
0
481
0
1
0
1
0
0.0
0.5098039215686274
0.0
0.8606741573033708
0.6947368421052632
1
185
0
0
0
1
0
0.0
0.5098039215686274
1.0
0.6752808988764045
0.13852280701754385
143 rows X 12 columns
Total time taken by feature scaling: 52.41 sec
```
19: AutoML in teradataml

```python
Feature selection using rfe ...
feature selected by RFE:
['sex_0', 'age', 'sex_1', 'pclass', 'passenger', 'fare']
Total time taken by feature selection: 32.31 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_age', 'r_pclass', 'r_passenger', 'r_fare']
Training dataset sample after scaling:
r_sex_0 r_sex_1 survived
id
r_age
r_pclass
r_passenger
r_fare
1
0
1
59
0.37254901960784315
0.5
0.36292134831460676
0.5087719298245614
1
0
1
67
0.2549019607843137
0.0
0.9584269662921349
0.6912280701754385
0
1
0
218
0.5098039215686274
0.0
0.6258426966292134
0.22807017543859648
0
1
0
75
0.3137254901960784
1.0
0.3393258426966292
0.0
1
0
1
91
0.23529411764705882
0.0
0.7741573033707865
0.22807017543859648
0
1
0
338
0.5294117647058824
1.0
0.27415730337078653
0.1267543859649123
0
1
0
274
0.5098039215686274
1.0
0.14157303370786517
0.13596491228070176
0
1
0
138
0.6274509803921569
1.0
0.9516853932584269
0.13852280701754385
1
0
1
66
0.23529411764705882
1.0
0.9325842696629213
0.25358245614035085
0
1
0
43
0.9607843137254902
0.0
0.2943820224719101
0.22807017543859648
500 rows X 8 columns
Testing dataset sample after scaling:
r_sex_0 r_sex_1 survived
id
r_age
r_pclass
r_passenger
r_fare
0
1
0
369
0.29411764705882354
0.5
0.43258426966292135
1.2894736842105263
```
19: AutoML in teradataml

```python
0
1
0
449
0.29411764705882354
1.0
0.19662921348314608
0.13779298245614036
0
1
1
373
0.43137254901960786
0.0
0.5438202247191011
1.597880701754386
1
0
1
537
0.6470588235294118
0.5
0.5820224719101124
0.45614035087719296
1
0
0
553
0.7450980392156863
1.0
0.7168539325842697
0.6962719298245614
0
1
0
24
0.5098039215686274
1.0
0.8685393258426967
0.1267543859649123
0
1
0
541
0.23529411764705882
1.0
0.3955056179775281
0.1268280701754386
1
0
1
29
0.4117647058823529
0.0
0.797752808988764
0.8684947368421052
0
1
0
481
0.5098039215686274
0.0
0.8606741573033708
0.6947368421052632
0
1
0
185
0.5098039215686274
1.0
0.6752808988764045
0.13852280701754385
143 rows X 8 columns
Total time taken by feature scaling: 45.27 sec
scaling Features of pca data ...
columns that will be scaled:
['passenger', 'pclass', 'age', 'sibsp', 'fare']
Training dataset sample after scaling:
embarked_2
id
sex_0
embarked_0
survived
sex_1
parch
embarked_1
passenger
pclass
age
sibsp
fare
0
8
0
0
0
1
0
1
0.5258426966292135
1.0
0.5098039215686274
0.0
0.1355263157894737
1
9
0
0
0
1
0
0
0.5943820224719101
0.5
0.39215686274509803
1.0
0.20175438596491227
1
17
1
0
0
0
0
0
0.11235955056179775
1.0
0.49019607843137253
0.0
0.13852280701754385
0
14
0
1
0
1
0
0
0.06741573033707865
1.0
0.37254901960784315
0.0
0.1268280701754386
1
15
0
0
1
1
0
0
```
19: AutoML in teradataml

```python
0.6393258426966292
1.0
0.5686274509803921
0.0
0.13779298245614036
1
23
1
0
1
0
0
0
0.18089
640449437
0.5
0.7254901960784313
0.0
0.27631578947368424
1
13
0
0
0
1
0
0
0.3415730337078652
1.0
0.5098039215686274
0.0
0.14122807017543862
1
21
0
0
1
1
0
0
0.5
0.0
0.0196078431372549
0.0
0.22807017543859648
1
12
1
0
1
0
0
0
0.0
6404494382023
1.0
0.5294117647058824
0.0
0.218859649122807
1
20
0
0
0
1
0
0
0.3865168539325843
0.5
0.6470588235294118
0.0
0.22807017543859648
500 rows X 13 columns
Testing dataset sample after scaling:
embarked_2
id
sex_0
embarked_0
survived
sex_1
parch
embarked_1
passenger
pclass
age
sibsp
fare
1
26
1
0
0
0
2
0
0.13370786516853933
1.0
-0.0196078431372549
2.0
0.5486842105263158
1
28
0
0
0
1
0
0
0.04157303370786517
1.0
0.35294117647058826
0.0
0.14122807017543862
1
124
0
0
1
1
0
0
0.37977528089
64
1.0
0.8235294117647058
0.0
0.14122807017543862
1
25
0
0
1
1
0
0
0.317977528089
66
1.0
0.3137254901960784
0.0
0.14122807017543862
1
31
1
0
1
0
0
0
0.4786516853932584
0.5
0.49019607843137253
0.5
0.45614035087719296
0
127
0
1
0
1
0
0
0.10786516853932585
0.0
1.3333333333333333
0.0
0.6079684210526316
1
30
0
0
0
1
0
0
0.27303370786516856
1.0
0.37254901960784315
0.0
0.125
1
126
0
0
0
1
0
0
0.12921348314606743
1.0
0.35294117647058826
0.0
```
19: AutoML in teradataml

```python
0.13903508771929823
1
27
0
0
1
1
0
0
0.5022471910112359
0.0
0.6078431372549019
0.0
0.46578947368421053
1
123
1
0
1
0
2
0
0.15280898876404495
0.0
0.3137254901960784
0.0
0.46111052631578947
143 rows X 13 columns
Total time taken by feature scaling: 46.21 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5']
Total time taken by PCA: 12.01 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Hyperparameters used for model training:
response_column :
survived
name : svm
model_type : Classification
lambda1 : (0.001, 0.02, 0.1)
alpha : (0.15, 0.85)
tolerance : (0.001, 0.01)
learning_rate : OPTIMAL
initial_eta : (0.05, 0.1)
momentum : (0.65, 0.8, 0.95)
nesterov : True
intercept : True
iter_num_no_change : (5, 10, 50)
local_sgd_iterations  : (10, 20)
iter_max : (300, 200, 400)
batch_size : (10, 50, 60, 80)
Total number of models for svm : 5184
```
19: AutoML in teradataml

```python
response_column : survived
name : decision_forest
tree_type : Classification
min_impurity : (0.0, 0.1, 0.2)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
num_trees : (-1, 20, 30)
Total number of models for decision_forest : 108
response_column : survived
name : glm
family : BINOMIAL
lambda1 : (0.001, 0.02, 0.1)
alpha : (0.15, 0.85)
learning_rate : OPTIMAL
initial_eta : (0.05, 0.1)
momentum : (0.65, 0.8, 0.95)
iter_num_no_change : (5, 10, 50)
iter_max : (300, 200, 400)
batch_size : (10, 50, 60, 80)
Total number of models for glm : 1296
response_column : survived
name : xgboost
model_type : Classification
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1, 0.2)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.1, 0.3)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
iter_num : (10, 20, 30)
Total number of models for xgboost : 2592
```
19: AutoML in teradataml

```python
Performing hyperParameter tuning ...
svm
decision_forest
glm
xgboost
Evaluating models performance ...
Evaluation completed.
Leaderboard
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
DECISIONFOREST_3
lasso
0.832168
0.832168
0.832168
0.832168
0.820710
0.825114
0.822727
0.833646
0.832168
0.832740
1
2
XGBOOST_3
lasso
0.825175
0.825175
0.825175
0.825175
0.816070
0.808573
0.811892
0.823842
0.825175
0.824126
2
3
XGBOOST_0
lasso
0.825175
0.825175
0.825175
0.825175
0.816070
0.808573
0.811892
0.823842
0.825175
0.824126
3
4
XGBOOST_2
pca
0.811189
0.811189
0.811189
0.811189
0.808163
0.782772
0.791444
```
19: AutoML in teradataml

```python
0.810161
0.811189
0.807150
4
5
DECISIONFOREST_0
lasso
0.804196
0.804196
0.804196
0.804196
0.816291
0.762588
0.775661
0.809972
0.804196
0.795244
5
6
DECISIONFOREST_2
pca
0.790210
0.790210
0.790210
0.790210
0.820383
0.736787
0.750581
0.807015
0.790210
0.774915
6
7
SVM_3
lasso
0.783217
0.783217
0.783217
0.783217
0.775737
0.753017
0.760547
0.780676
0.783217
0.778580
7
8
SVM_1
rfe
0.783217
0.783217
0.783217
0.783217
0.775737
0.753017
0.760547
0.780676
0.783217
0.778580
8
9
GLM_3
lasso
0.783217
0.783217
0.783217
0.783217
0.775737
0.753017
0.760547
0.780676
0.783217
0.778580
9
10
GLM_1
rfe
0.776224
0.776224
0.776224
0.776224
0.768939
0.743758
0.751628
0.773575
0.776224
0.770758
10
11
SVM_0
lasso
0.762238
0.762238
0.762238
0.762238
0.747332
0.750728
0.748864
0.764161
0.762238
0.763048
11
12
GLM_2
pca
0.755245
0.755245
0.755245
0.755245
0.739876
0.734186
0.736648
0.752996
0.755245
0.753777
12
13
SVM_2
pca
0.734266
0.734266
0.734266
0.734266
0.717544
0.706409
0.710465
0.729996
0.734266
0.730783
13
14
DECISIONFOREST_1
rfe
0.734266
0.734266
0.734266
0.734266
0.730270
0.684561
0.691950
0.732240
0.734266
0.719894
14
15
XGBOOST_1
rfe
0.713287
0.713287
0.713287
0.713287
0.707576
0.656783
0.661353
0.710172
0.713287
0.693811
15
16
GLM_0
lasso
0.601399
0.601399
0.601399
0.601399
0.634236
0.636080
0.601321
0.671311
0.601399
0.602685
16 rows X 13 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
```
19: AutoML in teradataml

```python
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 18/18
```
4.
* Display model leaderboard.
```python
aml.leaderboard()
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
DECISIONFOREST_3
lasso
0.816
0.816
0.816
0.816
0.806990
0.816058
0.810118
0.821300
0.816
0.817337
1
2
DECISIONFOREST_0
lasso
0.808
0.808
0.808
0.808
0.803885
0.787728
0.793616
0.806734
0.808
0.805385
2
3
DECISIONFOREST_1
rfe
0.808
0.808
0.808
0.808
0.803885
0.787728
0.793616
0.806734
0.808
0.805385
3
4
XGBOOST_2
pca
0.792
0.792
0.792
0.792
0.781821
0.781821
0.781821
0.792000
0.792
0.792000
4
5
XGBOOST_0
lasso
0.784
0.784
0.784
0.784
0.775751
0.786117
0.778325
0.792884
0.784
0.785986
5
6
XGBOOST_3
lasso
0.784
0.784
0.784
0.784
0.775751
0.786117
0.778325
0.792884
0.784
0.785986
6
7
XGBOOST_1
rfe
0.784
0.784
0.784
0.784
0.775751
0.786117
0.778325
0.792884
0.784
0.785986
7
8
DECISIONFOREST_2
pca
0.728
0.728
0.728
0.728
0.714403
0.711063
0.712527
0.726245
0.728
0.726933
8
9
GLM_2
pca
0.712
0.712
0.712
0.712
0.711222
0.665279
0.669118
0.711555
0.712
0.694847
9
10
GLM_0
lasso
0.704
0.704
0.704
0.704
0.702632
0.655075
0.657636
0.703200
0.704
0.684850
10
11
GLM_1
rfe
0.704
0.704
0.704
0.704
0.702632
0.655075
0.657636
0.703200
0.704
0.684850
11
12
GLM_3
lasso
0.632
0.632
0.632
0.632
0.610228
0.559613
0.535391
0.616985
0.632
0.581153
```
19: AutoML in teradataml

```python
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
DECISIONFOREST_3
lasso
0.832168
0.832168
0.832168
0.832168
0.820710
0.825114
0.822727
0.833646
0.832168
0.832740
1
2
XGBOOST_3
lasso
0.825175
0.825175
0.825175
0.825175
0.816070
0.808573
0.811892
0.823842
0.825175
0.824126
2
3
XGBOOST_0
lasso
0.825175
0.825175
0.825175
0.825175
0.816070
0.808573
0.811892
0.823842
0.825175
0.824126
3
4
XGBOOST_2
pca
0.811189
0.811189
0.811189
0.811189
0.808163
0.782772
0.791444
0.810161
0.811189
0.807150
4
5
DECISIONFOREST_0
lasso
0.804196
0.804196
0.804196
0.804196
0.816291
0.762588
0.775661
0.809972
0.804196
0.795244
5
6
DECISIONFOREST_2
pca
0.790210
0.790210
0.790210
0.790210
0.820383
0.736787
0.750581
0.807015
0.790210
0.774915
6
7
SVM_3
lasso
0.783217
0.783217
0.783217
0.783217
0.775737
0.753017
0.760547
0.780676
0.783217
0.778580
7
8
SVM_1
rfe
0.783217
0.783217
0.783217
0.783217
0.775737
0.753017
0.760547
0.780676
0.783217
0.778580
8
9
GLM_3
lasso
0.783217
0.783217
0.783217
0.783217
0.775737
0.753017
0.760547
0.780676
0.783217
0.778580
9
10
GLM_1
rfe
0.776224
0.776224
0.776224
0.776224
0.76
9
0.743758
0.751628
0.773575
0.776224
0.770758
10
11
SVM_0
lasso
0.762238
0.762238
0.762238
0.762238
0.747332
0.750728
0.748864
0.764161
0.762238
0.763048
11
12
GLM_2
pca
0.755245
0.755245
0.755245
0.755245
0.739876
0.734186
0.736648
0.752996
0.755245
0.753777
12
13
SVM_2
pca
0.734266
0.734266
0.734266
0.734266
0.717544
0.706409
0.710465
0.729996
0.734266
0.730783
13
14
DECISIONFOREST_1
rfe
0.734266
0.734266
```
19: AutoML in teradataml

```python
0.734266
0.734266
0.730270
0.684561
0.691950
0.732240
0.734266
0.719
14
15
XGBOOST_1
rfe
0.713287
0.713287
0.713287
0.713287
0.707576
0.656783
0.661353
0.710172
0.713287
0.693811
15
16
GLM_0
lasso
0.601399
0.601399
0.601399
0.601399
0.634236
0.636080
0.601321
0.671311
0.601399
0.602685
```
5.
* Display the best performing model.
```python
aml.leader()
RANK  MODEL_ID         FEATURE_SELECTION  ACCURACY   MICRO-PRECISION  MICRO-
RECALL  MICRO-F1   MACRO-PRECISION   MACRO-RECALL   MACRO-F1   WEIGHTED-
PRECISION   WEIGHTED-RECALL   WEIGHTED-F1
1     DECISIONFOREST_3 lasso              0.816      0.816
0.816         0.816      0.80699           0.816058       0.810118
0.8213               0.816             0.817337
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
DECISIONFOREST_3
lasso
0.832168
0.832168
0.832168
0.832168
0.82071 0.825114
0.822727
0.833646
0.832168
0.83274
```
6.
* Generate prediction on validation dataset using best performing model.
* Note:
* In the data preparation phase, AutoML generates the validation dataset by splitting the data
* provided during fitting into training and testing sets. AutoML's model training utilizes the training
* data, with the testing data acting as the validation dataset for model evaluation.
```python
prediction = aml.predict()
Following model is being used for generating prediction :
Model ID : DECISIONFOREST_3
Feature Selection Method : lasso
Prediction :
survived   id  prediction  prob
0         0  369           0  0.50
1         0  449           0  0.65
2         1  373           1  0.65
```
19: AutoML in teradataml

```python
3         1  537           1  0.90
4         0  553           0  0.60
5         0   24           0  0.95
6         0  541           0  0.65
7         1   29           1  1.00
8         0  481           0  0.85
9         0  185           0  1.00
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  Precision    Recall        F1
Support
SeqNum
0               0  CLASS_1       76       11   0.873563  0.853933  0.863636
89
1               1  CLASS_2       13       43   0.767857  0.796296  0.781818
54
ROC-AUC :
AUC
GINI
0.7669579692051602
0.5339159384103205
threshold_value tpr
fpr
0.04081632653061224
0.7962962962962963
0.14606741573033707
0.08163265306122448
0.7962962962962963
0.14606741573033707
0.1020408163265306
0.7962962962962963
0.14606741573033707
0.12244897959183673
0.7962962962962963
0.14606741573033707
0.16326530612244897
0.7962962962962963
0.14606741573033707
0.18367346938775508
0.7962962962962963
0.14606741573033707
0.14285714285714285
0.7962962962962963
0.14606741573033707
0.061224489795918366
0.7962962962962963
0.14606741573033707
0.02040816326530612
0.7962962962962963
0.14606741573033707
0.0
1.0
1.0
Confusion Matrix :
array([[76, 13],
[11, 43]], dtype=int64)
prediction.head()
survived
id
prediction
prob
0
553
0
0.6
0
184
0
0.85
0
633
0
0.95
0
200
0
1.0
0
448
0
1.0
0
480
0
0.85
0
208
0
0.95
0
24
0
0.95
```
19: AutoML in teradataml

```python
0
541
0
0.65
0
369
0
0.5
```
7.
* Display hyperparameters for trained model.
* a.
    * Display mode hyperparameters for rank 1.
```python
aml.model_hyperparameters(rank=1)
{'response_column': 'survived',
'name': 'decision_forest',
'tree_type': 'Classification',
'min_impurity': 0.0,
'max_depth': 5,
'min_node_size': 1,
'num_trees': 20,
'seed': 42,
'persist': False,
'output_prob': True,
'output_responses': ['1', '0']}
```
* b.
    * Display model hyperparameters for rank 4.
```python
aml.model_hyperparameters(rank=4)
{'response_column': 'survived',
'name': 'xgboost',
'model_type': 'Classification',
'column_sampling': 1,
'min_impurity': 0.0,
'lambda1': 0.01,
'shrinkage_factor': 0.5,
'max_depth': 5,
'min_node_size': 1,
'iter_num': 10,
'seed': 42,
'persist': False,
'output_prob': True,
'output_responses': ['1', '0']}
```
8.
* Generate prediction on test dataset using best performing model.
```python
prediction = aml.predict(titanic_test)
Data Transformation started ...
```
19: AutoML in teradataml

```python
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping futile columns :
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
202
0
3
male
None
8
2
69.55
None
S
15
303
0
3
male
19
0
0
0.0
None
S
11
465
0
3
male
None
0
0
8.05
None
S
19
183
0
3
male
9
4
2
31.3875 None
S
8
326
1
1
female
36
0
0
135.6333
C32
C
13
385
0
3
male
None
0
0
7.8958
None
S
21
120
0
3
female
2
4
2
31.275
None
S
10
751
1
2
female
4
1
1
23.0
None
S
18
425
0
3
male
18
1
1
20.2125 None
S
12
118
0
2
male
29
1
0
21.0
None
S
20
178 rows X 11 columns
Updated dataset after performing target column transformation :
sex
id
embarked
cabin
age
passenger
sibsp
parch
fare
pclass
survived
female
10
S
None
2
120
4
2
31.275
3
0
male
15
S
None
None
202
8
2
69.55
3
0
male
23
S
C95
None
528
0
0
221.7792
1
0
female
9
Q
None
None
265
0
0
7.75
3
0
male
12
S
None
18
425
1
1
20.2125 3
0
male
20
S
None
29
118
1
0
21.0
2
0
female
14
S
None
None
181
8
2
69.55
3
```
19: AutoML in teradataml

```python
0
female
22
S
B22
36
541
0
2
71.0
1
1
female
13
C
C32
36
326
0
0
135.6333
1
1
male
21
S
None
None
385
0
0
7.8958
3
0
178 rows X 11 columns
Updated dataset after dropping missing value containing columns :
sex
id
embarked
age
passenger
sibsp
parch
fare
pclass
survived
female
9
Q
None
265
0
0
7.75
3
0
male
11
S
19
303
0
0
0.0
3
0
male
19
S
None
465
0
0
8.05
3
0
female
13
C
36
326
0
0
135.6333
1
1
female
14
S
None
181
8
2
69.55
3
0
female
22
S
36
541
0
2
71.0
1
1
male
15
S
None
202
8
2
69.55
3
0
male
23
S
None
528
0
0
221.7792
1
0
male
12
S
18
425
1
1
20.2125 3
0
male
20
S
29
118
1
0
21.0
2
0
178 rows X 10 columns
Updated dataset after imputing missing value containing columns :
sex
id
embarked
age
passenger
sibsp
parch
fare
pclass
survived
female
9
Q
30
265
0
0
7.75
3
0
female
13
C
36
326
0
0
135.6333
1
1
male
21
S
30
385
0
0
7.8958
3
0
female
10
S
2
120
4
2
31.275
3
0
male
12
S
18
425
1
1
20.2125 3
0
male
20
S
29
118
1
0
21.0
2
0
male
15
S
30
202
8
2
69.55
3
0
male
23
S
30
528
0
0
221.7792
1
0
female
14
S
30
181
8
2
69.55
3
0
female
22
S
36
541
0
2
71.0
1
1
```
19: AutoML in teradataml

```python
178 rows X 10 columns
Updated dataset after performing categorical encoding :
sex_0
sex_1
id
embarked_0
embarked_1
embarked_2
age
passenger
sibsp
parch
fare
pclass
survived
1
0
163
0
0
1
35
212
0
0
21.0
2
1
1
0
40
0
0
1
30
236
0
0
7.55
3
0
1
0
48
0
0
1
38
26
1
5
31.3875 3
1
1
0
72
0
0
1
24
346
0
0
13.0
2
1
1
0
88
1
0
0
45
363
0
1
14.4542 3
0
1
0
104
0
0
1
21
403
1
0
9.825
3
0
0
1
27
0
0
1
20
665
1
0
7.925
3
1
0
1
51
0
0
1
26
70
2
0
8.6625
3
0
0
1
59
0
0
1
50
661
2
0
133.65
1
1
0
1
67
0
0
1
33
720
0
0
7.775
3
0
178 rows X 13 columns
Performing transformation carried out in data preparation phase ...
Updated dataset after performing Lasso feature selection:
id
sex_0
embarked_1
sex_1
embarked_0
age
embarked_2
passenger
sibsp
fare
pclass
survived
48
1
0
0
0
38
1
26
1
31.3875 3
1
88
1
0
0
1
45
0
363
0
14.4542 3
0
104
1
0
0
0
21
1
403
1
9.825
3
0
168
1
1
0
0
22
0
290
0
7.75
3
1
41
1
0
0
0
18
1
787
0
7.4958
3
1
105
1
0
0
0
47
1
872
1
```
19: AutoML in teradataml

```python
52.5542 1
1
59
0
0
1
0
50
1
661
2
133.65
1
1
83
0
0
1
0
42
1
350
0
8.6625
3
0
91
0
0
1
0
1
1
184
2
39.0
2
1
107
0
0
1
0
28
1
201
0
9.5
3
0
178 rows X 12 columns
Updated dataset after performing scaling on Lasso selected features :
sex_0
embarked_1
sex_1
embarked_0
embarked_2
survived
id
age
passenger
sibsp
fare
pclass
1
0
0
0
1
1
48
0.6862745098039216
0.028089887640449437
0.5
0.5506578947368421
1.0
1
0
0
1
0
0
88
0.8235294117647058
0.4067415730337079
0.0
0.25358245614035085
1.0
1
0
0
0
1
0
104
0.35294117647058826
0.451685393258427
0.5
0.17236842105263156
1.0
1
1
0
0
0
1
168
0.37254901960784315
0.32471910112359553
0.0
0.13596491228070176
1.0
1
0
0
0
1
1
41
0.29411764705882354
0.8831460674157303
0.0
0.13150526315789474
1.0
1
0
0
0
1
1
105
0.8627450980392157
0.9786516853932584
0.5
0.9220035087719298
0.0
0
0
1
0
1
1
59
0.9215686274509803
0.7415730337078652
1.0
2.3447368421052635
0.0
0
0
1
0
1
0
83
0.7647058823529411
0.3921348314606742
0.0
0.15197368421052632
1.0
0
0
1
0
1
1
91
-0.0392156862745098
0.20561797752808988
1.0
0.6842105263157895
0.5
0
0
1
0
1
0
107
0.49019607843137253
0.2247191011235955
0.0
0.16666666666666666
1.0
178 rows X 12 columns
Updated dataset after performing RFE feature selection:
id
sex_0
embarked_1
sex_1
age
embarked_0
embarked_2
passenger
sibsp
parch
pclass
fare
survived
48
1
0
0
38
0
1
26
1
5
3
31.3875 1
88
1
0
0
45
1
0
363
0
1
```
19: AutoML in teradataml

```python
3
14.4542 0
104
1
0
0
21
0
1
403
1
0
3
9.825
0
168
1
1
0
22
0
0
290
0
0
3
7.75
1
41
1
0
0
18
0
1
787
0
0
3
7.4958
1
105
1
0
0
47
0
1
872
1
1
1
52.5542 1
59
0
0
1
50
0
1
661
2
0
1
133.65
1
83
0
0
1
42
0
1
350
0
0
3
8.6625
0
91
0
0
1
1
0
1
184
2
1
2
39.0
1
107
0
0
1
28
0
1
201
0
0
3
9.5
0
178 rows X 13 columns
Updated dataset after performing scaling on RFE selected features :
id
r_sex_0 r_sex_1 r_embarked_0
r_parch r_embarked_1
survived
r_embarked_2
r_age
r_passenger
r_sibsp r_pclass
r_fare
48
1
0
0
5
0
1
1
0.6862745098039216
0.028089887640449437
0.5
1.0
0.5506578947368421
88
1
0
1
1
0
0
0
0.8235294117647058
0.4067415730337079
0.0
1.0
0.25358245614035085
104
1
0
0
0
0
0
1
0.35294117647058826
0.451685393258427
0.5
1.0
0.17236842105263156
168
1
0
0
0
1
1
0
0.37254
960784315
0.32471910112359553
0.0
1.0
0.13596491228070176
41
1
0
0
0
0
1
1
0.29411764705882354
0.8831460674157303
0.0
1.0
0.13150526315789474
105
1
0
0
1
0
1
1
0.8627450980392157
0.9786516853932584
0.5
0.0
0.9220035087719298
59
0
1
0
0
0
1
1
0.9215686274509803
0.7415730337078652
1.0
0.0
2.3447368421052635
```
19: AutoML in teradataml

```python
83
0
1
0
0
0
0
1
0.7647058823529411
0.3921348314606742
0.0
1.0
0.15197368421052632
91
0
1
0
1
0
1
1
-0.0392156862745098
0.20561797752808988
1.0
0.5
0.6842105263157895
107
0
1
0
0
0
0
1
0.49019607843137253
0.2247191011235955
0.0
1.0
0.16666666666666666
178 rows X 13 columns
Updated dataset after performing scaling for PCA feature selection :
sex_0
embarked_1
sex_1
embarked_0
embarked_2
parch
survived
id
passenger
pclass
age
sibsp
fare
1
0
0
0
1
5
1
48
0.028089887640449437
1.0
0.6862745098039216
0.5
0.5506578947368421
1
0
0
1
0
1
0
88
0.4067415730337079
1.0
0.8235294117647058
0.0
0.25358245614035085
1
0
0
0
1
0
0
104
0.451685393258427
1.0
0.35294117647058826
0.5
0.17236842105263156
1
1
0
0
0
0
1
168
0.32471910112359553
1.0
0.37254901960784315
0.0
0.13596491228070176
1
0
0
0
1
0
1
41
0.8831460674157303
1.0
0.29411764705882354
0.0
0.13150526315789474
1
0
0
0
1
1
1
105
0.9786516853932584
0.0
0.8627450980392157
0.5
0.9220035087719298
0
0
1
0
1
0
1
59
0.7415730337078652
0.0
0.9215686274509803
1.0
2.3447368421052635
0
0
1
0
1
0
0
83
0.3921348314606742
1.0
0.7647058823529411
0.0
0.15197368421052632
0
0
1
0
1
1
1
91
0.20561797752808988
0.5
-0.0392156862745098
1.0
0.6842105263157895
0
0
1
0
1
0
0
107
0.2247191011235955
1.0
0.49019607843137253
0.0
```
19: AutoML in teradataml

```python
0.16666666666666666
178 rows X 13 columns
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
survived
0
163
0.737094
-0.643210
-0.106455
-0.013106
0.211914
-0.291361
1
1
59
-0.264140
-0.088646
-1.334543
0.554672
0.042260
1.240676
1
2
40
0.633324
-0.702354
0.379652
-0.240178
0.187011
-0.247377
0
3
83
-0.655426
-0.162827
0.173182
-0.087436
0.108885
-0.110638
0
4
48
0.709263
-0.709839
0.215808
-0.146823
0.537865
0.255863
1
5
91
-0.425439
-0.137706
-0.349825
0.069458
0.538132
0.844262
1
6
72
0.728169
-0.647291
-0.048971
-0.068199
0.065490
-0.282484
1
7
107
-0.648931
-0.165699
0.217979
-0.118870
0.276722
-0.131621
0
8
88
1.126607
0.586259
0.442777
-0.465046
0.025732
-0.184397
0
9
131
-0.605878
-0.178108
0.167856
-0.131784
0.125334
0.383804
0
10 rows X 8 columns
Data Transformation completed.⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 10/10
Following model is being picked for evaluation:
Model ID : DECISIONFOREST_3
Feature Selection Method : lasso
Prediction :
id  prediction  prob_1  prob_0  survived
0   48           1    0.70    0.30         1
1   88           1    0.70    0.30         0
2  104           1    0.65    0.35         0
3  168           1    0.65    0.35         1
4   41           1    0.55    0.45         1
5  105           1    0.85    0.15         1
6   59           1    0.60    0.40         1
```
19: AutoML in teradataml

```python
7   83           0    0.15    0.85         0
8   91           1    0.60    0.40         1
9  107           0    0.15    0.85         0
ROC-AUC :
AUC
GINI
0.8167039620902988
0.6334079241805977
threshold_value tpr
fpr
0.04081632653061224
1.0
1.0
0.08163265306122448
1.0
0.9906542056074766
0.1020408163265306
1.0
0.9719626168224299
0.12244897959183673
1.0
0.9719626168224299
0.16326530612244897
0.9577464788732394
0.7850467289719626
0.18367346938775508
0.9577464788732394
0.7850467289719626
0.14285714285714285
1.0
0.9719626168224299
0.061224489795918366
1.0
0.9906542056074766
0.02040816326530612
1.0
1.0
0.0
1.0
1.0
Confusion Matrix :
array([[80, 27],
[19, 52]], dtype=int64)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping futile columns :
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
122
0
3
male
None
0
0
8.05
None
S
11
734
0
2
male
23
0
0
13.0
None
S
14
795
0
3
male
25
0
0
7.8958
None
S
22
326
1
1
female
36
0
0
135.6333
C32
C
13
242
1
3
female
None
1
0
15.5
None
Q
12
507
1
2
female
33
0
2
26.0
None
S
20
383
0
3
male
32
0
0
7.925
None
S
10
648
1
1
male
56
0
0
35.5
A26
C
```
19: AutoML in teradataml

```python
18
835
0
3
male
18
0
0
8.3
None
S
15
282
0
3
male
28
0
0
7.8542
None
S
23
Updated dataset after performing target column transformation :
cabin
id
sibsp
sex
age
parch
embarked
pclass
passenger
fare
survived
C32
13
0
female
36
0
C
1
326
135.6333
1
None
11
0
male
None
0
S
3
122
8.05
0
None
19
0
female
18
1
S
3
856
9.35
1
None
12
1
female
None
0
Q
3
242
15.5
1
None
14
0
male
23
0
S
2
734
13.0
0
None
22
0
male
25
0
S
3
795
7.8958
0
None
8
0
male
28
0
S
3
509
22.525
0
None
16
3
female
None
1
S
3
486
25.4667 0
None
10
0
male
32
0
S
3
383
7.925
0
A26
18
0
male
56
0
C
1
648
35.5
1
Updated dataset after dropping missing value containing columns :
id
sibsp
sex
age
parch
embarked
pclass
passenger
fare
survived
11
0
male
None
0
S
3
122
8.05
0
13
0
female
36
0
C
1
326
135.6333
1
21
0
male
11
0
C
3
732
18.7875 0
9
0
female
None
0
Q
3
265
7.75
0
12
1
female
None
0
Q
3
242
15.5
1
20
0
female
33
2
S
2
507
26.0
1
14
0
male
23
0
S
2
734
13.0
0
22
0
male
25
0
S
3
795
7.8958
0
15
0
male
18
0
S
3
835
8.3
0
23
0
male
28
0
S
3
282
7.8542
0
```
19: AutoML in teradataml

```python
Updated dataset after imputing missing value containing columns :
id
sibsp
sex
age
parch
embarked
pclass
passenger
fare
survived
118
0
male
19
0
S
3
647
7.8958
0
55
1
male
1
2
S
3
789
20.575
1
135
0
female
55
0
S
2
16
16.0
1
114
0
female
50
1
C
1
300
247.5208
1
66
0
male
35
0
S
3
615
8.05
0
83
0
male
17
0
S
3
434
7.125
0
72
0
male
23
0
S
3
754
7.8958
0
198
2
male
44
0
Q
1
246
90.0
0
38
0
female
36
2
S
1
541
71.0
1
80
5
male
11
2
S
3
60
46.9
0
Found additional 1 rows that contain missing values :
id
sibsp
sex
age
parch
embarked
pclass
passenger
fare
survived
183
1
female
45
4
S
3
168
27.9
0
40
1
male
49
0
C
1
600
56.9292 1
120
0
male
22
0
S
3
113
8.05
0
99
0
male
42
0
S
3
350
8.6625
0
80
5
male
11
2
S
3
60
46.9
0
38
0
female
36
2
S
1
541
71.0
1
122
0
male
61
0
S
1
626
32.3208 0
19
0
female
18
1
S
3
856
9.35
1
61
0
female
45
0
S
2
707
13.5
1
141
0
female
29
0
Q
3
48
7.75
1
Updated dataset after dropping additional missing value containing rows :
id
sibsp
sex
age
parch
embarked
pclass
passenger
fare
survived
99
0
male
42
0
S
3
350
8.6625
0
122
0
male
61
0
S
1
626
32.3208 0
19
0
female
18
1
S
3
856
9.35
1
80
5
male
11
2
S
3
60
46.9
0
61
0
female
45
0
S
2
707
13.5
1
141
0
female
29
0
Q
3
48
7.75
1
183
1
female
45
4
S
3
168
27.9
0
76
0
male
34
0
C
3
844
6.4375
0
101
1
female
45
1
S
1
857
164.8667
1
17
0
female
29
0
Q
3
301
7.75
1
```
19: AutoML in teradataml

```python
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713851702256826"'
Updated dataset after performing categorical encoding :
id
sibsp
sex_0
sex_1
age
parch
embarked_0
embarked_1
embarked_2
pclass
passenger
fare
survived
162
0
0
1
29
0
0
0
1
3
82
9.5
1
183
1
1
0
45
4
0
0
1
3
168
27.9
0
76
0
0
1
34
0
1
0
0
3
844
6.4375
0
80
5
0
1
11
2
0
0
1
3
60
46.9
0
40
1
0
1
49
0
1
0
0
1
600
56.9292 1
120
0
0
1
22
0
0
0
1
3
113
8.05
0
61
0
1
0
45
0
0
0
1
2
707
13.5
1
141
0
1
0
29
0
0
1
0
3
48
7.75
1
99
0
0
1
42
0
0
0
1
3
350
8.6625
0
95
0
0
1
28
0
0
0
1
1
24
35.5
1
Performing transformation carried out in data preparation phase ...
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713844397835341"'
Updated dataset after performing Lasso feature selection:
id
embarked_2
sex_0
sibsp
embarked_0
age
sex_1
pclass
embarked_1
passenger
fare
survived
87
1
0
0
0
32
1
3
0
520
7.8958
0
123
1
0
0
0
30
1
3
0
489
8.05
0
81
1
1
0
0
22
0
3
0
142
7.75
1
142
1
1
0
0
42
0
2
0
866
13.0
1
176
1
0
0
0
18
1
3
0
776
7.75
0
33
1
0
0
0
47
1
1
0
663
```
19: AutoML in teradataml

```python
25.5875 0
9
0
1
0
0
29
0
3
1
265
7.75
0
161
0
0
0
1
29
1
3
0
532
7.2292
0
153
0
0
0
1
29
1
3
0
860
7.2292
0
143
0
0
0
0
21
1
3
1
422
7.7333
0
Updated dataset after performing scaling on Lasso selected features :
embarked_2
id
sex_0
embarked_0
survived
sex_1
embarked_1
sibsp
age
pclass
passenger
fare
1
87
0
0
0
1
0
0.0
0.5686274509803921
1.0
0.5831460674157304
0.13852280701754385
1
123
0
0
0
1
0
0.0
0.5294117647058824
1.0
0.5483146067415731
0.14122807017543862
1
81
1
0
1
0
0
0.0
0.37254901960784315
1.0
0.15842696629213482
0.13596491228070176
1
142
1
0
1
0
0
0.0
0.7647058823529411
0.5
0.9719101123595506
0.22807017543859648
1
176
0
0
0
1
0
0.0
0.29411764705882354
1.0
0.8707865168539326
0.13596491228070176
1
33
0
0
0
1
0
0.0
0.8627450980392157
0.0
0.7438202247191011
0.4489035087719298
0
9
1
0
0
0
1
0.0
0.5098039215686274
1.0
0.2966292134831461
0.13596491228070176
0
161
0
1
0
1
0
0.0
0.5098039215686274
1.0
0.596629213483146
0.1268280701754386
0
153
0
1
0
1
0
0.0
0.5098039215686274
1.0
0.9651685393258427
0.1268280701754386
0
143
0
0
0
1
1
0.0
0.35294117647058826
1.0
0.4730337078651685
0.1356719298245614
Updated dataset after performing RFE feature selection:
id
sex_0
age
sex_1
pclass
passenger
fare
survived
87
0
32
1
3
520
7.8958
0
123
0
30
1
3
489
8.05
0
81
1
22
0
3
142
7.75
1
142
1
42
0
2
866
13.0
1
176
0
18
1
3
776
7.75
0
33
0
47
1
1
663
25.5875 0
9
1
29
0
3
265
7.75
0
161
0
29
1
3
532
7.2292
0
```
19: AutoML in teradataml

```python
153
0
29
1
3
860
7.2292
0
143
0
21
1
3
422
7.7333
0
Updated dataset after performing scaling on RFE selected features :
r_sex_0 r_sex_1 survived
id
r_age
r_pclass
r_passenger
r_fare
0
1
0
87
0.5686274509803921
1.0
0.5831460674157304
0.13852280701754385
0
1
0
123
0.5294117647058824
1.0
0.5483146067415731
0.14122807017543862
1
0
1
81
0.37254901960784315
1.0
0.15842696629213482
0.13596491228070176
1
0
1
142
0.7647058823529411
0.5
0.9719101123595506
0.22807017543859648
0
1
0
176
0.29411764705882354
1.0
0.8707865168539326
0.13596491228070176
0
1
0
33
0.8627450980392157
0.0
0.7438202247191011
0.4489035087719298
1
0
0
9
0.5098039215686274
1.0
0.2966292134831461
0.13596491228070176
0
1
0
161
0.5098039215686274
1.0
0.596629213483146
0.1268280701754386
0
1
0
153
0.5098039215686274
1.0
0.9651685393258427
0.1268280701754386
0
1
0
143
0.35294117647058826
1.0
0.4730337078651685
0.1356719298245614
Updated dataset after performing scaling for PCA feature selection :
embarked_2
sex_0
id
embarked_0
survived
sex_1
parch
embarked_1
passenger
pclass
age
sibsp
fare
0
0
153
1
0
1
0
0
0.9651685393258427
1.0
0.5098039215686274
0.0
0.1268280701754386
0
1
57
1
1
0
0
0
0.44157303370786516
0.0
0.39215686274509803
0.5
1.987280701754386
0
1
158
0
1
0
0
1
0.7831460674157303
1.0
0.5098039215686274
0.0
0.1356719298245614
0
1
175
1
1
0
0
0
0.42134831460674155
0.0
0.5098039215686274
0.5
1.4415929824561404
0
0
35
1
0
1
0
0
0.08202247191011236
1.0
0.45098039215686275
0.5
```
19: AutoML in teradataml

```python
0.25358245614035085
0
0
94
1
0
1
0
0
0.06404494382022471
1.0
0.49019607843137253
0.0
0.1268280701754386
1
1
156
0
0
0
1
0
0.350561797752809
0.5
0.45098039215686275
0.5
0.45614035087719296
1
0
89
0
0
1
1
0
0.1797752808988764
1.0
0.803921568627451
0.0
0.28245614035087724
1
0
87
0
0
1
0
0
0.5831460674157304
1.0
0.5686274509803921
0.0
0.13852280701754385
1
1
106
0
1
0
2
0
0.3831460674157303
0.0
0.4117647058823529
1.5
4.614035087719298
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
survived
0
156
0.798037
-0.658267
-0.179850
-0.126249
0.169360
0.212713
0
1
9
1.013059
0.113801
0.916631
0.603353
0.469476
-0.145605
0
2
89
-0.622825
-0.158456
0.178399
-0.225522
0.240607
-0.114017
0
3
161
-0.131053
1.130796
0.311504
-0.274859
-0.307450
-0.088209
0
4
87
-0.632859
-0.161795
0.229150
-0.071917
-0.133835
-0.079546
0
172
110
-0.555536
-0.114490
-0.188127
-0.068215
0.246072
-0.229338
0
173
64
-0.575110
-0.171498
0.125830
-0.147249
-0.017382
0.428679
0
174
11
-0.622259
-0.153929
0.255290
-0.286482
0.246139
-0.168777
0
175
34
0.671605
-0.685764
0.387038
-0.289769
0.073168
-0.259325
1
176
101
0.962386
-0.693145
-1.316424
0.531144
-0.066025
0.948557
1
177 rows × 8 columns
Data Transformation completed.
```
19: AutoML in teradataml

```python
Following model is being used for generating prediction :
Model ID : DECISIONFOREST_3
Feature Selection Method : lasso
Prediction :
survived   id  prediction  prob
0         0  153           0  1.00
1         1   57           1  0.95
2         1  158           1  0.70
3         1  175           1  0.95
4         0   35           0  0.80
5         0   94           0  0.90
6         0  156           1  1.00
7         0   89           0  0.95
8         0   87           0  0.95
9         1  106           1  0.95
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  Precision    Recall        F1
Support
SeqNum
1               1  CLASS_2       20       59   0.746835  0.819444  0.781457
72
0               0  CLASS_1       85       13   0.867347  0.809524  0.837438
105
ROC-AUC :
AUC
GINI
0.736441798941799
0.4728835978835979
threshold_value tpr
fpr
0.04081632653061224
0.8194444444444444
0.19047619047619047
0.08163265306122448
0.8194444444444444
0.19047619047619047
0.1020408163265306
0.8194444444444444
0.19047619047619047
0.12244897959183673
0.8194444444444444
0.19047619047619047
0.16326530612244897
0.8194444444444444
0.19047619047619047
0.18367346938775508
0.8194444444444444
0.19047619047619047
0.14285714285714285
0.8194444444444444
0.19047619047619047
0.061224489795918366
0.8194444444444444
0.19047619047619047
0.02040816326530612
0.8194444444444444
0.19047619047619047
0.0
1.0
1.0
Confusion Matrix :
```
19: AutoML in teradataml

```python
array([[85, 20],
[13, 59]], dtype=int64)
prediction.head()
id
prediction
prob_1
prob_0
survived
10
1
0.75
0.25
0
12
0
0.35
0.65
0
13
1
0.85
0.15
1
14
1
0.75
0.25
0
16
0
0.25
0.75
0
17
1
0.65
0.35
1
15
0
0.4
0.6
0
11
0
0.25
0.75
0
9
1
0.65
0.35
0
8
0
0.5
0.5
0
survived
id
prediction
prob
0
191
0
1.0
0
176
0
0.7
0
14
0
0.85
0
120
0
0.9
0
133
0
0.85
0
187
1
0.8
0
9
1
0.75
0
22
0
1.0
0
107
0
0.95
0
56
0
1.0
```
9.
* Generate evaluation metrics on test dataset using best performing model.
```python
performance_metrics = aml.evaluate(titanic_test)
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : DECISIONFOREST_3
Feature Selection Method : lasso
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  Precision    Recall        F1
Support
SeqNum
0               0  CLASS_1       80       19   0.808081  0.747664  0.776699
```
19: AutoML in teradataml

```python
107
1               1  CLASS_2       27       52   0.658228  0.732394  0.693333
71
SeqNum              Metric  MetricValue
0       3        Micro-Recall     0.741573
1       5     Macro-Precision     0.733154
2       6        Macro-Recall     0.740029
3       7            Macro-F1     0.735016
4       9     Weighted-Recall     0.741573
5      10         Weighted-F1     0.743446
6       8  Weighted-Precision     0.748308
7       4            Micro-F1     0.741573
8       2     Micro-Precision     0.741573
9       1            Accuracy     0.741573
performance_metrics
SeqNum
Prediction
Mapping CLASS_1 CLASS_2 Precision
Recall
F1
Support
0
0
CLASS_1 80
19
0.8080808080808081
0.7476635514018691
0.7766990291262135
107
1
1
CLASS_2 27
52
0.6582278481012658
0.7323943661971831
0.6933333333333334
71
```
### Example 4: Run AutoML for classification problem using early
### stopping timer, max_models and customization
This example predicts whether passenger aboard the RMS Titanic survived or not based on
different factors.
**Run AutoML to get the best performing model using following specifications:**
* Add customization for some specific process in AutoML run.
* Use only two models 'xgboost' and ‘decision forest’ for AutoML training.
* Set early stopping timer to 100 sec and max_models to 5.
* Opt for verbose level 2 to get detailed log.
1.
* Load data and split it to train and test datasets.
* a.
    * Load the example data and create teradataml DataFrame.
```python
load_example_data("teradataml", "titanic")
```
19: AutoML in teradataml

```python
titanic = DataFrame.from_table("titanic")
```
* b.
    * Perform sampling to get 80% for training and 20% for testing.
```python
titanic_sample = titanic.sample(frac = [0.8, 0.2])
```
* c.
    * Fetch train and test data.
```python
titanic_train= titanic_sample[titanic_sample['sampleid'] ==
1].drop('sampleid', axis=1)
titanic_test = titanic_sample[titanic_sample['sampleid'] ==
2].drop('sampleid', axis=1)
```
2.
* Add customization and generate custom config JSON file.
```python
AutoML.generate_custom_config("custom_titanic")
Generating custom config JSON for AutoML ...
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  1
Customizing Feature Engineering Phase ...
Available options for customization of feature engineering phase with
corresponding indices:
Index 1: Customize Missing Value Handling
```
19: AutoML in teradataml

```python
Index 2: Customize Bincode Encoding
Index 3: Customize String Manipulation
Index 4: Customize Categorical Encoding
Index 5: Customize Mathematical Transformation
Index 6: Customize Nonlinear Transformation
Index 7: Customize Antiselect Features
Index 8: Back to main menu
Index 9: Generate custom json and exit
Enter the list of indices you want to customize in feature engineering phase:
1,2,4,6,7,8
Customizing Missing Value Handling ...
Provide the following details to customize missing value handling:
Available missing value handling methods with corresponding indices:
Index 1: Drop Columns
Index 2: Drop Rows
Index 3: Impute Missing values
Enter the list of indices for missing value handling methods :  1,3
Enter the feature or list of features for dropping columns with missing
values:  cabin
Available missing value imputation methods with corresponding indices:
Index 1: Statistical Imputation
Index 2: Literal Imputation
Enter the list of corresponding index missing value imputation methods you
want to use:  1
Enter the feature or list of features for imputing missing values using
statistic values:  age
```
19: AutoML in teradataml

```python
Available statistical methods with corresponding indices:
Index 1: min
Index 2: max
Index 3: mean
Index 4: median
Index 5: mode
Enter the index of corresponding statistic imputation method for feature age:
4
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  0
Customization of missing value handling has been completed successfully.
Customizing Bincode Encoding ...
Provide the following details to customize binning and coding encoding:
Available binning methods with corresponding indices:
Index 1: Equal-Width
Index 2: Variable-Width
Enter the feature or list of features for binning:  pclass
Enter the index of corresponding binning method for feature pclass:  1
Enter the number of bins for feature pclass:  3
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  0
Customization of bincode encoding has been completed successfully.
Customizing Categorical Encoding ...
```
19: AutoML in teradataml

```python
Provide the following details to customize categorical encoding:
Available categorical encoding methods with corresponding indices:
Index 1: OneHotEncoding
Index 2: OrdinalEncoding
Index 3: TargetEncoding
Enter the list of corresponding index categorical encoding methods you want to
use:  2,3
Enter the feature or list of features for OrdinalEncoding:  pclass
Enter the feature or list of features for TargetEncoding:  embarked
Available target encoding methods with corresponding indices:
Index 1: CBM_BETA
Index 2: CBM_DIRICHLET
Index 3: CBM_GAUSSIAN_INVERSE_GAMMA
Enter the index of target encoding method for feature embarked:  3
Enter the response column for target encoding method for feature embarked:
survived
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  0
Customization of categorical encoding has been completed successfully.
Customizing Nonlinear Transformation ...
Provide the following details to customize nonlinear transformation:
Enter number of non-linear combination you want to make:  1
Provide the details for non-linear combination 1:
Enter the list of target feature/s for non-linear combination 1:  parch,sibsp
```
19: AutoML in teradataml

```python
Enter the formula for non-linear combination 1:  Y=(X0+X1+1)
Enter the resultant feature for non-linear combination 1:  family_count
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  0
Customization of nonlinear transformation has been completed successfully.
Customizing Antiselect Features ...
Enter the feature or list of features for antiselect:  passenger
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  0
Customization of antiselect features has been completed successfully.
Customization of feature engineering phase has been completed successfully.
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  4
```
19: AutoML in teradataml

```python
Generating custom json and exiting ...
Process of generating custom config file for AutoML has been completed
successfully.
'custom_titanic.json' file is generated successfully under the current working
directory.
```
3.
* Create an AutoML instance.
```python
aml = AutoML(task_type="Classification",
include=['decision_forest','xgboost'],
verbose=2,
max_runtime_secs=100,
max_models=5,
custom_config_file='custom_titanic.json')
```
4.
* Fit the data.
```python
aml.fit(titanic_train, titanic_train.survived)
Received below input for customization :
{
"MissingValueHandlingIndicator": true,
"MissingValueHandlingParam": {
"DroppingColumnIndicator": true,
"DroppingColumnList": [
"cabin"
],
"ImputeMissingIndicator": true,
"StatImputeList": [
"age"
],
"StatImputeMethod": [
"median"
]
},
"BincodeIndicator": true,
"BincodeParam": {
"pclass": {
"Type": "Equal-Width",
"NumOfBins": 3
}
},
"CategoricalEncodingIndicator": true,
```
19: AutoML in teradataml

```python
"CategoricalEncodingParam": {
"OrdinalEncodingIndicator": true,
"OrdinalEncodingList": [
"pclass"
],
"TargetEncodingIndicator": true,
"TargetEncodingList": {
"embarked": {
"encoder_method": "CBM_GAUSSIAN_INVERSE_GAMMA",
"response_column": "survived"
}
}
},
"NonLinearTransformationIndicator": true,
"NonLinearTransformationParam": {
"Combination_1": {
"target_columns": [
"parch",
"sibsp"
],
"formula": "Y=(X0+X1+1)",
"result_column": "family_count"
}
},
"AntiselectIndicator": true,
"AntiselectParam": {
"excluded_columns": [
"passenger"
]
}
}
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 713
Total Columns in the data: 12
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
```
19: AutoML in teradataml

```python
survived
INTEGER 713
0
None
440
273
0
0.0
100.0
age
INTEGER 570
143
None
5
565
0
20.05610098176718
79.94389901823281
ticket
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
name
VARCHAR(1000) CHARACTER SET LATIN
713
0
0
None
None
None
0.0
100.0
passenger
INTEGER 713
0
None
0
713
0
0.0
100.0
cabin
VARCHAR(20) CHARACTER SET LATIN 171
542
0
None
None
None
76.01683029453015
23.983169705469845
sex
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
parch
INTEGER 713
0
None
535
178
0
0.0
100.0
sibsp
INTEGER 713
0
None
479
234
0
0.0
100.0
pclass
INTEGER 713
0
None
0
713
0
0.0
100.0
embarked
VARCHAR(20) CHARACTER SET LATIN 712
1
0
None
None
None
0.1402524544179523
99.85974754558205
fare
FLOAT
713
0
None
11
702
0
0.0
100.0
passenger  survived   pclass      age    sibsp    parch     fare
func
50%      446.000     0.000    3.000   28.000    0.000    0.000   14.500
count    713.000   713.000  713.000  570.000  713.000  713.000  713.000
mean     450.778     0.383    2.289   29.937    0.536    0.405   32.552
min        1.000     0.000    1.000    0.000    0.000    0.000    0.000
max      890.000     1.000    3.000   80.000    8.000    6.000  512.329
75%      666.000     1.000    3.000   39.000    1.000    0.000   31.388
25%      237.000     0.000    1.000   21.000    0.000    0.000    7.896
std      254.742     0.486    0.845   14.717    1.120    0.838   48.654
Statistics of Data:
func
passenger
survived
pclass
age
sibsp
parch
fare
std
254.742 0.486
0.845
14.717
1.12
0.838
48.654
25%
237
0
1
21
0
0
7.896
50%
446
0
3
28
0
0
14.5
75%
666
1
3
39
1
0
31.388
max
890
1
3
80
8
6
512.329
min
1
0
1
0
0
0
0
mean
450.778 0.383
2.289
29.937
0.536
0.405
32.552
count
713
713
713
570
713
713
713
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
```
19: AutoML in teradataml

```python
name                      713
sex                       2
ticket                    566
cabin                     131
embarked                  3
Futile columns in dataset:
ColumnName
name
ticket
Target Column Distribution:
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0        age          21.037868
1      sibsp           4.908836
2       fare          13.183731
3      parch          24.964937
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.73 sec
Starting customized anti-select columns ...
Updated dataset sample after performing anti-select columns:
survived
pclass
name
sex
age
sibsp
parch
ticket
fare
cabin
embarked
0
3
Moore, Mr. Leonard Charles
male
None
0
0
A4. 54510
8.05
None
S
1
1
Young, Miss. Marie Grice
female
36
0
0
PC 17760
135.6333
C32
C
0
3
Williams, Mr. Howard Hugh "Harry"
male
None
0
0
A/5 2466
8.05
None
S
0
2
Berriman, Mr. William John
male
23
0
0
28425
13.0
None
S
```
19: AutoML in teradataml

```python
0
2
Fox, Mr. Stanley Hubert male
36
0
0
22
6
13.0
None
S
1
3
Murphy, Miss. Katherine "Kate"
female
None
1
0
367230
15.5
None
Q
0
3
Allum, Mr. Owen George
male
18
0
0
2223
8.3
None
S
1
2
Clarke, Mrs. Charles V (Ada Maria Winfield)
female
28
1
0
2003
26.0
None
S
0
3
Henry, Miss. Delia
female
None
0
0
382649
7.75
None
Q
0
2
Hocking, Mr. Richard George
male
23
2
1
29104
11.5
None
S
713 rows X 11 columns
Handling less significant features from data ...
Removing Futile columns:
['ticket', 'name']
Sample of Data after removing Futile columns:
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
0
2
male
36
0
0
13.0
None
S
12
0
3
female
None
0
0
7.75
None
Q
9
0
2
male
23
2
1
11.5
None
S
17
0
3
male
18
0
0
8.3
None
S
15
1
1
female
36
0
0
135.6333
C32
C
13
0
3
male
None
0
0
8.05
None
S
21
0
2
male
23
0
0
13.0
None
S
14
0
3
male
22
0
0
7.2292
None
C
22
1
3
female
14
1
0
11.2417 None
C
10
1
1
male
80
0
0
30.0
A23
S
18
713 rows X 10 columns
Total time to handle less significant features: 25.51 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
```
19: AutoML in teradataml

```python
Dropping these columns for handling customized missing value:
['cabin']
Updated dataset sample after performing customized missing value imputation:
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
1
1
female
28
1
0
89.1042 C
136
1
1
male
49
1
0
56.9292 C
208
1
2
female
4
2
1
39.0
S
240
1
1
female
18
2
2
262.375 C
256
1
3
female
38
1
5
31.3875 S
272
1
1
female
33
1
0
90.0
Q
288
0
3
male
28
0
0
7.75
Q
50
0
3
male
30
0
0
8.05
S
82
0
3
male
29
0
0
7.875
S
90
0
3
male
19
0
0
10.1708 S
98
713 rows X 9 columns
Proceeding with default option for handling remaining missing
values.
Checking Missing values in dataset ...
Columns with their missing values:
embarked: 1
Deleting rows of these columns for handling missing values:
['embarked']
Sample of dataset after removing 1 rows:
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
0
3
male
29
0
0
7.875
S
90
0
2
male
25
1
2
41.5792 C
122
0
2
male
28
0
0
0.0
S
130
0
2
male
54
0
0
26.0
S
138
0
3
male
17
0
0
8.6625
S
154
0
2
male
46
0
0
26.0
S
170
1
1
female
32
0
0
76.2917 C
104
1
1
female
44
0
1
57.9792 C
128
1
1
female
28
1
0
89.1042 C
136
1
1
female
63
1
0
77.9583 S
176
712 rows X 9 columns
```
19: AutoML in teradataml

```python
Total time to find missing values in data: 11.60 sec
Imputing Missing Values ...
Analysis completed. No imputation
required.
Time taken to perform imputation: 0.01 sec
Updated dataset sample after performing Equal-Width binning :-
embarked
survived
parch
age
sex
sibsp
fare
id
pclass
Q
1
0
28
female
0
7.75
449
pclass_3
Q
1
0
28
female
0
7.75
342
pclass_3
Q
1
0
28
female
0
7.7333
654
pclass_3
Q
1
0
29
male
0
7.75
750
pclass_3
Q
1
0
28
female
2
23.25
276
pclass_3
Q
1
0
28
female
0
7.7875
332
pclass_3
C
1
0
28
female
1
89.1042 136
pclass_1
C
1
2
18
female
2
262.375 256
pclass_1
C
1
0
28
female
0
7.225
360
pclass_3
C
1
1
23
male
0
63.3583 424
pclass_1
712 rows X 9 columns
No information provided for Variable-Width Transformation.
Skipping customized string
manipulation.
Starting Customized Categorical Feature Encoding ...
Updated dataset sample after performing ordinal encoding:
sibsp
survived
parch
age
sex
embarked
fare
id
pclass
4
0
2
9
female
S
31.275
571
2
4
0
1
7
male
S
39.6875 255
2
4
0
1
16
male
S
39.6875 735
2
4
0
2
2
female
S
31.275
26
2
4
0
1
2
male
Q
29.125
49
2
4
0
1
4
male
Q
29.125
590
2
5
0
2
16
female
S
46.9
172
2
5
0
2
1
male
S
46.9
19
2
5
0
2
14
male
S
46.9
164
2
5
0
2
9
male
S
46.9
492
2
712 rows X 9 columns
```
19: AutoML in teradataml

```python
Updated dataset sample after performing target encoding:
embarked
survived
parch
age
pclass
sex
sibsp
fare
id
0.36363636363636365
0
1
32
2
female
1
15.5
620
0.36363636363636365
1
0
28
2
female
1
15.5
634
0.36363636363636365
1
0
28
2
female
1
15.5
399
0.36363636363636365
1
0
28
2
female
2
23.25
276
0.36363636363636365
0
0
44
0
male
2
90.0
734
0.36363636363636365
0
1
8
2
male
4
29.125
421
0.33463796477495106
0
2
14
2
male
5
46.9
164
0.33463796477495106
0
2
11
2
male
5
46.9
384
0.33463796477495106
1
0
27
0
male
1
53.1
522
0.33463796477495106
1
0
22
0
female
1
66.6
227
712 rows X 9 columns
Performing encoding for categorical columns ...
ONE HOT Encoding these Columns:
['sex']
Sample of dataset after performing one hot encoding:
embarked
survived
parch
age
pclass
sex_0
sex_1
sibsp
fare
id
0.33463796477495106
1
0
36
2
1
0
1
17.4
186
0.33463796477495106
1
1
0
1
0
1
1
14.5
344
0.33463796477495106
1
1
37
0
0
1
1
52.5542 249
0.33463796477495106
1
5
38
2
1
0
1
31.3875 272
0.33463796477495106
0
0
44
1
0
1
1
26.0
437
```
19: AutoML in teradataml

```python
0.33463796477495106
1
1
22
1
1
0
1
29.0
67
0.5579710144
537
1
0
15
2
1
0
0
7.225
362
0.5579710144
537
1
1
50
0
1
0
0
247.5208
466
0.5579710144
537
1
2
22
0
1
0
0
49.5
546
0.5579710144
537
1
0
28
0
1
0
0
110.8833
650
712 rows X 10 columns
Time taken to encode the columns: 16.28 sec
Starting customized mathematical transformation ...
Skipping customized mathematical
transformation.
Starting customized non-linear transformation ...
Possible combination :
['Combination_1']
Updated dataset sample after performing non-liner transformation:
embarked
survived
parch
age
pclass
sex_0
sex_1
sibsp
fare
id
family_count
0.33463796477495106
1
1.0
37
0
0
1
1.0
52.5542 249
3.0
0.33463796477495106
0
0.0
44
1
0
1
1.0
26.0
437
2.0
0.33463796477495106
1
1.0
22
1
1
0
1.0
29.0
67
3.0
0.33463796477495106
0
2.0
26
2
0
1
1.0
20.575
490
4.0
0.33463796477495106
0
0.0
27
1
1
0
1.0
21.0
742
2.0
0.33463796477495106
0
0.0
40
2
1
0
1.0
9.475
301
2.0
0.36363636363636365
1
0.0
28
2
1
0
1.0
15.5
399
2.0
0.36363636363636365
0
0.0
44
0
0
1
2.0
90.0
734
3.0
0.36363636363636365
0
1.0
8
2
0
1
4.0
29.125
421
6.0
```
19: AutoML in teradataml

```python
0.36363636363636365
0
1.0
2
2
0
1
4.0
29.125
49
6.0
712 rows X 11 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
No information provided for performing customized feature scaling. Proceeding
with default option.
No information provided for performing customized imbalanced dataset sampling.
AutoML will Proceed with default option.
Starting customized outlier processing ...
No information provided for customized outlier processing. AutoML will proceed
with default settings.
Outlier preprocessing ...
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0         parch          25.000000
1  family_count          10.393258
2      embarked          19.241573
3         sibsp           4.915730
4           age           7.724719
5          fare          13.061798
Deleting rows of these columns:
['age', 'sibsp']
Sample of dataset after removing outlier rows:
embarked
survived
parch
age
pclass
sex_0
sex_1
sibsp
fare
id
family_count
0.33463796477495106
0
2.0
26
2
0
1
1.0
20.575
490
4.0
0.33463796477495106
0
0.0
40
2
1
0
1.0
9.475
301
2.0
0.33463796477495106
0
0.0
28
2
0
1
1.0
15.85
65
2.0
0.33463796477495106
1
1.0
45
0
1
0
1.0
164.8667
293
3.0
0.33463796477495106
1
0.0
42
0
0
1
1.0
```
19: AutoML in teradataml

```python
52.5542 453
2.0
0.33463796477495106
1
0.0
28
0
1
0
1.0
52.0
487
2.0
0.36363636363636365
1
0.0
28
2
1
0
0.0
7.75
517
1.0
0.36363636363636365
1
0.0
19
2
1
0
0.0
7.8792
331
1.0
0.36363636363636365
1
0.0
28
2
1
0
0.0
7.7333
654
1.0
0.36363636363636365
1
0.0
29
2
0
1
0.0
7.75
750
1.0
627 rows X 11 columns
median inplace of outliers:
['fare', 'embarked', 'parch', 'family_count']
Sample of dataset after performing MEDIAN inplace:
embarked
survived
parch
age
pclass
sex_0
sex_1
sibsp
fare
id
family_count
0.33463796477495106
0
0.0
28
1
0
1
0.0
15.05
378
1.0
0.33463796477495106
0
0.0
28
2
0
1
0.0
7.8958
144
1.0
0.33463796477495106
0
0.0
37
0
0
1
0.0
29.7
192
2.0
0.33463796477495106
0
0.0
33
2
0
1
0.0
7.8958
200
1.0
0.33463796477495106
0
0.0
45
2
1
0
0.0
14.4542 432
2.0
0.33463796477495106
0
0.0
27
0
0
1
0.0
13.0
536
3.0
0.36363636363636365
1
0.0
28
2
1
0
0.0
7.7333
654
1.0
0.36363636363636365
1
0.0
30
1
1
0
0.0
12.35
380
1.0
0.36363636363636365
1
0.0
28
2
1
0
0.0
7.8792
548
1.0
0.36363636363636365
1
0.0
28
2
1
0
0.0
7.8292
700
1.0
627 rows X 11 columns
Time Taken by Outlier processing: 57.51 sec
```
19: AutoML in teradataml

```python
Checking imbalance data ...
Imbalance Not Found.
Feature selection using lasso ...
feature selected by lasso:
['sibsp', 'sex_1', 'family_count', 'age', 'pclass', 'sex_0', 'embarked',
'fare']
Total time taken by feature selection: 4.60 sec
scaling Features of lasso data ...
columns that will be scaled:
['sibsp', 'family_count', 'age', 'pclass', 'embarked', 'fare']
Dataset sample after scaling:
survived
sex_0
sex_1
id
sibsp
family_count
age
pclass
embarked
fare
1
1
0
10
0.5
0.5
0.21568627450980393
1.0
0.0
0.1831516213961733
0
0
1
12
0.0
0.0
0.6470588235294118
0.5
0.0
0.21179813356967833
1
1
0
13
0.0
0.0
0.6470588235294118
0.0
0.0
0.21179813356967833
0
0
1
14
0.0
0.0
0.39215686274509803
0.5
0.0
0.21179813356967833
0
0
1
17
1.0
0.0
0.39215686274509803
0.5
0.0
0.1873598873885616
1
1
0
20
0.5
0.5
0.49019607843137253
1.0
1.0
0.25252854387153956
0
0
1
15
0.0
0.0
0.29411764705882354
1.0
0.0
0.13522496220217925
0
0
1
11
0.0
0.0
0.49019607843137253
1.0
0.0
0.13115192117199315
0
1
0
9
0.0
0.0
0.49019607843137253
1.0
1.0
0.12626427193576978
0
0
1
8
0.0
0.0
0.49019607843137253
1.0
1.0
0.12585696783275116
627 rows X 10 columns
```
19: AutoML in teradataml

```python
Total time taken by feature scaling: 38.58 sec
Feature selection using rfe ...
feature selected by RFE:
['sex_1', 'age', 'pclass', 'sex_0', 'embarked', 'sibsp', 'fare',
'family_count']
Total time taken by feature selection: 17.63 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_age', 'r_pclass', 'r_embarked', 'r_sibsp', 'r_fare', 'r_family_count']
Dataset sample after scaling:
survived
r_sex_0 r_sex_1 id
r_age
r_pclass
r_embarked
r_sibsp r_fare
r_family_count
1
1
0
10
0.21568627450980393
1.0
0.0
0.5
0.1831516213961733
0.5
0
0
1
12
0.6470588235294118
0.5
0.0
0.0
0.21179813356967833
0.0
1
1
0
13
0.6470588235294118
0.0
0.0
0.0
0.21179813356967833
0.0
0
0
1
14
0.39215686274509803
0.5
0.0
0.0
0.21179813356967833
0.0
0
0
1
17
0.39215686274509803
0.5
0.0
1.0
0.1873598873885616
0.0
1
1
0
20
0.49019607843137253
1.0
1.0
0.5
0.25252854387153956
0.5
0
0
1
15
0.29411764705882354
1.0
0.0
0.0
0.13522496220217925
0.0
0
0
1
11
0.49019607843137253
1.0
0.0
0.0
0.1311519211719
5
0.0
0
1
0
9
0.49019607843137253
1.0
1.0
0.0
0.12626427193576978
0.0
0
0
1
8
0.49019607843137253
1.0
1.0
0.0
0.12585696783275116
0.0
627 rows X 10 columns
Total time taken by feature scaling: 33.75 sec
scaling Features of pca data ...
```
19: AutoML in teradataml

```python
columns that will be scaled:
['embarked', 'age', 'pclass', 'sibsp', 'fare', 'family_count']
Dataset sample after scaling:
sex_1
survived
parch
sex_0
id
embarked
age
pclass
sibsp
fare
family_count
1
0
0.0
0
378
0.0
0.49019607843137253
0.5
0.0
0.24519707001720456
0.0
1
0
0.0
0
144
0.0
0.49019607843137253
1.0
0.0
0.12863966946457434
0.0
1
0
0.0
0
192
0.0
0.6666666666666666
0.0
0.0
0.4838772743861113
0.5
1
0
0.0
0
200
0.0
0.5882352941176471
1.0
0.0
0.12863966946457434
0.0
0
0
0.0
1
432
0.0
0.8235294117647058
1.0
0.0
0.23549019863406498
0.5
1
0
0.0
0
536
0.0
0.47058823529411764
0.0
0.0
0.21179813356967833
1.0
0
1
0.0
1
654
1.0
0.49019607843137253
1.0
0.0
0.12599219279495336
0.0
0
1
0.0
1
380
1.0
0.5294117647058824
0.5
0.0
0.2012082268911944
0.0
0
1
0.0
1
548
1.0
0.49019607843137253
1.0
0.0
0.12836921954016997
0.0
0
1
0.0
1
700
1.0
0.49019607843137253
1.0
0.0
0.12755461133413273
0.0
627 rows X 11 columns
Total time taken by feature scaling: 29.22 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5']
Total time taken by PCA: 6.39 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
```
19: AutoML in teradataml

```python
Starting customized hyperparameter update ...
Skipping customized hyperparameter tuning
Hyperparameters used for model training:
response_column : survived
name : xgboost
model_type : Classification
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1, 0.2)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.1, 0.3)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
iter_num : (10, 20, 30)
seed : 42
Total number of models for xgboost : 2592
response_column : survived
name : decision_forest
tree_type : Classification
min_impurity : (0.0, 0.1, 0.2)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
num_trees : (-1, 20, 30)
seed : 42
Total number of models for decision_forest : 108
Performing hyperparameter tuning ...
xgboost
decision_forest
```
19: AutoML in teradataml

```python
Leaderboard
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_0
lasso
0.769841
0.769841
0.769841
0.769841
0.760714
0.770872
0.763265
0.778968
0.769841
0.772033
1
2
XGBOOST_1
rfe
0.769841
0.769841
0.769841
0.769841
0.759259
0.767161
0.761908
0.775720
0.769841
0.771566
2
3
DECISIONFOREST_2
pca
0.753968
0.753968
0.753968
0.753968
0.746221
0.724490
0.730751
0.750937
0.753968
0.748321
3
4
DECISIONFOREST_0
lasso
0.746032
0.746032
0.746032
0.746032
0.733333
0.725417
0.728521
0.743210
0.746032
0.743843
4
5
DECISIONFOREST_1
rfe
0.746032
0.746032
0.746032
0.746032
0.733333
0.725417
0.728521
0.743210
0.746032
0.743843
5 rows X 13 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 19/19
```
5.
* Display model leaderboard.
```python
aml.leaderboard()
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_0
lasso
0.769841
0.769841
0.769841
0.769841
0.760714
0.770872
0.763265
0.778968
0.769841
0.772033
1
2
XGBOOST_1
rfe
0.769841
0.769841
0.769841
0.769841
0.759259
0.767161
0.761908
0.775720
0.769841
0.771566
```
19: AutoML in teradataml

```python
2
3
DECISIONFOREST_2
pca
0.753968
0.753968
0.753968
0.753968
0.746221
0.724490
0.730751
0.750937
0.753968
0.748321
3
4
DECISIONFOREST_0
lasso
0.746032
0.746032
0.746032
0.746032
0.733333
0.725417
0.728521
0.743210
0.746032
0.743843
4
5
DECISIONFOREST_1
rfe
0.746032
0.746032
0.746032
0.746032
0.733333
0.725417
0.728521
0.743210
0.746032
0.743843
```
6.
* Display the best performing model.
```python
aml.leader()
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_0
lasso
0.769841
0.769841
0.769841
0.769841
0.760714
0.770872
0.763265
0.778968
0.769841
0.772033
```
7.
* Display hyperparameters for trained model.
* a.
    * Display model hyperparameters for rank 1.
```python
aml.model_hyperparameters(rank=1)
{'response_column': 'survived',
'name': 'xgboost',
'model_type': 'Classification',
'column_sampling': 1,
'min_impurity': 0.1,
'lambda1': 0.01,
'shrinkage_factor': 0.5,
'max_depth': 8,
'min_node_size': 3,
'iter_num': 10,
'seed': 42,
'persist': False,
'output_prob': True,
'output_responses': ['1', '0'],
'max_models': 1}
```
* b.
    * Display hyperparameters for rqank 4.
```python
aml.model_hyperparameters(rank=4)
```
19: AutoML in teradataml

```python
{'response_column': 'survived',
'name': 'decision_forest',
'tree_type': 'Classification',
'min_impurity': 0.0,
'max_depth': 10,
'min_node_size': 2,
'num_trees': 20,
'seed': 42,
'persist': False,
'output_prob': True,
'output_responses': ['1', '0'],
'max_models': 1}
```
8.
* Generate prediction on test dataset using best performing model.
```python
prediction = aml.predict(titanic_test)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping futile columns :
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
591
0
3
male
35
0
0
7.125
None
S
11
486
0
3
female
None
3
1
25.4667 None
S
8
381
1
1
female
42
0
0
227.525 None
C
16
80
1
3
female
30
0
0
12.475
None
S
12
570
1
3
male
32
0
0
7.8542
None
S
15
162
1
2
female
40
0
0
15.75
None
S
23
873
0
1
male
33
0
0
5.0
B51 B53 B55
S
9
301
1
3
female
None
0
0
7.75
None
Q
17
751
1
2
female
4
1
1
23.0
None
S
10
543
0
3
female
11
4
2
31.275
None
S
18
```
19: AutoML in teradataml

```python
178 rows X 11 columns
Updated dataset after performing target column transformation :
embarked
cabin
parch
age
passenger
sex
pclass
sibsp
fare
id
survived
S
B51 B53 B55
0
33
873
male
1
0
5.0
9
0
C
None
0
11
732
male
3
0
18.7875 13
0
S
None
0
25
667
male
2
0
13.0
21
0
S
None
0
32
570
male
3
0
7.8542
15
1
S
None
0
30
80
female
3
0
12.475
12
1
S
None
0
21
38
male
3
0
8.05
20
0
S
None
0
35
591
male
3
0
7.125
11
0
S
None
0
31
19
female
3
1
18.0
19
0
S
None
1
4
751
female
2
1
23.0
10
1
S
None
2
11
543
female
3
4
31.275
18
0
178 rows X 11 columns
Updated dataset after dropping customized missing value containing columns :
embarked
parch
age
passenger
sex
pclass
sibsp
fare
id
survived
S
0
33
873
male
1
0
5.0
9
0
S
0
30
80
female
3
0
12.475
12
1
S
0
21
38
male
3
0
8.05
20
0
S
0
35
591
male
3
0
7.125
11
0
C
0
11
732
male
3
0
18.7875 13
0
S
0
25
667
male
2
0
13.0
21
0
S
0
32
570
male
3
0
7.8542
15
1
S
0
40
162
female
2
0
15.75
23
1
S
2
None
181
female
3
8
69.55
14
0
C
0
None
257
female
1
0
79.2
22
1
178 rows X 10 columns
```
19: AutoML in teradataml

```python
Updated dataset after imputing customized missing value containing columns :
embarked
parch
age
passenger
sex
pclass
sibsp
fare
id
survived
S
0
35
591
male
3
0
7.125
11
0
S
2
28
181
female
3
8
69.55
14
0
C
0
28
257
female
1
0
79.2
22
1
S
0
32
570
male
3
0
7.8542
15
1
S
0
33
873
male
1
0
5.0
9
0
Q
0
28
301
female
3
0
7.75
17
1
C
0
11
732
male
3
0
18.7875 13
0
S
0
25
667
male
2
0
13.0
21
0
S
0
30
80
female
3
0
12.475
12
1
S
0
21
38
male
3
0
8.05
20
0
178 rows X 10 columns
Found additional 1 rows that contain missing values :
embarked
parch
age
passenger
sex
pclass
sibsp
fare
id
survived
S
0
35
591
male
3
0
7.125
11
0
S
0
33
873
male
1
0
5.0
9
0
Q
0
28
301
female
3
0
7.75
17
1
C
0
11
732
male
3
0
18.7875 13
0
S
2
28
181
female
3
8
69.55
14
0
C
0
28
257
female
1
0
79.2
22
1
S
0
30
80
female
3
0
12.475
12
1
S
0
21
38
male
3
0
8.05
20
0
S
1
4
751
female
2
1
23.0
10
1
S
2
11
543
female
3
4
31.275
18
0
178 rows X 10 columns
Updated dataset after dropping additional missing value containing rows :
embarked
parch
age
passenger
sex
pclass
sibsp
fare
id
survived
S
0
33
873
male
1
0
5.0
9
0
S
1
28
486
female
3
3
25.4667 8
0
C
0
42
381
female
1
0
227.525 16
1
S
0
32
570
male
3
0
7.8542
15
1
S
0
30
80
female
3
0
12.475
12
1
S
0
21
38
male
3
0
8.05
20
0
S
0
35
591
male
3
0
7.125
11
0
S
0
31
19
female
3
1
18.0
19
0
```
19: AutoML in teradataml

```python
C
0
11
732
male
3
0
18.7875 13
0
S
0
25
667
male
2
0
13.0
21
0
177 rows X 10 columns
Updated dataset after performing customized equal width bin-code
transformation :
sibsp
survived
parch
age
passenger
sex
embarked
fare
id
pclass
3
0
2
19
28
male
S
263.0
46
pclass_1
3
0
1
28
230
female
S
25.4667 50
pclass_3
3
0
1
2
8
male
S
21.075
136
pclass_3
3
0
1
28
486
female
S
25.4667 8
pclass_3
1
1
0
17
308
female
C
108.9
64
pclass_1
1
1
0
24
317
female
S
26.0
155
pclass_2
1
1
1
4
751
female
S
23.0
10
pclass_2
1
1
0
28
432
female
S
16.1
83
pclass_3
1
0
0
31
19
female
S
18.0
19
pclass_3
1
1
1
0
832
male
S
18.75
131
pclass_2
177 rows X 10 columns
Updated dataset after performing customized categorical encoding :
embarked
sibsp
survived
parch
age
passenger
sex
pclass
fare
id
0.36363636363636365
0
1
0
15
23
female
2
8.0292
191
0.36363636363636365
0
1
0
28
574
female
2
7.75
153
0.36363636363636365
0
0
0
32
891
male
2
7.75
98
0.36363636363636365
0
1
0
16
209
female
2
7.75
59
0.36363636363636365
0
0
0
28
826
male
2
```
19: AutoML in teradataml

```python
6.95
164
0.36363636363636365
0
0
0
18
655
female
2
6.75
89
0.33463796477495106
0
0
0
20
841
male
2
7.925
171
0.33463796477495106
0
1
0
44
415
male
2
7.925
42
0.33463796477495106
0
0
0
16
575
male
2
8.05
58
0.33463796477495106
0
1
0
28
508
male
0
26.55
74
177 rows X 10 columns
Updated dataset after performing categorical encoding :
embarked
sibsp
survived
parch
age
passenger
sex_0
sex_1
pclass
fare
id
0.33463796477495106
0
0
0
26
620
0
1
1
10.5
130
0.33463796477495106
0
0
0
24
865
0
1
1
13.0
40
0.33463796477495106
0
0
0
41
762
0
1
2
7.125
48
0.33463796477495106
0
0
0
35
5
0
1
2
8.05
56
0.33463796477495106
0
0
0
21
73
0
1
1
73.5
96
0.33463796477495106
0
1
0
30
521
1
0
0
93.5
104
0.36363636363636365
0
1
0
15
23
1
0
2
8.0292
191
0.36363636363636365
0
1
0
28
574
1
0
2
7.75
153
0.36363636363636365
0
0
0
32
891
0
1
2
7.75
98
0.36363636363636365
0
1
0
16
209
1
0
2
7.75
59
177 rows X 11 columns
Updated dataset after performing customized non-linear transformation :
embarked
sibsp
survived
parch
age
passenger
sex_0
sex_1
pclass
fare
id
family_count
0.33463796477495106
0.0
0
0.0
41
762
0
1
```
19: AutoML in teradataml

```python
2
7.125
48
1.0
0.33463796477495106
0.0
0
0.0
21
73
0
1
1
73.5
96
1.0
0.33463796477495106
0.0
1
0.0
30
521
1
0
0
93.5
104
1.0
0.33463796477495106
0.0
0
0.0
24
235
0
1
1
10.5
112
1.0
0.33463796477495106
0.0
0
0.0
45
130
0
1
2
6.975
144
1.0
0.33463796477495106
0.0
0
0.0
28
170
0
1
2
56.4958 152
1.0
0.5579710144927537
0.0
1
0.0
30
843
1
0
0
31.0
163
1.0
0.5579710144927537
0.0
0
0.0
28
27
0
1
2
7.225
138
1.0
0.5579710144927537
0.0
1
0.0
42
381
1
0
0
227.525 16
1.0
0.5579710144927537
0.0
1
1.0
36
680
0
1
0
512.3292
41
2.0
177 rows X 12 columns
Updated dataset after performing customized anti-selection :
embarked
sibsp
survived
parch
age
sex_0
sex_1
pclass
fare
id
family_count
0.33463796477495106
0.0
0
0.0
41
0
1
2
7.125
48
1.0
0.33463796477495106
0.0
0
0.0
21
0
1
1
73.5
96
1.0
0.33463796477495106
0.0
1
0.0
30
1
0
0
93.5
104
1.0
0.33463796477495106
0.0
0
0.0
24
0
1
1
10.5
112
1.0
0.33463796477495106
0.0
0
0.0
45
0
1
2
6.975
144
1.0
0.33463796477495106
0.0
0
0.0
28
0
1
2
56.4958 152
1.0
0.36363636363636365
0.0
0
0.0
32
0
1
2
7.75
98
1.0
0.36363636363636365
0.0
0
0.0
28
0
1
2
6.95
164
1.0
0.36363636363636365
0.0
0
0.0
18
1
0
2
6.75
89
1.0
0.36363636363636365
0.0
1
0.0
28
1
0
2
```
19: AutoML in teradataml

```python
7.75
114
1.0
177 rows X 11 columns
Performing transformation carried out in data preparation phase ...
Updated dataset after performing Lasso feature selection:
id
sibsp
sex_1
family_count
age
pclass
sex_0
embarked
fare
survived
69
1.0
0
3.0
28
2
1
0.558
22.3583 1
76
1.0
0
2.0
28
2
1
0.558
14.4583 0
53
1.0
0
2.0
54
0
1
0.558
78.2667 1
22
0.0
0
1.0
28
0
1
0.558
79.2
1
101
1.0
0
3.0
9
2
1
0.558
15.2458 0
147
0.0
0
1.0
44
0
1
0.558
27.7208 1
48
0.0
1
1.0
41
2
0
0.3346
7.125
0
96
0.0
1
1.0
21
1
0
0.3346
73.5
0
112
0.0
1
1.0
24
1
0
0.3346
10.5
0
128
0.0
1
1.0
51
1
0
0.3346
12.525
0
177 rows X 10 columns
Updated dataset after performing scaling on Lasso selected features :
survived
sex_0
sex_1
id
sibsp
family_count
age
pclass
embarked
fare
0
0
1
112
0.0
0.0
0.4117647058823529
0.5
0.0
0.1710677232678171
0
0
1
152
0.0
0.0
0.49019607843137253
1.0
0.0
0.9204388457327565
0
0
1
176
0.0
0.0
0.29411764705882354
0.5
0.0
0.1873598873885616
1
0
1
33
0.0
0.0
0.49019607843137253
1.0
0.0
0.9204388457327565
0
0
1
81
0.0
0.0
0.803921568627451
1.0
0.0
0.13115192117199315
0
0
1
97
0.0
0.0
0.9411764705882353
1.0
0.0
0.12626427193576978
0
1
0
30
0.0
0.5
0.29411764705882354
1.0
7.703448275862078
0.23549019863406498
1
1
0
100
0.5
0.5
1.1176470588235294
0.0
7.703448275862078
1.2259853500860227
1
1
0
69
0.5
1.0
0.49019607843137253
1.0
7.703448275862078
0.3642650930608415
1
1
0
64
0.5
0.5
0.27450980392156865
0.0
```
19: AutoML in teradataml

```python
7.703448275862078
1.7742166727490747
177 rows X 10 columns
Updated dataset after performing RFE feature selection:
id
sex_1
age
pclass
sex_0
embarked
sibsp
fare
family_count
survived
112
1
24
1
0
0.3346
0.0
10.5
1.0
0
152
1
28
2
0
0.3346
0.0
56.4958 1.0
0
176
1
18
1
0
0.3346
0.0
11.5
1.0
0
33
1
28
2
0
0.3346
0.0
56.4958 1.0
1
81
1
44
2
0
0.3346
0.0
8.05
1.0
0
97
1
51
2
0
0.3346
0.0
7.75
1.0
0
30
0
18
2
1
0.558
0.0
14.4542 2.0
0
100
0
60
0
1
0.558
1.0
75.25
2.0
1
69
0
28
2
1
0.558
1.0
22.3583 3.0
1
64
0
17
0
1
0.558
1.0
108.9
2.0
1
177 rows X 10 columns
Updated dataset after performing scaling on RFE selected features :
survived
r_sex_0 r_sex_1 id
r_age
r_pclass
r_embarked
r_sibsp r_fare
r_family_count
0
0
1
112
0.4117647058823529
0.5
0.0
0.0
0.1710677232678171
0.0
0
0
1
152
0.49019607843137253
1.0
0.0
0.0
0.9204388457327565
0.0
0
0
1
176
0.29411764705882354
0.5
0.0
0.0
0.1873598873885616
0.0
1
0
1
33
0.49019607843137253
1.0
0.0
0.0
0.9204388457327565
0.0
0
0
1
81
0.803921568627451
1.0
0.0
0.0
0.13115192117199315
0.0
0
0
1
97
0.9411764705882353
1.0
0.0
0.0
0.12626427193576978
0.0
0
1
0
30
0.29411764705882354
1.0
7.703448275862078
0.0
0.23549019863406498
0.5
1
1
0
100
1.1176470588235294
0.0
7.703448275862078
0.5
1.2259853500860227
0.5
1
1
0
69
0.49019607843137253
1.0
7.703448275862078
0.5
0.3642650930608415
1.0
1
1
0
64
0.27450980392156865
0.0
7.703448275862078
0.5
1.7742166727490747
0.5
```
19: AutoML in teradataml

```python
177 rows X 10 columns
Updated dataset after performing scaling for PCA feature selection :
sex_1
survived
parch
sex_0
id
embarked
age
pclass
sibsp
fare
family_count
1
0
0.0
0
112
-0.001309202453986794
0.4117647058823529
0.5
0.0
0.1710677232678171
0.0
1
0
0.0
0
152
-0.001309202453986794
0.49019607843137253
1.0
0.0
0.9204388457327565
0.0
1
0
0.0
0
176
-0.001309202453986794
0.29411764705882354
0.5
0.0
0.1873598873885616
0.0
1
1
0.0
0
33
-0.001309202453986794
0.49019607843137253
1.0
0.0
0.9204388457327565
0.0
1
0
0.0
0
81
-0.001309202453986794
0.803921568627451
1.0
0.0
0.13115192117199315
0.0
1
0
0.0
0
97
-0.001309202453986794
0.9411764705882353
1.0
0.0
0.12626427193576978
0.0
0
0
1.0
1
30
7.702564417177909
0.29411764705882354
1.0
0.0
0.23549019863406498
0.5
0
1
0.0
1
100
7.702564417177909
1.1176470588235294
0.0
0.5
1.2259853500860227
0.5
0
1
1.0
1
69
7.702564417177909
0.49019607843137253
1.0
0.5
0.3642650930608415
1.0
0
1
0.0
1
64
7.702564417177909
0.27450980392156865
0.0
0.5
1.7742166727490747
0.5
177 rows X 11 columns
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
survived
0
48
-0.604079
-0.251408
-0.082146
-0.116307
-0.14
1388
-0.241917
0
1
30
1.083819
-2.515037
0.920687
7.062517
0.789084
0.377344
0
2
96
-0.439311
0.376303
-0.260180
0.024478
-0.076691
0.327340
0
3
100
1.388431
-1.238005
0.658307
7.497925
-0.008191
0.191678
1
4
112
-0.508756
0.115662
-0.259576
-0.036426
0.114863
0.093730
0
5
69
1.222904
-2.182757
1.518028
7.114807
0.549365
0.286522
1
6
128
-0.505860
0.195651
-0.310560
```
19: AutoML in teradataml

```python
0.035997
-0.172383
-0.335267
0
7
64
1.424510
-1.208196
0.735848
7.422800
0.318941
0.983343
1
8
152
-0.549918
-0.075297
-0.062591
-0.096272
-0.18
1032
0.112703
0
9
76
1.136537
-2.376952
1.142662
7.107592
0.354982
0.424665
0
10 rows X 8 columns
Data Transformation completed.⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 15/15
Following model is being picked for evaluation:
Model ID : XGBOOST_0
Feature Selection Method : lasso
Prediction :
id  Prediction  survived    prob_0    prob_1
0   69           1         1  0.212129  0.787871
1   76           1         0  0.212129  0.787871
2   53           1         1  0.014311  0.985689
3   22           1         1  0.014311  0.985689
4  101           1         0  0.212129  0.787871
5  147           1         1  0.124797  0.875203
6   48           0         0  0.913048  0.086952
7   96           1         0  0.308215  0.691785
8  112           0         0  0.764506  0.235494
9  128           0         0  0.813978  0.186022
ROC-AUC :
AUC
GINI
0.8124662709120346
0.6249325418240692
threshold_value tpr
fpr
0.04081632653061224
1.0
1.0
0.08163265306122448
1.0
1.0
0.1020408163265306
0.9852941176470589
0.9174311926605505
0.12244897959183673
0.9852941176470589
0.908256880733
0.16326530612244897
0.8823529411764706
0.5688073394495413
0.18367346938775508
0.8823529411764706
0.5688073394495413
0.14285714285714285
0.9852941176470589
0.908256880733
0.061224489795918366
1.0
1.0
0.02040816326530612
1.0
1.0
0.0
1.0
1.0
```
19: AutoML in teradataml

```python
Confusion Matrix :
array([[76, 33],
[11, 57]], dtype=int64)
prediction.head()
id
Prediction
survived
prob_0
prob_1
10
1
1
0.06468306615212427
0.9353169338478757
12
1
1
0.21212896501799006
0.7878710349820099
13
1
0
0.41290357079993467
0.5870964292000653
14
1
0
0.04773688538151222
0.9522631146184878
16
1
1
0.014311090515482183
0.9856889094845178
17
1
1
0.26754463336397494
0.7324553666360251
15
0
1
0.8516859663524107
0.14831403364758936
11
0
0
0.8516859663524107
0.14831403364758936
9
1
0
0.4883748454429272
0.5116251545570728
8
1
0
0.21212896501799006
0.7878710349820099
```
9.
* Generate evaluation metrics on test dataset using best performing model.
```python
performance_metrics = aml.evaluate(titanic_test)
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : XGBOOST_0
Feature Selection Method : lasso
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  Precision    Recall        F1
Support
SeqNum
0               0  CLASS_1       76       11   0.873563  0.697248  0.775510
109
1               1  CLASS_2       33       57   0.633333  0.838235  0.721519
68
SeqNum              Metric  MetricValue
0       3        Micro-Recall     0.751412
1       5     Macro-Precision     0.753448
2       6        Macro-Recall     0.767742
3       7            Macro-F1     0.748515
4       9     Weighted-Recall     0.751412
```
19: AutoML in teradataml

```python
5      10         Weighted-F1     0.754768
6       8  Weighted-Precision     0.781272
7       4            Micro-F1     0.751412
8       2     Micro-Precision     0.751412
9       1            Accuracy     0.751412
performance_metrics
SeqNum
Prediction
Mapping CLASS_1 CLASS_2 Precision
Recall
F1
Support
0
0
CLASS_1 76
11
0.8735632183908046
0.6972477064220184
0.7755102040816326
109
1
1
CLASS_2 33
57
0.6333333333333333
0.8382352941176471
0.7215189873417721
68
```
### Example 4: Run AutoML for Classification Problem using Early
### Stopping Timer and Customization
This example predict whether passenger aboard the RMS Titanic survived or not based on different factors.
**Run AutoML to get the best performing model using following specifications:**
* Add customization for some specific process in AutoML run.
* Use only two models ‘xgboost’ and ‘decision forest’ for AutoML training.
* Set early stopping timer to 300 sec.
* Opt for verbose level 2 to get detailed log.
1.
* Load data and split it to train and test datasets.
* a.
    * Load the example data and create teradataml DataFrame.
```python
load_example_data("teradataml", "titanic")
titanic = DataFrame.from_table("titanic")
```
* b.
    * Perform sampling to get 80% for training and 20% for testing.
```python
titanic_sample = titanic.sample(frac = [0.8, 0.2])
```
* c.
    * Fetch train and test data.
```python
titanic_train= titanic_sample[titanic_sample['sampleid'] ==
1].drop('sampleid', axis=1)
titanic_test = titanic_sample[titanic_sample['sampleid'] ==
2].drop('sampleid', axis=1)
```
19: AutoML in teradataml

2.
* Add customization and generate custom config JSON file.
```python
AutoML.generate_custom_config("custom_titanic")
Generating custom config JSON for AutoML ...
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  1
Customizing Feature Engineering Phase ...
Available options for customization of feature engineering phase with
corresponding indices:
Index 1: Customize Missing Value Handling
Index 2: Customize Bincode Encoding
Index 3: Customize String Manipulation
Index 4: Customize Categorical Encoding
Index 5: Customize Mathematical Transformation
Index 6: Customize Nonlinear Transformation
Index 7: Customize Antiselect Features
```
19: AutoML in teradataml

```python
Index 8: Back to main menu
Index 9: Generate custom json and exit
Enter the list of indices you want to customize in feature engineering phase:
1,2,4,6,7,8
Customizing Missing Value Handling ...
Provide the following details to customize missing value handling:
Available missing value handling methods with corresponding indices:
Index 1: Drop Columns
Index 2: Drop Rows
Index 3: Impute Missing values
Enter the list of indices for missing value handling methods :  1,3
Enter the feature or list of features for dropping columns with missing
values:  cabin
Available missing value imputation methods with corresponding indices:
Index 1: Statistical Imputation
Index 2: Literal Imputation
Enter the list of corresponding index missing value imputation methods you
want to use:  1
Enter the feature or list of features for imputing missing values using
statistic values:  age, embarked
Available statistical methods with corresponding indices:
Index 1: min
Index 2: max
Index 3: mean
Index 4: median
Index 5: mode
Enter the index of corresponding statistic imputation method for feature age:
4
Enter the index of corresponding statistic imputation method for feature
```
19: AutoML in teradataml

```python
embarked:  5
Customization of missing value handling has been completed successfully.
Customizing Bincode Encoding ...
Provide the following details to customize binning and coding encoding:
Available binning methods with corresponding indices:
Index 1: Equal-Width
Index 2: Variable-Width
Enter the feature or list of features for binning:  pclass
Enter the index of corresponding binning method for feature pclass:  2
Enter the number of bins for feature pclass:  2
Available value type of feature for variable binning with corresponding
indices:
Index 1: int
Index 2: float
Provide the range for bin 1 of feature pclass:
Enter the index of corresponding value type of feature pclass:  1
Enter the minimum value for bin 1 of feature pclass:  0
Enter the maximum value for bin 1 of feature pclass:  1
Enter the label for bin 1 of feature pclass:  low
Provide the range for bin 2 of feature pclass:
Enter the index of corresponding value type of feature pclass:  1
Enter the minimum value for bin 2 of feature pclass:  2
Enter the maximum value for bin 2 of feature pclass:  3
Enter the label for bin 2 of feature pclass:  high
Customization of bincode encoding has been completed successfully.
```
19: AutoML in teradataml

```python
Customizing Categorical Encoding ...
Provide the following details to customize categorical encoding:
Available categorical encoding methods with corresponding indices:
Index 1: OneHotEncoding
Index 2: OrdinalEncoding
Index 3: TargetEncoding
Enter the list of corresponding index categorical encoding methods you want to
use:  2,3
Enter the feature or list of features for OrdinalEncoding:  pclass
Enter the feature or list of features for TargetEncoding:  embarked
Available target encoding methods with corresponding indices:
Index 1: CBM_BETA
Index 2: CBM_DIRICHLET
Index 3: CBM_GAUSSIAN_INVERSE_GAMMA
Enter the index of target encoding method for feature embarked:  3
Enter the response column for target encoding method for feature embarked:
survived
Customization of categorical encoding has been completed successfully.
Customizing Nonlinear Transformation ...
Provide the following details to customize nonlinear transformation:
Enter number of non-linear combination you want to make:  1
Provide the details for non-linear combination 1:
Enter the list of target feature/s for non-linear combination 1:  parch,
sibsp
Enter the formula for non-linear combination 1:  Y=(X0+X1+1)
Enter the resultant feature for non-linear combination 1:  Family_count
```
19: AutoML in teradataml

```python
Customization of nonlinear transformation has been completed successfully.
Customizing Antiselect Features ...
Enter the feature or list of features for antiselect:  passenger
Customization of antiselect features has been completed successfully.
Customization of feature engineering phase has been completed successfully.
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  2
Customizing Data Preparation Phase ...
Available options for customization of data preparation phase with
corresponding indices:
Index 1: Customize Train Test Split
Index 2: Customize Data Imbalance Handling
Index 3: Customize Outlier Handling
Index 4: Customize Feature Scaling
Index 5: Back to main menu
Index 6: Generate custom json and exit
```
19: AutoML in teradataml

```python
Enter the list of indices you want to customize in data preparation phase:
1,2,3,4,5
Customizing Train Test Split ...
Enter the train size for train test split:  0.75
Customization of train test split has been completed successfully.
Customizing Data Imbalance Handling ...
Available data sampling methods with corresponding indices:
Index 1: SMOTE
Index 2: NearMiss
Enter the corresponding index data imbalance handling method:  1
Customization of data imbalance handling has been completed successfully.
Customizing Outlier Handling ...
Available outlier detection methods with corresponding indices:
Index 1: percentile
Index 2: tukey
Index 3: carling
Enter the corresponding index oulier handling method:  1
Enter the lower percentile value for outlier handling:  0.1
Enter the upper percentile value for outlier handling:  0.9
Enter the feature or list of features for outlier handling:  fare
Available outlier replacement methods with corresponding indices:
Index 1: delete
Index 2: median
Index 3: Any Numeric Value
Enter the index of corresponding replacement method for feature fare:  2
```
19: AutoML in teradataml

```python
Customization of outlier handling has been completed successfully.
Available feature scaling methods with corresponding indices:
Index 1: maxabs
Index 2: mean
Index 3: midrange
Index 4: range
Index 5: rescale
Index 6: std
Index 7: sum
Index 8: ustd
Enter the corresponding index feature scaling method:  6
Customization of feature scaling has been completed successfully.
Customization of data preparation phase has been completed successfully.
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  3
Customizing Model Training Phase ...
Available options for customization of model training phase with corresponding
indices:
Index 1: Customize Model Hyperparameter
Index 2: Back to main menu
```
19: AutoML in teradataml

```python
Index 3: Generate custom json and exit
Enter the list of indices you want to customize in model training phase:  1,2
Customizing Model Hyperparameter ...
Available models for hyperparameter tuning with corresponding indices:
Index 1: decision_forest
Index 2: xgboost
Index 3: knn
Index 4: glm
Index 5: svm
Available hyperparamters update methods with corresponding indices:
Index 1: ADD
Index 2: REPLACE
Enter the list of model indices for performing hyperparameter tuning:  2
Available hyperparameters for model 'xgboost' with corresponding indices:
Index 1: min_impurity
Index 2: max_depth
Index 3: min_node_size
Index 4: shrinkage_factor
Index 5: iter_num
Enter the list of hyperparameter indices for model 'xgboost':  3
Enter the index of corresponding update method for hyperparameters
'min_node_size' for model 'xgboost':  1
Enter the list of value for hyperparameter 'min_node_size' for model
'xgboost':  1,5
Customization of model hyperparameter has been completed successfully.
Customization of model training phase has been completed successfully.
Available main options for customization with corresponding indices:
```
19: AutoML in teradataml

```python
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  4
Generating custom json and exiting ...
Process of generating custom config file for AutoML has been completed
successfully.
'custom_titanic.json' file is generated successfully under the current working
directory.
```
3.
* Create an AutoML instance.
```python
aml = AutoML(task_type="Classification",
include=['decision_forest','xgboost'],
verbose=2,
max_runtime_secs=300,
custom_config_file='custom_titanic.json')
```
4.
* Fit the data.
```python
aml.fit(titanic_train, titanic_train.survived)
Received below input for customization :
{
"MissingValueHandlingIndicator": true,
"MissingValueHandlingParam": {
"DroppingColumnIndicator": true,
"DroppingColumnList": [
"cabin"
],
"ImputeMissingIndicator": true,
"StatImputeList": [
"age",
"embarked"
```
19: AutoML in teradataml

```python
],
"StatImputeMethod": [
"median",
"mode"
]
},
"BincodeIndicator": true,
"BincodeParam": {
"pclass": {
"Type": "Variable-Width",
"NumOfBins": 2,
"Bin_1": {
"min_value": 0,
"max_value": 1,
"label": "low"
},
"Bin_2": {
"min_value": 2,
"max_value": 3,
"label": "high"
}
}
},
"CategoricalEncodingIndicator": true,
"CategoricalEncodingParam": {
"OrdinalEncodingIndicator": true,
"OrdinalEncodingList": [
"pclass"
],
"TargetEncodingIndicator": true,
"TargetEncodingList": {
"embarked": {
"encoder_method": "CBM_GAUSSIAN_INVERSE_GAMMA",
"response_column": "survived"
}
}
},
"NonLinearTransformationIndicator": true,
"NonLinearTransformationParam": {
"Combination_1": {
"target_columns": [
"parch",
"sibsp"
],
```
19: AutoML in teradataml

```python
"formula": "Y=(X0+X1+1)",
"result_column": "Family_count"
}
},
"AntiselectIndicator": true,
"AntiselectParam": [
"passenger"
],
"TrainTestSplitIndicator": true,
"TrainingSize": 0.75,
"DataImbalanceIndicator": true,
"DataImbalanceMethod": "SMOTE",
"OutlierFilterIndicator": true,
"OutlierFilterMethod": "percentile",
"OutlierLowerPercentile": 0.1,
"OutlierUpperPercentile": 0.9,
"OutlierFilterParam": {
"fare": {
"replacement_value": "median"
}
},
"FeatureScalingIndicator": true,
"FeatureScalingMethod": "std",
"HyperparameterTuningIndicator": true,
"HyperparameterTuningParam": {
"xgboost": {
"min_node_size": {
"Method": "ADD",
"Value": [
1,
5
]
}
}
}
}
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 713
Total Columns in the data: 12
```
19: AutoML in teradataml

```python
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
cabin
VARCHAR(20) CHARACTER SET LATIN 167
546
0
None
None
None
76.57784011220197
23.422159887798035
pclass
INTEGER 713
0
None
0
713
0
0.0
100.0
sex
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
sibsp
INTEGER 713
0
None
483
230
0
0.0
100.0
parch
INTEGER 713
0
None
535
178
0
0.0
100.0
passenger
INTEGER 713
0
None
0
713
0
0.0
100.0
embarked
VARCHAR(20) CHARACTER SET LATIN 712
1
0
None
None
None
0.1402524544179523
99.85974754558205
fare
FLOAT
713
0
None
12
701
0
0.0
100.0
name
VARCHAR(1000) CHARACTER SET LATIN
713
0
0
None
None
None
0.0
100.0
survived
INTEGER 713
0
None
439
274
0
0.0
100.0
age
INTEGER 570
143
None
7
563
0
20.05610098176718
79.94389901823281
ticket
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
Statistics of Data:
func
passenger
survived
pclass
age
sibsp
parch
fare
50%
456
0
3
28
0
0
14.5
count
713
713
713
570
713
713
713
mean
452.764 0.384
2.293
29.335
0.53
0.394
33.635
min
1
0
1
0
0
0
0
max
891
1
3
71
8
5
512.329
75%
673
1
3
38
1
0
31.275
25%
235
0
2
20
0
0
7.925
std
257.37
0.487
0.839
14.481
1.111
0.797
52.824
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
name                      713
sex                       2
ticket                    561
cabin                     130
embarked                  3
```
19: AutoML in teradataml

```python
Futile columns in dataset:
ColumnName
ticket
name
Target Column Distribution:
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0        age          20.757363
1      sibsp           5.189341
2       fare          14.165498
3      parch          24.964937
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.64 sec
Handling less significant features from data ...
Removing Futile columns:
['ticket', 'name']
Sample of Data after removing Futile columns:
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
80
1
3
female
30
0
0
12.475
None
S
12
183
0
3
male
9
4
2
31.3875 None
S
8
509
0
3
male
28
0
0
22.525
None
S
16
305
0
3
male
None
0
0
8.05
None
S
13
835
0
3
male
18
0
0
8.3
None
S
```
19: AutoML in teradataml

```python
15
162
1
2
female
40
0
0
15.75
None
S
23
265
0
3
female
None
0
0
7.75
None
Q
9
530
0
2
male
23
2
1
11.5
None
S
17
61
0
3
male
22
0
0
7.2292
None
C
14
652
1
2
female
18
0
1
23.0
None
S
22
713 rows X 11 columns
Total time to handle less significant features: 20.57 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
Dropping these columns for handling customized missing value:
['cabin']
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713845516787654"'20
Updated dataset sample after performing customized missing value imputation:
passenger
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
427
1
2
female
28
1
0
26.0
S
31
671
1
2
female
40
1
1
39.0
S
47
528
0
1
male
28
0
0
221.7792
S
55
793
0
3
female
28
8
2
69.55
S
63
833
0
3
male
28
0
0
7.2292
C
79
282
0
3
male
28
0
0
7.8542
S
87
589
0
3
male
22
0
0
8.05
S
71
692
1
3
female
4
0
1
13.4167 C
39
162
1
2
female
40
0
0
15.75
S
23
835
0
3
male
18
0
0
8.3
S
15
713 rows X 10 columns
Proceeding with default option for handling remaining missing
```
19: AutoML in teradataml

```python
values.
Checking Missing values in dataset ...
Analysis Completed. No Missing Values
Detected.
Total time to find missing values in data: 6.47 sec
Imputing Missing Values ...
Analysis completed. No imputation
required.
Time taken to perform imputation: 0.01 sec
No information provided for Equal-Width
Transformation.
Variable-Width binning information:-
ColumnName
MinValue
MaxValue
Label
0
pclass
0
1
low
1
pclass
2
3
high
2 rows X 4 columns
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713845626085490"'20
Updated dataset sample after performing Variable-Width binning:
sibsp
parch
fare
age
id
survived
passenger
sex
embarked
pclass
3
0
15.85
33
691
1
86
female
S
high
3
1
25.4667 28
92
0
177
male
S
high
3
1
21.075
3
220
0
375
female
S
high
3
1
25.4667 28
700
0
410
female
S
high
3
2
27.9
10
646
0
820
male
S
high
3
2
27.9
9
718
0
635
female
S
high
0
0
7.25
30
32
0
366
male
S
high
0
0
8.05
16
56
1
221
male
S
high
0
0
24.15
39
80
0
812
male
S
high
0
0
8.05
43
88
0
669
male
S
high
713 rows X 10 columns
Skipping customized string manipulation.⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾
```
* ｜  25% - 5/20
```python
Starting Customized Categorical Feature Encoding ...
```
19: AutoML in teradataml

```python
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713845980398494"'20
Updated dataset sample after performing ordinal encoding:
sibsp
parch
fare
age
id
survived
passenger
sex
embarked
pclass
4
1
29.125
2
49
0
17
male
Q
0
4
1
29.125
4
558
0
172
male
Q
0
4
2
31.275
9
539
0
542
female
S
0
4
2
31.275
6
40
0
814
female
S
0
4
1
39.6875 14
429
0
687
male
S
0
4
1
39.6875 1
302
0
165
male
S
0
4
1
29.125
8
445
0
788
male
Q
0
4
2
31.3875 5
180
1
234
female
S
0
4
2
7.925
17
627
1
69
female
S
0
4
1
39.6875 16
735
0
267
male
S
0
713 rows X 10 columns
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713844678774235"'20
Updated dataset sample after performing target encoding:
embarked
sibsp
parch
fare
age
id
survived
passenger
sex
pclass
0.36363636363636365
1
0
15.5
28
398
0
365
male
0
0.36363636363636365
1
0
24.15
28
178
1
110
female
0
0.36363636363636365
1
0
24.15
28
457
0
769
male
0
0.36363636363636365
1
1
15.5
40
585
0
189
male
0
0.36363636363636365
2
0
23.25
28
420
1
302
male
0
0.36363636363636365
2
0
90.0
44
742
0
246
male
1
0.3300395256916996
1
1
29.0
22
67
1
324
female
0
0.3300395256916996
1
5
31.3875 38
280
1
26
female
0
0.3300395256916996
1
2
65.0
48
774
1
755
female
0
0.3300395256916996
0
0
7.8958
22
160
0
522
male
0
```
19: AutoML in teradataml

```python
713 rows X 10 columns
Performing encoding for categorical columns ...
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713845375023505"'20
ONE HOT Encoding these Columns:
['sex']
Sample of dataset after performing one hot encoding:
embarked
sibsp
parch
fare
age
id
survived
passenger
sex_0
sex_1
pclass
0.36363636363636365
1
0
24.15
28
457
0
769
0
1
0
0.36363636363636365
2
0
23.25
28
420
1
302
0
1
0
0.36363636363636365
2
0
90.0
44
742
0
246
0
1
1
0.36363636363636365
0
0
7.7375
28
304
0
779
0
1
0
0.36363636363636365
0
0
7.75
28
42
0
791
0
1
0
0.36363636363636365
0
0
7.75
32
466
0
891
0
1
0
0.3300395256916996
1
1
11.1333 1
197
1
173
1
0
0
0.3300395256916996
0
0
6.2375
61
298
0
327
0
1
0
0.3300395256916996
1
2
65.0
24
662
1
616
1
0
0
0.3300395256916996
0
0
14.0
54
139
0
318
0
1
0
713 rows X 11 columns
Time taken to encode the columns: 13.91 sec
Starting customized mathematical transformation ...
Skipping customized mathematical
transformation.
Starting customized non-linear transformation ...
```
19: AutoML in teradataml

```python
Possible combination :
['Combination_1']
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713844655147652"'20
Updated dataset sample after performing non-liner transformation:
embarked
sibsp
parch
fare
age
id
survived
passenger
sex_0
sex_1
pclass
Family_count
0.3300395256916996
0.0
0.0
0.0
28
106
0
278
0
1
0
1.0
0.3300395256916996
0.0
0.0
50.4958 31
518
0
868
0
1
1
1.0
0.3300395256916996
1.0
1.0
20.525
33
41
0
549
0
1
0
3.0
0.3300395256916996
2.0
0.0
133.65
50
203
1
661
0
1
1
3.0
0.3300395256916996
1.0
1.0
39.0
60
502
0
685
0
1
0
3.0
0.3300395256916996
1.0
0.0
26.0
25
444
0
729
0
1
0
2.0
0.3300395256916996
0.0
0.0
14.5
28
592
0
761
0
1
0
1.0
0.3300395256916996
1.0
0.0
17.8
18
509
0
50
1
0
0
2.0
0.3300395256916996
0.0
0.0
8.05
28
544
0
416
1
0
0
1.0
0.3300395256916996
0.0
0.0
8.05
22
71
0
589
0
1
0
1.0
713 rows X 12 columns
Starting customized anti-select columns ...
Updated dataset sample after performing anti-select columns:
embarked
sibsp
parch
fare
age
id
survived
sex_0
sex_1
pclass
Family_count
0.36363636363636365
2.0
0.0
90.0
44
742
0
0
1
1
3.0
0.36363636363636365
0.0
0.0
7.75
28
42
0
0
1
0
1.0
0.36363636363636365
0.0
0.0
7.75
32
466
0
0
1
0
1.0
0.36363636363636365
0.0
0.0
7.75
28
530
1
1
0
0
1.0
```
19: AutoML in teradataml

```python
0.36363636363636365
0.0
0.0
7.75
30
131
0
1
0
0
1.0
0.36363636363636365
0.0
0.0
7.8792
19
283
1
1
0
0
1.0
0.3300395256916996
0.0
0.0
8.05
22
71
0
0
1
0
1.0
0.3300395256916996
0.0
0.0
8.05
28
544
0
1
0
0
1.0
0.3300395256916996
0.0
0.0
0.0
28
106
0
0
1
0
1.0
0.3300395256916996
1.0
0.0
17.8
18
509
0
1
0
0
2.0
713 rows X 11 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
Spliting of dataset into training and testing ...
Training size :
0.75
Testing size  :
0.25
Training data sample
embarked
sibsp
parch
fare
age
id
survived
sex_0
sex_1
pclass
Family_count
0.3300395256916996
0.0
0.0
8.05
28
13
0
0
1
0
1.0
0.3300395256916996
0.0
0.0
22.525
28
16
0
0
1
0
1.0
0.3300395256916996
2.0
1.0
11.5
23
17
0
0
1
0
4.0
0.3300395256916996
4.0
2.0
31.275
2
18
0
1
0
0
7.0
0.3300395256916996
0.0
0.0
13.0
36
20
0
0
1
0
1.0
0.5763888888888888
0.0
0.0
18.7875 11
21
0
0
1
0
1.0
0.36363636363636365
0.0
0.0
7.75
28
9
0
1
0
0
1.0
```
19: AutoML in teradataml

```python
0.36363636363636365
4.0
1.0
29.125
2
49
0
0
1
0
6.0
0.36363636363636365
0.0
0.0
7.75
28
73
0
0
1
0
1.0
0.36363636363636365
0.0
0.0
7.75
40
96
0
0
1
0
1.0
534 rows X 11 columns
Testing data sample
embarked
sibsp
parch
fare
age
id
survived
sex_0
sex_1
pclass
Family_count
0.5763888888888888
0.0
0.0
7.2292
22
14
0
0
1
0
1.0
0.3300395256916996
0.0
0.0
7.25
30
32
0
0
1
0
1.0
0.3300395256916996
0.0
0.0
13.0
24
34
0
1
0
0
1.0
0.3300395256916996
0.0
0.0
26.55
34
35
1
0
1
1
1.0
0.3300395256916996
0.0
0.0
30.5
27
57
1
0
1
1
1.0
0.5763888888888888
1.0
0.0
24.0
28
62
1
1
0
0
2.0
0.36363636363636365
1.0
0.0
15.5
28
28
1
1
0
0
2.0
0.36363636363636365
0.0
0.0
7.75
28
42
0
0
1
0
1.0
0.36363636363636365
0.0
0.0
7.75
28
81
1
1
0
0
1.0
0.36363636363636365
0.0
5.0
29.125
39
196
0
1
0
0
6.0
179 rows X 11 columns
Time taken for spliting of data: 11.91 sec
Starting customized outlier processing ...
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0            id           9.817672
1           age           9.256662
2          fare           9.116410
```
19: AutoML in teradataml

```python
3  Family_count           2.805049
4         parch           1.683029
5         sibsp           3.225806
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713845708648720"'
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713853330820560"'/20
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713846364308763"'
Checking imbalance data ...
Imbalance Not Found.
Feature selection using lasso ...
feature selected by lasso:
['sibsp', 'sex_1', 'fare', 'sex_0', 'Family_count', 'age', 'pclass']
Total time taken by feature selection: 2.80 sec
scaling Features of lasso data ...
columns that will be scaled:
['sibsp', 'fare', 'Family_count', 'age', 'pclass']
Training dataset sample after scaling:
id
survived
sex_1
sex_0
sibsp
fare
Family_count
age
pclass
163
0
1
0
-0.4733327294262123
-0.7081553190636275
-0.5661773649877848
-0.09018879949184125
-0.5759086932341287
27
0
1
0
3.6567273606652484
0.91382440495327
3.5194809174916357
-2.2466433783863144
-0.5759086932341287
53
1
0
1
-0.4733327294262123
-0.08005100856639845
0.01748810393784668
0.38902332915137505
-0.5759086932341287
210
0
1
0
-0.4733327294262123
1.1201887036809417
-0.5661773649877848
1.9863970912954294
1.7363863608036514
461
0
1
0
-0.4733327294262123
0.33579644478911136
0.01748810393784668
-0.09018879949184125
-0.5759086932341287
43
0
0
1
0.3526792885920798
-0.28797473524415335
0.01748810393784668
0.1494172648297669
-0.5759086932341287
63
0
0
1
6.134763414720124
1.8557188868034997
```
19: AutoML in teradataml

```python
5.270477324268531
-0.09018879949184125
-0.5759086932341287
25
0
0
1
-0.4733327294262123
-0.7081553190636275
-0.5661773649877848
-0.09018879949184125
-0.5759086932341287
60
1
0
1
-0.4733327294262123
2.560580320241089
-0.5661773649877848
-1.0486130567782739
1.7363863608036514
17
0
1
0
1.1786913066103721
-0.5582755799252347
1.1848190417891098
-0.48953224002785484
-0.5759086932341287
534 rows X 9 columns
Testing dataset sample after scaling:
id
survived
sex_1
sex_0
sibsp
fare
Family_count
age
pclass
57
1
1
0
-0.4733327294262123
0.23183458145023392
-0.5661773649877848
-0.17005748759904396
1.7363863608036514
71
0
1
0
-0.4733327294262123
-0.7017429513328856
-0.5661773649877848
-0.5694009281350575
-0.5759086932341287
67
1
0
1
0.3526792885920798
0.16945746344690746
0.6011535728634781
-0.5694009281350575
-0.5759086932341287
79
0
1
0
-0.4733327294262123
-0.7358757103043059
-0.5661773649877848
-0.09018879949184125
-0.5759086932341287
82
0
1
0
0.3526792885920798
1.171649826033686
0.01748810393784668
-0.8090069924566656
1.7363863608036514
126
0
0
1
-0.4733327294262123
-0.713178756300162
-0.5661773649877848
-0.8888756805638683
-0.5759086932341287
99
1
0
1
-0.4733327294262123
7.751915966067934
0.01748810393784668
-1.1284817448854765
1.7363863608036514
62
1
0
1
0.3526792885920798
-0.03846626323084746
0.01748810393784668
-0.09018879949184125
-0.5759086932341287
34
0
0
1
-0.4733327294262123
-0.4958984619219083
-0.5661773649877848
-0.4096635519206521
-0.5759086932341287
32
0
1
0
-0.4733327294262123
-0.7350107476013265
-0.5661773649877848
0.06954857672256418
-0.5759086932341287
179 rows X 9 columns
```
19: AutoML in teradataml

```python
Total time taken by feature scaling: 44.70 sec
Feature selection using rfe ...
feature selected by RFE:
['sex_1', 'sex_0', 'age', 'pclass', 'fare', 'Family_count']
Total time taken by feature selection: 22.47 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_age', 'r_pclass', 'r_fare', 'r_Family_count']
Training dataset sample after scaling:
r_sex_1 id
survived
r_sex_0 r_age
r_pclass
r_fare
r_Family_count
1
163
0
0
-0.09018879949184125
-0.5759086932341287
-0.7081553190636275
-0.5661773649877848
1
27
0
0
-2.2466433783863144
-0.5759086932341287
0.91382440495327
3.5194809174916357
0
53
1
1
0.38902332915137505
-0.5759086932341287
-0.08005100856639845
0.01748810393784668
1
210
0
0
1.9863
912954294
1.7363863608036514
1.1201887036809417
-0.5661773649877848
1
461
0
0
-0.09018879949184125
-0.5759086932341287
0.33579644478911136
0.01748810393784668
0
43
0
1
0.1494172648297669
-0.5759086932341287
-0.28797473524415335
0.01748810393784668
0
63
0
1
-0.09018879949184125
-0.5759086932341287
1.8557188868034997
5.270477324268531
0
25
0
1
-0.09018879949184125
-0.5759086932341287
-0.7081553190636275
-0.5661773649877848
0
60
1
1
-1.0486130567782739
1.7363863608036514
2.560580320241089
-0.5661773649877848
1
17
0
0
-0.48953224002785484
-0.5759086932341287
-0.5582755799252347
1.1848190417891098
534 rows X 8 columns
```
19: AutoML in teradataml

```python
Testing dataset sample after scaling:
r_sex_1 id
survived
r_sex_0 r_age
r_pclass
r_fare
r_Family_count
1
57
1
0
-0.17005748759904396
1.7363863608036514
0.23183458145023392
-0.5661773649877848
1
71
0
0
-0.5694009281350575
-0.5759086932341287
-0.7017429513328856
-0.5661773649877848
0
67
1
1
-0.5694009281350575
-0.5759086932341287
0.16945746344690746
0.6011535728634781
1
79
0
0
-0.09018879949184125
-0.5759086932341287
-0.7358757103043059
-0.5661773649877848
1
82
0
0
-0.8090069924566656
1.7363863608036514
1.171649826033686
0.01748810393784668
0
126
0
1
-0.8888756805638683
-0.5759086932341287
-0.713178756300162
-0.5661773649877848
0
99
1
1
-1.1284817448854765
1.7363863608036514
7.751915966067934
0.01748810393784668
0
62
1
1
-0.09018879949184125
-0.5759086932341287
-0.03846626323084746
0.01748810393784668
0
34
0
1
-0.4096635519206521
-0.5759086932341287
-0.4958984619219083
-0.5661773649877848
1
32
0
0
0.06954857672256418
-0.5759086932341287
-0.7350107476013265
-0.56
61773649877848
179 rows X 8 columns
Total time taken by feature scaling: 42.68 sec
scaling Features of pca data ...
columns that will be scaled:
['embarked', 'sibsp', 'parch', 'fare', 'age', 'pclass', 'Family_count']
Training dataset sample after scaling:
id
survived
sex_1
sex_0
embarked
sibsp
parch
fare
age
pclass
Family_count
17
0
1
0
-0.5232459472817322
1.1786913066103721
```
19: AutoML in teradataml

```python
0.7672497196248651
-0.5582755799252346
-0.4895322400278547
-0.57
59086932341301
1.1848190417891102
31
1
0
1
-0.5232459472817322
0.3526792885920798
-0.50514577813811
0.04470322744025449
-0.09018879949184123
-0.5759086932341301
0.017488103937846687
60
1
0
1
-0.5232459472817322
-0.4733327294262123
-0.50514577813811
2.560580320241088
-1.0486130567782737
1.7363863608036554
-0.566177364987785
65
0
1
0
-0.5232459472817322
0.3526792885920798
-0.50514577813811
-0.37738193771558787
-0.09
018879949184123 -0.5759086932341301
0.017488103937846687
25
0
0
1
-0.5232459472817322
-0.4733327294262123
-0.50514577813811
-0.7081553190636273
-0.09018879949184123
-0.5759086932341301
-0.56
6177364987785
163
0
1
0
-0.5232459472817322
-0.4733327294262123
-0.50514577813811
-0.7081553190636273
-0.09018879949184123
-0.5759086932341301
-0.56
6177364987785
73
0
1
0
-0.17262793722408595
-0.4733327294262123
-0.50514577813811
-0.7142183749335507
-0.09018879949184123
-0.5759086932341301
-0.56
6177364987785
97
0
1
0
-0.17262793722408595
-0.4733327294262123
-0.50514577813811
-0.5229285463900163
2.226003155617037
-0.5759086932341301
-0.566177364987785
131
0
0
1
-0.17262793722408595
-0.4733327294262123
-0.50514577813811
-0.7142183749335507
0.06954857672256418
-0.5759086932341301
-0.566177364987785
133
1
0
1
-0.17262793722408595
-0.4733327294262123
-0.50514577813811
-0.7088456258361975
-0.09018879949184123
-0.5759086932341301
-0.56
6177364987785
534 rows X 11 columns
Testing dataset sample after scaling:
id
survived
sex_1
sex_0
embarked
sibsp
parch
fare
age
pclass
Family_count
14
0
1
0
```
19: AutoML in teradataml

```python
2.0476663405184086
-0.4733327294262123
-0.50514577813811
-0.73
58757103043057
-0.5694009281350575
-0.5759086932341301
-0.5661773649
87785
32
0
1
0
-0.5232459472817322
-0.4733327294262123
-0.50514577813811
-0.7350107476013262
0.06954857672256418
-0.5759086932341301
-0.566177364987785
34
0
0
1
-0.5232459472817322
-0.4733327294262123
-0.50514577813811
-0.4958984619219081
-0.40966355192065207
-0.5759086932341301
-0.56
6177364987785
35
1
1
0
-0.5232459472817322
-0.4733327294262123
-0.50514577813811
0.06757483737480756
0.389023329151375
1.7363863608036554
-0.566177364987785
57
1
1
0
-0.5232459472817322
-0.4733327294262123
-0.50514577813811
0.23183458145023386
-0.17005748759904393
1.7363863608036554
-0.566177364987785
62
1
0
1
2.0476663405184086
0.3526792885920798
-0.50514577813811
-0.03846626323084745
-0.09
018879949184123 -0.5759086932341301
0.017488103937846687
28
1
0
1
-0.17262793722408595
0.3526792885920798
-0.50514577813811
-0.3919365985830307
-0.09
018879949184123 -0.5759086932341301
0.017488103937846687
42
0
1
0
-0.17262793722408595
-0.4733327294262123
-0.50514577813811
-0.7142183749335507
-0.09018879949184123
-0.5759086932341301
-0.56
6177364987785
81
1
0
1
-0.17262793722408595
-0.4733327294262123
-0.50514577813811
-0.7142183749335507
-0.09018879949184123
-0.5759086932341301
-0.56
6177364987785
196
0
0
1
-0.17262793722408595
-0.4733327294262123
5.856831710676766
0.17465555661385126
0.7883667696873885
-0.5759086932341301
2.3521499796403735
179 rows X 11 columns
Total time taken by feature scaling: 42.52 sec
Dimension Reduction using pca ...
PCA columns:
```
19: AutoML in teradataml

```python
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5']
Total time taken by PCA: 11.87 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Starting customized hyperparameter update ...
Completed customized hyperparameter update.
Hyperparameters used for model training:
response_column :
survived
name : decision_forest
tree_type : Classification
min_impurity : (0.0, 0.1, 0.2)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
num_trees : (-1, 20, 30)
Total number of models for decision_forest : 108
response_column : survived
name : xgboost
model_type : Classification
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1, 0.2)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.1, 0.3)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3, 5)
iter_num : (10, 20, 30)
Total number of models for xgboost : 3456
```
19: AutoML in teradataml

```python
Performing hyperParameter tuning ...
decision_forest
xgboost
Evaluating models performance ...
Evaluation completed.
Leaderboard
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
XGBOOST_2
pca
0.793296
0.793296
0.793296
0.793296
0.781918
0.788603
0.784583
0.796967
0.793296
0.794506
1
2
DECISIONFOREST_3
lasso
0.793296
0.793296
0.793296
0.793296
0.785015
0.772398
0.777281
0.791188
0.793296
0.790961
2
3
XGBOOST_0
lasso
0.782123
0.782123
0.782123
0.782123
0.774160
0.757905
0.763684
0.779693
0.782123
0.778804
3
4
XGBOOST_3
lasso
0.782123
0.782123
0.782123
0.782123
0.774160
0.757905
0.763684
0.779693
0.782123
0.778804
4
5
DECISIONFOREST_0
lasso
0.770950
0.770950
0.770950
0.770950
0.763252
0.743412
0.749838
0.768262
0.770950
0.766484
5
6
DECISIONFOREST_2
pca
0.765363
0.765363
0.765363
0.765363
0.752372
0.752372
0.752372
0.765363
0.765363
0.765363
6
7
XGBOOST_1
rfe
0.664804
0.664804
0.664804
0.664804
0.646978
0.605731
0.602222
0.654215
0.664804
0.638361
7
8
DECISIONFOREST_1
rfe
0.664804
0.664804
0.664804
0.664804
0.653798
0.597628
0.588695
0.657792
0.664804
0.629221
```
19: AutoML in teradataml

```python
8 rows X 13 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 20/20
```
5.
* Display model leaderboard.
```python
aml.leaderboard()
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
XGBOOST_2
pca
0.793296
0.793296
0.793296
0.793296
0.781918
0.788603
0.784583
0.796967
0.793296
0.794506
1
2
DECISIONFOREST_3
lasso
0.793296
0.793296
0.793296
0.793296
0.785015
0.772398
0.777281
0.791188
0.793296
0.790961
2
3
XGBOOST_0
lasso
0.782123
0.782123
0.782123
0.782123
0.774160
0.757905
0.763684
0.779693
0.782123
0.778804
3
4
XGBOOST_3
lasso
0.782123
0.782123
0.782123
0.782123
0.774160
0.757905
0.763684
0.779693
0.782123
0.778804
4
5
DECISIONFOREST_0
lasso
0.770950
0.770950
0.770950
0.770950
0.763252
0.743412
0.749838
0.768262
0.770950
0.766484
5
6
DECISIONFOREST_2
pca
0.765363
0.765363
0.765363
0.765363
0.752372
0.752372
0.752372
0.765363
0.765363
0.765363
6
7
XGBOOST_1
rfe
0.664804
0.664804
0.664804
0.664804
0.646978
0.605731
0.602222
0.654215
0.664804
0.638361
7
8
DECISIONFOREST_1
rfe
0.664804
0.664804
0.664804
0.664804
0.653798
0.5
28
0.588695
0.657792
0.664804
0.629221
```
6.
* Display the best performing model.
```python
aml.leader()
```
19: AutoML in teradataml

```python
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
XGBOOST_2
pca
0.793296
0.793296
0.793296
0.793296
0.781918
0.788603
0.784583
0.796967
0.793296
0.794506
```
7.
* Generate prediction on validation dataset using best performing model.
* Note:
* In the data preparation phase, AutoML generates the validation dataset by splitting the data
* provided during fitting into training and testing sets. AutoML's model training utilizes the training
* data, with the testing data acting as the validation dataset for model evaluation.
```python
prediction = aml.predict()
Following model is being used for generating prediction :
Model ID : XGBOOST_2
Feature Selection Method : pca
Prediction :
survived   id  Prediction  Confidence_Lower  Confidence_upper
0         0   42           0             1.000             1.000
1         1   81           1             1.000             1.000
2         0   14           0             0.875             0.875
3         0  196           0             0.500             0.500
4         0  337           1             0.875             0.875
5         0   32           0             0.875             0.875
6         1   23           1             0.750             0.750
7         0   11           0             0.750             0.750
8         1   10           1             0.750             0.750
9         1   28           1             0.625             0.625
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  Precision    Recall        F1
Support
SeqNum
0               0  CLASS_1       89       16   0.847619  0.809091  0.827907
110
1               1  CLASS_2       21       53   0.716216  0.768116  0.741259
69
ROC-AUC :
```
19: AutoML in teradataml

```python
AUC
GINI
0.7152832674571804
0.4305665349143608
threshold_value tpr
fpr
0.04081632653061224
0.7681159420289855
0.19090909090909092
0.08163265306122448
0.7681159420289855
0.19090909090909092
0.1020408163265306
0.7681159420289855
0.19090909090909092
0.12244897959183673
0.7681159420289855
0.19090909090909092
0.16326530612244897
0.7681159420289855
0.19090909090909092
0.18367346938775508
0.7681159420289855
0.19090909090909092
0.14285714285714285
0.7681159420289855
0.19090909090909092
0.061224489795918366
0.7681159420289855
0.19090909090909092
0.02040816326530612
0.7681159420289855
0.19090909090909092
0.0
1.0
1.0
Confusion Matrix :
array([[89, 21],
[16, 53]], dtype=int64)
prediction.head()
survived
id
Prediction
Confidence_Lower
Confidence_upper
0
32
0
0.875
0.875
0
380
0
1.0
1.0
0
413
0
0.75
0.75
0
556
0
1.0
1.0
0
731
0
0.875
0.875
0
79
0
1.0
1.0
0
71
0
0.875
0.875
0
355
0
0.75
0.75
0
337
1
0.875
0.875
0
14
0
0.875
0.875
```
8.
* Generate prediction on test dataset using best performing model.
```python
prediction = aml.predict(titanic_test)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping futile columns :
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
469
0
3
male
None
0
0
7.725
None
Q
```
19: AutoML in teradataml

```python
8
570
1
3
male
32
0
0
7.8542
None
S
15
223
0
3
male
51
0
0
8.05
None
S
23
856
1
3
female
18
0
1
9.35
None
S
11
326
1
1
female
36
0
0
135.6333
C32
C
13
650
1
3
female
23
0
0
7.55
None
S
21
734
0
2
male
23
0
0
13.0
None
S
14
795
0
3
male
25
0
0
7.8958
None
S
22
631
1
1
male
80
0
0
30.0
A23
S
10
57
1
2
female
21
0
0
10.5
None
S
18
Updated dataset after performing target column transformation :
sibsp
cabin
parch
fare
age
id
passenger
sex
pclass
embarked
survived
0
None
1
9.35
18
11
856
female
3
S
1
0
B51 B53 B55
0
5.0
33
9
873
male
1
S
0
0
C106
0
30.5
None
17
299
male
1
S
1
0
None
0
8.05
21
12
38
male
3
S
0
0
A23
0
30.0
80
10
631
male
1
S
1
0
None
0
10.5
21
18
57
female
2
S
1
0
C32
0
135.6333
36
13
326
female
1
C
1
0
None
0
7.55
23
21
650
female
3
S
1
0
None
0
13.0
23
14
734
male
2
S
0
0
None
0
7.8958
25
22
795
male
3
S
0
```
19: AutoML in teradataml

```python
Updated dataset after dropping customized missing value containing columns :
sibsp
parch
fare
age
id
passenger
sex
pclass
embarked
survived
0
1
9.35
18
11
856
female
3
S
1
0
0
30.0
80
10
631
male
1
S
1
0
0
10.5
21
18
57
female
2
S
1
0
0
13.0
23
14
734
male
2
S
0
0
0
135.6333
36
13
326
female
1
C
1
0
0
7.55
23
21
650
female
3
S
1
0
0
8.05
21
12
38
male
3
S
0
0
0
7.8542
48
20
772
male
3
S
0
0
0
7.8542
32
15
570
male
3
S
1
0
0
8.05
51
23
223
male
3
S
0
Updated dataset after imputing customized missing value containing columns :
sibsp
parch
fare
age
id
passenger
sex
pclass
embarked
survived
4
2
31.275
4
116
851
male
3
S
0
4
1
39.6875 7
63
51
male
3
S
0
4
1
39.6875 2
62
825
male
3
S
0
4
2
31.275
11
42
543
female
3
S
0
0
0
8.05
44
49
697
male
3
S
0
0
0
7.75
22
57
142
female
3
S
1
0
0
7.75
65
65
281
male
3
Q
0
0
0
7.8542
21
105
624
male
3
S
0
0
0
8.05
55
113
153
male
3
S
0
0
5
39.6875 41
121
639
female
3
S
0
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713846972810707"'
Updated dataset after performing customized variable width bin-code
transformation :
sibsp
parch
fare
age
id
survived
passenger
sex
embarked
pclass
0
0
8.05
55
113
0
153
male
S
high
0
0
26.2875 36
153
1
513
male
S
low
0
0
7.2292
28
161
1
368
female
C
high
0
0
13.0
23
14
0
734
male
S
high
0
0
7.8958
24
54
0
295
male
S
high
0
0
0.0
49
70
0
598
male
S
high
4
1
39.6875 7
63
0
51
male
S
high
4
2
31.275
11
42
0
543
female
S
high
4
1
39.6875 2
62
0
825
male
S
high
```
19: AutoML in teradataml

```python
4
1
29.125
7
143
0
279
male
Q
high
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713846171306458"'
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713850847022753"'
Updated dataset after performing customized categorical encoding :
embarked
sibsp
parch
fare
age
id
survived
passenger
sex
pclass
0.36363636363636365
0
0
7.6292
28
47
0
503
female
0
0.36363636363636365
1
0
90.0
33
48
1
413
female
1
0.36363636363636365
1
0
15.5
28
154
1
187
female
0
0.36363636363636365
2
0
23.25
28
84
1
331
female
0
0.36363636363636365
0
0
7.75
65
65
0
281
male
0
0.36363636363636365
0
0
7.725
28
8
0
469
male
0
0.3300395256916996
0
0
7.8542
20
158
0
641
male
0
0.3300395256916996
0
0
7.925
39
126
0
529
male
0
0.3300395256916996
0
0
8.05
28
117
0
88
male
0
0.3300395256916996
0
0
0.0
28
190
0
675
male
0
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713846624114222"'
Updated dataset after performing categorical encoding :
embarked
sibsp
parch
fare
age
id
survived
passenger
sex_0
sex_1
pclass
0.3300395256916996
0
0
56.4958 26
61
1
510
0
1
0
0.3300395256916996
0
0
7.8958
24
54
0
295
0
1
0
0.3300395256916996
0
0
10.5
21
18
1
57
1
0
0
0.3300395256916996
0
0
10.1708 19
34
0
688
0
1
0
0.3300395256916996
0
0
7.8
21
122
0
52
```
19: AutoML in teradataml

```python
0
1
0
0.3300395256916996
0
0
7.4958
36
120
0
664
0
1
0
0.36363636363636365
0
0
7.6292
28
47
0
503
1
0
0
0.36363636363636365
1
0
90.0
33
48
1
413
1
0
1
0.36363636363636365
1
0
15.5
28
154
1
187
1
0
0
0.36363636363636365
2
0
23.25
28
84
1
331
1
0
0
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713846909702579"'
Updated dataset after performing customized non-linear transformation :
embarked
sibsp
parch
fare
age
id
survived
passenger
sex_0
sex_1
pclass
Family_count
0.3300395256916996
0.0
0.0
10.5
21
18
1
57
1
0
0
1.0
0.3300395256916996
0.0
0.0
8.05
45
60
1
339
0
1
0
1.0
0.3300395256916996
0.0
0.0
7.4958
36
120
0
664
0
1
0
1.0
0.3300395256916996
0.0
1.0
9.35
18
11
1
856
1
0
0
2.0
0.3300395256916996
0.0
0.0
26.2875 35
144
1
702
0
1
1
1.0
0.3300395256916996
0.0
0.0
10.5
26
138
0
620
0
1
0
1.0
0.36363636363636365
1.0
0.0
15.5
28
154
1
187
1
0
0
2.0
0.36363636363636365
0.0
0.0
7.75
65
65
0
281
0
1
0
1.0
0.36363636363636365
0.0
0.0
7.725
28
8
0
469
0
1
0
1.0
0.36363636363636365
0.0
0.0
7.75
28
90
0
127
0
1
0
1.0
Updated dataset after performing customized anti-selection :
embarked
sibsp
parch
fare
age
id
survived
sex_0
sex_1
pclass
Family_count
0.3300395256916996
0.0
0.0
10.5
21
18
1
1
0
0
1.0
0.3300395256916996
0.0
0.0
8.05
45
60
1
0
```
19: AutoML in teradataml

```python
1
0
1.0
0.3300395256916996
0.0
0.0
7.4958
36
120
0
0
1
0
1.0
0.3300395256916996
0.0
1.0
9.35
18
11
1
1
0
0
2.0
0.3300395256916996
0.0
0.0
26.2875 35
144
1
0
1
1
1.0
0.3300395256916996
0.0
0.0
10.5
26
138
0
0
1
0
1.0
0.36363636363636365
1.0
0.0
15.5
28
154
1
1
0
0
2.0
0.36363636363636365
0.0
0.0
7.75
65
65
0
0
1
0
1.0
0.36363636363636365
0.0
0.0
7.725
28
8
0
0
1
0
1.0
0.36363636363636365
0.0
0.0
7.75
28
90
0
0
1
0
1.0
Performing transformation carried out in data preparation phase ...
result data stored in table
'"AUTOML_USR"."ml__td_sqlmr_persist_out__1713849399082665"'
Updated dataset after performing Lasso feature selection:
id
sibsp
sex_1
fare
sex_0
Family_count
age
pclass
survived
190
0.0
1
0.0
0
1.0
28
0
0
28
1.0
1
20.2125 0
3.0
18
0
0
26
0.0
1
8.05
0
1.0
30
0
0
79
0.0
1
7.775
0
1.0
28
0
1
98
0.0
1
13.0
0
1.0
34
0
1
114
0.0
1
26.2875 0
1.0
42
1
1
142
0.0
0
7.75
1
1.0
45
0
0
16
0.0
0
7.55
1
1.0
28
0
0
134
1.0
0
26.0
1
2.0
29
0
1
110
0.0
0
55.0
1
2.0
28
1
1
Updated dataset after performing scaling on Lasso selected features :
id
survived
sex_1
sex_0
sibsp
fare
Family_count
age
pclass
190
0
1
0
-0.4733327294262123
-1.036500151284071
-0.5661773649877848
-0.09018879949184125
-0.5759086932341287
28
0
1
0
0.3526792885920798
-0.19596848618924687
0.6011535728634781
-0.8888756805638683
-0.5759086932341287
26
0
1
```
19: AutoML in teradataml

```python
0
-0.4733327294262123
-0.7017429513328856
-0.5661773649877848
0.06954857672256418
-0.5759086932341287
79
1
1
0
-0.4733327294262123
-0.713178756300162
-0.5661773649877848
-0.09018879949184125
-0.5759086932341287
98
1
1
0
-0.4733327294262123
-0.4958
619219083
-0.5661773649877848
0.38902332915137505
-0.5759086932341287
114
1
1
0
-0.4733327294262123
0.056658841724225466
-0.5661773649877848
1.0279728340089966
1.7363863608036514
142
0
0
1
-0.4733327294262123
-0.7142183749335509
-0.5661773649877848
1.2675788983306049
-0.5759086932341287
16
0
0
1
-0.4733327294262123
-0.7225353240006611
-0.5661773649877848
-0.09018879949184125
-0.5759086932341287
134
1
0
1
0.3526792885920798
0.044703227440254505
0.01748810393784668
-0.010320111384638534
-0.5759086932341287
110
1
0
1
-0.4733327294262123
1.250660842171233
0.01748810393784668
-0.09018879949184125
1.7363863608036514
Updated dataset after performing RFE feature selection:
id
sex_1
sex_0
age
pclass
fare
Family_count
survived
190
1
0
28
0
0.0
1.0
0
28
1
0
18
0
20.2125 3.0
0
26
1
0
30
0
8.05
1.0
0
79
1
0
28
0
7.775
1.0
1
98
1
0
34
0
13.0
1.0
1
114
1
0
42
1
26.2875 1.0
1
142
0
1
45
0
7.75
1.0
0
16
0
1
28
0
7.55
1.0
0
134
0
1
29
0
26.0
2.0
1
110
0
1
28
1
55.0
2.0
1
Updated dataset after performing scaling on RFE selected features :
r_sex_1 id
survived
r_sex_0 r_age
r_pclass
r_fare
r_Family_count
1
190
0
0
-0.09018879949184125
-0.5759086932341287
-1.036500151284071
-0.5661773649877848
1
28
0
0
-0.8888756805638683
-0.5759086932341287
-0.19596848618924687
0.6011535728634781
```
19: AutoML in teradataml

```python
1
26
0
0
0.06954857672256418
-0.5759086932341287
-0.7017429513328856
-0.56
61773649877848
1
79
1
0
-0.09018879949184125
-0.5759086932341287
-0.713178756300162
-0.5661773649877848
1
98
1
0
0.38902332915137505
-0.5759086932341287
-0.4958984619219083
-0.56
61773649877848
1
114
1
0
1.0279728340089966
1.7363863608036514
0.056658841724225466
-0.5661773649877848
0
142
0
1
1.2675788983306049
-0.5759086932341287
-0.7142183749335509
-0.56
61773649877848
0
16
0
1
-0.09018879949184125
-0.5759086932341287
-0.7225353240006611
-0.5661773649877848
0
134
1
1
-0.010320111384638534
-0.5759086932341287
0.044703227440254505
0.01748810393784668
0
110
1
1
-0.09018879949184125
1.7363863608036514
1.250660842171233
0.01748810393784668
Updated dataset after performing scaling for PCA feature selection :
id
survived
sex_1
sex_0
embarked
sibsp
parch
fare
age
pclass
Family_count
190
0
1
0
-0.5236584390582704
-0.4733327294262123
-0.50514577813811
-1.0365001512840708
-0.09018879949184123
-0.5759086932341301
-0.56
6177364987785
28
0
1
0
-0.5236584390582704
0.3526792885920798
0.7672497196248651
-0.1959684861892468
-0.8888756805638682
-0.57
59086932341301
0.6011535728634785
26
0
1
0
-0.5236584390582704
-0.4733327294262123
-0.50514577813811
-0.7017429513328853
0.06954857672256418
-0.5759086932341301
-0.566177364987785
79
1
1
0
-0.5236584390582704
-0.4733327294262123
-0.50514577813811
-0.7131787563001618
-0.09018879949184123
-0.5759086932341301
-0.56
6177364987785
98
1
1
0
-0.5236584390582704
-0.4733327294262123
-0.50514577813811
-0.4958984619219081
0.389023329151375
-0.5759086932341301
-0.566177364987785
```
19: AutoML in teradataml

```python
114
1
1
0
-0.5236584390582704
-0.4733327294262123
-0.50514577813811
0.05665884172422545
1.0279728340089966
1.7363863608036554
-0.566177364987785
142
0
0
1
-0.5236584390582704
-0.4733327294262123
-0.50514577813811
-0.7142183749335507
1.2675788983306047
-0.5759086932341301
-0.566177364987785
16
0
0
1
-0.5236584390582704
-0.4733327294262123
-0.50514577813811
-0.7225353240006609
-0.09018879949184123
-0.5759086932341301
-0.56
6177364987785
134
1
0
1
-0.5236584390582704
0.3526792885920798
-0.50514577813811
0.04470322744025449
-0.010320111384638533
-0.5759086932341301
0.017488103937846687
110
1
0
1
-0.5236584390582704
-0.4733327294262123
0.7672497196248651
1.2506608421712326
-0.09018879949184123
1.7363863608036554
0.017488103937846687
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
survived
0
34
-0.885025
-1.180737
0.100885
-0.635803
-0.007118
0.330957
0
1
142
-1.149064
-0.329648
-0.857521
0.961232
0.258437
-1.077695
0
2
120
-1.174492
-0.715732
-0.605833
0.365936
-0.214848
0.235200
0
3
16
-0.891443
-0.859990
-0.138386
0.002481
0.458539
-0.989043
0
4
190
-1.135244
-1.133515
-0.235449
0.025433
-0.140323
0.258624
0
173
183
1.321608
-1.392658
0.622355
-0.251383
1.497978
1.361819
1
174
138
-0.988370
-0.956751
-0.196295
-0.244715
-0.08
8857
0.295064
0
175
60
-1.305914
-0.424775
-0.988488
0.866806
-0.319594
0.189375
1
176
72
5.525947
-1.038043
-0.239991
-0.357263
-1.256634
0.570059
0
177
61
-0.478894
0.088547
-0.394947
-0.932471
```
19: AutoML in teradataml

```python
0.033191
0.408747
1
178 rows × 8 columns
Data Transformation completed.
Following model is being used for generating prediction :
Model ID : XGBOOST_2
Feature Selection Method : pca
Prediction :
survived   id  Prediction  Confidence_Lower  Confidence_upper
0         0  120           0             1.000             1.000
1         0  190           0             1.000             1.000
2         1  134           1             0.625             0.625
3         1  144           0             0.750             0.750
4         0   28           0             0.750             0.750
5         1  168           1             0.750             0.750
6         1  110           1             1.000             1.000
7         0   16           1             0.750             0.750
8         0  142           1             0.750             0.750
9         0   34           0             0.750             0.750
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  Precision    Recall        F1
Support
SeqNum
0               0  CLASS_1       99       25   0.798387  0.900000  0.846154
110
1               1  CLASS_2       11       43   0.796296  0.632353  0.704918
68
ROC-AUC :
AUC
GINI
0.7345588235294118
0.46911764705882364
threshold_value tpr
fpr
0.04081632653061224
0.6323529411764706
0.1
0.08163265306122448
0.6323529411764706
0.1
0.1020408163265306
0.6323529411764706
0.1
0.12244897959183673
0.6323529411764706
0.1
0.16326530612244897
0.6323529411764706
0.1
0.18367346938775508
0.6323529411764706
0.1
0.14285714285714285
0.6323529411764706
0.1
0.061224489795918366
0.6323529411764706
0.1
0.02040816326530612
0.6323529411764706
0.1
```
19: AutoML in teradataml

```python
0.0
1.0
1.0
Confusion Matrix :
array([[99, 11],
[25, 43]], dtype=int64)
prediction.head()
survived
id
Prediction
Confidence_Lower
Confidence_upper
0
28
0
0.75
0.75
0
152
0
0.625
0.625
0
103
0
1.0
1.0
0
31
0
0.875
0.875
0
43
0
0.875
0.875
0
37
0
0.875
0.875
0
127
1
0.875
0.875
0
26
0
1.0
1.0
0
190
0
1.0
1.0
0
120
0
1.0
1.0
```
### Example 5: Run AutoML for multiclass classification problem
### using early stopping timer and max_models
This example predicts the species of iris flower based on different factors.
**Run AutoML to acquire the most effective model with the following specifications:**
* Use early stopping timer to 100 sec and max_models to 5.
* Include only ‘xgboost’ model for training.
* Opt for verbose level 2 to get detailed log.
1.
* Load data and split it to train and test datasets.
* a.
    * Load the example data.
```python
load_example_data("teradataml", "iris_input")
```
* b.
    * Perform sampling to get 80% for training and 20% for testing.
```python
iris_sample = iris.sample(frac = [0.8, 0.2])
```
* c.
    * Fetch train and test data.
```python
iris_train= iris_sample[iris_sample['sampleid'] ==
1].drop('sampleid', axis=1)
```
19: AutoML in teradataml

```python
iris_test = iris_sample[iris_sample['sampleid'] ==
2].drop('sampleid', axis=1)
```
2.
* Create an AutoML instance.
```python
aml = AutoML(task_type="Classification"
include=['xgboost'],
verbose=2,
max_runtime_secs=100,
max_models=5)
```
3.
* Fit training data.
```python
aml.fit(iris_train, iris_train.species)
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 120
Total Columns in the data: 6
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
species INTEGER 120
0
None
0
120
0
0.0
100.0
petal_width
FLOAT
120
0
None
0
120
0
0.0
100.0
petal_length
FLOAT
120
0
None
0
120
0
0.0
100.0
id
INTEGER 120
0
None
0
120
0
0.0
100.0
sepal_width
FLOAT
120
0
None
0
120
0
0.0
100.0
sepal_length
FLOAT
120
0
None
0
120
0
0.0
100.0
id  sepal_length  sepal_width  petal_length  petal_width  species
func
min      1.000         4.400        2.200         1.200        0.100    1.000
std     43.485         0.827        0.442         1.766        0.764    0.827
25%     40.750         5.200        2.800         1.600        0.400    1.000
50%     82.500         5.900        3.000         4.500        1.400    2.000
75%    118.250         6.425        3.400         5.225        1.900    3.000
```
19: AutoML in teradataml

```python
max    149.000         7.900        4.400         6.900        2.500    3.000
mean    79.092         5.930        3.070         3.910        1.259    2.067
count  120.000       120.000      120.000       120.000      120.000  120.000
Statistics of Data:
func
id
sepal_length
sepal_width
petal_length
petal_width
species
std
43.485
0.827
0.442
1.766
0.764
0.827
25%
40.75
5.2
2.8
1.6
0.4
1
50%
82.5
5.9
3
4.5
1.4
2
75%
118.25
6.425
3.4
5.225
1.9
3
max
149
7.9
4.4
6.9
2.5
3
min
1
4.4
2.2
1.2
0.1
1
mean
79.092
5.93
3.07
3.91
1.259
2.067
count
120
120
120
120
120
120
Target Column Distribution:
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0  sepal_width           0.833333
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.65 sec
Handling less significant features from data ...
Total time to handle less significant features: 6.17 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
Checking Missing values in dataset ...
```
19: AutoML in teradataml

```python
Analysis Completed. No Missing Values
Detected.
Total time to find missing values in data: 8.95 sec
Imputing Missing Values ...
Analysis completed. No imputation
required.
Time taken to perform imputation: 0.00 sec
Performing encoding for categorical columns ...
Analysis completed. No categorical columns were
found.
Time taken to encode the columns: 1.90 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
Outlier preprocessing ...
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0  sepal_width           0.833333
Deleting rows of these columns:
['sepal_width']
Sample of dataset after removing outlier rows:
sepal_length
sepal_width
petal_length
petal_width
species id
5.4
3.9
1.3
0.4
1
17
7.3
2.9
6.3
1.8
3
55
7.0
3.2
4.7
1.4
2
47
7.4
2.8
6.1
1.9
3
16
7.2
3.6
6.1
2.5
3
34
7.2
3.0
5.8
1.6
3
120
6.7
3.1
4.4
1.4
2
52
6.7
3.3
5.7
2.5
3
144
4.5
2.3
1.3
0.3
1
126
6.1
2.8
4.7
1.2
2
43
119 rows X 6 columns
Time Taken by Outlier processing: 44.85 sec
```
19: AutoML in teradataml

```python
Checking imbalance data ...
Imbalance Not Found.
Feature selection using lasso ...
feature selected by lasso:
['sepal_length', 'sepal_width', 'petal_width', 'petal_length']
Total time taken by feature selection: 2.60 sec
scaling Features of lasso data ...
columns that will be scaled:
['sepal_length', 'sepal_width', 'petal_width', 'petal_length']
Dataset sample after scaling:
species id
sepal_length
sepal_width
petal_width
petal_length
1
10
0.1999999999999998
0.5999999999999999
0.04166666666666667
0.052631578947368425
2
12
0.3714285714285714
0.19999999999999996
0.375
0.40350877192982454
2
13
0.1999999999999998
0.1499999999999999
0.4166666666666667
0.3157894736842105
3
14
0.6571428571428571
0.44999999999999996
0.9583333333333333
0.7719298245614034
3
16
0.8571428571428571
0.2999999999999998
0.75
0.8596491228070174
1
17
0.2857142857142857
0.8499999999999999
0.12500000000000003
0.017543859649122823
3
15
0.457142857142857
0.3999999999999999
0.7083333333333334
0.631578947368421
3
11
0.34285714285714264
0.2999999999999998
0.7916666666666666
0.6491228070175439
3
9
0.5428571428571427
0.5499999999999998
1.0
0.8421052631578947
2
8
0.39999999999999986
0.19999999999999996
0.4583333333333333
0.49122807017543857
119 rows X 6 columns
Total time taken by feature scaling: 28.92 sec
```
19: AutoML in teradataml

```python
Feature selection using rfe ...
feature selected by RFE:
['petal_length', 'petal_width']
Total time taken by feature selection: 7.59 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_petal_length', 'r_petal_width']
Dataset sample after scaling:
species id
r_petal_length
r_petal_width
1
10
0.052631578947368425
0.04166666666666667
2
12
0.40350877192982454
0.375
2
13
0.3157894736842105
0.4166666666666667
3
14
0.7719298245614034
0.9583333333333333
3
16
0.8596491228070174
0.75
1
17
0.017543859649122823
0.12500000000000003
3
15
0.631578947368421
0.7083333333333334
3
11
0.6491228070175439
0.7916666666666666
3
9
0.8421052631578947
1.0
2
8
0.49122807017543857
0.4583333333333333
119 rows X 4 columns
Total time taken by feature scaling: 25.35 sec
scaling Features of pca data ...
columns that will be scaled:
['sepal_length', 'sepal_width', 'petal_length', 'petal_width']
Dataset sample after scaling:
species id
sepal_length
sepal_width
petal_length
petal_width
1
17
0.2857142857142857
0.8499999999999999
0.017543859649122823
0.12500000000000003
3
34
0.7999999999999999
0.7
0.8596491228070174
1.0
3
120
0.7999999999999999
0.3999999999999999
0.8070175438596491
0.625
1
126
0.02857142857142847
0.04999999999999982
0.017543859649122823
0.08333333333333333
3
70
0.5428571428571427
0.2999999999999998
```
19: AutoML in teradataml

```python
0.6842105263157894
0.5833333333333334
2
52
0.6571428571428571
0.44999999999999996
0.5614035087719298
0.5416666666666666
3
144
0.6571428571428571
0.5499999999999998
0.7894736842105263
1.0
2
43
0.4857142857142855
0.2999999999999998
0.6140350877192983
0.4583333333333333
2
84
0.4857142857142855
0.3999999999999999
0.5964912280701753
0.5416666666666666
3
55
0.8285714285714284
0.34999999999999987
0.894736842105263
0.7083333333333334
119 rows X 6 columns
Total time taken by feature scaling: 23.25 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1']
Total time taken by PCA: 6.86 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Hyperparameters used for model training:
response_column : species
name : xgboost
model_type : Classification
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.1, 0.2)
max_depth : (5, 6, 7, 8)
min_node_size : (1, 2)
iter_num : (10, 20)
seed : 42
Total number of models for xgboost : 768
```
19: AutoML in teradataml

```python
Performing hyperparameter tuning ...
xgboost
Leaderboard
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_1
lasso
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
1
2
XGBOOST_2
rfe
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
2
3
XGBOOST_3
rfe
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
3
4
XGBOOST_5
pca
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
4
5
XGBOOST_4
pca
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
5 rows X 13 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 14/14
```
4.
* Display model leaderboard.
```python
aml.leaderboard()
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
```
19: AutoML in teradataml

```python
0
1
XGBOOST_1
lasso
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
1
2
XGBOOST_2
rfe
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
2
3
XGBOOST_3
rfe
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
3
4
XGBOOST_5
pca
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
4
5
XGBOOST_4
pca
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
```
5.
* Display best performing model.
```python
aml.leader()
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_1
lasso
0.916667
0.916667
0.916667
0.916667
0.939394
0.916667
0.919048
0.931818
0.916667
0.914881
```
6.
* Display model hyperparameters for trained model.
```python
aml.model_hyperparameters(rank=2)
{'response_column': 'species',
'name': 'xgboost',
'model_type': 'Classification',
'column_sampling': 1,
'min_impurity': 0.0,
'lambda1': 0.01,
'shrinkage_factor': 0.1,
'max_depth': 7,
'min_node_size': 1,
'iter_num': 20,
'seed': 42,
'persist': False,
'output_prob': True,
```
19: AutoML in teradataml

```python
'output_responses': ['1', '3', '2'],
'max_models': 2}
aml.model_hyperparameters(rank=4)
{'response_column': 'species',
'name': 'xgboost',
'model_type': 'Classification',
'column_sampling': 1,
'min_impurity': 0.1,
'lambda1': 0.1,
'shrinkage_factor': 0.5,
'max_depth': 7,
'min_node_size': 2,
'iter_num': 10,
'seed': 42,
'persist': False,
'output_prob': True,
'output_responses': ['1', '3', '2'],
'max_models': 2}
```
7.
* Generate prediction on test dataset using best performing model.
```python
prediction = aml.predict(iris_test)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping irrelevent columns :
sepal_length
sepal_width
petal_length
petal_width
species
5.9
3.0
5.1
1.8
3
6.9
3.1
4.9
1.5
2
5.6
2.5
3.9
1.1
2
5.7
2.5
5.0
2.0
3
6.6
3.0
4.4
1.4
2
5.0
3.0
1.6
0.2
1
5.0
2.0
3.5
1.0
2
5.0
3.2
1.2
0.2
1
6.7
3.0
5.0
1.7
2
5.7
2.9
4.2
1.3
2
30 rows X 5 columns
```
19: AutoML in teradataml

```python
Updated dataset after performing target column transformation :
sepal_width
sepal_length
petal_width
id
petal_length
species
3.7
5.4
0.2
9
1.5
1
3.0
5.9
1.8
10
5.1
3
3.4
4.6
0.3
18
1.4
1
3.1
6.9
1.5
11
4.9
2
3.0
6.5
1.8
13
5.5
3
3.2
4.6
0.2
21
1.4
1
2.0
5.0
1.0
14
3.5
2
3.2
5.0
0.2
22
1.2
1
3.0
6.7
1.7
15
5.0
2
2.9
5.7
1.3
23
4.2
2
30 rows X 6 columns
Performing transformation carried out in data preparation phase ...
Updated dataset after performing Lasso feature selection:
id
sepal_length
sepal_width
petal_width
petal_length
species
18
4.6
3.4
0.3
1.4
1
35
4.3
3.0
0.1
1.1
1
39
4.6
3.6
0.2
1.0
1
26
5.5
3.5
0.2
1.3
1
36
4.8
3.0
0.3
1.4
1
24
4.4
3.2
0.2
1.3
1
13
6.5
3.0
1.8
5.5
3
12
5.7
2.5
2.0
5.0
3
27
6.9
3.1
2.1
5.4
3
38
5.8
2.8
2.4
5.1
3
30 rows X 6 columns
Updated dataset after performing scaling on Lasso selected features :
species id
sepal_length
sepal_width
petal_width
petal_length
1
18
0.05714285714285694
0.5999999999999999
0.08333333333333333
0.035087719298245605
1
35
-0.028571428571428723
0.3999999999999999
0.0
-0.017543859649122782
1
39
0.05714285714285694
0.7
0.04166666666666667
-0.035087719298245605
1
26
0.31428571428571417
0.6499999999999999
0.04166666666666667
0.017543859649122823
```
19: AutoML in teradataml

```python
1
36
0.11428571428571413
0.3
0.08333333333333333
0.035087719298245605
1
24
0.0
0.5
0.04166666666666667
0.017543859649122823
3
13
0.5
0.3
0.7083333333333334
0.7543859649122806
3
12
0.3714285714285714
0.14

99
0.7916666666666666
0.6666666666666666
3
27
0.7142857142857143
0.44

996
0.8333333333333334
0.7368421052631579
3
38
0.3

9986
0.2

998
0.9583333333333333
0.6842105263157894
30 rows X 6 columns
Updated dataset after performing RFE feature selection:
id
petal_length
petal_width
species
18
1.4
0.3
1
35
1.1
0.1
1
39
1.0
0.2
1
26
1.3
0.2
1
36
1.4
0.3
1
24
1.3
0.2
1
13
5.5
1.8
3
12
5.0
2.0
3
27
5.4
2.1
3
38
5.1
2.4
3
30 rows X 4 columns
Updated dataset after performing scaling on RFE selected features :
species id
r_petal_length
r_petal_width
1
18
0.035087719298245605
0.08333333333333333
1
35
-0.017543859649122782
0.0
1
39
-0.035087719298245605
0.04166666666666667
1
26
0.017543859649122823
0.04166666666666667
1
36
0.035087719298245605
0.08333333333333333
1
24
0.017543859649122823
0.04166666666666667
3
13
0.7543859649122806
0.7083333333333334
3
12
0.6666666666666666
0.7916666666666666
3
27
0.7368421052631579
0.8333333333333334
3
38
0.6842105263157894
0.9583333333333333
30 rows X 4 columns
Updated dataset after performing scaling for PCA feature selection :
species id
sepal_length
sepal_width
petal_length
petal_width
```
19: AutoML in teradataml

```python
1
18
0.05714285714285694
0.5999999999999999
0.035087719298245605
0.08333333333333333
1
35
-0.028571428571428723
0.3999999999999999
-0.017543859649122782
0.0
1
39
0.05714285714285694
0.7
-0.035087719298245605
0.04166666666666667
1
26
0.31428571428571417
0.6499999999999999
0.017543859649122823
0.04166666666666667
1
36
0.11428571428571413
0.3999999999999999
0.035087719298245605
0.08333333333333333
1
24
0.0
0.5
0.017543859649122823
0.04166666666666667
3
13
0.5999999999999999
0.3999999999999999
0.7543859649122806
0.7083333333333334
3
12
0.3714285714285714
0.1499999999999999
0.6666666666666666
0.7916666666666666
3
27
0.7142857142857143
0.44999999999999996
0.7368421052631579
0.8333333333333334
3
38
0.39999999999999986
0.2999999999999998
0.6842105263157894
0.9583333333333333
30 rows X 6 columns
C:\ProgramData\anaconda3\lib\site-packages\sklearn\base.py:450: UserWarning:
X does not have valid feature names, but PCA was fitted with feature names
warnings.warn(
Updated dataset after performing PCA feature selection :
id
col_0
col_1
species
0
22
-0.704914
-0.036801
1
1
13
0.386389
0.033137
3
2
32
-0.677663
-0.141239
1
3
12
0.330417
-0.273881
3
4
18
-0.723513
0.018075
1
5
27
0.494452
0.126603
3
6
21
-0.732106
-0.077591
1
7
38
0.432138
-0.115574
3
8
35
-0.810235
-0.201762
1
9
10
0.269159
-0.024800
3
10 rows X 4 columns
Data Transformation completed.⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 10/10
Following model is being picked for evaluation:
Model ID : XGBOOST_1
```
19: AutoML in teradataml

```python
Feature Selection Method : lasso
Prediction :
id  Prediction  species    prob_1    prob_2    prob_3
0  18           1        1  0.722230  0.143831  0.133939
1  35           1        1  0.612462  0.196722  0.190816
2  39           1        1  0.666637  0.189117  0.144246
3  26           1        1  0.673271  0.186346  0.140383
4  36           1        1  0.719373  0.148393  0.132234
5  24           1        1  0.612294  0.196942  0.190763
6  13           3        3  0.127005  0.135347  0.737648
7  12           3        3  0.195043  0.191731  0.613225
8  27           3        3  0.125647  0.144593  0.729760
9  38           3        3  0.188122  0.191233  0.620645
Confusion Matrix :
array([[13,  0,  0],
[ 0, 10,  2],
[ 0,  0,  5]], dtype=int64)
prediction.head()
id
Prediction
species prob_1
prob_2
prob_3
10
3
3
0.17308871627873726
0.1809314579007279
0.6459798258205349
12
3
3
0.19504311581488437
0.19173143961700242
0.6132254445681132
13
3
3
0.12700497468475547
0.13534664882654565
0.7376483764886987
14
2
2
0.19930249591172716
0.5829781753256542
0.21771932876261862
16
1
1
0.7394828901064238
0.13669212644923803
0.12382498344433823
17
2
2
0.13696417342421638
0.7425026084811045
0.12053321809467908
15
3
2
0.1320286423509596
0.15621056715460405
0.7117607904944363
11
3
2
0.1320411339336852
0.15620831901910395
0.711750547047211
9
1
1
0.7429946876595535
0.13259228517029126
0.1244130271701553
8
2
2
0.14573745579194453
0.734954619345097
0.11930792486295863
```
19: AutoML in teradataml

8.
* Generate evaluation metrics on test dataset using best performing model.
```python
performance_metrics = aml.evaluate(iris_test)
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : XGBOOST_1
Feature Selection Method : lasso
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  CLASS_3  Precision    Recall
F1  Support
SeqNum
0               1  CLASS_1       13        0        0   1.000000  1.000000
1.000000       13
2               3  CLASS_3        0        2        5   0.714286  1.000000
0.833333        5
1               2  CLASS_2        0       10        0   1.000000  0.833333
0.909091       12
SeqNum              Metric  MetricValue
0       3        Micro-Recall     0.933333
1       5     Macro-Precision     0.904762
2       6        Macro-Recall     0.944444
3       7            Macro-F1     0.914141
4       9     Weighted-Recall     0.933333
5      10         Weighted-F1     0.935859
6       8  Weighted-Precision     0.952381
7       4            Micro-F1     0.933333
8       2     Micro-Precision     0.933333
9       1            Accuracy     0.933333
performance_metrics
SeqNum
Prediction
Mapping CLASS_1 CLASS_2 CLASS_3 Precision
Recall
F1
Support
0
1
CLASS_1 13
0
0
1.0
1.0
1.0
13
2
3
CLASS_3 0
2
5
0.7142857142857143
1.0
0.8333333333333333
5
1
2
CLASS_2 0
10
0
1.0
0.8333333333333334
0.9090909090909091
12
```
19: AutoML in teradataml

### Example 5: Run AutoML for Multiclass Classification Problem
### using Early Stopping Timer
This example predicts the species of iris flower based on different factors.
**Run AutoML to acquire the most effective model with the following specifications:**
* Use early stopping timer to 300 sec.
* Include only ‘xgboost’ model for training.
* Opt for verbose level 2 to get detailed log.
1.
* Load data and split it to train and test datasets.
* a.
    * Load the example data and create teradataml DataFrame.
```python
load_example_data("teradataml", "iris_input")
```
* b.
    * Perform sampling to get 80% for training and 20% for testing.
```python
iris_sample = iris.sample(frac = [0.8, 0.2])
```
* c.
    * Fetch train and test data.
```python
iris_train= iris_sample[iris_sample['sampleid'] ==
1].drop('sampleid', axis=1)
iris_test = iris_sample[iris_sample['sampleid'] ==
2].drop('sampleid', axis=1)
```
2.
* Create an AutoML instance.
```python
aml = AutoML(task_type="Classification"
include=['xgboost'],
verbose=2,
max_runtime_secs=300)
```
3.
* Fit training data.
```python
aml.fit(iris_train, iris_train.species)
```
4.
* Display model leaderboard.
```python
aml.leaderboard()
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
```
19: AutoML in teradataml

```python
0
1
XGBOOST_2
pca
0.958333
0.958333
0.958333
0.958333
0.962963
0.958333
0.958170
0.962963
0.958333
0.958170
1
2
XGBOOST_5
pca
0.958333
0.958333
0.958333
0.958333
0.962963
0.958333
0.958170
0.962963
0.958333
0.958170
2
3
XGBOOST_1
rfe
0.875000
0.875000
0.875000
0.875000
0.878307
0.875000
0.874510
0.878307
0.875000
0.874510
3
4
XGBOOST_4
rfe
0.875000
0.875000
0.875000
0.875000
0.878307
0.875000
0.874510
0.878307
0.875000
0.874510
4
5
XGBOOST_7
rfe
0.875000
0.875000
0.875000
0.875000
0.878307
0.875000
0.874510
0.878307
0.875000
0.874510
5
6
XGBOOST_0
lasso
0.791667
0.791667
0.791667
0.791667
0.812121
0.791667
0.784076
0.812121
0.791667
0.784076
6
7
XGBOOST_3
lasso
0.791667
0.791667
0.791667
0.791667
0.812121
0.791667
0.784076
0.812121
0.791667
0.784076
7
8
XGBOOST_6
lasso
0.750000
0.750000
0.750000
0.750000
0.857143
0.750000
0.709091
0.857143
0.750000
0.709091
```
5.
* Display the best performing model.
```python
aml.leader()
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
XGBOOST_2
pca
0.958333
0.958333
0.958333
0.958333
0.962963
0.958333
0.95817
0.962963
0.958333
0.95817
```
6.
* Generate prediction on validation dataset using best performing model.
* Note:
* In the data preparation phase, AutoML generates the validation dataset by splitting the data
* provided during fitting into training and testing sets. AutoML's model training utilizes the training
* data, with the testing data acting as the validation dataset for model evaluation.
19: AutoML in teradataml

```python
prediction = aml.predict()
Following model is being used for generating prediction :
Model ID : XGBOOST_2
Feature Selection Method : pca
Prediction :
id  Prediction  Confidence_Lower  Confidence_upper  species
0   27           2             0.625             0.625        2
1   29           3             0.750             0.750        3
2   30           1             1.000             1.000        1
3   31           2             0.625             0.625        2
4   91           3             0.750             0.750        3
5   84           2             0.625             0.625        2
6  130           2             1.000             1.000        2
7   28           3             0.750             0.750        3
8  114           1             1.000             1.000        1
9   25           1             1.000             1.000        1
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  CLASS_3  Precision  Recall
F1  Support
SeqNum
0               1  CLASS_1        8        0        0   1.000000   1.000
1.000000        8
2               3  CLASS_3        0        0        7   1.000000   0.875
0.933333        8
1               2  CLASS_2        0        8        1   0.888889   1.000
0.941176        8
Confusion Matrix :
array([[8, 0, 0],
[0, 8, 0],
[0, 1, 7]], dtype=int64)
prediction.head()
id
Prediction
Confidence_Lower
Confidence_upper
species
28
3
0.75
0.75
3
30
1
1.0
1.0
1
31
2
0.625
0.625
2
84
2
0.625
0.625
2
```
19: AutoML in teradataml

```python
91
3
0.75
0.75
3
93
2
0.625
0.625
2
85
1
1.0
1.0
1
29
3
0.75
0.75
3
27
2
0.625
0.625
2
25
1
1.0
1.0
1
```
7.
* Generate prediction on test dataset using best performing model.
```python
prediction = aml.predict(iris_test)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping irrelevent columns :
sepal_length
sepal_width
petal_length
petal_width
species
6.4
2.8
5.6
2.1
3
4.4
2.9
1.4
0.2
1
5.6
3.0
4.1
1.3
2
5.7
2.5
5.0
2.0
3
4.9
2.4
3.3
1.0
2
5.5
2.3
4.0
1.3
2
6.1
2.8
4.7
1.2
2
5.1
3.8
1.6
0.2
1
5.1
3.4
1.5
0.2
1
5.9
3.0
5.1
1.8
3
Updated dataset after performing target column transformation :
petal_width
sepal_length
petal_length
sepal_width
id
species
0.2
4.4
1.4
2.9
9
1
0.2
5.1
1.5
3.4
10
1
1.8
5.9
5.1
3.0
18
3
0.4
5.4
1.5
3.4
15
1
1.0
4.9
3.3
2.4
14
2
1.3
5.5
4.0
2.3
22
2
2.0
5.7
5.0
2.5
12
3
1.4
6.7
4.4
3.1
20
2
2.1
6.4
5.6
2.8
13
3
1.8
6.5
5.5
3.0
21
3
Performing transformation carried out in data preparation phase ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713853197024405"'
```
19: AutoML in teradataml

```python
Updated dataset after performing Lasso feature selection:
id
sepal_length
petal_width
sepal_width
petal_length
species
19
5.1
0.2
3.8
1.6
1
17
5.6
1.3
3.0
4.1
2
34
6.2
2.3
3.4
5.4
3
26
5.1
0.4
3.7
1.5
1
15
5.4
0.4
3.4
1.5
1
32
6.7
2.5
3.3
5.7
3
36
4.8
0.3
3.0
1.4
1
28
5.8
1.2
2.7
3.9
2
38
6.3
1.5
2.8
5.1
3
12
5.7
2.0
2.5
5.0
3
Updated dataset after performing scaling on Lasso selected features :
id
species sepal_length
petal_width
sepal_width
petal_length
19
1
0.2121212121212119
0.04166666666666667
0.8888888888888887
0.07017543859649125
38
3
0.5757575757575756
0.5833333333333334
0.33333333333333315
0.6842105263157894
12
3
0.3939393939393939
0.7916666666666666
0.16666666666666657
0.6666666666666666
17
2
0.3636363636363634
0.5
0.44444444444444436
0.5087719298245613
22
2
0.33333333333333326
0.5
0.055555555555555365
0.49122807017543857
35
1
0.30303030303030304
0.04166666666666667
0.6666666666666666
0.08771929824561403
15
1
0.30303030303030304
0.12500000000000003
0.6666666666666666
0.052631578947368425
32
3
0.6969696969696969
1.0
0.6111111111111109
0.7894736842105263
36
1
0.12121212121212106
0.08333333333333333
0.44444444444444436
0.035087719298245605
28
2
0.4242424242424241
0.4583333333333333
0.2777777777777778
0.4736842105263158
Updated dataset after performing RFE feature selection:
id
petal_length
petal_width
species
15
1.5
0.4
1
22
4.0
1.3
2
35
1.7
0.2
1
38
5.1
1.5
3
36
1.4
0.3
1
```
19: AutoML in teradataml

```python
28
3.9
1.2
2
19
1.6
0.2
1
30
5.1
2.4
3
17
4.1
1.3
2
34
5.4
2.3
3
Updated dataset after performing scaling on RFE selected features :
id
species r_petal_length
r_petal_width
19
1
0.07017543859649125
0.04166666666666667
38
3
0.6842105263157894
0.5833333333333334
12
3
0.6666666666666666
0.7916666666666666
36
1
0.035087719298245605
0.08333333333333333
22
2
0.49122807017543857
0.5
35
1
0.08771929824561403
0.04166666666666667
17
2
0.5087719298245613
0.5
34
3
0.7368421052631579
0.9166666666666666
15
1
0.052631578947368425
0.12500000000000003
32
3
0.7894736842105263
1.0
Updated dataset after performing scaling for PCA feature selection :
id
species sepal_length
sepal_width
petal_length
petal_width
22
2
0.33333333333333326
0.055555555555555365
0.49122807017543857
0.5
38
3
0.5757575757575756
0.33333333333333315
0.6842105263157894
0.5833333333333334
12
3
0.3939393939393939
0.16666666666666657
0.6666666666666666
0.7916666666666666
15
1
0.30303030303030304
0.6666666666666666
0.052631578947368425
0.12500000000000003
19
1
0.2121212121212119
0.8888888888888887
0.07017543859649125
0.04166666666666667
30
3
0.4242424242424241
0.33333333333333315
0.6842105263157894
0.9583333333333333
36
1
0.12121212121212106
0.44444444444444436
0.035087719298245605
0.08333333333333333
28
2
0.4242424242424241
0.2777777777777778
0.4736842105263158
0.4583333333333333
17
2
0.3636363636363634
0.44444444444444436
0.5087719298245613
0.5
34
3
0.5454545454545454
0.6666666666666666
0.7368421052631579
0.9166666666666666
Updated dataset after performing PCA feature selection :
id
col_0
col_1
species
```
19: AutoML in teradataml

```python
0
26
-0.637053
-0.237890
1
1
17
0.019809
0.057742
2
2
22
0.074623
0.428188
2
3
19
-0.688597
-0.282715
1
4
38
0.298288
0.083267
3
5
36
-0.635618
0.157913
1
6
15
-0.561092
-0.117245
1
7
20
0.219649
-0.117778
2
8
34
0.451922
-0.235519
3
9
35
-0.590568
-0.109948
1
Data Transformation completed.
Following model is being used for generating prediction :
Model ID : XGBOOST_2
Feature Selection Method : pca
Prediction :
id  Prediction  Confidence_Lower  Confidence_upper  species
0  22           2             0.875             0.875        2
1  38           2             0.500             0.500        3
2  36           1             1.000             1.000        1
3  15           1             1.000             1.000        1
4  34           3             0.750             0.750        3
5  35           1             1.000             1.000        1
6  20           2             0.625             0.625        2
7  19           1             1.000             1.000        1
8  17           2             0.625             0.625        2
9  26           1             1.000             1.000        1
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  CLASS_3  Precision    Recall
F1  Support
SeqNum
0               1  CLASS_1        9        0        0   1.000000  1.000000
1.000000        9
2               3  CLASS_3        0        0        9   1.000000  0.818182
0.900000       11
1               2  CLASS_2        0       10        2   0.833333  1.000000
0.909091       10
Confusion Matrix :
array([[ 9,  0,  0],
```
19: AutoML in teradataml

```python
[ 0, 10,  0],
[ 0,  2,  9]], dtype=int64)
prediction.head()
id
Prediction
Confidence_Lower
Confidence_upper
species
10
1
1.0
1.0
1
12
3
0.625
0.625
3
13
3
0.75
0.75
3
14
2
1.0
1.0
2
16
2
0.625
0.625
2
17
2
0.625
0.625
2
15
1
1.0
1.0
1
11
2
0.625
0.625
2
9
1
1.0
1.0
1
8
3
0.75
0.75
3
```
### Example 6: Run AutoClassifier for Multiclass Classification
### Problem using Early Stopping Timer
This example predicts the species of iris flower based on different factors.
**Run AutoML to acquire the most effective model with the following specifications:**
* Use early stopping timer to 100300 sec.
* Include only 'xgboost' model for training.
* Opt for verbose level 2 to get detailed log.
* Add customization for some specific processes of AutoClassifier.
1.
* Load data and split it to train and test datasets.
* a.
    * Load the example data and create teradataml DataFrame.
```python
load_example_data("teradataml", "iris_input")
```
* b.
    * Perform sampling to get 80% for training and 20% for testing.
```python
iris_sample = iris.sample(frac = [0.8, 0.2])
```
* c.
    * Fetch train and test data.
```python
iris_train= iris_sample[iris_sample['sampleid'] ==
1].drop('sampleid', axis=1)
iris_test = iris_sample[iris_sample['sampleid'] ==
2].drop('sampleid', axis=1)
```
19: AutoML in teradataml

2.
* Add customization.
```python
AutoClassifier.generate_custom_config("custom_iris")
Generating custom config JSON for AutoML ...
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  2
Customizing Data Preparation Phase ...
Available options for customization of data preparation phase with
corresponding indices:
Index 1: Customize Data Imbalance Handling
Index 2: Customize Outlier Handling
Index 3: Customize Feature Scaling
Index 4: Back to main menu
Index 5: Generate custom json and exit
Enter the list of indices you want to customize in data preparation phase:
1,3
```
19: AutoML in teradataml

```python
Customizing Data Imbalance Handling ...
Available data sampling methods with corresponding indices:
Index 1: SMOTE
Index 2: NearMiss
Enter the corresponding index data imbalance handling method:  1
Customization of data imbalance handling has been completed successfully.
Available feature scaling methods with corresponding indices:
Index 1: maxabs
Index 2: mean
Index 3: midrange
Index 4: range
Index 5: rescale
Index 6: std
Index 7: sum
Index 8: ustd
Enter the corresponding index feature scaling method:  6
Available options for generic arguments:
Index 0: Default
Index 1: volatile
Index 2: persist
Enter the indices for generic arguments :  1
Customization of feature scaling has been completed successfully.
Available options for customization of data preparation phase with
corresponding indices:
Index 1: Customize Data Imbalance Handling
Index 2: Customize Outlier Handling
Index 3: Customize Feature Scaling
Index 4: Back to main menu
```
19: AutoML in teradataml

```python
Index 5: Generate custom json and exit
Enter the list of indices you want to customize in data preparation phase:  4
Customization of data preparation phase has been completed successfully.
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  3
Customizing Model Training Phase ...
Available options for customization of model training phase with corresponding
indices:
Index 1: Customize Model Hyperparameter
Index 2: Back to main menu
Index 3: Generate custom json and exit
Enter the list of indices you want to customize in model training phase:  1
Customizing Model Hyperparameter ...
Available models for hyperparameter tuning with corresponding indices:
```
19: AutoML in teradataml

```python
Index 1: decision_forest
Index 2: xgboost
Index 3: knn
Index 4: glm
Index 5: svm
Available hyperparamters update methods with corresponding indices:
Index 1: ADD
Index 2: REPLACE
Enter the list of model indices for performing hyperparameter tuning:  2
Available hyperparameters for model 'xgboost' with corresponding indices:
Index 1: min_impurity
Index 2: max_depth
Index 3: min_node_size
Index 4: shrinkage_factor
Index 5: iter_num
Enter the list of hyperparameter indices for model 'xgboost':  2
Enter the index of corresponding update method for hyperparameters 'max_depth'
for model 'xgboost':  2
Enter the list of value for hyperparameter 'max_depth' for model 'xgboost':
3, 4
Customization of model hyperparameter has been completed successfully.
Available options for customization of model training phase with corresponding
indices:
Index 1: Customize Model Hyperparameter
Index 2: Back to main menu
Index 3: Generate custom json and exit
Enter the list of indices you want to customize in model training phase:  3
```
19: AutoML in teradataml

```python
Customization of model training phase has been completed successfully.
Process of generating custom config file for AutoML has been completed
successfully.
'custom_iris.json' file is generated successfully under the current working
directory.
Generating custom config JSON for AutoML ...
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  1
Customizing Feature Engineering Phase ...
Available options for customization of feature engineering phase with
corresponding indices:
Index 1: Customize Missing Value Handling
Index 2: Customize Bincode Encoding
Index 3: Customize String Manipulation
Index 4: Customize Categorical Encoding
Index 5: Customize Mathematical Transformation
```
19: AutoML in teradataml

```python
Index 6: Customize Nonlinear Transformation
Index 7: Customize Antiselect Features
Index 8: Back to main menu
Index 9: Generate custom json and exit
Enter the list of indices you want to customize in feature engineering phase:
8
Customization of feature engineering phase has been completed successfully.
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
Enter the index you want to customize:  2
Customizing Data Preparation Phase ...
Available options for customization of data preparation phase with
corresponding indices:
Index 1: Customize Train Test Split
Index 2: Customize Data Imbalance Handling
Index 3: Customize Outlier Handling
```
19: AutoML in teradataml

```python
Index 4: Customize Feature Scaling
Index 5: Back to main menu
Index 6: Generate custom json and exit
Enter the list of indices you want to customize in data preparation phase:  1,
4, 5
Customizing Train Test Split ...
Enter the train size for train test split:  0.85
Customization of train test split has been completed successfully.
Available feature scaling methods with corresponding indices:
Index 1: maxabs
Index 2: mean
Index 3: midrange
Index 4: range
Index 5: rescale
Index 6: std
Index 7: sum
Index 8: ustd
Enter the corresponding index feature scaling method:  4
Customization of feature scaling has been completed successfully.
Customization of data preparation phase has been completed successfully.
Available main options for customization with corresponding indices:
Index 1: Customize Feature Engineering Phase
Index 2: Customize Data Preparation Phase
Index 3: Customize Model Training Phase
Index 4: Generate custom json and exit
```
19: AutoML in teradataml

```python
Enter the index you want to customize:  3
Customizing Model Training Phase ...
Available options for customization of model training phase with corresponding
indices:
Index 1: Customize Model Hyperparameter
Index 2: Back to main menu
Index 3: Generate custom json and exit
Enter the list of indices you want to customize in model training phase:  1
Customizing Model Hyperparameter ...
Available models for hyperparameter tuning with corresponding indices:
Index 1: decision_forest
Index 2: xgboost
Index 3: knn
Index 4: glm
Index 5: svm
Available hyperparamters update methods with corresponding indices:
Index 1: ADD
Index 2: REPLACE
Enter the list of model indices for performing hyperparameter tuning:  2
Available hyperparameters for model 'xgboost' with corresponding indices:
Index 1: min_impurity
Index 2: max_depth
Index 3: min_node_size
Index 4: shrinkage_factor
Index 5: iter_num
```
19: AutoML in teradataml

```python
Enter the list of hyperparameter indices for model 'xgboost':  2
Enter the index of corresponding update method for hyperparameters 'max_depth'
for model 'xgboost':  2
Enter the list of value for hyperparameter 'max_depth' for model 'xgboost':
3,4
Customization of model hyperparameter has been completed successfully.
Available options for customization of model training phase with corresponding
indices:
Index 1: Customize Model Hyperparameter
Index 2: Back to main menu
Index 3: Generate custom json and exit
Enter the list of indices you want to customize in model training phase:  3
Customization of model training phase has been completed successfully.
Process of generating custom config file for AutoML has been completed
successfully.
'custom_iris.json' file is generated successfully under the current working
directory.
```
3.
* Create an AutoML instance.
```python
aml = AutoClassifier(include=['xgboost'],
verbose=2,
max_runtime_secs=100,
custom_config_file='custom_iris.json')
aml = AutoClassifier(include=['xgboost'],
verbose=2,
max_runtime_secs=300,
custom_config_file='custom_iris.json')
```
19: AutoML in teradataml

4.
* Fit training data.
```python
aml.fit(iris_train, iris_train.species)
Received below input for customization :
{
"DataImbalanceIndicator": true,
"DataImbalanceMethod": "SMOTE",
"FeatureScalingIndicator": true,
"FeatureScalingParam": {
"FeatureScalingMethod": "std",
"volatile": true
},
"HyperparameterTuningIndicator": true,
"HyperparameterTuningParam": {
"xgboost": {
"max_depth": {
"Method": "REPLACE",
"Value": [
3,
4
]
}
}
}
}
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 120
Total Columns in the data: 6
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
sepal_width
FLOAT
120
0
None
0
120
0
0.0
100.0
petal_length
FLOAT
120
0
None
0
120
0
0.0
100.0
id
INTEGER 120
0
None
0
120
0
0.0
100.0
```
19: AutoML in teradataml

```python
species INTEGER 120
0
None
0
120
0
0.0
100.0
sepal_length
FLOAT
120
0
None
0
120
0
0.0
100.0
petal_width
FLOAT
120
0
None
0
120
0
0.0
100.0
id  sepal_length  sepal_width  petal_length  petal_width  species
func
min      1.000         4.300        2.200         1.000        0.100    1.000
std     43.378         0.799        0.445         1.729        0.745    0.809
25%     37.750         5.200        2.800         1.500        0.300    1.000
50%     73.500         5.750        3.000         4.300        1.300    2.000
75%    111.250         6.300        3.300         5.025        1.800    3.000
max    150.000         7.900        4.400         6.900        2.500    3.000
mean    74.150         5.810        3.055         3.718        1.177    1.983
count  120.000       120.000      120.000       120.000      120.000  120.000
Statistics of Data:
func
id
sepal_length
sepal_width
petal_length
petal_width
species
50%
73.5
5.75
3
4.3
1.3
2
count
120
120
120
120
120
120
mean
74.15
5.81
3.055
3.718
1.177
1.983
min
1
4.3
2.2
1
0.1
1
max
150
7.9
4.4
6.9
2.5
3
75%
111.25
6.3
3.3
5.025
1.8
3
25%
37.75
5.2
2.8
1.5
0.3
1
std
43.378
0.799
0.445
1.729
0.745
0.809
Target Column Distribution:
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0  sepal_width                2.5
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.52 sec
```
19: AutoML in teradataml

```python
Starting customized anti-select columns ...
Skipping customized anti-select
columns.
Handling less significant features from data ...
Total time to handle less significant features: 6.13 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
Proceeding with default option for missing value imputation.
Proceeding with default option for handling remaining missing
values.
Checking Missing values in dataset ...
Analysis Completed. No Missing Values
Detected.
Total time to find missing values in data: 7.43 sec
Imputing Missing Values ...
Analysis completed. No imputation
required.
Time taken to perform imputation: 0.00 sec
No information provided for Variable-Width Transformation.
Skipping customized string
manipulation.
Starting Customized Categorical Feature Encoding ...
AutoML will proceed with default encoding
technique.
Performing encoding for categorical columns ...
Analysis completed. No categorical columns were
found.
Time taken to encode the columns: 1.44 sec
Starting customized mathematical transformation ...
Skipping customized mathematical
transformation.
Starting customized non-linear transformation ...
Skipping customized non-linear
transformation.
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
```
19: AutoML in teradataml

```python
Data preparation started ...
Starting customized outlier processing ...
No information provided for customized outlier processing. AutoML will proceed
with default settings.
Outlier preprocessing ...
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0  sepal_width                2.5
Deleting rows of these columns:
['sepal_width']
Sample of dataset after removing outlier rows:
sepal_length
sepal_width
petal_length
petal_width
species id
5.4
3.0
4.5
1.5
2
63
5.4
3.9
1.3
0.4
1
9
5.4
3.4
1.7
0.2
1
115
4.4
3.0
1.3
0.2
1
95
4.4
3.2
1.3
0.2
1
40
6.6
2.9
4.6
1.3
2
27
4.4
2.9
1.4
0.2
1
41
5.4
3.9
1.7
0.4
1
77
5.4
3.4
1.5
0.4
1
39
5.4
3.7
1.5
0.2
1
33
117 rows X 6 columns
Time Taken by Outlier processing: 39.01 sec
Checking imbalance data ...
Imbalance Not Found.
Feature selection using lasso ...
feature selected by lasso:
['sepal_width', 'petal_length', 'petal_width', 'sepal_length']
Total time taken by feature selection: 2.63 sec
scaling Features of lasso data ...
```
19: AutoML in teradataml

```python
columns that will be scaled:
['sepal_width', 'petal_length', 'petal_width', 'sepal_length']
Dataset sample after scaling:
id
species sepal_width
petal_length
petal_width
sepal_length
10
3
-2.0350680494119757
0.7177312821556723
0.40679582737559905
0.22565555366314133
12
2
-1.0481127674173594
-0.1618905056817609
-0.2731343412379006
-0.14795340546781913
13
2
-0.801373946918705
0.24859966197570801
0.13482375993019924
-0.27248972517814
14
3
0.43232015557456555
0.8936556397231588
1.4946840971571982
0.7238008325044227
16
2
-1.0481127674173594
0.13131675693071682
-0.0011622737925007988
-0.023417085757499345
18
2
0.6790589760732187
0.5418069245881857
0.5427818610982991
0.5992645127941018
15
3
-0.06115748542274311
0.6004483771106811
0.8147539285436989
0.22565555366314133
11
3
-0.5546351264200517
0.6590898296331769
1.0867259959890987
-0.27248972517814
9
1
2.1594918990651437
-1.4520024611766627
-1.0890505435741
-0.52
15623645987796
8
2
-0.06115748542274311
0.3658825670206992
0.270809793652899
0.9728734719250622
117 rows X 6 columns
Total time taken by feature scaling: 30.28 sec
Feature selection using rfe ...
feature selected by RFE:
['sepal_length', 'sepal_width', 'petal_length', 'petal_width']
Total time taken by feature selection: 7.70 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_sepal_length', 'r_sepal_width', 'r_petal_length', 'r_petal_width']
```
19: AutoML in teradataml

```python
Dataset sample after scaling:
id
species r_sepal_length
r_sepal_width
r_petal_length
r_petal_width
10
3
0.22565555366314133
-2.0350680494119757
0.7177312821556723
0.40679582737559905
12
2
-0.14795340546781913
-1.0481127674173594
-0.1618905056817609
-0.2731343412379006
13
2
-0.27248972517814
-0.801373946918705
0.24859966197570801
0.13482375993019924
14
3
0.7238008325044227
0.43232015557456555
0.8936556397231588
1.4946840971571982
16
2
-0.023417085757499345
-1.0481127674173594
0.13131675693071682
-0.0011622737925007988
18
2
0.5992645127941018
0.6790589760732187
0.5418069245881857
0.5427818610982991
15
3
0.22565555366314133
-0.06115748542274311
0.6004483771106811
0.8147539285436989
11
3
-0.27248972517814
-0.5546351264200517
0.6590898296331769
1.0867259959890987
9
1
-0.5215623645987796
2.1594918990651437
-1.4520024611766627
-1.0890505435741
8
2
0.9728734719250622
-0.06115748542274311
0.3658825670206992
0.270809793652899
117 rows X 6 columns
Total time taken by feature scaling: 28.48 sec
scaling Features of pca data ...
columns that will be scaled:
['sepal_length', 'sepal_width', 'petal_length', 'petal_width']
Dataset sample after scaling:
id
species sepal_length
sepal_width
petal_length
petal_width
87
3
0.5992645127941051
-0.3078963059213974
1.0695799972906452
0.8147539285436984
128
3
1.7200913901869874
0.43232015557456543
1.3041458073806276
0.8147539285436984
33
1
-0.5215623645987763
1.6660142580678357
-1.3347195561316718
-1.361022611019501
39
1
-0.5215623645987763
0.9257977965718729
-1.3347195561316718
-1.089050543574101
```
19: AutoML in teradataml

```python
100
3
1.0974097916353864
-1.2948515879160134
1.1868629023356365
0.8147539285436984
35
2
0.3501918733734644
-0.5546351264200516
0.5418069245881857
-0.001162273792501403
28
3
0.3501918733734644
-1.0481127674173591
1.0695799972906452
0.2708097936528984
47
2
1.4710187507663468
0.43232015557456543
0.5418069245881857
0.2708097936528984
10
3
0.22565555366314463
-2.035068049411975
0.7177312821556723
0.4067958273755985
106
3
0.4747281930837853
0.9257977965718729
0.9522970922456546
1.4946840971571977
117 rows X 6 columns
Total time taken by feature scaling: 23.24 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1']
Total time taken by PCA: 6.30 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Starting customized hyperparameter update ...
Completed customized hyperparameter update.
Hyperparameters used for model training:
response_column :
species
name : xgboost
model_type : Classification
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.1, 0.2)
```
19: AutoML in teradataml

```python
max_depth : (3, 4)
min_node_size : (1, 2)
iter_num : (10, 20)
seed : 42
Total number of models for xgboost : 384
Performing hyperparameter tuning ...
xgboost
Leaderboard
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_2
pca
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.0000
1.000000
1.000000
1
2
XGBOOST_3
lasso
0.958333
0.958333
0.958333
0.958333
0.966667
0.958333
0.960234
0.9625
0.958333
0.958041
2
3
XGBOOST_1
rfe
0.958333
0.958333
0.958333
0.958333
0.966667
0.958333
0.960234
0.9625
0.958333
0.958041
3
4
XGBOOST_0
lasso
0.958333
0.958333
0.958333
0.958333
0.966667
0.958333
0.960234
0.9625
0.958333
0.958041
4 rows X 13 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 18/18
Received below input for customization :
{
"TrainTestSplitIndicator": true,
"TrainingSize": 0.85,
```
19: AutoML in teradataml

```python
"DataImbalanceIndicator": true,
"DataImbalanceMethod": "SMOTE",
"FeatureScalingIndicator": true,
"FeatureScalingMethod": "range",
"HyperparameterTuningIndicator": true,
"HyperparameterTuningParam": {
"xgboost": {
"max_depth": {
"Method": "ADD",
"Value": [
3,
4
]
}
}
}
}
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 120
Total Columns in the data: 6
Column Summary:
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
species INTEGER 120
0
None
0
120
0
0.0
100.0
sepal_length
FLOAT
120
0
None
0
120
0
0.0
100.0
petal_length
FLOAT
120
0
None
0
120
0
0.0
100.0
sepal_width
FLOAT
120
0
None
0
120
0
0.0
100.0
petal_width
FLOAT
120
0
None
0
120
0
0.0
100.0
id
INTEGER 120
0
None
0
120
0
0.0
100.0
Statistics of Data:
func
id
sepal_length
sepal_width
petal_length
petal_width
species
```
19: AutoML in teradataml

```python
std
42.746
0.828
0.445
1.784
0.764
0.825
25%
37.75
5.1
2.775
1.5
0.275
1
50%
72.5
5.8
3
4.2
1.3
2
75%
110.25
6.4
3.3
5.1
1.8
3
max
149
7.7
4.4
6.9
2.5
3
min
2
4.3
2
1
0.1
1
mean
73.642
5.818
3.033
3.683
1.158
1.975
count
120
120
120
120
120
120
Target Column Distribution:
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0  sepal_width           0.833333
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.57 sec
Handling less significant features from data ...
Total time to handle less significant features: 5.83 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
Proceeding with default option for missing value
imputation.
Proceeding with default option for handling remaining missing
values.
Checking Missing values in dataset ...
Analysis Completed. No Missing Values
```
19: AutoML in teradataml

```python
Detected.
Total time to find missing values in data: 7.15 sec
Imputing Missing Values ...
Analysis completed. No imputation
required.
Time taken to perform imputation: 0.01 sec
No information provided for Variable-Width
Transformation.
Skipping customized string manipulation.⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾
```
* ｜  26% - 5/19
```python
Starting Customized Categorical Feature Encoding ...
AutoML will proceed with default encoding
technique.
Performing encoding for categorical columns ...
Analysis completed. No categorical columns were
found.
Time taken to encode the columns: 1.42 sec
Starting customized mathematical transformation ...
Skipping customized mathematical
transformation.
Starting customized non-linear transformation ...
Skipping customized non-linear
transformation.
Starting customized anti-select columns ...
Skipping customized anti-select
columns.
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
Spliting of dataset into training and testing ...
Training size :
0.85
```
19: AutoML in teradataml

```python
Testing size  :
0.15
Training data sample
sepal_length
sepal_width
petal_length
petal_width
species id
5.1
3.4
1.5
0.2
1
10
5.0
2.0
3.5
1.0
2
14
5.0
3.2
1.2
0.2
1
22
5.7
2.6
3.5
1.0
2
12
6.3
3.3
6.0
2.5
3
9
5.4
3.9
1.3
0.4
1
17
5.1
2.5
3.0
1.1
2
13
5.6
2.7
4.2
1.3
2
21
6.7
3.0
5.0
1.7
2
15
6.0
3.0
4.8
1.8
3
23
102 rows X 6 columns
Testing data sample
sepal_length
sepal_width
petal_length
petal_width
species id
6.4
3.2
5.3
2.3
3
30
5.7
2.9
4.2
1.3
2
31
6.3
2.9
5.6
1.8
3
103
6.3
3.4
5.6
2.4
3
27
6.5
2.8
4.6
1.5
2
28
6.7
2.5
5.8
1.8
3
108
6.4
2.7
5.3
1.9
3
29
5.4
3.9
1.7
0.4
1
85
6.2
2.2
4.5
1.5
2
107
5.6
2.9
3.6
1.3
2
110
18 rows X 6 columns
Time taken for spliting of data: 11.26 sec
Starting customized outlier processing ...
No information provided for customized outlier processing. AutoML will proceed
with default settings.
Outlier preprocessing ...
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0  sepal_width           0.833333
```
19: AutoML in teradataml

```python
Deleting rows of these columns:
['sepal_width']
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713845938369288"'19
Sample of training dataset after removing outlier rows:
sepal_length
sepal_width
petal_length
petal_width
species id
7.2
3.6
6.1
2.5
3
34
7.4
2.8
6.1
1.9
3
24
6.2
3.4
5.4
2.3
3
106
6.1
2.8
4.7
1.2
2
35
7.3
2.9
6.3
1.8
3
71
7.0
3.2
4.7
1.4
2
63
6.7
3.3
5.7
2.1
3
95
6.7
3.0
5.2
2.3
3
87
5.4
3.9
1.3
0.4
1
17
5.4
3.4
1.5
0.4
1
47
101 rows X 6 columns
Time Taken by Outlier processing: 35.55 sec
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713844347449263"'19
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713849039953629"'
Checking imbalance data ...
Imbalance Not Found.
Feature selection using lasso ...
feature selected by lasso:
['petal_width', 'sepal_width', 'sepal_length', 'petal_length']
Total time taken by feature selection: 3.37 sec
scaling Features of lasso data ...
columns that will be scaled:
['petal_width', 'sepal_width', 'sepal_length', 'petal_length']
Training dataset sample after scaling:
```
19: AutoML in teradataml

```python
id
species petal_width
sepal_width
sepal_length
petal_length
40
3
0.7916666666666666
0.4761904761904763
0.6470588235294118
0.711864406779661
80
1
0.08333333333333333
0.7142857142857144
0.23529411764705874
0.06779661016949151
99
1
0.0
0.4761904761904763
0.0
0.016949152542372895
61
3
0.7083333333333334
0.4761904761904763
0.6470588235294118
0.7627118644067796
93
2
0.625
0.3333333333333335
0.5
0.6949152542372881
34
3
1.0
0.7619047619047621
0.8529411764705882
0.8644067796610169
78
2
0.5
0.1428571428571428
0.35294117647058826
0.5084745762711864
19
2
0.5
0.4285714285714286
0.676470588235294
0.6101694915254237
17
1
0.12500000000000003
0.9047619047619049
0.323529411764706
0.05084745762711865
76
1
0.04166666666666667
0.6666666666666667
0.14705882352941174
0.1016949152542373
101 rows X 6 columns
Testing dataset sample after scaling:
id
species petal_width
sepal_width
sepal_length
petal_length
110
2
0.5
0.4285714285714286
0.3823529411764705
0.4406779661016949
29
3
0.75
0.3333333333333335
0.6176470588235295
0.7288135593220338
116
1
0.08333333333333333
0.4761904761904763
0.14705882352941174
0.06779661016949151
108
3
0.7083333333333334
0.23809523809523814
0.7058823529411765
0.8135593220338982
30
3
0.9166666666666666
0.5714285714285716
0.6176470588235295
0.7288135593220338
28
2
0.5833333333333334
0.38095238095238093
0.6470588235294118
0.6101694915254237
127
1
0.0
0.523809523809524
0.17647058823529427
0.0847457627118644
122
3
0.8750000000000001
0.4761904761904763
0.6470588235294118
0.8135593220338982
101
2
0.375
0.09523809523809534
0.5
0.5084745762711864
26
2
0.625
0.6190476190476191
0.588235294117647
0.6271186440677966
```
19: AutoML in teradataml

```python
18 rows X 6 columns
Total time taken by feature scaling: 42.16 sec
Feature selection using rfe ...
feature selected by RFE:
['petal_length', 'petal_width']
Total time taken by feature selection: 10.35 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_petal_length', 'r_petal_width']
Training dataset sample after scaling:
id
species r_petal_length
r_petal_width
40
3
0.711864406779661
0.7916666666666666
80
1
0.06779661016949151
0.08333333333333333
99
1
0.016949152542372895
0.0
61
3
0.7627118644067796
0.7083333333333334
93
2
0.6949152542372881
0.625
34
3
0.8644067796610169
1.0
78
2
0.5084745762711864
0.5
19
2
0.6101694915254237
0.5
17
1
0.05084745762711865
0.12500000000000003
76
1
0.1016949152542373
0.04166666666666667
101 rows X 4 columns
Testing dataset sample after scaling:
id
species r_petal_length
r_petal_width
110
2
0.4406779661016949
0.5
29
3
0.7288135593220338
0.75
116
1
0.06779661016949151
0.08333333333333333
108
3
0.8135593220338982
0.7083333333333334
30
3
0.7288135593220338
0.9166666666666666
28
2
0.6101694915254237
0.5833333333333334
127
1
0.0847457627118644
0.0
122
3
0.8135593220338982
0.8750000000000001
101
2
0.5084745762711864
0.375
26
2
0.6271186440677966
0.625
```
19: AutoML in teradataml

```python
18 rows X 4 columns
Total time taken by feature scaling: 40.20 sec
scaling Features of pca data ...
columns that will be scaled:
['sepal_length', 'sepal_width', 'petal_length', 'petal_width']
Training dataset sample after scaling:
id
species sepal_length
sepal_width
petal_length
petal_width
95
3
0.7058823529411765
0.6190476190476191
0.7966101694915254
0.8333333333333334
24
3
0.911764705882353
0.38095238095238093
0.8644067796610169
0.75
106
3
0.5588235294117647
0.6666666666666667
0.7457627118644068
0.9166666666666666
35
2
0.5294117647058822
0.38095238095238093
0.6271186440677966
0.4583333333333333
34
3
0.8529411764705882
0.7619047619047621
0.8644067796610169
1.0
112
3
0.8529411764705882
0.4761904761904763
0.8135593220338982
0.625
17
1
0.323529411764706
0.9047619047619049
0.05084745762711865
0.12500000000000003
47
1
0.323529411764706
0.6666666666666667
0.0847457627118644
0.12500000000000003
71
3
0.8823529411764705
0.4285714285714286
0.8983050847457626
0.7083333333333334
63
2
0.7941176470588235
0.5714285714285716
0.6271186440677966
0.5416666666666666
101 rows X 6 columns
Testing dataset sample after scaling:
id
species sepal_length
sepal_width
petal_length
petal_width
27
3
0.588235294117647
0.6666666666666667
0.7796610169491525
0.9583333333333333
30
3
0.6176470588235295
0.5714285714285716
0.7288135593220338
0.9166666666666666
110
2
0.3823529411764705
0.4285714285714286
0.4406779661016949
0.5
31
2
0.411764705882353
0.4285714285714286
0.5423728813559322
0.5
```
19: AutoML in teradataml

```python
29
3
0.6176470588235295
0.3333333333333335
0.7288135593220338
0.75
85
1
0.323529411764706
0.9047619047619049
0.11864406779661016
0.12500000000000003
28
2
0.6470588235294118
0.38095238095238093
0.6101694915254237
0.5833333333333334
108
3
0.7058823529411765
0.23809523809523814
0.8135593220338982
0.7083333333333334
103
3
0.588235294117647
0.4285714285714286
0.7796610169491525
0.7083333333333334
107
2
0.5588235294117647
0.09523809523809534
0.5932203389830508
0.5833333333333334
18 rows X 6 columns
Total time taken by feature scaling: 36.83 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1']
Total time taken by PCA: 11.03 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Starting customized hyperparameter update ...
Completed customized hyperparameter update.
Hyperparameters used for model training:
response_column :
species
name : xgboost
model_type : Classification
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.1, 0.2)
```
19: AutoML in teradataml

```python
max_depth : (3, 4, 5, 6, 7, 8)
min_node_size : (1, 2)
iter_num : (10, 20)
Total number of models for xgboost : 1152
Performing hyperParameter tuning ...
xgboost
Evaluating models performance ...
Evaluation completed.
Leaderboard
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
XGBOOST_0
lasso
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1
2
XGBOOST_1
rfe
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
2
3
XGBOOST_3
lasso
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
3
4
XGBOOST_4
rfe
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
4
5
XGBOOST_7
rfe
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
5
6
XGBOOST_6
lasso
0.944444
0.944444
0.944444
0.944444
0.952381
0.944444
0.944056
0.952381
0.944444
0.944056
6
7
XGBOOST_2
pca
0.888889
0.888889
0.888889
0.888889
0.916667
0.888889
0.885714
```
19: AutoML in teradataml

```python
0.916667
0.888889
0.885714
7
8
XGBOOST_5
pca
0.888889
0.888889
0.888889
0.888889
0.916667
0.888889
0.885714
0.916667
0.888889
0.885714
8 rows X 13 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 19/19
```
5.
* Display model leaderboard.
```python
aml.leaderboard()
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_2
pca
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.0000
1.000000
1.000000
1
2
XGBOOST_3
lasso
0.958333
0.958333
0.958333
0.958333
0.966667
0.958333
0.960234
0.9625
0.958333
0.958041
2
3
XGBOOST_1
rfe
0.958333
0.958333
0.958333
0.958333
0.966667
0.958333
0.960234
0.9625
0.958333
0.958041
3
4
XGBOOST_0
lasso
0.958333
0.958333
0.958333
0.958333
0.966667
0.958333
0.960234
0.9625
0.958333
0.958041
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
XGBOOST_0
lasso
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1
2
XGBOOST_1
rfe
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
2
3
XGBOOST_3
lasso
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
```
19: AutoML in teradataml

```python
1.000000
1.000000
1.000000
3
4
XGBOOST_4
rfe
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
4
5
XGBOOST_7
rfe
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
1.000000
5
6
XGBOOST_6
lasso
0.944444
0.944444
0.944444
0.944444
0.952381
0.944444
0.944056
0.952381
0.944444
0.944056
6
7
XGBOOST_2
pca
0.888889
0.888889
0.888889
0.888889
0.916667
0.888889
0.885714
0.916667
0.888889
0.885714
7
8
XGBOOST_5
pca
0.888889
0.888889
0.888889
0.888889
0.916667
0.888889
0.885714
0.916667
0.888889
0.885714
```
6.
* Display the best performing model.
```python
aml.leader()
RANK  MODEL_ID    FEATURE_SELECTION  ACCURACY   MICRO-PRECISION  MICRO-
RECALL  MICRO-F1   MACRO-PRECISION   MACRO-RECALL   MACRO-F1   WEIGHTED-
PRECISION   WEIGHTED-RECALL   WEIGHTED-F1
1     XGBOOST_2   pca                1.0        1.0
1.0           1.0        1.0               1.0            1.0
1.0                  1.0               1.0
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
XGBOOST_0
lasso
1.0
1.0
1.0
1.0
1.0
1.0
1.0
1.0
1.0
1.0
```
7.
* Display model hyperparameters for trained model.
```python
aml.model_hyperparameters(rank=1)
{'response_column': 'species',
'name': 'xgboost',
'model_type': 'Classification',
'column_sampling': 1,
'min_impurity': 0.0,
'lambda1': 0.01,
'shrinkage_factor': 0.5,
```
19: AutoML in teradataml

```python
'max_depth': 3,
'min_node_size': 1,
'iter_num': 10,
'seed': 42,
'persist': False,
'output_prob': True,
'output_responses': ['1', '3', '2']}
aml.model_hyperparameters(rank=4)
{'response_column': 'species',
'name': 'xgboost',
'model_type': 'Classification',
'column_sampling': 1,
'min_impurity': 0.0,
'lambda1': 0.01,
'shrinkage_factor': 0.5,
'max_depth': 3,
'min_node_size': 1,
'iter_num': 10,
'seed': 42,
'persist': False,
'output_prob': True,
'output_responses': ['1', '3', '2']}
```
8.
* Generate prediction on validation dataset using best performing model.
* Note:
* In the data preparation phase, AutoML generates the validation dataset by splitting the data
* provided during fitting into training and testing sets. AutoML's model training utilizes the training
* data, with the testing data acting as the validation dataset for model evaluation.
```python
prediction = aml.predict()
Following model is being used for generating prediction :
Model ID : XGBOOST_0
Feature Selection Method : lasso
Prediction :
id  Prediction  Confidence_Lower  Confidence_upper  species
0  110           2             0.750             0.750        2
1   29           3             0.750             0.750        3
2  116           1             0.750             0.750        1
```
19: AutoML in teradataml

```python
3  108           3             0.750             0.750        3
4   30           3             1.000             1.000        3
5   28           2             0.625             0.625        2
6  127           1             0.750             0.750        1
7  122           3             1.000             1.000        3
8  101           2             1.000             1.000        2
9   26           2             0.500             0.500        2
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  CLASS_3  Precision  Recall   F1
Support
SeqNum
0               1  CLASS_1        6        0        0        1.0     1.0
1.0        6
2               3  CLASS_3        0        0        6        1.0     1.0
1.0        6
1               2  CLASS_2        0        6        0        1.0     1.0
1.0        6
Confusion Matrix :
array([[6, 0, 0],
[0, 6, 0],
[0, 0, 6]], dtype=int64)
prediction.head()
id
Prediction
Confidence_Lower
Confidence_upper
species
28
2
0.625
0.625
2
30
3
1.0
1.0
3
31
2
0.75
0.75
2
82
1
0.75
0.75
1
101
2
1.0
1.0
2
103
3
1.0
1.0
3
85
1
0.875
0.875
1
29
3
0.75
0.75
3
27
3
1.0
1.0
3
26
2
0.5
0.5
2
```
9.
* Generate prediction on test dataset using best performing model.
```python
prediction = aml.predict(iris_test)
```
19: AutoML in teradataml

```python
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping irrelevent columns :
sepal_length
sepal_width
petal_length
petal_width
species
7.4
2.8
6.1
1.9
3
7.7
3.8
6.7
2.2
3
6.5
2.8
4.6
1.5
2
5.1
2.5
3.0
1.1
2
5.0
2.0
3.5
1.0
2
6.7
3.1
5.6
2.4
3
5.1
3.4
1.5
0.2
1
5.1
3.7
1.5
0.4
1
6.3
3.3
6.0
2.5
3
7.7
2.8
6.7
2.0
3
30 rows X 5 columns
Updated dataset after performing target column transformation :
petal_length
sepal_width
sepal_length
id
petal_width
species
1.5
3.4
5.1
10
0.2
1
3.5
2.0
5.0
14
1.0
2
5.6
3.1
6.7
22
2.4
3
5.0
3.0
6.7
15
1.7
2
6.0
3.3
6.3
9
2.5
3
6.7
2.8
7.7
17
2.0
3
6.1
2.8
7.4
8
1.9
3
5.2
3.0
6.5
16
2.0
3
5.6
3.4
6.3
11
2.4
3
1.6
3.2
4.7
19
0.2
1
30 rows X 6 columns
Performing transformation carried out in data preparation phase ...
Updated dataset after performing Lasso feature selection:
id
sepal_width
petal_length
petal_width
sepal_length
species
17
2.8
6.7
2.0
7.7
3
26
3.4
1.6
0.4
5.0
1
20
2.8
4.6
1.5
6.5
2
```
19: AutoML in teradataml

```python
19
3.2
1.6
0.2
4.7
1
36
2.9
4.3
1.3
6.4
2
28
3.1
4.4
1.4
6.7
2
15
3.0
5.0
1.7
6.7
2
32
2.9
4.3
1.3
6.2
2
38
3.1
1.6
0.2
4.8
1
12
3.8
6.7
2.2
7.7
3
30 rows X 6 columns
Updated dataset after performing scaling on Lasso selected features :
id
species sepal_width
petal_length
petal_width
sepal_length
38
1
0.18558133507591124
-1.2760781036091762
-1.3610226110195
-1.26
87802828607015
17
3
-0.5546351264200517
1.7146359750380966
1.0867259959890987
2.3427729887385853
34
2
-1.541590408414668
-0.0446076006367697
-0.2731343412379006
-0.39702604488845983
15
2
-0.06115748542274311
0.7177312821556723
0.6787678948209989
1.097409791635383
36
2
-0.30789630592139744
0.30724111449820335
0.13482375993019924
0.7238008325044227
28
2
0.18558133507591124
0.3658825670206992
0.270809793652899
1.097409791635383
19
1
0.43232015557456555
-1.2760781036091762
-1.3610226110195
-1.39
33166025710215
30
1
0.43232015557456555
-1.5106439136991585
-1.3610226110195
-1.01
9707643440061
26
1
0.9257977965718731
-1.2760781036091762
-1.0890505435741
-1.01
9707643440061
20
2
-0.5546351264200517
0.4831654720656899
0.40679582737559905
0.8483371522147425
30 rows X 6 columns
Updated dataset after performing RFE feature selection:
id
sepal_length
sepal_width
petal_length
petal_width
species
22
6.7
3.1
5.6
2.4
3
```
19: AutoML in teradataml

```python
36
6.4
2.9
4.3
1.3
2
28
6.7
3.1
4.4
1.4
2
19
4.7
3.2
1.6
0.2
1
38
4.8
3.1
1.6
0.2
1
12
7.7
3.8
6.7
2.2
3
15
6.7
3.0
5.0
1.7
2
32
6.2
2.9
4.3
1.3
2
17
7.7
2.8
6.7
2.0
3
34
5.5
2.4
3.7
1.0
2
30 rows X 6 columns
Updated dataset after performing scaling on RFE selected features :
id
species r_sepal_length
r_sepal_width
r_petal_length
r_petal_width
22
3
1.097409791635383
0.18558133507591124
1.0695799972906452
1.6306701308798983
15
2
1.097409791635383
-0.06115748542274311
0.7177312821556723
0.6787678948209989
32
2
0.474728193083782
-0.30789630592139744
0.30724111449820335
0.13482375993019924
36
2
0.7238008325044227
-0.30789630592139744
0.30724111449820335
0.13482375993019924
38
1
-1.2687802828607015
0.18558133507591124
-1.2760781036091762
-1.3610226110195
12
3
2.3427729887385853
1.9127530785664895
1.7146359750380966
1.3586980634344987
19
1
-1.3933166025710215
0.43232015557456555
-1.2760781036091762
-1.3610226110195
30
1
-1.019707643440061
0.43232015557456555
-1.5106439136991585
-1.3610226110195
17
3
2.3427729887385853
-0.5546351264200517
1.7146359750380966
1.0867259959890987
34
2
-0.39702604488845983
-1.541590408414668
-0.0446076006367697
-0.2731343412379006
30 rows X 6 columns
Updated dataset after performing scaling for PCA feature selection :
id
species sepal_length
sepal_width
petal_length
petal_width
38
1
-1.2687802828606982
0.18558133507591118
-1.2760781036091762
-1.361022611019501
19
1
-1.393316602571018
0.43232015557456543
-1.2760781036091762
-1.361022611019501
```
19: AutoML in teradataml

```python
30
1
-1.0197076434400576
0.43232015557456543
-1.5106439136991585
-1.361022611019501
17
3
2.3427729887385884
-0.5546351264200516
1.7146359750380966
1.0867259959890982
22
3
1.0974097916353864
0.18558133507591118
1.0695799972906452
1.6306701308798979
35
3
-0.023417085757496025
-0.8013739469187048
0.7763727346781676
0.9507399622663981
15
2
1.0974097916353864
-0.061157485422743095
0.7177312821556723
0.6787678948209983
32
2
0.4747281930837853
-0.3078963059213974
0.30724111449820335
0.13482375993019866
26
1
-1.0197076434400576
0.9257977965718729
-1.2760781036091762
-1.089050543574101
20
2
0.8483371522147457
-0.5546351264200516
0.4831654720656899
0.4067958273755985
30 rows X 6 columns
Updated dataset after performing PCA feature selection :
id
col_0
col_1
species
0
26
-2.134501
0.376449
1
1
17
2.979318
0.472867
3
2
22
2.068188
0.711279
3
3
19
-2.352833
-0.238068
1
4
38
-2.222807
-0.419043
1
5
36
0.713335
0.002790
2
6
15
1.389572
0.414597
2
7
20
1.099709
-0.157185
2
8
34
0.019662
-1.592394
2
9
35
1.189404
-0.672774
3
10 rows X 4 columns
Data Transformation completed.⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 15/15
Following model is being picked for evaluation:
Model ID : XGBOOST_2
Feature Selection Method : pca
Prediction :
id  Prediction  species    prob_1    prob_2    prob_3
0  22           3        3  0.001111  0.005453  0.993436
1  38           1        1  0.997480  0.001717  0.000803
2  36           2        2  0.003154  0.986508  0.010337
```
19: AutoML in teradataml

```python
3  15           3        2  0.001111  0.005453  0.993436
4  34           2        2  0.003659  0.987273  0.009068
5  35           3        3  0.001353  0.019395  0.979252
6  20           2        2  0.003516  0.957607  0.038877
7  19           1        1  0.997480  0.001717  0.000803
8  17           3        3  0.001111  0.005453  0.993436
9  26           1        1  0.997480  0.001717  0.000803
Confusion Matrix :
array([[10,  0,  0],
[ 0,  7,  1],
[ 0,  0, 12]], dtype=int64)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping irrelevent columns :
sepal_length
sepal_width
petal_length
petal_width
species
7.7
3.8
6.7
2.2
3
5.9
3.2
4.8
1.8
2
4.6
3.2
1.4
0.2
1
5.1
3.5
1.4
0.2
1
5.7
3.8
1.7
0.3
1
6.8
3.2
5.9
2.3
3
6.7
3.1
5.6
2.4
3
5.8
2.8
5.1
2.4
3
5.9
3.0
5.1
1.8
3
4.6
3.4
1.4
0.3
1
Updated dataset after performing target column transformation :
sepal_length
id
sepal_width
petal_width
petal_length
species
5.5
9
4.2
0.2
1.4
1
5.7
11
3.8
0.3
1.7
1
6.8
19
3.2
2.3
5.9
3
5.9
10
3.0
1.8
5.1
3
5.9
13
3.2
1.8
4.8
2
4.6
21
3.2
0.2
1.4
1
7.7
12
3.8
2.2
6.7
3
5.7
20
2.5
2.0
5.0
3
6.7
14
3.1
2.4
5.6
3
5.8
22
2.8
2.4
5.1
3
Performing transformation carried out in data preparation phase ...
result data stored in table
```
19: AutoML in teradataml

```python
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713849678727662"'
Updated dataset after performing Lasso feature selection:
id
petal_width
sepal_width
sepal_length
petal_length
species
26
0.4
3.4
5.0
1.6
1
22
2.4
2.8
5.8
5.1
3
35
1.6
3.4
6.0
4.5
2
36
1.8
3.0
6.1
4.9
3
17
1.5
3.1
6.7
4.7
2
34
0.2
3.1
4.6
1.5
1
15
0.5
3.3
5.1
1.7
1
32
1.2
3.0
5.7
4.2
2
38
2.1
3.0
7.1
5.9
3
12
2.2
3.8
7.7
6.7
3
Updated dataset after performing scaling on Lasso selected features :
id
species petal_width
sepal_width
sepal_length
petal_length
26
1
0.12500000000000003
0.6666666666666667
0.2058823529411765
0.1016949152542373
17
2
0.5833333333333334
0.523809523809524
0.7058823529411765
0.6271186440677966
34
1
0.04166666666666667
0.523809523809524
0.088235294117647
0.0847457627118644
38
3
0.8333333333333334
0.4761904761904763
0.8235294117647057
0.8305084745762712
36
3
0.7083333333333334
0.4761904761904763
0.5294117647058822
0.6610169491525424
28
2
0.5416666666666666
0.523809523809524
0.7058823529411765
0.576271186440678
19
3
0.9166666666666666
0.5714285714285716
0.7352941176470588
0.8305084745762712
30
2
0.5
0.23809523809523814
0.35294117647058826
0.5084745762711864
15
1
0.16666666666666669
0.6190476190476191
0.23529411764705874
0.11864406779661016
32
2
0.4583333333333333
0.4761904761904763
0.411764705882353
0.5423728813559322
Updated dataset after performing RFE feature selection:
id
petal_length
petal_width
species
26
1.6
0.4
1
17
4.7
1.5
2
34
1.5
0.2
1
```
19: AutoML in teradataml

```python
36
4.9
1.8
3
22
5.1
2.4
3
35
4.5
1.6
2
38
5.9
2.1
3
12
6.7
2.2
3
15
1.7
0.5
1
32
4.2
1.2
2
Updated dataset after performing scaling on RFE selected features :
id
species r_petal_length
r_petal_width
22
3
0.6949152542372881
0.9583333333333333
17
2
0.6271186440677966
0.5833333333333334
34
1
0.0847457627118644
0.04166666666666667
38
3
0.8305084745762712
0.8333333333333334
15
1
0.11864406779661016
0.16666666666666669
32
2
0.5423728813559322
0.4583333333333333
36
3
0.6610169491525424
0.7083333333333334
28
2
0.576271186440678
0.5416666666666666
19
3
0.8305084745762712
0.9166666666666666
30
2
0.5084745762711864
0.5
Updated dataset after performing scaling for PCA feature selection :
id
species sepal_length
sepal_width
petal_length
petal_width
22
3
0.4411764705882352
0.38095238095238093
0.6949152542372881
0.9583333333333333
36
3
0.5294117647058822
0.4761904761904763
0.6610169491525424
0.7083333333333334
28
2
0.7058823529411765
0.523809523809524
0.576271186440678
0.5416666666666666
19
3
0.7352941176470588
0.5714285714285716
0.8305084745762712
0.9166666666666666
17
2
0.7058823529411765
0.523809523809524
0.6271186440677966
0.5833333333333334
34
1
0.088235294117647
0.523809523809524
0.0847457627118644
0.04166666666666667
38
3
0.8235294117647057
0.4761904761904763
0.8305084745762712
0.8333333333333334
12
3
1.0
0.8571428571428572
0.9661016949152542
0.8750000000000001
15
1
0.23529411764705874
0.6190476190476191
0.11864406779661016
0.16666666666666669
32
2
0.411764705882353
0.4761904761904763
0.5423728813559322
0.4583333333333333
```
19: AutoML in teradataml

```python
Updated dataset after performing PCA feature selection :
id
col_0
col_1
species
0
26
-0.552814
-0.064726
1
1
17
0.306241
-0.131285
2
2
22
0.488587
0.103787
3
3
19
0.641246
-0.184527
3
4
38
0.648012
-0.133717
3
5
36
0.333749
-0.014838
3
6
15
-0.493909
-0.034387
1
7
20
0.388955
0.248874
3
8
34
-0.640616
0.115578
1
9
35
0.190191
-0.175907
2
Data Transformation completed.
Following model is being used for generating prediction :
Model ID : XGBOOST_0
Feature Selection Method : lasso
Prediction :
id  Prediction  Confidence_Lower  Confidence_upper  species
0  26           1             0.875             0.875        1
1  36           3             1.000             1.000        3
2  28           2             0.750             0.750        2
3  15           1             0.875             0.875        1
4  17           2             0.625             0.625        2
5  34           1             0.750             0.750        1
6  38           3             1.000             1.000        3
7  12           3             1.000             1.000        3
8  19           3             1.000             1.000        3
9  30           2             1.000             1.000        2
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  CLASS_3  Precision    Recall
F1  Support
SeqNum
0               1  CLASS_1        8        0        0   1.000000  1.000000
1.000000        8
2               3  CLASS_3        0        1       11   0.916667  1.000000
0.956522       11
1               2  CLASS_2        0       10        0   1.000000  0.909091
0.952381       11
Confusion Matrix :
array([[ 8,  0,  0],
```
19: AutoML in teradataml

```python
[ 0, 10,  1],
[ 0,  0, 11]], dtype=int64)
prediction.head()
id
Prediction
species prob_1
prob_2
prob_3
10
1
1
0.9974796820350456
0.001716876719969664
0.0008034412449846343
12
3
3
0.0011112337307529446
0.005452541701712984
0.9934362245675341
13
2
2
0.002354870676477184
0.6354616937763028
0.36218343554722005
14
2
2
0.003658600107511803
0.9872729931172249
0.009068406775263282
16
3
3
0.0011112337307529446
0.005452541701712984
0.9934362245675341
17
3
3
0.0011112337307529446
0.005452541701712984
0.9934362245675341
15
3
2
0.0011112337307529446
0.005452541701712984
0.9934362245675341
11
3
3
0.0011112337307529446
0.005452541701712984
0.9934362245675341
9
3
3
0.0011112337307529446
0.005452541701712984
0.9934362245675341
8
3
3
0.0011112337307529446
0.005452541701712984
0.9934362245675341
id
Prediction
Confidence_Lower
Confidence_upper
species
10
3
0.875
0.875
3
12
3
1.0
1.0
3
13
3
0.875
0.875
2
14
3
1.0
1.0
3
16
2
0.75
0.75
2
17
2
0.625
0.625
2
15
1
0.875
0.875
1
11
1
0.875
0.875
1
9
1
0.875
0.875
1
8
1
0.875
0.875
1
```
10. Generate evaluation metrics on test dataset using best performing model.
```python
performance_metrics = aml.evaluate(iris_test)
```
19: AutoML in teradataml

```python
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : XGBOOST_2
Feature Selection Method : pca
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  CLASS_3  Precision  Recall
F1  Support
SeqNum
0               1  CLASS_1       10        0        0   1.000000   1.000
1.000000       10
2               3  CLASS_3        0        1       12   0.923077   1.000
0.960000       12
1               2  CLASS_2        0        7        0   1.000000   0.875
0.933333        8
SeqNum              Metric  MetricValue
0       3        Micro-Recall     0.966667
1       5     Macro-Precision     0.974359
2       6        Macro-Recall     0.958333
3       7            Macro-F1     0.964444
4       9     Weighted-Recall     0.966667
5      10         Weighted-F1     0.966222
6       8  Weighted-Precision     0.969231
7       4            Micro-F1     0.966667
8       2     Micro-Precision     0.966667
9       1            Accuracy     0.966667
performance_metrics
SeqNum
Prediction
Mapping CLASS_1 CLASS_2 CLASS_3 Precision
Recall
F1
Support
0
1
CLASS_1 10
0
0
1.0
1.0
1.0
10
2
3
CLASS_3 0
1
12
0.9230769230769231
1.0
0.9600000000000001
12
1
2
CLASS_2 0
7
0
1.0
0.875
0.9333333333333333
8
```
11. Generate prediction on test dataset using second best performing model.
```python
prediction = aml.predict(iris_test,2)
```
19: AutoML in teradataml

```python
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : XGBOOST_3
Feature Selection Method : lasso
Prediction :
id  Prediction  species    prob_1    prob_2    prob_3
0  38           1        1  0.998938  0.000531  0.000531
1  36           2        2  0.000962  0.998226  0.000813
2  28           2        2  0.000935  0.998252  0.000813
3  15           3        2  0.000871  0.002996  0.996133
4  17           3        3  0.000548  0.000505  0.998947
5  34           2        2  0.000818  0.998369  0.000813
6  19           1        1  0.998938  0.000531  0.000531
7  30           1        1  0.998938  0.000531  0.000531
8  26           1        1  0.998938  0.000531  0.000531
9  20           2        2  0.000942  0.998245  0.000813
Confusion Matrix :
array([[10,  0,  0],
[ 0,  7,  1],
[ 0,  0, 12]], dtype=int64)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping irrelevent columns :
sepal_length
sepal_width
petal_length
petal_width
species
5.5
4.2
1.4
0.2
1
7.7
3.8
6.7
2.2
3
5.7
2.5
5.0
2.0
3
6.7
3.1
5.6
2.4
3
5.9
3.2
4.8
1.8
2
4.6
3.2
1.4
0.2
1
5.7
3.8
1.7
0.3
1
6.8
3.2
5.9
2.3
3
5.9
3.0
5.1
1.8
3
4.6
3.4
1.4
0.3
1
Updated dataset after performing target column transformation :
sepal_length
id
sepal_width
petal_width
petal_length
species
```
19: AutoML in teradataml

```python
5.9
13
3.2
1.8
4.8
2
6.7
14
3.1
2.4
5.6
3
5.8
22
2.8
2.4
5.1
3
5.1
8
3.5
0.2
1.4
1
5.9
10
3.0
1.8
5.1
3
4.6
18
3.4
0.3
1.4
1
7.7
12
3.8
2.2
6.7
3
5.7
20
2.5
2.0
5.0
3
5.7
11
3.8
0.3
1.7
1
6.8
19
3.2
2.3
5.9
3
Performing transformation carried out in data preparation phase ...
result data stored in table
'"AUTOML_USER"."ml__td_sqlmr_persist_out__1713844745578509"'
Updated dataset after performing Lasso feature selection:
id
petal_width
sepal_width
sepal_length
petal_length
species
19
2.3
3.2
6.8
5.9
3
22
2.4
2.8
5.8
5.1
3
35
1.6
3.4
6.0
4.5
2
26
0.4
3.4
5.0
1.6
1
36
1.8
3.0
6.1
4.9
3
28
1.4
3.1
6.7
4.4
2
15
0.5
3.3
5.1
1.7
1
32
1.2
3.0
5.7
4.2
2
38
2.1
3.0
7.1
5.9
3
12
2.2
3.8
7.7
6.7
3
Updated dataset after performing scaling on Lasso selected features :
id
species petal_width
sepal_width
sepal_length
petal_length
17
2
0.5833333333333334
0.523809523809524
0.7058823529411765
0.6271186440677966
19
3
0.9166666666666666
0.5714285714285716
0.7352941176470588
0.8305084745762712
30
2
0.5
0.23809523809523814
0.35294117647058826
0.5084745762711864
15
1
0.16666666666666669
0.6190476190476191
0.23529411764705874
0.11864406779661016
26
1
0.12500000000000003
0.6666666666666667
0.2058823529411765
0.1016949152542373
20
3
0.7916666666666666
0.23809523809523814
0.411764705882353
0.6779661016949152
36
3
0.7083333333333334
0.4761904761904763
0.5294117647058822
0.6610169491525424
```
19: AutoML in teradataml

```python
28
2
0.5416666666666666
0.523809523809524
0.7058823529411765
0.576271186440678
38
3
0.8333333333333334
0.4761904761904763
0.8235294117647057
0.8305084745762712
12
3
0.8750000000000001
0.8571428571428572
1.0
0.9661016949152542
Updated dataset after performing RFE feature selection:
id
petal_length
petal_width
species
26
1.6
0.4
1
22
5.1
2.4
3
35
4.5
1.6
2
36
4.9
1.8
3
19
5.9
2.3
3
30
4.0
1.3
2
15
1.7
0.5
1
32
4.2
1.2
2
38
5.9
2.1
3
12
6.7
2.2
3
Updated dataset after performing scaling on RFE selected features :
id
species r_petal_length
r_petal_width
22
3
0.6949152542372881
0.9583333333333333
38
3
0.8305084745762712
0.8333333333333334
12
3
0.9661016949152542
0.8750000000000001
17
2
0.6271186440677966
0.5833333333333334
19
3
0.8305084745762712
0.9166666666666666
30
2
0.5084745762711864
0.5
15
1
0.11864406779661016
0.16666666666666669
32
2
0.5423728813559322
0.4583333333333333
36
3
0.6610169491525424
0.7083333333333334
28
2
0.576271186440678
0.5416666666666666
Updated dataset after performing scaling for PCA feature selection :
id
species sepal_length
sepal_width
petal_length
petal_width
17
2
0.7058823529411765
0.523809523809524
0.6271186440677966
0.5833333333333334
15
1
0.23529411764705874
0.6190476190476191
0.11864406779661016
0.16666666666666669
32
2
0.411764705882353
0.4761904761904763
0.5423728813559322
0.4583333333333333
36
3
0.5294117647058822
0.4761904761904763
0.6610169491525424
0.7083333333333334
22
3
0.4411764705882352
0.38095238095238093
```
19: AutoML in teradataml

```python
0.6949152542372881
0.9583333333333333
35
2
0.5
0.6666666666666667
0.5932203389830508
0.625
19
3
0.7352941176470588
0.5714285714285716
0.8305084745762712
0.9166666666666666
30
2
0.35294117647058826
0.23809523809523814
0.5084745762711864
0.5
38
3
0.8235294117647057
0.4761904761904763
0.8305084745762712
0.8333333333333334
12
3
1.0
0.8571428571428572
0.9661016949152542
0.8750000000000001
Updated dataset after performing PCA feature selection :
id
col_0
col_1
species
0
26
-0.552814
-0.064726
1
1
17
0.306241
-0.131285
2
2
22
0.488587
0.103787
3
3
19
0.641246
-0.184527
3
4
38
0.648012
-0.133717
3
5
36
0.333749
-0.014838
3
6
15
-0.493909
-0.034387
1
7
20
0.388955
0.248874
3
8
34
-0.640616
0.115578
1
9
35
0.190191
-0.175907
2
Data Transformation completed.
Following model is being used for generating prediction :
Model ID : XGBOOST_1
Feature Selection Method : rfe
Prediction :
id  Prediction  Confidence_Lower  Confidence_upper  species
0  17           2             0.625             0.625        2
1  38           3             1.000             1.000        3
2  12           3             1.000             1.000        3
3  36           2             0.500             0.500        3
4  22           3             0.875             0.875        3
5  35           2             0.625             0.625        2
6  19           3             1.000             1.000        3
7  30           2             1.000             1.000        2
8  15           1             0.875             0.875        1
9  32           2             1.000             1.000        2
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  CLASS_3  Precision    Recall
```
19: AutoML in teradataml

```python
F1  Support
SeqNum
0               1  CLASS_1        8        0        0   1.000000  1.000000
1.000000        8
2               3  CLASS_3        0        0       10   1.000000  0.909091
0.952381       11
1               2  CLASS_2        0       11        1   0.916667  1.000000
0.956522       11
Confusion Matrix :
array([[ 8,  0,  0],
[ 0, 11,  0],
[ 0,  1, 10]], dtype=int64)
prediction.head()
id
Prediction
species prob_1
prob_2
prob_3
10
1
1
0.9989377670822677
0.0005312951795478172
0.000530937738184492
12
3
3
0.0006801883644343667
0.0009692414607328684
0.9983505701748328
13
2
2
0.0008180359310818579
0.9983690346735865
0.0008129293953315769
14
2
2
0.0008180359310818579
0.9983690346735865
0.0008129293953315769
16
3
3
0.000543436036706532
0.0005265372144978478
0.9989300267487955
17
3
3
0.0005478504971630089
0.0005047187958750853
0.9989474307069619
15
3
2
0.0008712061087393717
0.0029962703677184187
0.9961325235235423
11
3
3
0.0006801883644343667
0.0009692414607328684
0.9983505701748328
9
3
3
0.0006801883644343667
0.0009692414607328684
0.9983505701748328
8
3
3
0.0005478504971630089
0.0005047187958750853
0.9989474307069619
id
Prediction
Confidence_Lower
Confidence_upper
species
10
3
0.875
0.875
3
12
3
1.0
1.0
3
13
2
0.5
0.5
2
14
3
1.0
1.0
3
```
19: AutoML in teradataml

```python
16
2
1.0
1.0
2
17
2
0.625
0.625
2
15
1
0.875
0.875
1
11
1
0.875
0.875
1
9
1
0.875
0.875
1
8
1
0.875
0.875
1
```
12. Generate evaluation metrics on test dataset using second best performing model.
```python
performance_metrics = aml.evaluate(iris_test,2)
Skipping data transformation as data is already transformed.
Following model is being picked for evaluation:
Model ID : XGBOOST_3
Feature Selection Method : lasso
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  CLASS_3  Precision  Recall
F1  Support
SeqNum
0               1  CLASS_1       10        0        0   1.000000   1.000
1.000000       10
2               3  CLASS_3        0        1       12   0.923077   1.000
0.960000       12
1               2  CLASS_2        0        7        0   1.000000   0.875
0.933333        8
SeqNum              Metric  MetricValue
0       3        Micro-Recall     0.966667
1       5     Macro-Precision     0.974359
2       6        Macro-Recall     0.958333
3       7            Macro-F1     0.964444
4       9     Weighted-Recall     0.966667
5      10         Weighted-F1     0.966222
6       8  Weighted-Precision     0.969231
7       4            Micro-F1     0.966667
8       2     Micro-Precision     0.966667
9       1            Accuracy     0.966667
performance_metrics
```
19: AutoML in teradataml

```python
SeqNum
Prediction
Mapping CLASS_1 CLASS_2 CLASS_3 Precision
Recall
F1
Support
2
3
CLASS_3      0
1      12   0.92307
1.0
0.9600000000000001
12
1
2
CLASS_2      0
7       0       1.0
0.875
0.9333333333333333
8
0
1
CLASS_1      10       0       0       1.0
1.0
1.0
10
```
### Example 7: Run Trained AutoML Models across Multiple
### Sessions using deploy and load APIs
This example runs AutoML to obtain the best models, deploy them in the database, and subsequently load
the models. Utilize the loaded models to predict the survival of passengers aboard the RMS Titanic based
on various factors and assess the performance of the model.
* These methods are applicable for all three APIs i.e., AutoML, AutoRegressor and AutoClassifer.
* Use early stopping timer to 360 sec.
* Opt for verbose level 2 to get detailed log.
1.
* Load the titanic data and create teradataml DataFrame.
```python
load_example_data('teradataml','titanic')
df = DataFrame('titanic')
```
2.
* Create an AutoML instance.
```python
aml = AutoML(task_type="Classification",
max_runtime_secs=360,
verbose=2)
```
3.
* Fit the data.
```python
aml.fit(df, df.survived)
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 891
Total Columns in the data: 12
Column Summary:
```
19: AutoML in teradataml

```python
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
name
VARCHAR(1000) CHARACTER SET LATIN
891
0
0
None
None
None
0.0
100.0
ticket
VARCHAR(20) CHARACTER SET LATIN 891
0
0
None
None
None
0.0
100.0
passenger
INTEGER 891
0
None
0
891
0
0.0
100.0
sibsp
INTEGER 891
0
None
608
283
0
0.0
100.0
parch
INTEGER 891
0
None
678
213
0
0.0
100.0
fare
FLOAT
891
0
None
15
876
0
0.0
100.0
cabin
VARCHAR(20) CHARACTER SET LATIN 204
687
0
None
None
None
77.10437710437711
22.895622895622896
embarked
VARCHAR(20) CHARACTER SET LATIN 889
2
0
None
None
None
0.2244668911335578
99.77553310886644
sex
VARCHAR(20) CHARACTER SET LATIN 891
0
0
None
None
None
0.0
100.0
age
INTEGER 714
177
None
7
707
0
19.865319865319865
80.13468013468014
pclass
INTEGER 891
0
None
0
891
0
0.0
100.0
survived
INTEGER 891
0
None
549
342
0
0.0
100.0
passenger  survived   pclass      age    sibsp    parch     fare
func
min        1.000     0.000    1.000    0.000    0.000    0.000    0.000
std      257.354     0.487    0.836   14.536    1.103    0.806   49.693
25%      223.500     0.000    2.000   20.000    0.000    0.000    7.910
50%      446.000     0.000    3.000   28.000    0.000    0.000   14.454
75%      668.500     1.000    3.000   38.000    1.000    0.000   31.000
max      891.000     1.000    3.000   80.000    8.000    6.000  512.329
mean     446.000     0.384    2.309   29.679    0.523    0.382   32.204
count    891.000   891.000  891.000  714.000  891.000  891.000  891.000
Statistics of Data:
func
passenger
survived
pclass
age
sibsp
parch
fare
50%
446
0
3
28
0
0
14.454
count
891
891
891
714
891
891
891
mean
446
0.384
2.309
29.679
0.523
0.382
32.204
min
1
0
1
0
0
0
0
max
891
1
3
80
8
6
512.329
75%
668.5
1
3
38
1
0
31
25%
223.5
0
2
20
0
0
7.91
```
19: AutoML in teradataml

```python
std
257.354 0.487
0.836
14.536
1.103
0.806
49.693
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
name                      891
sex                       2
ticket                    681
cabin                     147
embarked                  3
Futile columns in dataset:
ColumnName
ticket
name
Install seaborn and matplotlib libraries to visualize the data.
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0      parch          23.905724
1      sibsp           5.162738
2        age          20.763187
3       fare          13.019080
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.32 sec
Handling less significant features from data ...
Removing Futile columns:
['ticket', 'name']
Sample of Data after removing Futile columns:
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
326
1
1
female
36
0
0
135.6333
C32
C
12
591
0
3
male
35
0
0
7.125
None
S
```
19: AutoML in teradataml

```python
11
387
0
3
male
1
5
2
46.9
None
S
15
734
0
2
male
23
0
0
13.0
None
S
6
162
1
2
female
40
0
0
15.75
None
S
14
265
0
3
female
None
0
0
7.75
None
Q
5
530
0
2
male
23
2
1
11.5
None
S
9
244
0
3
male
22
0
0
7.125
None
S
13
40
1
3
female
14
1
0
11.2417 None
C
10
122
0
3
male
None
0
0
8.05
None
S
7
891 rows X 11 columns
Total time to handle less significant features: 16.75 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates. No
action needed.
Total time to handle date features: 0.00 sec
Checking Missing values in dataset ...
Columns with their missing values:
embarked: 2
age: 177
cabin: 687
Deleting rows of these columns for handling missing values:
['embarked']
Sample of dataset after removing 2 rows:
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
244
0
3
male
22
0
0
7.125
None
S
13
40
1
3
female
14
1
0
11.2417 None
C
```
19: AutoML in teradataml

```python
10
162
1
2
female
40
0
0
15.75
None
S
14
122
0
3
male
None
0
0
8.05
None
S
7
387
0
3
male
1
5
2
46.9
None
S
15
469
0
3
male
None
0
0
7.725
None
Q
4
61
0
3
male
22
0
0
7.2292
None
C
8
326
1
1
female
36
0
0
135.6333
C32
C
12
591
0
3
male
35
0
0
7.125
None
S
11
734
0
2
male
23
0
0
13.0
None
S
6
889 rows X 11 columns
Dropping these columns for handling missing values:
['cabin']
Sample of dataset after removing 1 columns:
passenger
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
387
0
3
male
1
5
2
46.9
S
15
530
0
2
male
23
2
1
11.5
S
9
244
0
3
male
22
0
0
7.125
S
13
734
0
2
male
23
0
0
13.0
S
6
162
1
2
female
40
0
0
15.75
S
14
469
0
3
male
None
0
0
7.725
Q
4
61
0
3
male
22
0
0
7.2292
C
8
326
1
1
female
36
0
0
135.6333
C
12
40
1
3
female
14
1
0
11.2417 C
10
265
0
3
female
None
0
0
7.75
Q
5
889 rows X 10 columns
Total time to find missing values in data: 13.32 sec
Imputing Missing Values ...
```
19: AutoML in teradataml

```python
Columns with their imputation method:
age: mean
Sample of dataset after Imputation:
passenger
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
387
0
3
male
1
5
2
46.9
S
15
530
0
2
male
23
2
1
11.5
S
9
244
0
3
male
22
0
0
7.125
S
13
469
0
3
male
29
0
0
7.725
Q
4
326
1
1
female
36
0
0
135.6333
C
12
734
0
2
male
23
0
0
13.0
S
6
40
1
3
female
14
1
0
11.2417 C
10
162
1
2
female
40
0
0
15.75
S
14
61
0
3
male
22
0
0
7.2292
C
8
265
0
3
female
29
0
0
7.75
Q
5
889 rows X 10 columns
Time taken to perform imputation: 14.37 sec
Performing encoding for categorical columns ...
ONE HOT Encoding these Columns:
['sex', 'embarked']
Sample of dataset after performing one hot encoding:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
244
0
3
0
1
22
0
0
7.125
0
0
1
13
101
0
3
1
0
28
0
0
7.8958
0
0
1
21
570
1
3
0
1
32
0
0
7.8542
0
0
1
25
835
0
3
0
1
18
0
0
8.3
0
0
1
29
692
1
3
1
0
4
0
1
13.4167 1
0
0
37
284
1
3
0
1
19
0
0
8.05
0
0
1
41
427
1
2
1
0
28
1
0
26.0
0
0
1
33
```
19: AutoML in teradataml

```python
305
0
3
0
1
29
0
0
8.05
0
0
1
17
530
0
2
0
1
23
2
1
11.5
0
0
1
9
265
0
3
1
0
29
0
0
7.75
0
1
0
5
889 rows X 13 columns
Time taken to encode the columns: 16.34 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Data preparation started ...
Outlier preprocessing ...
Columns with outlier percentage :-
ColumnName  OutlierPercentage
0      sibsp           5.174353
1       fare          12.823397
2      parch          23.959505
3        age           7.311586
Deleting rows of these columns:
['sibsp', 'age']
Sample of dataset after removing outlier rows:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
244
0
3
0
1
22
0
0
7.125
0
0
1
13
101
0
3
1
0
28
0
0
7.8958
0
0
1
21
570
1
3
0
1
32
0
0
7.8542
0
0
1
25
835
0
3
0
1
18
0
0
8.3
0
0
1
29
692
1
3
1
0
4
0
1
13.4167 1
0
0
37
284
1
3
0
1
19
0
0
8.05
0
0
1
41
```
19: AutoML in teradataml

```python
427
1
2
1
0
28
1
0
26.0
0
0
1
33
305
0
3
0
1
29
0
0
8.05
0
0
1
17
530
0
2
0
1
23
2
1
11.5
0
0
1
9
265
0
3
1
0
29
0
0
7.75
0
1
0
5
785 rows X 13 columns
median inplace of outliers:
['fare', 'parch']
Sample of dataset after performing MEDIAN inplace:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
244
0
3
0
1
22
0
0
7.125
0
0
1
13
101
0
3
1
0
28
0
0
7.8958
0
0
1
21
570
1
3
0
1
32
0
0
7.8542
0
0
1
25
835
0
3
0
1
18
0
0
8.3
0
0
1
29
692
1
3
1
0
4
0
0
13.4167 1
0
0
37
284
1
3
0
1
19
0
0
8.05
0
0
1
41
427
1
2
1
0
28
1
0
26.0
0
0
1
33
305
0
3
0
1
29
0
0
8.05
0
0
1
17
530
0
2
0
1
23
2
0
11.5
0
0
1
9
265
0
3
1
0
29
0
0
7.75
0
1
0
5
785 rows X 13 columns
Time Taken by Outlier processing: 42.46 sec
Checking imbalance data ...
```
19: AutoML in teradataml

```python
Imbalance Not Found.
Feature selection using lasso ...
feature selected by lasso:
['fare', 'embarked_1', 'sex_0', 'embarked_2', 'sex_1', 'passenger', 'age',
'sibsp', 'embarked_0', 'pclass']
Total time taken by feature selection: 3.47 sec
scaling Features of lasso data ...
columns that will be scaled:
['fare', 'passenger', 'age', 'sibsp', 'pclass']
Dataset sample after scaling:
embarked_1
sex_0
embarked_2
sex_1
id
embarked_0
survived
fare
passenger
age
sibsp
pclass
0
0
1
1
6
0
0
0.22807017543859648
0.8235955056179776
0.39215686274509803
0.0
0.5
0
0
0
1
8
1
0
0.1268280701754386
0.06741573033707865
0.37254901960784315
0.0
1.0
0
0
1
1
9
0
0
0.20175438596491227
0.5943820224719101
0.39215686274509803
1.0
0.5
0
1
0
0
10
1
1
0.19722280701754386
0.043820224719101124
0.21568627450980393
0.5
1.0
0
1
0
0
12
1
1
0.22807017543859648
0.3651685393258427
0.6470588235294118
0.0
0.0
0
0
1
1
13
0
0
0.125
0.27303370786516856
0.37254901960784315
0.0
1.0
0
0
1
1
11
0
0
0.125
0.6629213483146067
0.6274509803921569
0.0
1.0
0
0
1
1
7
0
0
0.14122807017543862
0.13595505617977527
0.5098039215686274
0.0
1.0
1
1
0
0
5
0
0
0.13596491228070176
0.2966292134831461
0.5098039215686274
0.0
1.0
1
0
0
1
4
0
0
0.1355263157894737
0.5258426966292135
0.5098039215686274
0.0
1.0
785 rows X 12 columns
Total time taken by feature scaling: 28.07 sec
Feature selection using rfe ...
```
19: AutoML in teradataml

```python
feature selected by RFE:
['sex_0', 'embarked_2', 'sex_1', 'passenger', 'age', 'sibsp', 'embarked_0',
'pclass', 'fare']
Total time taken by feature selection: 13.76 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_passenger', 'r_age', 'r_sibsp', 'r_pclass', 'r_fare']
Dataset sample after scaling:
r_sex_0 r_embarked_0
r_sex_1 r_embarked_2
id
survived
r_passenger
r_age
r_sibsp r_pclass
r_fare
0
0
1
1
6
0
0.8235955056179776
0.39215686274509803
0.0
0.5
0.22807017543859648
0
1
1
0
8
0
0.06741573033707865
0.37254901960784315
0.0
1.0
0.1268280701754386
0
0
1
1
9
0
0.5943820224719101
0.39215686274509803
1.0
0.5
0.20175438596491227
1
1
0
0
10
1
0.043820224719101124
0.21568627450980393
0.5
1.0
0.19722280701754386
1
1
0
0
12
1
0.3651685393258427
0.6470588235294118
0.0
0.0
0.22807017543859648
0
0
1
1
13
0
0.27303370786516856
0.37254901960784315
0.0
1.0
0.125
0
0
1
1
11
0
0.6629213483146067
0.6274509803921569
0.0
1.0
0.125
0
0
1
1
7
0
0.13595505617977527
0.5098039215686274
0.0
1.0
0.14122807017543862
1
0
0
0
5
0
0.2966292134831461
0.5098039215686274
0.0
1.0
0.13596491228070176
0
0
1
0
4
0
0.5258426966292135
0.5098039215686274
0.0
1.0
0.1355263157894737
785 rows X 11 columns
Total time taken by feature scaling: 27.81 sec
scaling Features of pca data ...
columns that will be scaled:
['passenger', 'pclass', 'age', 'sibsp', 'fare']
```
19: AutoML in teradataml

```python
Dataset sample after scaling:
parch
embarked_1
sex_0
embarked_2
sex_1
id
embarked_0
survived
passenger
pclass
age
sibsp
fare
0
0
1
0
0
12
1
1
0.3651685393258427
0.0
0.6470588235294118
0.0
0.22807017543859648
0
0
1
0
0
10
1
1
0.043820224719101124
1.0
0.21568627450980393
0.5
0.19722280701754386
0
0
1
1
0
14
0
1
0.18089887640449437
0.5
0.7254901960784313
0.0
0.27631578947368424
0
1
1
0
0
5
0
0
0.2966292134831461
1.0
0.5098039215686274
0.0
0.13596491228070176
0
0
0
1
1
13
0
0
0.27303370786516856
1.0
0.37254901960784315
0.0
0.125
0
0
0
1
1
7
0
0
0.13595505617977527
1.0
0.5098039215686274
0.0
0.14122807017543862
0
0
0
1
1
11
0
0
0.6629213483146067
1.0
0.6274509803921569
0.0
0.125
0
0
1
1
0
19
0
1
0.9606741573033708
1.0
0.29411764705882354
0.0
0.16403508771929823
0
0
0
1
1
9
0
0
0.5943820224719101
0.5
0.39215686274509803
1.0
0.20175438596491227
0
0
0
1
1
6
0
0
0.8235955056179776
0.5
0.39215686274509803
0.0
0.22807017543859648
785 rows X 13 columns
Total time taken by feature scaling: 25.60 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5']
Total time taken by PCA: 5.56 sec
```
19: AutoML in teradataml

```python
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Model Training started ...
Hyperparameters used for model training:
response_column : survived
name : decision_forest
tree_type : Classification
min_impurity : (0.0, 0.1, 0.2)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
num_trees : (-1, 20, 30)
seed : 42
Total number of models for decision_forest : 108
response_column : survived
name : xgboost
model_type : Classification
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1, 0.2)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.1, 0.3)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
iter_num : (10, 20, 30)
seed : 42
Total number of models for xgboost : 2592
response_column : survived
name : knn
model_type : Classification
k : (3, 5, 6, 8, 10, 12)
id_column : id
voting_weight : 1.0
Total number of models for knn : 6
```
19: AutoML in teradataml

```python
response_column : survived
name : svm
model_type : Classification
lambda1 : (0.001, 0.02, 0.1)
alpha : (0.15, 0.85)
tolerance : (0.001, 0.01)
learning_rate : OPTIMAL
initial_eta : (0.05, 0.1)
momentum : (0.65, 0.8, 0.95)
nesterov : True
intercept : True
iter_num_no_change : (5, 10, 50)
local_sgd_iterations  : (10, 20)
iter_max : (300, 200, 400)
batch_size : (10, 50, 60, 80)
Total number of models for svm : 5184
response_column : survived
name : glm
family : BINOMIAL
lambda1 : (0.001, 0.02, 0.1)
alpha : (0.15, 0.85)
learning_rate : OPTIMAL
initial_eta : (0.05, 0.1)
momentum : (0.65, 0.8, 0.95)
iter_num_no_change : (5, 10, 50)
iter_max : (300, 200, 400)
batch_size : (10, 50, 60, 80)
Total number of models for glm : 1296
Performing hyperparameter tuning ...
decision_forest
```
19: AutoML in teradataml

```python
xgboost
knn
svm
glm
Leaderboard
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_1
rfe
0.828025
0.828025
0.828025
0.828025
0.821774
0.815874
0.818482
0.827064
0.828025
0.827230
1
2
XGBOOST_3
lasso
0.821656
0.821656
0.821656
0.821656
0.814347
0.810611
0.812329
0.820866
0.821656
0.821123
2
3
XGBOOST_0
lasso
0.821656
0.821656
0.821656
0.821656
0.814347
0.810611
0.812329
0.820866
0.821656
0.821123
3
4
DECISIONFOREST_0
lasso
0.815287
0.815287
0.815287
0.815287
0.809737
0.799745
0.803792
0.813996
0.815287
0.813774
4
5
DECISIONFOREST_1
rfe
0.815287
0.815287
0.815287
0.815287
0.809737
0.799745
0.803792
0.813996
0.815287
0.813774
5
6
XGBOOST_2
pca
0.796178
0.796178
0.796178
0.796178
0.800745
0.767148
0.776114
0.798275
0.796178
0.790202
```
19: AutoML in teradataml

```python
6
7
KNN_9
lasso
0.789809
0.789809
0.789809
0.789809
0.788383
0.764686
0.771893
0.789239
0.789809
0.785330
7
8
SVM_1
rfe
0.789809
0.789809
0.789809
0.789809
0.799504
0.756282
0.766084
0.794729
0.789809
0.781743
8
9
SVM_0
lasso
0.789809
0.789809
0.789809
0.789809
0.799504
0.756282
0.766084
0.794729
0.789809
0.781743
9
10
GLM_3
lasso
0.783439
0.783439
0.783439
0.783439
0.774289
0.781834
0.776839
0.788668
0.783439
0.784906
10
11
KNN_4
rfe
0.783439
0.783439
0.783439
0.783439
0.782523
0.756621
0.764056
0.783054
0.783439
0.778271
11
12
KNN_0
lasso
0.783439
0.783439
0.783439
0.783439
0.782523
0.756621
0.764056
0.783054
0.783439
0.778271
12
13
GLM_1
rfe
0.783439
0.783439
0.783439
0.783439
0.799275
0.745416
0.755586
0.792117
0.783439
0.772929
13
14
GLM_0
lasso
0.783439
0.783439
0.783439
0.783439
0.799275
0.745416
0.755586
0.792117
0.783439
0.772929
14
15
SVM_2
pca
0.783439
0.783439
0.783439
0.783439
0.805342
0.742615
0.753145
0.795955
0.783439
0.771322
15
16
SVM_3
lasso
0.770701
0.770701
0.770701
0.770701
0.763807
0.774109
0.765672
0.781306
0.770701
0.772887
16
17
KNN_8
pca
0.770701
0.770701
0.770701
0.770701
0.770738
0.740492
0.748128
0.770718
0.770701
0.763977
17
18
DECISIONFOREST_3
lasso
0.770701
0.770701
0.770701
0.770701
0.778258
0.734890
0.743650
0.774644
0.770701
0.761153
18
19
GLM_2
pca
0.757962
0.757962
0.757962
0.757962
0.784435
0.710357
0.718159
0.774157
0.757962
0.740422
19
20
DECISIONFOREST_2
pca
0.751592
0.751592
0.751592
0.751592
0.740877
0.733107
0.736134
0.749100
0.751592
0.749558
20 rows X 13 columns
```
19: AutoML in teradataml

```python
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation -> 4.
Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜
100% - 18/18
```
4.
* Display leaderboard.
```python
aml.leaderboard()
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_1
rfe
0.828025
0.828025
0.828025
0.828025
0.821774
0.815874
0.818482
0.827064
0.828025
0.827230
1
2
XGBOOST_3
lasso
0.821656
0.821656
0.821656
0.821656
0.814347
0.810611
0.812329
0.820866
0.821656
0.821123
2
3
XGBOOST_0
lasso
0.821656
0.821656
0.821656
0.821656
0.814347
0.810611
0.812329
0.820866
0.821656
0.821123
3
4
DECISIONFOREST_0
lasso
0.815287
0.815287
0.815287
0.815287
0.809737
0.799745
0.803792
0.813996
0.815287
0.813774
4
5
DECISIONFOREST_1
rfe
0.815287
0.815287
0.815287
0.815287
0.809737
0.799745
0.803792
0.813996
0.815287
0.813774
5
6
XGBOOST_2
pca
0.796178
0.796178
0.796178
0.796178
0.800745
0.767148
0.776114
0.798275
0.796178
0.790202
6
7
KNN_9
lasso
0.789809
0.789809
0.789809
0.789809
0.788383
0.764686
0.771893
0.789239
0.789809
0.785330
7
8
SVM_1
rfe
0.789809
0.789809
0.789809
0.789809
0.799504
0.756282
0.766084
0.794729
0.789809
0.781743
8
9
SVM_0
lasso
0.789809
0.789809
0.789809
0.789809
0.799504
0.756282
0.766084
0.794729
0.789809
0.781743
9
10
GLM_3
lasso
0.783439
0.783439
0.783439
0.783439
0.774289
0.781834
0.776839
0.788668
0.783439
0.784906
10
11
KNN_4
rfe
0.783439
0.783439
0.783439
```
19: AutoML in teradataml

```python
0.783439
0.782523
0.756621
0.764056
0.783054
0.783439
0.778271
11
12
KNN_0
lasso
0.783439
0.783439
0.783439
0.783439
0.782523
0.756621
0.764056
0.783054
0.783439
0.778271
12
13
GLM_1
rfe
0.783439
0.783439
0.783439
0.783439
0.799275
0.745416
0.755586
0.792117
0.783439
0.772929
13
14
GLM_0
lasso
0.783439
0.783439
0.783439
0.783439
0.799275
0.745416
0.755586
0.792117
0.783439
0.772929
14
15
SVM_2
pca
0.783439
0.783439
0.783439
0.783439
0.805342
0.742615
0.753145
0.795955
0.783439
0.771322
15
16
SVM_3
lasso
0.770701
0.770701
0.770701
0.770701
0.763807
0.774109
0.765672
0.781306
0.770701
0.772887
16
17
KNN_8
pca
0.770701
0.770701
0.770701
0.770701
0.770738
0.740492
0.748128
0.770718
0.770701
0.763977
17
18
DECISIONFOREST_3
lasso
0.770701
0.770701
0.770701
0.770701
0.778258
0.734890
0.743650
0.774644
0.770701
0.761153
18
19
GLM_2
pca
0.757962
0.757962
0.757962
0.757962
0.784435
0.710357
0.718159
0.774157
0.757962
0.740422
19
20
DECISIONFOREST_2
pca
0.751592
0.751592
0.751592
0.751592
0.740877
0.733107
0.736134
0.749100
0.751592
0.749558
```
5.
* Display best performing model.
```python
aml.leader()
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL WEIGHTED-F1
0
1
XGBOOST_1
rfe
0.828025
0.828025
0.828025
0.828025
0.821774     0.815874
0.818482
0.827064
0.828025     0.82723
```
6.
* Display model hyperparameters for trained model.
```python
aml.model_hyperparameters(rank=1)
```
19: AutoML in teradataml

```python
{'response_column': 'survived',
'name': 'xgboost',
'model_type': 'Classification',
'column_sampling': 1,
'min_impurity': 0.0,
'lambda1': 0.01,
'shrinkage_factor': 0.5,
'max_depth': 5,
'min_node_size': 1,
'iter_num': 10,
'seed': 42,
'persist': False,
'output_prob': True,
'output_responses': ['1', '0']}
aml.model_hyperparameters(rank=3)
{'response_column': 'survived',
'name': 'xgboost',
'model_type': 'Classification',
'column_sampling': 1,
'min_impurity': 0.0,
'lambda1': 0.01,
'shrinkage_factor': 0.5,
'max_depth': 5,
'min_node_size': 1,
'iter_num': 10,
'seed': 42,
'persist': False,
'output_prob': True,
'output_responses': ['1', '0']}
```
7.
* Deploy models to the database using one of the following methods:
    * Using  top_n  argument:
```python
aml.deploy(table_name='top_models', top_n=10)
Model Deployment Completed Successfully.
```
    * Passing list of ranks using  ranks  argument:
```python
aml.deploy(table_name='top_models', top_n=10)
```
19: AutoML in teradataml

```python
Model Deployment Completed Successfully.
```
    * Passing range object using  ranks  argument:
```python
aml.deploy(table_name='ranged_models', ranks=range(3,10))
Model Deployment Completed Successfully.
```
8.
* Load models using one of the following methods:
    * Create an instance of AutoML object in a separate session:
    * a.
    * Create an instance of AutoML.
```python
aml_tp = AutoML()
```
    * b.
    * Load models using table name.
```python
top_models = aml_tp.load(table_name="top_models")
top_models
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-
RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL
WEIGHTED-F1
DATA_TABLE
0
1
XGBOOST_1
rfe
0.828025
0.828025
0.828025
0.828025
0.821774
0.815874
0.818482
0.827064
0.828025
0.827230
ml__survived_rfe_172459750511629
1
2
XGBOOST_3
lasso
0.821656
0.821656
0.821656
0.821656
0.814347
0.810611
0.812329
0.820866
0.821656
0.821123
ml__survived_lasso_172460103550342
2
3
XGBOOST_0
lasso
0.821656
0.821656
0.821656
0.821656
0.814347
0.810611
0.812329
0.820866
0.821656
0.821123
ml__survived_lasso_172460103550342
3
4
DECISIONFOREST_0
lasso
0.815287
0.815287
0.815287
0.815287
0.809737
0.799745
0.803792
0.813996
0.815287
0.813774
ml__survived_lasso_172460103550342
4
5
DECISIONFOREST_1
rfe
0.815287
0.815287
0.815287
0.815287
0.809737
0.799745
0.803792
0.813996
0.815287
0.813774
ml__survived_rfe_172459750511629
```
19: AutoML in teradataml

```python
5
6
XGBOOST_2
pca
0.796178
0.796178
0.796178
0.796178
0.800745
0.767148
0.776114
0.798275
0.796178
0.790202
ml__survived_pca_172459977539015
6
7
KNN_9
lasso
0.789809
0.789809
0.789809
0.789809
0.788383
0.764686
0.771893
0.789239
0.789809
0.785330
ml__survived_lasso_172460103550342
7
8
SVM_1
rfe
0.789809
0.789809
0.789809
0.789809
0.799504
0.756282
0.766084
0.794729
0.789809
0.781743
ml__survived_rfe_172459750511629
8
9
SVM_0
lasso
0.789809
0.789809
0.789809
0.789809
0.799504
0.756282
0.766084
0.794729
0.789809
0.781743
ml__survived_lasso_172460103550342
9
10
GLM_3
lasso
0.783439
0.783439
0.783439
0.783439
0.774289
0.781834
0.776839
0.788668
0.783439
0.784906
ml__survived_lasso_172460103550342
```
    * c.
    * Generate prediction and evaluation metrics.
```python
test_data = df.iloc[:200]
```
    * Generate predictions using rank.
```python
preds = aml_tp.predict(data=test_data, rank=2)
Generating prediction using:
Model Name: DECISIONFOREST
Feature Selection: lasso
Completed:  ｜
⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜  100%
- 10/10
```
    * Generate a predictions sample.
```python
preds
id
prediction
prob_1
prob_0
survived
272
0
0.0
1.0
0
596
0
0.0
1.0
0
720
0
0.0
1.0
0
```
19: AutoML in teradataml

```python
760
0
0.0
1.0
0
632
0
0.0
1.0
0
320
1
1.0
0.0
1
624
1
1.0
0.0
0
232
0
0.0
1.0
0
140
1
1.0
0.0
0
628
1
1.0
0.0
1
```
    * Generate evaluation metrics.
```python
metrics = aml_tp.evaluate(data=test_data, rank=2)
Generating performance metrics using:
Model Name: XGBOOST
Feature Selection: lasso
metrics
output_data Output
SeqNum              Metric  MetricValue
0       3        Micro-Recall     0.854271
1       5     Macro-Precision     0.839748
2       6        Macro-Recall     0.871632
3       7            Macro-F1     0.846781
4       9     Weighted-Recall     0.854271
5      10         Weighted-F1     0.857506
6       8  Weighted-Precision     0.876348
7       4            Micro-F1     0.854271
8       2     Micro-Precision     0.854271
9       1            Accuracy     0.854271
result Output
Prediction  Mapping  CLASS_1  CLASS_2  Precision
Recall        F1  Support
SeqNum
0               0  CLASS_1      107        5   0.955357  0.816794
0.880658      131
1               1  CLASS_2       24       63   0.724138  0.926471
0.812903       68
```
    * Using existing AutoML object in the same session.
19: AutoML in teradataml

* a.
    * Load models using table name.
```python
range_models = aml.load(table_name="top_models")
range_models
RANK
MODEL_ID
FEATURE_SELECTION
ACCURACY
MICRO-
PRECISION
MICRO-RECALL
MICRO-F1
MACRO-PRECISION MACRO-
RECALL
MACRO-F1
WEIGHTED-PRECISION
WEIGHTED-RECALL
WEIGHTED-F1
DATA_TABLE
0
1
XGBOOST_0
lasso
0.821656
0.821656
0.821656
0.821656
0.814347
0.810611
0.812329
0.820866
0.821656
0.821123
ml__survived_lasso_172461084799122
1
2
DECISIONFOREST_0
lasso
0.815287
0.815287
0.815287
0.815287
0.809737
0.799745
0.803792
0.813996
0.815287
0.813774
ml__survived_lasso_172461084799122
2
3
DECISIONFOREST_1
rfe
0.815287
0.815287
0.815287
0.815287
0.809737
0.799745
0.803792
0.813996
0.815287
0.813774
ml__survived_rfe_172459587321937
3
4
XGBOOST_2
pca
0.796178
0.796178
0.796178
0.796178
0.800745
0.767148
0.776114
0.798275
0.796178
0.790202
ml__survived_pca_172460474902349
4
5
KNN_9
lasso
0.789809
0.789809
0.789809
0.789809
0.788383
0.764686
0.771893
0.789239
0.789809
0.785330
ml__survived_lasso_172461084799122
5
6
SVM_1
rfe
0.789809
0.789809
0.789809
0.789809
0.799504
0.756282
0.766084
0.794729
0.789809
0.781743
ml__survived_rfe_172459587321937
6
7
SVM_0
lasso
0.789809
0.789809
0.789809
0.789809
0.799504
0.756282
0.766084
0.794729
0.789809
0.781743
ml__survived_lasso_172461084799122
7
8
GLM_3
lasso
0.783439
0.783439
0.783439
0.783439
0.774289
0.781834
0.776839
0.788668
0.783439
0.784906
ml__survived_lasso_172461084799122
```
    * b.
    * Generate prediction and evaluation metrics.
19: AutoML in teradataml

* Note:
```python
Predict  and  evaluate  can be utilized by both fitted models and loaded models. When
```
    * generating predictions and evaluations using loaded models within the same session
    * using an existing AutoML instance, the parameter  use_loaded_models  is set to  True .
    * Otherwise, fitted models are utilized for generating predictions and evaluation metrics
    * instead of loaded models.
```python
test_data = df.iloc[:200]
```
    * Generate predictions using rank.
```python
preds = aml.predict(data=test_data,
rank=3, use_loaded_models=True)
Generating prediction using:
Model Name: DECISIONFOREST
Feature Selection: rfe
Completed:  ｜
⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾ ｜  100%
- 10/10
```
    * Generate prediction sample.
```python
preds
id
prediction
prob_1
prob_0
survived
40
1
1.0
0.0
1
264
0
0.0
1.0
1
260
0
0.0
1.0
0
148
1
1.0
0.0
1
128
1
1.0
0.0
1
80
1
1.0
0.0
1
772
1
1.0
0.0
1
584
0
0.0
1.0
0
272
0
0.0
1.0
0
112
1
1.0
0.0
0
```
    * Generate evaluation metrics.
```python
metrics = aml.evaluate(data=test_data,
rank=3, use_loaded_models=True)
```
19: AutoML in teradataml

```python
Generating performance metrics using:
Model Name: DECISIONFOREST
Feature Selection: rfe
metrics
output_data Output
SeqNum              Metric  MetricValue
0       3        Micro-Recall     0.718593
1       5     Macro-Precision     0.733822
2       6        Macro-Recall     0.757970
3       7            Macro-F1     0.714783
4       9     Weighted-Recall     0.718593
5      10         Weighted-F1     0.725219
6       8  Weighted-Precision     0.790258
7       4            Micro-F1     0.718593
8       2     Micro-Precision     0.718593
9       1            Accuracy     0.718593
result Output
Prediction  Mapping  CLASS_1  CLASS_2  Precision
Recall        F1  Support
SeqNum
0               0  CLASS_1       83        8   0.912088  0.633588
0.747748      131
1               1  CLASS_2       48       60   0.555556  0.882353
0.681818       68
```
9.
* Removed saved models from the database by passing the table name to the API.
```python
aml.remove_saved_models(table_name="mixed_models")
```
### Example 7: Run AutoML for Classification Problem Using Early
### Stopping Metrics and max_models
This example predicts whether passenger aboard the RMS Titanic survived or not based on
different factors. Run AutoClassifier to get the best performing model out of available models with
**following specifications:**
* Use 3 models for training i.e., ‘glm’, ‘svm’ and ‘xgboost’
19: AutoML in teradataml

* Utilize two different early stopping criteria - early stopping metrics ‘MICRO-RECALL’ with threshold
* value 0.9 and maximum models that will be trained as 13
* Opt for verbose level 2 to get detailed log.
1.
* Load data and split it to train and test datasets.
* a.
    * Load the example data and create teradataml DataFrame.
```python
load_example_data("teradataml", "titanic")
titanic = DataFrame.from_table("titanic")
```
* b.
    * Perform sampling to get 80% for training and 20% for testing.
```python
titanic_sample = titanic.sample(frac = [0.8, 0.2])
```
* c.
    * Fetch train and test data.
```python
titanic_train= titanic_sample[titanic_sample['sampleid'] ==
1].drop('sampleid', axis=1)
titanic_test = titanic_sample[titanic_sample['sampleid'] ==
2].drop('sampleid', axis=1)
```
2.
* Create Autoclassifier instance and fit on dataset.
* a.
    * Create an instance of AutoML.
```python
aml = AutoML(include=['glm','svm','xgboost'],
verbose=2,
stopping_metric='MICRO-RECALL',
stopping_tolerance=0.9,
max_models=13)
```
* b.
    * Fit the data.
```python
aml.fit(titanic_train, 'survived')
Task type is set to Classification as target column is having distinct
values less than or equal to 20.
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation ->
4. Model Training & Evaluation
Feature Exploration started ...
Data Overview:
Total Rows in the data: 713
Total Columns in the data: 12
Column Summary:
```
19: AutoML in teradataml

```python
ColumnName
Datatype
NonNullCount
NullCount
BlankCount
ZeroCount
PositiveCount
NegativeCount
NullPercentage
NonNullPercentage
sex
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
name
VARCHAR(1000) CHARACTER SET LATIN
713
0
0
None
None
None
0.0
100.0
parch
INTEGER 713
0
None
542
171
0
0.0
100.0
passenger
INTEGER 713
0
None
0
713
0
0.0
100.0
fare
FLOAT
713
0
None
12
701
0
0.0
100.0
cabin
VARCHAR(20) CHARACTER SET LATIN 167
546
0
None
None
None
76.57784011220197
23.422159887798035
survived
INTEGER 713
0
None
440
273
0
0.0
100.0
pclass
INTEGER 713
0
None
0
713
0
0.0
100.0
embarked
VARCHAR(20) CHARACTER SET LATIN 711
2
0
None
None
None
0.2805049088359046
99.71949509116409
sibsp
INTEGER 713
0
None
489
224
0
0.0
100.0
age
INTEGER 565
148
None
5
560
0
20.757363253856944
79.24263674614306
ticket
VARCHAR(20) CHARACTER SET LATIN 713
0
0
None
None
None
0.0
100.0
Statistics of Data:
func
passenger
survived
pclass
age
sibsp
parch
fare
50%
448
0
3
28
0
0
14.5
count
713
713
713
565
713
713
713
mean
447.447 0.383
2.307
29.517
0.525
0.383
32.735
min
2
0
1
0
0
0
0
max
891
1
3
80
8
6
512.329
75%
674
1
3
38
1
0
31.275
25%
223
0
2
21
0
0
7.896
std
256.643 0.486
0.841
14.462
1.14
0.811
49.141
Categorical Columns with their Distinct values:
ColumnName                DistinctValueCount
name                      713
```
19: AutoML in teradataml

```python
sex                       2
ticket                    569
cabin                     128
embarked                  3
Futile columns in dataset:
ColumnName
name
ticket
Target Column Distribution:
Columns with outlier
percentage :-
ColumnName  OutlierPercentage
0        age          22.159888
1      parch          23.983170
2      sibsp           4.908836
3       fare          14.305750
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation ->
4. Model Training & Evaluation
Feature Engineering started ...
Handling duplicate records present in dataset ...
Analysis completed. No action
taken.
Total time to handle duplicate records: 1.68 sec
Handling less significant features from data ...
Removing Futile columns:
['ticket', 'name']
Sample of Data after removing Futile columns:
```
19: AutoML in teradataml

```python
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
265
0
3
female
None
0
0
7.75
None
Q
9
122
0
3
male
None
0
0
8.05
None
S
11
591
0
3
male
35
0
0
7.125
None
S
19
734
0
2
male
23
0
0
13.0
None
S
14
326
1
1
female
36
0
0
135.6333
C32
C
13
305
0
3
male
None
0
0
8.05
None
S
21
631
1
1
male
80
0
0
30.0
A23
S
10
120
0
3
female
2
4
2
31.275
None
S
18
80
1
3
female
30
0
0
12.475
None
S
12
345
0
2
male
36
0
0
13.0
None
S
20
713 rows X 11 columns
Total time to handle less significant features: 19.67 sec
Handling Date Features ...
Analysis Completed. Dataset does not contain any feature related to dates.
No action needed.
Total time to handle date features: 0.00 sec
Checking Missing values in dataset ...
Columns with their missing values:
age: 148
cabin: 546
embarked: 2
Deleting rows of these columns for handling missing values:
['embarked']
```
19: AutoML in teradataml

```python
Sample of dataset after removing 2 rows:
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
122
0
3
male
None
0
0
8.05
None
S
11
570
1
3
male
32
0
0
7.8542
None
S
15
835
0
3
male
18
0
0
8.3
None
S
23
265
0
3
female
None
0
0
7.75
None
Q
9
631
1
1
male
80
0
0
30.0
A23
S
10
120
0
3
female
2
4
2
31.275
None
S
18
734
0
2
male
23
0
0
13.0
None
S
14
61
0
3
male
22
0
0
7.2292
None
C
22
80
1
3
female
30
0
0
12.475
None
S
12
345
0
2
male
36
0
0
13.0
None
S
20
711 rows X 11 columns
Dropping these columns for handling missing values:
['cabin']
Sample of dataset after removing 1 columns:
passenger
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
265
0
3
female
None
0
0
7.75
Q
9
326
1
1
female
36
0
0
135.6333
C
13
305
0
3
male
None
0
0
8.05
S
21
80
1
3
female
30
0
0
12.475
S
12
734
0
2
male
23
0
0
13.0
S
14
61
0
3
male
22
0
0
7.2292
C
22
631
1
1
male
80
0
0
30.0
S
10
120
0
3
female
2
4
2
31.275
S
18
570
1
3
male
32
0
0
7.8542
S
15
835
0
3
male
18
0
0
8.3
S
23
```
19: AutoML in teradataml

```python
711 rows X 10 columns
Total time to find missing values in data: 15.02 sec
Imputing Missing Values ...
Columns with their imputation method:
age: mean
Sample of dataset after Imputation:
passenger
survived
pclass
sex
age
sibsp
parch
fare
embarked
id
162
1
2
female
40
0
0
15.75
S
31
223
0
3
male
51
0
0
8.05
S
47
692
1
3
female
4
0
1
13.4167 C
55
753
0
3
male
33
0
0
9.5
S
63
671
1
2
female
40
1
1
39.0
S
79
528
0
1
male
29
0
0
221.7792
S
87
202
0
3
male
29
8
2
69.55
S
71
427
1
2
female
28
1
0
26.0
S
39
835
0
3
male
18
0
0
8.3
S
23
570
1
3
male
32
0
0
7.8542
S
15
711 rows X 10 columns
Time taken to perform imputation: 15.64 sec
Performing encoding for categorical columns ...
result data stored in table
'"automl_user"."ml__td_sqlmr_persist_out__1713324989208786"'7
ONE HOT Encoding these Columns:
['sex', 'embarked']
Sample of dataset after performing one hot encoding:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
38
0
3
0
1
21
0
0
8.05
0
0
1
28
772
0
3
0
1
48
0
0
7.8542
0
0
1
44
425
0
3
0
1
18
1
1
20.2125 0
0
1
52
```
19: AutoML in teradataml

```python
118
0
2
0
1
29
1
0
21.0
0
0
1
60
852
0
3
0
1
74
0
0
7.775
0
0
1
76
505
1
1
1
0
16
0
0
86.5
0
0
1
84
587
0
2
0
1
47
0
0
15.0
0
0
1
68
507
1
2
1
0
33
0
2
26.0
0
0
1
36
345
0
2
0
1
36
0
0
13.0
0
0
1
20
80
1
3
1
0
30
0
0
12.475
0
0
1
12
711 rows X 13 columns
Time taken to encode the columns: 13.25 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation ->
4. Model Training & Evaluation
Data preparation started ...
Spliting of dataset into training and testing ...
Training size :
0.8
Testing size  : 0.2
Training data sample
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
265
0
3
1
0
29
0
0
7.75
0
1
0
9
122
0
3
0
1
29
0
0
8.05
0
0
1
11
591
0
3
0
1
35
0
0
7.125
0
0
1
19
570
1
3
0
1
32
0
0
7.8542
0
0
1
15
326
1
1
1
0
36
0
0
135.6333
1
0
0
13
```
19: AutoML in teradataml

```python
305
0
3
0
1
29
0
0
8.05
0
0
1
21
734
0
2
0
1
23
0
0
13.0
0
0
1
14
61
0
3
0
1
22
0
0
7.2292
1
0
0
22
80
1
3
1
0
30
0
0
12.475
0
0
1
12
345
0
2
0
1
36
0
0
13.0
0
0
1
20
568 rows X 13 columns
Testing data sample
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
101
0
3
1
0
28
0
0
7.8958
0
0
1
25
387
0
3
0
1
1
5
2
46.9
0
0
1
27
871
0
3
0
1
26
0
0
7.8958
0
0
1
123
38
0
3
0
1
21
0
0
8.05
0
0
1
28
732
0
3
0
1
11
0
0
18.7875 1
0
0
29
196
1
1
1
0
58
0
0
146.5208
1
0
0
125
652
1
2
1
0
18
0
1
23.0
0
0
1
30
585
0
3
0
1
29
0
0
8.7125
1
0
0
126
162
1
2
1
0
40
0
0
15.75
0
0
1
31
139
0
3
0
1
16
0
0
9.2167
0
0
1
127
143 rows X 13 columns
Time taken for spliting of data: 10.71 sec
Outlier preprocessing ...
Columns with outlier
percentage :-
```
19: AutoML in teradataml

```python
ColumnName  OutlierPercentage
0        age           6.751055
1       fare          14.064698
2      sibsp           4.922644
3      parch          24.050633
Deleting rows of these columns:
['sibsp', 'age']
result data stored in table
'"automl_user"."ml__td_sqlmr_persist_out__1713328140917821"'7
Sample of training dataset after removing outlier rows:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
parch
fare
embarked_0
embarked_1
embarked_2
id
427
1
2
1
0
28
1
0
26.0
0
0
1
39
692
1
3
1
0
4
0
1
13.4167 1
0
0
55
753
0
3
0
1
33
0
0
9.5
0
0
1
63
671
1
2
1
0
40
1
1
39.0
0
0
1
79
589
0
3
0
1
22
0
0
8.05
0
0
1
103
833
0
3
0
1
29
0
0
7.2292
1
0
0
111
528
0
1
0
1
29
0
0
221.7792
0
0
1
87
223
0
3
0
1
51
0
0
8.05
0
0
1
47
835
0
3
0
1
18
0
0
8.3
0
0
1
23
570
1
3
0
1
32
0
0
7.8542
0
0
1
15
494 rows X 13 columns
median inplace of outliers:
['fare', 'parch']
result data stored in table
'"automl_user"."ml__td_sqlmr_persist_out__1713332169253155"'7
Sample of training dataset after performing MEDIAN inplace:
passenger
survived
pclass
sex_0
sex_1
age
sibsp
```
19: AutoML in teradataml

```python
parch
fare
embarked_0
embarked_1
embarked_2
id
507
1
2
1
0
33
0
0
26.0
0
0
1
36
425
0
3
0
1
18
1
0
20.2125 0
0
1
52
118
0
2
0
1
29
1
0
21.0
0
0
1
60
587
0
2
0
1
47
0
0
15.0
0
0
1
68
362
0
2
0
1
29
1
0
27.7208 1
0
0
92
198
0
3
0
1
42
0
0
8.4042
0
0
1
100
505
1
1
1
0
16
0
0
13.0
0
0
1
84
772
0
3
0
1
48
0
0
7.8542
0
0
1
44
345
0
2
0
1
36
0
0
13.0
0
0
1
20
80
1
3
1
0
30
0
0
12.475
0
0
1
12
494 rows X 13 columns
Time Taken by Outlier processing: 48.73 sec
result data stored in table
'"automl_user"."ml__td_sqlmr_persist_out__1713325332166272"'7
result data stored in table
'"automl_user"."ml__td_sqlmr_persist_out__1713325412941558"'
Checking imbalance data ...
Imbalance Not Found.
Feature selection using lasso ...
feature selected by lasso:
['sex_1', 'embarked_0', 'pclass', 'fare', 'age', 'sibsp', 'sex_0',
'embarked_2', 'passenger', 'embarked_1']
Total time taken by feature selection: 2.79 sec
scaling Features of lasso data ...
```
19: AutoML in teradataml

```python
columns that will be scaled:
['pclass', 'fare', 'age', 'sibsp', 'passenger']
Training dataset sample after scaling:
id
sex_1
embarked_0
survived
sex_0
embarked_2
embarked_1
pclass
fare
age
sibsp
passenger
40
1
0
0
0
1
0
1.0
0.1327683615819209
0.6458333333333334
0.0
0.40719910011248595
80
0
1
1
1
0
0
0.0
0.24482
2787194
0.5833333333333334
0.0
0.2440944881889764
326
0
0
1
1
0
1
1.0
0.14595103578154425
0.5208333333333334
0.0
0.4128233970753656
734
0
0
1
1
1
0
0.5
0.24482
2787194
0.6666666666666666
0.0
0.36670416197975253
509
1
0
0
0
1
0
1.0
0.3032015065913371
0.5416666666666666
0.5
0.28346456692913385
101
0
0
1
1
1
0
0.0
1.0
0.6041666666666666
0.5
0.9088863892013498
570
1
1
0
0
0
0
1.0
0.13606403013182672
0.5208333333333334
0.0
0.0281214848143982
591
1
1
0
0
0
0
1.0
0.13606403013182672
0.5208333333333334
0.0
0.5860517435320585
530
1
0
0
0
1
0
0.5
0.24482
2787194
0.625
0.0
0.8110236220472441
469
0
0
1
1
1
0
0.0
0.24482
2787194
0.375
0.0
0.39932508436445446
494 rows X 12 columns
Testing dataset sample after scaling:
id
sex_1
embarked_0
survived
sex_0
embarked_2
embarked_1
pclass
fare
age
sibsp
passenger
120
0
1
1
1
0
0
0.0
1.678045197740113
0.5208333333333334
0.5
0.953880764904387
242
1
0
0
0
0
1
1.0
0.14595103578154425
0.5208333333333334
0.0
0.140607424071991
650
0
1
1
1
0
0
0.0
2.0881977401129945
0.5208333333333334
0.0
0.34308211473565803
244
1
0
0
0
1
0
1.0
0.3032015065913371
0.5208333333333334
0.5
0.7176602924634421
```
19: AutoML in teradataml

```python
486
0
0
1
1
1
0
0.5
0.4896421845574388
0.5208333333333334
0.5
0.05849268841394826
747
1
0
0
0
0
1
1.0
0.14595103578154425
0.5208333333333334
0.0
0.688413948256468
202
1
0
0
0
1
0
0.5
0.2448210922787194
0.7916666666666666
0.0
0.16647919010123735
122
1
1
0
0
0
0
1.0
0.14869679849340867
0.6458333333333334
0.0
0.9516310461192351
549
1
1
0
0
0
0
0.0
2.5542994350282484
0.375
0.0
0.4184476940382452
774
1
0
0
0
1
0
0.0
1.455508474576271
0.3541666666666667
0.0
0.11361079865016872
143 rows X 12 columns
Total time taken by feature scaling: 45.68 sec
Feature selection using rfe ...
feature selected by RFE:
['pclass', 'age', 'sex_0', 'sex_1', 'passenger', 'fare']
Total time taken by feature selection: 31.48 sec
scaling Features of rfe data ...
columns that will be scaled:
['r_pclass', 'r_age', 'r_passenger', 'r_fare']
Training dataset sample after scaling:
id
r_sex_1 r_sex_0 survived
r_pclass
r_age
r_passenger
r_fare
40
1
0
0
1.0
0.6458333333333334
0.40719910011248595
0.1327683615819209
80
0
1
1
0.0
0.5833333333333334
0.2440944881889764
0.2448210922787194
326
0
1
1
1.0
0.5208333333333334
0.4128233970753656
0.14595103578154425
734
0
1
1
0.5
0.6666666666666666
0.36670416197975253
0.2448210922787194
509
1
0
0
1.0
0.5416666666666666
```
19: AutoML in teradataml

```python
0.28346456692913385
0.3032015065913371
101
0
1
1
0.0
0.6041666666666666
0.9088863892013498
1.0
570
1
0
0
1.0
0.5208333333333334
0.0281214848143982
0.13606403013182672
591
1
0
0
1.0
0.5208333333333334
0.5860517435320585
0.13606403013182672
530
1
0
0
0.5
0.625
0.8110236220472441
0.2448210922787194
469
0
1
1
0.0
0.375
0.39932508436445446
0.2448210922787194
494 rows X 8 columns
Testing dataset sample after scaling:
id
r_sex_1 r_sex_0 survived
r_pclass
r_age
r_passenger
r_fare
120
0
1
1
0.0
0.5208333333333334
0.953880764904387
1.678045197740113
242
1
0
0
1.0
0.5208333333333334
0.140607424071991
0.14595103578154425
650
0
1
1
0.0
0.5208333333333334
0.34308211473565803
2.0881977401129945
244
1
0
0
1.0
0.5208333333333334
0.7176602924634421
0.3032015065913371
486
0
1
1
0.5
0.5208333333333334
0.05849268841394826
0.4896421845574388
747
1
0
0
1.0
0.5208333333333334
0.688413948256468
0.14595103578154425
202
1
0
0
0.5
0.7916666666666666
0.16647919010123735
0.2448210922787194
122
1
0
0
1.0
0.6458333333333334
0.9516310461192351
0.14869679849340867
549
1
0
0
0.0
0.375
0.4184476940382452
2.5542994350282484
774
1
0
0
0.0
0.3541666666666667
0.11361079865016872
1.455508474576271
143 rows X 8 columns
Total time taken by feature scaling: 46.86 sec
scaling Features of pca data ...
```
19: AutoML in teradataml

```python
columns that will be scaled:
['passenger', 'pclass', 'age', 'sibsp', 'fare']
Training dataset sample after scaling:
id
sex_1
embarked_0
parch
survived
sex_0
embarked_2
embarked_1
passenger
pclass
age
sibsp
fare
9
0
0
0
0
1
0
1
0.2958380202474691
1.0
0.5208333333333334
0.0
0.14595103578154425
11
1
0
0
0
0
1
0
0.13498312710911137
1.0
0.5208333333333334
0.0
0.15160075329566855
19
1
0
0
0
0
1
0
0.6625421822272216
1.0
0.6458333333333334
0.0
0.13418079096045196
15
1
0
0
1
0
1
0
0.6389201349831272
1.0
0.5833333333333334
0.0
0.14791337099811674
13
0
1
0
1
1
0
0
0.3644544431946007
0.0
0.6666666666666666
0.0
0.2448210922787194
21
1
0
0
0
0
1
0
0.3408323959505062
1.0
0.5208333333333334
0.0
0.15160075329566855
14
1
0
0
0
0
1
0
0.8233970753655793
0.5
0.3958333333333333
0.0
0.2448210922787194
22
1
1
0
0
0
0
0
0.06636670416197975
1.0
0.375
0.0
0.13614312617702448
12
0
0
0
1
1
1
0
0.08773903262092239
1.0
0.5416666666666666
0.0
0.23493408662900186
20
1
0
0
0
0
1
0
0.3858267716535433
0.5
0.6666666666666666
0.0
0.2448210922787194
494 rows X 13 columns
Testing dataset sample after scaling:
id
sex_1
embarked_0
parch
survived
sex_0
embarked_2
embarked_1
passenger
pclass
age
sibsp
fare
25
0
0
0
0
1
1
0
0.11136107986501688
1.0
0.5
0.0
0.14869679849340867
29
1
1
0
0
0
0
0
```
19: AutoML in teradataml

```python
0.8211473565804275
1.0
0.14583333333333334
0.0
0.3538135593220339
125
0
1
0
1
1
0
0
0.21822272215973004
0.0
1.125
0.0
2.759337099811676
26
0
0
1
1
1
1
0
0.84251968503937
0.5
0.0
0.5
0.4331450094161958
27
1
0
2
0
0
1
0
0.4330708661417323
1.0
-0.0625 2.5
0.8832391713747646
123
1
0
0
0
0
1
0
0.9775028121484814
1.0
0.4583333333333333
0.0
0.14869679849340867
28
1
0
0
0
0
1
0
0.04049493813273341
1.0
0.3541666666666667
0.0
0.15160075329566855
124
1
0
1
0
0
1
0
0.1968503937007874
1.0
0.5208333333333334
1.5
0.47959887005649715
31
0
0
0
1
1
1
0
0.17997750281214847
0.5
0.75
0.0
0.2966101694915254
127
1
0
0
0
0
1
0
0.15410573678290213
1.0
0.25
0.0
0.1735725047080979
143 rows X 13 columns
Total time taken by feature scaling: 44.56 sec
Dimension Reduction using pca ...
PCA columns:
['col_0', 'col_1', 'col_2', 'col_3', 'col_4', 'col_5']
Total time taken by PCA: 12.29 sec
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation ->
4. Model Training & Evaluation
Model Training started ...
Hyperparameters used for model training:
response_column :
survived
name : xgboost
```
19: AutoML in teradataml

```python
model_type : Classification
column_sampling : (1, 0.6)
min_impurity : (0.0, 0.1, 0.2)
lambda1 : (0.01, 0.1, 1, 10)
shrinkage_factor : (0.5, 0.1, 0.3)
max_depth : (5, 6, 8, 10)
min_node_size : (1, 2, 3)
iter_num : (10, 20, 30)
Total number of models for xgboost : 2592
response_column : survived
name : svm
model_type : Classification
lambda1 : (0.001, 0.02, 0.1)
alpha : (0.15, 0.85)
tolerance : (0.001, 0.01)
learning_rate : OPTIMAL
initial_eta : (0.05, 0.1)
momentum : (0.65, 0.8, 0.95)
nesterov : True
intercept : True
iter_num_no_change : (5, 10, 50)
local_sgd_iterations  : (10, 20)
iter_max : (300, 200, 400)
batch_size : (10, 50, 60, 80)
Total number of models for svm : 5184
response_column : survived
name : glm
family : BINOMIAL
lambda1 : (0.001, 0.02, 0.1)
alpha : (0.15, 0.85)
learning_rate : OPTIMAL
initial_eta : (0.05, 0.1)
momentum : (0.65, 0.8, 0.95)
iter_num_no_change : (5, 10, 50)
iter_max : (300, 200, 400)
batch_size : (10, 50, 60, 80)
```
19: AutoML in teradataml

```python
Total number of models for glm : 1296
Performing hyperParameter tuning ...
xgboost
svm
glm
Evaluating models performance ...
Evaluation completed.
Leaderboard
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-
Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-
F1
0
1
GLM_3
rfe
0.811189
0.811189
0.811189
0.811189
0.802198
0.795455
0.798413
0.809806
0.811189
0.810124
1
2
GLM_4
pca
0.811189
0.811189
0.811189
0.811189
0.803978
0.792045
0.796843
0.809512
0.811189
0.809301
2
3
GLM_1
lasso
0.804196
0.804196
0.804196
0.804196
0.797330
0.782955
0.788462
0.802365
0.804196
0.801775
3
4
GLM_2
rfe
0.804196
0.804196
0.804196
0.804196
0.799867
0.779545
0.786658
0.802782
0.804196
0.800774
```
19: AutoML in teradataml

```python
4
5
GLM_5
pca
0.804196
0.804196
0.804196
0.804196
0.803061
0.776136
0.784731
0.803768
0.804196
0.799669
5
6
XGBOOST_2
rfe
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
6
7
XGBOOST_3
rfe
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
7
8
XGBOOST_4
pca
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
8
9
SVM_1
lasso
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
9
10
SVM_3
rfe
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
10
11
SVM_4
pca
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
11
12
XGBOOST_0
lasso
0.797203
0.797203
0.797203
0.797203
0.785598
0.790909
0.787866
0.799782
0.797203
0.798136
12
13
GLM_0
lasso
0.797203
0.797203
0.797203
0.797203
0.788602
0.777273
0.781794
0.795203
0.797203
0.795175
13 rows X 13 columns
1. Feature Exploration -> 2. Feature Engineering -> 3. Data Preparation ->
4. Model Training & Evaluation
Completed:  ｜ ⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾
```
    * ｜  100% - 17/17
3.
* Get model leaderboard.
```python
aml.leaderboard()
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
GLM_3
rfe
0.811189
0.811189
0.811189
```
19: AutoML in teradataml

```python
0.811189
0.802198
0.795455
0.798413
0.809806
0.811189
0.810124
1
2
GLM_4
pca
0.811189
0.811189
0.811189
0.811189
0.803978
0.792045
0.796843
0.809512
0.811189
0.809301
2
3
GLM_1
lasso
0.804196
0.804196
0.804196
0.804196
0.797330
0.782955
0.788462
0.802365
0.804196
0.801775
3
4
GLM_2
rfe
0.804196
0.804196
0.804196
0.804196
0.799867
0.779545
0.786658
0.802782
0.804196
0.800774
4
5
GLM_5
pca
0.804196
0.804196
0.804196
0.804196
0.803061
0.776136
0.784731
0.803768
0.804196
0.799669
5
6
XGBOOST_2
rfe
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
6
7
XGBOOST_3
rfe
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
7
8
XGBOOST_4
pca
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
8
9
SVM_1
lasso
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
9
10
SVM_3
rfe
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
10
11
SVM_4
pca
0.804196
0.804196
0.804196
0.804196
0.806977
0.772727
0.782675
0.805367
0.804196
0.798457
11
12
XGBOOST_0
lasso
0.797203
0.797203
0.797203
0.797203
0.785598
0.790909
0.787866
0.799782
0.797203
0.798136
12
13
GLM_0
lasso
0.797203
0.797203
0.797203
0.797203
0.788602
0.777273
0.781794
0.795203
0.797203
0.795175
```
4.
* Get best performing model.
```python
aml.leader()
```
19: AutoML in teradataml

```python
Rank
Model-ID
Feature-Selection
Accuracy
Micro-
Precision
Micro-Recall
Micro-F1
Macro-Precision Macro-Recall
Macro-F1
Weighted-Precision
Weighted-Recall Weighted-F1
0
1
GLM_3
rfe
0.811189
0.811189
0.811189
0.811189
0.802198
0.795455
0.798413
0.809806
0.811189
0.810124
```
5.
* Generate prediction on validation dataset using best performing model.
* Note:
* In the data preparation phase, AutoML generates the validation dataset by splitting the data
* provided during fitting into training and testing sets. AutoML's model training utilizes the training
* data, with the testing data acting as the validation dataset for model evaluation.
```python
prediction = aml.predict(rank=4)
Following model is being used for generating prediction :
Model ID : GLM_2
Feature Selection Method : rfe
Prediction :
id  prediction      prob  survived
0  120         1.0  0.920487         1
1  242         0.0  0.881366         0
2  650         1.0  0.930461         1
3  244         0.0  0.875438         0
4  486         1.0  0.801441         1
5  747         0.0  0.881366         0
6  202         0.0  0.791899         0
7  122         0.0  0.881265         0
8  549         1.0  0.528228         0
9  774         0.0  0.568290         0
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  Precision    Recall        F1
Support
SeqNum
1               1  CLASS_2       10       37   0.787234  0.672727  0.725490
55
0               0  CLASS_1       78       18   0.812500  0.886364  0.847826
88
```
19: AutoML in teradataml

```python
ROC-AUC :
AUC
GINI
0.7413223140495868
0.48264462809917363
threshold_value tpr
fpr
0.04081632653061224
0.6727272727272727
0.11363636363636363
0.08163265306122448
0.6727272727272727
0.11363636363636363
0.1020408163265306
0.6727272727272727
0.11363636363636363
0.12244897959183673
0.6727272727272727
0.11363636363636363
0.16326530612244897
0.6727272727272727
0.11363636363636363
0.18367346938775508
0.6727272727272727
0.11363636363636363
0.14285714285714285
0.6727272727272727
0.11363636363636363
0.061224489795918366
0.6727272727272727
0.11363636363636363
0.02040816326530612
0.6727272727272727
0.11363636363636363
0.0
1.0
1.0
Confusion Matrix :
array([[78, 10],
[18, 37]], dtype=int64)
prediction.head()
id
prediction
prob
survived
120
1.0
0.9204874367314612
1
242
0.0
0.8813661538418516
0
650
1.0
0.9304605628045387
1
244
0.0
0.8754375621867835
0
486
1.0
0.8014409602906027
1
747
0.0
0.8813661538418516
0
202
0.0
0.7918987528859897
0
122
0.0
0.8812647618359073
0
549
1.0
0.5282277418701877
0
774
0.0
0.5682901048373651
0
```
6.
* Generate prediction on test dataset using best performing model.
```python
prediction = aml.predict(titanic_test, rank=13)
Data Transformation started ...
Performing transformation carried out in feature engineering phase ...
Updated dataset after dropping futile columns :
passenger
survived
pclass
sex
age
sibsp
parch
fare
cabin
embarked
id
301
1
3
female
None
0
0
7.75
None
Q
```
19: AutoML in teradataml

```python
9
282
0
3
male
28
0
0
7.8542
None
S
15
15
0
3
female
14
0
0
7.8542
None
S
23
40
1
3
female
14
1
0
11.2417 None
C
10
242
1
3
female
None
1
0
15.5
None
Q
12
240
0
2
male
33
0
0
12.275
None
S
20
713
1
1
male
48
1
0
52.0
C126
S
11
854
1
1
female
16
0
1
39.4
D28
S
19
795
0
3
male
25
0
0
7.8958
None
S
14
244
0
3
male
22
0
0
7.125
None
S
22
Updated dataset after performing target column transformation :
id
pclass
fare
embarked
age
cabin
parch
sibsp
sex
passenger
survived
8
3
7.225
C
None
None
0
0
male
774
0
14
3
7.8958
S
25
None
0
0
male
795
0
22
3
7.125
S
22
None
0
0
male
244
0
11
1
52.0
S
48
C126
0
1
male
713
1
12
3
15.5
Q
None
None
0
1
female
242
1
20
2
12.275
S
33
None
0
0
male
240
0
15
3
7.8542
S
28
None
0
0
male
282
0
23
3
7.8542
S
14
None
0
0
female
15
0
10
3
11.2417 C
14
None
0
1
female
40
1
18
2
13.0
S
24
None
0
0
female
200
0
```
19: AutoML in teradataml

```python
Updated dataset after dropping missing value containing columns :
id
pclass
fare
embarked
age
parch
sibsp
sex
passenger
survived
8
3
7.225
C
None
0
0
male
774
0
10
3
11.2417 C
14
0
1
female
40
1
18
2
13.0
S
24
0
0
female
200
0
14
3
7.8958
S
25
0
0
male
795
0
12
3
15.5
Q
None
0
1
female
242
1
20
2
12.275
S
33
0
0
male
240
0
11
1
52.0
S
48
0
1
male
713
1
19
1
39.4
S
16
1
0
female
854
1
15
3
7.8542
S
28
0
0
male
282
0
23
3
7.8542
S
14
0
0
female
15
0
Updated dataset after imputing missing value containing columns :
id
pclass
fare
embarked
age
parch
sibsp
sex
passenger
survived
34
3
14.4542 C
15
0
1
female
831
1
13
2
13.0
S
28
0
0
female
444
1
11
1
52.0
S
48
0
1
male
713
1
9
3
7.75
Q
29
0
0
female
301
1
68
3
6.4375
C
34
0
0
male
844
0
87
3
7.2292
C
29
0
0
male
569
0
89
3
27.9
S
4
2
3
male
64
0
156
3
8.05
S
29
0
0
male
46
0
17
2
10.5
S
66
0
0
male
34
0
101
2
26.0
S
44
0
1
male
237
0
result data stored in table
'"automl_user"."ml__td_sqlmr_persist_out__1713326001300629"'
Updated dataset after performing categorical encoding :
id
pclass
fare
embarked_0
embarked_1
embarked_2
age
parch
sibsp
sex_0
sex_1
passenger
survived
34
3
14.4542 1
0
0
15
0
1
1
0
831
1
13
2
13.0
0
0
1
28
0
0
1
0
444
1
11
1
52.0
0
0
1
48
0
1
0
1
713
1
9
3
7.75
0
1
0
29
0
0
1
0
301
1
68
3
6.4375
1
0
0
34
0
0
0
1
844
0
87
3
7.2292
1
0
0
29
0
0
0
```
19: AutoML in teradataml

```python
1
569
0
89
3
27.9
0
0
1
4
2
3
0
1
64
0
156
3
8.05
0
0
1
29
0
0
0
1
46
0
17
2
10.5
0
0
1
66
0
0
0
1
34
0
101
2
26.0
0
0
1
44
0
1
0
1
237
0
Performing transformation carried out in data preparation phase ...
result data stored in table
'"automl_user"."ml__td_sqlmr_persist_out__1713326491643092"'
Updated dataset after performing Lasso feature selection:
id
sex_1
embarked_0
pclass
fare
age
sibsp
sex_0
embarked_2
passenger
embarked_1
survived
139
1
0
3
7.75
29
0
0
0
460
1
0
97
1
0
2
73.5
18
0
0
1
386
0
0
15
1
0
3
7.8542
28
0
0
1
282
0
0
32
1
0
3
8.05
43
0
0
1
669
0
0
51
0
0
2
13.0
38
0
1
1
358
0
0
108
1
0
3
7.0542
51
0
0
1
632
0
0
133
1
0
1
28.5
45
0
0
1
332
0
0
179
1
1
2
24.0
30
1
0
0
309
0
0
78
0
0
3
7.775
25
0
1
1
247
0
0
162
1
1
3
7.225
29
0
0
0
355
0
0
Updated dataset after performing scaling on Lasso selected features :
id
sex_1
embarked_0
survived
sex_0
embarked_2
embarked_1
pclass
fare
age
sibsp
passenger
61
0
0
1
1
1
0
1.0
0.20966666666666667
-0.0625 0.5
0.1923509561304837
101
1
0
0
0
1
0
0.5
0.4896421845574388
0.8333333333333334
0.5
0.2643419572553431
```
19: AutoML in teradataml

```python
17
1
0
0
0
1
0
0.5
0.19774011299435026
1.2916666666666667
0.0
0.0359955005624297
40
1
0
0
0
1
0
0.0
1.152071563088512
0.875
0.5
0.10236220472440945
122
1
0
0
0
1
0
0.5
0.19774011299435026
0.6458333333333334
0.0
0.9122609673790776
19
0
0
1
1
1
0
0.0
0.7419962335216572
0.25
0.0
0.9583802024746907
99
0
0
1
1
1
0
0.0
3.979990583804143
0.5208333333333334
0.0
0.8200224971878515
95
1
0
1
0
1
0
0.5
0.5461393596986818
-0.08333333333333333
0.0
0.08661417322834646
162
1
1
0
0
0
0
1.0
0.13606403013182672
0.5208333333333334
0.0
0.39707536557930256
78
0
0
0
1
1
0
1.0
0.14642184557438795
0.4375
0.0
0.2755905511811024
Updated dataset after performing RFE feature selection:
id
pclass
age
sex_0
sex_1
passenger
fare
survived
160
3
36
0
1
664
7.4958
0
116
3
1
1
0
382
15.7417 1
154
3
21
1
0
437
34.375
0
28
3
44
0
1
604
8.05
0
167
3
45
1
0
168
27.9
0
163
2
29
1
0
597
33.0
1
188
3
29
1
0
410
25.4667 0
36
3
20
1
0
114
9.825
0
141
2
54
0
1
250
26.0
0
61
3
1
1
0
173
11.1333 1
Updated dataset after performing scaling on RFE selected features :
id
r_sex_1 r_sex_0 survived
r_pclass
r_age
r_passenger
r_fare
80
1
0
0
1.0
0.375
0.4431946006749156
0.14681355932203388
162
1
0
0
1.0
0.5208333333333334
0.39707536557930256
0.13606403013182672
78
0
1
0
1.0
0.4375
0.2755905511811024
0.14642184557438795
122
1
0
0
0.5
0.6458333333333334
0.9122609673790776
0.19774011299435026
99
0
1
1
0.0
0.5208333333333334
0.8200224971878515
3.979990583804143
95
1
0
1
0.5
-0.08333333333333333
```
19: AutoML in teradataml

```python
0.08661417322834646
0.5461393596986818
61
0
1
1
1.0
-0.0625 0.1923509561304837
0.20966666666666667
141
1
0
0
0.5
1.0416666666666667
0.27896512935883017
0.4896421845574388
101
1
0
0
0.5
0.8333333333333334
0.2643419572553431
0.4896421845574388
17
1
0
0
0.5
1.2916666666666667
0.0359955005624297
0.19774011299435026
Updated dataset after performing scaling for PCA feature selection :
id
sex_1
embarked_0
parch
survived
sex_0
embarked_2
embarked_1
passenger
pclass
age
sibsp
fare
122
1
0
0
0
0
1
0
0.9122609673790776
0.5
0.6458333333333334
0.0
0.19774011299435026
61
0
0
1
1
1
1
0
0.1923509561304837
1.0
-0.0625 0.5
0.20966666666666667
141
1
0
0
0
0
1
0
0.27896512935883017
0.5
1.0416666666666667
0.5
0.4896421845574388
40
1
0
0
0
0
1
0
0.10236220472440945
0.0
0.875
0.5
1.152071563088512
101
1
0
0
0
0
1
0
0.2643419572553431
0.5
0.8333333333333334
0.5
0.4896421845574388
17
1
0
0
0
0
1
0
0.0359955005624297
0.5
1.2916666666666667
0.0
0.19774011299435026
162
1
1
0
0
0
0
0
0.39707536557930256
1.0
0.5208333333333334
0.0
0.13606403013182672
78
0
0
0
0
1
1
0
0.2755905511811024
1.0
0.4375
0.0
0.14642184557438795
99
0
0
0
1
1
1
0
0.8200224971878515
0.0
0.5208333333333334
0.0
3.979990583804143
95
1
0
2
1
0
1
0
0.08661417322834646
0.5
-0.08333333333333333
0.0
0.5461393596986818
Updated dataset after performing PCA feature selection :
id
col_0
col_1
col_2
col_3
col_4
col_5
survived
0
183
1.010659
0.087650
1.015931
0.540143
```
19: AutoML in teradataml

```python
0.427567
-0.295310
1
1
101
-0.508872
-0.061286
-0.395290
0.094121
0.437546
0.196669
0
2
40
-0.372873
-0.029375
-0.969576
0.345081
0.664177
0.172283
0
3
122
-0.580576
-0.086576
-0.265136
0.176906
-0.382837
0.010624
0
4
80
-0.659454
-0.112057
0.207767
-0.152085
0.008856
-0.126807
0
173
103
0.851246
-0.646328
-0.718562
0.252324
-0.018630
0.420395
1
174
168
-0.551856
-0.072476
-0.219560
0.024517
0.179139
-0.274188
0
175
166
-0.677373
-0.119088
0.163534
-0.037520
-0.340589
0.049311
0
176
23
0.619989
-0.697611
0.415298
-0.379321
0.293878
-0.337729
0
177
164
0.932179
-0.652585
-1.199964
0.536862
0.406523
0.124425
1
178 rows × 8 columns
Data Transformation completed.
Following model is being used for generating prediction :
Model ID : GLM_0
Feature Selection Method : lasso
Prediction :
id  prediction      prob  survived
0  101         0.0  0.842330         0
1   40         0.0  0.613704         0
2  120         0.0  0.883461         0
3  122         0.0  0.865047         0
4   61         1.0  0.797549         1
5  141         0.0  0.876889         0
6  162         0.0  0.865844         0
7   78         1.0  0.655147         0
8   99         1.0  0.950835         1
9   95         0.0  0.582591         1
Performance Metrics :
Prediction  Mapping  CLASS_1  CLASS_2  Precision    Recall        F1
Support
```
19: AutoML in teradataml