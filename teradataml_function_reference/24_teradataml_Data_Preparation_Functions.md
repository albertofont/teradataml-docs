# teradataml Data Preparation Functions

Data Preparation Aggregate Functions
avg
avg
Functions
avg(value_expression)
DESCRIPTION:
    Function returns the arithmetic average of all values in value_expression.
PARAMETERS:
    value_expression:
        Required Argument.
        Specifies a ColumnExpression of a numeric column for which the average
        is to be computed.
        Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        Note:
            Nulls are not included in the result computation.
NOTE:
    Function accepts positional arguments only.
ALTERNATE NAMES:
    1. ave
    2. average
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the average value for the "gpa" column.
        # Import func from sqlalchemy to execute avg function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> avg_func_ = func.avg(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, avg_gpa_=avg_func_)
        >>> print(df)
           avg_gpa_
        0   3.54175
        >>>
        # "average" can be used as an alternative function name.
        >>> average_func_ = func.average(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, average_gpa_ = average_func_)
        >>> print(df)
           average_gpa_
        0       3.54175
        >>>
        # Example 2: Calculate the average "gpa" for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(average_gpa_=func.ave(admissions_train.gpa.expression))
          programming  average_gpa_
        0    Advanced      3.615625
        1      Novice      3.294545
        2    Beginner      3.660000
        >>>
    corr
    corr
    Functions
    corr(value_expression1, value_expression2)
    DESCRIPTION:
        Function returns the Sample Pearson product moment correlation coefficient
        of its arguments for all non-null data point pairs.
        The Sample Pearson product moment correlation coefficient is a measure of
        the linear association between variables. The boundary on the computed
        coefficient ranges from -1.00 to +1.00.
        Note that high correlation does not imply a causal relationship between
        the variables.
        The coefficient of correlation between two variables has the following four extreme values:
            1. -1.00 : Association between the variables is perfectly linear, but inverse.
                       As the value for 'value_expression1' varies, the value for 
                       'value_expression2' varies identically in the opposite direction.
            2. 0     : Association between the variables does not exist and they are considered 
                       to be uncorrelated.
            3. +1.00 : Association between the variables is perfectly linear.
                       As the value for 'value_expression1' varies, the value for 
                       'value_expression2' varies identically in the same direction.
            4. NULL  : Association between the variables cannot be measured because there
                       are no non-null data point pairs in the data used for the computation.
    PARAMETERS:
        value_expression1:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric literal
            to be correlated with value_expression2.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        value_expression2:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric literal
            to be correlated with value_expression1.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the correlation between "gpa" and "admitted" columns.
        # Import func from sqlalchemy to execute corr function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> corr_func_ = func.corr(admissions_train.gpa.expression, admissions_train.admitted.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, corr_gpa_admitted_=corr_func_)
        >>> print(df)
           corr_gpa_admitted_
        0           -0.022265
        >>>
        # Example 2: Calculate the correlation between "gpa" and "admitted" columns
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(corr_gpa_admitted_=corr_func_)
          programming  corr_gpa_admitted_
        0    Beginner           -0.417565
        1    Advanced            0.487737
        2      Novice           -0.114656
        >>>
    count
    count
    Functions
    count(value_expression)
    DESCRIPTION:
        Function returns the total number of qualified rows in value_expression.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a column for which the number of values
            is to be counted.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Get the total number of students.
        # Import func from sqlalchemy to execute count function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> count_func_ = func.count(admissions_train.id.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, count_id_=count_func_)
        >>> print(df)
          count_id_
        0         40
        >>>
        # Example 2: Get the total number of students for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(count_id_=count_func_)
          programming count_id_
        0    Beginner        13
        1    Advanced        16
        2      Novice        11
        >>>
    covar_pop
    covar_pop
    Functions
    covar_pop(value_expression1, value_expression2)
    DESCRIPTION:
        Function returns the population covariance of its arguments for all
        non-null data point pairs.
        Covariance measures whether or not two random variables vary in the
        same way. It is the average of the products of deviations for each
        non-null data point pair.
        Note that high covariance does not imply a causal relationship between
        the variables.
        When there are no non-null data point pairs in the data used for the 
        computation, the function returns NULL.
    PARAMETERS:
        value_expression1:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric literal
            to be paired with value_expression2 to determine their population covariance.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        value_expression2:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric literal
            to be paired with value_expression1 to determine their population covariance.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the population covariance between "gpa" and "admitted" columns.
        # Import func from sqlalchemy to execute covar_pop function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> covar_pop_func_ = func.covar_pop(admissions_train.gpa.expression, admissions_train.admitted.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, covar_pop_gpa_admitted_=covar_pop_func_)
        >>> print(df)
           covar_pop_gpa_admitted_
        0                -0.005388
        >>>
        # Example 2: Calculate the population covariance between "gpa" and "admitted" columns
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(covar_pop_gpa_admitted_=covar_pop_func_)
          programming  covar_pop_gpa_admitted_
        0    Beginner                -0.069231
        1    Advanced                 0.091055
        2      Novice                -0.031488
        >>>
    covar_samp
    covar_samp
    Functions
    covar_samp(value_expression1, value_expression2)
    DESCRIPTION:
        Function returns the sample covariance of its arguments for all
        non-null data point pairs.
        Covariance measures whether or not two random variables vary in the
        same way. It is the average of the products of deviations for each
        non-null data point pair.
        Note that high covariance does not imply a causal relationship between
        the variables.
        When there are no non-null data point pairs in the data used for the 
        computation, the function returns NULL.
    PARAMETERS:
        value_expression1:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric literal
            to be paired with value_expression2 to determine their sample covariance.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        value_expression2:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric literal
            to be paired with value_expression1 to determine their sample covariance.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the sample covariance between "gpa" and "admitted" column.
        # Import func from sqlalchemy to execute covar_samp function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> covar_samp_func_ = func.covar_samp(admissions_train.gpa.expression, admissions_train.admitted.expression)
        >>> 
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, covar_samp_gpa_admitted_=covar_samp_func_)
        >>> print(df)
           covar_samp_gpa_admitted_
        0                 -0.005526
        >>>
        # Example 2: Calculate sample covariance between "gpa" and "admitted" columns
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(covar_samp_gpa_admitted_=covar_samp_func_)
          programming  covar_samp_gpa_admitted_
        0    Beginner                 -0.075000
        1    Advanced                  0.097125
        2      Novice                 -0.034636
        >>>
    kurtosis
    kurtosis
    Functions
    kurtosis(value_expression)
    DESCRIPTION:
        Function returns the kurtosis of the distribution of value_expression.
        Kurtosis is the fourth moment of the distribution of the standardized (z) values.
        It is a measure of the outlier (rare, extreme observation) character of the distribution as
        compared with the normal (or Gaussian) distribution.
            * The normal distribution has a kurtosis of 0.
            * Positive kurtosis indicates that the distribution is more outlier-prone than the
              normal distribution.
            * Negative kurtosis indicates that the distribution is less outlier-prone than the
              normal distribution.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column for which the kurtosis of 
            the distribution of its values is to be computed.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. Null values are not included in the result computation.
                2. Following conditions will produce null result:
                    a. Fewer than three non-null data points in the data used for the computation.
                    b. Standard deviation for a column is equal to 0.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the kurtosis value for the "gpa" column.
        # Import func from sqlalchemy to execute kurtosis function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> kurtosis_func_ = func.kurtosis(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, kurtosis_gpa_=kurtosis_func_)
        >>> print(df)
           kurtosis_gpa_
        0       4.052659
        >>>
        # Example 2: Calculate the kurtosis "gpa" for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(kurtosis_gpa_=func.kurtosis(admissions_train.gpa.expression))
          programming  kurtosis_gpa_
        0    Beginner       5.439392
        1    Advanced       8.480554
        2      Novice       1.420745
        >>>
    max
    max
    Functions
    max(value_expression)
    DESCRIPTION:
        Function returns a column value that is the maximum value for value_expression.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a column for which the maximum value 
            is to be computed.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the maximum value for the "gpa" column.
        # Import func from sqlalchemy to execute max function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> max_func_ = func.max(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, max_gpa_=max_func_)
        >>> print(df)
           max_gpa_
        0       4.0
        >>>
        # Example 2: Calculate the maximum "gpa" for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(max_gpa_=func.max(admissions_train.gpa.expression))
          programming  max_gpa_
        0    Beginner       4.0
        1    Advanced       4.0
        2      Novice       4.0
        >>>
    min
    min
    Functions
    min(value_expression)
    DESCRIPTION:
        Function returns a column value that is the minimum value for value_expression.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a column for which the minimum value 
            is to be computed.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the minimum value for the "gpa" column.
        # Import func from sqlalchemy to execute min function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> min_func_ = func.min(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, min_gpa_=min_func_)
        >>> print(df)
           min_gpa_
        0      1.87
        >>>
        # Example 2: Calculate the minimum "gpa" for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(min_gpa_=func.min(admissions_train.gpa.expression))
          programming  min_gpa_
        0    Beginner      2.65
        1    Advanced      1.98
        2      Novice      1.87
        >>>
    regr_avgx
    regr_avgx
    Functions
    regr_avgx(dependent_variable_expression, independent_variable_expression)
    DESCRIPTION:
        Function returns the mean of the independent_variable_expression for all
        non-null data pairs of the dependent and independent variable arguments.
        When there are fewer than two non-null data point pairs in the data used
        for the computation, the function returns NULL.
    PARAMETERS:
        dependent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing a
            dependent variable for the regression.
            A dependent variable is something that is measured in response to a treatment.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        independent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing an
            independent variable for the regression.
            An independent variable is a treatment: something that is varied under 
            your control to test the behavior of another variable.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the mean of the "gpa" column (independent variable) with
        #            respect to values in "admitted" column (dependent variable).
        # Import func from sqlalchemy to execute regr_avgx function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> regr_avgx_func_ = func.regr_avgx(admissions_train.admitted.expression, admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, regr_avgx_=regr_avgx_func_)
        >>> print(df)
           regr_avgx_
        0     3.54175
        >>>
        # Example 2: Calculate the mean of the "gpa" column (independent variable) with
        #            respect to values in "admitted" column (dependent variable) for each 
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(regr_avgx_=regr_avgx_func_)
          programming  regr_avgx_
        0    Beginner    3.660000
        1    Advanced    3.615625
        2      Novice    3.294545
        >>>
    regr_avgy
    regr_avgy
    Functions
    regr_avgy(dependent_variable_expression, independent_variable_expression)
    DESCRIPTION:
        Function returns the mean of the dependent_variable_expression for all
        non-null data pairs of the dependent and independent variable arguments.
        When there are fewer than two non-null data point pairs in the data used
        for the computation, the function returns NULL.
    PARAMETERS:
        dependent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing a
            dependent variable for the regression.
            A dependent variable is something that is measured in response to a treatment.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        independent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing an
            independent variable for the regression.
            An independent variable is a treatment: something that is varied under 
            your control to test the behavior of another variable.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the mean of the "admitted" column (dependent variable) with 
        #            respect to the values in "gpa" column (independent variable).
        # Import func from sqlalchemy to execute regr_avgy function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> regr_avgy_func_ = func.regr_avgy(admissions_train.admitted.expression, admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, regr_avgy_=regr_avgy_func_)
        >>> print(df)
           regr_avgy_
        0        0.65
        >>>
        # Example 2: Calculate the mean of the "admitted" column (dependent variable) with 
        #            respect to the values in "gpa" column (independent variable) for each 
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(regr_avgy_=regr_avgy_func_)
          programming  regr_avgy_
        0    Beginner    0.384615
        1    Advanced    0.812500
        2      Novice    0.727273
        >>>
    regr_count
    regr_count
    Functions
    regr_count(dependent_variable_expression, independent_variable_expression)
    DESCRIPTION:
        Function returns the count of all non-null data pairs of the dependent and
        independent variable arguments.
    PARAMETERS:
        dependent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing a
            dependent variable for the regression.
            A dependent variable is something that is measured in response to a treatment.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        independent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing an
            independent variable for the regression.
            An independent variable is a treatment: something that is varied under 
            your control to test the behavior of another variable.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Get the total number of non-null pairs between "gpa" column (independent variable)
        #            and "admitted" column (dependent variable).
        # Import func from sqlalchemy to execute regr_count function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> regr_count_func_ = func.regr_count(admissions_train.admitted.expression, admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, regr_count_=regr_count_func_)
        >>> print(df)
           regr_count_
        0           40
        >>>
        # Example 2: Get the total number of non-null pairs between "gpa" column (independent variable)
        #            and "admitted" column (dependent variable) for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(regr_count_=regr_count_func_)
          programming  regr_count_
        0    Beginner           13
        1    Advanced           16
        2      Novice           11
        >>>
    regr_intercept
    regr_intercept
    Functions
    regr_intercept(dependent_variable_expression, independent_variable_expression)
    DESCRIPTION:
        Function returns the intercept of the univariate linear regression line through 
        all non-null data pairs of the dependent and independent variable arguments.
        The intercept is the point at which the regression line through the non-null
        data pairs in the sample intersects the ordinate, or y-axis, of the graph.
        The plot of the linear regression on the variables is used to predict the behavior
        of the dependent variable from the change in the independent variable.
        There can be a strong nonlinear relationship between independent and dependent
        variables, and the computation of the simple linear regression between such variable
        pairs does not reflect such a relationship.
    PARAMETERS:
        dependent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing a
            dependent variable for the regression.
            A dependent variable is something that is measured in response to a treatment.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        independent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing an
            independent variable for the regression.
            An independent variable is a treatment: something that is varied under 
            your control to test the behavior of another variable.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the intercept of the "gpa" column (independent variable) with
        #            "admitted" column (dependent variable).
        # Import func from sqlalchemy to execute regr_intercept function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> regr_intercept_func_ = func.regr_intercept(admissions_train.admitted.expression, admissions_train.gpa.expressi
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, regr_intercept_=regr_intercept_func_)
        >>> print(df)
           regr_intercept_
        0         0.724144
        >>>
        # Example 2: Calculate the intercept of the "gpa" column (independent variable) with
        #            "admitted" column (dependent variable) for each
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(regr_intercept_=regr_intercept_func_)
          programming  regr_intercept_
        0    Beginner         2.566361
        1    Advanced        -0.626557
        2      Novice         1.000091
        >>>
    regr_r2
    regr_r2
    Functions
    regr_r2(dependent_variable_expression, independent_variable_expression)
    DESCRIPTION:
        Function returns the coefficient of determination for all non-null data
        pairs of the dependent and independent variable arguments.
        When there are fewer than two non-null data point pairs in the data used
        for the computation, the function returns NULL.
    PARAMETERS:
        dependent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing a
            dependent variable for the regression.
            A dependent variable is something that is measured in response to a treatment.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        independent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing an
            independent variable for the regression.
            An independent variable is a treatment: something that is varied under 
            your control to test the behavior of another variable.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Get the coefficient of determination for the values forming a pair between
        #            "gpa" column (independent variable) and "admitted" column (dependent variable).
        # Import func from sqlalchemy to execute regr_r2 function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> regr_r2_func_ = func.regr_r2(admissions_train.admitted.expression, admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, regr_r2_=regr_r2_func_)
        >>> print(df)
           regr_r2_
        0  0.000496
        >>>
        # Example 2: Get the coefficient of determination for the values forming a pair between
        #            "gpa" column (independent variable) and "admitted" column (dependent variable)
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(regr_r2_=regr_r2_func_)
          programming  regr_r2_
        0    Beginner  0.174361
        1    Advanced  0.237888
        2      Novice  0.013146
        >>>
    regr_slope
    regr_slope
    Functions
    regr_slope(dependent_variable_expression, independent_variable_expression)
    DESCRIPTION:
        Function returns the slope of the univariate linear regression line through
        all non-null data pairs of the dependent and independent variable arguments.
        When there are fewer than two non-null data point pairs in the data used
        for the computation, the function returns NULL.
        The slope of the best fit linear regression is a measure of the rate of change
        of the regression of one independent variable on the dependent variable.
        The plot of the linear regression on the variables is used to predict the behavior
        of the dependent variable from the change in the independent variable.
        Note that this computation assumes a linear relationship between the variables.
        There can be a strong nonlinear relationship between independent and dependent
        variables, and the computation of the simple linear regression between such
        variable pairs does not reflect such a relationship.
    PARAMETERS:
        dependent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing a
            dependent variable for the regression.
            A dependent variable is something that is measured in response to a treatment.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        independent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing an
            independent variable for the regression.
            An independent variable is a treatment: something that is varied under 
            your control to test the behavior of another variable.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Get the slope of the univariate linear regression line through
        #            the data pairs of values in "gpa" column (independent variable)
        #            and "admitted" column (dependent variable).
        # Import func from sqlalchemy to execute regr_slope function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> regr_slope_func_ = func.regr_slope(admissions_train.admitted.expression, admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, regr_slope_=regr_slope_func_)
        >>> print(df)
           regr_slope_
        0    -0.020934
        >>>
        # Example 2: Get the slope of the univariate linear regression line through
        #            the data pairs of values in "gpa" column (independent variable)
        #            and "admitted" column (dependent variable) for each
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(regr_slope_=regr_slope_func_)
          programming  regr_slope_
        0    Beginner    -0.596105
        1    Advanced     0.398010
        2      Novice    -0.082809
        >>>
    regr_sxx
    regr_sxx
    Functions
    regr_sxx(dependent_variable_expression, independent_variable_expression)
    DESCRIPTION:
        Function returns the sum of the squares of the independent_variable_expression
        for all non-null data pairs of the dependent and independent variable arguments.
        When there are fewer than two non-null data point pairs in the data used
        for the computation, the function returns NULL.
    PARAMETERS:
        dependent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing a
            dependent variable for the regression.
            A dependent variable is something that is measured in response to a treatment.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        independent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing an
            independent variable for the regression.
            An independent variable is a treatment: something that is varied under 
            your control to test the behavior of another variable.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the sum of the squares of the values in "gpa" column
        #            (independent variable) with respect to values in "admitted"
        #            column (dependent variable).
        # Import func from sqlalchemy to execute regr_sxx function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> regr_sxx_func_ = func.regr_sxx(admissions_train.admitted.expression, admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, regr_sxx_=regr_sxx_func_)
        >>> print(df)
           regr_sxx_
        0  10.294177
        >>>
        # Example 2: Calculate the sum of the squares of the values in "gpa" column
        #            (independent variable) with respect to values in "admitted"
        #            column (dependent variable) for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(regr_sxx_=regr_sxx_func_)
          programming  regr_sxx_
        0    Beginner   1.509800
        1    Advanced   3.660394
        2      Novice   4.182673
        >>>
    regr_sxy
    regr_sxy
    Functions
    regr_sxy(dependent_variable_expression, independent_variable_expression)
    DESCRIPTION:
        Function returns the sum of the products of the independent_variable_expression 
        and the dependent_variable_expression for all non-null data pairs of the dependent 
        and independent variable arguments.
        When there are fewer than two non-null data point pairs in the data used
        for the computation, the function returns NULL.
    PARAMETERS:
        dependent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing a
            dependent variable for the regression.
            A dependent variable is something that is measured in response to a treatment.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        independent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing an
            independent variable for the regression.
            An independent variable is a treatment: something that is varied under 
            your control to test the behavior of another variable.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the sum of the products of the "gpa" column (independent variable)
        #            with respect to values in "admitted" column (dependent variable).
        # Import func from sqlalchemy to execute regr_sxy function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> regr_sxy_func_ = func.regr_sxy(admissions_train.admitted.expression, admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, regr_sxy_=regr_sxy_func_)
        >>> print(df)
           regr_sxy_
        0    -0.2155
        >>>
        # Example 2: Calculate the sum of the products of the "gpa" column (independent variable)
        #            with respect to values in "admitted" column (dependent variable) for each
        #            level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(regr_sxy_=regr_sxy_func_)
          programming  regr_sxy_
        0    Beginner  -0.900000
        1    Advanced   1.456875
        2      Novice  -0.346364
        >>>
    regr_syy
    regr_syy
    Functions
    regr_syy(dependent_variable_expression, independent_variable_expression)
    DESCRIPTION:
        Function returns the sum of the squares of the dependent_variable_expression
        for all non-null data pairs of the dependent and independent variable arguments.
        When there are fewer than two non-null data point pairs in the data used
        for the computation, the function returns NULL.
    PARAMETERS:
        dependent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing a
            dependent variable for the regression.
            A dependent variable is something that is measured in response to a treatment.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        independent_variable_expression:
            Required Argument.
            Specifies a ColumnExpression of a column or a literal representing an
            independent variable for the regression.
            An independent variable is a treatment: something that is varied under 
            your control to test the behavior of another variable.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the sum of the squares of the values in "admitted" column
        #            (dependent variable) with respect to values in "gpa"
        #            column (independent variable).
        # Import func from sqlalchemy to execute regr_syy function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> regr_syy_func_ = func.regr_syy(admissions_train.admitted.expression, admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, regr_syy_=regr_syy_func_)
        >>> print(df)
           regr_syy_
        0        9.1
        >>>
        # Example 2: Calculate the sum of the squares of the values in "admitted" column
        #            (dependent variable) with respect to values in "gpa"
        #            column (independent variable) for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(regr_syy_=regr_syy_func_)
          programming  regr_syy_
        0    Beginner   3.076923
        1    Advanced   2.437500
        2      Novice   2.181818
        >>>
    skew
    skew
    Functions
    skew(value_expression)
    DESCRIPTION:
        Function returns the skewness of the distribution of value_expression.
        Skewness is the third moment of a distribution. It is a measure of the 
        asymmetry of the distribution about its mean compared with the normal 
        (or Gaussian) distribution.
            * The normal distribution has a skewness of 0.
            * Positive skewness indicates a distribution having an asymmetric tail
              extending toward more positive values.
            * Negative skewness indicates an asymmetric tail extending toward more 
              negative values.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column for which the skewness 
            of the distribution of its values is to be computed.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. Nulls are not included in the result computation.
                2. Following conditions will produce null result:
                    a. Fewer than three non-null data points in the data used for the 
                       computation.
                    b. Standard deviation for a column is equal to 0.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the skewness of the distribution of the "gpa" column.
        # Import func from sqlalchemy to execute skew function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> skew_func_ = func.skew(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, skew_gpa_=skew_func_)
        >>> print(df)
           skew_gpa_
        0  -2.058969
        >>>
        # Example 2: Calculate the skewness of the distribution of "gpa" column for 
        #            each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(skew_gpa_=func.skew(admissions_train.gpa.expression))
          programming  skew_gpa_
        0    Beginner  -2.084085
        1    Advanced  -2.703078
        2      Novice  -1.459620
        >>>
    stddev_pop
    stddev_pop
    Functions
    stddev_pop(value_expression)
    DESCRIPTION:
        Function returns the population standard deviation for the non-null data 
        points in value_expression.
        The standard deviation is the second moment of a population. For a population,
        it is a measure of dispersion from the mean of that population.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column for which population 
            standard deviation is to be computed.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. Nulls are not included in the result computation.
                2. When there are no non-null data points in the population, the 
                   function returns NULL.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the population standard deviation for the values in
        #            "gpa" column.
        # Import func from sqlalchemy to execute stddev_pop function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> stddev_pop_func_ = func.stddev_pop(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, stddev_pop_gpa_=stddev_pop_func_)
        >>> print(df)
           stddev_pop_gpa_
        0         0.507301
        >>>
        # Example 2: Calculate the population standard deviation for the values in
        #            "gpa" column for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(stddev_pop_gpa_=func.stddev_pop(admissions_train.gpa.expression
          programming  stddev_pop_gpa_
        0    Advanced         0.478304
        1      Novice         0.616638
        2    Beginner         0.340791
        >>>
    stddev_samp
    stddev_samp
    Functions
    stddev_samp(value_expression)
    DESCRIPTION:
        Function returns the sample standard deviation for the non-null data 
        points in value_expression.
        The standard deviation is the second moment of a distribution. For a sample,
        it is a measure of dispersion from the mean of that sample. The computation
        is more conservative for the population standard deviation to minimize the
        effect of outliers on the computed value.
        When there are fewer than two non-null data points in the sample used for the
        computation, the function returns NULL.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column for which sample
            standard deviation is to be computed.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the sample standard deviation for the values in
        #            "gpa" column.
        # Import func from sqlalchemy to execute stddev_samp function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> stddev_samp_func_ = func.stddev_samp(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, stddev_samp_gpa_=stddev_samp_func_)
        >>> print(df)
           stddev_samp_gpa_
        0          0.513764
        >>>
        # Example 2: Calculate the sample standard deviation for the values in
        #            "gpa" column for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(stddev_samp_gpa_=func.stddev_samp(admissions_train.gpa.expressi
          programming  stddev_samp_gpa_
        0    Advanced          0.493990
        1      Novice          0.646736
        2    Beginner          0.354706
        >>>
    sum
    sum
    Functions
    sum(value_expression)
    DESCRIPTION:
        Function returns a column value that is the arithmetic sum of value_expression.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column for which the sum 
            is to be computed.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the total number of admitted students. This can be
        #            done by calculating the sum of values in the "admitted" column.
        #            This column contains value 1 if student is admitted else contains 0.
        # Import func from sqlalchemy to execute sum function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> sum_func_ = func.sum(admissions_train.admitted.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, sum_admitted_=sum_func_)
        >>> print(df)
           sum_admitted_
        0             26
        >>>
        # Example 2: Calculate the total number of admitted students for each level of
        #            programming. This can be done by calculating the sum of values in
        #            the "admitted" column and grouping data on "programming" column.
        #            The "admitted" column contains value 1 is student is admitted else
        #            contains 0.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(res=func.sum(admissions_train.admitted.expression))
          programming  res
        0    Advanced   13
        1      Novice    8
        2    Beginner    5
        >>>
    var_pop
    var_pop
    Functions
    var_pop(value_expression)
    DESCRIPTION:
        Function returns the population variance for the data points in
        value_expression.
        The variance of a population is a measure of dispersion from the mean
        of that population.
        When there are no non-null data points in the population the
        function returns NULL.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column for which population variance 
            is to be computed.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the population variance for the values in "gpa" column.
        # Import func from sqlalchemy to execute var_pop function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> var_pop_func_ = func.var_pop(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, var_pop_gpa_=var_pop_func_)
        >>> print(df)
           var_pop_gpa_
        0      0.257354
        >>>
        # Example 2: Calculate the population variance for the values in "gpa" column
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(var_pop_gpa_=func.var_pop(admissions_train.gpa.expression))
          programming  var_pop_gpa_
        0    Advanced      0.228775
        1      Novice      0.380243
        2    Beginner      0.116138
        >>>
    var_samp
    var_samp
    Functions
    var_samp(value_expression)
    DESCRIPTION:
        Function returns the sample variance for the data points in
        value_expression.
        The variance of a sample is a measure of dispersion from the mean of
        that sample. It is the square of the sample standard deviation.
        The computation is more conservative than that for the population
        standard deviation to minimize the effect of outliers on the computed
        value.
        When the sample used for the computation has fewer than two non-null
        data points, the function returns NULL.
    PARAMETERS:
        value_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column for which sample variance
            is to be computed.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example 1: Calculate the sample variance for the values in "gpa" column.
        # Import func from sqlalchemy to execute var_samp function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> var_samp_func_ = func.var_samp(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(True, var_samp_gpa_=var_samp_func_)
        >>> print(df)
           var_samp_gpa_
        0       0.263953
        >>>
        # Example 2: Calculate the sample variance for the values in "gpa" column
        #            for each level of programming.
        # Note:
        #   When assign() is run after DataFrame.groupby(), the function ignores
        #   the "drop_columns" argument.
        >>> admissions_train.groupby("programming").assign(var_samp_gpa_=func.var_samp(admissions_train.gpa.expression))
          programming  var_samp_gpa_
        0    Advanced       0.244026
        1      Novice       0.418267
        2    Beginner       0.125817
        >>>
    Arithmetic Functions
    abs
    abs
    Functions
    abs(column_expression)
    DESCRIPTION:
        Function computes the absolute value of an argument.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column on which ABS() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported. For more information on implicit type conversion,
                   see Teradata Vantage™ Data Types and Literals, B035-1143.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates absolute value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute ABS function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> abs_func_ = func.abs(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(abs_gpa_=abs_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  abs_gpa_
        id
        22     yes  3.46    Novice    Beginner         0      3.46
        36      no  3.00  Advanced      Novice         0      3.00
        15     yes  4.00  Advanced    Advanced         1      4.00
        38     yes  2.65  Advanced    Beginner         1      2.65
        5       no  3.44    Novice      Novice         0      3.44
        17      no  3.83  Advanced    Advanced         1      3.83
        34     yes  3.85  Advanced    Beginner         0      3.85
        13      no  4.00  Advanced      Novice         1      4.00
        26     yes  3.57  Advanced    Advanced         1      3.57
        19     yes  1.98  Advanced    Advanced         0      1.98
        >>>
    case_n
    case_n
    Functions
    case_n(conditional_column_expressions)
    DESCRIPTION:
        Function evaluates a list of conditions and returns the position of the first
        condition that evaluates to TRUE, provided that no prior condition in the
        list evaluates to UNKNOWN.
    PARAMETERS:
        conditional_column_expressions:
            Required Argument.
            Specifies a condition column expression or multiple comma-separated condition
            column expressions to evaluate.
            Format for the column_expression: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute case_n() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: We pass multiple conditional column expressions separated by comma.
        >>> case_n_func_ = func.case_N(admissions_train.stats.expression == 'Novice',
        ...                            admissions_train.stats.expression == 'Beginner')
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(case_n_func_=case_n_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  case_n_func_
        id
        15     yes  4.00  Advanced    Advanced         1           NaN
        7      yes  2.33    Novice      Novice         1           1.0
        22     yes  3.46    Novice    Beginner         0           1.0
        17      no  3.83  Advanced    Advanced         1           NaN
        13      no  4.00  Advanced      Novice         1           NaN
        38     yes  2.65  Advanced    Beginner         1           NaN
        26     yes  3.57  Advanced    Advanced         1           NaN
        5       no  3.44    Novice      Novice         0           1.0
        34     yes  3.85  Advanced    Beginner         0           NaN
        40     yes  3.95    Novice    Beginner         0           1.0
        >>>
    ceil
    ceil
    Functions
    ceil(column_expression)
    DESCRIPTION:
        Function returns the smallest integer value that is not less than the input argument.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column on which ceil() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    ALTERNATE NAME:
        ceiling
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates ceil value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute ceil() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> ceil_func_ = func.ceil(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(ceil_gpa_ = ceil_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  ceil_gpa_
        id
        13      no  4.00  Advanced      Novice         1        4.0
        36      no  3.00  Advanced      Novice         0        3.0
        15     yes  4.00  Advanced    Advanced         1        4.0
        40     yes  3.95    Novice    Beginner         0        4.0
        22     yes  3.46    Novice    Beginner         0        4.0
        38     yes  2.65  Advanced    Beginner         1        3.0
        26     yes  3.57  Advanced    Advanced         1        4.0
        5       no  3.44    Novice      Novice         0        4.0
        7      yes  2.33    Novice      Novice         1        3.0
        19     yes  1.98  Advanced    Advanced         0        2.0
        >>>
        # "ceiling" can be used as an alternative function name.
        >>> ceiling_func_ = func.ceiling(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(ceiling_gpa_ = ceiling_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  ceiling_gpa_
        id
        13      no  4.00  Advanced      Novice         1        4.0
        36      no  3.00  Advanced      Novice         0        3.0
        15     yes  4.00  Advanced    Advanced         1        4.0
        40     yes  3.95    Novice    Beginner         0        4.0
        22     yes  3.46    Novice    Beginner         0        4.0
        38     yes  2.65  Advanced    Beginner         1        3.0
        26     yes  3.57  Advanced    Advanced         1        4.0
        5       no  3.44    Novice      Novice         0        4.0
        7      yes  2.33    Novice      Novice         1        3.0
        19     yes  1.98  Advanced    Advanced         0        2.0
        >>>
    degrees
    degrees
    Functions
    degrees(column_expression_or_constant)
    DESCRIPTION:
        Function takes a value specified in radians and converts it to degrees.
    PARAMETERS:
        column_expression_or_constant:
            Required Argument.
            Specifies a ColumnExpression containing radian values or constant representing a radian value.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported. For more information on implicit type conversion,
                   see Teradata Vantage™ Data Types and Literals, B035-1143.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute degrees() function.
        >>> from sqlalchemy import func
        # Calculate degrees for values in "admitted" column.
        # Create a sqlalchemy Function object.
        >>> degrees_func_ = func.degrees(admissions_train.admitted.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(degrees_admitted_func_ = degrees_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  degrees_admitted_func_
        id
        22     yes  3.46    Novice    Beginner         0                 0.00000
        26     yes  3.57  Advanced    Advanced         1                57.29578
        5       no  3.44    Novice      Novice         0                 0.00000
        17      no  3.83  Advanced    Advanced         1                57.29578
        13      no  4.00  Advanced      Novice         1                57.29578
        19     yes  1.98  Advanced    Advanced         0                 0.00000
        36      no  3.00  Advanced      Novice         0                 0.00000
        15     yes  4.00  Advanced    Advanced         1                57.29578
        34     yes  3.85  Advanced    Beginner         0                 0.00000
        38     yes  2.65  Advanced    Beginner         1                57.29578
        >>>
    exp
    exp
    Functions
    exp(column_expression)
    DESCRIPTION:
        Function raises e (the base of natural logarithms) to the power of the argument, where e = 2.71828182845905.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression containing numeric values.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported. For more information on implicit type conversion,
                   see Teradata Vantage™ Data Types and Literals, B035-1143.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute radians() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> exp_func_ = func.exp(admissions_train.admitted.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(exp_admitted_func_ = exp_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  exp_admitted_func_
        id
        15     yes  4.00  Advanced    Advanced         1            2.718282
        7      yes  2.33    Novice      Novice         1            2.718282
        22     yes  3.46    Novice    Beginner         0            1.000000
        17      no  3.83  Advanced    Advanced         1            2.718282
        13      no  4.00  Advanced      Novice         1            2.718282
        38     yes  2.65  Advanced    Beginner         1            2.718282
        26     yes  3.57  Advanced    Advanced         1            2.718282
        5       no  3.44    Novice      Novice         0            1.000000
        34     yes  3.85  Advanced    Beginner         0            1.000000
        40     yes  3.95    Novice    Beginner         0            1.000000
        >>>
    floor
    ﬂoor
    Functions
    ﬂoor(column_expression)
    DESCRIPTION:
        Function returns the largest integer equal to or less than the input argument.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column on which floor() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates floor value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute floor() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> floor_func_ = func.floor(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(floor_gpa_func_ = floor_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  floor_gpa_func_
        id
        22     yes  3.46    Novice    Beginner         0              3.0
        36      no  3.00  Advanced      Novice         0              3.0
        15     yes  4.00  Advanced    Advanced         1              4.0
        38     yes  2.65  Advanced    Beginner         1              2.0
        5       no  3.44    Novice      Novice         0              3.0
        17      no  3.83  Advanced    Advanced         1              3.0
        34     yes  3.85  Advanced    Beginner         0              3.0
        13      no  4.00  Advanced      Novice         1              4.0
        26     yes  3.57  Advanced    Advanced         1              3.0
        19     yes  1.98  Advanced    Advanced         0              1.0
        >>>
    ln
    ln
    Functions
    ln(column_expression_or_constant)
    DESCRIPTION:
        Function returns the natural logarithm of the argument.
    PARAMETERS:
        column_expression_or_constant:
            Required Argument.
            Specifies a ColumnExpression of a column containing numeric values or a numeric constant
            to compute the natural logarithm.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported. For more information on implicit type conversion,
                   see Teradata Vantage™ Data Types and Literals, B035-1143.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example computes natural logarithm of values in "gpa" and a constant 10.
        # Import func from sqlalchemy to execute ln() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object and pass the Function object as input to DataFrame.assign().
        # Note: We are using 'ln' and 'LN' as function names. Function names are case-insensitive.
        >>> df = admissions_train.assign(natural_log_gpa_func_ = func.ln(admissions_train.gpa.expression),
        ...                              natural_log_const_func_ = func.LN(10))
        >>> print(df)
           masters   gpa     stats programming  admitted  natural_log_const_func_  natural_log_gpa_func_
        id
        22     yes  3.46    Novice    Beginner         0                 2.302585               1.241269
        36      no  3.00  Advanced      Novice         0                 2.302585               1.098612
        15     yes  4.00  Advanced    Advanced         1                 2.302585               1.386294
        38     yes  2.65  Advanced    Beginner         1                 2.302585               0.974560
        5       no  3.44    Novice      Novice         0                 2.302585               1.235471
        17      no  3.83  Advanced    Advanced         1                 2.302585               1.342865
        34     yes  3.85  Advanced    Beginner         0                 2.302585               1.348073
        13      no  4.00  Advanced      Novice         1                 2.302585               1.386294
        26     yes  3.57  Advanced    Advanced         1                 2.302585               1.272566
        19     yes  1.98  Advanced    Advanced         0                 2.302585               0.683097
        >>>
    log
    log
    Functions
    log(column_expression_or_constant)
    DESCRIPTION:
        Function returns the base 10 logarithm of an argument.
    PARAMETERS:
        column_expression_or_constant:
            Required Argument.
            Specifies a ColumnExpression of a column containing numeric values or a numeric constant
            to compute the base 10 logarithm.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported. For more information on implicit type conversion,
                   see Teradata Vantage™ Data Types and Literals, B035-1143.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example computes logarithm of values in "gpa" * 10 and a constant 10.
        # Import func from sqlalchemy to execute log() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object and pass the Function object as input to DataFrame.assign().
        # Note: We are using 'log' and 'LOG' as function names. Function names are case-insensitive.
        >>> df = admissions_train.assign(log_gpa_func_ = func.LOG(admissions_train.gpa.expression * 10),
        ...                              log_const_func = func.log(10))
        >>> print(df)
           masters   gpa     stats programming  admitted  log_const_func  log_gpa_func_
        id
        22     yes  3.46    Novice    Beginner         0             1.0       1.539076
        36      no  3.00  Advanced      Novice         0             1.0       1.477121
        15     yes  4.00  Advanced    Advanced         1             1.0       1.602060
        38     yes  2.65  Advanced    Beginner         1             1.0       1.423246
        5       no  3.44    Novice      Novice         0             1.0       1.536558
        17      no  3.83  Advanced    Advanced         1             1.0       1.583199
        34     yes  3.85  Advanced    Beginner         0             1.0       1.585461
        13      no  4.00  Advanced      Novice         1             1.0       1.602060
        26     yes  3.57  Advanced    Advanced         1             1.0       1.552668
        19     yes  1.98  Advanced    Advanced         0             1.0       1.296665
        >>>
    mod
    mod
    Functions
    mod(column_expression_or_constant1, column_expression_or_constant2)
    DESCRIPTION:
        Function returns the remainder (modulus) of dividend (column_expression_or_constant1)
        divided by divisor (column_expression_or_constant1).
    PARAMETERS:
        column_expression_or_constant1:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value that is the dividend.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        column_expression_or_constant2:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value that is the divisor.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute mod() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object and pass the Function object as input to DataFrame.assign().
        # Note: We are using 'mod' and 'MOD' as function names. Function names are case-insensitive.
        >>> df = admissions_train.assign(modgpa2 = func.mod(admissions_train.gpa.expression, 2),
        ...                              modgpa12 = func.MOD(12, admissions_train.gpa.expression))
        >>> print(df)
           masters   gpa     stats programming  admitted  modgpa12  modgpa2
        id
        22     yes  3.46    Novice    Beginner         0      1.62     1.46
        36      no  3.00  Advanced      Novice         0      0.00     1.00
        15     yes  4.00  Advanced    Advanced         1      0.00     0.00
        38     yes  2.65  Advanced    Beginner         1      1.40     0.65
        5       no  3.44    Novice      Novice         0      1.68     1.44
        17      no  3.83  Advanced    Advanced         1      0.51     1.83
        34     yes  3.85  Advanced    Beginner         0      0.45     1.85
        13      no  4.00  Advanced      Novice         1      0.00     0.00
        26     yes  3.57  Advanced    Advanced         1      1.29     1.57
        19     yes  1.98  Advanced    Advanced         0      0.12     1.98
        >>>
    nullifzero
    nullifzero
    Functions
    nullifzero(column_expression_or_constant)
    DESCRIPTION:
        Function converts data from zero to null to avoid problems with division by zero.
    PARAMETERS:
        column_expression_or_constant:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value to be converted to null.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported. For more information on implicit type conversion,
                   see Teradata Vantage™ Data Types and Literals, B035-1143.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute nullifzero() function.
        >>> from sqlalchemy import func
        # Use case of NULLIFZERO() function - Function can be used, when we are dividing by a column that
        # may contain 0, so that we can avoid errors coming from expressions such as:
        #         8 / 0
        # Note:
        #        In this example, we have combined teradataml ColumnExpression and SQLAlchemy func object
        #        admissions_train.gpa -- Is a teradataml ColumnExpression
        #        func.nullifzero(admissions_train.admitted.expression) -- Is SQLAlchemy func object
        >>> df = admissions_train.assign(admitted_null_if_zero = func.nullifzero(admissions_train.admitted.expression))
        >>> print(df)
           masters   gpa     stats programming  admitted  admitted_null_if_zero
        id
        22     yes  3.46    Novice    Beginner         0                    NaN
        36      no  3.00  Advanced      Novice         0                    NaN
        15     yes  4.00  Advanced    Advanced         1                    1.0
        38     yes  2.65  Advanced    Beginner         1                    1.0
        5       no  3.44    Novice      Novice         0                    NaN
        17      no  3.83  Advanced    Advanced         1                    1.0
        34     yes  3.85  Advanced    Beginner         0                    NaN
        13      no  4.00  Advanced      Novice         1                    1.0
        26     yes  3.57  Advanced    Advanced         1                    1.0
        19     yes  1.98  Advanced    Advanced         0                    NaN
        >>>
    power
    power
    Functions
    power(column_expression_or_constant1, column_expression_or_constant2)
    DESCRIPTION:
        Function returns the base value (column_expression_or_constant1) raised to the
        power of the exponent value (column_expression_or_constant2).
    PARAMETERS:
        column_expression_or_constant1:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value that is the base value.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        column_expression_or_constant2:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value that is the exponent value.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute power() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object and pass the Function object as input to DataFrame.assign().
        # Note: We are using 'power' and 'POWER' as function names. Function names are case-insensitive.
        >>> df = admissions_train.assign(pow2gpa = func.power(admissions_train.gpa.expression, 2),
        ...                              pow_admitted_gpa = func.POWER(admissions_train.gpa.expression, admissions_train.a
        >>> print(df)
           masters   gpa     stats programming  admitted  pow2gpa  pow_admitted_gpa
        id
        13      no  4.00  Advanced      Novice         1  16.0000              4.00
        26     yes  3.57  Advanced    Advanced         1  12.7449              3.57
        5       no  3.44    Novice      Novice         0  11.8336              1.00
        19     yes  1.98  Advanced    Advanced         0   3.9204              1.00
        15     yes  4.00  Advanced    Advanced         1  16.0000              4.00
        40     yes  3.95    Novice    Beginner         0  15.6025              1.00
        7      yes  2.33    Novice      Novice         1   5.4289              2.33
        22     yes  3.46    Novice    Beginner         0  11.9716              1.00
        36      no  3.00  Advanced      Novice         0   9.0000              1.00
        38     yes  2.65  Advanced    Beginner         1   7.0225              2.65
        >>>
    radians
    radians
    Functions
    radians(column_expression_or_constant)
    DESCRIPTION:
        Function takes a value specified in degrees and converts it to radians.
    PARAMETERS:
        column_expression_or_constant:
            Required Argument.
            Specifies a ColumnExpression containing degrees values or constant representing a degrees value.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported. For more information on implicit type conversion,
                   see Teradata Vantage™ Data Types and Literals, B035-1143.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example converts values in the "admitted" column to radians with help of SQLAlchemy.
        # Import func from sqlalchemy to execute radians() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> radians_func_ = func.radians(admissions_train.admitted.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(radians_admitted_func_ = radians_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  radians_admitted_func_
        id
        22     yes  3.46    Novice    Beginner         0                0.000000
        36      no  3.00  Advanced      Novice         0                0.000000
        15     yes  4.00  Advanced    Advanced         1                0.017453
        38     yes  2.65  Advanced    Beginner         1                0.017453
        5       no  3.44    Novice      Novice         0                0.000000
        17      no  3.83  Advanced    Advanced         1                0.017453
        34     yes  3.85  Advanced    Beginner         0                0.000000
        13      no  4.00  Advanced      Novice         1                0.017453
        26     yes  3.57  Advanced    Advanced         1                0.017453
        19     yes  1.98  Advanced    Advanced         0                0.000000
        >>>
    round
    round
    Functions
    round(column_expression_or_constant1, column_expression_or_constant2)
    DESCRIPTION:
        Function returns numeric value (column_expression_or_constant1) rounded by the
        places_value (column_expression_or_constant2) number of places to the right or
        left of the decimal point.
        ROUND functions as follows:
            * It rounds places_value places to the right of the decimal point if places_value
              is positive.
            * It rounds places_value places to the left of the decimal point if places_value
              is negative.
            * It rounds to 0 places if places_value is zero or is omitted.
            * If numeric_value or places_value is NULL, the function returns NULL.
            * ROUND rounds the value away from zero and it only rounds when the next digit is
              a value of 5 or greater.
    PARAMETERS:
        column_expression_or_constant1:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant value that
            is the numeric value to be rounded.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        column_expression_or_constant2:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant value that
            defines the number of places to round.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute round() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object and pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(round_gpa_1 = func.Round(admissions_train.gpa.expression, 1),
        ...                      round_gpa_admitted = func.round(admissions_train.gpa.expression, admissions_train.admitte
        ...                      round_const_3 = func.ROUND(2.433412, 3))
        >>> print(df)
           masters   gpa     stats programming  admitted  round_const_3  round_gpa_1  round_gpa_admitted
        id
        22     yes  3.46    Novice    Beginner         0          2.433          3.5                 3.0
        36      no  3.00  Advanced      Novice         0          2.433          3.0                 3.0
        15     yes  4.00  Advanced    Advanced         1          2.433          4.0                 4.0
        38     yes  2.65  Advanced    Beginner         1          2.433          2.6                 2.6
        5       no  3.44    Novice      Novice         0          2.433          3.4                 3.0
        17      no  3.83  Advanced    Advanced         1          2.433          3.8                 3.8
        34     yes  3.85  Advanced    Beginner         0          2.433          3.9                 4.0
        13      no  4.00  Advanced      Novice         1          2.433          4.0                 4.0
        26     yes  3.57  Advanced    Advanced         1          2.433          3.6                 3.6
        19     yes  1.98  Advanced    Advanced         0          2.433          2.0                 2.0
        >>>
    sign
    sign
    Functions
    sign(column_expression_or_constant)
    DESCRIPTION:
        Function returns the sign of a numeric argument.
        SIGN functions as follows:
            * If numeric value is < 0, -1 is returned.
            * If numeric value is = 0, 0 is returned.
            * If numeric value is > 0, 1 is returned.
    PARAMETERS:
        column_expression_or_constant:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute sign() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object and pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(sign_gpa = func.sign(admissions_train.gpa.expression),
        ...                              sign_const = func.sign(-3))
        >>> print(df)
           masters   gpa     stats programming  admitted sign_const sign_gpa
        id
        5       no  3.44    Novice      Novice         0         -1        1
        34     yes  3.85  Advanced    Beginner         0         -1        1
        13      no  4.00  Advanced      Novice         1         -1        1
        40     yes  3.95    Novice    Beginner         0         -1        1
        22     yes  3.46    Novice    Beginner         0         -1        1
        19     yes  1.98  Advanced    Advanced         0         -1        1
        36      no  3.00  Advanced      Novice         0         -1        1
        15     yes  4.00  Advanced    Advanced         1         -1        1
        7      yes  2.33    Novice      Novice         1         -1        1
        17      no  3.83  Advanced    Advanced         1         -1        1
        >>>
    sqrt
    sqrt
    Functions
    sqrt(column_expression_or_constant)
    DESCRIPTION:
        Function computes the square root of an argument.
    PARAMETERS:
        column_expression_or_constant:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a constant value.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported. For more information on implicit type conversion,
                   see Teradata Vantage™ Data Types and Literals, B035-1143.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute sign() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object and pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(sqrt_gpa = func.sqrt(admissions_train.gpa.expression),
        ...                              sqrt_const = func.SQRT(25))
        >>> print(df)
           masters   gpa     stats programming  admitted  sqrt_const  sqrt_gpa
        id
        5       no  3.44    Novice      Novice         0         5.0  1.854724
        34     yes  3.85  Advanced    Beginner         0         5.0  1.962142
        13      no  4.00  Advanced      Novice         1         5.0  2.000000
        40     yes  3.95    Novice    Beginner         0         5.0  1.987461
        22     yes  3.46    Novice    Beginner         0         5.0  1.860108
        19     yes  1.98  Advanced    Advanced         0         5.0  1.407125
        36      no  3.00  Advanced      Novice         0         5.0  1.732051
        15     yes  4.00  Advanced    Advanced         1         5.0  2.000000
        7      yes  2.33    Novice      Novice         1         5.0  1.526434
        17      no  3.83  Advanced    Advanced         1         5.0  1.957039
        >>>
    trunc
    trunc
    Functions
    trunc(column_expression_or_constant1, column_expression_or_constant2)
    DESCRIPTION:
        Function returns numeric_value (column_expression_or_constant1) truncated
        places_value (column_expression_or_constant2) places to the right or left
        of the decimal point.
        TRUNC functions as follows:
            * It truncates places_value places to the right of the decimal point if
              places_value is positive.
            * It truncates (makes 0) places_value places to the left of the decimal
              point if places_value is negative.
            * It truncates to 0 places if places_value is zero or is omitted.
            * If numeric_value or places_value is NULL, the function returns NULL.
    PARAMETERS:
        column_expression_or_constant1:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant value.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        column_expression_or_constant2:
            Optional Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant value.
            This decides the number of places to truncate.
            If not specified, values specified by "column_expression_or_constant1" are
            truncated to 0 places by default.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute sign() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object and pass the Function object as input to DataFrame.assign().
        # Note: We are using 'TRUNC', 'trunc' and 'Trunc' as function names. Function names are case-insensitive.
        >>> df = admissions_train.assign(trunc_gpa_1 = func.TRUNC(admissions_train.gpa.expression),
        ...                        trunc_gpa_admitted = func.trunc(admissions_train.gpa.expression,
        ...                                                        admissions_train.admitted.expression),
        ...                        trunc_const_3 = func.Trunc(234232342.433412, -3))
        >>> print(df)
           masters   gpa     stats programming  admitted  trunc_const_3  trunc_gpa_1  trunc_gpa_admitted
        id
        5       no  3.44    Novice      Novice         0    234232000.0          3.0                 3.0
        7      yes  2.33    Novice      Novice         1    234232000.0          2.0                 2.3
        22     yes  3.46    Novice    Beginner         0    234232000.0          3.0                 3.0
        17      no  3.83  Advanced    Advanced         1    234232000.0          3.0                 3.8
        13      no  4.00  Advanced      Novice         1    234232000.0          4.0                 4.0
        19     yes  1.98  Advanced    Advanced         0    234232000.0          1.0                 1.0
        36      no  3.00  Advanced      Novice         0    234232000.0          3.0                 3.0
        15     yes  4.00  Advanced    Advanced         1    234232000.0          4.0                 4.0
        34     yes  3.85  Advanced    Beginner         0    234232000.0          3.0                 3.0
        40     yes  3.95    Novice    Beginner         0    234232000.0          3.0                 3.0
        >>>
    width_bucket
    width_bucket
    Functions
    width_bucket(column_expression, lower_bound, upper_bound, partition_count)
    DESCRIPTION:
        Function returns the number of the partition to which column_expression
        is assigned.
        Following rules apply to width_bucket:
            * If any argument is null, then the result is also null.
            * If lower_bound < upper_bound, then following rules apply:
                * If column_expression < lower_bound, then 0 returned.
                * If column_expression >= upper_bound, the partition_count + 1 is returned.
                  If the result cannot be represented by the data type specified for the
                  result, then an error is returned.
                * Else the greatest exact numeric value with scale 0 that is less than or
                  equal to the following expression is returned.
                  ((partition_count)(column_expression - lower_bound)/(upper_bound - lower_bound)) + 1
            * If lower_bound > upper_bound, then following rules apply:
                * If column_expression > lower_bound, then 0 returned.
                * If column_expression <= upper_bound, the partition_count + 1 is returned.
                  If the result cannot be represented by the data type specified for the
                  result, then an error is returned.
                * Else the least exact numeric value with scale 0 that is less than or equal
                  to the following expression is returned.
                  ((partition_count)(lower_bound - column_expression)/(lower_bound - upper_bound)) + 1
            * Error is reported in following cases:
                * If partition_count <= 0 or if partition_count > 2147483646
                * If lower_bound = upper_bound
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a column for which a partition number is to be returned.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        lower_bound:
            Required Argument.
            Specifies the lower boundary for the range of values to be partitioned equally.
            Types: float or int
        upper_bound:
            Required Argument.
            Specifies the upper boundary for the range of values to be partitioned equally.
            Types: float or int
        partition_count:
            Required Argument.
            Specified the number of partitions to be created. This value also specifies
            the width of the partitions by default. The number of partitions created is
            partition_count + 2. Partition 0 and partition partition_count + 1 account
            for values of column_expression that are outside the lower and upper boundaries.
            Types: float or int
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute sign() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object and pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(bucket_gpa_ = func.Width_bucket(admissions_train.gpa.expression, 2.5, 3.5, 3))
        >>> print(df)
           masters   gpa     stats programming  admitted  bucket_gpa_
        id
        22     yes  3.46    Novice    Beginner         0            3
        36      no  3.00  Advanced      Novice         0            2
        15     yes  4.00  Advanced    Advanced         1            4
        38     yes  2.65  Advanced    Beginner         1            1
        5       no  3.44    Novice      Novice         0            3
        17      no  3.83  Advanced    Advanced         1            4
        34     yes  3.85  Advanced    Beginner         0            4
        13      no  4.00  Advanced      Novice         1            4
        26     yes  3.57  Advanced    Advanced         1            4
        19     yes  1.98  Advanced    Advanced         0            0
        >>>
    zeroifnull
    zeroifnull
    Functions
    zeroifnull(column_expression)
    DESCRIPTION:
        Function converts data from null to 0 to avoid cases where a null result creates an error.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column containing null values to be converted to 0.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported. For more information on implicit type conversion,
                   see Teradata Vantage™ Data Types and Literals, B035-1143.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe","admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        # We will process the data to introduce some null values in the dataframe, which we will convert to 0
        # using "zeroifnull()" function.
        >>> df = DataFrame('admissions_train')
        >>> df1 = df[df.gpa == 4].select(['id', 'stats', 'masters', 'gpa'])
        >>> df2 = df[df.gpa < 2].select(['id', 'stats', 'programming', 'admitted'])
        # Let's concat both df1 and df2.
        >>> cdf = df1.concat(df2)
        >>> cdf
               stats masters  gpa programming  admitted
        id
        29    Novice     yes  4.0        None       NaN
        24  Advanced    None  NaN      Novice       1.0
        19  Advanced    None  NaN    Advanced       0.0
        15  Advanced     yes  4.0        None       NaN
        13  Advanced      no  4.0        None       NaN
        >>>
        # Let's persist the cdf to Vantage.
        >>> copy_to_sql(cdf, "zeroifnull_test_table")
        >>> newdf = DataFrame("zeroifnull_test_table")
        >>> newdf
           id     stats masters  gpa programming  admitted
        0  19  Advanced    None  NaN    Advanced       0.0
        1  24  Advanced    None  NaN      Novice       1.0
        2  13  Advanced      no  4.0        None       NaN
        3  29    Novice     yes  4.0        None       NaN
        4  15  Advanced     yes  4.0        None       NaN
        >>>
        # Import func from sqlalchemy to execute zeroifnull() function.
        >>> from sqlalchemy import func
        # We are converting null values in 'admitted' column to 0 using 'zeroifnull()' function.
        >>> rdf = newdf.assign(admitted_zero_if_null = func.zeroifnull(newdf.admitted.expression))
        >>> print(rdf)
           id     stats masters  gpa programming  admitted  admitted_zero_if_null
        0  29    Novice     yes  4.0        None       NaN                      0
        1  24  Advanced    None  NaN      Novice       1.0                      1
        2  19  Advanced    None  NaN    Advanced       0.0                      0
        3  15  Advanced     yes  4.0        None       NaN                      0
        4  13  Advanced      no  4.0        None       NaN                      0
        >>>
    Hyperbolic Functions
    acosh
    acosh
    Functions
    acosh(column_expression)
    DESCRIPTION:
        Function computes the inverse hyperbolic cosine value of an argument.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which acosh() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates inverse hyperbolic cosine value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute acosh() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> acosh_func_ = func.acosh(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(acosh_gpa_=acosh_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  acosh_gpa_
        id
        13      no  4.00  Advanced      Novice         1    2.063437
        26     yes  3.57  Advanced    Advanced         1    1.945493
        5       no  3.44    Novice      Novice         0    1.906790
        19     yes  1.98  Advanced    Advanced         0    1.305333
        15     yes  4.00  Advanced    Advanced         1    2.063437
        40     yes  3.95    Novice    Beginner         0    2.050440
        7      yes  2.33    Novice      Novice         1    1.489414
        22     yes  3.46    Novice    Beginner         0    1.912847
        36      no  3.00  Advanced      Novice         0    1.762747
        38     yes  2.65  Advanced    Beginner         1    1.630040
        >>>
    asinh
    asinh
    Functions
    asinh(column_expression)
    DESCRIPTION:
        Function computes the inverse hyperbolic sine value of an argument.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which asinh() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates inverse hyperbolic sine value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute asinh() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> asinh_func_ = func.asinh(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(asinh_gpa_=asinh_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  asinh_gpa_
        id
        13      no  4.00  Advanced      Novice         1    2.094713
        26     yes  3.57  Advanced    Advanced         1    1.984775
        5       no  3.44    Novice      Novice         0    1.949105
        19     yes  1.98  Advanced    Advanced         0    1.434655
        15     yes  4.00  Advanced    Advanced         1    2.094713
        40     yes  3.95    Novice    Beginner         0    2.082514
        7      yes  2.33    Novice      Novice         1    1.582175
        22     yes  3.46    Novice    Beginner         0    1.954673
        36      no  3.00  Advanced      Novice         0    1.818446
        38     yes  2.65  Advanced    Beginner         1    1.701543
        >>>
    atanh
    atanh
    Functions
    atanh(column_expression)
    DESCRIPTION:
        Function computes the inverse hyperbolic tangent value of an argument.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which atanh() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates inverse hyperbolic tan value for the "gpa column values/10" with help of SQLAlchemy.
        # Import func from sqlalchemy to execute atanh() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> atanh_func_ = func.atanh(admissions_train.gpa.expression / 10)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(atanh_gpa_=atanh_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  atanh_gpa_
        id
        15     yes  4.00  Advanced    Advanced         1    0.423649
        7      yes  2.33    Novice      Novice         1    0.237359
        22     yes  3.46    Novice    Beginner         0    0.360893
        17      no  3.83  Advanced    Advanced         1    0.403571
        13      no  4.00  Advanced      Novice         1    0.423649
        38     yes  2.65  Advanced    Beginner         1    0.271478
        26     yes  3.57  Advanced    Advanced         1    0.373443
        5       no  3.44    Novice      Novice         0    0.358622
        34     yes  3.85  Advanced    Beginner         0    0.405917
        40     yes  3.95    Novice    Beginner         0    0.417711
        >>>
    cosh
    cosh
    Functions
    cosh(column_expression)
    DESCRIPTION:
        Function computes the hyperbolic cosine value of an argument.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which cosh() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates hyperbolic cos value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute cosh() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> cosh_func_ = func.cosh(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(cosh_gpa_=cosh_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  cosh_gpa_
        id
        22     yes  3.46    Novice    Beginner         0  15.924203
        36      no  3.00  Advanced      Novice         0  10.067662
        15     yes  4.00  Advanced    Advanced         1  27.308233
        38     yes  2.65  Advanced    Beginner         1   7.112345
        5       no  3.44    Novice      Novice         0  15.609511
        17      no  3.83  Advanced    Advanced         1  23.042124
        34     yes  3.85  Advanced    Beginner         0  23.507171
        13      no  4.00  Advanced      Novice         1  27.308233
        26     yes  3.57  Advanced    Advanced         1  17.772375
        19     yes  1.98  Advanced    Advanced         0   3.690406
        >>>
    sinh
    sinh
    Functions
    sinh(column_expression)
    DESCRIPTION:
        Function computes the hyperbolic sine value of an argument.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which sinh() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates hyperbolic sine value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute sinh() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> sinh_func_ = func.sinh(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(sinh_gpa_=sinh_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  sinh_gpa_
        id
        22     yes  3.46    Novice    Beginner         0  15.892773
        26     yes  3.57  Advanced    Advanced         1  17.744219
        5       no  3.44    Novice      Novice         0  15.577447
        17      no  3.83  Advanced    Advanced         1  23.020414
        13      no  4.00  Advanced      Novice         1  27.289917
        19     yes  1.98  Advanced    Advanced         0   3.552337
        36      no  3.00  Advanced      Novice         0  10.017875
        15     yes  4.00  Advanced    Advanced         1  27.289917
        34     yes  3.85  Advanced    Beginner         0  23.485892
        38     yes  2.65  Advanced    Beginner         1   7.041694
        >>>
    tanh
    tanh
    Functions
    tanh(column_expression)
    DESCRIPTION:
        Function computes the hyperbolic tangent value of an argument.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which tanh() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates hyperbolic tan value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute tanh() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> tanh_func_ = func.tanh(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(tanh_gpa_=tanh_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  tanh_gpa_
        id
        22     yes  3.46    Novice    Beginner         0   0.998026
        36      no  3.00  Advanced      Novice         0   0.995055
        15     yes  4.00  Advanced    Advanced         1   0.999329
        38     yes  2.65  Advanced    Beginner         1   0.990066
        5       no  3.44    Novice      Novice         0   0.997946
        17      no  3.83  Advanced    Advanced         1   0.999058
        34     yes  3.85  Advanced    Beginner         0   0.999095
        13      no  4.00  Advanced      Novice         1   0.999329
        26     yes  3.57  Advanced    Advanced         1   0.998416
        19     yes  1.98  Advanced    Advanced         0   0.962587
        >>>
    Trigonometric Functions
    acos
    acos
    Functions
    acos(column_expression)
    DESCRIPTION:
        Function computes the arccosine value of an argument.
        The arccosine is the angle whose cosine is the argument.
        Result values return an angle in the range 0 to π radians, inclusive.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column  or a numeric constant
            on which acos() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates arccosine value for the "gpa column values / 10" with help of SQLAlchemy.
        # Import func from sqlalchemy to execute acos() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> acos_func_ = func.acos(admissions_train.gpa.expression / 10)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(acos_gpa_=acos_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  acos_gpa_
        id
        5       no  3.44    Novice      Novice         0   1.219623
        34     yes  3.85  Advanced    Beginner         0   1.175589
        13      no  4.00  Advanced      Novice         1   1.159279
        40     yes  3.95    Novice    Beginner         0   1.164728
        22     yes  3.46    Novice    Beginner         0   1.217492
        19     yes  1.98  Advanced    Advanced         0   1.371479
        36      no  3.00  Advanced      Novice         0   1.266104
        15     yes  4.00  Advanced    Advanced         1   1.159279
        7      yes  2.33    Novice      Novice         1   1.335635
        17      no  3.83  Advanced    Advanced         1   1.177755
        >>>
    asin
    asin
    Functions
    asin(column_expression)
    DESCRIPTION:
        Function computes the arcsine value of an argument.
        The arcsine is the angle whose sine is the argument.
        Result values return an angle in the range -π /2 to π /2 radians, inclusive.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which asin() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates arcsine value for the "gpa column values / 10" with help of SQLAlchemy.
        # Import func from sqlalchemy to execute asin() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> asin_func_ = func.asin(admissions_train.gpa.expression / 10)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(asin_gpa_=asin_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  asin_gpa_
        id
        15     yes  4.00  Advanced    Advanced         1   0.411517
        7      yes  2.33    Novice      Novice         1   0.235161
        22     yes  3.46    Novice    Beginner         0   0.353304
        17      no  3.83  Advanced    Advanced         1   0.393042
        13      no  4.00  Advanced      Novice         1   0.411517
        38     yes  2.65  Advanced    Beginner         1   0.268204
        26     yes  3.57  Advanced    Advanced         1   0.365054
        5       no  3.44    Novice      Novice         0   0.351174
        34     yes  3.85  Advanced    Beginner         0   0.395208
        40     yes  3.95    Novice    Beginner         0   0.406068
        >>>
    atan
    atan
    Functions
    atan(column_expression)
    DESCRIPTION:
        Function computes the arctangent value of an argument.
        The arctangent is the angle whose tangent is the argument.
        Result values return an angle in the range -π /2 to π /2 radians, inclusive.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which atan() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates arctangent value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute atan() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> atan_func_ = func.atan(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(atan_gpa_=atan_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  atan_gpa_
        id
        5       no  3.44    Novice      Novice         0   1.287895
        34     yes  3.85  Advanced    Beginner         0   1.316672
        13      no  4.00  Advanced      Novice         1   1.325818
        40     yes  3.95    Novice    Beginner         0   1.322841
        22     yes  3.46    Novice    Beginner         0   1.289446
        19     yes  1.98  Advanced    Advanced         0   1.103116
        36      no  3.00  Advanced      Novice         0   1.249046
        15     yes  4.00  Advanced    Advanced         1   1.325818
        7      yes  2.33    Novice      Novice         1   1.165387
        17      no  3.83  Advanced    Advanced         1   1.315401
        >>>
    atan2
    atan2
    Functions
    atan2(x, y)
    DESCRIPTION:
        Function computes the arctangent value based on x and y coordinates provided as inputs.
        The arctangent is the angle whose tangent is the argument.
        Notes:
            1. Result values return an angle between -π and π radians, excluding -π.
            2. A positive result represents a counterclockwise angle from the x-axis.
            3. A negative result represents a clockwise angle from the x-axis.
            4. If both x and y are 0, an error is returned.
    PARAMETERS:
        x:
            Required Argument.
            Specifies the x-coordinate of a point to use in the arctangent calculation.
            Accepts a ColumnExpression of a numeric column or a numeric constant.
            Format for ColumnExpression the argument: '<dataframe>.<dataframe_column>.expression'.
        y:
            Required Argument.
            Specifies the y-coordinate of a point to use in the arctangent calculation.
            Accepts a ColumnExpression of a numeric column or a numeric constant.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        Notes:
            1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
               based on implicit type conversion rules. If an argument cannot be converted, an
               error is reported.
            2. Unsupported column types:
                a. BYTE or VARBYTE
                b. LOBs (BLOB or CLOB)
                c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates arctangent value for the "gpa" column values as x-coordinate and
        # a numeric value 1 as y-coordinate with help of SQLAlchemy.
        # Import func from sqlalchemy to execute atan2() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> atan2_func_ = func.atan2(admissions_train.gpa.expression, 1)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(atan2_gpa_=atan2_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  atan2_gpa_
        id
        5       no  3.44    Novice      Novice         0    0.282901
        34     yes  3.85  Advanced    Beginner         0    0.254125
        13      no  4.00  Advanced      Novice         1    0.244979
        40     yes  3.95    Novice    Beginner         0    0.247955
        22     yes  3.46    Novice    Beginner         0    0.281351
        19     yes  1.98  Advanced    Advanced         0    0.467680
        36      no  3.00  Advanced      Novice         0    0.321751
        15     yes  4.00  Advanced    Advanced         1    0.244979
        7      yes  2.33    Novice      Novice         1    0.405410
        17      no  3.83  Advanced    Advanced         1    0.255395
        >>>
    cos
    cos
    Functions
    cos(column_expression)
    DESCRIPTION:
        Function computes the cosine value of an argument.
        The cosine of an angle is the ratio of two sides of a right triangle.
        The ratio is the length of the side adjacent to the angle divided by
        the length of the hypotenuse.
        The cosine of argument returns values in radians in the range -1 to 1, inclusive.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which cos() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates cosine value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute cos() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> cos_func_ = func.cos(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(cos_gpa_=cos_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  cos_gpa_
        id
        15     yes  4.00  Advanced    Advanced         1 -0.653644
        7      yes  2.33    Novice      Novice         1 -0.688344
        22     yes  3.46    Novice    Beginner         0 -0.949735
        17      no  3.83  Advanced    Advanced         1 -0.772259
        13      no  4.00  Advanced      Novice         1 -0.653644
        38     yes  2.65  Advanced    Beginner         1 -0.881582
        26     yes  3.57  Advanced    Advanced         1 -0.909629
        5       no  3.44    Novice      Novice         0 -0.955806
        34     yes  3.85  Advanced    Beginner         0 -0.759399
        40     yes  3.95    Novice    Beginner         0 -0.690651
        >>>
    sin
    sin
    Functions
    sin(column_expression)
    DESCRIPTION:
        Function computes the sine value of an argument.
        The sine of an angle is the ratio of two sides of a right triangle.
        The ratio is the length of the side opposite to the angle divided
        by the length of the hypotenuse.
        The sine of argument returns values in radians in the range -1 to 1, inclusive.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which sin() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates sine value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute sin() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> sin_func_ = func.sin(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(sin_gpa_=sin_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  sin_gpa_
        id
        5       no  3.44    Novice      Novice         0 -0.293998
        34     yes  3.85  Advanced    Beginner         0 -0.650625
        13      no  4.00  Advanced      Novice         1 -0.756802
        40     yes  3.95    Novice    Beginner         0 -0.723188
        22     yes  3.46    Novice    Beginner         0 -0.313054
        19     yes  1.98  Advanced    Advanced         0  0.917438
        36      no  3.00  Advanced      Novice         0  0.141120
        15     yes  4.00  Advanced    Advanced         1 -0.756802
        7      yes  2.33    Novice      Novice         1  0.725384
        17      no  3.83  Advanced    Advanced         1 -0.635308
        >>>
    tan
    tan
    Functions
    tan(column_expression)
    DESCRIPTION:
        Function computes the tangent value of an argument.
        The tangent of an angle is the ratio of two sides of a right triangle. 
        The ratio is the length of the side opposite to the angle divided 
        by the length of the side adjacent to the angle.
        The tangent of argument returns values in radians.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant
            on which tan() is requested.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the type of the column/argument is not FLOAT, column values are converted to FLOAT
                   based on implicit type conversion rules. If an argument cannot be converted, an
                   error is reported.
                2. Unsupported column types:
                    a. BYTE or VARBYTE
                    b. LOBs (BLOB or CLOB)
                    c. CHARACTER or VARCHAR if the server character set is GRAPHIC
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example calculates tangent value for the "gpa" column with help of SQLAlchemy.
        # Import func from sqlalchemy to execute tan() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> tan_func_ = func.tan(admissions_train.gpa.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(tan_gpa_=tan_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  tan_gpa_
        id
        15     yes  4.00  Advanced    Advanced         1  1.157821
        7      yes  2.33    Novice      Novice         1 -1.053811
        22     yes  3.46    Novice    Beginner         0  0.329623
        17      no  3.83  Advanced    Advanced         1  0.822662
        13      no  4.00  Advanced      Novice         1  1.157821
        38     yes  2.65  Advanced    Beginner         1 -0.535436
        26     yes  3.57  Advanced    Advanced         1  0.456695
        5       no  3.44    Novice      Novice         0  0.307592
        34     yes  3.85  Advanced    Beginner         0  0.856763
        40     yes  3.95    Novice    Beginner         0  1.047111
        >>>
    Bit Byte Manipulation Functions
    bitand
    bitand
    Functions
    bitand(target_arg, bit_mask_arg)
    DESCRIPTION:
        Function performs the logical AND operation on the corresponding bits from
        the two input arguments.
        Function takes two bit patterns of equal length and performs the logical AND
        operation on each pair of corresponding bits. If the bits at the same position
        are both 1, then the result is 1; otherwise, the result is 0.
        If the target_arg and bit_mask_arg arguments differ in length, the arguments are
        processed as follows:
            * The target_arg and bit_mask_arg arguments are aligned on their least
              significant byte/bit.
            * The smaller argument is padded with zeros to the left until it becomes
              the same size as the larger argument.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of a numeric or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, DECIMAL, NUMBER, and VARBYTE(n)
        bit_mask_arg:
            Required Argument.
            Specifies a ColumnExpression of a numeric or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Argument Type Rules and Support:
                The data type of the bit_mask_arg parameter varies depending upon the data type
                of the target_arg parameter. The following table shows the supported column/literal
                value types for target_arg and bit_mask_arg parameters and their permitted
                combinations:
                 ===============================================
                | target_arg type | bit_mask_arg type           |
                 ===============================================
                | BYTEINT         | BYTE(1) or BYTEINT          |
                | SMALLINT        | BYTE(2) or SMALLINT         |
                | INTEGER         | BYTE(4) or INTEGER          |
                | BIGINT          | BYTE(8) or BIGINT           |
                | NUMBER(38,0)    | VARBYTE(16) or NUMBER(38,0) |
                | VARBYTE(n)      | VARBYTE(n)                  |
                 ===============================================
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example performs logical AND operation operation on:
        #   1. "id" and "byte_col" columns.
        #   2. "varbyte_col" and "byte_col" columns.
        # Import func from sqlalchemy to execute bitand function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> bit_byte_func_ = func.BITAND(bytes_table.id_col.expression, bytes_table.byte_col.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = bytes_table.assign(bitand_id_=bit_byte_func_,
        ...                         bitand_byte_=func.bitand(bytes_table.varbyte_col.expression, bytes_table.byte_col.expr
        ...                        )
        >>> print(df)
               byte_col      varbyte_col             blob_col bitand_byte_  bitand_id_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'        b'20'           0
        1         b'62'      b'62717765'  b'3331363136323633'        b'60'           0
        0         b'63'      b'627A7863'  b'3330363136323633'        b'63'           0
        >>>
    bitnot
    bitnot
    Functions
    bitnot(target_arg)
    DESCRIPTION:
        Function performs a bitwise complement on the binary representation of the input argument.
        The bitwise NOT, or complement, is a unary operation which performs logical negation on
        each bit, forming the ones' complement of the specified binary value. The digits in the
        argument which were 0 become 1, and vice versa.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example performs a bitwise complement operation on "varbyte_col" column.
        # Import func from sqlalchemy to execute bitnot function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> bit_byte_func_ = func.BITNOT(bytes_table.varbyte_col.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = bytes_table.assign(bitnot_byte_=bit_byte_func_)
        >>> print(df)
               byte_col      varbyte_col             blob_col      bitnot_byte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'  b'-616263643133'
        1         b'62'      b'62717765'  b'3331363136323633'      b'-62717766'
        0         b'63'      b'627A7863'  b'3330363136323633'      b'-627A7864'
        >>>
    bitor
    bitor
    Functions
    bitor(target_arg, bit_mask_arg)
    DESCRIPTION:
        Function performs the logical OR operation on the corresponding bits from the
        two input arguments.
        Function takes two bit patterns of equal length and performs the logical OR
        operation on each pair of corresponding bits as follows:
            * If either of the bits from the input arguments is 1 then result is 1.
            * If both of the bits from the input arguments are 0 then result is 0.
        If the target_arg and bit_mask_arg arguments differ in length, the arguments are
        processed as follows:
            * The target_arg and bit_mask_arg arguments are aligned on their least
              significant byte/bit.
            * The smaller argument is padded with zeros to the left until it becomes the
              same size as the larger argument.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
        bit_mask_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Argument Type Rules and Support:
                The data type of the bit_mask_arg parameter varies depending upon the data type
                of the target_arg parameter. The following table shows the supported column/literal
                value types for target_arg and bit_mask_arg parameters and their permitted
                combinations:
                 ===============================================
                | target_arg type | bit_mask_arg type           |
                 ===============================================
                | BYTEINT         | BYTE(1) or BYTEINT          |
                | SMALLINT        | BYTE(2) or SMALLINT         |
                | INTEGER         | BYTE(4) or INTEGER          |
                | BIGINT          | BYTE(8) or BIGINT           |
                | VARBYTE(n)      | VARBYTE(n)                  |
                 ===============================================
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example performs bitor operation on "id" and "byte_col" columns.
        # Import func from sqlalchemy to execute bitor function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> bit_byte_func_ = func.Bitor(bytes_table.id_col.expression, bytes_table.byte_col.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = bytes_table.assign(bitor_byte_=bit_byte_func_)
        >>> print(df)
               byte_col      varbyte_col             blob_col  bitor_byte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'   1627389954
        1         b'62'      b'62717765'  b'3331363136323633'   1644167169
        0         b'63'      b'627A7863'  b'3330363136323633'   1660944384
        >>>
    bitxor
    bitxor
    Functions
    bitxor(target_arg, bit_mask_arg)
    DESCRIPTION:
        Function performs a bitwise XOR operation on the binary representation of the
        two input arguments.
        Function takes two bit patterns of equal length and performs the exclusive OR
        operation on each pair of corresponding bits as follows:
            * The result in each position is 1 if the two bits are different.
            * The result in each position is 0 if the two bits are same.
        If the target_arg and bit_mask_arg arguments differ in length, the arguments
        are processed as follows:
            * The target_arg and bit_mask_arg arguments are aligned on their least
              significant byte/bit.
            * The smaller argument is padded with zeros to the left until it becomes
              the same size as the larger argument.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
        bit_mask_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Argument Type Rules and Support:
                The data type of the bit_mask_arg parameter varies depending upon the data type
                of the target_arg parameter. The following table shows the supported column/literal
                value types for target_arg and bit_mask_arg parameters and their permitted
                combinations:
                 ===============================================
                | target_arg type | bit_mask_arg type           |
                 ===============================================
                | BYTEINT         | BYTE(1) or BYTEINT          |
                | SMALLINT        | BYTE(2) or SMALLINT         |
                | INTEGER         | BYTE(4) or INTEGER          |
                | BIGINT          | BYTE(8) or BIGINT           |
                | VARBYTE(n)      | VARBYTE(n)                  |
                 ===============================================
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example performs bitxor operation on "id" and "byte_col" columns.
        # Import func from sqlalchemy to execute bitxor function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> bit_byte_func_ = func.BITXOR(bytes_table.id_col.expression, bytes_table.byte_col.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = bytes_table.assign(bitxor_int_byte_= bit_byte_func_)
        >>> print(df)
               byte_col      varbyte_col             blob_col  bitxor_int_byte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'        1627389954
        1         b'62'      b'62717765'  b'3331363136323633'        1644167169
        0         b'63'      b'627A7863'  b'3330363136323633'        1660944384
        >>>
    countset
    countset
    Functions
    countset(target_arg, target_arg_value)
    DESCRIPTION:
        Function returns the count of the binary bits within the target_arg expression
        that are either set to 1 or set to 0, depending on the target_arg_value value.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
        target_arg_value:
            Optional Argument.
            Specifies a ColumnExpression of an integer column or an integer literal representing
            a binary bit value, 0 or 1, to be counted within the target_arg expression.
            If target_arg_value is not specified, the default is 1.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example counts number of 0's and 1's in within values of "varbyte_col" column.
        # Import func from sqlalchemy to execute countset function.
        >>> from sqlalchemy import func
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> df = bytes_table.assign(count_1s_varbyte_= func.COUNTSET(bytes_table.varbyte_col.expression),
        ...                         count_0s_varbyte_= func.countset(bytes_table.varbyte_col.expression, 0)
        ...                        )
        >>>
        >>> print(df)
               byte_col      varbyte_col             blob_col  count_0s_varbyte_  count_1s_varbyte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'                 29                 19
        1         b'62'      b'62717765'  b'3331363136323633'                 15                 17
        0         b'63'      b'627A7863'  b'3330363136323633'                 16                 16
        >>>
    getbit
    getbit
    Functions
    getbit(target_arg, target_bit_arg)
    DESCRIPTION:
        Function returns the bit specified by target_bit_arg from the target_arg byte
        expression and returns either 0 or 1 to indicate the value of that bit.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
        target_bit_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal.
            Following rules apply to the values of target_bit_arg:
                * The range of input values can vary from 0 (bit 0 is the least significant
                  bit) to the (sizeof(target_arg) - 1).
                * If target_bit_arg is negative or out-of-range (meaning that it exceeds the size
                  of target_arg), an error is returned.
                * If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example retrieves the fourth bit from the values in "varbyte_col" column.
        # Import func from sqlalchemy to execute getbit function.
        >>> from sqlalchemy import func
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> df = bytes_table.assign(getbit_varbyte_= func.getbit(bytes_table.varbyte_col.expression, 3))
        >>> print(df)
               byte_col      varbyte_col             blob_col  getbit_varbyte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'                  0
        1         b'62'      b'62717765'  b'3331363136323633'                  0
        0         b'63'      b'627A7863'  b'3330363136323633'                  0
        >>>
    rotateleft
    rotateleft
    Functions
    rotateleft(target_arg, num_bits_arg)
    DESCRIPTION:
        Function returns an expression rotated to the left by the specified number of bits,
        with the most significant bits wrapping around to the right.
        Following rules apply to the function:
            * If num_bits_arg is equal to zero, then the function returns target_arg
              unchanged.
            * If num_bits_arg is negative, then the function rotates the bits to the right
              instead of the left.
            * If target_arg and/or num_bits_arg are NULL, then the function returns NULL.
            * If num_bits_arg is larger than the size of target_arg, then the function rotates
              (num_bits_arg MOD sizeof(target_arg)) bits. The scope of the rotation operation
              is bounded by the size of the target_arg expression.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
            Note:
                When operating against an integer value (BYTEINT, SMALLINT, INTEGER, or BIGINT),
                rotating a bit into the most significant position will result in the integer
                becoming negative. This is because all integers in Vantage are signed integers.
        num_bits_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the number of bit positions to rotate.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example rotates values in "varbyte_col" column to left by 3 bits.
        # Import func from sqlalchemy to execute rotateleft function.
        >>> from sqlalchemy import func
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> df = bytes_table.assign(rotateleft_varbyte_= func.rotateleft(bytes_table.varbyte_col.expression, 3))
        >>> print(df)
               byte_col      varbyte_col             blob_col rotateleft_varbyte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'      b'B131B218993'
        1         b'62'      b'62717765'  b'3331363136323633'         b'138BBB2B'
        0         b'63'      b'627A7863'  b'3330363136323633'         b'13D3C31B'
        >>>
    rotateright
    rotateright
    Functions
    rotateright(target_arg, num_bits_arg)
    DESCRIPTION:
        Function returns an expression rotated to the right by the specified number of bits,
        with the least significant bits wrapping around to the left.
        Following rules apply to the function:
            * If num_bits_arg is equal to zero, then the function returns target_arg
              unchanged.
            * If num_bits_arg is negative, then the function rotates the bits to the left
              instead of the right.
            * If target_arg and/or num_bits_arg are NULL, then the function returns NULL.
            * If num_bits_arg is larger than the size of target_arg, then the function rotates
              (num_bits_arg MOD sizeof(target_arg)) bits. The scope of the rotation operation
              is bounded by the size of the target_arg expression.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
            Note:
                When operating against an integer value (BYTEINT, SMALLINT, INTEGER, or BIGINT),
                rotating a bit into the most significant position will result in the integer
                becoming negative. This is because all integers in Vantage are signed integers.
        num_bits_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the number of bit positions to rotate.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example rotates values in "varbyte_col" column to right by 3 bits.
        # Import func from sqlalchemy to execute rotateright function.
        >>> from sqlalchemy import func
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> df = bytes_table.assign(rotateright_varbyte_= func.rotateright(bytes_table.varbyte_col.expression, 3))
        >>> print(df)
               byte_col      varbyte_col             blob_col rotateright_varbyte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'      b'4C2C4C6C8626'
        1         b'62'      b'62717765'  b'3331363136323633'         b'-53B1D114'
        0         b'63'      b'627A7863'  b'3330363136323633'          b'6C4F4F0C'
        >>>
    setbit
    setbit
    Functions
    setbit(target_arg, target_bit_arg, target_value_arg)
    DESCRIPTION:
        Function sets the value of the bit specified by target_bit_arg to the value
        of target_value_arg in the target_arg byte expression.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
        target_bit_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal.
            Following rules apply to the values of target_bit_arg:
                * The range of input values can vary from 0 (bit 0 is the least significant
                  bit) to the (sizeof(target_arg) - 1).
                * If target_bit_arg is negative or out-of-range (meaning that it exceeds the size
                  of target_arg), an error is returned.
                * If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
        target_value_arg:
            Optional Argument.
            Specifies a ColumnExpression of an integer column or an integer literal representing
            a binary bit value, 0 or 1, to be set within the target_arg expression.
            If target_value_arg is not specified, the default is 1.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example set the fourth bit from the values in "varbyte_col" column with "1".
        # Import func from sqlalchemy to execute setbit function.
        >>> from sqlalchemy import func
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> df = bytes_table.assign(setbit_varbyte_= func.setbit(bytes_table.varbyte_col.expression, 3))
        >>> print(df)
               byte_col      varbyte_col             blob_col  setbit_varbyte_
        id_col
        0         b'63'      b'627A7863'  b'3330363136323633'      b'627A786B'
        2         b'61'  b'616263643132'  b'6162636431323233'  b'61626364313A'
        1         b'62'      b'62717765'  b'3331363136323633'      b'6271776D'
        >>>
    shiftleft
    shiftleft
    Functions
    shiftleft(target_arg, num_bits_arg)
    DESCRIPTION:
        Function returns the expression target_arg shifted by the specified
        number of bits (num_bits_arg) to the left. The bits in the most significant
        positions are lost, and the bits in the least significant positions are
        filled with zeros.
        Following rules apply to the function:
            * If num_bits_arg is equal to zero, then the function returns target_arg
              unchanged.
            * If num_bits_arg is negative, then the function shifts the bits to the right
              instead of the left.
            * If target_arg and/or num_bits_arg are NULL, then the function returns NULL.
            * The scope of the shift operation is bounded by the size of the target_arg
              expression. Specifying a shift that is outside the range of target_arg
              results in an error.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
            Note:
                When operating against an integer value (BYTEINT, SMALLINT, INTEGER, or
                BIGINT), shifting a bit into the most significant position will result in
                the integer becoming negative. This is because all integers in Vantage are
                signed integers.
        num_bits_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the number of bit positions to shift.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example shifts values in "varbyte_col" column to left by 3 bits.
        # Import func from sqlalchemy to execute shiftleft function.
        >>> from sqlalchemy import func
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> df = bytes_table.assign(shiftleft_varbyte_= func.shiftleft(bytes_table.varbyte_col.expression, 3))
        >>> print(df)
               byte_col      varbyte_col             blob_col shiftleft_varbyte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'     b'B131B218990'
        1         b'62'      b'62717765'  b'3331363136323633'        b'138BBB28'
        0         b'63'      b'627A7863'  b'3330363136323633'        b'13D3C318'
        >>>
    shiftright
    shiftright
    Functions
    shiftright(target_arg, num_bits_arg)
    DESCRIPTION:
        Function returns the expression target_arg shifted by the specified
        number of bits (num_bits_arg) to the right. The bits in the least significant
        positions are lost, and the bits in the most significant positions are
        filled with zeros.
        Following rules apply to the function:
            * If num_bits_arg is equal to zero, then the function returns target_arg
              unchanged.
            * If num_bits_arg is negative, then the function shifts the bits to the left
              instead of the right.
            * If target_arg and/or num_bits_arg are NULL, then the function returns NULL.
            * The scope of the shift operation is bounded by the size of the target_arg
              expression. Specifying a shift that is outside the range of target_arg
              results in an SQL error.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
            Note:
                When operating against an integer value (BYTEINT, SMALLINT, INTEGER, or
                BIGINT), shifting a bit into the most significant position will result in
                the integer becoming negative. This is because all integers in Vantage are
                signed integers.
        num_bits_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the number of bit positions to shift.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example shifts values in "varbyte_col" column to right by 3 bits.
        # Import func from sqlalchemy to execute shiftright function.
        >>> from sqlalchemy import func
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> df = bytes_table.assign(shiftright_varbyte_= func.shiftright(bytes_table.varbyte_col.expression, 3))
        >>> print(df)
               byte_col      varbyte_col             blob_col shiftright_varbyte_
        id_col
        0         b'63'      b'627A7863'  b'3330363136323633'          b'C4F4F0C'
        2         b'61'  b'616263643132'  b'6162636431323233'      b'C2C4C6C8626'
        1         b'62'      b'62717765'  b'3331363136323633'          b'C4E2EEC'
        >>>
    subbitstr
    subbitstr
    Functions
    subbitstr(target_arg, position_arg, num_bits_arg)
    DESCRIPTION:
        Function extracts a bit substring from the target_arg input expression
        based on the specified bit position. Function returns a VARBYTE string,
        thus resulting number of bits returned are rounded to the byte boundary
        greater than the number of bits requested.
        The bits returned are right-justified, and the excess bits (those exceeding
        the requested number of bits) are filled with zeros.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer or byte column or a numeric or byte
            literal value.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, BIGINT, and VARBYTE(n)
        position_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the starting position of the bit substring to be extracted.
            If position_arg is negative or out-of-range (meaning that it exceeds
            the size of target_arg), an error is returned.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
        num_bits_arg:
            Required Argument.
            Specifies a ColumnExpression of an integer column or an integer literal
            indicating the length of the bit substring to be extracted. This
            specifies the number of bits for the function to return.
            If num_bits_arg is negative, or is greater than the number of bits
            remaining after the starting position_arg is taken into account, an error
            is returned.
            If the argument is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: INTEGER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example requests that 3 bits be returned starting at the third bit
        # from values in "varbyte_col" column.
        # Import func from sqlalchemy to execute subbitstr function.
        >>> from sqlalchemy import func
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> df = bytes_table.assign(subbitstr_varbyte_= func.subbitstr(bytes_table.varbyte_col.expression, 2, 3))
        >>> print(df)
               byte_col      varbyte_col             blob_col subbitstr_varbyte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'               b'4'
        1         b'62'      b'62717765'  b'3331363136323633'               b'1'
        0         b'63'      b'627A7863'  b'3330363136323633'               b'0'
        >>>
    to_byte
    to_byte
    Functions
    to_byte(target_arg)
    DESCRIPTION:
        Function converts a numeric data type to the Vantage byte representation
        (byte value) of the input value.
    PARAMETERS:
        target_arg:
            Required Argument.
            Specifies a ColumnExpression of an numeric column or a numeric literal value.
            If target_arg is NULL, the function returns NULL.
            Format of a ColumnExpression of a column: '<dataframe>.<dataframe_column>.expression'.
            Supported SQL column types: BYTEINT, SMALLINT, INTEGER, and BIGINT
            The size of the byte string returned varies according to the data type of the
            target_arg input argument as shown below:
             ==================================================
            | IF target_arg type is | THEN the result type is  |
             ==================================================
            | BYTEINT               | BYTE(1)                  |
            | SMALLINT              | BYTE(2)                  |
            | INTEGER               | BYTE(4)                  |
            | BIGINT                | BYTE(8)                  |
            ===================================================
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        0         b'63'      b'627A7863'  b'3330363136323633'
        >>>
        # Example converts values in "id_col" to bytes.
        # Import func from sqlalchemy to execute to_byte function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: Function name case does not matter. We can use the function name in
        #       lower case, upper case or mixed case.
        >>> bit_byte_func_ = func.to_byte(bytes_table.id_col.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = bytes_table.assign(id_col_to_byte_=bit_byte_func_)
        >>> print(df)
               byte_col      varbyte_col             blob_col id_col_to_byte_
        id_col
        2         b'61'  b'616263643132'  b'6162636431323233'            b'2'
        1         b'62'      b'62717765'  b'3331363136323633'            b'1'
        0         b'63'      b'627A7863'  b'3330363136323633'            b'0'
        >>>
    Date and Time Functions
    current_date
    current_date
    Functions
    current_date()
    DESCRIPTION:
        Function returns the current date at the time when the request started.
        If it is invoked more than once during the request, the same date is returned.
        The date returned does not change during the duration of the request.
        The value returned depends on the setting of the DBS Control flag TimeDateWZControl
        as follows:
            * If the TimeDateWZControl flag is enabled, function returns a date constructed
              from the session time and session time zone.
            * If the TimeDateWZControl flag is disabled, function returns a date constructed
              from the time value local to the Vantage server and the session time
              zone.
        For more information, see "DBS Control (dbscontrol)" in Teradata Vantage™ - Database Utilities , B035-1102 .
    PARAMETERS:
        None.
    ALTERNATE NAME:
        curdate
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute current_date() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> current_date_ = func.current_date()
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(current_date_col=current_date_)
        >>> print(df)
           masters   gpa     stats programming  admitted current_date_col
        id
        13      no  4.00  Advanced      Novice         1         20/07/28
        26     yes  3.57  Advanced    Advanced         1         20/07/28
        5       no  3.44    Novice      Novice         0         20/07/28
        19     yes  1.98  Advanced    Advanced         0         20/07/28
        15     yes  4.00  Advanced    Advanced         1         20/07/28
        40     yes  3.95    Novice    Beginner         0         20/07/28
        7      yes  2.33    Novice      Novice         1         20/07/28
        22     yes  3.46    Novice    Beginner         0         20/07/28
        36      no  3.00  Advanced      Novice         0         20/07/28
        38     yes  2.65  Advanced    Beginner         1         20/07/28
        >>>
        # "curdate" can be used as an alternative function name.
        >>> current_date_ = func.curdate()
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(current_date_col=current_date_)
        >>> print(df)
           masters   gpa     stats programming  admitted current_date_col
        id
        5       no  3.44    Novice      Novice         0         20/07/28
        7      yes  2.33    Novice      Novice         1         20/07/28
        22     yes  3.46    Novice    Beginner         0         20/07/28
        19     yes  1.98  Advanced    Advanced         0         20/07/28
        15     yes  4.00  Advanced    Advanced         1         20/07/28
        17      no  3.83  Advanced    Advanced         1         20/07/28
        34     yes  3.85  Advanced    Beginner         0         20/07/28
        13      no  4.00  Advanced      Novice         1         20/07/28
        36      no  3.00  Advanced      Novice         0         20/07/28
        40     yes  3.95    Novice    Beginner         0         20/07/28
        >>>
    current_time
    current_time
    Functions
    current_time(fractional_precision)
    DESCRIPTION:
        Function returns the current time when the request started.
        If function is invoked more than once during the request, the same time is returned.
        The time returned does not change during the duration of the request.
        The value returned depends on the setting of the DBS Control flag TimeDateWZControl
        as follows:
            * If the TimeDateWZControl flag is enabled, function returns a time constructed
              from the session time and session time zone.
            * If the TimeDateWZControl flag is disabled, function returns a time constructed
              from the time value local to the Vantage server and the session time
              zone.
    PARAMETERS:
        fractional_precision:
            Optional Argument.
            Specifies a precision range for the returned value. The valid range
            is 0 through 6. The default is 0.
    NOTE:
        Function accepts positional arguments only.
    ALTERNATE NAME:
        curtime
        Note:
            The function with this alternate name does not accept 'fractional_precision' value.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute current_time() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> current_time_ = func.current_time()
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(current_time_col=current_time_)
        >>> print(df)
           masters   gpa     stats programming  admitted current_time_col
        id
        13      no  4.00  Advanced      Novice         1   02:32:17+00:00
        36      no  3.00  Advanced      Novice         0   02:32:17+00:00
        15     yes  4.00  Advanced    Advanced         1   02:32:17+00:00
        40     yes  3.95    Novice    Beginner         0   02:32:17+00:00
        22     yes  3.46    Novice    Beginner         0   02:32:17+00:00
        38     yes  2.65  Advanced    Beginner         1   02:32:17+00:00
        26     yes  3.57  Advanced    Advanced         1   02:32:17+00:00
        5       no  3.44    Novice      Novice         0   02:32:17+00:00
        7      yes  2.33    Novice      Novice         1   02:32:17+00:00
        19     yes  1.98  Advanced    Advanced         0   02:32:17+00:00
        >>>
        # Get the current time with fractional precision value as 3.
        >>> df = admissions_train.assign(current_time_col=func.current_time(3))
        >>> print(df)
           masters   gpa     stats programming  admitted    current_time_col
        id
        13      no  4.00  Advanced      Novice         1  02:50:59.570+00:00
        26     yes  3.57  Advanced    Advanced         1  02:50:59.570+00:00
        5       no  3.44    Novice      Novice         0  02:50:59.570+00:00
        19     yes  1.98  Advanced    Advanced         0  02:50:59.570+00:00
        15     yes  4.00  Advanced    Advanced         1  02:50:59.570+00:00
        40     yes  3.95    Novice    Beginner         0  02:50:59.570+00:00
        7      yes  2.33    Novice      Novice         1  02:50:59.570+00:00
        22     yes  3.46    Novice    Beginner         0  02:50:59.570+00:00
        36      no  3.00  Advanced      Novice         0  02:50:59.570+00:00
        38     yes  2.65  Advanced    Beginner         1  02:50:59.570+00:00
        >>>
        # "curtime" can be used as an alternative function name.
        >>> current_time_ = func.curtime()
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(current_time_col=current_time_)
        >>> print(df)
           masters   gpa     stats programming  admitted current_time_col
        id
        5       no  3.44    Novice      Novice         0         02:32:47
        34     yes  3.85  Advanced    Beginner         0         02:32:47
        13      no  4.00  Advanced      Novice         1         02:32:47
        40     yes  3.95    Novice    Beginner         0         02:32:47
        22     yes  3.46    Novice    Beginner         0         02:32:47
        19     yes  1.98  Advanced    Advanced         0         02:32:47
        36      no  3.00  Advanced      Novice         0         02:32:47
        15     yes  4.00  Advanced    Advanced         1         02:32:47
        7      yes  2.33    Novice      Novice         1         02:32:47
        17      no  3.83  Advanced    Advanced         1         02:32:47
        >>>
    current_timestamp
    current_timestamp
    Functions
    current_timestamp(fractional_precision)
    DESCRIPTION:
        Function returns the current timestamp when the request started.
        If function is invoked more than once during the request, the same timestamp is returned.
        The timestamp returned does not change during the duration of the request.
        The value returned depends on the setting of the DBS Control flag TimeDateWZControl as follows:
            * If the TimeDateWZControl flag is enabled, function returns a timestamp constructed from
              the session time and session time zone.
            * If the TimeDateWZControl flag is disabled, function returns a timestamp constructed from
              the time value local to the Vantage server and the session time zone.
    PARAMETERS:
        fractional_precision:
            Optional Argument.
            Specifies a precision range for the returned value. The valid range
            is 0 through 6. The default is 0.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute current_timestamp() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> current_timestamp_ = func.current_timestamp()
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(current_timestamp_col=current_timestamp_)
        >>> print(df)
           masters   gpa     stats programming  admitted current_timestamp_col
        id
        13      no  4.00  Advanced      Novice         1   02:32:17+00:00
        36      no  3.00  Advanced      Novice         0   02:32:17+00:00
        15     yes  4.00  Advanced    Advanced         1   02:32:17+00:00
        40     yes  3.95    Novice    Beginner         0   02:32:17+00:00
        22     yes  3.46    Novice    Beginner         0   02:32:17+00:00
        38     yes  2.65  Advanced    Beginner         1   02:32:17+00:00
        26     yes  3.57  Advanced    Advanced         1   02:32:17+00:00
        5       no  3.44    Novice      Novice         0   02:32:17+00:00
        7      yes  2.33    Novice      Novice         1   02:32:17+00:00
        19     yes  1.98  Advanced    Advanced         0   02:32:17+00:00
        >>>
        # Get current timestamp value with fractional precision set to 6.
        >>> df = admissions_train.assign(current_timestamp_col=func.current_timestamp(6))
        >>> print(df)
           masters   gpa     stats programming  admitted             current_timestamp_col
        id
        5       no  3.44    Novice      Novice         0  2020-07-28 02:55:07.140000+00:00
        34     yes  3.85  Advanced    Beginner         0  2020-07-28 02:55:07.140000+00:00
        13      no  4.00  Advanced      Novice         1  2020-07-28 02:55:07.140000+00:00
        40     yes  3.95    Novice    Beginner         0  2020-07-28 02:55:07.140000+00:00
        22     yes  3.46    Novice    Beginner         0  2020-07-28 02:55:07.140000+00:00
        19     yes  1.98  Advanced    Advanced         0  2020-07-28 02:55:07.140000+00:00
        36      no  3.00  Advanced      Novice         0  2020-07-28 02:55:07.140000+00:00
        15     yes  4.00  Advanced    Advanced         1  2020-07-28 02:55:07.140000+00:00
        7      yes  2.33    Novice      Novice         1  2020-07-28 02:55:07.140000+00:00
        17      no  3.83  Advanced    Advanced         1  2020-07-28 02:55:07.140000+00:00
        >>>
    Hash-Related Functions
    hashamp
    hashamp
    Functions
    hashamp(arg_expression)
    DESCRIPTION:
        Function finds the primary AMP corresponding to the hash bucket number specified
        in the arg_expression and returns the AMP ID. If no hash bucket number is specified,
        function returns one less than the maximum number of AMPs in the system.
    PARAMETERS:
        arg_expression:
            Optional Argument.
            Specifies an expression that evaluates to a valid hash bucket number.
            Supported Types/Values:
                * ColumnExpression, i.e., SQLAlchemy Column -
                  Format for the argument: '<dataframe>.<dataframe_column>.expression'.
                * A Literal Value.
                * SQLAlchemy TextClause
                    Specifies a TextClause object that is generated by passing a string
                    to 'text()' function from SQLAlchemy.
                    Following are the supported String Formats that can be passed to
                    text() function:
                        * Contiguous Map:
                          Format: MAP = Contiguousmap_name
                          For example,
                            text("MAP = " + Contiguousmap_name)
                        * ColumnExpression with Contiguous Map:
                          Format: ColumnExpression/literal value MAP = Contiguousmap_name
                          For example,
                            text(str(ColumnExpression) + " MAP = " + Contiguousmap_name)
                        * ColumnExpression with Sparse Map:
                          Format: ColumnExpression MAP = sparsemap_name COLOCATE USING = colocation_name
                          For example,
                            text(str(ColumnExpression) + " MAP = " + sparsemap_name + " COLOCATE USING = " + colocation_na
                    where,
                        MAP - Specifies an object that specifies which AMPs store the rows of a table.
                        Contiguousmap_name - Specifies the name of the contiguous map, the map that
                                             includes all AMPs within a specified range.
                        sparsemap_name - Specifies the name of the sparse map, the map that includes a
                                         subset of AMPs from a contiguous map.
                        COLOCATE USING - Specifies a clause that forces tables that use the same sparse
                                         map to be stored on the same subset of AMPs.
                                         COLOCATE USING is required with a sparse map. It cannot be used
                                         with a contiguous map.
                        colocation_name - Specifies the colocation name, usually databasename_tablename.
    NOTES:
        1. Function accepts positional arguments only.
        2. If no argument is passed to hashamp(), function returns an INTEGER that is one less than the
           maximum number of AMPs in the system.
        3. If ColumnExpression is not specified:
            a. For a  contiguous map: The function returns an INTEGER representing the highest AMP number
                                      in the specified or default contiguous map. For a contiguous map
                                      starting at AMP zero, adding one to the result gives the total number
                                      of AMPs in the contiguous map.
            b. For a sparse map: Expression must be specified for a sparse map.
        4. If ColumnExpression is specified, the function returns the ID of the primary AMP corresponding
           to the hash bucket number specified in ColumnExpression, based on the specified or default map.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute hashamp function.
        >>> from sqlalchemy import func
        # Example 1: Executing hashamp() function without argument to return primary AMP number.
        # Create a sqlalchemy Function object.
        >>> hashamp_func_ = func.hashamp()
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashamp_=hashamp_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  hashamp_
        id
        13      no  4.00  Advanced      Novice         1         3
        26     yes  3.57  Advanced    Advanced         1         3
        5       no  3.44    Novice      Novice         0         3
        19     yes  1.98  Advanced    Advanced         0         3
        15     yes  4.00  Advanced    Advanced         1         3
        40     yes  3.95    Novice    Beginner         0         3
        7      yes  2.33    Novice      Novice         1         3
        22     yes  3.46    Novice    Beginner         0         3
        36      no  3.00  Advanced      Novice         0         3
        38     yes  2.65  Advanced    Beginner         1         3
        >>>
        # Example 2: Executing hashamp() function to return primary AMP number corresponding
        #            to values in "id" column.
        # Create a sqlalchemy Function object.
        >>> hashamp_func_ = func.hashamp(admissions_train.id.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashamp_id_=hashamp_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  hashamp_id_
        id
        5       no  3.44    Novice      Novice         0            0
        7      yes  2.33    Novice      Novice         1            2
        22     yes  3.46    Novice    Beginner         0            1
        19     yes  1.98  Advanced    Advanced         0            2
        15     yes  4.00  Advanced    Advanced         1            3
        17      no  3.83  Advanced    Advanced         1            0
        34     yes  3.85  Advanced    Beginner         0            1
        13      no  4.00  Advanced      Novice         1            2
        36      no  3.00  Advanced      Novice         0            3
        40     yes  3.95    Novice    Beginner         0            1
        >>>
        # Example 3: Executing hashamp() function to return primary AMP number using
        #            contiguous map.
        # To use contiguous map, we must import 'text' from sqlalchemy.
        >>> from sqlalchemy import text
        # Create a sqlalchemy Function object.
        >>> hashamp_func_ = func.hashamp(text("MAP = TD_Map1"))
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashamp_contiguous_map_=hashamp_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  hashamp_contiguous_map_
        id
        13      no  4.00  Advanced      Novice         1                        3
        26     yes  3.57  Advanced    Advanced         1                        3
        5       no  3.44    Novice      Novice         0                        3
        19     yes  1.98  Advanced    Advanced         0                        3
        15     yes  4.00  Advanced    Advanced         1                        3
        40     yes  3.95    Novice    Beginner         0                        3
        7      yes  2.33    Novice      Novice         1                        3
        22     yes  3.46    Novice    Beginner         0                        3
        36      no  3.00  Advanced      Novice         0                        3
        38     yes  2.65  Advanced    Beginner         1                        3
        >>>
        # Example 4: Executing hashamp() function to return primary AMP number using
        #            contiguous map and using values from "id" column.
        # To use contiguous map, we must import 'text' from sqlalchemy.
        >>> from sqlalchemy import text
        # Create a sqlalchemy Function object.
        >>> hashamp_func_ = func.hashamp(text(str(admissions_train.id.expression) + " MAP = TD_Map1"))
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashamp_id_col_contiguous_map_=hashamp_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  hashamp_id_col_contiguous_map_
        id
        13      no  4.00  Advanced      Novice         1                               2
        26     yes  3.57  Advanced    Advanced         1                               3
        5       no  3.44    Novice      Novice         0                               0
        19     yes  1.98  Advanced    Advanced         0                               2
        15     yes  4.00  Advanced    Advanced         1                               3
        40     yes  3.95    Novice    Beginner         0                               1
        7      yes  2.33    Novice      Novice         1                               2
        22     yes  3.46    Novice    Beginner         0                               1
        36      no  3.00  Advanced      Novice         0                               3
        38     yes  2.65  Advanced    Beginner         1                               3
        >>>
    hashbakamp
    hashbakamp
    Functions
    hashbakamp(arg_expression)
    DESCRIPTION:
        Function finds the fallback AMP corresponding to the hash bucket number specified
        in the expression and returns the AMP ID. If no hash bucket is specified, function
        returns one less than the maximum number of fallback AMPs in the system.
    PARAMETERS:
        arg_expression:
            Optional Argument.
            Specifies an expression that evaluates to a valid hash bucket number.
            Supported Types/Values:
                * ColumnExpression, i.e., SQLAlchemy Column -
                  Format for the argument: '<dataframe>.<dataframe_column>.expression'.
                * A Literal Value.
                * SQLAlchemy TextClause
                    Specifies a TextClause object that is generated by passing a string
                    to 'text()' function from SQLAlchemy.
                    Following are the supported String Formats that can be passed to
                    text() function:
                        * Contiguous Map:
                          Format: MAP = Contiguousmap_name
                          For example,
                            text("MAP = " + Contiguousmap_name)
                        * ColumnExpression with Contiguous Map:
                          Format: ColumnExpression/literal value MAP = Contiguousmap_name
                          For example,
                            text(str(ColumnExpression) + " MAP = " + Contiguousmap_name)
                        * ColumnExpression with Sparse Map:
                          Format: ColumnExpression MAP = sparsemap_name COLOCATE USING = colocation_name
                          For example,
                            text(str(ColumnExpression) + " MAP = " + sparsemap_name + " COLOCATE USING = " + colocation_na
                    where,
                        MAP - Specifies an object that specifies which AMPs store the rows of a table.
                        Contiguousmap_name - Specifies the name of the contiguous map, the map that
                                             includes all AMPs within a specified range.
                        sparsemap_name - Specifies the name of the sparse map, the map that includes a
                                         subset of AMPs from a contiguous map.
                        COLOCATE USING - Specifies a clause that forces tables that use the same sparse
                                         map to be stored on the same subset of AMPs.
                                         COLOCATE USING is required with a sparse map. It cannot be used
                                         with a contiguous map.
                        colocation_name - Specifies the colocation name, usually databasename_tablename.
    NOTES:
        1. Function accepts positional arguments only.
        2. If no argument is passed to hashbakamp(), function returns an INTEGER that is one less than the
           maximum number of AMPs in the system.
        3. If ColumnExpression is not specified:
            a. For a  contiguous map: The function returns an INTEGER representing the highest fallback
                                      AMP number in the specified or default contiguous map. For a contiguous
                                      map starting at AMP zero, adding one to the result gives the total number
                                      of AMPs in the contiguous map.
            b. For a sparse map: Expression must be specified for a sparse map.
        4. If ColumnExpression is specified, the function returns the ID of the fallback AMP corresponding
           to the hash bucket number specified in ColumnExpression, based on the specified or default map.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute hashbakamp() function.
        >>> from sqlalchemy import func
        # Example 1: Executing hashbakamp() function without argument to return fallback AMP number.
        # Create a sqlalchemy Function object.
        >>> hashbakamp_func_ = func.hashbakamp()
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashbakamp_=hashbakamp_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted hashbakamp_
        id
        13      no  4.00  Advanced      Novice         1           3
        26     yes  3.57  Advanced    Advanced         1           3
        5       no  3.44    Novice      Novice         0           3
        19     yes  1.98  Advanced    Advanced         0           3
        15     yes  4.00  Advanced    Advanced         1           3
        40     yes  3.95    Novice    Beginner         0           3
        7      yes  2.33    Novice      Novice         1           3
        22     yes  3.46    Novice    Beginner         0           3
        36      no  3.00  Advanced      Novice         0           3
        38     yes  2.65  Advanced    Beginner         1           3
        >>>
        # Example 2: Executing hashbakamp() function to return fallback AMP number corresponding
        #            to values in "id" column.
        # Create a sqlalchemy Function object.
        >>> hashbakamp_func_ = func.hashbakamp(admissions_train.id.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashbakamp_id_=hashbakamp_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted hashbakamp_id_
        id
        5       no  3.44    Novice      Novice         0              2
        7      yes  2.33    Novice      Novice         1              0
        22     yes  3.46    Novice    Beginner         0              3
        19     yes  1.98  Advanced    Advanced         0              0
        15     yes  4.00  Advanced    Advanced         1              1
        17      no  3.83  Advanced    Advanced         1              2
        34     yes  3.85  Advanced    Beginner         0              3
        13      no  4.00  Advanced      Novice         1              0
        36      no  3.00  Advanced      Novice         0              1
        40     yes  3.95    Novice    Beginner         0              3
        >>>
        # Example 3: Executing hashbakamp() function to return fallback AMP number using
        #            contiguous map.
        # To use contiguous map, we must import 'text' from sqlalchemy.
        >>> from sqlalchemy import text
        # Create a sqlalchemy Function object.
        >>> hashbakamp_func_ = func.hashbakamp(text("MAP = TD_Map1"))
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashbakamp_contiguous_map_=hashbakamp_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted hashbakamp_contiguous_map_
        id
        5       no  3.44    Novice      Novice         0                          3
        34     yes  3.85  Advanced    Beginner         0                          3
        13      no  4.00  Advanced      Novice         1                          3
        40     yes  3.95    Novice    Beginner         0                          3
        22     yes  3.46    Novice    Beginner         0                          3
        19     yes  1.98  Advanced    Advanced         0                          3
        36      no  3.00  Advanced      Novice         0                          3
        15     yes  4.00  Advanced    Advanced         1                          3
        7      yes  2.33    Novice      Novice         1                          3
        17      no  3.83  Advanced    Advanced         1                          3
        >>>
        # Example 4: Executing hashbakamp() function to return fallback AMP number using
        #            contiguous map and using values from "id" column.
        # To use contiguous map, we must import 'text' from sqlalchemy.
        >>> from sqlalchemy import text
        # Create a sqlalchemy Function object.
        >>> hashbakamp_func_ = func.hashbakamp(text(str(admissions_train.id.expression) + " MAP = TD_Map1"))
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashbakamp_id_col_contiguous_map_=hashbakamp_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted hashbakamp_id_col_contiguous_map_
        id
        13      no  4.00  Advanced      Novice         1                                 0
        26     yes  3.57  Advanced    Advanced         1                                 1
        5       no  3.44    Novice      Novice         0                                 2
        19     yes  1.98  Advanced    Advanced         0                                 0
        15     yes  4.00  Advanced    Advanced         1                                 1
        40     yes  3.95    Novice    Beginner         0                                 3
        7      yes  2.33    Novice      Novice         1                                 0
        22     yes  3.46    Novice    Beginner         0                                 3
        36      no  3.00  Advanced      Novice         0                                 1
        38     yes  2.65  Advanced    Beginner         1                                 1
        >>>
    hashbucket
    hashbucket
    Functions
    hashbucket(expression)
    DESCRIPTION:
        Function returns the hash bucket number that corresponds to a specified row hash value.
        If no row hash value is specified, functions returns the highest hash bucket number.
    PARAMETERS:
        expression:
            Optional Argument.
            Specifies a ColumnExpression of a column that evaluates to a valid BYTE(4)
            row hash value.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If expression does not appear in the argument list, then function returns an
            INTEGER value that is the highest hash bucket number.
            If expression evaluates to NULL, then function returns NULL.
            If expression evaluates to a valid BYTE(4) row hash value, then function
            returns the hash bucket number corresponding to the row hash value.
            The range of values for hash bucket numbers depends on the system setting
            of the hash bucket size.
                * If the hash bucket size is 16 bits, the hash bucket numbers can have
                  a value from 0 to 65535.
                * If the hash bucket size is 20 bits, the hash bucket numbers can have
                  a value from 0 to 1048575.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "bytes_table")
        >>>
        # Create a DataFrame on 'bytes_table' table.
        >>> bytes_table = DataFrame("bytes_table")
        >>> bytes_table
               byte_col      varbyte_col             blob_col
        id_col
        0         b'63'      b'627A7863'  b'3330363136323633'
        2         b'61'  b'616263643132'  b'6162636431323233'
        1         b'62'      b'62717765'  b'3331363136323633'
        >>>
        # Import func from sqlalchemy to execute hashbucket() function.
        >>> from sqlalchemy import func
        # Example 1: Return the maximum hash bucket values. To do so, execute function without any argument.
        # Create a sqlalchemy Function object.
        >>> hashbucket_func_ = func.hashbucket()
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = bytes_table.assign(hashbucket_max_=hashbucket_func_)
        >>> print(df)
               byte_col      varbyte_col             blob_col  hashbucket_max_
        id_col
        0         b'63'      b'627A7863'  b'3330363136323633'          1048575
        2         b'61'  b'616263643132'  b'6162636431323233'          1048575
        1         b'62'      b'62717765'  b'3331363136323633'          1048575
        >>>
        # Example 2: Return the maximum hash bucket values corresponding to values in "byte_col" column.
        # Create a sqlalchemy Function object.
        # Function accepts column of type BYTE(4) only. Let's cast byte_col column to BYTE(4).
        >>> from teradatasqlalchemy import BYTE
        >>> hashbucket_func_ = func.hashbucket(bytes_table.byte_col.cast(BYTE(4)).expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = bytes_table.assign(hashbucket_byte_col_=hashbucket_func_)
        >>> print(df)
               byte_col      varbyte_col             blob_col  hashbucket_byte_col_
        id_col
        0         b'63'      b'627A7863'  b'3330363136323633'           405504
        2         b'61'  b'616263643132'  b'6162636431323233'           397312
        1         b'62'      b'62717765'  b'3331363136323633'           401408
        >>>
    hashrow
    hashrow
    Functions
    hashrow(expression1, expression2, expressionN)
    DESCRIPTION:
        Function returns the hexadecimal row hash value for an expression or sequence of
        expressions. If no expression is specified, function returns the maximum hash
        code value.
        Function is particularly useful for identifying the statistical properties of the
        current primary index, or to evaluate these properties for other columns to determine
        their suitability as a future primary index. You can also use these statistics to
        help minimize hash synonyms and enhance the uniformity of data distribution.
        There are a maximum of 4,294,967,295 hash codes available in the system, ranging from
        '00000000'XB to 'FFFFFFFF'XB.
    PARAMETERS:
        expression1:
            Optional Argument.
            Specifies a ColumnExpression of a column that make up a (potential) index.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        expression2:
            Optional Argument.
            Specifies a ColumnExpression of a column that make up a (potential) index.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        expressionN:
            Optional Argument.
            Specifies a ColumnExpression of a column that make up a (potential) index.
            N here represents any number, i.e, user is allowed to pass as many expressions
            as he or she can.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
        Notes:
            1. If no arguments are passed, then function returns the maximum hash code value.
            2. If only one is passed and it evaluates to NULL, then function returns '00000000'XB.
            3. If all expression evaluates to NULL, then function returns '00000000'XB.
            4. If an expression that evaluates to 0, empty string, whitespace, or a similar
               value is passed, then function returns '00000000'XB.
            5. If a valid, non-NULL expression is passed, then function evaluates expression or
               the list of expressions and applies the hash function on the result. Function
               returns the resulting row hash value.
            6. If multiple expressions are passed, where some expressions can evaluate to NULL,
               then function evaluates expression or the list of expressions and applies the hash
               function on the result. Function returns the resulting row hash value.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute hashrow() function.
        >>> from sqlalchemy import func
        # Example 1: Return the maximum hash code value. To do so, execute function without any argument.
        # Create a sqlalchemy Function object.
        >>> hashrow_func_ = func.hashrow()
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashrow_max_=hashrow_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted hashrow_max_
        id
        5       no  3.44    Novice      Novice         0        b'-1'
        34     yes  3.85  Advanced    Beginner         0        b'-1'
        13      no  4.00  Advanced      Novice         1        b'-1'
        40     yes  3.95    Novice    Beginner         0        b'-1'
        22     yes  3.46    Novice    Beginner         0        b'-1'
        19     yes  1.98  Advanced    Advanced         0        b'-1'
        36      no  3.00  Advanced      Novice         0        b'-1'
        15     yes  4.00  Advanced    Advanced         1        b'-1'
        7      yes  2.33    Novice      Novice         1        b'-1'
        17      no  3.83  Advanced    Advanced         1        b'-1'
        >>>
        # Example 2: Return the hexadecimal row hash value corresponding to values in "stats" column.
        # Create a sqlalchemy Function object.
        >>> hashrow_func_ = func.hashrow(admissions_train.stats.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashrow_stats_=hashrow_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted hashrow_stats_
        id
        5       no  3.44    Novice      Novice         0     b'C8D3967'
        34     yes  3.85  Advanced    Beginner         0   b'-69A5305C'
        13      no  4.00  Advanced      Novice         1   b'-69A5305C'
        40     yes  3.95    Novice    Beginner         0     b'C8D3967'
        22     yes  3.46    Novice    Beginner         0     b'C8D3967'
        19     yes  1.98  Advanced    Advanced         0   b'-69A5305C'
        36      no  3.00  Advanced      Novice         0   b'-69A5305C'
        15     yes  4.00  Advanced    Advanced         1   b'-69A5305C'
        7      yes  2.33    Novice      Novice         1     b'C8D3967'
        17      no  3.83  Advanced    Advanced         1   b'-69A5305C'
        >>>
        # Example 3: Return the hexadecimal row hash value corresponding to all columns in "admissions_train"
        #            DataFrame.
        # Create a sqlalchemy Function object.
        >>> hashrow_func_ = func.hashrow(admissions_train.id.expression,
        ...                              admissions_train.masters.expression,
        ...                              admissions_train.stats.expression,
        ...                              admissions_train.programming.expression,
        ...                              admissions_train.admitted.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(hashrow_all_col_=hashrow_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted hashrow_all_col_
        id
        5       no  3.44    Novice      Novice         0      b'2EF3CB74'
        34     yes  3.85  Advanced    Beginner         0     b'-2B8F2EE7'
        13      no  4.00  Advanced      Novice         1      b'5EDB072F'
        40     yes  3.95    Novice    Beginner         0     b'-44A7BF48'
        22     yes  3.46    Novice    Beginner         0      b'11DFF574'
        19     yes  1.98  Advanced    Advanced         0     b'-2763FD49'
        36      no  3.00  Advanced      Novice         0     b'-178A3958'
        15     yes  4.00  Advanced    Advanced         1      b'682DBBCD'
        7      yes  2.33    Novice      Novice         1      b'4C9EA097'
        17      no  3.83  Advanced    Advanced         1     b'-3A2234F5'
        >>>
    Regular Expression Functions
    regexp_instr
    regexp_instr
    Functions
    regexp_instr(source_string, regexp_string, position_arg, occurrence_arg, return_opt, match_arg)
    DESCRIPTION:
        Function searches source_string for a match to regexp_string.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal to
            search for a match.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If source_string is NULL, NULL is returned.
        regexp_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal which
            is to be used as regex.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If regexp_string is NULL, NULL is returned.
        position_arg:
            Optional Argument.
            Specifies the position in source_string from which to start searching.
            Argument accepts a ColumnExpression of a numeric column or a numeric constant.
            If the value is greater than the input string length, zero is returned.
            If the value is NULL, the value NULL is returned.
            If argument is not specified, the default value (1) is used.
        occurrence_arg:
            Optional Argument.
            Specifies the number of the occurrence to be returned.
            For example, if occurrence_arg is 2, the function matches the first occurrence
            in source_string and starts searching from the character following the first
            occurrence in source_string for the second occurrence in source_string.
            Argument accepts a ColumnExpression of a numeric column or a numeric constant.
            If the value is greater than the number of matches found, 0 is returned.
            If the value is NULL, a NULL result is returned.
            If argument is not specified, the default value (1) is used.
        return_opt:
            Optional Argument.
            Specifies a numeric value 0 or 1 that decides return value for the match.
            Argument accepts a ColumnExpression of a numeric column or a numeric constant.
            Valid values are:
                * 0 = function returns the beginning position of the match (default).
                * 1 = function returns the end position (character following the occurrence) of the match.
            If the value is NULL, NULL is returned.
            If argument is not specified, the default value (0) value is used.
        match_arg:
            Optional Argument.
            Specifies a character which decides the handling of regex matching.
            Valid values are:
                * 'i' = case-insensitive matching.
                * 'c' = case sensitive matching.
                * 'n' = the period character (match any character) can match the newline character.
                * 'm' = source_string is treated as multiple lines instead of as a single line.
                        With this option, the '^' and '$' characters apply to each line in source_string
                        instead of the entire source_string.
                * 'l' = if source_string exceeds the current maximum allowed source_string size
                        (currently 16 MB), a NULL is returned instead of an error. This is useful for
                        long-running queries where you do not want long strings causing an error that
                        would make the query fail.
                * 'x' = ignore whitespace.
            The argument can contain more than one character.
            Notes:
                1. If a character in the argument is not valid, then that error is reported.
                2. If match_arg is not specified, is NULL, or is empty:
                    a. The match is case-sensitive.
                    b. A period does not match the newline character.
                    c. source_string is treated as a single line.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example searches for "d" substring in "stats" column and returns the position of the
        # matched string based on integer value in "admitted" column.
        # Import func from sqlalchemy to execute regexp_instr() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: Function name is case-insensitive.
        >>> regex_instr_ = func.REGEXP_INSTR(admissions_train.stats.expression, "d", 1, 1,
        ...                                  admissions_train.admitted.expression, 'c')
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(regex_instr_col=regex_instr_)
        >>> print(df)
           masters   gpa     stats programming  admitted  regex_instr_col
        id
        22     yes  3.46    Novice    Beginner         0                0
        36      no  3.00  Advanced      Novice         0                2
        15     yes  4.00  Advanced    Advanced         1                3
        38     yes  2.65  Advanced    Beginner         1                3
        5       no  3.44    Novice      Novice         0                0
        17      no  3.83  Advanced    Advanced         1                3
        34     yes  3.85  Advanced    Beginner         0                2
        13      no  4.00  Advanced      Novice         1                3
        26     yes  3.57  Advanced    Advanced         1                3
        19     yes  1.98  Advanced    Advanced         0                2
        >>>
    regexp_replace
    regexp_replace
    Functions
    regexp_replace(source_string, regexp_string, replace_string, position_arg, occurrence_arg, match_arg)
    DESCRIPTION:
        Function replaces portions of source_string that match regexp_string with
        the replace_string.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to replace portion that matches regexp_string with the replace_string.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If source_string is NULL, NULL is returned.
        regexp_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to use for regex matching.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If regexp_string is NULL, NULL is returned.
        replace_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to use as replacement.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If a replace_string is not specified, is NULL or is an empty string, the matches are
            removed from the result.
        position_arg:
            Optional Argument.
            A numeric argument.
            Specifies the position in source_string from which to start searching.
            Argument accepts a ColumnExpression of a numeric column or a numeric constant.
            If the value greater than the input string length, NULL is returned.
            If the value is NULL, the value NULL is returned.
            If argument is not specified, the default value (1) is used.
        occurrence_arg:
            Optional Argument.
            Specifies the occurrence to replace the match with replace_string.
            Argument accepts a ColumnExpression of a numeric column or a numeric constant.
                * If a value of 0 is specified, all occurrences are replaced.
                * If the value is greater than 1, the search begins for the second
                  occurrence beginning with the first character following the first
                  occurrence of the regexp_string, and so on.
            If occurrence_arg is greater than the number of matches found, nothing
            is replaced and source_string is returned.
            If occurrence_arg is NULL, a NULL result is returned. If occurrence_arg
            is not specified, 0 is the default value.
        match_arg:
            Optional Argument.
            Specifies a character which decides the handling of regex matching.
            Valid values are:
                * 'i' = case-insensitive matching.
                * 'c' = case sensitive matching.
                * 'n' = the period character (match any character) can match the newline character.
                * 'm' = source_string is treated as multiple lines instead of as a single line.
                        With this option, the '^' and '$' characters apply to each line in source_string
                        instead of the entire source_string.
                * 'l' = if source_string exceeds the current maximum allowed source_string size
                        (currently 16 MB), a NULL is returned instead of an error. This is useful for
                        long-running queries where you do not want long strings causing an error that
                        would make the query fail.
                * 'x' = ignore whitespace.
            The argument can contain more than one character.
            Notes:
                1. If a character in the argument is not valid, then that character is ignored.
                2. If match_arg is not specified, is NULL, or is empty:
                    a. The match is case-sensitive.
                    b. A period does not match the newline character.
                    c. source_string is treated as a single line.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example searches for "vice" substring from "stats" column and replaces
        # the same with "w You See Me".
        # Import func from sqlalchemy to execute regexp_replace() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: Function name is case-insensitive.
        >>> regexp_replace_ = func.Regexp_Replace(admissions_train.stats.expression, "vice",
        ...                                       "w You See Me", 1, 1, 'c')
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(regexp_replace_col=regexp_replace_)
        >>> print(df)
           masters   gpa     stats programming  admitted regexp_replace_col
        id
        15     yes  4.00  Advanced    Advanced         1           Advanced
        7      yes  2.33    Novice      Novice         1     Now You See Me
        22     yes  3.46    Novice    Beginner         0     Now You See Me
        17      no  3.83  Advanced    Advanced         1           Advanced
        13      no  4.00  Advanced      Novice         1           Advanced
        38     yes  2.65  Advanced    Beginner         1           Advanced
        26     yes  3.57  Advanced    Advanced         1           Advanced
        5       no  3.44    Novice      Novice         0     Now You See Me
        34     yes  3.85  Advanced    Beginner         0           Advanced
        40     yes  3.95    Novice    Beginner         0     Now You See Me
        >>>
    regexp_similar
    regexp_similar
    Functions
    regexp_similar(source_string, regexp_string, match_arg)
    DESCRIPTION:
        Function compares source_string to regexp_string and returns integer value.
        Function returns following integer value:
            * 1 (true) if the entire source_string matches regexp_string.
            * 0 (false) if the entire source_string does not match regexp_string.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            from which substring is to be extracted.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If source_string is NULL, NULL is returned.
        regexp_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            which is to be used as regex.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If regexp_string is NULL, NULL is returned.
        match_arg:
            Optional Argument.
            Specifies a character which decides the handling of regex matching.
            Valid values are:
                * 'i' = case-insensitive matching.
                * 'c' = case sensitive matching.
                * 'n' = the period character (match any character) can match the newline character.
                * 'm' = source_string is treated as multiple lines instead of as a single line.
                        With this option, the '^' and '$' characters apply to each line in source_string
                        instead of the entire source_string.
                * 'l' = if source_string exceeds the current maximum allowed source_string size
                        (currently 16 MB), a NULL is returned instead of an error. This is useful for
                        long-running queries where you do not want long strings causing an error that
                        would make the query fail.
                * 'x' = ignore whitespace.
            The argument can contain more than one character.
            Notes:
                1. If a character in the argument is not valid, then that character is ignored.
                2. If match_arg is not specified, is NULL, or is empty:
                    a. The match is case-sensitive.
                    b. A period does not match the newline character.
                    c. source_string is treated as a single line.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example compares strings in "stats" column and "programming" column.
        # Import func from sqlalchemy to execute regexp_similar() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: Function name is case-insensitive.
        >>> regexp_similar_ = func.REGEXP_SIMILAR(admissions_train.stats.expression,
        ...                                       admissions_train.programming.expression, 'c')
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(regexp_similar_col=regexp_similar_)
        >>> print(df)
           masters   gpa     stats programming  admitted  regexp_similar_col
        id
        5       no  3.44    Novice      Novice         0                   1
        34     yes  3.85  Advanced    Beginner         0                   0
        13      no  4.00  Advanced      Novice         1                   0
        40     yes  3.95    Novice    Beginner         0                   0
        22     yes  3.46    Novice    Beginner         0                   0
        19     yes  1.98  Advanced    Advanced         0                   1
        36      no  3.00  Advanced      Novice         0                   0
        15     yes  4.00  Advanced    Advanced         1                   1
        7      yes  2.33    Novice      Novice         1                   1
        17      no  3.83  Advanced    Advanced         1                   1
        >>>
    regexp_substr
    regexp_substr
    Functions
    regexp_substr(source_string, regexp_string, position_arg, occurrence_arg, match_arg)
    DESCRIPTION:
        Function extracts a substring from source_string that matches a regular
        expression specified by regexp_string.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            from which substring is to be extracted.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If source_string is NULL, NULL is returned.
        regexp_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            which is to be used as regex.
            Format for the argument: '<dataframe>.<dataframe_column>.expression'.
            If regexp_string is NULL, NULL is returned.
        position_arg:
            Optional Argument.
            Specifies the position in source_string from which to start searching.
            Argument accepts a ColumnExpression of a numeric column or a numeric constant.
            If the value greater than the input string length, NULL is returned.
            If the value is NULL, the value NULL is returned.
            If argument is not specified, the default value (1) is used.
        occurrence_arg:
            Optional Argument.
            Specifies the number of the occurrence to be returned.
            For example, if occurrence_arg is 2, the function matches the first occurrence
            in source_string and starts searching from the character following the first
            occurrence in source_string for the second occurrence in source_string.
            Argument accepts a ColumnExpression of a numeric column or a numeric constant.
            If the value is greater than the number of matches found, NULL is returned.
            If the value is NULL, a NULL result is returned.
            If argument is not specified, the default value (1) is used.
        match_arg:
            Optional Argument.
            Specifies a character which decides the handling of regex matching.
            Valid values are:
                * 'i' = case-insensitive matching.
                * 'c' = case sensitive matching.
                * 'n' = the period character (match any character) can match the newline character.
                * 'm' = source_string is treated as multiple lines instead of as a single line.
                        With this option, the '^' and '$' characters apply to each line in source_string
                        instead of the entire source_string.
                * 'l' = if source_string exceeds the current maximum allowed source_string size
                        (currently 16 MB), a NULL is returned instead of an error. This is useful for
                        long-running queries where you do not want long strings causing an error that
                        would make the query fail.
                * 'x' = ignore whitespace.
            The argument can contain more than one character.
            Notes:
                1. If a character in the argument is not valid, then that character is ignored.
                2. If match_arg is not specified, is NULL, or is empty:
                    a. The match is case-sensitive.
                    b. A period does not match the newline character.
                    c. source_string is treated as a single line.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example extracts "no" substring from "stats" column.
        # Import func from sqlalchemy to execute regexp_substr() function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Note: Function name is case-insensitive.
        >>> regex_substr_ = func.REGEXP_SUBSTR(admissions_train.stats.expression, "no", 1, 1, 'i')
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(regex_substr_col=regex_substr_)
        >>> print(df)
           masters   gpa     stats programming  admitted regex_substr_col
        id
        15     yes  4.00  Advanced    Advanced         1             None
        7      yes  2.33    Novice      Novice         1               No
        22     yes  3.46    Novice    Beginner         0               No
        17      no  3.83  Advanced    Advanced         1             None
        13      no  4.00  Advanced      Novice         1             None
        38     yes  2.65  Advanced    Beginner         1             None
        26     yes  3.57  Advanced    Advanced         1             None
        5       no  3.44    Novice      Novice         0               No
        34     yes  3.85  Advanced    Beginner         0             None
        40     yes  3.95    Novice    Beginner         0               No
        >>>
    String Functions
    ascii
    ascii
    Functions
    ascii(column_expression)
    DESCRIPTION:
        Function returns the decimal representation of the first character in
        column_expression as a NUMBER value. The decimal representation will reflect
        the character set of the input string.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a character column or a string literal for
            which the ASCII value is to be returned.
            If argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, and CLOB
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example returns the ascii representation for character string in "stats" column.
        # Import func from sqlalchemy to execute ascii function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> ascii_func_ = func.ascii(admissions_train.stats.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(ascii_stats_=ascii_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted ascii_stats_
        id
        13      no  4.00  Advanced      Novice         1           65
        26     yes  3.57  Advanced    Advanced         1           65
        5       no  3.44    Novice      Novice         0           78
        19     yes  1.98  Advanced    Advanced         0           65
        15     yes  4.00  Advanced    Advanced         1           65
        40     yes  3.95    Novice    Beginner         0           78
        7      yes  2.33    Novice      Novice         1           78
        22     yes  3.46    Novice    Beginner         0           78
        36      no  3.00  Advanced      Novice         0           65
        38     yes  2.65  Advanced    Beginner         1           65
        >>>
    char2hexint
    char2hexint
    Functions
    char2hexint(column_expression)
    DESCRIPTION:
        Function returns the hexadecimal representation for a character string.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a character column or a string literal for
            which the hexadecimal representation is to be returned.
            If the argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR and VARCHAR.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example returns the hexadecimal representation for character string in "stats" column.
        # Import func from sqlalchemy to execute char2hexint function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> char2hexint_func_ = func.char2hexint(admissions_train.stats.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(char2hexint_gpa_=char2hexint_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  char2hexint_gpa_
        id
        13      no  4.00  Advanced      Novice         1  416476616E636564
        26     yes  3.57  Advanced    Advanced         1  416476616E636564
        5       no  3.44    Novice      Novice         0  4E6F76696365
        19     yes  1.98  Advanced    Advanced         0  416476616E636564
        15     yes  4.00  Advanced    Advanced         1  416476616E636564
        40     yes  3.95    Novice    Beginner         0  4E6F76696365
        7      yes  2.33    Novice      Novice         1  4E6F76696365
        22     yes  3.46    Novice    Beginner         0  4E6F76696365
        36      no  3.00  Advanced      Novice         0  416476616E636564
        38     yes  2.65  Advanced    Beginner         1  416476616E636564
        >>>
    chr
    chr
    Functions
    chr(column_expression)
    DESCRIPTION:
        Function returns the Latin ASCII character of a given a numeric code value.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a numeric column or a numeric constant for
            which the Latin ASCII value is to be returned.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Notes:
                1. If the argument is null, then result is null.
                2. Numeric value must be zero or greater.  If column_expression is greater than 255,
                   an operation of column_expression mod 256 is executed to return a value
                   between 0 and 255.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "employee_info")
        >>>
        # Create a DataFrame on 'employee_info' table.
        >>> employee_info = DataFrame("employee_info")
        >>> employee_info
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Example returns the Latin ASCII character for values in "employee_no" column.
        # Import func from sqlalchemy to execute chr function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> chr_func_ = func.chr(employee_info.employee_no.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = employee_info.assign(chr_employee_no_=chr_func_)
        >>> print(df)
                    first_name marks   dob joined_date chr_employee_no_
        employee_no
        101              abcde  None  None    02/12/05                e
        100               abcd  None  None        None                d
        112               None  None  None    18/12/05                p
        >>>
    concat
    concat
    Functions
    concat(column_expression1, column_expression2, column_expression_n)
    DESCRIPTION:
        Function returns a string formed by concatenating the arguments in a
        left-to-right direction.
    PARAMETERS:
        column_expression1:
            Required Argument.
            Specifies a ColumnExpression of a byte, numeric, or string column or a literal.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
        column_expression2:
            Required Argument.
            Specifies a ColumnExpression of a byte, numeric, or string column or a literal.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
        column_expression_n:
            Optional Arguments.
            Specifies multiple ColumnExpressions of a byte, numeric, or string column or literal
            that can be used for concatenation.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute concat function.
        >>> from sqlalchemy import func
        # Example 1: Concatenate strings from "stats" and "programming" columns.
        # Create a sqlalchemy Function object.
        >>> concat_func_ = func.concat(admissions_train.stats.expression, admissions_train.programming.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(concat_gpa_=concat_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted       concat_gpa_
        id
        15     yes  4.00  Advanced    Advanced         1  AdvancedAdvanced
        7      yes  2.33    Novice      Novice         1      NoviceNovice
        22     yes  3.46    Novice    Beginner         0    NoviceBeginner
        17      no  3.83  Advanced    Advanced         1  AdvancedAdvanced
        13      no  4.00  Advanced      Novice         1    AdvancedNovice
        38     yes  2.65  Advanced    Beginner         1  AdvancedBeginner
        26     yes  3.57  Advanced    Advanced         1  AdvancedAdvanced
        5       no  3.44    Novice      Novice         0      NoviceNovice
        34     yes  3.85  Advanced    Beginner         0  AdvancedBeginner
        40     yes  3.95    Novice    Beginner         0    NoviceBeginner
        >>>
        # Example 2: Concatenate strings from "programming" columns and numeric value from "gpa" column
        #             with sevaral literal values.
        # Create a sqlalchemy Function object.
        >>> concat_func_ = func.concat("Student with  id: ", admissions_train.id.expression, " has gpa: ", admissions_trai
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(concat_string_=concat_func_)
        >>> df = admissions_train.assign(concat_string_=concat_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted                                                                  
        id
        22     yes  3.46    Novice    Beginner         0  Student with  id:          22 has gpa:  3.46000000000000E 000 an
        26     yes  3.57  Advanced    Advanced         1  Student with  id:          26 has gpa:  3.57000000000000E 000 an
        5       no  3.44    Novice      Novice         0    Student with  id:           5 has gpa:  3.44000000000000E 000 
        17      no  3.83  Advanced    Advanced         1  Student with  id:          17 has gpa:  3.83000000000000E 000 an
        13      no  4.00  Advanced      Novice         1    Student with  id:          13 has gpa:  4.00000000000000E 000 
        19     yes  1.98  Advanced    Advanced         0  Student with  id:          19 has gpa:  1.98000000000000E 000 an
        36      no  3.00  Advanced      Novice         0    Student with  id:          36 has gpa:  3.00000000000000E 000 
        15     yes  4.00  Advanced    Advanced         1  Student with  id:          15 has gpa:  4.00000000000000E 000 an
        34     yes  3.85  Advanced    Beginner         0  Student with  id:          34 has gpa:  3.85000000000000E 000 an
        38     yes  2.65  Advanced    Beginner         1  Student with  id:          38 has gpa:  2.65000000000000E 000 an
        >>>
    editdistance
    editdistance
    Functions
    editdistance(string_column_expression1, string_column_expression2, ci, cd, cs, ct)
    DESCRIPTION:
        Function returns the minimum number of edit operations (insertions, deletions,
        substitutions and transpositions) required to transform string1 (string_column_expression1)
        into string2 (string_column_expression2).
        EDITDISTANCE measures the similarity between two strings. A low number of deletions,
        insertions, substitutions or transpositions implies a high similarity. The insertions,
        deletions, substitutions, and transpositions are based on the Damerau-Levenshtein
        Distance algorithm with modifications for costed operations.
    PARAMETERS:
        string_column_expression1:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Support column types are: CHARACTER, VARCHAR, or CLOB.
        string_column_expression2:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Support column types are: CHARACTER, VARCHAR, or CLOB.
        ci:
            Optional Argument.
            Specifies the relative cost of an insert operation.
            The value specified must be a non-negative integer.
            If not specified, a default value of 1 is used.
        cd:
            Optional Argument.
            Specifies the relative cost of a delete operation.
            The value specified must be a non-negative integer.
            If not specified, a default value of 1 is used.
        cs:
            Optional Argument.
            Specifies the relative cost of a substitute operation.
            The value specified must be a non-negative integer.
            If not specified, a default value of 1 is used.
        ct:
            Optional Argument.
            Specifies the relative cost of a transpose operation.
            The value specified must be a non-negative integer.
            If not specified, a default value of 1 is used.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute editdistance function.
        >>> from sqlalchemy import func
        # Example 1: Calculate the EDITDISTANCE between values in "stats" and "programming" columns.
        # Create a sqlalchemy Function object.
        >>> editdistance_func_ = func.editdistance(admissions_train.stats.expression, admissions_train.programming.express
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(editdistance_gpa_=editdistance_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  editdistance_gpa_
        id
        15     yes  4.00  Advanced    Advanced         1                  0
        7      yes  2.33    Novice      Novice         1                  0
        22     yes  3.46    Novice    Beginner         0                  6
        17      no  3.83  Advanced    Advanced         1                  0
        13      no  4.00  Advanced      Novice         1                  5
        38     yes  2.65  Advanced    Beginner         1                  6
        26     yes  3.57  Advanced    Advanced         1                  0
        5       no  3.44    Novice      Novice         0                  0
        34     yes  3.85  Advanced    Beginner         0                  6
        40     yes  3.95    Novice    Beginner         0                  6
        >>>
        # Example 2: Calculate the EDITDISTANCE between values in "stats" and "programming" columns with
        #            with cost associated with the edit operations passed.
        # Create a sqlalchemy Function object.
        # Note: We are using 'EDITDISTANCE' as function name. Function name is case-insensitive.
        >>> editdistance_func_ = func.EDITDISTANCE(admissions_train.stats.expression, admissions_train.programming.express
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(editdistance_gpa_=editdistance_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  editdistance_gpa_
        id
        22     yes  3.46    Novice    Beginner         0                  8
        36      no  3.00  Advanced      Novice         0                  5
        15     yes  4.00  Advanced    Advanced         1                  0
        38     yes  2.65  Advanced    Beginner         1                  6
        5       no  3.44    Novice      Novice         0                  0
        17      no  3.83  Advanced    Advanced         1                  0
        34     yes  3.85  Advanced    Beginner         0                  6
        13      no  4.00  Advanced      Novice         1                  5
        26     yes  3.57  Advanced    Advanced         1                  0
        19     yes  1.98  Advanced    Advanced         0                  0
        >>>
    index
    index
    Functions
    index(string_column_expression1, string_column_expression2)
    DESCRIPTION:
        Function returns the position in string1 (string_column_expression1)
        where string2 (string_column_expression2) starts.
        The following rules apply to the value that INDEX returns:
            1. If string2 is not found in string_expression_1, then the result is zero.
            2. If string2 is null, then the result is null.
            3. If the arguments are character types, INDEX returns a logical character position,
               not a byte position, except when the server character set of the arguments is KANJI1
               and the session client character set is KanjiEBCDIC.
    PARAMETERS:
        string_column_expression1:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to be searched.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types are:
                1. Character
                2. Byte - If argument is of type BYTE, then string_column_expression2 must be of type BYTE.
                3. Numeric - If argument is numeric, then it is converted implicitly to CHARACTER type.
        string_column_expression2:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            which is used as a substring to be searched for its position within the
            full string in "string_column_expression1"
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types are:
                1. Character
                2. Byte - If argument is of type BYTE, then string_column_expression1 must be of type BYTE.
                3. Numeric - If argument is numeric, then it is converted implicitly to CHARACTER type.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example returns the start position of "ce" in "stats" column.
        # Import func from sqlalchemy to execute index function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> index_func_ = func.index(admissions_train.stats.expression, "ce")
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(index_ce_stats_=index_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  index_ce_stats_
        id
        15     yes  4.00  Advanced    Advanced         1                6
        7      yes  2.33    Novice      Novice         1                5
        22     yes  3.46    Novice    Beginner         0                5
        17      no  3.83  Advanced    Advanced         1                6
        13      no  4.00  Advanced      Novice         1                6
        38     yes  2.65  Advanced    Beginner         1                6
        26     yes  3.57  Advanced    Advanced         1                6
        5       no  3.44    Novice      Novice         0                5
        34     yes  3.85  Advanced    Beginner         0                6
        40     yes  3.95    Novice    Beginner         0                5
        >>>
    initcap
    initcap
    Functions
    initcap(column_expression)
    DESCRIPTION:
        Function modifies a string argument and returns the string with the first character
        in each word in uppercase and all other characters in lowercase. Words are delimited
        by white space or characters that are not alphanumeric.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            If the argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, or CLOB
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example: Convert the first character to uppercase for strings in "masters" column.
        # Import func from sqlalchemy to execute initcap function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> initcap_func_ = func.initcap(admissions_train.masters.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(initcap_masters_=initcap_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted initcap_masters_
        id
        15     yes  4.00  Advanced    Advanced         1              Yes
        7      yes  2.33    Novice      Novice         1              Yes
        22     yes  3.46    Novice    Beginner         0              Yes
        17      no  3.83  Advanced    Advanced         1               No
        13      no  4.00  Advanced      Novice         1               No
        38     yes  2.65  Advanced    Beginner         1              Yes
        26     yes  3.57  Advanced    Advanced         1              Yes
        5       no  3.44    Novice      Novice         0               No
        34     yes  3.85  Advanced    Beginner         0              Yes
        40     yes  3.95    Novice    Beginner         0              Yes
        >>>
    instr
    instr
    Functions
    instr(source_string, search_string, position, occurrence)
    DESCRIPTION:
        Function searches the source_string argument for occurrences of search_string.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
        search_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that the function searches for in source_string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
        position:
            Optional Argument.
            Specifies the position as an integer to begin searching at in the source_string.
            If position is not specified, the search starts at the beginning of source_string.
            If position is negative, the function counts and searches backwards from the end of
            source_string.
            It cannot have a value of zero.
        occurrence:
            Optional Argument.
            Specifies an integer which decides occurrence of search_string to find in source_string.
            If occurrence is not specified, the function searches for the first occurrence.
            If occurrence is greater than 1, the function searches for additional occurrences
            beginning with the second character in the previous occurrence.
            It cannot be zero or a negative value.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example searches for a string "e" in "programming" column values.
        # Import func from sqlalchemy to execute instr function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> instr_func_ = func.instr(admissions_train.programming.expression, "e")
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(instr_e_in_programming_=instr_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted instr_e_in_programming_
        id
        15     yes  4.00  Advanced    Advanced         1                       7
        7      yes  2.33    Novice      Novice         1                       6
        22     yes  3.46    Novice    Beginner         0                       2
        17      no  3.83  Advanced    Advanced         1                       7
        13      no  4.00  Advanced      Novice         1                       6
        38     yes  2.65  Advanced    Beginner         1                       2
        26     yes  3.57  Advanced    Advanced         1                       7
        5       no  3.44    Novice      Novice         0                       6
        34     yes  3.85  Advanced    Beginner         0                       2
        40     yes  3.95    Novice    Beginner         0                       2
        >>>
    left
    left
    Functions
    left(source_string, length)
    DESCRIPTION:
        Function truncates input string to a specified number of characters desired from
        the left side of the string.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal to
            truncate.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, and CLOB
        length:
            Required Argument.
            Specifies a positive integer specifying the number of characters desired from
            the left side of the string. If the number of character exceeds the number of
            characters in the original string, the original string is returned.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    ALTERNATE NAME:
        td_left
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example truncates values in "programming" column to a length of 3.
        # Import func from sqlalchemy to execute left function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> left_func_ = func.left(admissions_train.programming.expression, 3)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(left_3_programming_=left_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted left_3_programming_
        id
        15     yes  4.00  Advanced    Advanced         1                 Adv
        34     yes  3.85  Advanced    Beginner         0                 Beg
        13      no  4.00  Advanced      Novice         1                 Nov
        38     yes  2.65  Advanced    Beginner         1                 Beg
        5       no  3.44    Novice      Novice         0                 Nov
        40     yes  3.95    Novice    Beginner         0                 Beg
        7      yes  2.33    Novice      Novice         1                 Nov
        22     yes  3.46    Novice    Beginner         0                 Beg
        26     yes  3.57  Advanced    Advanced         1                 Adv
        17      no  3.83  Advanced    Advanced         1                 Adv
        >>>
    length
    length
    Functions
    length(column_expression)
    DESCRIPTION:
        Function returns the number of characters in the expression.
    PARAMETERS:
        column_expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            If the argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, or CLOB
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example returns length of character string in "programming" column.
        # Import func from sqlalchemy to execute length function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> length_func_ = func.length(admissions_train.programming.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(length_programming_=length_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted length_programming_
        id
        5       no  3.44    Novice      Novice         0                   6
        34     yes  3.85  Advanced    Beginner         0                   8
        13      no  4.00  Advanced      Novice         1                   6
        40     yes  3.95    Novice    Beginner         0                   8
        22     yes  3.46    Novice    Beginner         0                   8
        19     yes  1.98  Advanced    Advanced         0                   8
        36      no  3.00  Advanced      Novice         0                   6
        15     yes  4.00  Advanced    Advanced         1                   8
        7      yes  2.33    Novice      Novice         1                   6
        17      no  3.83  Advanced    Advanced         1                   8
        >>>
    locate
    locate
    Functions
    locate(string_expr1, string_expr2, n1)
    DESCRIPTION:
        Function returns the position of the first occurrence of string_expr1 within string_expr2.
        The search for the first occurrence of string_expr1 begins with the first character
        position in string_expr2 unless the optional argument, n1, is specified.
    PARAMETERS:
        string_expr1:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal for
            substring to be searched for its position within the full string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Support types of arguments:
                1. Character, except for CLOB
                2. Byte, except for BLOB
                    If string_expr1 is of type BYTE, then string_expr2 must be of type BYTE.
                3. Numeric - This is converted implicitly to CHARACTER type.
        string_expr2:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal for
            the full string to be searched in "string_expr1".
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Support types of arguments:
                1. Character, except for CLOB
                2. Byte, except for BLOB
                    If string_expr1 is of type BYTE, then string_expr2 must be of type BYTE.
                3. Numeric - This is converted implicitly to CHARACTER type.
        n1:
            Optional Argument.
            Specifies where the search begins with the character position indicated by the
            value of n1. Accepts a positive integer value.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example returns the position of "ce" in a string in "stats" column.
        # Import func from sqlalchemy to execute locate function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> locate_func_ = func.locate("ce", admissions_train.stats.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(locate_stats_=locate_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  locate_stats_
        id
        5       no  3.44    Novice      Novice         0              5
        34     yes  3.85  Advanced    Beginner         0              6
        13      no  4.00  Advanced      Novice         1              6
        40     yes  3.95    Novice    Beginner         0              5
        22     yes  3.46    Novice    Beginner         0              5
        19     yes  1.98  Advanced    Advanced         0              6
        36      no  3.00  Advanced      Novice         0              6
        15     yes  4.00  Advanced    Advanced         1              6
        7      yes  2.33    Novice      Novice         1              5
        17      no  3.83  Advanced    Advanced         1              6
        >>>
    lower
    lower
    Functions
    lower(character_string_expression)
    DESCRIPTION:
        Function returns a character string identical to character_string_expression,
        except that all uppercase letters are replaced with their lowercase equivalents.
    PARAMETERS:
        character_string_expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to be converted to lowercase.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example converts strings in "stats" column to lowercase.
        # Import func from sqlalchemy to execute lower function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> lower_func_ = func.lower(admissions_train.stats.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(lower_stats_=lower_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted lower_stats_
        id
        22     yes  3.46    Novice    Beginner         0     novice
        36      no  3.00  Advanced      Novice         0   advanced
        15     yes  4.00  Advanced    Advanced         1   advanced
        38     yes  2.65  Advanced    Beginner         1   advanced
        5       no  3.44    Novice      Novice         0     novice
        17      no  3.83  Advanced    Advanced         1   advanced
        34     yes  3.85  Advanced    Beginner         0   advanced
        13      no  4.00  Advanced      Novice         1   advanced
        26     yes  3.57  Advanced    Advanced         1   advanced
        19     yes  1.98  Advanced    Advanced         0   advanced
        >>>
    lpad
    lpad
    Functions
    lpad(source_string, length, ﬁll_string)
    DESCRIPTION:
        Function returns the source_string padded to the left with the characters
        in fill_string so that the resulting string has 'length' characters.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to be padded. If the length of source_string is greater than 'length',
            source_string is truncated to 'length' characters.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: VARCHAR, or CLOB
        length:
            Required Argument.
            Specifies a ColumnExpression of an int column or an integer literal specifying
            the number of characters in the resulting string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: INTEGER, BIGINT, or NUMBER
        fill_string:
            Optional Argument.
            Specifies a ColumnExpression of a string column or a string literal
            used to pad the source_string.
            The sequence of characters in fill_string is replicated as necessary.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, or CLOB
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute lpad function.
        >>> from sqlalchemy import func
        # Example 1: Pad string in "stats" column with 0.
        # Create a sqlalchemy Function object.
        >>> lpad_func_ = func.lpad(admissions_train.stats.expression, 10, "0")
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(lpad_gpa_=lpad_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted   lpad_gpa_
        id
        15     yes  4.00  Advanced    Advanced         1  00Advanced
        34     yes  3.85  Advanced    Beginner         0  00Advanced
        13      no  4.00  Advanced      Novice         1  00Advanced
        38     yes  2.65  Advanced    Beginner         1  00Advanced
        5       no  3.44    Novice      Novice         0  0000Novice
        40     yes  3.95    Novice    Beginner         0  0000Novice
        7      yes  2.33    Novice      Novice         1  0000Novice
        22     yes  3.46    Novice    Beginner         0  0000Novice
        26     yes  3.57  Advanced    Advanced         1  00Advanced
        17      no  3.83  Advanced    Advanced         1  00Advanced
        >>>
        # Example 2: Pad string in "stats" column with strings from "masters" column.
        # Create a sqlalchemy Function object.
        >>> lpad_func_ = func.lpad(admissions_train.stats.expression, 20, admissions_train.masters.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(lpad_gpa_=lpad_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted             lpad_gpa_
        id
        13      no  4.00  Advanced      Novice         1  nonononononoAdvanced
        26     yes  3.57  Advanced    Advanced         1  yesyesyesyesAdvanced
        5       no  3.44    Novice      Novice         0  nononononononoNovice
        19     yes  1.98  Advanced    Advanced         0  yesyesyesyesAdvanced
        15     yes  4.00  Advanced    Advanced         1  yesyesyesyesAdvanced
        40     yes  3.95    Novice    Beginner         0  yesyesyesyesyeNovice
        7      yes  2.33    Novice      Novice         1  yesyesyesyesyeNovice
        22     yes  3.46    Novice    Beginner         0  yesyesyesyesyeNovice
        36      no  3.00  Advanced      Novice         0  nonononononoAdvanced
        38     yes  2.65  Advanced    Beginner         1  yesyesyesyesAdvanced
        >>>
    ltrim
    ltrim
    Functions
    ltrim(expression1, expression2)
    DESCRIPTION:
        Function returns the argument expression1, with its left-most characters removed up
        to the first character that is not in the argument expression2.
    PARAMETERS:
        expression1:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            If argument is NULL, NULL is returned.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
        expression2:
            Optional Argument.
            Specifies a ColumnExpression of a string column or a string literal that
            will be removed from string in expression1. If expression2 is specified, it must
            be the same data type as expression1. If expression2 is not specified, the default is
            to use a single space character.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Note:
                Column expression of both arguments can be of following type:
                    a. Character/String types: CHAR, VARCHAR, or CLOB
                    b. Integer types: BYTEINT, SMALLINT, INTEGER, or BIGINT
                    c. Numeric types: FLOAT, REAL, DOUBLE PRECISION, DECIMAL, NUMERIC, or NUMBER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example left trim string in "stats" column if it has "Adv"
        # Import func from sqlalchemy to execute ltrim function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> ltrim_func_ = func.ltrim(admissions_train.stats.expression, "Adv")
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(ltrim_gpa_=ltrim_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted ltrim_gpa_
        id
        15     yes  4.00  Advanced    Advanced         1      anced
        7      yes  2.33    Novice      Novice         1     Novice
        22     yes  3.46    Novice    Beginner         0     Novice
        17      no  3.83  Advanced    Advanced         1      anced
        13      no  4.00  Advanced      Novice         1      anced
        38     yes  2.65  Advanced    Beginner         1      anced
        26     yes  3.57  Advanced    Advanced         1      anced
        5       no  3.44    Novice      Novice         0     Novice
        34     yes  3.85  Advanced    Beginner         0      anced
        40     yes  3.95    Novice    Beginner         0     Novice
        >>>
    ngram
    ngram
    Functions
    ngram(string_column_expression1, string_column_expression2, length, position)
    DESCRIPTION:
        Function returns the number of n-gram matches between string_column_expression1
        and string_column_expression2. A high number of matching n-gram patterns implies
        a high similarity between the two strings.
        For positional n-gram matching, the position as well as the pattern must match
        when measuring similarity. The position value indicates how far away positionally
        the match may be between the 2 strings as follows:
            * If position is set to a value of zero, the match must be at the same position
              in the 2 strings.
            * If position is set to a value of x , the match must be within x positions in
              the 2 strings.
              For example, if position = 2, then the match must be within 2 positions in the
              2 strings.
        The function returns zero in the following cases:
            * If the length argument is greater than the length of either string_column_expression1
              or string_column_expression2.
            * If the length argument is less than or equal to 0 or if either string_column_expression1 or
              string_column_expression2 is an empty string.
        Patterns beyond the length of 255 are ignored.
    PARAMETERS:
        string_or_column_expression1:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            If the argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, or CLOB
        string_or_column_expression2:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            If the argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, or CLOB
        length:
            Required Argument.
            Specifies an integer value ninn-gram, which is the comparison length.
        position:
            Optional Argument.
            Specifies an integer value that the n-gram is a positional n-gram match.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example returns n-gram value for character string in "stats" and "programming" column.
        # Import func from sqlalchemy to execute ngram function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> ngram_func_ = func.ngram(admissions_train.stats.expression, admissions_train.programming.expression, 3)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(ngram_col_=ngram_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  ngram_col_
        id
        13      no  4.00  Advanced      Novice         1           0
        26     yes  3.57  Advanced    Advanced         1           6
        5       no  3.44    Novice      Novice         0           4
        19     yes  1.98  Advanced    Advanced         0           6
        15     yes  4.00  Advanced    Advanced         1           6
        40     yes  3.95    Novice    Beginner         0           0
        7      yes  2.33    Novice      Novice         1           4
        22     yes  3.46    Novice    Beginner         0           0
        36      no  3.00  Advanced      Novice         0           0
        38     yes  2.65  Advanced    Beginner         1           0
        >>>
    nvp
    nvp
    Functions
    nvp(instring, name_to_search, name_delimiters, value_delimiters, occurrence)
    DESCRIPTION:
        Function extracts the value of a name-value pair where the name in the pair matches
        the name and the number of the occurrence specified.
    PARAMETERS:
        instring:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            containing the name-value pairs separated by multi-byte delimiters..
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
        name_to_search:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            whose instring value NVP returns.
        name_delimiters:
            Optional Argument.
            Specifies the multi-byte delimiters used to separate name-value pairs.
            Delimiters can contain any characters. They are separated from each
            other in the string by spaces. If a space is used as part of a delimiter,
            it must be escaped using a backslash (\). The maximum length of any
            delimiter is 10, and the maximum size of this parameter is 32.
            The default value is & (ampersand).
        value_delimiters:
            Optional Argument.
            Specifies the multi-byte delimiters used to associate a name to its value in a
            name-value pair.
            Delimiters can contain any characters. They are separated from each other in the
            string by spaces. If a space is used as part of a delimiter, it must be escaped
            using a backslash (\). The maximum length of any delimiter is 10, and the
            maximum size of this parameter is 32.
            The default value is = (equal sign).
        occurrence:
            Optional Argument.
            Specifies the number of occurrences of name_to_search that NVP searches for.
            The default value is 1.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "employee_info")
        >>>
        # Create a DataFrame on 'employee_info' table.
        >>> employee_info = DataFrame("employee_info")
        >>> employee_info
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Import func from sqlalchemy to execute nvp function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        # Retrieve nvp value for string literal "entree:orange chicken#entree2:honey salmon"
        # and return a new teradataml DataFrame by adding retrieved nvp value as a column
        # to admission_train
        >>> nvp_func_ = func.NVP('entree:orange chicken#entree2:honey salmon', 'entree','#', ':', 1)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = employee_info.assign(nvp_literal_=nvp_func_)
        >>> print(df)
                    first_name marks   dob joined_date    nvp_literal_
        employee_no
        101              abcde  None  None    02/12/05  orange chicken
        100               abcd  None  None        None  orange chicken
        112               None  None  None    18/12/05  orange chicken
        >>>
    oreplace
    oreplace
    Functions
    oreplace(source_string, search_string, replace_string)
    DESCRIPTION:
        Function replaces every occurrence of search_string in the source_string
        with the replace_string. Use this function either to replace or remove
        portions of a string.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            If argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, or CLOB
        search_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that the function searches for in source_string.
            If argument is null, then result is null.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, or CLOB
        replace_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that replaces the characters specified by search_string.
            If argument is NULL or is an empty string, or is omitted, all
            occurrences of search_string are removed from the source_string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, or CLOB
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example removes occurrence of "ce" in "stats" column.
        # Import func from sqlalchemy to execute oreplace function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> oreplace_func_ = func.oreplace(admissions_train.stats.expression, "ce")
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(oreplace_stats_=oreplace_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted oreplace_stats_
        id
        13      no  4.00  Advanced      Novice         1          Advand
        26     yes  3.57  Advanced    Advanced         1          Advand
        5       no  3.44    Novice      Novice         0            Novi
        19     yes  1.98  Advanced    Advanced         0          Advand
        15     yes  4.00  Advanced    Advanced         1          Advand
        40     yes  3.95    Novice    Beginner         0            Novi
        7      yes  2.33    Novice      Novice         1            Novi
        22     yes  3.46    Novice    Beginner         0            Novi
        36      no  3.00  Advanced      Novice         0          Advand
        38     yes  2.65  Advanced    Beginner         1          Advand
        >>>
    otranslate
    otranslate
    Functions
    otranslate(source_string, from_string, to_string)
    DESCRIPTION:
        Function returns source_string with every occurrence of each character in
        from_string replaced with the corresponding character in to_string.
        If the first character in from_string occurs in the source_string, all
        occurrences of it are replaced by the first character in to_string. This
        repeats for all characters in from_string and for all characters in
        to_string. The replacement is performed character-by-character, that is,
        the replacement of the second character is done on the string resulting
        from the replacement of the first character.
        If from_string contains more characters than to_string, the extra characters
        are removed from the source_string.
        If from_string contains fewer characters than to_string, the extra characters
        in to_string have no effect.
        If the same character occurs more than once in from_string, only the replacement
        character from the to_string corresponding to the first occurrence is used.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            If argument is NULL, NULL is returned.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR
        from_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that will be replaced in source_string.
            If argument is NULL, the function returns source_string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR
        to_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that replaces the characters specified by from_string.
            If argument is NULL or empty, the function removes the characters
            specified in from_string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example replaces the occurrence of from_string ('d') in the string in
        # "stats" column by the character in to_string ('X').
        # Import func from sqlalchemy to execute otranslate function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> otranslate_func_ = func.otranslate(admissions_train.stats.expression, "d", "X")
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(otranslate_stats_=otranslate_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted otranslate_stats_
        id
        22     yes  3.46    Novice    Beginner         0            Novice
        26     yes  3.57  Advanced    Advanced         1          AXvanceX
        5       no  3.44    Novice      Novice         0            Novice
        17      no  3.83  Advanced    Advanced         1          AXvanceX
        13      no  4.00  Advanced      Novice         1          AXvanceX
        19     yes  1.98  Advanced    Advanced         0          AXvanceX
        36      no  3.00  Advanced      Novice         0          AXvanceX
        15     yes  4.00  Advanced    Advanced         1          AXvanceX
        34     yes  3.85  Advanced    Beginner         0          AXvanceX
        38     yes  2.65  Advanced    Beginner         1          AXvanceX
        >>>
    reverse
    reverse
    Functions
    reverse(source_string)
    DESCRIPTION:
        Function reverses the input string.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal to be reversed.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, and CLOB
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example reverses strings in "stats" column.
        # Import func from sqlalchemy to execute reverse function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> reverse_func_ = func.reverse(admissions_train.stats.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(reverse_stats_=reverse_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted reverse_stats_
        id
        13      no  4.00  Advanced      Novice         1       decnavdA
        26     yes  3.57  Advanced    Advanced         1       decnavdA
        5       no  3.44    Novice      Novice         0         ecivoN
        19     yes  1.98  Advanced    Advanced         0       decnavdA
        15     yes  4.00  Advanced    Advanced         1       decnavdA
        40     yes  3.95    Novice    Beginner         0         ecivoN
        7      yes  2.33    Novice      Novice         1         ecivoN
        22     yes  3.46    Novice    Beginner         0         ecivoN
        36      no  3.00  Advanced      Novice         0       decnavdA
        38     yes  2.65  Advanced    Beginner         1       decnavdA
        >>>
    right
    right
    Functions
    right(source_string, length)
    DESCRIPTION:
        Starting from the end of the input string, a substring is created with the
        number of characters specified by the second parameter.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal to
            create substring from.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, and CLOB
        length:
            Required Argument.
            Specifies the number of characters desired from the right side of the string. If this
            number exceeds the number of characters in the source_string, then the original
            source_string is returned.
            This argument must be a positive integer.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    ALTERNATE NAME:
        td_right
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example truncates values in "programming" column to a length of 3.
        # Import func from sqlalchemy to execute right function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> right_func_ = func.right(admissions_train.programming.expression, 3)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(right_3_programming_=right_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted right_3_programming_
        id
        22     yes  3.46    Novice    Beginner         0                  ner
        26     yes  3.57  Advanced    Advanced         1                  ced
        5       no  3.44    Novice      Novice         0                  ice
        17      no  3.83  Advanced    Advanced         1                  ced
        13      no  4.00  Advanced      Novice         1                  ice
        19     yes  1.98  Advanced    Advanced         0                  ced
        36      no  3.00  Advanced      Novice         0                  ice
        15     yes  4.00  Advanced    Advanced         1                  ced
        34     yes  3.85  Advanced    Beginner         0                  ner
        38     yes  2.65  Advanced    Beginner         1                  ner
        >>>
    rpad
    rpad
    Functions
    rpad(source_string, length, ﬁll_string)
    DESCRIPTION:
        Function returns the source_string padded to the right with the characters
        in fill_string so that the resulting string has 'length' characters.
    PARAMETERS:
        source_string:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to be padded. If the length of source_string is greater than 'length',
            source_string is truncated to 'length' characters.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: VARCHAR, or CLOB
        length:
            Required Argument.
            Specifies a ColumnExpression of an int column or an integer literal
            specifying the number of characters in the resulting string.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: INTEGER, BIGINT, or NUMBER
        fill_string:
            Optional Argument.
            Specifies a ColumnExpression of a string column or a string literal
            used to pad the source_string.
            The sequence of characters in fill_string is replicated as necessary.
            If argument is not specified, source_string will be padded to the right
            with space characters.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR, VARCHAR, or CLOB
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Import func from sqlalchemy to execute rpad function.
        >>> from sqlalchemy import func
        # Example 1: Pad string in "stats" column with 0.
        # Create a sqlalchemy Function object.
        >>> rpad_func_ = func.rpad(admissions_train.stats.expression, 10, "0")
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(rpad_gpa_=rpad_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted   rpad_gpa_
        id
        5       no  3.44    Novice      Novice         0  Novice0000
        34     yes  3.85  Advanced    Beginner         0  Advanced00
        13      no  4.00  Advanced      Novice         1  Advanced00
        40     yes  3.95    Novice    Beginner         0  Novice0000
        22     yes  3.46    Novice    Beginner         0  Novice0000
        19     yes  1.98  Advanced    Advanced         0  Advanced00
        36      no  3.00  Advanced      Novice         0  Advanced00
        15     yes  4.00  Advanced    Advanced         1  Advanced00
        7      yes  2.33    Novice      Novice         1  Novice0000
        17      no  3.83  Advanced    Advanced         1  Advanced00
        >>>
        # Example 2: Pad string in "stats" column with strings from "masters" column.
        # Create a sqlalchemy Function object.
        >>> rpad_func_ = func.rpad(admissions_train.stats.expression, 20, admissions_train.masters.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(rpad_gpa_=rpad_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted             rpad_gpa_
        id
        13      no  4.00  Advanced      Novice         1  Advancednononononono
        26     yes  3.57  Advanced    Advanced         1  Advancedyesyesyesyes
        5       no  3.44    Novice      Novice         0  Novicenonononononono
        19     yes  1.98  Advanced    Advanced         0  Advancedyesyesyesyes
        15     yes  4.00  Advanced    Advanced         1  Advancedyesyesyesyes
        40     yes  3.95    Novice    Beginner         0  Noviceyesyesyesyesye
        7      yes  2.33    Novice      Novice         1  Noviceyesyesyesyesye
        22     yes  3.46    Novice    Beginner         0  Noviceyesyesyesyesye
        36      no  3.00  Advanced      Novice         0  Advancednononononono
        38     yes  2.65  Advanced    Beginner         1  Advancedyesyesyesyes
        >>>
    rtrim
    rtrim
    Functions
    rtrim(expression1, expression2)
    DESCRIPTION:
        Function returns the argument expression1, with its right-most characters removed up
        to the first character that is not in the argument expression2.
    PARAMETERS:
        expression1:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            If argument is NULL, NULL is returned.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
        expression2:
            Optional Argument.
            Specifies a ColumnExpression of a string column or a string literal that
            will be removed from string in expression1. If expression2 is specified, it must
            be the same data type as expression1. If expression2 is not specified, the default is
            to use a single space character.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Note:
                Column expression of both arguments can be of following type:
                    a. Character/String types: CHAR, VARCHAR, or CLOB
                    b. Integer types: BYTEINT, SMALLINT, INTEGER, or BIGINT
                    c. Numeric types: FLOAT, REAL, DOUBLE PRECISION, DECIMAL, NUMERIC, or NUMBER
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example right trim string in "stats" column if it has "ced"
        # Import func from sqlalchemy to execute rtrim function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> rtrim_func_ = func.rtrim(admissions_train.stats.expression, "ced")
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(rtrim_stats_=rtrim_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted rtrim_stats_
        id
        5       no  3.44    Novice      Novice         0       Novi
        34     yes  3.85  Advanced    Beginner         0      Advan
        13      no  4.00  Advanced      Novice         1      Advan
        40     yes  3.95    Novice    Beginner         0       Novi
        22     yes  3.46    Novice    Beginner         0       Novi
        19     yes  1.98  Advanced    Advanced         0      Advan
        36      no  3.00  Advanced      Novice         0      Advan
        15     yes  4.00  Advanced    Advanced         1      Advan
        7      yes  2.33    Novice      Novice         1       Novi
        17      no  3.83  Advanced    Advanced         1      Advan
        >>>
    soundex
    soundex
    Functions
    soundex(string_expression)
    DESCRIPTION:
        Function returns a character string that represents the Soundex code for
        string_expression.
        Soundex is a system that codes surnames having the same or similar sounds,
        but variant spellings. The Soundex system was first used by the National
        Archives in 1880 to index the United States census.
        Soundex codes begin with the first letter of the surname followed by a three-digit
        code. Zeros are added to names that do not have enough letters.
    PARAMETERS:
        string_expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            that contains a surname to be evaluated in simple Latin characters.
            Soundex is case insensitive.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example returns the Soundex code for character string in "programming" column.
        # Import func from sqlalchemy to execute soundex function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> soundex_func_ = func.soundex(admissions_train.programming.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(soundex_programming_=soundex_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted soundex_programming_
        id
        13      no  4.00  Advanced      Novice         1                 N120
        26     yes  3.57  Advanced    Advanced         1                 A315
        5       no  3.44    Novice      Novice         0                 N120
        19     yes  1.98  Advanced    Advanced         0                 A315
        15     yes  4.00  Advanced    Advanced         1                 A315
        40     yes  3.95    Novice    Beginner         0                 B256
        7      yes  2.33    Novice      Novice         1                 N120
        22     yes  3.46    Novice    Beginner         0                 B256
        36      no  3.00  Advanced      Novice         0                 N120
        38     yes  2.65  Advanced    Beginner         1                 B256
        >>>
    string_cs
    string_cs
    Functions
    string_cs(string_expression)
    DESCRIPTION:
        Function returns a heuristically derived integer value that you can use to help determine
        which KANJI1-compatible client character set was used to encode string_expression.
        The result value can also help determine which client character set to use to interpret
        the character data.
        ===========================================================================================
        | IF the result value is …  | THEN the heuristic found that string_expression  …            |
         ===========================================================================================
        |   -1                      |   most likely uses a single-byte client character set         |
        |                           |   encoding, but it may also contain a mix of encodings.       |
         -------------------------------------------------------------------------------------------
        |   0                       |   does not contain anything distinguishable from any          |
        |                           |   particular character set, so any character set that you use |
        |                           |   to interpret string_expression provides the same result.    |
        |                           |   Not all translations use the same interpretation for the    |
        |                           |   characters represented by 0x5C and 0x7E, however.           |
        |                           |   If string_expression contains:                              |
        |                           |       * 0x5C and you want it to be interpreted as             |
        |                           |         REVERSE SOLIDUS, use a single-byte character set.     |
        |                           |       * 0x7E and you want it to be interpreted as TILDE, use  |
        |                           |         a single-byte character set.                          |
        |                           |       * 0x5C and you want it to be interpreted as YEN SIGN,   |
        |                           |       * 0x7E and you want it to be interpreted as OVERLINE,   |
        |                           |         use any of the following:                             |
        |                           |           * KANJISJIS_0S                                      |
        |                           |           * KANJIEBCDIC5026_0I                                |
        |                           |           * KANJIEBCDIC5035_0I                                |
        |                           |           * KATAKANAEBCDIC                                    |
        |                           |           * KANJIEUC_0U                                       |
         -------------------------------------------------------------------------------------------
        |   1                       |   uses the encoding of one of the following:                  |
        |                           |       * KANJIEBCDIC5026_0I                                    |
        |                           |       * KANJIEBCDIC5035_0I                                    |
        |                           |       * KATAKANAEBCDIC                                        |
         -------------------------------------------------------------------------------------------
        |   2                       |   uses the encoding of KANJIEUC_0U.                           |
         -------------------------------------------------------------------------------------------
        |   3                       |   uses the encoding of KANJISJIS_0S.                          |
         -------------------------------------------------------------------------------------------
        Function helps determine which encoding to use when using the TRANSLATE function to
        translate a string from the KANJI1 server character set to the UNICODE server character set.
         ===========================================================================================
        | IF the result value is …  | THEN substitute the following value for source_TO_target in   |
        |                           | TRANSLATE(string_expression USING source_to_target ) …        |
         ===========================================================================================
        |   -1                      |   KANJI1_SBC_TO_UNICODE.                                      |
         -------------------------------------------------------------------------------------------
        |   0                       |   KANJI1_SBC_TO_UNICODE.                                      |
         -------------------------------------------------------------------------------------------
        |   1                       |   KANJI1_KANJIEBCDIC_TO_UNICODE.                              |
         -------------------------------------------------------------------------------------------
        |   2                       |   KANJI1_KANJIEUC_TO_UNICODE.                                 |
         -------------------------------------------------------------------------------------------
        |   3                       |   KANJI1_KANJISJIS_TO_UNICODE.                                |
         -------------------------------------------------------------------------------------------
    PARAMETERS:
        string_expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
            Supported column types: CHAR and VARCHAR
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example returns the heuristically derived integer value for character string in "stats" column.
        # Import func from sqlalchemy to execute string_cs function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> string_cs_func_ = func.string_cs(admissions_train.stats.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(string_cs_stats_=string_cs_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted  string_cs_stats_
        id
        5       no  3.44    Novice      Novice         0                 0
        34     yes  3.85  Advanced    Beginner         0                 0
        13      no  4.00  Advanced      Novice         1                 0
        40     yes  3.95    Novice    Beginner         0                 0
        22     yes  3.46    Novice    Beginner         0                 0
        19     yes  1.98  Advanced    Advanced         0                 0
        36      no  3.00  Advanced      Novice         0                 0
        15     yes  4.00  Advanced    Advanced         1                 0
        7      yes  2.33    Novice      Novice         1                 0
        17      no  3.83  Advanced    Advanced         1                 0
        >>>
    upper
    upper
    Functions
    upper(character_string_expression)
    DESCRIPTION:
        Function returns a character string identical to character_string_expression,
        except that all lowercase letters are replaced with their uppercase equivalents.
    PARAMETERS:
        character_string_expression:
            Required Argument.
            Specifies a ColumnExpression of a string column or a string literal
            to be converted to uppercase.
            Format of a ColumnExpression of a string column: '<dataframe>.<dataframe_column>.expression'.
    NOTE:
        Function accepts positional arguments only.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", "admissions_train")
        >>>
        # Create a DataFrame on 'admissions_train' table.
        >>> admissions_train = DataFrame("admissions_train")
        >>> admissions_train
           masters   gpa     stats programming  admitted
        id
        22     yes  3.46    Novice    Beginner         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        38     yes  2.65  Advanced    Beginner         1
        5       no  3.44    Novice      Novice         0
        17      no  3.83  Advanced    Advanced         1
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        26     yes  3.57  Advanced    Advanced         1
        19     yes  1.98  Advanced    Advanced         0
        >>>
        # Example converts strings in "stats" column to uppercase.
        # Import func from sqlalchemy to execute upper function.
        >>> from sqlalchemy import func
        # Create a sqlalchemy Function object.
        >>> upper_func_ = func.upper(admissions_train.stats.expression)
        >>>
        # Pass the Function object as input to DataFrame.assign().
        >>> df = admissions_train.assign(upper_stats_=upper_func_)
        >>> print(df)
           masters   gpa     stats programming  admitted upper_stats_
        id
        13      no  4.00  Advanced      Novice         1     ADVANCED
        26     yes  3.57  Advanced    Advanced         1     ADVANCED
        5       no  3.44    Novice      Novice         0       NOVICE
        19     yes  1.98  Advanced    Advanced         0     ADVANCED
        15     yes  4.00  Advanced    Advanced         1     ADVANCED
        40     yes  3.95    Novice    Beginner         0       NOVICE
        7      yes  2.33    Novice      Novice         1       NOVICE
        22     yes  3.46    Novice    Beginner         0       NOVICE
        36      no  3.00  Advanced      Novice         0     ADVANCED
        38     yes  2.65  Advanced    Beginner         1     ADVANCED
        >>>