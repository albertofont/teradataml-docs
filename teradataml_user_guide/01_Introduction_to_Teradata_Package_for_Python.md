# Introduction to Teradata Package for Python

## Welcome to Teradata Package for Python
#### Using the Teradata Package for Python (teradataml)
The Teradata Package for Python (teradataml) is an open-source Python library package that combines the
benefits of open-source Python language environment with the massive parallel processing capabilities of
Teradata Vantage.
The purpose of this document is to help data scientists:
* Describe available features and functionalities of teradataml.
* Install, uninstall, and upgrade the teradataml package.
* Connect to Vantage.
* Use teradataml for data management, exploration, and execution of analytic functions.
* Understand limitations and considerations of teradataml.
#### Why Would I Use this Content?
The teradataml package is a Python library package like other open-source Python packages. The package
interface makes available to Python users a collection of functions for analytics that reside on Vantage, so
that Python users can perform in-database analytics with no SQL coding required. Also, the teradataml
package provides functions for data manipulation and transformation, data filtering and sub-setting, and can
be used in conjunction with open-source Python capabilities.
#### How Do I Use this Content?
Use this guide as a reference to find descriptions, usage notes, and examples of features and functions
available in the teradataml package.
#### How Do I Get Started?
In your Python environment:
1. Install the teradataml package .
2. Establish connection to Vantage .
Then you can load data, manipulate and transform data for analysis, and execute analytic functions.
#### References to Other Relevant Content
On the Teradata documentation web site https://docs.teradata.com/:
* Teradata Package for Python Function Reference, B
00-4008
* Teradata Vantage™ Machine Learning Engine Analytic Function Reference, B
00-4003
* Database Engine 20 - In-Database Analytic Functions, B035-1206
* Teradata Package for R User Guide, B700-4005
* Teradata Package for R Function Reference, B700-4007
* Teradata Vantage™ Modules for Jupyter Installation Guide, B700-4010
* For VantageCloud Lake only: Build Scalable Python Analytics with Open Analytics Framework
On the Python Package Index (PyPI) site https://pypi.org/user/teradata/:
* Teradata SQL Driver for Python (teradatasql)
* Teradata SQL Driver Dialect for SQLAlchemy (teradatasqlalchemy)
## Teradata Package for Python Product Overview
The Teradata Package for Python product combines the benefits of the open source Python language
environment with the massive parallel processing capabilities of Teradata Vantage, which includes the
Machine Learning Engine analytic functions and the Database Engine 20 in-database analytic functions.
The Teradata Package for Python allows users to develop and run Python programs that take advantage of
the Big Data and Machine Learning analytics capabilities of Vantage.
The Teradata Package for Python is teradataml, a Python library package like other open source Python
packages. The package interface makes available to Python users a collection of functions for analytics that
reside on Vantage, so that Python users can perform analytics with no SQL coding required. Specifically,
the teradataml package provides functions for data manipulation and transformation, data filtering and
sub-setting, and can be used in conjunction with open source Python libraries. The teradataml package
uses SQLAlchemy and provides an interface similar to the Pandas Python library.
The Teradata Package for Python works over connections to:
* Vantage with Database Engine 20 and ML Engine
  Note:
    Machine Learning Engine is a separate legacy engine that is not part of the current standard
    Vantage offer. If your system has the required ML Engine, you can find ML Engine Specific
    Settings and Examples here: ML Engine.
* VantageCloud Lake, VantageCloud Enterprise and VantageCore with Database Engine 20 only
  Note:
    For this type of connection, only Database Engine 20 analytic functions are accessible.
#### Teradata Vantage Modules for Jupyter
Teradata Package for Python is included in the Docker image of Teradata Vantage Modules for Jupyter,
which also includes JupyterLab and other components to run as a Docker container on a client machine.
Teradata Vantage Modules for Jupyter allows users to access Vantage in Python, R or SQL from
JupyterLab notebooks.
See Teradata Vantage™ Modules for Jupyter Installation Guide, B700-4010.
## The teradataml Package
The teradataml package runs on the client system and is designed for data management, exploration, and
execution of analytic functions.
The current version of the teradataml package includes over 100 functions, organized into these
functional areas:
* Utility and database management functions
* Data exploration and preparation functions
* Analytic functions across Vantage
  These functions support high-speed analytics processing required to operationalize analytics and
  automate data partitioning and parallel processing in Vantage.
#### teradataml, SQLAlchemy and Pandas
For Python users familiar with the Pandas Python package, the teradataml package builds on the concept
and syntax of the pandas DataFrame object by creating the teradataml DataFrame object for data residing
on Vantage.
The look and feel of a teradataml DataFrame is similar to a pandas DataFrame in Python. A teradataml
DataFrame is a reference to a database object on the Python client, and it can represent a table, view, or
query in Database Engine 20.
The teradataml library provides an API to access or manipulate a teradataml DataFrame. These functions
generate an SQL request to be executed in the Database Engine 20 or ML Engine through a Python DB-API
connection. The teradataml package uses teradatasqlalchemy, an implementation of SQLAlchemy’s
Dialect interface, to provide enhanced support for rendering SQL. The teradatasqlalchemy dialect
leverages the teradatasql DB-API implementation for connecting to Vantage.
The teradataml DataFrame supports lazy evaluation for a variety of operations on Vantage such as
exploration, transformation, and machine learning. Only a subset of data is ever retrieved from Vantage,
unless the user explicitly requests data to be transferred to the client.
These characteristics result in a pandas-like experience for users who want to perform analytics on Vantage
from their preferred Python environment.
## Key Feature Additions and Changes
The following table lists the key feature additions and changes in the Teradata Package for
Python, teradataml.
Date Release Description
July 2025 20.00.
    00.06
        * New Features/Functionality
          * teradataml: SDK
          Added new client teradataml.sdk.Client to make REST calls
          through SDK.
          New exception added in teradataml, specifically for REST APIs
          TeradatamlRestException that has attribute json_response providing
          proper printable json.
          Exposed different ways of authentication through Client.
          ▪Client credentials authentication through ClientCredentialsAuth class.
          ▪Device code authentication through DeviceCodeAuth class.
          ▪Bearer authentication through BearerAuth class.
          * teradataml: ModelOps SDK
          ▪teradataml exposes Python interfaces for all the REST APIs provided by
            Teradata Vantage ModelOps.
          ▪Added support for blueprint() method which prints available classes in
            modelops module.
          ▪Added new client ModelOpsClient with some additional function
            compared to teradataml.sdk.Client.
          ▪teradataml classes are added for the schema in ModelOps
            OpenAPI specification.
            >>> from teradataml.sdk.modelops import ModelOpsClient,
            Projects
            >>> from teradataml.common.exceptions
            import TeradatamlRestException
            >>> from teradataml.sdk import DeviceCodeAuth,
            BearerAuth, ClientCredentialsAuth # Authentication
            related classes.
            >>> from teradataml.sdk.modelops import models # All
            classes related to OpenAPI schema are present in
            this module.
            # Print available classes in modelops module.
            >>> from teradataml.sdk.modelops import blueprint
            >>> blueprint()
            # Create ClientCredentialsAuth object and create
            ModelOpsClient object.
            >>> cc_obj = ClientCredentialsAuth(auth_client_
            id="<client_id>",
            auth_client_
            secret="<client_secret>",
            auth_token_url="https:
            //<example.com>/token")
            >>> client = ModelOpsClient(base_url="<base_url>",
            auth=cc_obj, ssl_verify=False)
            # Create Projects object.
            >>> p = Projects(client=client)
            # Create project using `body` argument taking object
            of ProjectRequestBody.
            >>> project_paylod = {
            "name": "dummy_project",
            "description": "dummy_project created
            for testing",
            "groupId": "<group_ID>",
Date Release Description
            "gitRepositoryUrl": "/app/built-in/empty",
            "branch": "<branch>"
            }
            >>> p.create_project(body=models.
            ProjectRequestBody(**project_payload))
          * teradataml: Functions
          ▪get_formatters() - Get the formatters for NUMERIC, DATE and
            CHAR types.
          * teradataml: DataFrame Methods
          ▪get_snapshot() - Gets the snapshot data of a teradataml DataFrame
            created on OTF table for a given snapshot id or timestamp.
          ▪from_pandas(): Creates a teradataml DataFrame from a
            pandas DataFrame.
          ▪from_records(): Creates a teradataml DataFrame from a list.
          ▪from_dict(): Creates a teradataml DataFrame from a dictionary.
          * teradataml: DataFrame Property
          ▪history - Returns snapshot history for a DataFrame created on OTF table.
          ▪manifests - Returns manifest information for a DataFrame created on
            OTF table.
          ▪partitions - Returns partition information for a DataFrame created on
            OTF table.
          ▪snapshots - Returns snapshot information for a DataFrame created on
            OTF table.
          * teradataml DataFrameColumn a.k.a. ColumnExpression
|  | ▪ | DataFrameColumn.rlike() | - Function to match a string against a regular |  |  |
| - | - | ----------------------- | ---------------------------------------------- | - | - |
|  |  | expression pattern. |  |  |  |
|  | ▪ | DataFrameColumn.substring_index() |  | - Function to return the substring |  |
|  |  | from a column before a specified delimiter, up to a given occurrence count. |  |  |  |
|  | ▪ | DataFrameColumn.count_delimiters() |  |  | - Function to count the total |
|  |  | number of occurrences of a specified delimiter. |  |  |  |
| * Updates |  |  |  |  |  |

          * teradataml DataFrameColumn a.k.a. ColumnExpression
          ▪DataFrameColumn.like()
            Added argument escape_char to specify the escape character for the
            LIKE pattern.
            Argument pattern now accepts DataFrameColumn as input.
          ▪DataFrameColumn.ilike()
            Argument pattern now accepts DataFrameColumn as input.
            Added argument escape_char to specify the escape character for the
            ILIKE pattern.
          ▪DataFrameColumn.parse_url() - Added argument key to extract a specific
            query parameter when url_part is set to "QUERY".
          ▪teradataml: DataFrame function
            * groupby(), cube(), and rollup()
            Added argument include_grouping_columns to include aggregations
            on the grouping columns.
            * DataFrame(): New argument data, that accepts input data to create a
            teradataml DataFrame, is added.
Date Release Description
          ▪teradataml Options
|  | * Configuration Options |  |  |
| - | ----------------------- | - | - |
|  |  | configure.use_short_object_name | specifies whether to use a |
|  |  | shorter name for temporary database objects which are created by |  |
|  |  | teradataml internally. |  |
| ▪BYOM Function |  |  |  |

            * Supports special characters.
May 2025 20.00.
    00.05
        * New Features/Functionality
          * teradataml AutoML
          ▪New methods added for AutoML(), AutoRegressor(), and
            AutoClassifier():
            get_persisted_tables() - List the persisted tables created during
            AutoML execution.
            visualize() - Generates visualizations to analyze and understand the
            underlying patterns in the data.
          * AutoDataPrep - Automated Data Preparation
          AutoDataPrep simplifies the data preparation process by automating the
          different aspects of data cleaning and transformation, enabling seamless
          exploration, transformation, and optimization of datasets.
          Methods of AutoDataPrep
          ▪__init__() - Instantiate an object of AutoDataPrep with given parameters.
          ▪fit() - Perform fit on specified data and target column.
          ▪get_data() - Retrieve the data after AutoDataPrep.
          ▪load() - Load the saved datasets from the database.
          ▪deploy() - Persist the datasets generated by AutoDataPrep in
            the database.
          ▪delete_data() - Deletes the deployed dataset from the database.
          ▪visualize() - Generates visualizations to analyze and understand the
            underlying patterns in the data.
          * teradataml: In-Database Analytic Functions
          ▪New Analytics Database Analytic Functions:
            Apriori()
            NERExtractor()
            TextMorph()
          * teradataml: Functions
          td_range() - Creates a DataFrame with a specified range of numbers.
          * teradataml DataFrameColumn a.k.a. ColumnExpression
          DataFrameColumn.to_number() - Function converts a string-like
          representation of a number to NUMBER type.
        * Updates
          * teradataml: DataFrame function
          DataFrame.agg(): You can request for different percentiles while running
          agg function.
          New argument debug is added to DataFrame.map_row(), DataFrame.map_
          partition(), DataFrame.apply(), and udf(). During the execution of
          these functions, teradataml internally generates scripts, which are garbage
          collected implicitly. To debug the failures, this argument allows you to control
          the garbage collection of the script. When set to False (default), script
Date Release Description
          generated is garbage collected, otherwise script is not garbage collected and
          displays the path to the script, and you are responsible to remove the script
          if required.
          map_row(), map_partition(), and apply()
          ▪Raises a TeradataMlException, if the Python interpreter major version
            is different between the Vantage Python environment and the local
            user environment.
          ▪Displays a warning, if dill package version is different between the
            Vantage Python environment and the local user environment.
          DataFrame.describe() - Argument include is no longer supported.
          assign() - Optimized SQL query to enhance the performance for
          consecutive assign calls.
          * teradataml: Context Creation
          create_context()
          ▪Enables user to set the authentication token while creating the connection.
            This authentication token is required to access services running on
            Teradata Vantage.
          ▪New argument sql_timeout is added to specify timeout for SQL statement
            execution triggered from the current session.
          * teradataml: UAF Functions
          Integer type value is now accepted as a valid value for function arguments
          accepting float type.
          * General functions
          copy_to_sql() and read_csv() support the VECTOR data type.
          * teradataml DataFrameColumn a.k.a. ColumnExpression
          String Functions
          ▪DataFrameColumn.substr() - Arguments start_pos and length now
            accept DataFrameColumn as input.
          ▪DataFrameColumn.to_char() - Argument formatter now accepts
            DataFrameColumn as input.
          * teradataml: In-Database Analytic Functions
          Updated Analytics Database Analytic Functions:
          ▪SMOTE() is now supported on 17.20.00.00 as well.
          ▪TextParser()
            New arguments added: enforce_token_limit, delimiter_regex, doc_
            id_column, list_positions, token_frequency, output_by_word
March
2025
    20.00.
    00.04
        * New Features/Functionality
          * teradataml: DataFrame
| ▪Introduced a new feature 'Exploratory Data Analysis UI' (EDA UI) to |  |  |  |  |
| -------------------------------------------------------------------- | - | - | - | - |
|  | enhance the user experience of teradataml with Jupyter notebook. EDA |  |  |  |
|  | UI is displayed by default when a teradataml DataFrame is printed in the |  |  |  |
|  | Jupyter notebook. |  |  |  |
| ▪You can control the EDA UI using a new configuration option |  |  |  | display. |
|  | enable_ui | . You can disable by setting | display.enable_ui | to False. |
| ▪EDA UI works based on ipywidgets. In some cases, ipywidgets do not work |  |  |  |  |
|  | on a Jupyter notebook. If this occurs, Teradata recommends choosing a |  |  |  |
|  | kernel again. |  |  |  |
| ▪New Function |  |  |  |  |
Date Release Description
            get_output() is added to get the result of Analytic function when executed
            from EDA UI.
          * OpenSourceML
          ▪td_lightgbm - A teradataml OpenSourceML module
            * deploy() - You can now deploy the models created by lightgbm Booster
            and sklearn modules. Deploying the model stores the model in Vantage
            for future use with td_lightgbm.
            td_lightgbm.deploy() - Deploy the lightgbm Booster or any scikit-
            learn model trained outside Vantage.
            td_lightgbm.train().deploy() - Deploys the lightgbm Booster
            object trained within Vantage.
            td_lightgbm.<sklearn_class>().deploy() - Deploys lightgbm's
            sklearn class object created/trained within Vantage.
            * load() - You can load the deployed models back in the current session
            so you can use the lightgbm functions with the td_lightgbm module.
            td_lightgbm.load() - Load the deployed model in the current session.
          * FeatureStore
          FeatureStore.delete() function is added to drop the Feature Store and
          corresponding repo from Vantage.
          * Database Utility
          db_python_version_diff() - Identifies the Python interpreter major version
          difference between the interpreter installed on Vantage versus interpreter on
          the local user environment.
          db_python_package_version_diff() - Identifies the Python package
          version difference between the packages installed on Vantage versus the
          local user environment.
          * BYOM Function
          ONNXEmbeddings() - Calculate embeddings values in Vantage using an
          embeddings model that has been created outside Vantage and stored in
          ONNX format.
          * teradataml Options
|  | ▪Configuration Options |  |  |  |
| - | ---------------------- | - | - | - |
|  |  | configure.temp_object_type |  | - You can create volatile tables or views |
|  |  | for teradataml internal use. By default, teradataml internally creates the |  |  |
|  |  | views for some of the operations. This new configuration option lets you |  |  |
|  |  | create volatile tables instead of views. This provides greater flexibility for |  |  |
|  |  | users who lack the necessary permissions to create view or need to create |  |  |
|  |  | views on tables without WITH GRANT permissions. |  |  |
|  | ▪Display Options |  |  |  |
|  |  | display.enable_ui | - Specifies whether to display exploratory data |  |
|  |  | analysis UI when DataFrame is printed. By default, this option is enabled |  |  |
|  |  | (True), allowing exploratory data analysis UI to be displayed. When set to |  |  |
|  |  | False, exploratory data analysis UI is hidden. |  |  |
| * Updates |  |  |  |  |

| ▪ | describe() |  |  |  |  |  |  |  |
| - | ---------- | - | - | - | - | - | - | - |
|  | New argument added: |  |  | pivot | . |  |  |  |
|  | When | pivot | is set to False, non-numeric columns are no longer supported |  |  |  |  |  |
|  | for generating statistics. Use |  |  |  | CategoricalSummary | and | ColumnSummary | . |

          * teradataml: DataFrame function
Date Release Description
|  |  | ▪ | fillna() | : Accepts new argument partition_column to partition the data |  |  |  |  |  |  |  |
| - | - | - | -------- | ------------------------------------------------------------- | - | - | - | - | - | - | - |
|  |  |  | and impute null values accordingly. |  |  |  |  |  |  |  |  |
|  |  | ▪Optimized performance for |  |  |  |  |  | DataFrame.plot() |  |  |  |
|  |  |  | DataFrame.plot() |  |  |  | will not regenerate the image when run more than once |  |  |  |  |
|  |  |  | with same arguments. |  |  |  |  |  |  |  |  |
|  | *Hyper Parameter Tuner |  |  |  |  |  |  |  |  |  |  |
|  |  | GridSearch() |  |  | and | RandomSearch() |  |  | now display a message to refer to |  | get_ |
|  |  | error_log() |  | API when model training fails in HPT. |  |  |  |  |  |  |  |
|  | *teradataml Options |  |  |  |  |  |  |  |  |  |  |
|  |  | Configuration Options: |  |  |  |  | configure.indb_install_location |  |  | determines |  |
|  |  | the installation location of the In-DB Python package based on the installed |  |  |  |  |  |  |  |  |  |
|  |  | RPM version. |  |  |  |  |  |  |  |  |  |
|  | *teradataml Context Creation |  |  |  |  |  |  |  |  |  |  |
|  |  | create_context() |  |  |  | : Enables you to create connection using either |  |  |  |  |  |
|  |  | parameters set in environment or config file, in addition to the previous |  |  |  |  |  |  |  |  |  |
|  |  | method. Newly added options help users to hide the sensitive data from |  |  |  |  |  |  |  |  |  |
|  |  | the script. |  |  |  |  |  |  |  |  |  |
|  | *OpenSourceML |  |  |  |  |  |  |  |  |  |  |
|  |  | Raises a TeradataMlException if the Python interpreter major version is |  |  |  |  |  |  |  |  |  |
|  |  | different between the Teradata Package for Python environment and the local |  |  |  |  |  |  |  |  |  |
|  |  | user environment. |  |  |  |  |  |  |  |  |  |
|  |  | Displays a warning, if specific Python package versions are different between |  |  |  |  |  |  |  |  |  |
|  |  | the Teradata Package for Python environment and the local user environment. |  |  |  |  |  |  |  |  |  |
| * Bug Fixes |  |  |  |  |  |  |  |  |  |  |  |

          * td_lightgbm OpenSourceML module: In multi model case, td_lightgbm.
          Dataset().add_features_from() function should add features of one
          partition in first Dataset to features of the same partition in second Dataset.
          This is not the case before and this function fails. This has been fixed.
          * Fixed a minor bug in the Shap() and converted argument training_method
          to required argument.
          * Fixed PCA-related warnings in AutoML.
          * AutoML no longer fails when data with all categorical columns are provided.
          * Fixed AutoML issue with upsampling method.
          * Excluded the identifier column from outlier processing in AutoML.
          * DataFrame.set_index() no longer modifies the original DataFrame's index
          when argument append is used.
          * concat() function now supports the DataFrame with column name starts with
          digit or contains special characters or contains reserved keywords.
          * create_env() proceeds to install other files even if current file
          installation fails.
          * Corrected the error message being raised in create_env() when
          authentication is not set.
          * Added missing argument charset for Vantage Analytics Library functions.
          * New argument seed is added to AutoML, AutoRegressor and AutoClassifier
          to ensure consistency on result.
          * Analytic functions now work even if name of columns for underlying tables is
          non-ascii characters.
Date Release Description
December
2024
    20.00.
    00.03
        * teradataml no longer supports setting the auth_token using set_config_
          params(). Use set_auth_token() to set the token.
        * New Features/Functionality
          * teradataml: DataFrame
          ▪New Function
            alias() - Creates a DataFrame with alias name.
          ▪New Property
            db_object_name - Gets the underlying database object name, on which
            DataFrame is created.
          * teradataml: GeoDataFrame
          ▪New Function
            alias() - Creates a GeoDataFrame with alias name.
          * teradataml: DataFrameColumn a.k.a. ColumnExpression
|  | ▪Arithmetic Functions |  |  |  |
| - | --------------------- | - | - | - |
|  |  | DataFrameColumn.isnan() | - Evaluates expression to determine if the |  |
|  |  | floating-point argument is a NaN (Not-a-Number) value. |  |  |
|  |  | DataFrameColumn.isinf() | - Evaluates expression to determine if the |  |
|  |  | floating-point argument is an infinite number. |  |  |
|  |  | DataFrameColumn.isfinite() |  | - Evaluates expression to determine if it is |
|  |  | a finite floating value. |  |  |
| *FeatureStore - handles feature management within the environment |  |  |  |  |

          ▪FeatureStore Components
            * Feature - Represents a feature which is used in ML Modeling.
            * Entity - Represents the columns which serves as uniqueness for the data
            used in ML Modeling.
            * DataSource - Represents the source of Data.
            * FeatureGroup - Collection of Feature, Entity and DataSource.
            Methods
            * apply() - Adds Feature, Entity, DataSource to a FeatureGroup.
            * from_DataFrame() - Creates a FeatureGroup from
              teradataml DataFrame.
            * from_query() - Creates a FeatureGroup using a SQL query.
            * remove() - Removes Feature, Entity, or DataSource from
              a FeatureGroup.
            * reset_labels() - Removes the labels assigned to the FeatureGroup,
              that are set using set_labels().
            * set_labels() - Sets the Features as labels for a FeatureGroup.
            Properties
            * features - Get the features of a FeatureGroup.
            * labels - Get the labels of FeatureGroup.
          ▪FeatureStore
            Methods
            * apply() - Adds Feature, Entity, DataSource, FeatureGroup
            to FeatureStore.
            * archive_data_source() - Archives a specified DataSource from
            a FeatureStore.
            * archive_entity() - Archives a specified Entity from a FeatureStore.
            * archive_feature() - Archives a specified Feature from a FeatureStore.
Date Release Description
            * archive_feature_group() - Archives a specified FeatureGroup
            from a FeatureStore. Method archives underlying Feature, Entity,
            DataSource also.
            * delete_data_source() - Deletes an archived DataSource.
            * delete_entity() - Deletes an archived Entity.
            * delete_feature() - Deletes an archived Feature.
            * delete_feature_group() - Deletes an archived FeatureGroup.
            * get_data_source() - Get the DataSources associated
            with FeatureStore.
            * get_dataset() - Get the teradataml DataFrame based on Features,
            Entities and DataSource from FeatureGroup.
            * get_entity() - Get the Entity associated with FeatureStore.
            * get_feature() - Get the Feature associated with FeatureStore.
            * get_feature_group() - Get the FeatureGroup associated
            with FeatureStore.
            * list_data_sources() - List DataSources.
            * list_entities() - List Entities.
            * list_feature_groups() - List FeatureGroups.
            * list_features() - List Features.
            * list_repos() - List available repos which are configured
            for FeatureStore.
            * repair() - Repairs the underlying FeatureStore schema on database.
            * set_features_active() - Marks the Features as active.
            * set_features_inactive() - Marks the Features as inactive.
            * setup() - Set up the FeatureStore for a repo.
            Properties
            * repo - Property for FeatureStore repo.
            * grant - Property to Grant access on FeatureStore to user.
            * revoke - Property to Revoke access on FeatureStore from user.
          * teradataml: Table Operator Functions
          ▪Image2Matrix() - Converts an image into a matrix.
          * teradataml: Analytic Functions
          ▪Database Engine 20 Functions
            * CFilter()
            * NaiveBayes()
            * TDNaiveBayesPredict()
            * Shap()
            * SMOTE()
          ▪teradataml: Unbounded Array Framework (UAF) Functions:
            * CopyArt()
          * General Functions
          ▪File Management Functions
            list_files() - List the installed files in Database.
          * OpenSourceML: LightGBM
Date Release Description
| teradataml adds support for lightGBM package through |  |  |  | OpensourceML |
| ---------------------------------------------------- | - | - | - | ------------ |
| ( | OpenML | ) feature. |  |  |
| The following functionality is added: |  |  |  |  |
| ▪ | td_lightgbm |  | - Interface object to run lightgbm functions and classes |  |
|  | through Database Engine 20. |  |  |  |
|  | Example usage: |  |  |  |

            from teradataml import td_lightgbm, DataFrame
            df_train = DataFrame("multi_model_classification")
            feature_columns = ["col1", "col2", "col3", "col4"]
            label_columns = ["label"]
            part_columns = ["partition_column_1", "partition_
            column_2"]
            df_x = df_train.select(feature_columns)
            df_y = df_train.select(label_columns)
            # Dataset creation.
            # Single model case.
            obj_s = td_lightgbm.Dataset(df_x, df_y, silent=True,
            free_raw_data=False)
            # Multi model case.
            obj_m = td_lightgbm.Dataset(df_x, df_y, free_raw_
            data=False, partition_columns=part_columns)
            obj_m_v = td_lightgbm.Dataset(df_x, df_y, free_raw_
            data=False, partition_columns=part_columns)
            ## Model training.
            # Single model case.
            opt = td_lightgbm.train(params={}, train_set = obj_s,
            num_boost_round=30)
            opt.predict(data=df_x, num_iteration=20,
            pred_contrib=True)
            # Multi model case.
            opt = td_lightgbm.train(params={}, train_set = obj_m,
            num_boost_round=30,
            callbacks=[td_lightgbm.record_
            evaluation(rec)],
            valid_sets=[obj_m_v, obj_m_v])
            # Passing `label` argument to get it returned in
            output DataFrame.
            opt.predict(data=df_x, label=df_y, num_iteration=20)
          ▪Added support for accessing scikit-learn APIs using exposed interface
            object td_lightgbm.
          * teradataml: Functions
          ▪register() - Registers a user defined function (UDF).
Date Release Description
          ▪call_udf() - Calls a registered user defined function (UDF) and
            returns ColumnExpression.
          ▪list_udfs() - List all the UDFs registered using 'register()' function.
          ▪deregister() - Deregisters a user defined function (UDF).
          * teradataml: Options
          ▪Configuration Options
            table_operator - Specifies the name of table operator.
        * Updates
          * General functions
          set_auth_token() - Added base_url parameter which accepts the CCP url.
          'ues_url' will be deprecated in future, and you will need to specify 'base_
          url' instead.
          * teradataml: DataFrame function
|  | ▪ | join() |  |  |  |  |  |
| - | - | ------ | - | - | - | - | - |
|  |  | Now supports compound ColumnExpression having more than one binary |  |  |  |  |  |
|  |  | operator in |  | on | argument. |  |  |
|  |  | Now supports ColumnExpression containing FunctionExpression(s) in |  |  |  |  |  |
|  |  | on | argument. |  |  |  |  |
|  |  | self-join now expects aliased DataFrame in |  |  |  | other | argument. |
| *teradataml: GeoDataFrame function |  |  |  |  |  |  |  |
|  | ▪ | join() |  |  |  |  |  |
|  |  | Now supports compound ColumnExpression having more than one binary |  |  |  |  |  |
|  |  | operator in on argument. |  |  |  |  |  |
|  |  | Now supports ColumnExpression containing FunctionExpressions in |  |  |  |  |  |
|  |  | on | argument. |  |  |  |  |
|  |  | self-join now expects aliased DataFrame in |  |  |  | other | argument. |
| *teradataml: Unbounded Array Framework (UAF) Functions |  |  |  |  |  |  |  |

|  | ▪ | TextParser() |  |  |  |  |
| - | - | ------------ | - | - | - | - |
|  |  | Argument name |  | covert_to_lowercase | changed to | convert_ |
|  |  | to_lowercase | . |  |  |  |
| * Bug Fixes: |  |  |  |  |  |  |

          ▪SAX() - Default value added for window_size and output_frequency.
          ▪DickeyFuller()
            Supports TDAnalyticResult as input.
            Default value added for max_lags.
            Removed parameter drift_trend_formula.
            Updated permitted values for algorithm.
          * teradataml: AutoML
          ▪AutoML, AutoRegressor, and AutoClassifier
            Now supports DECIMAL datatype as input.
          * teradataml: Database Engine 20 Analytic Functions
          * db_list_tables() now returns correct results when '%' is used.
October
2024
    20.00.
    00.02
        * New Features/Functionality
          * teradataml: Database Engine 20 Analytic Functions
          ▪New Database Engine 20 Analytic Functions:
            TFIDF()
            Unpivoting()
Date Release Description
            Pivoting()
          ▪New Unbounded Array Framework (UAF) Functions:
            AutoArima()
            DWT()
            DWT2D()
            FilterFactory1d()
            IDWT()
            IDWT2D()
            IQR()
            Matrix2Image()
            SAX()
            WindowDFFT()
          * teradataml: Functions
          ▪udf() - Creates a user defined function (UDF) and
            returns ColumnExpression.
          ▪materialize() - Persists dataframe into database for current session.
          ▪create_temp_view() - Creates a temporary view for session on
            the DataFrame.
          * teradataml: DataFrame
|  | ▪New function |  | set_session_param() |  |  |  | is added to set the database |  |
| - | ------------- | - | ------------------- | - | - | - | ---------------------------- | - |
|  |  | session parameters. |  |  |  |  |  |  |
|  | ▪New function |  | unset_session_param() |  |  |  |  | is added to unset database |
|  |  | session parameters. |  |  |  |  |  |  |
| *teradataml DataFrameColumn a.k.a. ColumnExpression |  |  |  |  |  |  |  |  |
|  | ▪_Date Time Functions_ |  |  |  |  |  |  |  |
|  |  | DataFrameColumn.to_timestamp() |  |  |  |  | - Converts string or integer value to a |  |
|  |  | TIMESTAMP data type or TIMESTAMP WITH TIME ZONE data type. |  |  |  |  |  |  |
|  |  | DataFrameColumn.extract() |  | - Extracts date component to a |  |  |  |  |
|  |  | numeric value. |  |  |  |  |  |  |
|  |  | DataFrameColumn.to_interval() |  |  |  | - Converts a numeric value or |  |  |
|  |  | string value into INTERVAL_DAY_TO_SECOND or INTERVAL_YEAR_ |  |  |  |  |  |  |
|  |  | TO_MONTH value. |  |  |  |  |  |  |
|  | ▪_String Functions_ |  |  |  |  |  |  |  |
|  |  | DataFrameColumn.parse_url() |  |  | - Extracts a part from a URL. |  |  |  |
|  | ▪_Arithmetic Functions _ |  |  |  |  |  |  |  |
|  |  | DataFrameColumn.log - Returns the logarithm value of the column with |  |  |  |  |  |  |
|  |  | respect to 'base'. |  |  |  |  |  |  |
| *teradataml: AutoML |  |  |  |  |  |  |  |  |

          ▪New Methods added for AutoML(), AutoRegressor(), and
            AutoClassifier():
            evaluate() - Added new method in AutoML to perform evaluation
            on the data using the best model or the model of users choice from
            the leaderboard.
            New function added: load(), deploy() and remove_saved_model().
            load() - Loads the saved model from database.
            deploy() - Saves the trained model inside database.
            remove_saved_model() - Removes the saved model in database.
            model_hyperparameters() - Returns the hyperparameter of fitted or
            loaded models.
        * Updates
          * teradataml: AutoML
Date Release Description
          ▪AutoML(), AutoRegressor()
            New performance metrics added for task type regression i.e., "MAPE",
            "MPE", "ME", "EV", "MPD" and "MGD".
          ▪AutoML(), AutoRegressor() and AutoClassifier()
            New arguments added: volatile, persist.
            predict() - Data input is now mandatory for generating predictions. Default
            model evaluation is now removed.
          * teradataml: Options
          ▪set_config_params()
            Following arguments will be deprecated in the future: ues_url and
            auth_token
          * DataFrameColumn.cast(): Accepts 2 new arguments format and timezone.
          * DataFrame.assign(): Accepts ColumnExpressions returned by udf().
        * teradataml DataFrame
          * to_pandas()- Function returns the pandas dataframe with Decimal column
          types as float instead of object. If user want to datatype to be object set
          argument coerce_float to False.
        * Database Utility
          * list_td_reserved_keywords() - Accepts a list of strings as argument.
        * Updates to existing UAF Functions:
          * ACF() - round_results parameter removed as it was used for internal testing.
          * BreuschGodfrey() - Added default_value 0.05 for parameter
          significance_level.
          * GoldfeldQuandt()
          Removed parameters weights and formula.
          Replaced parameter orig_regr_paramcnt with const_term.
          Changed description for parameter algorithm. Please refer document for
          more details.
          Note: This will break backward compatibility.
          * HoltWintersForecaster() - Default value of parameter seasonal_
          periods removed.
          * IDFFT2() - Removed parameter output_fmt_row_major as it is used for
          internal testing.
          * Resample() - Added parameter output_fmt_index_style.
        * Bug Fixes
          * KNN predict() function can now predict on test data which doesn't contain
          target column.
          * Metrics functions are supported on the Lake system.
          * The following OpenSourceML functions - sklearn modules are fixed.
          ▪sklearn.ensemble:
            ExtraTreesClassifier - apply()
            ExtraTreesRegressor - apply()
            RandomForestClassifier - apply()
            RandomForestRegressor - apply()
          ▪sklearn.impute:
            SimpleImputer - transform(), fit_transform(), inverse_transform()
            MissingIndicator - transform(), fit_transform()
          ▪sklearn.kernel_approximations:
            Nystroem - transform(), fit_transform()
            PolynomialCountSketch - transform(), fit_transform()
Date Release Description
            RBFSampler - transform(), fit_transform()
          ▪sklearn.neighbours:
            KNeighborsTransformer - transform(), fit_transform()
            RadiusNeighborsTransformer - transform(), fit_transform()
          ▪sklearn.preprocessing:
            KernelCenterer - transform()
            OneHotEncoder - transform(), inverse_transform()
          * OpenSourceML returns teradataml objects for model attributes and functions
          instead of sklearn objects so that the user can perform further operations like
          score(), predict() etc on top of the returned objects.
          * AutoML predict() function now generates correct ROC-AUC value for
          positive class.
          * deploy() method of Script and Apply classes retries model deployment if there
          is any intermittent network issues.
August
2024
    20.00.
    00.01
        * teradataml no longer supports Python versions less than 3.8.
        * Added new feature - Personal Access Token (PAT) support in teradataml
          set_auth_token() - teradataml now supports authentication via PAT in
          addition to OAuth 2.0 Device Authorization Grant (formerly known as the Device
          Flow).
          It accepts UES URL, Personal AccessToken (PAT), and Private Key file
          generated from VantageCloud Lake Console and optional argument username
          and expiration_time in seconds.
        * Updated Database Engine 20 analytic functions:
          * ANOVA() - New arguments added: group_name_column, group_value_
          name, group_names, num_groups for data containing group values and
          group names.
          * FTest() - New arguments added: sample_name_column, sample_name_
          value, first_sample_name, second_sample_name.
          * GLM()
          ▪Supports stepwise regression and accept new arguments stepwise_
            direction, max_steps_num, and initial_stepwise_columns.
          ▪New arguments added: attribute_data, parameter_data, iteration_
            mode, and partition_column.
          * GetFutileColumns() - Arguments category_summary_column and
          threshold_value are now optional.
          * KMeans() - New argument added: initialcentroids_method.
          * NonLinearCombineFit() - Argument result_column is now optional.
          * ROC() - Argument positive_class is now optional.
          * SVMPredict() - New argument added: model_type.
          * ScaleFit()
          ▪New arguments added: ignoreinvalid_locationscale, unused_
            attributes, attribute_name_column, attribute_value_column.
          ▪Arguments attribute_name_column, attribute_value_column, and
            target_attributes are supported for sparse input.
          ▪Arguments attribute_data, parameter_data, and partition_column
            are supported for partitioning.
Date Release Description
          * ScaleTransform() - New arguments added: attribute_name_column and
          attribute_value_column support for sparse input.
          * TDGLMPredict() - New arguments added: family and partition_column.
          * XGBoost() - New argument base_score is added for initial prediction value
          for all data points.
          * XGBoostPredict() - New argument detailed is added for detailed
          information of each prediction.
          * ZTest() - New arguments added: sample_name_column, sample_value_
          column, first_sample_name, and second_sample_name.
        * teradataml: AutoML
          * AutoML(), AutoRegressor(), and AutoClassifier() - New argument
          max_models is added as an early stopping criterion to limit the maximum
          number of models to be trained.
        * teradataml: DataFrame functions
          * DataFrame.agg() - Accepts ColumnExpressions and list of
          ColumnExpressions as arguments.
        * teradataml: General Functions
| *Data Transfer Utility - | fastload() | updates |
| ------------------------ | ---------- | ------- |

          ▪Improved error and warning table handling with following new arguments.
            * err_staging_db
            * err_tbl_name
            * warn_tbl_name
            * err_tbl_1_suffix
            * err_tbl_2_suffix
          * Change in behavior of save_errors argument. When save_errors is set to
          True, error information will be available in two persistent tables ERR_1 and
          ERR_2. When save_errors is set to False, error information will be available
          in single pandas dataframe.
          * Garbage collector location is now configurable. You can set configure.local_
          storage to a desired location.
        * Updates:
          * UAF functions now work if the database name has special characters.
          * OpenSourceML can now read and process NULL/nan values.
          * Boolean values output will now be returned as VARBYTE column with 0 or 1
          values in OpenSourceML.
          * Fixed bug for Apply's deploy().
          * Issue with volatile table creation is fixed where it is created in the correct
          database, i.e., user's spool space, regardless of the temp database specified.
          * ColumnTransformer function now processes its arguments in the order they
          are passed.
March
2024
    20.00.
    00.00
        * Added new feature - teradataml open-source machine learning functions
          (teradataml OpenSourceML) that dynamically exposes open-source packages
          through Teradata Vantage. It provides an interface object through which
          exposed classes and functions of open-source packages can be accessed
          with the same syntax and arguments.
        * Added new feature - AutoML that automates the process of building, training,
          and validating machine learning models. It involves automation of various
Date Release Description
|  | aspects of the machine learning workflow, such as feature exploration, feature |  |  |  |  |  |  |
| - | ------------------------------------------------------------------------------ | - | - | - | - | - | - |
|  | engineering, data preparation, model training and evaluation for given dataset. |  |  |  |  |  |  |
| * Added new deploy() method to deploy models generated after running script, in |  |  |  |  |  |  |  |
|  | database when connected to VantageCloud Enterprise (as part of Script table |  |  |  |  |  |  |
|  | operator), or in user environment when connected to VantageCloud Lake (as |  |  |  |  |  |  |
|  | part of Apply table operator). |  |  |  |  |  |  |
| * Added new DataFrame manipulation functions |  | cube() | , | rollup() | , | replace | . |
| * Added eight categories of new DataFrame Column functions: |  |  |  |  |  |  |  |

          * Bit Byte Manipulation Functions
          * Comparison Functions
          * Date Time Functions
          * Hyperbolic Functions
          * Regular Arithmetic Functions
          * Regular Expression Functions
          * String Functions
          * Trigonometric Functions
        * Removed functionalities that have been deprecated:
          * Machine Learning Engine functions
          * Model Cataloging feature
          * Sandbox feature that supports testing script in both Script table operator and
          Apply table operator.
February
20
    17.20.
    00.07
        Updated Open Analytics Framework APIs to support VantageCloud Lake use of
        Anaconda for building conda environments to run Python analytic workload on
        Open Analytics Framework:
        * Updated create_env() with new argument conda_env to specify whether the
          environment to be created is a conda environment or not.
        * Output of list environment APIs have a new column "conda" to show whether the
          environment is a conda environment or not.
        * Updated set_auth_token to address Open Analytics Login Issue with
          teradataml 17.20.00.05 and 17.20.00.06.
        * Updated list_user_envs() with new argument conda_env to specify whether
          to filter the conda environments when listing user environments.
January
20
    17.20.
    00.06
        * New teradataml DataFrame Column functions:
          * 19 new Bit Byte Manipulation Functions
          * 4 new Regular Expression Functions
          * 2 new Display Functions
        * New and updated Open Analytics Framework APIs:
          * Updated create_env() so user can create one or more user environments
          using the new argument template by providing specifications in template
          json file.
          * New UserEnv Class property models, and methods install_model() and
          uninstall_model() to list, install and uninstall models in user environment.
          * New UserEnv Class method snapshot() to take snapshot of
          user environment.
        * New BYOM function DataRobotPredict() to score the data in Vantage using
          the model trained externally in datarobot and stored in Vantage.
Date Release Description
        * Updated DataFrame functions:
          * DataFrame.describe() method to accept argument statistics to specify the
          aggregate operation to perform.
          * DataFrame.sort() method to accept ColumnExpression, and
          enable sorting.
          * DataFrame.sample() method to support column stratification.
        * Updated general function view_log() to download the APPLY query logs.
        * Updated Database Engine 20 analytic functions so arguments which accept
          floating numbers will accept integers.
        * Updated DataFrame.plot() function to ignore the null values while
          plotting data.
October
2023
    17.20.
    00.05
        * New hyperparameter tuning feature to determine the optimal set of
          hyperparameters for the given dataset and learning model.
          * GridSearch algorithm covers all possible parameter values to identify
          optimal hyperparameters.
          * RandomSearch algorithm performs random sampling on hyperparameter
          space to identify optimal hyperparameters.
        * New plotting feature to visualize analytic results.
        * New teradataml DataFrame functions:
|  | * | DataFrame.plot() |  | to generate plots on teradataml DataFrame. |  |  |  |  |  |  |  |  |  |
| - | - | ---------------- | - | ------------------------------------------ | - | - | - | - | - | - | - | - | - |
|  | * | DataFrame.itertuples() |  |  | to iterate over teradataml DataFrame rows as |  |  |  |  |  |  |  |  |
|  |  | namedtuples or list. |  |  |  |  |  |  |  |  |  |  |  |
| * New teradataml GeoDataFrame function |  |  |  |  |  |  |  | GeoDataFrame.plot() |  |  | to generate |  |  |
|  | plots on teradataml GeoDataFrame. |  |  |  |  |  |  |  |  |  |  |  |  |
| * New BYOM function |  |  | DataikuPredict() |  |  |  | to score the data in Vantage using the |  |  |  |  |  |  |
|  | model trained externally in Dataiku UI and stored in Vantage. |  |  |  |  |  |  |  |  |  |  |  |  |
| * New teradataml DataFrame Column functions: |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  | *Regular Arithmetic Functions |  |  |  |  |  |  |  |  |  |  |  |  |
|  | *Trigonometric Functions |  |  |  |  |  |  |  |  |  |  |  |  |
|  | *Hyperbolic Functions |  |  |  |  |  |  |  |  |  |  |  |  |
|  | *String Functions |  |  |  |  |  |  |  |  |  |  |  |  |
| * New general function |  |  |  | async_run_status() |  |  |  |  | to check the status of |  |  |  |  |
|  | asynchronous runs using unique run ids. |  |  |  |  |  |  |  |  |  |  |  |  |
| * New teradataml configuration option |  |  |  |  |  | configure.indb_install_location |  |  |  |  |  |  | to |
|  | specify the installation location of in-database Python package. |  |  |  |  |  |  |  |  |  |  |  |  |
| * Updated Open Analytics Framework APIs: |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  | * | set_auth_token() |  | does not accept username and password anymore. |  |  |  |  |  |  |  |  |  |
|  |  | Instead, function opens up a browser session and user should authenticate |  |  |  |  |  |  |  |  |  |  |  |
|  |  | in browser. |  |  |  |  |  |  |  |  |  |  |  |
|  | *User environments, files and libraries related APIs updated to support |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  | R environment. |  |  |  |  |  |  |  |  |  |  |  |
| * Updated Unbounded Array Framework (UAF) function |  |  |  |  |  |  |  |  |  | ArimaEstimate() |  | to |  |
|  | support for CSS algorithm via algorithm argument. |  |  |  |  |  |  |  |  |  |  |  |  |
## Teradata Package for Python Function Reference
The Teradata Package for Python, teradataml, includes functions for data management, exploration,
preparation and analytics in Vantage.
See Teradata Package for Python Function Reference at https://docs.teradata.com/ for detailed description
and usage examples of the teradataml functions.