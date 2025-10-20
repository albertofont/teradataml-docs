# teradataml Hyperparameter Tuning

GridSearch
Methods
__init__
teradataml.hyperparameter_tuner.optimizer.GridSearch.__init__ = __init__(self, func, params)
        DESCRIPTION:
            GridSearch is an exhaustive search algorithm that covers all possible
            parameter values to identify optimal hyperparameters. It works for 
            teradataml analytic functions from SQLE, BYOM, VAL and UAF features.
            teradataml GridSearch allows user to perform hyperparameter tuning for 
            all model trainer and non-model trainer functions.
            When used for model trainer functions:
                * Based on evaluation metrics search determines best model.
                * All methods and properties can be used.
            When used for non-model trainer functions:
                * Only fit() method is supported.
                * User can choose the best output as they see fit to use this.
            teradataml GridSearch also allows user to use input data as the 
            hyperparameter. This option can be suitable when the user wants to
            identify the best models for a set of input data. When user passes
            set of data as hyperparameter for model trainer function, the search
            determines the best data along with the best model based on the 
            evaluation metrics.
            Note:
                * configure.temp_object_type="VT" follows sequential execution.
        PARAMETERS:
            func:
                Required Argument.
                Specifies a teradataml analytic function from SQLE, VAL, and UAF.
                Types:
                    teradataml Analytic Functions
                        * Advanced analytic functions
                        * UAF
                        * VAL
                    Refer to display_analytic_functions() function for list of functions.
            params:
                Required Argument.
                Specifies the parameter(s) of a teradataml analytic function. 
9/14/2025, 16:50
Page 1,387 of 1,675
                The parameter(s) must be in dictionary. keys refers to the 
                argument names and values refers to argument values for corresponding
                arguments. 
                Notes:
                    * One can specify the argument value in a tuple to run HPT 
                      with different arguments.
                    * Model trainer function arguments "id_column", "input_columns",
                      and "target_columns" must be passed in fit() method.
                    * All required arguments of non-model trainer function must 
                      be passed while GridSearch object creation.
                Types: dict
        RETURNS:
            None
        RAISES:
            TeradataMlException, TypeError, ValueError
        EXAMPLES:
            >>> # Example 1: Model trainer function. Performing hyperparameter-tuning 
            >>> #            on SVM model trainer function.
            >>> # Load the example data.
            >>> load_example_data("teradataml", ["cal_housing_ex_raw"])
            >>> # Create teradataml DataFrame objects.
            >>> data_input = DataFrame.from_table("cal_housing_ex_raw")
            >>> # Scale "target_columns" with respect to 'STD' value of the column.
            >>> fit_obj = ScaleFit(data=data_input,
                                   target_columns=['MedInc', 'HouseAge', 'AveRooms',
                                                   'AveBedrms', 'Population', 'AveOccup',
                                                'Latitude', 'Longitude'],
                                  scale_method="STD")
            >>> # Transform the data.
            >>> transform_obj = ScaleTransform(data=data_input,
                                               object=fit_obj.output,
                                               accumulate=["id", "MedHouseVal"])
            >>> # Define parameter space for model training.
            >>> params = {"input_columns":['MedInc', 'HouseAge', 'AveRooms',
                                          'AveBedrms', 'Population', 'AveOccup',
                                          'Latitude', 'Longitude'],
                         "response_column":"MedHouseVal",
                         "model_type":"regression",
                         "batch_size":(11, 50, 75),
                         "iter_max":(100, 301),
                         "lambda1":0.1,
                         "alpha":0.5,
                         "iter_num_no_change":60,
                         "tolerance":0.01,
                         "intercept":False,
                         "learning_rate":"INVTIME",
                         "initial_data":0.5,
                         "decay_rate":0.5,
                         "momentum":0.6,
                         "nesterov":True,
                         "local_sgd_iterations":1}
            >>> # Required argument for model prediction and evaluation.
            >>> eval_params = {"id_column": "id",
                               "accumulate": "MedHouseVal"}
            >>> # Import trainer function and optimizer.
            >>> from teradataml import SVM, GridSearch
            >>> # Initialize the GridSearch optimizer with model trainer 
            >>> # function and parameter space required for model training.
            >>> gs_obj = GridSearch(func=SVM, params=params)
            >>> # Perform model optimization for SVM function.
            >>> # Evaluation and prediction arguments are passed along with 
            >>> # training dataframe.
            >>> gs_obj.fit(data=transform_obj.result, **eval_params)
            >>> # View trained models.
            >>> gs_obj.models
                  MODEL_ID DATA_ID                                         PARAMETERS STATUS       MAE
                0    SVM_3    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
                1    SVM_0    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.660815
                2    SVM_1    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.660815
                3    SVM_2    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
                4    SVM_4    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
9/14/2025, 16:50
Page 1,388 of 1,675
                5    SVM_5    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
            >>> # View model evaluation stats.
            >>> gs_obj.model_stats
                  MODEL_ID DATA_ID                                         PARAMETERS STATUS       MAE
                0    SVM_3    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772`
                1    SVM_0    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.660815
                2    SVM_1    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.660815
                3    SVM_2    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
                4    SVM_4    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
                5    SVM_5    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772`
            >>> # View best data, model ID and score.
            >>> print("Best data ID: ", gs_obj.best_data_id)
                Best data ID:  DF_0
            >>> print("Best model ID: ", gs_obj.best_model_id)
                Best model ID:  SVM_3
            >>> print("Best model score: ",gs_obj.best_score_)
                Best model score:  2.616772068334627
            >>> # Performing prediction on sampled data using best trained model.
            >>> test_data = transform_obj.result.iloc[:5]
            >>> gs_pred = gs_obj.predict(newdata=test_data, **eval_params)
            >>> print("Prediction result: 
", gs_pred.result)
                Prediction result:
                     id  prediction  MedHouseVal
                0   686    0.202843        1.578
                1  2018    0.149868        0.578
                2  1754    0.211870        1.651
                3   670    0.192414        1.922
                4   244    0.247545        1.117
            >>> # Perform evaluation using best model.
            >>> gs_obj.evaluate()
                ############ result Output ############
                        MAE       MSE  MSLE        MAPE         MPE      RMSE  RMSLE        ME       R2       EV  MPD  MGD
                0  2.616772  8.814968   0.0  101.876866  101.876866  2.969001    0.0  5.342344 -4.14622 -0.14862  NaN  NaN
            >>> # Retrieve any trained model.
            >>> gs_obj.get_model("SVM_1")
                ############ output_data Output ############
                   iterNum      loss       eta  bias
                0        3  2.060386  0.028868   0.0
                1        5  2.055509  0.022361   0.0
                2        6  2.051982  0.020412   0.0
                3        7  2.048387  0.018898   0.0
                4        9  2.041521  0.016667   0.0
                5       10  2.038314  0.015811   0.0
                6        8  2.044882  0.017678   0.0
                7        4  2.058757  0.025000   0.0
                8        2  2.065932  0.035355   0.0
                9        1  1.780877  0.050000   0.0
                ############ result Output ############
                                         predictor    estimate       value
                attribute
                 7                        Latitude    0.155095        None
                -9         Learning Rate (Initial)    0.050000        None
                -17                   OneClass SVM         NaN       FALSE
                -14                        Epsilon    0.100000        None
                 5                      Population    0.000000        None
                -12                       Nesterov         NaN        TRUE
                -5                             BIC   73.297397        None
                -7                           Alpha    0.500000  Elasticnet
                -3          Number of Observations   55.000000        None
                 0                     (Intercept)    0.000000        None
            >>> # Update the default model.
            >>> gs_obj.set_model("SVM_1")
            >>> # Example 2: Model trainer function. Performing hyperparameter-tuning 
            >>> #            on SVM model trainer function using unlabeled multiple-dataframe.
            >>> # Slicing transformed dataframe into two part to present 
            >>> # multiple-dataframe support.
            >>> train_df_1 = transform_obj.result.iloc[:30]
            >>> train_df_2 = transform_obj.result.iloc[30:]
9/14/2025, 16:50
Page 1,389 of 1,675
            >>> # Initialize the GridSearch optimizer with model trainer 
            >>> # function and parameter space required for model training.
            >>> gs_obj = GridSearch(func=SVM, params=params)
            >>> # Perform model optimization for SVM function for 
            >>> # unlabeled multiple-dataframe support.
            >>> # Evaluation and prediction arguments are passed along with 
            >>> # training dataframe.
            >>> gs_obj.fit(data=(train_df_1, train_df_2), **eval_params)
            >>> # View trained models.
            >>> gs_obj.models
                MODEL_ID DATA_ID                                         PARAMETERS STATUS       MAE
                0     SVM_3    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.650505
                1     SVM_1    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.650505
                2     SVM_2    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.326521
                3     SVM_0    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.326521
                4     SVM_7    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.650505
                5     SVM_4    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.326521
                6     SVM_6    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.326521
                7     SVM_5    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.650505
                8     SVM_9    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.650505
                9    SVM_10    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.326521
                10   SVM_11    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.650505
                11    SVM_8    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.326521
            >>> # View model evaluation stats.
            >>> gs_obj.model_stats
                MODEL_ID       MAE       MSE  MSLE        MAPE  ...        ME        R2        EV  MPD  MGD
                0     SVM_3  2.650505  8.459088   0.0  159.159527  ...  5.282729 -2.930531  0.333730  NaN  NaN
                1     SVM_1  2.650505  8.459088   0.0  159.159527  ...  5.282729 -2.930531  0.333730  NaN  NaN
                2     SVM_2  2.326521  6.218464   0.0   90.629648  ...  3.776410 -6.987358 -0.034968  NaN  NaN
                3     SVM_0  2.326521  6.218464   0.0   90.629648  ...  3.776410 -6.987358 -0.034968  NaN  NaN
                4     SVM_7  2.650505  8.459088   0.0  159.159527  ...  5.282729 -2.930531  0.333730  NaN  NaN
                5     SVM_4  2.326521  6.218464   0.0   90.629648  ...  3.776410 -6.987358 -0.034968  NaN  NaN
                6     SVM_6  2.326521  6.218464   0.0   90.629648  ...  3.776410 -6.987358 -0.034968  NaN  NaN
                7     SVM_5  2.650505  8.459088   0.0  159.159527  ...  5.282729 -2.930531  0.333730  NaN  NaN
                8     SVM_9  2.650505  8.459088   0.0  159.159527  ...  5.282729 -2.930531  0.333730  NaN  NaN
                9    SVM_10  2.326521  6.218464   0.0   90.629648  ...  3.776410 -6.987358 -0.034968  NaN  NaN
                10   SVM_11  2.650505  8.459088   0.0  159.159527  ...  5.282729 -2.930531  0.333730  NaN  NaN
                11    SVM_8  2.326521  6.218464   0.0   90.629648  ...  3.776410 -6.987358 -0.034968  NaN  NaN
            >>> # View best data, model ID and score.
            >>> print("Best data ID: ", gs_obj.best_data_id)
                Best data ID:  DF_0
            >>> print("Best model ID: ", gs_obj.best_model_id)
                Best model ID:  SVM_2
            >>> print("Best model score: ",gs_obj.best_score_)
                Best model score:  2.3265213466885375
            >>> # Performing prediction on sampled data using best trained model.
            >>> test_data = transform_obj.result.iloc[:5]
            >>> gs_pred = gs_obj.predict(newdata=test_data, **eval_params)
            >>> print("Prediction result: 
", gs_pred.result)
                Prediction result:       
                     id  prediction  MedHouseVal
                0   686   -0.214558        1.578
                1  2018    0.224954        0.578
                2  1754   -0.484374        1.651
                3   670   -0.288802        1.922
                4   244   -0.097476        1.117
            >>> # Perform evaluation using best model.
            >>> gs_obj.evaluate()
                ############ result Output ############
                        MAE       MSE  MSLE       MAPE        MPE      RMSE  RMSLE       ME        R2        EV  MPD  MGD
                0  2.326521  6.218464   0.0  90.629648  90.629648  2.493685    0.0  3.77641 -6.987358 -0.034968  NaN  NaN
            >>> # Retrieve any trained model.
            >>> gs_obj.get_model("SVM_1")
                ############ output_data Output ############
                   iterNum      loss       eta  bias
                0        3  2.078232  0.028868   0.0
                1        5  2.049456  0.022361   0.0
                2        6  2.037157  0.020412   0.0
                3        7  2.028186  0.018898   0.0
                4        9  2.012801  0.016667   0.0
                5       10  2.007469  0.015811   0.0
                6        8  2.020026  0.017678   0.0
9/14/2025, 16:50
Page 1,390 of 1,675
                7        4  2.063343  0.025000   0.0
                8        2  2.092763  0.035355   0.0
                9        1  2.112669  0.050000   0.0
                ############ result Output ############
                                         predictor    estimate       value
                attribute
                 7                        Latitude    0.077697        None
                -9         Learning Rate (Initial)    0.050000        None
                -17                   OneClass SVM         NaN       FALSE
                -14                        Epsilon    0.100000        None
                 5                      Population   -0.120322        None
                -12                       Nesterov         NaN        TRUE
                -5                             BIC   50.583018        None
                -7                           Alpha    0.500000  Elasticnet
                -3          Number of Observations   31.000000        None
                 0                     (Intercept)    0.000000        None
            >>> # Update the default model.
            >>> gs_obj.set_model("SVM_1")
            >>> # Example 3: Model trainer function. Performing hyperparameter-tuning
            >>> #            on SVM model trainer function using labeled multiple-dataframe.
            >>> # Initialize the GridSearch optimizer with model trainer
            >>> # function and parameter space required for model training.
            >>> gs_obj = GridSearch(func=SVM, params=params)
            >>> # Perform model optimization for SVM function for
            >>> # labeled multiple-dataframe support.
            >>> # Evaluation and prediction arguments are passed along with
            >>> # training dataframe.
            >>> gs_obj.fit(data={"Data-1":train_df_1, "Data-2":train_df_2}, **eval_params)
            >>> # View trained models.
            >>> gs_obj.models
                   MODEL_ID DATA_ID                                         PARAMETERS STATUS       MAE
                0     SVM_1  Data-2  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.286463
                1     SVM_3  Data-2  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.286463
                2     SVM_2  Data-1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.156109
                3     SVM_0  Data-1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.156109
                4     SVM_7  Data-2  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.286463
                5     SVM_4  Data-1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.156109
                6     SVM_5  Data-2  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.286463
                7     SVM_6  Data-1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.156109
                8    SVM_10  Data-1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.156109
                9     SVM_8  Data-1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.156109
                10    SVM_9  Data-2  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.286463
                11   SVM_11  Data-2  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.286463
            >>> # View model evaluation stats.
            >>> gs_obj.model_stats
                   MODEL_ID       MAE       MSE      MSLE        MAPE  ...        ME        R2        EV  MPD  MGD
                0     SVM_1  2.286463  5.721906  0.115319  120.188468  ...  3.280316 -3.436736  0.616960  NaN  NaN
                1     SVM_3  2.286463  5.721906  0.115319  120.188468  ...  3.280316 -3.436736  0.616960  NaN  NaN
                2     SVM_2  2.156109  6.986356  0.000000   97.766138  ...  4.737632 -2.195437 -0.235152  NaN  NaN
                3     SVM_0  2.156109  6.986356  0.000000   97.766138  ...  4.737632 -2.195437 -0.235152  NaN  NaN
                4     SVM_7  2.286463  5.721906  0.115319  120.188468  ...  3.280316 -3.436736  0.616960  NaN  NaN
                5     SVM_4  2.156109  6.986356  0.000000   97.766138  ...  4.737632 -2.195437 -0.235152  NaN  NaN
                6     SVM_5  2.286463  5.721906  0.115319  120.188468  ...  3.280316 -3.436736  0.616960  NaN  NaN
                7     SVM_6  2.156109  6.986356  0.000000   97.766138  ...  4.737632 -2.195437 -0.235152  NaN  NaN
                8    SVM_10  2.156109  6.986356  0.000000   97.766138  ...  4.737632 -2.195437 -0.235152  NaN  NaN
                9     SVM_8  2.156109  6.986356  0.000000   97.766138  ...  4.737632 -2.195437 -0.235152  NaN  NaN
                10    SVM_9  2.286463  5.721906  0.115319  120.188468  ...  3.280316 -3.436736  0.616960  NaN  NaN
                11   SVM_11  2.286463  5.721906  0.115319  120.188468  ...  3.280316 -3.436736  0.616960  NaN  NaN
            [12 rows x 13 columns]
            >>> # View best data, model ID and score.
            >>> print("Best data ID: ", gs_obj.best_data_id)
                Best data ID:  Data-1
            >>> print("Best model ID: ", gs_obj.best_model_id)
                Best model ID:  SVM_2
            >>> print("Best model score: ",gs_obj.best_score_)
                Best model score:  2.156108718480682
            >>> # Performing prediction on sampled data using best trained model.
            >>> test_data = transform_obj.result.iloc[:5]
            >>> gs_pred = gs_obj.predict(newdata=test_data, **eval_params)
            >>> print("Prediction result: 
", gs_pred.result)
9/14/2025, 16:50
Page 1,391 of 1,675
                Prediction result:
                     id  prediction  MedHouseVal
                0   686   -0.512750        1.578
                1  2018    0.065364        0.578
                2  1754   -0.849449        1.651
                3   670   -0.657097        1.922
                4   244   -0.285946        1.117
            >>> # Perform evaluation using best model.
            >>> gs_obj.evaluate()
                ############ result Output ############
                        MAE       MSE  MSLE       MAPE        MPE      RMSE  RMSLE        ME        R2        EV  MPD  MGD
                0  2.156109  6.986356   0.0  97.766138  83.453982  2.643172    0.0  4.737632 -2.195437 -0.235152  NaN  NaN
            >>> # Retrieve any trained model.
            >>> gs_obj.get_model("SVM_1")
                ############ output_data Output ############
                   iterNum      loss       eta  bias
                0        3  2.238049  0.028868   0.0
                1        5  2.198618  0.022361   0.0
                2        6  2.183347  0.020412   0.0
                3        7  2.171550  0.018898   0.0
                4        9  2.154619  0.016667   0.0
                5       10  2.147124  0.015811   0.0
                6        8  2.162718  0.017678   0.0
                7        4  2.217790  0.025000   0.0
                8        2  2.257826  0.035355   0.0
                9        1  2.286324  0.050000   0.0
                ############ result Output ############
                                          predictor   estimate                                   value
                attribute
                -7                           Alpha    0.500000                              Elasticnet
                -3          Number of Observations   31.000000                                    None
                 5                      Population   -0.094141                                    None
                 0                     (Intercept)    0.000000                                    None
                -17                   OneClass SVM         NaN                                   FALSE
                -16                         Kernel         NaN                                  LINEAR
                -1                   Loss Function         NaN  EPSILON_INSENSITIVE
                 7                        Latitude    0.169825                                    None
                -9         Learning Rate (Initial)    0.050000                                    None
                -14                        Epsilon    0.100000                                    None
            >>> # Update the default model.
            >>> gs_obj.set_model("SVM_1")
            >>> # Example 4: Model trainer function. Performing hyperparameter-tuning
            >>> #            on SVM model trainer function by passing unlabeled
            >>> #            multiple-dataframe as model hyperparameter.
            >>> # Define parameter space for model training.
            >>> params = {"data":(train_df_1, train_df_2),
                          "input_columns":['MedInc', 'HouseAge', 'AveRooms',
                                          'AveBedrms', 'Population', 'AveOccup',
                                          'Latitude', 'Longitude'],
                          "response_column":"MedHouseVal",
                          "model_type":"regression",
                          "batch_size":(11, 50, 75),
                          "iter_max":(100, 301),
                          "lambda1":0.1,
                          "alpha":0.5,
                          "iter_num_no_change":60,
                          "tolerance":0.01,
                          "intercept":False,
                          "learning_rate":"INVTIME",
                          "initial_data":0.5,
                          "decay_rate":0.5,
                          "momentum":0.6,
                          "nesterov":True,
                          "local_sgd_iterations":1}
            >>> # Initialize the GridSearch optimizer with model trainer
            >>> # function and parameter space required for model training.
            >>> gs_obj = GridSearch(func=SVM, params=params)
            >>> # Perform model optimization for SVM function for
            >>> # labeled multiple-dataframe support.
            >>> # Evaluation and prediction arguments are passed along with
            >>> # training dataframe.
9/14/2025, 16:50
Page 1,392 of 1,675
            >>> gs_obj.fit(**eval_params)
            >>> # View trained models.
            >>> gs_obj.models
                   MODEL_ID DATA_ID                                         PARAMETERS STATUS       MAE
                0     SVM_0    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.007936
                1     SVM_1    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.517338
                2     SVM_3    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.517338
                3     SVM_2    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.007936
                4     SVM_5    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.517338
                5     SVM_7    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.517338
                6     SVM_6    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.007936
                7     SVM_4    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.007936
                8     SVM_9    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.517338
                9     SVM_8    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.007936
                10   SVM_11    DF_1  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.517338
                11   SVM_10    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.007936
            >>> # View model evaluation stats.
            >>> gs_obj.model_stats
                   MODEL_ID       MAE       MSE      MSLE        MAPE  ...        ME        R2        EV  MPD  MGD
                0     SVM_0  2.007936  5.402427  0.007669   88.199346  ...  3.981598 -6.898063 -1.003772  NaN  NaN
                1     SVM_1  2.517338  7.470182  0.000000  118.722467  ...  4.035658 -7.827958 -0.716572  NaN  NaN
                2     SVM_3  2.517338  7.470182  0.000000  118.722467  ...  4.035658 -7.827958 -0.716572  NaN  NaN
                3     SVM_2  2.007936  5.402427  0.007669   88.199346  ...  3.981598 -6.898063 -1.003772  NaN  NaN
                4     SVM_5  2.517338  7.470182  0.000000  118.722467  ...  4.035658 -7.827958 -0.716572  NaN  NaN
                5     SVM_7  2.517338  7.470182  0.000000  118.722467  ...  4.035658 -7.827958 -0.716572  NaN  NaN
                6     SVM_6  2.007936  5.402427  0.007669   88.199346  ...  3.981598 -6.898063 -1.003772  NaN  NaN
                7     SVM_4  2.007936  5.402427  0.007669   88.199346  ...  3.981598 -6.898063 -1.003772  NaN  NaN
                8     SVM_9  2.517338  7.470182  0.000000  118.722467  ...  4.035658 -7.827958 -0.716572  NaN  NaN
                9     SVM_8  2.007936  5.402427  0.007669   88.199346  ...  3.981598 -6.898063 -1.003772  NaN  NaN
                10   SVM_11  2.517338  7.470182  0.000000  118.722467  ...  4.035658 -7.827958 -0.716572  NaN  NaN
                11   SVM_10  2.007936  5.402427  0.007669   88.199346  ...  3.981598 -6.898063 -1.003772  NaN  NaN
            [12 rows x 13 columns]
            >>> # View best data, model ID and score.
            >>> print("Best data ID: ", gs_obj.best_data_id)
                Best data ID:  DF_0
            >>> print("Best model ID: ", gs_obj.best_model_id)
                Best model ID:  SVM_0
            >>> print("Best model score: ",gs_obj.best_score_)
                Best model score:  2.0079362549355104
            >>> # Performing prediction on sampled data using best trained model.
            >>> test_data = transform_obj.result.iloc[:5]
            >>> gs_pred = gs_obj.predict(newdata=test_data, **eval_params)
            >>> print("Prediction result: 
", gs_pred.result)
                Prediction result:       
                     id  prediction  MedHouseVal
                0   686   -0.365955        1.578
                1  2018    0.411846        0.578
                2  1754   -0.634807        1.651
                3   670   -0.562927        1.922
                4   244   -0.169730        1.117
            >>> # Perform evaluation using best model.
            >>> gs_obj.evaluate()
                ############ result Output ############
                        MAE       MSE      MSLE       MAPE        MPE      RMSE     RMSLE        ME        R2        EV  MPD  M
                0  2.007936  5.402427  0.007669  88.199346  88.199346  2.324312  0.087574  3.981598 -6.898063 -1.003772  NaN  N
            >>> # Retrieve any trained model.
            >>> gs_obj.get_model("SVM_1")
                ############ output_data Output ############
                   iterNum      loss       eta  bias
                0        3  2.154842  0.028868   0.0
                1        5  2.129916  0.022361   0.0
                2        6  2.118539  0.020412   0.0
                3        7  2.107991  0.018898   0.0
                4        9  2.089022  0.016667   0.0
                5       10  2.080426  0.015811   0.0
                6        8  2.098182  0.017678   0.0
                7        4  2.142030  0.025000   0.0
                8        2  2.168233  0.035355   0.0
                9        1  2.186740  0.050000   0.0
                ############ result Output ############
                                          predictor   estimate       value
                attribute
9/14/2025, 16:50
Page 1,393 of 1,675
                 7                        Latitude    0.010463        None
                -9         Learning Rate (Initial)    0.050000        None
                -17                   OneClass SVM         NaN       FALSE
                -14                        Epsilon    0.100000        None
                 5                      Population   -0.348591        None
                -12                       Nesterov         NaN        TRUE
                -5                             BIC   50.585888        None
                -7                           Alpha    0.500000  Elasticnet
                -3          Number of Observations   31.000000        None
                 0                     (Intercept)    0.000000        None
            >>> # Update the default model.
            >>> gs_obj.set_model("SVM_1")
            >>> # Example 5: Non-Model trainer function. Performing GridSearch
            >>> #            on AntiSelect model trainer function.
            >>> # Load the example dataset.
            >>> load_example_data("teradataml", "titanic")
            >>> # Create teradaraml dataframe.
            >>> titanic = DataFrame.from_table("titanic")
            >>> # Define the non-model trainer function parameter space.
            >>> # Include input data in parameter space for non-model trainer function.
            >>> params = {"data":titanic, "exclude":(
                                            ['survived', 'name', 'age'],
                                            ["ticket", "parch", "sex", "age"])}
            >>> # Import non-model trainer function and optimizer.
            >>> from teradataml import Antiselect, GridSearch
            >>> # Initialize the GridSearch optimizer with non-model trainer
            >>> # function and parameter space required for non-model training.
            >>> gs_obj = GridSearch(func=Antiselect, params=params)
            >>> # Perform execution of Antiselect function.
            >>> gs_obj.fit()
            >>> # View trained models.
            >>> gs_obj.models
                       MODEL_ID                                         PARAMETERS STATUS
                0  ANTISELECT_1  {'data': '"titanic"', 'exclude': ['ticket', 'p...   PASS
                1  ANTISELECT_0  {'data': '"titanic"', 'exclude': ['survived', ...   PASS
            >>> # Retrieve any trained model using "MODEL_ID".
            >>> gs_obj.get_model("ANTISELECT_1")
                ############ result Output ############
                   passenger  survived  pclass                                                name  sibsp      fare cabin embar
                0        162         1       2  Watt, Mrs. James (Elizabeth "Bessie" Inglis Milne)      0   15.7500  None      
                1        591         0       3                                Rintamaki, Mr. Matti      0    7.1250  None      
                2        387         0       3                     Goodwin, Master. Sidney Leonard      5   46.9000  None      
                3        469         0       3                                  Scanlan, Mr. James      0    7.7250  None      
                4        326         1       1                            Young, Miss. Marie Grice      0  135.6333   C32      
                5        265         0       3                                  Henry, Miss. Delia      0    7.7500  None      
                6        530         0       2                         Hocking, Mr. Richard George      2   11.5000  None      
                7        244         0       3                       Maenpaa, Mr. Matti Alexanteri      0    7.1250  None      
                8         61         0       3                               Sirayanian, Mr. Orsen      0    7.2292  None      
                9        122         0       3                          Moore, Mr. Leonard Charles      0    8.0500  None      
evaluate
teradataml.hyperparameter_tuner.optimizer.GridSearch.evaluate = evaluate(self, **kwargs)
DESCRIPTION:
    Function uses trained models from SQLE, VAL and UAF features for 
    evaluations. evaluations are made using the default trained model.
    Notes: 
        * Evaluation supported for evaluatable model-trainer functions.
        * Best model is set as default model by default.
        * Default model can be changed using "set_model()" method.
PARAMETERS:
    kwargs:
        Optional Argument.
        Specifies the keyword arguments. Accepts additional arguments 
        required for the teradataml analytic function evaluations. 
        While "kwargs" is empty then internal sampled test dataset
        and arguments used for evaluation. Otherwise, 
        All arguments required with validation data need to be passed
        for evaluation.
RETURNS:
    Output teradataml DataFrames can be accessed using attribute
9/14/2025, 16:50
Page 1,394 of 1,675
    references, such as HPTEvaluateObj.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Perform evaluation using best model.
    >>> optimizer_obj.evaluate(newdata=test_data, **eval_params)
        ############ result Output ############
                MAE       MSE  MSLE        MAPE         MPE      RMSE  RMSLE        ME       R2       EV  MPD  MGD
        0  2.616772  8.814968   0.0  101.876866  101.876866  2.969001    0.0  5.342344 -4.14622 -0.14862  NaN  NaN
ﬁt
teradataml.hyperparameter_tuner.optimizer.GridSearch.ﬁt = ﬁt(self, data=None, evaluation_metric=None, early_stop=None, frac=0.8, run_parallel=True, wait=True,
verbose=0, stratify_column=None, sample_id_column=None, sample_seed=None, max_time=None, **kwargs)
DESCRIPTION:
    Function to perform hyperparameter tuning using GridSearch algorithm.
    Notes:
        * In the Model trainer function, the best parameters are 
          selected based on training results.
        * In the Non model trainer function, First execution parameter
          set is selected as the best parameters.
PARAMETERS:
    data:
        Optional Argument.
        Specifies the input teradataml DataFrame for model trainer function.
        Notes:
            * DataFrame need not to be passed in fit() methods, when "data" is 
              passed as a model hyperparameters ("params"). 
            * "data" is a required argument for model trainer functions.
            * "data" is ignored for non-model trainer functions.
            * "data" can be contain single DataFrame or multiple DataFrame.
            * One can pass multiple dataframes to "data". Hyperparameter 
              tuning is performed on all the dataframes for every model 
              parameter.
            * "data" can be either a dictionary OR a tuple OR a dataframe.
                * If it is a dictionary then Key represents the label for 
                  dataframe and Value represents the dataframe.
                * If it is a tuple then teradataml converts it to dictionary
                  by generating the labels internally.
                * If it is a dataframe then teradataml label it as "DF_0".
        Types: teradataml DataFrame, dictionary, tuples
    evaluation_metric:
        Optional Argument.
        Specifies the evaluation metrics to considered for model 
        evaluation.
        Notes:
            * evaluation_metric applicable for model trainer functions.
            * Best model is not selected when evaluation returns 
              non-finite values.
        Permitted Values:
            * Classification: Accuracy, Micro-Precision, Micro-Recall,
                              Micro-F1, Macro-Precision, Macro-Recall,
                              Macro-F1, Weighted-Precision, 
                              Weighted-Recall,
                              Weighted-F1.
            * Regression: MAE, MSE, MSLE, MAPE, MPE, RMSE, RMSLE, ME, 
                          R2, EV, MPD, MGD
        Default Value:
            * Classification: Accuracy
            * Regression: MAE
        Types: str
    early_stop:
        Optional Argument.
        Specifies the early stop mechanism value for model trainer 
        functions. Hyperparameter tuning ends model training when 
        the training model evaluation metric attains "early_stop" value.
        Note:
            * Early stopping supports only when evaluation returns 
              finite value.
        Types: int or float
    frac:
        Optional Argument.
9/14/2025, 16:50
Page 1,395 of 1,675
        Specifies the split percentage of rows to be sampled for training 
        and testing dataset. "frac" argument value must range between (0, 1).
        Notes: 
            * This "frac" argument is not supported for non-model trainer 
              function.
            * The "frac" value is considered as train split percentage and 
              The remaining percentage is taken into account for test splitting.
        Default Value: 0.8
        Types: float
    run_parallel:
        Optional Argument.
        Specifies the parallel execution functionality of hyperparameter 
        tuning. When "run_parallel" set to true, model functions are 
        executed concurrently. Otherwise, model functions are executed 
        sequentially.
        Default Value: True
        Types: bool
    wait:
        Optional Argument.
        Specifies whether to wait for the completion of execution 
        of hyperparameter tuning or not. When set to False, hyperparameter 
        tuning is executed in the background and user can use "is_running()" 
        method to check the status. Otherwise it waits until the execution 
        is complete to return the control back to user.
        Default Value: True
        Type: bool
    verbose:
        Optional Argument.
        Specifies whether to log the model training information and display 
        the logs. When it is set to 1, progress bar alone logged in the 
        console. When it is set to 2, along with progress bar, execution 
        steps and execution time is logged in the console. When it is set 
        to 0, nothing is logged in the console. 
        Note:
            * verbose is not significant when "wait" is 'False'.
        Default Value: 0
        Type: bool
    sample_seed:
        Optional Argument.
        Specifies the seed value that controls the shuffling applied
        to the data before applying the Train-Test split. Pass an int for 
        reproducible output across multiple function calls.
        Notes:
            * When the argument is not specified, different
              runs of the query generate different outputs.
            * It must be in the range [0, 2147483647]
            * Seed is supported for stratify column.
        Types: int
    stratify_column:
        Optional Argument.
        Specifies column name that contains the labels indicating
        which data needs to be stratified for TrainTest split. 
        Notes:
            * seed is supported for stratify column.
        Types: str
    sample_id_column:
        Optional Argument.
        Specifies the input data column name that has the
        unique identifier for each row in the input.
        Note:
            * Mandatory when "sample_seed" argument is present.
        Types: str
    max_time:
        Optional Argument.
        Specifies the maximum time for the completion of Hyperparameter tuning execution.
        Default Value: None
        Types: int or float
    kwargs:
        Optional Argument.
        Specifies the keyword arguments. Accepts additional arguments 
        required for the teradataml analytic function.
RETURNS:
    None
RAISES:
9/14/2025, 16:50
Page 1,396 of 1,675
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> # Create an instance of the GridSearch algorithm called "optimizer_obj" 
    >>> optimizer_obj = GridSearch(func=SVM, params=params)
    >>> eval_params = {"id_column": "id",
                       "accumulate": "MedHouseVal"}
    >>> # Example 1: Passing single DataFrame for model trainer function.
    >>> optimizer_obj.fit(data=train_df,
                          evaluation_metric="MAE",
                          early_stop=70.9,
                          **eval_params)
    >>> # Example 2: Passing multiple datasets as tuple of DataFrames for 
    >>> #            model trainer function.
    >>> optimizer_obj.fit(data=(train_df_1, train_df_2),
                          evaluation_metric="MAE",
                          early_stop=70.9,
                          **eval_params)  
    >>> # Example 3: Passing multiple datasets as dictionary of DataFrames 
    >>> #            for model trainer function.
    >>> optimizer_obj.fit(data={"Data-1":train_df_1, "Data-2":train_df_2},
                          evaluation_metric="MAE",
                          early_stop=70.9,
                          **eval_params)  
    >>> # Example 4: No data argument passed in fit() method for model trainer function.
    >>> #            Note: data argument must be passed while creating HPT object as 
    >>> #                  model hyperparameters.
    >>> # Define parameter space for model training with "data" argument.
    >>> params = {"data":(df1, df2),
                  "input_columns":['MedInc', 'HouseAge', 'AveRooms',
                                   'AveBedrms', 'Population', 'AveOccup',
                                   'Latitude', 'Longitude'],
                  "response_column":"MedHouseVal",
                  "model_type":"regression",
                  "batch_size":(11, 50, 75),
                  "iter_max":(100, 301),
                  "intercept":False,
                  "learning_rate":"INVTIME",
                  "nesterov":True,
                  "local_sgd_iterations":1}
    >>> # Create "optimizer_obj" using GridSearch algorithm and perform 
    >>> # fit() method without any "data" argument for model trainer function.
    >>> optimizer_obj.fit(evaluation_metric="MAE",
                          early_stop=70.9,
                          **eval_params) 
    >>> # Example 5: Do not pass data argument in fit() method for 
    >>> #            non-model trainer function.
    >>> #            Note: data argument must be passed while creating HPT  
    >>> #                  object as model hyperparameters.
    >>> optimizer_obj.fit() 
    >>> # Example 6: Passing "verbose" argument value '1' in fit() method to 
    >>> #            display model log.
    >>> optimizer_obj.fit(data=train_df, evaluation_metric="R2",
                          verbose=1, **eval_params)
        completed: |████████████████████████████████████████████████████████████| 100% - 6/6
    >>> # Example 7: max_time argument is passed in fit() method.
    >>> # Model training parameters
    >>> model_params = {"input_columns":['sepal_length', 'sepal_width', 'petal_length', 'petal_width'],
    ...                 "response_column" :'species',
    ...                 "max_depth":(5,10,15),
    ...                 "lambda1" :(1000.0,0.001),
    ...                 "model_type" :"Classification",
    ...                 "seed":32,
    ...                 "shrinkage_factor":0.1,
    ...                 "iter_num":(5, 50)}
    >>>
    >>> eval_params = {"id_column": "id",
    ...                "accumulate":"species",
    ...                "model_type":'Classification',
    ...                "object_order_column":['task_index', 'tree_num', 'iter','class_num', 'tree_order']
                    }
    >>>
    >>> # Import model trainer function and optimizer.
    >>> from teradataml import XGBoost, GridSearch
    >>>
9/14/2025, 16:50
Page 1,397 of 1,675
    >>> # Initialize the GridSearch optimizer with model trainer
    >>> # function and parameter space required for model training.
    >>> gs_obj = GridSearch(func=XGBoost, params=model_params)
    >>>
    >>> # fit() method with max_time argument(in seconds) for model trainer function.
    >>> gs_obj.fit(data=data, max_time=30, verbose=2, **eval_params)
        Model_id:XGBOOST_2 - Run time:33.277s - Status:PASS - ACCURACY:0.933               
        Model_id:XGBOOST_3 - Run time:33.276s - Status:PASS - ACCURACY:0.933               
        Model_id:XGBOOST_0 - Run time:33.279s - Status:PASS - ACCURACY:0.967                
        Model_id:XGBOOST_1 - Run time:33.278s - Status:PASS - ACCURACY:0.933                
        Computing: ｜⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾｜ 33% - 4/12
    >>>
    >>> # status 'SKIP' for the models which are not completed within the max_time.
    >>> gs_obj.models
            MODEL_ID    DATA_ID                                        PARAMETERS       STATUS  ACCURACY
        0       XGBOOST_2       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       PASS    0.933333
        1       XGBOOST_4       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       SKIP    NaN
        2       XGBOOST_5       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       SKIP    NaN
        3       XGBOOST_6       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       SKIP    NaN
        4       XGBOOST_7       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       SKIP    NaN
        5       XGBOOST_8       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       SKIP    NaN
        6       XGBOOST_9       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       SKIP    NaN
        7       XGBOOST_10      DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       SKIP    NaN
        8       XGBOOST_11      DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       SKIP    NaN
        9       XGBOOST_3       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       PASS    0.933333
        10      XGBOOST_0       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       PASS    0.966667
        11      XGBOOST_1       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       PASS    0.933333
get_error_log
teradataml.hyperparameter_tuner.optimizer.GridSearch.get_error_log = get_error_log(self, model_id)
DESCRIPTION:
    Function to get the error logs of a failed model training in the fit method.
PARAMETERS:
    model_id:
        Required Argument.
        Specifies the unique identifier for model.
        Note:
             * Only failed model training error log is returned.
        Types: str
RETURNS:
    string
RAISES:
    TypeError, ValueError
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the error log.
    >>> optimizer_obj.get_error_log("SVM_2")
        "[Teradata][teradataml](TDML_2082) Value of 'iter_max' must be greater 
        than or equal to 1 and less than or equal to 10000000."
get_input_data
teradataml.hyperparameter_tuner.optimizer.GridSearch.get_input_data = get_input_data(self, data_id)
DESCRIPTION:
    Function to get the input data used by model trainer functions.
    Unique identifiers (data_id) is used to get the training data.
    In case of unlabeled data such as single dataframe or tuple of 
    dataframe, default unique identifiers are assigned. Hence, unlabeled
    training data is retrieved using default unique identifiers.
    Notes:
        * Function only returns input data for model trainer functions.
        * Train and Test sampled data are returned for supervised 
          model trainer function (evaluatable functions).
        * Train data is returned for unsupervised-model trainer function 
          (non-evaluatable functions).
PARAMETERS:
    data_id:
        Required Argument.
        Specifies the unique data identifier used for model training.
        Types: str
RETURNS:
    teradataml DataFrame
RAISES:
    ValueError
9/14/2025, 16:50
Page 1,398 of 1,675
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the training data.
    >>> optimizer_obj.get_input_data(data_id="DF_1")
        [{'data':       id  MedHouseVal    MedInc  HouseAge  AveRooms  AveBedrms  Population  AveOccup  Latitude  Longitude
        0  19789        0.660 -1.154291 -0.668250  0.862203   7.021803   -1.389101 -1.106515  2.367716  -1.710719
        1  17768        1.601 -0.447350 -0.162481 -0.431952  -0.156872    2.436223  2.172854  0.755780  -1.016640
        2  19722        0.675 -0.076848  1.439120  1.805547   1.944759   -1.186169  0.326739  1.459894  -0.974996
        3  18022        3.719  1.029892  0.343287  0.635952  -0.480133   -0.914869 -0.160824  0.711496  -1.067540
        4  15749        3.500 -0.182247  1.776299 -0.364226   0.035715   -0.257239 -0.970166  0.941772  -1.294272
        5  11246        2.028 -0.294581 -0.583955 -0.265916  -0.270654    0.182266 -0.703494 -0.807444   0.764827
        6  16736        3.152  0.943735  1.439120 -0.747066  -1.036053   -1.071138 -0.678411  0.906345  -1.234118
        7  12242        0.775 -1.076758 -0.752545 -0.424517   0.460470    0.742228 -0.597809 -0.838443   1.241428
        8  14365        2.442 -0.704218  1.017646 -0.428965  -0.367301   -1.014707 -1.333045 -1.294568   1.121121
        9  18760        1.283  0.019018 -1.258313  0.754993   0.013994    0.094365  0.222254  2.195008  -1.201728}, 
        {'newdata':       id  MedHouseVal    MedInc  HouseAge  AveRooms  AveBedrms  Population  AveOccup  Latitude  Longitude
        0  16102        2.841  0.206284  1.270530 -0.248620  -0.224210   -0.059733 -0.242386  0.937344  -1.317408
        1  15994        3.586  0.306050  1.439120  0.255448  -0.334613   -0.160657 -0.426510  0.937344  -1.303526
        2  15391        2.541  0.423107 -1.595492  0.951807  -0.061005    1.955480  0.517572 -1.055434   1.236801
        3  18799        0.520 -0.677565 -0.415366  0.548756   1.254406   -0.883398 -0.534060  2.358859  -1.035149
        4  19172        1.964  0.247152 -0.162481  0.428766  -0.427459   -0.175849 -0.451380  1.238475  -1.396070
        5  18164        3.674  0.295345 -1.258313 -1.078181   0.175885    0.045531 -1.298667  0.760208  -1.099930
        6  13312        1.598  0.484475 -1.342608  0.767557  -0.229585    0.113899  0.361520 -0.692306   0.949915
        7  12342        1.590 -0.520029 -0.246776  0.973345   1.407755    2.325532 -0.406887 -0.798587   1.445024}]
get_model
teradataml.hyperparameter_tuner.optimizer.GridSearch.get_model = get_model(self, model_id)
DESCRIPTION:
    Function to get the model.
PARAMETERS:
    model_id:
        Required Argument.
        Specifies the unique identifier for model.
        Notes:
             * Trained model results returned for model trainer functions.
             * Executed function results returned for non-model trainer
               functions.
        Types: str
RETURNS:
    Object of teradataml analytic functions.
    Note:
        * Attribute references remains same as that of the function 
          attributes.
RAISES:
    TeradataMlException, ValueError
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the trained model.
    >>> optimizer_obj.get_model(model_id="SVM_1")
    ############ output_data Output ############
       iterNum      loss       eta  bias
    0        3  2.265289  0.028868   0.0
    1        5  2.254413  0.022361   0.0
    2        6  2.249260  0.020412   0.0
    3        7  2.244463  0.018898   0.0
    4        9  2.235800  0.016667   0.0
    5       10  2.231866  0.015811   0.0
    6        8  2.239989  0.017678   0.0
    7        4  2.259956  0.025000   0.0
    8        2  2.271862  0.035355   0.0
    9        1  2.280970  0.050000   0.0
    ############ result Output ############
                              predictor  estimate                                   value
    attribute
    -7                           Alpha    0.50000                              Elasticnet
    -3          Number of Observations   31.00000                                    None
     5                      Population   -0.32384                                    None
     0                     (Intercept)    0.00000                                    None
    -17                   OneClass SVM        NaN                                   FALSE
    -16                         Kernel        NaN                                  LINEAR
    -1                   Loss Function        NaN  EPSILON_INSENSITIVE
9/14/2025, 16:50
Page 1,399 of 1,675
     7                        Latitude    0.00000                                    None
    -9         Learning Rate (Initial)    0.05000                                    None
    -14                        Epsilon    0.10000                                    None
get_parameter_grid
teradataml.hyperparameter_tuner.optimizer.GridSearch.get_parameter_grid = get_parameter_grid(self)
DESCRIPTION:
    Returns the value of the attribute _parameter_grid.
RETURNS:
    dict
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve parameter grid.
    >>> optimizer_obj.get_parameter_grid()
        [{'param': {'input_columns': ['MedInc', 'HouseAge', 'AveRooms', 'AveBedrms', 
                                     'Population', 'AveOccup', 'Latitude', 'Longitude'], 
                    'response_column': 'MedHouseVal', 'model_type': 'regression', 
                    'batch_size': 75, 'iter_max': 100, 'lambda1': 0.1, 'alpha': 0.5, 
                    'iter_num_no_change': 60, 'tolerance': 0.01, 'intercept': False,
                    'learning_rate': 'INVTIME', 'initial_data': 0.5, 'decay_rate': 0.5, 
                    'momentum': 0.6, 'nesterov': True, 'local_sgd_iterations': 1, 
                    'data': '"ALICE"."ml__select__1696593660430612"'}, 
         'data_id': 'DF_0'}, 
         {'param': {'input_columns': ['MedInc', 'HouseAge', 'AveRooms', 'AveBedrms', 
                                     'Population', 'AveOccup', 'Latitude', 'Longitude'], 
                    'response_column': 'MedHouseVal', 'model_type': 'regression', 
                    'batch_size': 75, 'iter_max': 100, 'lambda1': 0.1, 'alpha': 0.5, 
                    'iter_num_no_change': 60, 'tolerance': 0.01, 'intercept': False, 
                    'learning_rate': 'INVTIME', 'initial_data': 0.5, 'decay_rate': 0.5, 
                    'momentum': 0.6, 'nesterov': True, 'local_sgd_iterations': 1, 
                    'data': '"ALICE"."ml__select__1696593660430612"'}, 
         'data_id': 'DF_1'}]
is_running
teradataml.hyperparameter_tuner.optimizer.GridSearch.is_running = is_running(self)
DESCRIPTION:
    Check whether hyperparameter tuning is completed or not. Function 
    returns True when execution is in progress. Otherwise it returns False.
PARAMETERS:
    None
RAISES:
    None
RETURNS:
    bool
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the model execution status.
    >>> optimizer_obj.is_running() 
        False
predict
teradataml.hyperparameter_tuner.optimizer.GridSearch.predict = predict(self, **kwargs)
DESCRIPTION:
    Function uses model training function generated models from SQLE, 
    VAL and UAF features for predictions. Predictions are made using 
    the best trained model. Predict function is not supported for
    non-model trainer function.
PARAMETERS:
    kwargs:
        Optional Argument.
        Specifies the keyword arguments. Accepts all merge model 
        predict feature arguments required for the teradataml 
        analytic function predictions.
RETURNS:
    Output teradataml DataFrames can be accessed using attribute
    references, such as HPTObj.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
9/14/2025, 16:50
Page 1,400 of 1,675
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Perform prediction using "optimizer_obj".
    >>> optimizer_obj.predict(newdata=test_data, **eval_params)
             id  prediction  MedHouseVal
        0   686    0.202843        1.578
        1  2018    0.149868        0.578
        2  1754    0.211870        1.651
        3   670    0.192414        1.922
        4   244    0.247545        1.117
set_model
teradataml.hyperparameter_tuner.optimizer.GridSearch.set_model = set_model(self, model_id)
DESCRIPTION:
    Function to set the model to use for Prediction.
PARAMETERS:
    model_id:
        Required Argument.
        Specifies the unique identifier for model.
        Note:
             * Not significant for non-model trainer functions.
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException, ValueError
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Set the default trained model.
    >>> optimizer_obj.set_model(model_id="SVM_1")
Properties
best_data_id
teradataml.hyperparameter_tuner.optimizer.GridSearch.best_data_id
DESCRIPTION:
    Returns the "data_id" of a sampled data used for training the best model.
    Note:
        "best_data_id" is not supported for non-model trainer functions.
RETURNS:
    String representing the best "data_id"
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best data id.
    >>> optimizer_obj.best_data_id
        DF_0
best_model
teradataml.hyperparameter_tuner.optimizer.GridSearch.best_model
DESCRIPTION:
    Returns the best trained model obtained from hyperparameter tuning.
    Note:
        "best_model" is not supported for non-model trainer functions.
RETURNS:
    object of trained model.
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best model.
    >>> optimizer_obj.best_model
        ############ output_data Output ############
           iterNum      loss       eta  bias
        0        3  2.060386  0.028868   0.0
9/14/2025, 16:50
Page 1,401 of 1,675
        1        5  2.055509  0.022361   0.0
        2        6  2.051982  0.020412   0.0
        3        7  2.048387  0.018898   0.0
        4        9  2.041521  0.016667   0.0
        5       10  2.038314  0.015811   0.0
        6        8  2.044882  0.017678   0.0
        7        4  2.058757  0.025000   0.0
        8        2  2.065932  0.035355   0.0
        9        1  1.780877  0.050000   0.0
        ############ result Output ############
                                 predictor    estimate       value
        attribute
         7                        Latitude    0.155095        None
        -9         Learning Rate (Initial)    0.050000        None
        -17                   OneClass SVM         NaN       FALSE
        -14                        Epsilon    0.100000        None
         5                      Population    0.000000        None
        -12                       Nesterov         NaN        TRUE
        -5                             BIC   73.297397        None
        -7                           Alpha    0.500000  Elasticnet
        -3          Number of Observations   55.000000        None
         0                     (Intercept)    0.000000        None
best_model_id
teradataml.hyperparameter_tuner.optimizer.GridSearch.best_model_id
DESCRIPTION:
    Returns the model id of the model with best score.
    Note:
        "best_model_id" is not supported for non-model trainer functions.
RETURNS:
    String representing the best model id.
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best model id.
    >>> optimizer_obj.best_model_id
        'SVM_2'
best_params_
teradataml.hyperparameter_tuner.optimizer.GridSearch.best_params_
DESCRIPTION:
    Returns the parameters used for the model with best score.
    Note:
        "best_params_" is not supported for non-model trainer functions.
RETURNS:
    dict
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best parameters.
    >>> optimizer_obj.best_params_
        {'input_columns': ['MedInc', 'HouseAge', 'AveRooms', 
                          'AveBedrms', 'Population', 'AveOccup', 'Latitude', 'Longitude'], 
         'response_column': 'MedHouseVal', 'model_type': 'regression', 
         'batch_size': 50, 'iter_max': 301, 'lambda1': 0.1, 'alpha': 0.5, 
         'iter_num_no_change': 60, 'tolerance': 0.01, 'intercept': False, 
         'learning_rate': 'INVTIME', 'initial_data': 0.5, 'decay_rate': 0.5, 
         'momentum': 0.6, 'nesterov': True, 'local_sgd_iterations': 1, 
         'data': '"ALICE"."ml__select__1696595493985650"'}
best_sampled_data_
teradataml.hyperparameter_tuner.optimizer.GridSearch.best_sampled_data_
DESCRIPTION:
    Returns the best sampled data used for training the best model.
    Note:
        "best_sampled_data_" is not supported for non-model trainer functions.
RETURNS:
    list of DataFrames.
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
9/14/2025, 16:50
Page 1,402 of 1,675
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best sampled data.
    >>> optimizer_obj.best_sampled_data_
        [{'data':       id  MedHouseVal    MedInc  HouseAge  AveRooms  AveBedrms  Population  AveOccup  Latitude  Longitude
        0   5233        0.955 -0.895906  0.680467 -0.387272  -0.202806   -0.125930  2.130214 -0.754303   0.653775
        1  10661        3.839  2.724825 -1.258313  0.876263  -1.142947   -0.751004 -0.187396 -0.878298   0.852744
        2  10966        1.896  0.057849  0.343287 -0.141762  -0.664624   -0.095545  0.588981 -0.829586   0.815727
        3   3687        1.741 -0.383816 -1.679787 -0.849458   0.108000    0.718354  1.083500 -0.630308   0.593621
        4   7114        2.187 -0.245392  0.258993  0.225092  -0.205781   -0.171508 -0.035650 -0.763160   0.755573
        5   5300        3.500 -0.955800 -1.005429 -1.548811  -0.130818    2.630473 -0.601956 -0.696734   0.556604
        6    686        1.578 -0.152084 -0.078186 -0.625426  -0.513581   -0.685892 -0.533101  0.906345  -1.141575
        7   9454        0.603 -1.109609 -0.499660  0.355748   0.379188   -0.364674 -0.356799  1.827451  -1.655193
        8   5202        1.000 -0.307539  1.101940 -0.379623  -0.570271   -0.141123  0.595366 -0.754303   0.635266
        9   5769        2.568 -0.413546  0.343287 -0.922324  -0.028824    1.165456  0.031374 -0.656879   0.626012}, 
        {'newdata':      id  MedHouseVal    MedInc  HouseAge  AveRooms  AveBedrms  Population  AveOccup  Latitude  Longitude
        0  1754        1.651 -0.026315  0.596172  0.454207  -0.027273    0.068320 -0.082765  1.017055  -1.234118
        1  3593        2.676  1.241775  0.090403  1.024283  -0.367626   -0.045626  0.252048 -0.621452   0.542722
        2  7581        1.334 -0.714880 -1.258313 -0.604140  -0.259612    3.058041  0.857406 -0.776445   0.658402
        3  8783        2.500 -0.170156  0.596172  0.163717   0.398242   -0.668529 -0.728130 -0.820729   0.621385
        4  5611        1.587 -0.712366 -0.415366 -1.275716   0.012960    0.860515  0.764870 -0.820729   0.639893
        5   244        1.117 -0.605796  1.101940 -0.160367   0.426668    1.022209  1.041018  0.946201  -1.187846}]
best_score_
teradataml.hyperparameter_tuner.optimizer.GridSearch.best_score_
DESCRIPTION:
    Returns the best score of the model out of all generated models.
    Note:
        "best_score_" is not supported for non-model trainer functions.
RETURNS:
    String representing the best score.
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best score.
    >>> optimizer_obj.best_score_
        2.060386
model_stats
teradataml.hyperparameter_tuner.optimizer.GridSearch.model_stats
DESCRIPTION:
    Returns the model statistics of the model with best score.
RETURNS:
    pandas DataFrame.
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the model stats.
    >>> optimizer_obj.model_stats
          MODEL_ID DATA_ID                                         PARAMETERS STATUS       MAE
        0    SVM_3    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772`
        1    SVM_0    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.660815
        2    SVM_1    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.660815
        3    SVM_2    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
        4    SVM_4    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
        5    SVM_5    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772`
models
teradataml.hyperparameter_tuner.optimizer.GridSearch.models
DESCRIPTION:
    Returns the generated models metadata.
RETURNS:
    pandas DataFrame
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve models metadata.
    >>> optimizer_obj.models
          MODEL_ID DATA_ID                                         PARAMETERS STATUS       MAE
        0    SVM_3    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
        1    SVM_0    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.660815
        2    SVM_1    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.660815
9/14/2025, 16:50
Page 1,403 of 1,675
        3    SVM_2    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
        4    SVM_4    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
        5    SVM_5    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS  2.616772
RandomSearch
Methods
__init__
teradataml.hyperparameter_tuner.optimizer.RandomSearch.__init__ = __init__(self, func, params, n_iter=10, **kwargs)
        DESCRIPTION:
            RandomSearch algorithm performs random sampling on hyperparameter 
            space to identify optimal hyperparameters. It works for
            teradataml analytic functions from SQLE, BYOM, VAL and UAF features.
            teradataml RandomSearch allows user to perform hyperparameter tuning for 
            all model trainer and non-model trainer functions.
            When used for model trainer functions:
                * Based on evaluation metrics search determines best model.
                * All methods and properties can be used.
            When used for non-model trainer functions:
                * Only fit() method is supported.
                * User can choose the best output as they see fit to use this.
            teradataml RandomSearch also allows user to use input data as the 
            hyperparameter. This option can be suitable when the user wants to
            identify the best models for a set of input data. When user passes
            set of data as hyperparameter for model trainer function, the search
            determines the best data along with the best model based on the 
            evaluation metrics.
            Note:
                * configure.temp_object_type="VT" follows sequential execution.
        PARAMETERS:
            func:
                Required Argument.
                Specifies a teradataml analytic function from SQLE, VAL, and UAF.
                Types:
                    teradataml Analytic Functions
                        * Advanced analytic functions
                        * UAF
                        * VAL
                    Refer to display_analytic_functions() function for list of functions.
            params:
                Required Argument.
                Specifies the parameter(s) of a teradataml analytic function. 
                The parameter(s) must be in dictionary. keys refers to the 
                argument names and values refers to argument values for corresponding
                arguments. 
                Notes:
                    * One can specify the argument value in a tuple to run HPT 
                      with different arguments.
                    * Model trainer function arguments "id_column", "input_columns",
                      and "target_columns" must be passed in fit() method.
                    * All required arguments of non-model trainer function must be
                      passed while RandomSearch object creation.
                Types: dict
            n_iter:
                Optional Argument.
                Specifies the number of iterations random search need to be performed.
                Note:
                    * n_iter must be less than the size of parameter populations.
                Default Value: 10
                Types: int
        RETURNS:
            None
        RAISES:
            TeradataMlException, TypeError, ValueError
        EXAMPLES:
            >>> # Example 1: Model trainer function. Performing hyperparameter-tuning
            >>> #            on SVM model trainer function using random search algorithm.
            >>> # Load the example data.
            >>> load_example_data("teradataml", ["cal_housing_ex_raw"])
            >>> # Create teradataml DataFrame objects.
            >>> data_input = DataFrame.from_table("cal_housing_ex_raw")
            >>> # Scale "target_columns" with respect to 'STD' value of the column.
9/14/2025, 16:50
Page 1,404 of 1,675
            >>> fit_obj = ScaleFit(data=data_input,
                                   target_columns=['MedInc', 'HouseAge', 'AveRooms',
                                                   'AveBedrms', 'Population', 'AveOccup',
                                                   'Latitude', 'Longitude'],
                                   scale_method="STD")
            >>> # Transform the data.
            >>> transform_obj = ScaleTransform(data=data_input,
                                               object=fit_obj.output,
                                               accumulate=["id", "MedHouseVal"])
            >>> # Define parameter space for model training.
            >>> # Note: These parameters create 6 models based on batch_size and iter_max.
            >>> params = {"input_columns":['MedInc', 'HouseAge', 'AveRooms',
                                           'AveBedrms', 'Population', 'AveOccup',
                                           'Latitude', 'Longitude'],
                           "response_column":"MedHouseVal",
                           "model_type":"regression",
                           "batch_size":(11, 50, 75),
                           "iter_max":(100, 301),
                           "lambda1":0.1,
                           "alpha":0.5,
                           "iter_num_no_change":60,
                           "tolerance":0.01,
                           "intercept":False,
                           "learning_rate":"INVTIME",
                           "initial_data":0.5,
                           "decay_rate":0.5,
                           "momentum":0.6,
                           "nesterov":True,
                           "local_sgd_iterations":1}
            >>> # Import trainer function and optimizer.
            >>> from teradataml import SVM, RandomSearch
            >>> # Initialize the random search optimizer with model trainer
            >>> # function and parameter space required for model training.
            >>> rs_obj = RandomSearch(func=SVM, params=params, n_iter=3)
            >>> # Perform model optimization for SVM function.
            >>> # Evaluation and prediction arguments are passed along with
            >>> # training dataframe.
            >>> rs_obj.fit(data=transform_obj.result, evaluation_metric="R2",
                           id_column="id", verbose=1)
                completed: |████████████████████████████████████████████████████████████| 100% - 3/3
            >>> # View trained models.
            >>> rs_obj.models
                  MODEL_ID DATA_ID                                         PARAMETERS STATUS        R2
                0    SVM_2    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS -3.668091
                1    SVM_1    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS -3.668091
                2    SVM_0    DF_0  {'input_columns': ['MedInc', 'HouseAge', 'AveR...   PASS -3.668091
            >>> # View model evaluation stats.
            >>> rs_obj.model_stats
                  MODEL_ID       MAE       MSE  MSLE        MAPE  ...        ME        R2        EV  MPD  MGD
                0    SVM_2  2.354167  6.715689   0.0  120.054758  ...  3.801619 -3.668091  0.184238  NaN  NaN
                1    SVM_1  2.354167  6.715689   0.0  120.054758  ...  3.801619 -3.668091  0.184238  NaN  NaN
                2    SVM_0  2.354167  6.715689   0.0  120.054758  ...  3.801619 -3.668091  0.184238  NaN  NaN
                [3 rows x 13 columns]
            >>> # Performing prediction on sampled data using best trained model.
            >>> test_data = transform_obj.result.iloc[:5]
            >>> rs_pred = rs_obj.predict(newdata=test_data, id_column="id")
            >>> print("Prediction result: 
", rs_pred.result)
                Prediction result:
                     id  prediction
                0   686   -0.024033
                1  2018   -0.069738
                2  1754   -0.117881
                3   670   -0.021818
                4   244   -0.187346
            >>> # Perform evaluation using best model.
            >>> rs_obj.evaluate()
            ############ result Output ############
                    MAE       MSE  MSLE        MAPE         MPE      RMSE  RMSLE        ME        R2        EV  MPD  MGD
            0  2.354167  6.715689   0.0  120.054758  120.054758  2.591465    0.0  3.801619 -3.668091  0.184238  NaN  NaN
            >>> # Retrieve any trained model.
            >>> rs_obj.get_model("SVM_1")
                ############ output_data Output ############
9/14/2025, 16:50
Page 1,405 of 1,675
                   iterNum      loss       eta  bias
                0        3  2.012817  0.028868   0.0
                1        5  2.010455  0.022361   0.0
                2        6  2.009331  0.020412   0.0
                3        7  2.008276  0.018898   0.0
                4        9  2.006384  0.016667   0.0
                5       10  2.005518  0.015811   0.0
                6        8  2.007302  0.017678   0.0
                7        4  2.011636  0.025000   0.0
                8        2  2.014326  0.035355   0.0
                9        1  2.016398  0.050000   0.0
                ############ result Output ############
                                          predictor   estimate                                   value
                attribute
                -7                           Alpha    0.500000                              Elasticnet
                -3          Number of Observations   55.000000                                    None
                 5                      Population    0.000000                                    None
                 0                     (Intercept)    0.000000                                    None
                -17                   OneClass SVM         NaN                                   FALSE
                -16                         Kernel         NaN                                  LINEAR
                -1                   Loss Function         NaN  EPSILON_INSENSITIVE
                 7                        Latitude   -0.076648                                    None
                -9         Learning Rate (Initial)    0.050000                                    None
                -14                        Epsilon    0.100000                                    None
            >>> # View best data, model ID, score and parameters.
            >>> print("Best data ID: ", rs_obj.best_data_id)
                Best data ID:  DF_0
            >>> print("Best model ID: ", rs_obj.best_model_id)
                Best model ID:  SVM_2
            >>> print("Best model score: ", rs_obj.best_score_)
                Best model score:  -3.6680912444156455
            >>> print("Best model parameters: ", rs_obj.best_params_)
                Best model parameters:  {'input_columns': ['MedInc', 'HouseAge', 'AveRooms', 
                                        'AveBedrms', 'Population', 'AveOccup', 'Latitude', 'Longitude'], 
                                        'response_column': 'MedHouseVal', 'model_type': 'regression', 
                                        'batch_size': 50, 'iter_max': 301, 'lambda1': 0.1, 'alpha': 0.5, 
                                        'iter_num_no_change': 60, 'tolerance': 0.01, 'intercept': False, 
                                        'learning_rate': 'INVTIME', 'initial_data': 0.5, 'decay_rate': 0.5, 
                                        'momentum': 0.6, 'nesterov': True, 'local_sgd_iterations': 1, 
                                        'data': '"ALICE"."ml__select__1696595493985650"'}
            >>> # Update the default model.
            >>> rs_obj.set_model("SVM_1")
            >>> # Example 2: Non-Model trainer function. Performing random search
            >>> #            on AntiSelect model trainer function using random
            >>> #            search algorithm.
            >>> # Load the example dataset.
            >>> load_example_data("teradataml", "titanic")
            >>> # Create teradaraml dataframe.
            >>> titanic = DataFrame.from_table("titanic")
            >>> # Define the non-model trainer function parameter space.
            >>> # Include input data in parameter space for non-model trainer function.
            >>> # Note: These parameters creates two model hyperparameters.
            >>> params = {"data":titanic, "exclude":(['survived', 'age'],['age'],
                                                     ['survived', 'name', 'age'],
                                                     ['ticket'],['parch'],['sex','age'],
                                                     ['survived'], ['ticket','parch'],
                                                     ["ticket", "parch", "sex", "age"])}
            >>> # Import non-model trainer function and optimizer.
            >>> from teradataml import Antiselect, RandomSearch
            >>> # Initialize the random search optimizer with non-model trainer
            >>> # function and parameter space required for non-model training.
            >>> rs_obj = RandomSearch(func=Antiselect, params=params, n_iter=4)
            >>> # Perform execution of Antiselect function.
            >>> rs_obj.fit()
            >>> # Note: Since it is a non-model trainer function model ID, score
            >>> # and parameters are not applicable here.
            >>> # View trained models.
            >>> rs_obj.models
                       MODEL_ID                                         PARAMETERS STATUS
                0  ANTISELECT_1  {'data': '"titanic"', 'exclude': ['survived', ...   PASS
9/14/2025, 16:50
Page 1,406 of 1,675
                1  ANTISELECT_3  {'data': '"titanic"', 'exclude': ['ticket', 'p...   PASS
                2  ANTISELECT_2     {'data': '"titanic"', 'exclude': ['survived']}   PASS
                3  ANTISELECT_0   {'data': '"titanic"', 'exclude': ['sex', 'age']}   PASS
            >>> # Retrieve any trained model using "MODEL_ID".
            >>> rs_obj.get_model("ANTISELECT_0")
                ############ result Output ############
                   passenger  survived  pclass                                                name  sibsp  parch             ti
                0        162         1       2  Watt, Mrs. James (Elizabeth "Bessie" Inglis Milne)      0      0         C.A. 3
                1        591         0       3                                Rintamaki, Mr. Matti      0      0  STON/O 2. 310
                2        387         0       3                     Goodwin, Master. Sidney Leonard      5      2            CA 
                3        469         0       3                                  Scanlan, Mr. James      0      0              3
                4        326         1       1                            Young, Miss. Marie Grice      0      0           PC 1
                5        265         0       3                                  Henry, Miss. Delia      0      0             38
                6        530         0       2                         Hocking, Mr. Richard George      2      1              2
                7        244         0       3                       Maenpaa, Mr. Matti Alexanteri      0      0  STON/O 2. 310
                8         61         0       3                               Sirayanian, Mr. Orsen      0      0               
                9        122         0       3                          Moore, Mr. Leonard Charles      0      0          A4. 5
evaluate
teradataml.hyperparameter_tuner.optimizer.RandomSearch.evaluate = evaluate(self, **kwargs)
DESCRIPTION:
    Function uses trained models from SQLE, VAL and UAF features for 
    evaluations. evaluations are made using the default trained model.
    Notes: 
        * Evaluation supported for evaluatable model-trainer functions.
        * Best model is set as default model by default.
        * Default model can be changed using "set_model()" method.
PARAMETERS:
    kwargs:
        Optional Argument.
        Specifies the keyword arguments. Accepts additional arguments 
        required for the teradataml analytic function evaluations. 
        While "kwargs" is empty then internal sampled test dataset
        and arguments used for evaluation. Otherwise, 
        All arguments required with validation data need to be passed
        for evaluation.
RETURNS:
    Output teradataml DataFrames can be accessed using attribute
    references, such as HPTEvaluateObj.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Perform evaluation using best model.
    >>> optimizer_obj.evaluate(newdata=test_data, **eval_params)
        ############ result Output ############
                MAE       MSE  MSLE        MAPE         MPE      RMSE  RMSLE        ME       R2       EV  MPD  MGD
        0  2.616772  8.814968   0.0  101.876866  101.876866  2.969001    0.0  5.342344 -4.14622 -0.14862  NaN  NaN
ﬁt
teradataml.hyperparameter_tuner.optimizer.RandomSearch.ﬁt = ﬁt(self, data=None, evaluation_metric=None, early_stop=None, frac=0.8, run_parallel=True, wait=True,
verbose=0, stratify_column=None, sample_id_column=None, sample_seed=None, max_time=None, **kwargs)
DESCRIPTION:
    Function to perform hyperparameter tuning using RandomSearch algorithm.
    Notes:
        * In the Model trainer function, the best parameters are 
          selected based on training results.
        * In the Non model trainer function, First execution parameter
          set is selected as the best parameters.
PARAMETERS:
    data:
        Optional Argument.
        Specifies the input teradataml DataFrame for model trainer function.
        Notes:
            * DataFrame need not to be passed in fit() methods, when "data" is 
              passed as a model hyperparameters ("params"). 
            * "data" is a required argument for model trainer functions.
            * "data" is ignored for non-model trainer functions.
            * "data" can be contain single DataFrame or multiple DataFrame.
            * One can pass multiple dataframes to "data". Hyperparameter 
              tuning is performed on all the dataframes for every model 
9/14/2025, 16:50
Page 1,407 of 1,675
              parameter.
            * "data" can be either a dictionary OR a tuple OR a dataframe.
                * If it is a dictionary then Key represents the label for 
                  dataframe and Value represents the dataframe.
                * If it is a tuple then teradataml converts it to dictionary
                  by generating the labels internally.
                * If it is a dataframe then teradataml label it as "DF_0".
        Types: teradataml DataFrame, dictionary, tuples
    evaluation_metric:
        Optional Argument.
        Specifies the evaluation metrics to considered for model 
        evaluation.
        Notes:
            * evaluation_metric applicable for model trainer functions.
            * Best model is not selected when evaluation returns 
              non-finite values.
        Permitted Values:
            * Classification: Accuracy, Micro-Precision, Micro-Recall,
                              Micro-F1, Macro-Precision, Macro-Recall,
                              Macro-F1, Weighted-Precision, 
                              Weighted-Recall,
                              Weighted-F1.
            * Regression: MAE, MSE, MSLE, MAPE, MPE, RMSE, RMSLE, ME, 
                          R2, EV, MPD, MGD
        Default Value:
            * Classification: Accuracy
            * Regression: MAE
        Types: str
    early_stop:
        Optional Argument.
        Specifies the early stop mechanism value for model trainer 
        functions. Hyperparameter tuning ends model training when 
        the training model evaluation metric attains "early_stop" value.
        Note:
            * Early stopping supports only when evaluation returns 
              finite value.
        Types: int or float
    frac:
        Optional Argument.
        Specifies the split percentage of rows to be sampled for training 
        and testing dataset. "frac" argument value must range between (0, 1).
        Notes: 
            * This "frac" argument is not supported for non-model trainer 
              function.
            * The "frac" value is considered as train split percentage and 
              The remaining percentage is taken into account for test splitting.
        Default Value: 0.8
        Types: float
    run_parallel:
        Optional Argument.
        Specifies the parallel execution functionality of hyperparameter 
        tuning. When "run_parallel" set to true, model functions are 
        executed concurrently. Otherwise, model functions are executed 
        sequentially.
        Default Value: True
        Types: bool
    wait:
        Optional Argument.
        Specifies whether to wait for the completion of execution 
        of hyperparameter tuning or not. When set to False, hyperparameter 
        tuning is executed in the background and user can use "is_running()" 
        method to check the status. Otherwise it waits until the execution 
        is complete to return the control back to user.
        Default Value: True
        Type: bool
    verbose:
        Optional Argument.
        Specifies whether to log the model training information and display 
        the logs. When it is set to 1, progress bar alone logged in the 
        console. When it is set to 2, along with progress bar, execution 
        steps and execution time is logged in the console. When it is set 
        to 0, nothing is logged in the console. 
        Note:
            * verbose is not significant when "wait" is 'False'.
        Default Value: 0
        Type: bool
9/14/2025, 16:50
Page 1,408 of 1,675
    sample_seed:
        Optional Argument.
        Specifies the seed value that controls the shuffling applied
        to the data before applying the Train-Test split. Pass an int for 
        reproducible output across multiple function calls.
        Notes:
            * When the argument is not specified, different
              runs of the query generate different outputs.
            * It must be in the range [0, 2147483647]
            * Seed is supported for stratify column.
        Types: int
    stratify_column:
        Optional Argument.
        Specifies column name that contains the labels indicating
        which data needs to be stratified for TrainTest split. 
        Notes:
            * seed is supported for stratify column.
        Types: str
    sample_id_column:
        Optional Argument.
        Specifies the input data column name that has the
        unique identifier for each row in the input.
        Note:
            * Mandatory when "sample_seed" argument is present.
        Types: str
    max_time:
        Optional Argument.
        Specifies the maximum time for the completion of Hyperparameter tuning execution.
        Default Value: None
        Types: int or float
    kwargs:
        Optional Argument.
        Specifies the keyword arguments. Accepts additional arguments 
        required for the teradataml analytic function.
RETURNS:
    None
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> # Create an instance of the RandomSearch algorithm called "optimizer_obj" 
    >>> optimizer_obj = RandomSearch(func=SVM, params=params, n_iter=3)
    >>> eval_params = {"id_column": "id",
                       "accumulate": "MedHouseVal"}
    >>> # Example 1: Passing single DataFrame for model trainer function.
    >>> optimizer_obj.fit(data=train_df,
                          evaluation_metric="MAE",
                          early_stop=70.9,
                          **eval_params)
    >>> # Example 2: Passing multiple datasets as tuple of DataFrames for 
    >>> #            model trainer function.
    >>> optimizer_obj.fit(data=(train_df_1, train_df_2),
                          evaluation_metric="MAE",
                          early_stop=70.9,
                          **eval_params)  
    >>> # Example 3: Passing multiple datasets as dictionary of DataFrames 
    >>> #            for model trainer function.
    >>> optimizer_obj.fit(data={"Data-1":train_df_1, "Data-2":train_df_2},
                          evaluation_metric="MAE",
                          early_stop=70.9,
                          **eval_params)  
    >>> # Example 4: No data argument passed in fit() method for model trainer function.
    >>> #            Note: data argument must be passed while creating HPT object as 
    >>> #                  model hyperparameters.
    >>> # Define parameter space for model training with "data" argument.
    >>> params = {"data":(df1, df2),
                  "input_columns":['MedInc', 'HouseAge', 'AveRooms',
                                   'AveBedrms', 'Population', 'AveOccup',
                                   'Latitude', 'Longitude'],
                  "response_column":"MedHouseVal",
                  "model_type":"regression",
                  "batch_size":(11, 50, 75),
                  "iter_max":(100, 301),
9/14/2025, 16:50
Page 1,409 of 1,675
                  "intercept":False,
                  "learning_rate":"INVTIME",
                  "nesterov":True,
                  "local_sgd_iterations":1}
    >>> # Create "optimizer_obj" using RandomSearch algorithm and perform 
    >>> # fit() method without any "data" argument for model trainer function.
    >>> optimizer_obj.fit(evaluation_metric="MAE",
                          early_stop=70.9,
                          **eval_params) 
    >>> # Example 5: Do not pass data argument in fit() method for 
    >>> #            non-model trainer function.
    >>> #            Note: data argument must be passed while creating HPT  
    >>> #                  object as model hyperparameters.
    >>> optimizer_obj.fit() 
    >>> # Example 6: Passing "verbose" argument value '1' in fit() method to 
    >>> #            display model log.
    >>> optimizer_obj.fit(data=train_df, evaluation_metric="R2",
                          verbose=1, **eval_params)
        completed: |████████████████████████████████████████████████████████████| 100% - 6/6
    >>> # Example 7: max_time argument is passed in fit() method.
    >>> # Model training parameters
    >>> model_params = {"input_columns":['sepal_length', 'sepal_width', 'petal_length', 'petal_width'],
    ...                 "response_column" : 'species',
    ...                 "max_depth":(5,10,15),
    ...                 "lambda1" : (1000.0,0.001),
    ...                 "model_type" :"Classification",
    ...                 "seed":32,
    ...                 "shrinkage_factor":0.1,
    ...                 "iter_num":(5, 50)}
    >>>
    >>> eval_params = {"id_column": "id",
    ...                "accumulate": "species",
    ...                "model_type":'Classification',
    ...                "object_order_column":['task_index', 'tree_num', 'iter','class_num', 'tree_order']
    ...               }
    >>>
    >>> # Import model trainer and optimizer
    >>> from teradataml import XGBoost, RandomSearch
    >>>
    >>> # Initialize the RandomSearch optimizer with model trainer
    >>> # function and parameter space required for model training.
    >>> rs_obj = RandomSearch(func=XGBoost, params=model_params, n_iter=5)
    >>>
    >>> # fit() method with max_time argument(in seconds) for model trainer function.
    >>> rs_obj.fit(data=data, max_time=30, verbose=2, **eval_params)
        Model_id:XGBOOST_3 - Run time:28.292s - Status:PASS - ACCURACY:0.8                 
        Model_id:XGBOOST_0 - Run time:28.291s - Status:PASS - ACCURACY:0.867               
        Model_id:XGBOOST_2 - Run time:28.289s - Status:PASS - ACCURACY:0.867               
        Model_id:XGBOOST_1 - Run time:28.291s - Status:PASS - ACCURACY:0.867               
        Computing: ｜⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫿⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾⫾｜ 80% - 4/5
    >>>
    >>> # status 'SKIP' for the models which are not completed within the max_time.
    >>> rs_obj.models
            MODEL_ID    DATA_ID                                        PARAMETERS       STATUS  ACCURACY
        0       XGBOOST_3       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       PASS    0.800000
        1       XGBOOST_4       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       SKIP    NaN
        2       XGBOOST_0       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       PASS    0.866667
        3       XGBOOST_2       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       PASS    0.866667
        4       XGBOOST_1       DF_0    {'input_columns': ['sepal_length', 'sepal_widt...       PASS    0.866667
get_error_log
teradataml.hyperparameter_tuner.optimizer.RandomSearch.get_error_log = get_error_log(self, model_id)
DESCRIPTION:
    Function to get the error logs of a failed model training in the fit method.
PARAMETERS:
    model_id:
        Required Argument.
        Specifies the unique identifier for model.
        Note:
             * Only failed model training error log is returned.
        Types: str
RETURNS:
    string
RAISES:
    TypeError, ValueError
9/14/2025, 16:50
Page 1,410 of 1,675
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the error log.
    >>> optimizer_obj.get_error_log("SVM_2")
        "[Teradata][teradataml](TDML_2082) Value of 'iter_max' must be greater 
        than or equal to 1 and less than or equal to 10000000."
get_input_data
teradataml.hyperparameter_tuner.optimizer.RandomSearch.get_input_data = get_input_data(self, data_id)
DESCRIPTION:
    Function to get the input data used by model trainer functions.
    Unique identifiers (data_id) is used to get the training data.
    In case of unlabeled data such as single dataframe or tuple of 
    dataframe, default unique identifiers are assigned. Hence, unlabeled
    training data is retrieved using default unique identifiers.
    Notes:
        * Function only returns input data for model trainer functions.
        * Train and Test sampled data are returned for supervised 
          model trainer function (evaluatable functions).
        * Train data is returned for unsupervised-model trainer function 
          (non-evaluatable functions).
PARAMETERS:
    data_id:
        Required Argument.
        Specifies the unique data identifier used for model training.
        Types: str
RETURNS:
    teradataml DataFrame
RAISES:
    ValueError
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the training data.
    >>> optimizer_obj.get_input_data(data_id="DF_1")
        [{'data':       id  MedHouseVal    MedInc  HouseAge  AveRooms  AveBedrms  Population  AveOccup  Latitude  Longitude
        0  19789        0.660 -1.154291 -0.668250  0.862203   7.021803   -1.389101 -1.106515  2.367716  -1.710719
        1  17768        1.601 -0.447350 -0.162481 -0.431952  -0.156872    2.436223  2.172854  0.755780  -1.016640
        2  19722        0.675 -0.076848  1.439120  1.805547   1.944759   -1.186169  0.326739  1.459894  -0.974996
        3  18022        3.719  1.029892  0.343287  0.635952  -0.480133   -0.914869 -0.160824  0.711496  -1.067540
        4  15749        3.500 -0.182247  1.776299 -0.364226   0.035715   -0.257239 -0.970166  0.941772  -1.294272
        5  11246        2.028 -0.294581 -0.583955 -0.265916  -0.270654    0.182266 -0.703494 -0.807444   0.764827
        6  16736        3.152  0.943735  1.439120 -0.747066  -1.036053   -1.071138 -0.678411  0.906345  -1.234118
        7  12242        0.775 -1.076758 -0.752545 -0.424517   0.460470    0.742228 -0.597809 -0.838443   1.241428
        8  14365        2.442 -0.704218  1.017646 -0.428965  -0.367301   -1.014707 -1.333045 -1.294568   1.121121
        9  18760        1.283  0.019018 -1.258313  0.754993   0.013994    0.094365  0.222254  2.195008  -1.201728}, 
        {'newdata':       id  MedHouseVal    MedInc  HouseAge  AveRooms  AveBedrms  Population  AveOccup  Latitude  Longitude
        0  16102        2.841  0.206284  1.270530 -0.248620  -0.224210   -0.059733 -0.242386  0.937344  -1.317408
        1  15994        3.586  0.306050  1.439120  0.255448  -0.334613   -0.160657 -0.426510  0.937344  -1.303526
        2  15391        2.541  0.423107 -1.595492  0.951807  -0.061005    1.955480  0.517572 -1.055434   1.236801
        3  18799        0.520 -0.677565 -0.415366  0.548756   1.254406   -0.883398 -0.534060  2.358859  -1.035149
        4  19172        1.964  0.247152 -0.162481  0.428766  -0.427459   -0.175849 -0.451380  1.238475  -1.396070
        5  18164        3.674  0.295345 -1.258313 -1.078181   0.175885    0.045531 -1.298667  0.760208  -1.099930
        6  13312        1.598  0.484475 -1.342608  0.767557  -0.229585    0.113899  0.361520 -0.692306   0.949915
        7  12342        1.590 -0.520029 -0.246776  0.973345   1.407755    2.325532 -0.406887 -0.798587   1.445024}]
get_model
teradataml.hyperparameter_tuner.optimizer.RandomSearch.get_model = get_model(self, model_id)
DESCRIPTION:
    Function to get the model.
PARAMETERS:
    model_id:
        Required Argument.
        Specifies the unique identifier for model.
        Notes:
             * Trained model results returned for model trainer functions.
             * Executed function results returned for non-model trainer
               functions.
        Types: str
RETURNS:
    Object of teradataml analytic functions.
    Note:
        * Attribute references remains same as that of the function 
9/14/2025, 16:50
Page 1,411 of 1,675
          attributes.
RAISES:
    TeradataMlException, ValueError
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the trained model.
    >>> optimizer_obj.get_model(model_id="SVM_1")
    ############ output_data Output ############
       iterNum      loss       eta  bias
    0        3  2.265289  0.028868   0.0
    1        5  2.254413  0.022361   0.0
    2        6  2.249260  0.020412   0.0
    3        7  2.244463  0.018898   0.0
    4        9  2.235800  0.016667   0.0
    5       10  2.231866  0.015811   0.0
    6        8  2.239989  0.017678   0.0
    7        4  2.259956  0.025000   0.0
    8        2  2.271862  0.035355   0.0
    9        1  2.280970  0.050000   0.0
    ############ result Output ############
                              predictor  estimate                                   value
    attribute
    -7                           Alpha    0.50000                              Elasticnet
    -3          Number of Observations   31.00000                                    None
     5                      Population   -0.32384                                    None
     0                     (Intercept)    0.00000                                    None
    -17                   OneClass SVM        NaN                                   FALSE
    -16                         Kernel        NaN                                  LINEAR
    -1                   Loss Function        NaN  EPSILON_INSENSITIVE
     7                        Latitude    0.00000                                    None
    -9         Learning Rate (Initial)    0.05000                                    None
    -14                        Epsilon    0.10000                                    None
get_parameter_grid
teradataml.hyperparameter_tuner.optimizer.RandomSearch.get_parameter_grid = get_parameter_grid(self)
DESCRIPTION:
    Returns the value of the attribute _parameter_grid.
RETURNS:
    dict
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve parameter grid.
    >>> optimizer_obj.get_parameter_grid()
        [{'param': {'input_columns': ['MedInc', 'HouseAge', 'AveRooms', 'AveBedrms', 
                                     'Population', 'AveOccup', 'Latitude', 'Longitude'], 
                    'response_column': 'MedHouseVal', 'model_type': 'regression', 
                    'batch_size': 75, 'iter_max': 100, 'lambda1': 0.1, 'alpha': 0.5, 
                    'iter_num_no_change': 60, 'tolerance': 0.01, 'intercept': False,
                    'learning_rate': 'INVTIME', 'initial_data': 0.5, 'decay_rate': 0.5, 
                    'momentum': 0.6, 'nesterov': True, 'local_sgd_iterations': 1, 
                    'data': '"ALICE"."ml__select__1696593660430612"'}, 
         'data_id': 'DF_0'}, 
         {'param': {'input_columns': ['MedInc', 'HouseAge', 'AveRooms', 'AveBedrms', 
                                     'Population', 'AveOccup', 'Latitude', 'Longitude'], 
                    'response_column': 'MedHouseVal', 'model_type': 'regression', 
                    'batch_size': 75, 'iter_max': 100, 'lambda1': 0.1, 'alpha': 0.5, 
                    'iter_num_no_change': 60, 'tolerance': 0.01, 'intercept': False, 
                    'learning_rate': 'INVTIME', 'initial_data': 0.5, 'decay_rate': 0.5, 
                    'momentum': 0.6, 'nesterov': True, 'local_sgd_iterations': 1, 
                    'data': '"ALICE"."ml__select__1696593660430612"'}, 
         'data_id': 'DF_1'}]
is_running
teradataml.hyperparameter_tuner.optimizer.RandomSearch.is_running = is_running(self)
DESCRIPTION:
    Check whether hyperparameter tuning is completed or not. Function 
    returns True when execution is in progress. Otherwise it returns False.
PARAMETERS:
    None
9/14/2025, 16:50
Page 1,412 of 1,675
RAISES:
    None
RETURNS:
    bool
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the model execution status.
    >>> optimizer_obj.is_running() 
        False
predict
teradataml.hyperparameter_tuner.optimizer.RandomSearch.predict = predict(self, **kwargs)
DESCRIPTION:
    Function uses model training function generated models from SQLE, 
    VAL and UAF features for predictions. Predictions are made using 
    the best trained model. Predict function is not supported for
    non-model trainer function.
PARAMETERS:
    kwargs:
        Optional Argument.
        Specifies the keyword arguments. Accepts all merge model 
        predict feature arguments required for the teradataml 
        analytic function predictions.
RETURNS:
    Output teradataml DataFrames can be accessed using attribute
    references, such as HPTObj.<attribute_name>.
    Output teradataml DataFrame attribute name is:
        result
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Perform prediction using "optimizer_obj".
    >>> optimizer_obj.predict(newdata=test_data, **eval_params)
             id  prediction  MedHouseVal
        0   686    0.202843        1.578
        1  2018    0.149868        0.578
        2  1754    0.211870        1.651
        3   670    0.192414        1.922
        4   244    0.247545        1.117
set_model
teradataml.hyperparameter_tuner.optimizer.RandomSearch.set_model = set_model(self, model_id)
DESCRIPTION:
    Function to set the model to use for Prediction.
PARAMETERS:
    model_id:
        Required Argument.
        Specifies the unique identifier for model.
        Note:
             * Not significant for non-model trainer functions.
        Types: str
RETURNS:
    None
RAISES:
    TeradataMlException, ValueError
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Set the default trained model.
    >>> optimizer_obj.set_model(model_id="SVM_1")
Properties
best_data_id
teradataml.hyperparameter_tuner.optimizer.RandomSearch.best_data_id
DESCRIPTION:
9/14/2025, 16:50
Page 1,413 of 1,675
    Returns the "data_id" of a sampled data used for training the best model.
    Note:
        "best_data_id" is not supported for non-model trainer functions.
RETURNS:
    String representing the best "data_id"
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best data id.
    >>> optimizer_obj.best_data_id
        DF_0
best_model
teradataml.hyperparameter_tuner.optimizer.RandomSearch.best_model
DESCRIPTION:
    Returns the best trained model obtained from hyperparameter tuning.
    Note:
        "best_model" is not supported for non-model trainer functions.
RETURNS:
    object of trained model.
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best model.
    >>> optimizer_obj.best_model
        ############ output_data Output ############
           iterNum      loss       eta  bias
        0        3  2.060386  0.028868   0.0
        1        5  2.055509  0.022361   0.0
        2        6  2.051982  0.020412   0.0
        3        7  2.048387  0.018898   0.0
        4        9  2.041521  0.016667   0.0
        5       10  2.038314  0.015811   0.0
        6        8  2.044882  0.017678   0.0
        7        4  2.058757  0.025000   0.0
        8        2  2.065932  0.035355   0.0
        9        1  1.780877  0.050000   0.0
        ############ result Output ############
                                 predictor    estimate       value
        attribute
         7                        Latitude    0.155095        None
        -9         Learning Rate (Initial)    0.050000        None
        -17                   OneClass SVM         NaN       FALSE
        -14                        Epsilon    0.100000        None
         5                      Population    0.000000        None
        -12                       Nesterov         NaN        TRUE
        -5                             BIC   73.297397        None
        -7                           Alpha    0.500000  Elasticnet
        -3          Number of Observations   55.000000        None
         0                     (Intercept)    0.000000        None
best_model_id
teradataml.hyperparameter_tuner.optimizer.RandomSearch.best_model_id
DESCRIPTION:
    Returns the model id of the model with best score.
    Note:
        "best_model_id" is not supported for non-model trainer functions.
RETURNS:
    String representing the best model id.
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best model id.
    >>> optimizer_obj.best_model_id
        'SVM_2'
best_params_
teradataml.hyperparameter_tuner.optimizer.RandomSearch.best_params_
9/14/2025, 16:50
Page 1,414 of 1,675
DESCRIPTION:
    Returns the parameters used for the model with best score.
    Note:
        "best_params_" is not supported for non-model trainer functions.
RETURNS:
    dict
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best parameters.
    >>> optimizer_obj.best_params_
        {'input_columns': ['MedInc', 'HouseAge', 'AveRooms', 
                          'AveBedrms', 'Population', 'AveOccup', 'Latitude', 'Longitude'], 
         'response_column': 'MedHouseVal', 'model_type': 'regression', 
         'batch_size': 50, 'iter_max': 301, 'lambda1': 0.1, 'alpha': 0.5, 
         'iter_num_no_change': 60, 'tolerance': 0.01, 'intercept': False, 
         'learning_rate': 'INVTIME', 'initial_data': 0.5, 'decay_rate': 0.5, 
         'momentum': 0.6, 'nesterov': True, 'local_sgd_iterations': 1, 
         'data': '"ALICE"."ml__select__1696595493985650"'}
best_sampled_data_
teradataml.hyperparameter_tuner.optimizer.RandomSearch.best_sampled_data_
DESCRIPTION:
    Returns the best sampled data used for training the best model.
    Note:
        "best_sampled_data_" is not supported for non-model trainer functions.
RETURNS:
    list of DataFrames.
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best sampled data.
    >>> optimizer_obj.best_sampled_data_
        [{'data':       id  MedHouseVal    MedInc  HouseAge  AveRooms  AveBedrms  Population  AveOccup  Latitude  Longitude
        0   5233        0.955 -0.895906  0.680467 -0.387272  -0.202806   -0.125930  2.130214 -0.754303   0.653775
        1  10661        3.839  2.724825 -1.258313  0.876263  -1.142947   -0.751004 -0.187396 -0.878298   0.852744
        2  10966        1.896  0.057849  0.343287 -0.141762  -0.664624   -0.095545  0.588981 -0.829586   0.815727
        3   3687        1.741 -0.383816 -1.679787 -0.849458   0.108000    0.718354  1.083500 -0.630308   0.593621
        4   7114        2.187 -0.245392  0.258993  0.225092  -0.205781   -0.171508 -0.035650 -0.763160   0.755573
        5   5300        3.500 -0.955800 -1.005429 -1.548811  -0.130818    2.630473 -0.601956 -0.696734   0.556604
        6    686        1.578 -0.152084 -0.078186 -0.625426  -0.513581   -0.685892 -0.533101  0.906345  -1.141575
        7   9454        0.603 -1.109609 -0.499660  0.355748   0.379188   -0.364674 -0.356799  1.827451  -1.655193
        8   5202        1.000 -0.307539  1.101940 -0.379623  -0.570271   -0.141123  0.595366 -0.754303   0.635266
        9   5769        2.568 -0.413546  0.343287 -0.922324  -0.028824    1.165456  0.031374 -0.656879   0.626012}, 
        {'newdata':      id  MedHouseVal    MedInc  HouseAge  AveRooms  AveBedrms  Population  AveOccup  Latitude  Longitude
        0  1754        1.651 -0.026315  0.596172  0.454207  -0.027273    0.068320 -0.082765  1.017055  -1.234118
        1  3593        2.676  1.241775  0.090403  1.024283  -0.367626   -0.045626  0.252048 -0.621452   0.542722
        2  7581        1.334 -0.714880 -1.258313 -0.604140  -0.259612    3.058041  0.857406 -0.776445   0.658402
        3  8783        2.500 -0.170156  0.596172  0.163717   0.398242   -0.668529 -0.728130 -0.820729   0.621385
        4  5611        1.587 -0.712366 -0.415366 -1.275716   0.012960    0.860515  0.764870 -0.820729   0.639893
        5   244        1.117 -0.605796  1.101940 -0.160367   0.426668    1.022209  1.041018  0.946201  -1.187846}]
best_score_
teradataml.hyperparameter_tuner.optimizer.RandomSearch.best_score_
DESCRIPTION:
    Returns the best score of the model out of all generated models.
    Note:
        "best_score_" is not supported for non-model trainer functions.
RETURNS:
    String representing the best score.
EXAMPLES:
    >>> # Create an instance of the search algorithm called "optimizer_obj" 
    >>> # by referring "__init__()" method.
    >>> # Perform "fit()" method on the optimizer_obj to populate model records.
    >>> # Retrieve the best score.
    >>> optimizer_obj.best_score_
        2.060386
model_stats
teradataml.hyperparameter_tuner.optimizer.RandomSearch.model_stats
DESCRIPTION:
    Returns the model statistics of the model with best score.