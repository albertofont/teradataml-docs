# Appendix E: Additional Information

## Changes and Additions
The table lists changes and additions in this release of the Teradata Package for Python User
Guide, B700-4006.
Date Release Description
June 2025 20.00.00.
      05
          * AutoML updates
|  | *New AutoML methods: | get_persisted_tables | and | visualize |
| - | -------------------- | -------------------- | --- | --------- |
|  | *New AutoDataPrep component was added |  |  |  |
|  | *AutoML examples were updated |  |  |  |
| * DataFrame updates: |  |  |  |  |

|  | *New functions for lightgbm package support: |  |  |  | deploy() | and | load() |  |
| - | -------------------------------------------- | - | - | - | -------- | --- | ------ | - |
|  | *New warning and exception were added if the Python interpreter major |  |  |  |  |  |  |  |
|  |  | version is different between the Teradata Package for Python environment |  |  |  |  |  |  |
|  |  | and the local user environment. |  |  |  |  |  |  |
| * New FeatureStore function: |  |  | FeatureStore.delete() |  |  |  |  |  |
| * New Database Utilities functions: |  |  |  | db_python_version_diff() |  |  | and | db_ |
|  | python_package_version_diff() |  |  |  |  |  |  |  |
| * teradataml Options updates: |  |  |  |  |  |  |  |  |

          * debug argument added to DataFrame.map_row(), DataFrame.map_
            partition(), DataFrame.apply(), and udf()
          * The following arguments were added to set_auth_token():
            kid
            auth_mech
            password
          * General Functions updates:
          * New function added: td_range()
          * read_csv() and copy_to_sql() support loading vector data to
            the database)
          * DataFrameColumn update:
          * to_number() added to String Functions
March
2025
      20.00.00.
      04
          * DataFrame updates
          * Exploratory Data Analysis UI feature has been added to enhance the user
            experience of teradataml with Jupyter notebook.
          * New DataFrame functions: get_output() and fillna
          * New pivot argument added to describe()
          * DataFrame.plot() will not regenerate the image when run more than once
            with same arguments.
          * Hyper Parameter Tuner (HPT) update: GridSearch() and RandomSearch()
          now display a message to refer to get_error_log() API when model training
          fails in HPT.
          * OpenSourceML updates
Date Release Description
          * New Configuration option: configure.indb_install_location
          * New Display option: display.enable_ui
          * create_context() update enables you to create connection using either
            parameters set in environment or config file, in addition to the previous
            method. Newly added options help users to hide the sensitive data from
            the script.
December
2024
      20.00.00.
      03
          * teradataml no longer supports setting auth_token using set_config_
          params(). Use set_auth_token() to set the token.
          * set_auth_token() - Added base_url parameter which accepts the CCP url.
          * FeatureStore support added to handle feature management within
          the environment
          * Added support for lightGBM package through OpensourceML
          (OpenML) feature.
          * New functions:
          register()
          call_udf()
          list_udfs()
          deregister()
          * DataFrame updates:
          * New alias() function
          * New db_object_name property
          * Updated join() to support compound ColumnExpression having
            more than one binary operator and/or ColumnExpression containing
            FunctionExpression(s) in on argument. self-join now expects aliased
            DataFrame in other argument.
          * GeoDataFrame updates:
          * New alias() function
          * Updated join() to support compound ColumnExpression having
            more than one binary operator and/or ColumnExpression containing
            FunctionExpression(s) in on argument. self-join now expects aliased
            DataFrame in other argument.
          * New DataFrameColumn a.k.a. ColumnExpression functions:
          * Arithmetic Functions - DataFrameColumn.isnan(), DataFrameColumn.
            isinf(), DataFrameColumn.isfinite()
          * New Table Operator function - Image2Matrix()
          * New File Management function - list_files()
          * Configuration Option update: table_operator specifies the name of
          table operator.
          * AutoML update - AutoML, AutoRegressor, and AutoClassifier now support
          DECIMAL datatype as input.
October
2024
      20.00.00.
      02
          * New functions
          udf()
          materialize()
          create_temp_view()
          * DataFrame updates:
          * New functions:
            set_session_param()
Date Release Description
            unset_session_param()
          * Updates:
            DataFrameColumn.cast(): Accepts 2 new arguments format
            and timezone.
            DataFrame.assign(): Accepts ColumnExpressions returned by udf().
            to_pandas() - Returns the pandas dataframe with Decimal column types as
            float instead of object.
          * New teradataml DataFrameColumn a.k.a. ColumnExpression functions:
          * _Date Time Functions_
            DataFrameColumn.to_timestamp()
            DataFrameColumn.extract()
            DataFrameColumn.to_interval()
          * _String Functions_
            DataFrameColumn.parse_url()
          * _Arithmetic Functions _
            DataFrameColumn.log
          * AutoML updates:
          * evaluate() method added to AutoML(), AutoRegressor(), and
            AutoClassifier():
            New functions added: load(), deploy(), remove_saved_model(), and
            model_hyperparameters()
          * AutoML(), AutoRegressor(): New performance metrics added for task
            type regression i.e., "MAPE", "MPE", "ME", "EV", "MPD", and "MGD".
          * AutoML(), AutoRegressor(), and AutoClassifier():
            New arguments added: volatile, persist.
            predict() - Data input is now mandatory for generating predictions. Default
            model evaluation has been removed.
          * set_config_params(): ues_url and auth_token arguments will be
          deprecated in a future release
          * list_td_reserved_keywords() accepts a list of strings as argument.
August
2024
      20.00.00.
      01
          * Streamlined the installation process to include wheel file installation option
          * Added max_models argument to AutoML
          * Added new AutoML example
March
2024
      20.00.00.
      00
          * Add new section of AutoML in teradataml
          * Added new section of teradataml Open-Source Machine Learning Functions
          * Added new deploy() method to both Script table operator and Apply
          table operator
          * Updated DataFrame Manipulation functions:
          * Added new functions cube, rollup, replace
          * Updated existing functions join
          * Updated fit methods for both GridSearch and RandomSearch, and added
          new examples for these two algorithms in Hyperparameter Tunning section
          * Added new DataFrame Column functions categories:
          * Regular Arithmetic Functions
          * String Functions
          * Bit-Byte Manipulation Functions
Date Release Description
          * Regular Expression Functions
          * Date Time Functions
          * Comparison Functions
          * Trigonometric Functions
          * Hyperbolic Functions
          * Updated User Permission in Vantage
          * Updated dependencies requirements
          * Removed Model Cataloging section
## Teradata Links
Link Description
https://docs.teradata.com/ Search Teradata Documentation, customize content to your needs, and
                download PDFs.
                Customers: Log in to access Orange Books.
https://support.teradata.com Helpful resources in one place:
                * Support requests
                * Account management and software downloads
                * Knowledge base, community, and support policies
                * Product documentation
                * Learning resources, including Teradata University
https://www.teradata.com/University/Overview Teradata education network
https://support.teradata.com/community Link to Teradata community