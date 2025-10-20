# Feature Store in teradataml

Feature Store, also known as  Teradata Enterprise Feature Store  or Teradata EFS, is a centralized repository
to store and manage the lifecycle of Features that are used in ML models. Along with Features, you can also
store the components required for building an ML model, preventing the need to modify the ML pipeline even
you modify underlying Features or a Data Source which are used while building the ML model.
**Advantages of and topics that detail Feature Store follow:**
* The same Features can be used for multiple ML models.
* FeatureStore decouples model generation from Feature Engineering. So, Data Scientists can focus on
* model generation while Data Engineers can focus on Feature Engineering.
* Since Features are stored and versioned with a name, you can improve the trust and reliability on ML
* model. This is helpful when you work with ML models containing that include a large number of Features.
* Feature Store Components
* Setting up FeatureStore
* Use FeatureStore in teradataml Analytic Functions
## Feature Store Components
**The Feature store contains the following components:**
* Feature
* Entity
* Data Source
* Feature Group
* Feature Store
### Feature
The Feature component is synonymous with features used in machine learning (ML) models. This
component stores all the details of a Feature, and is required for building an ML model. The following details
**can be stored in Feature:**
* Name of column from the table containing the data
* Data type (e.g., string, number, date)
* Whether it is of type Categorical or Continuous
You can also add a description and tags.
### Entity
Every dataset holds one or more unique identifiers for each row in the dataset. For example, a weekly retail
store sales holds week number to identify details for a week, and a local telephone number along with area
20

code holds uniqueness for telephone number. If you observe, the dataset in first example has one entity
to represent unique record, and the dataset for the second example includes two entities to represent the
unique record.
Entity in FeatureStore serves the same purpose; it is identified with a name, and stores the column names
that identifies the uniqueness of data.
### Data Source
This component stores the source of the data. Teradata EFS always refers to a SQL as a data source. You
can identify a data source with a name and it holds the corresponding SQL as data source.
The Data Source component also stores information about the name of the column to indicate when the
corresponding record is created. The Recorded Time column in the following example of a patient profile
contains the date and time at which the patient readings are taken.
Patient ID
Recorded Time
Pregnancies
Age
BMI
61
2024-04-10 11:10:59.000000
8
39
33
0
2024-04-11 11:10:58.000000
6
50
34
40
2024-04-13 11:10:58.000000
3
26
34
99
2024-04-12 11:10:55.000000
1
31
50
Data Source can also store this column information along with the source. This information is helpful when
you want to generate a model on historic data. For example, you can get a dataset for the patients who took
the test between 10th April and 12th April and feed it to your ML model.
### Feature Group
Identified with a name, it logically combines Feature, Entity, and Data Source. You can form a group with at
least one Feature, one Entity, and one Data Source. Each component that comprises a feature group can
be part of multiple Feature Groups.
You can always add or remove Features, Entities, and Data Sources from a Feature Group.
Since you store the details of Features, Entity, and DataSource in a Feature Group, you can always access
the details of individual components from the Feature Group. For example, you can access the description
of Data Source from Feature Group.
While generating an ML model, you may need to generate an ML model from two or more different groups.
In such cases, you can combine different groups to form a new group. Then use the new group to feed your
ML model.
Combining multiple Feature Groups creates a unique Feature Group that includes the Features and Data
Source which has data for all of the combined Features. See  Combining one or more Feature Groups into
a single FeatureGroup  for prerequisites and an example.
20: Feature Store in teradataml

### Feature Store
The Feature Store component stores Features, Entities, Data Sources, and Feature Groups in a database
called repo.
You can retrieve any of these components from Feature Store and use them in your ML model.
**Prerequisites for using Feature Store:**
* The repo must be set up.
* Refer to the setup method in  FeatureStore Methods .
* You must have appropriate permissions to access the repo and the Feature Store components.
* See  Manage Access for FeatureStore .
### Feature Object
Feature stores all the details of the Feature. The following example explains creating a categorical feature
named 'sales_Feb' for column 'Feb' from 'sales' DataFrame.
```python
from teradataml import DataFrame
df = DataFrame("sales")
df
df
Feb    Jan    Mar    Apr    datetime
accounts
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
from teradataml import Feature, FeatureType
feature = Feature('sales_Feb', column=df.Feb,
feature_type=FeatureType.CONTINUOUS, description='Feature for February sales.',
tags=['sales', 'Monthly sales'])
feature
Feature(name=sales_Feb)

```
20: Feature Store in teradataml

### Properties
name
    * Specifies unique name of the Feature in Feature Store.
    * Example:
```python
feature.name
'sales_Feb'

```
column_name
    * Specifies name of the column the Feature belongs to.
    * Example:
```python
feature.column_name
'Feb'

```
description
    * Specifies the description for the Feature.
    * Example:
```python
feature.description
'Feature for February sales.'

```
tags
    * Specifies the tags for the Feature.
    * Example:
```python
feature.tags
['sales', 'Monthly sales']
```
data_type
    * Specifies the data type for the Feature.
    * Example:
20: Feature Store in teradataml

```python
feature.data_type
FLOAT()
```
feature_type
    * Specifies whether a Feature is continuous or categorical.
    * Continuous example:
```python
feature.feature_type
<FeatureType.CONTINUOUS: 1>
```
status
    * Specifies the status of the Feature (Active or Inactive).
    * Active: It will participate during model generation.
    * Inactive: It won't participate even though it is available in FeatureStore.
    * Example:
```python
feature.status
<FeatureStatus.ACTIVE: 1>
```
### Entity Object
Entity stores all details of an Entity. The following example explains creating an Entity for column account
in sales data.
```python
df = DataFrame("sales")
df
Feb    Jan    Mar    Apr    datetime
accounts
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
from teradataml import Entity
entity = Entity('sales_accounts', df.accounts, description='Monthly
sales Entity')
```
20: Feature Store in teradataml

```python
entity
Entity(name=sales_accounts)

```
### Properties
name
    * Specifies the name of the Entity in the Feature Store.
    * Example:
```python
entity.name
'sales_accounts'

```
columns
    * Specifies the names of the columns that uniquely identify the data.
    * Example:
```python
entity.columns
['accounts']
```
description
    * Specifies the description for the Entity.
    * Example:
```python
entity.description
'Monthly sales Entity'

```
### DataSource Object
DataSource stores all details of a Data Source. Data Source can refer to either teradataml DataFrame or
SQL query as a source. The following example explains creating a Data Source for sales data.
```python
df = DataFrame("sales")
df
Feb    Jan    Mar    Apr    datetime
accounts
```
20: Feature Store in teradataml

```python
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
from teradataml import DataSource
ds = DataSource('Sales_Data', df, timestamp_col_name='datetime',
description="Montly sales source")
ds
DataSource(Sales_Data)

```
### Properties
name
    * Specifies unique name of the Data Source in the Feature Store.
    * Example:
```python
ds.name
'Sales_Data'
```
timestamp_col_name
    * Specifies the name of the column containing the record created time in the dataset.
    * Example:
```python
ds.timestamp_col_name
'datetime'
```
source
    * Specifies the source details of the dataset.
    * Example:
```python
ds.source
'select * from "sales"'
```
description
    * Specifies the description of DataSource.
20: Feature Store in teradataml

* Example:
```python
ds.description
'Monthly sales source'
```
### FeatureGroup Object
FeatureGroup contains details of the Feature Group. You can create multiple Feature Groups, as well as
combine Feature Groups to create a new Feature Group. See  Combining one or more Feature Groups into
a single FeatureGroup .
The following example explains a Feature Group for sales data.
```python
df = DataFrame("sales")
df
Feb    Jan    Mar    Apr    datetime
accounts
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
```
Create a FeatureGroup for 'sales' DataFrame.
```python
from teradataml import DataSource, Entity, Feature, FeatureGroup
f1 = Feature('sales_jan', df.Jan, description='January sales')
f2 = Feature('sales_feb', df.Feb, description='February sales')
f3 = Feature('sales_mar', df.Mar, description='March sales')
f4 = Feature('sales_apr', df.Apr, description='April sales')
entity=Entity('sales', df.accounts)
ds=DataSource('sales', df, timestamp_col_name='datetime')
fg = FeatureGroup('sales', features=[f1, f2, f3, f4], entity=entity,
data_source=ds, description='sales group')
fg
FeatureGroup(sales, features=[Feature(name=sales_jan),
Feature(name=sales_feb), Feature(name=sales_mar), Feature(name=sales_apr)],
entity=Entity(name=sales), data_source=DataSource(name=sales))

```
20: Feature Store in teradataml

### Properties
features
    * Returns a list of features from FeatureGroup.
    * Example getting the associated features from FeatureGroup 'fg':
```python
fg.features
[Feature(name=sales_jan),
Feature(name=sales_feb),
Feature(name=sales_mar),
Feature(name=sales_apr)]

```
labels
    * Returns the list of features marked as labels from FeatureGroup.
    * Note:
    * Check methods  set_labels()  and  reset_labels()  to set and reset Features as
    * labels for the current session.
    * Example:
```python
fg.labels
[]
```
entity
    * Returns the Entity from FeatureGroup.
    * Example:
```python
fg.entity
Entity(name=sales)

```
data_source
    * Returns the DataSource from FeatureGroup.
    * Example:
20: Feature Store in teradataml

```python
fg.data_source
DataSource(name=sales)

```
description
    * Returns the description for FeatureGroup.
    * Example:
```python
fg.description
'sales group'

```
### Methods
from_DataFrame
    * Creates FeatureGroup from teradataml DataFrame.
    * Example creating a FeatureGroup from 'sale's DataFrame, and specifying 'accounts' column
    * as entity and 'datetime' column as timestamp column:
```python
df = DataFrame("sales")
df
Feb    Jan    Mar    Apr    datetime
accounts
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
fg = FeatureGroup.from_DataFrame(
name='sales',
entity_columns='accounts',
df=df,
timestamp_col_name='datetime'
fg
FeatureGroup(sales, features=[Feature(name=Feb),
Feature(name=Jan), Feature(name=Mar), Feature(name=Apr)],
```
20: Feature Store in teradataml

```python
entity=Entity(name=sales), data_source=DataSource(name=sales))

```
from_query
    * Creates FeatureGroup from SQL query.
    * Example creating a FeatureGroup from query 'SELECT * FROM SALES', and specifying
    * 'accounts' column as entity and 'datetime' column as timestamp column:
```python
query = 'SELECT * FROM SALES'
fg = FeatureGroup.from_query(
name='sales',
entity_columns='accounts',
query=query,
timestamp_col_name='datetime'
fg
FeatureGroup(sales, features=[Feature(name=Feb),
Feature(name=Jan), Feature(name=Mar), Feature(name=Apr)],
entity=Entity(name=sales), data_source=DataSource(name=sales))

```
apply
    * Updates FeatureGroup with Feature, Entity, or DataSource. You can also use this method to
    * add new Features to an existing FeatureGroup.
    * Example creates a FeatureGroup for January sales, creates a new Feature, and adds it to
    * the FeatureGroup:
```python
df = DataFrame("sales")
from teradataml import DataSource, Entity, Feature, FeatureGroup
f1 = Feature('sales_jan', df.Jan, description='January sales')
entity = Entity('sales', df.accounts)
ds = DataSource('sales', df, timestamp_col_name='datetime')
fg = FeatureGroup('sales', features=f1,
entity=entity, data_source=ds)
fg
FeatureGroup(sales, features=[Feature(name=sales_jan)],
entity=Entity(name=sales), data_source=DataSource(name=sales))
```
20: Feature Store in teradataml

```python
f2 = Feature('sales_feb', df.Feb, description='February sales')
fg.apply(f2)
True
fg
FeatureGroup(sales, features=[Feature(name=sales_jan),
Feature(name=sales_feb)],
entity=Entity(name=sales), data_source=DataSource(name=sales))
```
remove
    * Removes a Feature, Entity, or DataSource from a FeatureGroup.
    * Example:
```python
fg.remove(f2)
True
fg
FeatureGroup(sales, features=[Feature(name=sales_jan)],
entity=Entity(name=sales), data_source=DataSource(name=sales))
```
set_labels
    * Sets the one or more Feature as labels for FeatureGroup in the current session.
    * If you lose the session, you need to set the labels again in a new session.
    * Use this method to refer to FeatureGroup in your ML models.
    * Example creating a FeatureGroup from query 'SELECT * FROM SALES', and specifying
    * 'accounts' column as entity column and 'datetime' column as timestamp column:
```python
query = 'SELECT * FROM SALES'
fg = FeatureGroup.from_query(
name='sales',
entity_columns='accounts',
query=query,
timestamp_col_name='datetime'
fg.features
[Feature(name=Feb), Feature(name=Jan),
Feature(name=Mar), Feature(name=Apr)]
```
20: Feature Store in teradataml

```python
fg.labels
[]
```
    * Set 'Apr' as label to generate an ML model to predict April sales from Jan, Feb, and Mar sales.
```python
fg.set_labels('Apr')
True
fg.features
[Feature(name=Feb), Feature(name=Jan), Feature(name=Mar)]
fg.labels
Feature(name=Apr)
```
reset_labels
    * Removes all labels set for FeatureGroup in the current session.
    * Example that resets labels, checks features, and resets labels again:
```python
fg.reset_labels()
True
fg.features
[Feature(name=Feb), Feature(name=Jan),
Feature(name=Mar), Feature(name=Apr)]
fg.labels
[]
fg.labels
[]
```
Combining one or more Feature Groups into a single FeatureGroup
You can combine multiple FeatureGroup objects using the + operator. Combining multiple FeatureGroup
objects creates a new FeatureGroup object.
**Prerequisites for combining multiple Feature Groups:**
* Entities must be identical.
* If the Data Source includes timestamp column details, then both data sources should have same
* timestamp column names..
Let’s look at an example to see how this works.
20: Feature Store in teradataml

### Create two different FeatureGroups
Before creating two FeatureGroups, let’s look at two different data sets: patient_profile
and medical_readings.
Patient Profile
```python
load_example_data('dataframe', 'patient_profile')
patient_profile_df = DataFrame('patient_profile')
patient_profile_df
record_timestamp  pregnancies  age   bmi  skin_thickness
patient_id
17          2024-04-10 11:10:59.000000            7   31  29.6             0.0
34          2024-04-10 11:10:59.000000           10   45  27.6            31.0
13          2024-04-10 11:10:59.000000            1   59  30.1            23.0
53          2024-04-10 11:10:59.000000            8   58  33.7            34.0
11          2024-04-10 11:10:59.000000           10   34  38.0             0.0
51          2024-04-10 11:10:59.000000            1   26  24.2            15.0
32          2024-04-10 11:10:59.000000            3   22  24.8            11.0
15          2024-04-10 11:10:59.000000            7   32  30.0             0.0
99          2024-04-10 11:10:59.000000            1   31  49.7            51.0
0           2024-04-10 11:10:59.000000            6   50  33.6            35.0
```
Medical Readings
```python
load_example_data('dataframe', 'medical_readings')
medical_readings_df = DataFrame('medical_readings')
medical_readings_df
record_timestamp  glucose  blood_pressure  insulin
diabetes_pedigree_function  outcome
patient_id
17          2024-04-10 11:10:59.000000      107              74
0                       0.254        1
34          2024-04-10 11:10:59.000000      122              78
0                       0.512        0
13          2024-04-10 11:10:59.000000      189              60
846                       0.398        1
53          2024-04-10 11:10:59.000000      176              90
300                       0.467        1
11          2024-04-10 11:10:59.000000      168              74
0                       0.537        1
```
20: Feature Store in teradataml

```python
51          2024-04-10 11:10:59.000000      101              50
36                       0.526        0
32          2024-04-10 11:10:59.000000       88              58
54                       0.267        0
15          2024-04-10 11:10:59.000000      100               0
0                       0.484        1
99          2024-04-10 11:10:59.000000      122              90
220                       0.325        1
0           2024-04-10 11:10:59.000000      148              72
0                       0.627        1

```
### Create two FeatureGroups for the two datasets
Let's first create individual FeatureGroups.
```python
patient_profile_fg = FeatureGroup.from_DataFrame(
name='PatientProfile',
df=patient_profile_df,
entity_columns='patient_id',
timestamp_col_name='record_timestamp'
medical_readings_fg = FeatureGroup.from_DataFrame(
name='MedicalReadings',
df=medical_readings_df,
entity_columns='patient_id',
timestamp_col_name='record_timestamp'
print(patient_profile_fg.features)
[Feature(name=pregnancies), Feature(name=age),
Feature(name=bmi), Feature(name=skin_thickness)]
print(medical_readings_fg.features)
[Feature(name=glucose), Feature(name=blood_pressure), Feature(name=insulin),
Feature(name=diabetes_pedigree_function), Feature(name=outcome)]
```
### Combine the two FeatureGroups
```python
new_fg = patient_profile_fg + medical_readings_fg
```
20: Feature Store in teradataml

### Examine the combined FeatureGroup properties
```python
print(new_fg.name)
'PatientProfile_MedicalReadings'
print(new_fg.features)
[Feature(name=pregnancies), Feature(name=age), Feature(name=bmi),
Feature(name=skin_thickness), Feature(name=glucose),
Feature(name=blood_pressure), Feature(name=insulin),
Feature(name=diabetes_pedigree_function), Feature(name=outcome)]
print(new_fg.entity)
Entity(name=PatientProfile_MedicalReadings)
print(new_fg.data_source)
DataSource(name=PatientProfile_MedicalReadings)
```
### FeatureStore Object
FeatureStore store the details of the Feature Store. Every Feature Store points to a single repo so you can
create a FeatureStore object by passing the repo name to it.
You can access, modify, or delete Feature Store components using the FeatureStore object. See  Setting
up FeatureStore .
The following example refers to database LabReports as repo to store all Feature Store components.
```python
from teradataml import FeatureStore
fs = FeatureStore('LabReports')
```
## Setting up FeatureStore
**Use the following topics to set up FeatureStore for use with ML models:**
* FeatureStore Methods
* Add a component to FeatureStore
* Get the FeatureStore Components
* Modify an existing component in FeatureStore
* Search for components in FeatureStore
* Get dataset from FeatureStore
* Remove components from FeatureStore
* Disable and enable Features without Removing them from FeatureStore
* Manage Access for FeatureStore
20: Feature Store in teradataml

### FeatureStore Methods
setup
    * Sets up Feature Store for a repo. Setting up Feature Store creates the corresponding
    * database if it is not available, then it will create database objects in the corresponding
    * database. You must have the following database privileges set up Feature Store (create the
    * corresponding database):
    * CREATE DATABASE privilege
    * CREATE TABLE privilege
    * When you create a database for FeatureStore, you can specify the perm space and spool
    * space to be allocated to new database. Refer to  Teradata Package for Python Function
    * Reference  for FeatureStore details.
    * Example:
```python
from teradataml import FeatureStore
fs = FeatureStore("vfs_v1")
fs.setup()
True

```
repair
    * If any database component is removed, then repair() tries to create those
    * missing components.
    * Note:
    * repair() only tries to create missing database objects and not the associated data for the
    * corresponding component.
    * Example:
```python
from teradataml import FeatureStore
fs = FeatureStore("vfs_v1")
fs.repair()
True

```
20: Feature Store in teradataml

list_repos
    * Specifies the available repos that are configured for FeatureStore.
    * Example:
```python
from teradataml import FeatureStore
FeatureStore.list_repos()
repos
0  vfs_v1
```
### Add a component to FeatureStore
Use the apply() method to register the Feature, Entity, DataSource or FeatureGroup to a repo.
**Note:**
    * If you add FeatureGroup to FeatureStore, the underlying Features, Entity and DataSource are
    * also added.
    * If any of the components already exists, then apply method updates the corresponding
    * component properties.
### Example: Adding a Feature to FeatureStore
Create a Feature for column 'Feb' from 'sales' DataFrame and register with repo 'vfs_v1'.
```python
from teradataml import Feature, FeatureStore
feature = Feature('sales:Feb', df.Feb)
fs = FeatureStore('vfs_v1')
fs.apply(feature)
True

```
### Example: Adding an Entity to FeatureStore
Create an Entity for 'sales' DataFrame and register with repo 'vfs_v1'.
```python
from teradataml import Entity, FeatureStore
```
20: Feature Store in teradataml

```python
entity = Entity('sales:accounts', df.accounts)
fs = FeatureStore('vfs_v1')
fs.apply(entity)
True

```
### Example: Adding a DataSource to FeatureStore
Create a DataSource for 'sales' DataFrame and register with repo 'vfs_v1'.
```python
from teradataml import DataSource, FeatureStore
ds = DataSource('Sales_Data', df)
fs = FeatureStore('vfs_v1')
fs.apply(ds)
True

```
### Example: Adding a FeatureGroup to FeatureStore
Create a FeatureStore with all objects created using the components in the previous examples and register
with repo 'vfs_v1'.
```python
from teradataml import FeatureGroup, FeatureStore
fg = FeatureGroup('Sales',
features=feature,
entity=entity,
data_source=data_source)
fs = FeatureStore('vfs_v1')
fs.apply(fg)
True

```
20: Feature Store in teradataml

### Example: Create a Feature Group and add the Feature Group and associated
### Components to a FeatureStore
```python
load_example_data("byom", "iris_test")
iris_df = DataFrame("iris_test")
from teradataml import FeatureGroup, FeatureStore
fg = FeatureGroup.from_DataFrame('iris_data',
df=iris_df, entity_columns='id')
fs = FeatureStore('vfs_v1')
fs.apply(fg)
True

```
### Get the FeatureStore Components
### get_feature
get_feature() retrieves the object of Feature from FeatureStore.
Example gets the feature 'sales_mar' from repo 'vfs_v1'.
```python
feature = fs.get_feature('sales_mar')
feature
Feature(name=sales_mar)

```
### get_entity
get_entity() retrieves the object of Entity from FeatureStore.
Example get the Entity 'admissions_id' from repo 'vfs_v1'.
```python
entity = fs.get_entity('admissions_id')
entity
Entity(name=admissions_id)

```
20: Feature Store in teradataml

### get_data_souorce
get_data_source() retrieves the object of DataSource from FeatureStore.
Example gets the DataSource 'admissions' from repo 'vfs_v1'.
```python
ds = fs.get_data_source('admissions')
ds
DataSource(name=admissions)

```
### get_feature_group
get_feature_group() retrieves the object of FeatureGroup from FeatureStore.
Example gets FeatureGroup with group name 'sales' from repo 'vfs_v1'.
```python
fg = fs.get_feature_group('sales')
fg
FeatureGroup(sales, features=[Feature(name=Jan),
Feature(name=Feb), Feature(name=Apr), Feature(name=Mar)],
entity=Entity(name=sales), data_source=DataSource(name=sales))

```
### Modify an existing component in FeatureStore
**You can use apply() to modify an existing component:**
1.
* Get the corresponding component.
2.
* Modify the corresponding component's property.
3.
* Pass the same component again to apply().
### Usage
* Do not not modify  name  of any component. If you modify  name  and push it to FeatureStore, a new
* component will get created with that name.
* You can modify multiple properties of a component and add it back to FeatureStore once all the
* modifications are done.
* If you get a FeatureGroup, you can modify the underlying Features, Entity, or DataSource. If you
* push FeatureGroup after modification, then underlying Features, Entity, or DataSource will also
* get modified.
20: Feature Store in teradataml

### Example: Modify a Feature
Get the feature 'sales_mar' from repo 'vfs_v1', modify the description, and update it to FeatureStore.
```python
feature = fs.get_feature('sales_mar')
feature.description
'Feature of sales.'

feature.description = 'Feature for March sales.'
fs.apply(feature)
True
fs.get_feature('sales_mar').description
'Feature for March sales.'
```
### Example: Modify a FeatureGroup and underlying DataSource
Get the FeatureGroup with group name 'sales' from repo 'vfs_v1'.
```python
fg = fs.get_feature_group('sales')
```
Update the FeatureGroup description.
```python
fg.description = 'Sales Group'
```
Update the associated DataSource description.
```python
fg.data_source.description = 'sales data source.'
```
Update the details to repo.
```python
fs.apply(fg)
True
```
Verify the updated details.
```python
fg = fs.get_feature_group('sales')
fg.description
'Sales Group'
```
20: Feature Store in teradataml

```python
fg.data_source.description
'sales data source.'
```
### Search for components in FeatureStore
Use the following functions to search for the relevant component in FeatureStore.
### list_features
Use list_features() to retrieve available Features in the teradataml DataFrame. You can use any filter that
is supported by teradataml DataFrame to search Feature details.
**Note:**
You can use the same API to list down archived Features. See  Remove components
from FeatureStore .
**Example:**
```python
fs.list_features()
column_name description               creation_time modified_time  tags
data_type feature_type  status group_name
name
Mar          Mar        None  2024-09-30 11:21:43.314118          None  None
BIGINT   CONTINUOUS  ACTIVE      sales
Jan          Jan        None  2024-09-30 11:21:42.655343          None  None
BIGINT   CONTINUOUS  ACTIVE      sales
Apr          Apr        None  2024-09-30 11:21:44.143402          None  None
BIGINT   CONTINUOUS  ACTIVE      sales
Feb          Feb        None  2024-09-30 11:21:41.542627          None
None     FLOAT   CONTINUOUS  ACTIVE      sales

```
### list_entities
Use list_entities() to retrieve available Entities in the teradataml DataFrame. You can use any filter that is
supported by teradataml DataFrame to search Entity details.
**Note:**
You can use the same API to list down archived Entities. See  Remove components
from FeatureStore .
**Example:**
20: Feature Store in teradataml

```python
fs.list_entities()
description
name  entity_column
sales accounts             None

```
### list_data_sources
Use list_data_sources() to retrieve available Data Sources in the teradataml DataFrame. You can use any
filter that is supported by teradataml DataFrame to search Data Source details.
**Note:**
You can use the same API to list down archived Data Sources. See  Remove components
from FeatureStore .
**Example:**
```python
fs.list_data_sources()
description timestamp_col_name                            source
name
admissions        None               None  select * from "admissions_train"

```
### list_feature_groups
Use list_feature_groups() to retrieve available Feature Groups in the teradataml DataFrame. You can use
any filter that is supported by teradataml DataFrame to search Feature Group details.
**Note:**
You can use the same API to list down archived Feature Groups. See  Remove components
from FeatureStore .
```python
fs.list_feature_groups()
description data_source_name entity_name
name
admissions        None       admissions  admissions

```
20: Feature Store in teradataml

### Get dataset from FeatureStore
### get_dataset
Use get_dataset() to get teradataml DataFrame from the group name.
### Feed historic data to your ML models
You can feed only a portion of a dataset to ML model by using teradataml filter options. get_dataset()
returns a teradataml DataFrame using Data Source for Feature Group. Once you get the teradataml
DataFrame, you can use various filter options provided by teradataml to filter only specific records and then
feed the filtered DataFrame to your ML model.
### Example 1: Get dataset for 'sales' group
```python
fs = FeatureStore("vfs_v1")
patient_profile_fg = FeatureGroup.from_DataFrame(
name='PatientProfile',
df=patient_profile_df,
entity_columns='patient_id',
timestamp_col_name='record_timestamp'
fs.apply(patient_profile_fg)
True
df = fs.get_dataset(patient_profile_fg.name)
df
record_timestamp   bmi  age  skin_thickness  pregnancies
patient_id
19          2024-04-10 11:10:59.000000  34.6   32            30.0            1
59          2024-04-10 11:10:59.000000  41.5   22            41.0            0
38          2024-04-10 11:10:59.000000  38.2   27            42.0            2
78          2024-04-10 11:10:59.000000  43.2   26             0.0            0
36          2024-04-10 11:10:59.000000  33.2   35             0.0           11
97          2024-04-10 11:10:59.000000  20.4   22            18.0            1
57          2024-04-10 11:10:59.000000  46.8   31            60.0            0
80          2024-04-10 11:10:59.000000  22.4   22            13.0            3
40          2024-04-10 11:10:59.000000  34.0   26            25.0            3
61          2024-04-10 11:10:59.000000  32.9   39             0.0            8
```
20: Feature Store in teradataml

### Example 2: Combine two Feature Groups and get the dataset for the combined
### Feature Group
```python
patient_profile_fg = FeatureGroup.from_DataFrame(
name='PatientProfile',
df=patient_profile_df,
entity_columns='patient_id',
timestamp_col_name='record_timestamp'
fs.apply(patient_profile_fg)
True
medical_readings_fg = FeatureGroup.from_DataFrame(
name='MedicalReadings',
df=medical_readings_df,
entity_columns='patient_id',
timestamp_col_name='record_timestamp'
combined_fg = patient_profile_fg + medical_readings_fg
fs.apply(combined_fg)
fs.get_dataset(combined_fg.name)
patient_id            record_timestamp  outcome  age   bmi  skin_thickness
diabetes_pedigree_function  blood_pressure  insulin  glucose  pregnancies
0          17  2024-04-10 11:10:59.000000        1   31  29.6
0.0                       0.254              74        0      107            7
1          34  2024-04-10 11:10:59.000000        0   45  27.6
31.0                       0.512              78        0      122           10
2          13  2024-04-10 11:10:59.000000        1   59  30.1
23.0                       0.398              60      846      189            1
3          61  2024-04-10 11:10:59.000000        1   39  32.9
0.0                       0.270              72        0      133            8
4          19  2024-04-10 11:10:59.000000        1   32  34.6
30.0                       0.529              70       96      115            1
5          80  2024-04-10 11:10:59.000000        0   22  22.4
13.0                       0.140              44        0      113            3
6          59  2024-04-10 11:10:59.000000        0   22  41.5
41.0                       0.173              64      142      105            0
7          38  2024-04-10 11:10:59.000000        1   27  38.2
42.0                       0.503              68        0       90            2
8          40  2024-04-10 11:10:59.000000        0   26  34.0
```
20: Feature Store in teradataml

```python
25.0                       0.271              64       70      180            3
9          15  2024-04-10 11:10:59.000000        1   32  30.0
0.0                       0.484               0        0      100            7
```
### Remove components from FeatureStore
Use the following archive or delete functions for a component in FeatureStore.
**Note:**
    * You cannot archive a component which is a part of Feature Group. You will have to remove the
    * component from Feature Group using FeatureGroup.remove(), upload modified Feature Group
    * using FeatureStore.apply(), then archive the component.
    * The delete function removes only the archived component. It won't act on the non-
    * archived component.
### archive_feature
archive_feature() archives a feature from FeatureStore.
**Note:**
Archiving a Feature will not remove the Feature completely from FeatureStore. They are marked as
unavailable for any further processing. You can still see the archived Features using list_features()
by setting the additional argument  archived  to True.
**Example:**
```python
from teradataml import DataFrame, Feature, FeatureStore
df = DataFrame("sales")
Create Feature for Column 'Feb'.
feature = Feature(name="sales_data_Feb", column=df.Feb)
Create FeatureStore for the repo 'staging_repo'.
fs = FeatureStore("staging_repo")
Apply the Feature to FeatureStore.
fs.apply(feature)
True
List the available Features.
Note that group_name is None. So 'sales_data_Feb' feature is not associated
with any group.
```
20: Feature Store in teradataml

```python
Since it is not part of any group, it can be archived.
fs.list_features()
column_name description               creation_time
modified_time  tags data_type feature_type  status group_name
name
sales_data_Feb         Feb        None  2024-10-03 18:21:03.720464
None  None     FLOAT   CONTINUOUS  ACTIVE       None
Archive Feature with name "sales_data_Feb".
fs.archive_feature(feature=feature)
Feature 'sales_data_Feb' is archived.
True
List the available Features after archive.
fs.list_features()
Empty DataFrame
Columns: [column_name, description, creation_time, modified_time, tags,
data_type, feature_type, status, group_name]
Index: []

List all the archived Features.
fs.list_features(archived=True)
name column_name description               creation_time
modified_time  tags data_type feature_type  status
archived_time group_name
0  sales_data_Feb         Feb        None  2024-10-03 18:21:03.720464
None  None     FLOAT   CONTINUOUS  ACTIVE  2024-09-30 11:30:49.160000      sales

```
### delete_feature
delete_feature() deletes an archived Feature from FeatureStore.
**Example:**
```python
fs.delete_feature(feature=feature)
Feature 'sales_data_Feb' is deleted.
True

```
### archive_entity
archive_entity() archives an entity from FeatureStore.
20: Feature Store in teradataml

**Note:**
Archiving an Entity will not remove the Entity completely from Feature Store. They are marked as
unavailable for any further processing. You can still see the archived Entities using list_entities() by
setting the additional argument  archived  to True.
**Example archiving an Entity and listing all archived Entities in repo 'vfs_v1':**
**Note:**
An Entity cannot be archived if it is a part of FeatureGroup. You must create another Entity, update
FeatureGroup with other Entity, then archive Entity 'sales'.
```python
entity = Entity('store_sales', columns=df.accounts)
Update new entity to FeatureGroup.
fg.apply(entity)
Update FeatureGroup to FeatureStore. This will update Entity
from 'sales' to 'store_sales' for FeatureGroup 'sales'.
fs.apply(fg)
True
Let's archive Entity 'sales' since it is not part of any FeatureGroup.
fs.archive_entity('sales')
Entity 'sales' is archived.
True

List the archived entities.
fs.list_entities(archived=True)
name description               creation_time modified_time
archived_time entity_column
0  sales        None  2024-10-18 05:41:36.932856          None  2024-10-18
05:50:00.930000      accounts

```
### delete_entity
delete_entity() deletes an archived Entity from FeatureStore.
**Example:**
```python
fs.delete_entity(entity=entity.name)
Entity 'sales_data' is deleted.
```
20: Feature Store in teradataml

```python
True

```
### archive_data_source
archive_data_source() archive a Data Source from FeatureStore.
**Note:**
Archiving a Data Source will not remove the Data Source completely from Feature Store. They are
marked as unavailable for any further processing. You can still see the archived Data Sources using
list_data_sources() by setting the additional argument  archived  to True.
**Example:**
```python
Archive a Data Source and list all the archived DataSources in the
repo 'vfs_v1'.
Let's first archive the DataSource.
fs.archive_data_source('admissions')
DataSource 'admissions' is archived.
True
List archived DataSources.
fs.list_data_sources(archived=True)
description timestamp_col_name
source               archived_time
name
admissions        None               None  select * from "admissions_train"
2024-09-30 12:05:39.220000

```
### delete_data_source
delete_data_source() deletes an archived Data Source from FeatureStore.
**Example:**
```python
fs.delete_data_source("sales_data")
DataSource 'sales_data' is deleted.
True

```
### archive_feature_group
archive_feature_group() archives a Feature Group from FeatureStore.
20: Feature Store in teradataml

**Note:**
Archiving a Feature Group will not remove the Feature Group completely from Feature Store. They
are marked as unavailable for any further processing. You can still see the archived Feature Groups
using list_feature_groups() by setting the additional argument  archived  to True.
**Example:**
```python
from teradataml import FeatureGroup, FeatureStore, load_example_data
admissions=DataFrame("admissions_train")
Create FeatureStore for repo 'vfs_v1'.
fs = FeatureStore("vfs_v1")
Create a FeatureGroup from DataFrame.
fg = FeatureGroup.from_DataFrame("admissions",
df=admissions, entity_columns='id')
Apply FeatureGroup to FeatureStore.
fs.apply(fg)
True
List all the effective FeatureGroups in the repo 'vfs_v1'.
fs.list_feature_groups()
description data_source_name entity_name
name
admissions        None       admissions  admissions

Let's archive the FeatureGroup.
fs.archive_feature_group("admissions")
True

List archived FeatureGroups.
fs.list_feature_groups(archived=True)
name description data_source_name
entity_name               archived_time
0  admissions        None       admissions  admissions
2024-09-30 12:05:39.220000

List archived Features.
fs.list_features(archived=True)
```
20: Feature Store in teradataml

```python
name  column_name description  tags data_type
feature_type  status               creation_time modified_time
archived_time  group_name
0          gpa          gpa        None  None     FLOAT   CONTINUOUS  ACTIVE
2024-11-05 06:45:10.368228          None  2024-11-05 06:47:05.480000  admissions
1     admitted     admitted        None  None   INTEGER   CONTINUOUS  ACTIVE
2024-11-05 06:45:10.431708          None  2024-11-05 06:47:05.480000  admissions
2      masters      masters        None  None   VARCHAR   CONTINUOUS  ACTIVE
2024-11-05 06:45:10.306466          None  2024-11-05 06:47:05.480000  admissions
3        stats        stats        None  None   VARCHAR   CONTINUOUS  ACTIVE
2024-11-05 06:45:10.389375          None  2024-11-05 06:47:05.480000  admissions
4  programming  programming        None  None   VARCHAR   CONTINUOUS  ACTIVE
2024-11-05 06:45:10.410277          None  2024-11-05 06:47:05.480000  admissions
List archived Entities.
fs.list_entities(archived=True)
name description               creation_time
modified_time               archived_time entity_column
0  admissions        None  2024-11-05 06:45:10.453333          None  2024-11-05
06:47:05.550000            id
List archived Data Sources.
fs.list_data_sources(archived=True)
name description timestamp_col_name
source               creation_time modified_time               archived_time
0  admissions        None               None  select * from "admissions_train"
2024-11-05 06:45:10.578087          None  2024-11-05 06:47:05.600000
```
### delete_feature_group
delete_feature_group() deletes an archived Feature Group from FeatureStore.
**Note:**
Unlike archive_feature_group(), delete_feature_group() won’t delete underlying Features, Data
Source, and Entity.
```python
from teradataml import DataFrame, FeatureGroup, FeatureStore
df = DataFrame("sales")
Create FeatureGroup from teradataml DataFrame.
fg = FeatureGroup.from_DataFrame(name="sales", entity_columns="accounts",
df=df, timestamp_col_name="datetime")
Create FeatureStore for the repo 'staging_repo'.
```
20: Feature Store in teradataml

```python
fs = FeatureStore("staging_repo")
Apply FeatureGroup to FeatureStore.
fs.apply(fg)
True
Let's first archive FeatureGroup with name "sales".
fs.archive_feature_group(feature_group='sales')
FeatureGroup 'sales' is archived.
True
Delete FeatureGroup with name "sales".
fs.delete_feature_group(feature_group='sales')
FeatureGroup 'sales' is deleted.
True

```
### Disable and enable Features without Removing them
### from FeatureStore
### set_features_inactive
Use set_feature_inactive() to mark the Feature status as inactive. If a Feature is inactive, then it won't be
part of any further processing. Setting a Feature to Active includes it in processing again.
**Example:**
```python
from teradataml import DataFrame, DataSource,
FeatureStore, load_example_data
Create DataFrame on admissions data.
df = DataFrame("admissions_train")
df
masters   gpa     stats programming  admitted
id
34     yes  3.85  Advanced    Beginner         0
32     yes  3.46  Advanced    Beginner         0
11      no  3.13  Advanced    Advanced         1
40     yes  3.95    Novice    Beginner         0
38     yes  2.65  Advanced    Beginner         1
36      no  3.00  Advanced      Novice         0
7      yes  2.33    Novice      Novice         1
26     yes  3.57  Advanced    Advanced         1
19     yes  1.98  Advanced    Advanced         0
```
20: Feature Store in teradataml

```python
13      no  4.00  Advanced      Novice         1

Create FeatureGroup from DataFrame df.
fg = FeatureGroup.from_DataFrame(name='admissions',
df=df, entity_columns='id')
Apply the FeatureGroup to FeatureStore 'vfs_v1'.
fs = FeatureStore('vfs_v1')
fs.apply(fg)
True
Get FeatureGroup 'admissions' from FeatureStore.
fg = fs.get_feature_group('admissions')
fg
FeatureGroup(admissions, features=[Feature(name=masters),
Feature(name=programming), Feature(name=admitted),
Feature(name=stats), Feature(name=gpa)],
entity=Entity(name=admissions), data_source=DataSource(name=admissions))
Set the Feature 'programming' inactive.
fs.set_features_inactive('programming')
True
Get FeatureGroup again after setting feature inactive.
fg = fs.get_feature_group('admissions')
fg
FeatureGroup(admissions, features=[Feature(name=masters),
Feature(name=stats), Feature(name=admitted), Feature(name=gpa)],
entity=Entity(name=admissions), data_source=DataSource(name=admissions))

```
### set_features_active
Use set_features_active() to mark the status of a Feature as active.
```python
Mark Feature 'programming' from 'inactive' to 'active'.
fs.set_features_active('programming')
Get FeatureGroup again after setting feature active.
fg = fs.get_feature_group('admissions')
fg
FeatureGroup(admissions, features=[Feature(name=masters),
Feature(name=programming), Feature(name=admitted),
```
20: Feature Store in teradataml

```python
Feature(name=stats), Feature(name=gpa)],
entity=Entity(name=admissions), data_source=DataSource(name=admissions))

```
### Manage Access for FeatureStore
If you are the owner of Feature Store, you can grant either read access, write access, or read and write
access to other users. Similarly, you can revoke granted access from other users.
Users who are granted read access can access the following APIs on the corresponding Feature Store.
* list_features
* list_entities
* list_data_sources
* list_feature_groups
* get_feature
* get_entity
* get_data_source
* get_feature_group
Users who are granted write access can access the following APIs on the corresponding Feature Store.
* apply
* archive_feature_group
* delete_feature_group
* archive_entity
* delete_entity
* archive_data_source
* delete_data_source
* archive_feature_group
* delete_feature_group
Users with both read and write access can access any API on Feature Store.
### grant.read
grant.read() grants read access to users.
```python
from teradataml import FeatureStore
fs = FeatureStore("vfs_v1")
fs.grant.read('BoB')
True
```
20: Feature Store in teradataml

### grant.write
grant.write() grants write access to users.
```python
from teradataml import FeatureStore
fs = FeatureStore("vfs_v1")
fs.grant.write('BoB')
True
```
### grant.read_write
grant.read_write() grants both read and write access to users.
```python
from teradataml import FeatureStore
fs = FeatureStore("vfs_v1")
fs.grant.read_write('BoB')
True
```
### revoke.read
revoke.read() revokes read access from users.
```python
from teradataml import FeatureStore
fs = FeatureStore("vfs_v1")
fs.revoke.read('BoB')
True
```
### revoke.write
revoke.write() revokes write access from users.
```python
from teradataml import FeatureStore
fs = FeatureStore("vfs_v1")
fs.revoke.write('BoB')
True
```
20: Feature Store in teradataml

### revoke.read_write
revoke.read_write() revokes both read and write access from users.
```python
from teradataml import FeatureStore
fs = FeatureStore("vfs_v1")
fs.revoke.read_write('BoB')
True
```
### delete
delete() removes the FeatureStore and its components from repository.
**Note:**
The delete() function removes all the associated database objects along with data. Do not use this
function if you don't have permission to perform the following actions on the database used by this
**Feature Store:**
    * Drop triggers
    * Drop tables
    * Drop the database
```python
from teradataml import FeatureStore
fs = FeatureStore("vfs_v1")
fs.setup()
True
Delete FeatureStore.
fs.delete()
True

```
## Use FeatureStore in teradataml Analytic Functions
All teradataml analytic functions accept Features as input so you can retrieve these Features from
FeatureStore and use them in the analytic functions.
The following example predicts diabetes for a patient using teradataml analytic function XGBoost.
20: Feature Store in teradataml

Preprocessing: Store the Features and Data Source in Feature Store
1.
* Load data into Vantage.
```python
from teradataml import DataFrame, load_example_data
load_example_data('dataframe', 'medical_readings')
df = DataFrame('medical_readings')
df
record_timestamp  glucose  blood_pressure  insulin
diabetes_pedigree_function  outcome
patient_id
17          2024-04-10 11:10:59.000000      107              74
0                       0.254        1
34          2024-04-10 11:10:59.000000      122              78
0                       0.512        0
13          2024-04-10 11:10:59.000000      189              60
846                       0.398        1
53          2024-04-10 11:10:59.000000      176              90
300                       0.467        1
11          2024-04-10 11:10:59.000000      168              74
0                       0.537        1
51          2024-04-10 11:10:59.000000      101              50
36                       0.526        0
32          2024-04-10 11:10:59.000000       88              58
54                       0.267        0
15          2024-04-10 11:10:59.000000      100               0
0                       0.484        1
99          2024-04-10 11:10:59.000000      122              90
220                       0.325        1
0           2024-04-10 11:10:59.000000      148              72
0                       0.627        1
```
2.
* Group the Features, Entity, and Data Source.
```python
from teradataml import FeatureGroup
fg = FeatureGroup.from_DataFrame(
name='MedicalReadings',
df=medical_readings_df,
entity_columns='patient_id',
timestamp_col_name='record_timestamp'
```
20: Feature Store in teradataml

```python

```
3.
* Store the components in FeatureStore.
```python
fs = FeatureStore('vfs_v1')
fs.apply(fg)
True

```
Retrieve components and dataset
4.
* Retrieve those components from FeatureStore.
```python
fg = fs.get_feature_group('MedicalReadings')
```
5.
* Retrieve the dataset from FeatureStore.
Get historic data (ML models only)
6.
* Use only the readings taken during week 15 for building an ML model.
```python
df = df[df.record_timestamp.week()==15  ]
```
Test and train data
7.
* Prepare test and train data.
```python
sampled_df = df.sample(frac=[0.7, 0.3])
train_df = sampled_df[sampled_df.sampleid==2]
test_df = sampled_df[sampled_df.sampleid==1]
```
8.
* Generate a model using Features in FeatureStore.
```python
fg.set_labels('outcome')
True
from teradataml import XGBoost
model = XGBoost(data=train_df,
input_columns=fg.features,
response_column = fg.labels,
max_depth=3,
lambda1 = 1000.0,
model_type='Classification',
seed=-1,
```
20: Feature Store in teradataml

```python
shrinkage_factor=0.1,
iter_num=2)

```
9.
* Predict the outcome using test data.
```python
model_out = model.predict(newdata=test_df,
id_column='patient_id',
model_type='Classification'
model_out.result
patient_id  Prediction  Confidence_Lower  Confidence_upper
0          17           0               0.5               0.5
1          13           0               0.5               0.5
2          32           0               0.5               0.5
3          40           1               1.0               1.0
4          80           0               1.0               1.0
5          59           0               1.0               1.0
6          38           0               0.5               0.5
7          76           0               1.0               1.0
8          19           0               0.5               0.5
9          34           0               0.5               0.5

```
20: Feature Store in teradataml