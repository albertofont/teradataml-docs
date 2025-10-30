# teradataml Window Aggregates

corr
teradataml.dataframe.window.corr = corr(expression)
DESCRIPTION:
    Function returns the Sample Pearson product moment correlation coefficient
    of its arguments for all non-null data point pairs in a teradataml
    DataFrame or ColumnExpression over the specified window.
    The Sample Pearson product moment correlation coefficient is a measure of
    the linear association between variables. The boundary on the computed
    coefficient ranges from -1.00 to +1.00.
    Note that high correlation does not imply a causal relationship between
    the variables.
    The coefficient of correlation between two variables has the following four extreme values:
        1. -1.00 : Association between the variables is perfectly linear, but inverse.
                   As the value in ColumnExpression varies, the value for
                   "expression" varies identically in the opposite direction.
        2. 0     : Association between the variables does not exist and they are considered
                   to be uncorrelated.
        3. +1.00 : Association between the variables is perfectly linear.
                   As the value in ColumnExpression varies, the value for
                   "expression" varies identically in the same direction.
        4. NULL  : Association between the variables cannot be measured because there
                   are no non-null data point pairs in the data used for the computation.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a numeric column or name of the column or
        a numeric literal to be correlated with ColumnExpression.
        Types: ColumnExpression OR str OR int OR float
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the correlation for the 'gpa' and 'admitted'
    #            in a Rolling window, partitioned over programming.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns=admissions_train.programming,
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute corr() on the Rolling window and attach it to the teradataml
    # DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing corr() along with
    #       count() window aggregate operations.
    >>> df = admissions_train.assign(corr_gpa_admitted=window.corr(admissions_train.admitted),
    ...                              count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  corr_gpa_admitted  count_gpa
    id
    11      no  3.13  Advanced    Advanced         1           0.927146          3
    27     yes  3.96  Advanced    Advanced         0          -0.793047          3
    26     yes  3.57  Advanced    Advanced         1          -0.292306          3
    6      yes  3.50  Beginner    Advanced         1          -0.989980          3
    9       no  3.82  Advanced    Advanced         1                NaN          3
    25      no  3.96  Advanced    Advanced         1                NaN          3
    39     yes  3.75  Advanced    Beginner         0                NaN          1
    31     yes  3.50  Advanced    Beginner         1          -1.000000          2
    29     yes  4.00    Novice    Beginner         0          -0.866025          3
    21      no  3.87    Novice    Beginner         1          -0.701039          3
    >>>
    # Example 2: Calculate the correlation between all the valid columns
    #            and 'gpa' in teradataml DataFrame, in an Expanding window,
    #            partitioned over 'programming', and order by 'id' in descending
    #            order.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute corr() on the Expanding window.
    >>> df = window.corr(admissions_train.gpa)
    >>> df
       masters   gpa     stats programming  admitted  admitted_corr  gpa_corr   id_corr
    id
    35      no  3.68    Novice    Beginner         1       0.974355       1.0 -0.225018
    28      no  3.93  Advanced    Advanced         1       0.880019       1.0 -0.691040
    25      no  3.96  Advanced    Advanced         1       0.848441       1.0 -0.768352
    24      no  1.87  Advanced      Novice         1       0.216553       1.0  0.251225
    17      no  3.83  Advanced    Advanced         1       0.262403       1.0 -0.084948
    16      no  3.70  Advanced    Advanced         1       0.271885       1.0 -0.131117
    40     yes  3.95    Novice    Beginner         0            NaN      -1.0       NaN
    39     yes  3.75  Advanced    Beginner         0            NaN       1.0  1.000000
    38     yes  2.65  Advanced    Beginner         1      -0.989743       1.0  0.928571
    34     yes  3.85  Advanced    Beginner         0      -0.990867       1.0 -0.041862
    >>>
    # Example 3: Calculate the correlation between 'gpa' and all the valid columns
    #            in teradataml DataFrame, which are grouped by 'masters' and 'gpa'
    #            in a Contracting window, partitioned over 'masters' and order by
    #            'masters' with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             order_columns=group_by_df.masters.nulls_first(),
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute corr() on the Contracting window.
    >>> window.corr(admissions_train.gpa)
      masters   gpa  gpa_corr
    0      no  3.71       1.0
    1      no  3.52       1.0
    2      no  3.68       1.0
    3      no  3.83       1.0
    4      no  3.55       1.0
    5      no  3.96       1.0
    6     yes  3.59       1.0
    7     yes  3.95       1.0
    8     yes  3.46       1.0
    9     yes  3.76       1.0
    >>>
count
teradataml.dataframe.window.count = count(distinct=False, skipna=False)
DESCRIPTION:
    Function returns the total number of qualified rows in a teradataml
    DataFrame or ColumnExpression over the specified window.
PARAMETERS:
    distinct:
        Optional Argument.
        Specifies a flag that decides whether to consider duplicate values in
        a column or not.
        Default Values: False
        Types: bool
    skipna:
        Optional Argument.
        Specifies a flag that decides whether to skip null values or not.
        Default Values: False
        Types: bool
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the count of values for the 'gpa' column
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns=admissions_train.programming,
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute count() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing count() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(count_gpa=window.count(), max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  max_gpa
    id
    11      no  3.13  Advanced    Advanced         1          3     1.98
    27     yes  3.96  Advanced    Advanced         0          3     3.13
    26     yes  3.57  Advanced    Advanced         1          3     3.45
    6      yes  3.50  Beginner    Advanced         1          3     3.50
    9       no  3.82  Advanced    Advanced         1          3     3.50
    25      no  3.96  Advanced    Advanced         1          3     3.60
    39     yes  3.75  Advanced    Beginner         0          1     3.75
    31     yes  3.50  Advanced    Beginner         1          2     3.50
    29     yes  4.00    Novice    Beginner         0          3     3.50
    21      no  3.87    Novice    Beginner         1          3     3.50
    >>>
    # Example 2: Calculate the count of all the valid columns in teradataml
    #            DataFrame, in an Expanding window, partitioned over 'programming',
    #            and order by 'id'.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id,
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute count() on the Expanding window.
    >>> df = window.count()
    >>> df
       masters   gpa     stats programming  admitted  admitted_count  gpa_count  id_count  masters_count  programming_count  st
    id
    4      yes  3.50  Beginner      Novice         1               3          3         3              3                  3    
    7      yes  2.33    Novice      Novice         1               5          5         5              5                  5    
    14     yes  3.45  Advanced    Advanced         0               6          6         6              6                  6    
    15     yes  4.00  Advanced    Advanced         1               7          7         7              7                  7    
    19     yes  1.98  Advanced    Advanced         0               9          9         9              9                  9    
    20     yes  3.90  Advanced    Advanced         1              10         10        10             10                 10    
    3       no  3.70    Novice    Beginner         1               1          1         1              1                  1    
    5       no  3.44    Novice      Novice         0               2          2         2              2                  2    
    8       no  3.60  Beginner    Advanced         1               3          3         3              3                  3    
    9       no  3.82  Advanced    Advanced         1               4          4         4              4                  4    
    >>>
    # Example 3: Calculate the count of all the valid columns in teradataml
    #            DataFrame, which are grouped by 'masters' and 'gpa' in a
    #            Contracting window, partitioned over 'masters'.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute count() on the Contracting window.
    >>> window.count()
      masters   gpa  gpa_count  masters_count
    0     yes  3.79          8              8
    1     yes  3.50         10             10
    2     yes  3.96         11             11
    3     yes  4.00         12             12
    4     yes  3.90         14             14
    5     yes  2.33         15             15
    6      no  3.52          6              6
    7      no  3.83          7              7
    8      no  3.82          8              8
    9      no  3.55          9              9
    >>>
covar_pop
teradataml.dataframe.window.covar_pop = covar_pop(expression)
DESCRIPTION:
    Function returns the population covariance of its arguments for all
    non-null data point pairs over the specified window.
    Covariance measures whether or not two random variables vary in the
    same way. It is the average of the products of deviations for each
    non-null data point pair. The function considers ColumnExpression
    as one variable and "expression" as another variable for calculating
    population covariance.
    Notes:
        1. When there are no non-null data point pairs in the data used for
           the computation, the function returns None.
        2. High covariance does not imply a causal relationship between
           the variables.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a numeric column or name of the column
        or a numeric literal to be paired with another variable to determine
        their population covariance.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the population covariance for 'gpa' and 'admitted'
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns=admissions_train.programming,
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute covar_pop() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing covar_pop() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(covar_pop_gpa=window.covar_pop(admissions_train.admitted),
    ...                              max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  covar_pop_gpa  max_gpa
    id
    15     yes  4.00  Advanced    Advanced         1       0.000000     4.00
    16      no  3.70  Advanced    Advanced         1       0.000000     4.00
    11      no  3.13  Advanced    Advanced         1       0.000000     3.96
    9       no  3.82  Advanced    Advanced         1       0.000000     3.82
    19     yes  1.98  Advanced    Advanced         0       0.373333     3.82
    27     yes  3.96  Advanced    Advanced         0       0.117778     3.96
    1      yes  3.95  Beginner    Beginner         0       0.000000     3.95
    34     yes  3.85  Advanced    Beginner         0       0.000000     3.95
    32     yes  3.46  Advanced    Beginner         0       0.000000     3.95
    40     yes  3.95    Novice    Beginner         0       0.000000     3.95
    >>>
    # Example 2: Calculate covariance population between 'admitted' and all the
    #            valid columns in teradataml DataFrame, in an Expanding window,
    #            partitioned over 'masters', and order by 'id'.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id,
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute covar_pop() on Expanding window.
    >>> df = window.covar_pop(admissions_train.admitted)
    >>> df
       masters   gpa     stats programming  admitted  admitted_covar_pop  gpa_covar_pop  id_covar_pop
    id
    4      yes  3.50  Beginner      Novice         1            0.222222      -0.078889      0.555556
    7      yes  2.33    Novice      Novice         1            0.240000      -0.178800      1.000000
    14     yes  3.45  Advanced    Advanced         0            0.250000      -0.152500      0.000000
    15     yes  4.00  Advanced    Advanced         1            0.244898      -0.094898      0.571429
    19     yes  1.98  Advanced    Advanced         0            0.246914       0.035309      0.246914
    20     yes  3.90  Advanced    Advanced         1            0.240000       0.053200      0.640000
    3       no  3.70    Novice    Beginner         1            0.000000       0.000000      0.000000
    5       no  3.44    Novice      Novice         0            0.250000       0.065000     -0.500000
    8       no  3.60  Beginner    Advanced         1            0.222222       0.046667      0.111111
    9       no  3.82  Advanced    Advanced         1            0.187500       0.050000      0.312500
    >>>
    # Example 3: Calculate covariance population between 'admitted' and all the
    #            valid columns in a teradataml DataFrame, which are grouped by
    #            'masters', 'admitted' and 'gpa' in a Contracting window,
    #            partitioned over 'masters'.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "admitted", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute covar_pop() on Contracting window.
    >>> window.covar_pop(admissions_train.admitted)
      masters  admitted   gpa  admitted_covar_pop  gpa_covar_pop
    0     yes         1  3.50            0.250000       0.021875
    1     yes         1  3.59            0.250000      -0.063500
    2     yes         1  4.00            0.247934      -0.074628
    3     yes         1  2.65            0.243056      -0.071250
    4     yes         0  3.95            0.244898      -0.052449
    5     yes         1  2.33            0.248889      -0.036178
    6      no         1  3.65            0.000000       0.000000
    7      no         1  3.87            0.000000       0.000000
    8      no         1  3.71            0.000000       0.000000
    9      no         1  3.93            0.000000       0.000000
    >>>
covar_samp
teradataml.dataframe.window.covar_samp = covar_samp(expression)
DESCRIPTION:
    Function returns the sample covariance of its arguments for all
    non-null data point pairs over the specified window.
    Covariance measures whether or not two random variables vary in the
    same way. It is the average of the products of deviations for each
    non-null data point pair. The function considers ColumnExpression as
    one variable and "expression" as another variable for calculating sample
    covariance.
    Notes:
        1. When there are no non-null data point pairs in the data used for
           the computation, the function returns None.
        2. High covariance does not imply a causal relationship between
           the variables.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a numeric column or name of the column
        or a numeric literal to be paired with another variable to determine
        their sample covariance.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the sample covariance for 'gpa' and 'admitted'
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns=admissions_train.programming,
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute covar_samp() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing covar_samp() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(covar_pop_gpa=window.covar_pop(admissions_train.admitted),
    ...                              max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  covar_samp_gpa  max_gpa
    id
    15     yes  4.00  Advanced    Advanced         1        0.000000     4.00
    16      no  3.70  Advanced    Advanced         1        0.000000     4.00
    11      no  3.13  Advanced    Advanced         1        0.000000     3.96
    9       no  3.82  Advanced    Advanced         1        0.000000     3.82
    19     yes  1.98  Advanced    Advanced         0        0.560000     3.82
    27     yes  3.96  Advanced    Advanced         0        0.176667     3.96
    1      yes  3.95  Beginner    Beginner         0             NaN     3.95
    34     yes  3.85  Advanced    Beginner         0        0.000000     3.95
    32     yes  3.46  Advanced    Beginner         0        0.000000     3.95
    40     yes  3.95    Novice    Beginner         0        0.000000     3.95
    >>>
    # Example 2: Calculate covariance sample between all the valid columns
    #            and 'admitted' in teradataml DataFrame, in an Expanding window,
    #            partitioned over 'masters', and order by 'id'.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id,
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute covar_samp() on Expanding window.
    >>> df = window.covar_samp(admissions_train.admitted)
    >>> df
       masters   gpa     stats programming  admitted  admitted_covar_samp  gpa_covar_samp  id_covar_samp
    id
    4      yes  3.50  Beginner      Novice         1             0.333333       -0.118333       0.833333
    7      yes  2.33    Novice      Novice         1             0.300000       -0.223500       1.250000
    14     yes  3.45  Advanced    Advanced         0             0.300000       -0.183000       0.000000
    15     yes  4.00  Advanced    Advanced         1             0.285714       -0.110714       0.666667
    19     yes  1.98  Advanced    Advanced         0             0.277778        0.039722       0.277778
    20     yes  3.90  Advanced    Advanced         1             0.266667        0.059111       0.711111
    3       no  3.70    Novice    Beginner         1                  NaN             NaN            NaN
    5       no  3.44    Novice      Novice         0             0.500000        0.130000      -1.000000
    8       no  3.60  Beginner    Advanced         1             0.333333        0.070000       0.166667
    9       no  3.82  Advanced    Advanced         1             0.250000        0.066667       0.416667
    >>>
    # Example 3: Calculate covariance sample between all the valid columns
    #            and 'admitted' in a teradataml DataFrame, which are grouped by
    #            'masters', 'admitted' and 'gpa' in a Contracting window,
    #            partitioned over 'masters'.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "admitted", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute covar_samp() on Contracting window.
    >>> window.covar_samp(admissions_train.admitted)
      masters  admitted   gpa  admitted_covar_samp  gpa_covar_samp
    0     yes         1  3.81             0.285714       -0.212857
    1     yes         1  2.65             0.266667       -0.192444
    2     yes         0  3.45             0.254545       -0.170727
    3     yes         0  3.95             0.242424       -0.108485
    4     yes         0  3.75             0.247253       -0.079286
    5     yes         0  3.79             0.257143       -0.050571
    6      no         1  3.65             0.000000        0.000000
    7      no         1  3.87             0.000000        0.000000
    8      no         1  3.71             0.125000        0.048929
    9      no         1  3.93             0.194444        0.118889
    >>>
cume_dist
teradataml.dataframe.window.cume_dist = cume_dist()
DESCRIPTION:
    Function returns the cumulative distribution of values in a teradataml
    DataFrame or ColumnExpression over the specified window.
    Notes:
         1. Window parameter "order_columns" should not be None.
         2. Unlike other window aggregate functions, executing cume_dist()
            on a window created on teradataml DataFrame does not create
            multiple columns, that is, it results in one new column in addition
            to the original teradataml DataFrame columns. For calculating
            cumulative distribution, value passed to "order_columns" parameter
            of window is considered and not the ColumnExpression.
PARAMETERS:
    None.
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the cumulative distribution over the values in a
    #            window, partitioned over 'programming' and sort by 'id'.
    # Create a window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns=admissions_train.programming,
    ...                                      order_columns=admissions_train.id)
    # Execute cume_dist() on the window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing cume_dist() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(cume_dist=window.cume_dist(), max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  cume_dist  max_gpa
    id
    3       no  3.70    Novice    Beginner         1   0.230769      4.0
    22     yes  3.46    Novice    Beginner         0   0.384615      4.0
    29     yes  4.00    Novice    Beginner         0   0.461538      4.0
    31     yes  3.50  Advanced    Beginner         1   0.538462      4.0
    34     yes  3.85  Advanced    Beginner         0   0.692308      4.0
    35      no  3.68    Novice    Beginner         1   0.769231      4.0
    6      yes  3.50  Beginner    Advanced         1   0.062500      4.0
    8       no  3.60  Beginner    Advanced         1   0.125000      4.0
    9       no  3.82  Advanced    Advanced         1   0.187500      4.0
    10      no  3.71  Advanced    Advanced         1   0.250000      4.0
    >>>
    # Example 2: Calculate the cumulative distribution on the window
    #            created on teradataml DataFrame, partitioned over 'masters',
    #            and order by 'id'.
    # Create a window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id)
    >>>
    # Execute cume_dist() on window.
    >>> df = window.cume_dist()
    >>> df
       masters   gpa     stats programming  admitted  col_cume_dist
    id
    4      yes  3.50  Beginner      Novice         1       0.136364
    7      yes  2.33    Novice      Novice         1       0.227273
    14     yes  3.45  Advanced    Advanced         0       0.272727
    15     yes  4.00  Advanced    Advanced         1       0.318182
    19     yes  1.98  Advanced    Advanced         0       0.409091
    20     yes  3.90  Advanced    Advanced         1       0.454545
    3       no  3.70    Novice    Beginner         1       0.055556
    5       no  3.44    Novice      Novice         0       0.111111
    8       no  3.60  Beginner    Advanced         1       0.166667
    9       no  3.82  Advanced    Advanced         1       0.222222
    >>>
dense_rank
teradataml.dataframe.window.dense_rank = dense_rank()
DESCRIPTION:
    Function returns the ordered ranking of all the rows in a teradataml
    DataFrame or ColumnExpression, according to "order_columns", over the
    specified window.
    Notes:
         1. Window parameter "order_columns" should not be None.
         2. Unlike other window aggregate functions, executing dense_rank()
            on a window created on teradataml DataFrame does not create
            multiple columns, that is, it results in one new column in addition
            to the original teradataml DataFrame columns. For calculating
            dense_rank, value passed to "order_columns" parameter of window
            is considered and not the ColumnExpression.
PARAMETERS:
    None.
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Get the ordered ranking of rows over the values in a window,
    #            partitioned over 'programming' and sort by 'gpa'.
    # Create a window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      order_columns="gpa")
    # Execute dense_rank() on the window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing dense_rank() along
    #       with mean() window aggregate operations.
    >>> df = admissions_train.assign(dense_rank=window.dense_rank(), mean_gpa=window.mean())
    >>> df
       masters   gpa     stats programming  admitted  dense_rank  mean_gpa
    id
    32     yes  3.46  Advanced    Beginner         0           2  3.660000
    35      no  3.68    Novice    Beginner         1           4  3.660000
    3       no  3.70    Novice    Beginner         1           5  3.660000
    39     yes  3.75  Advanced    Beginner         0           6  3.660000
    34     yes  3.85  Advanced    Beginner         0           8  3.660000
    21      no  3.87    Novice    Beginner         1           9  3.660000
    19     yes  1.98  Advanced    Advanced         0           1  3.615625
    11      no  3.13  Advanced    Advanced         1           2  3.615625
    14     yes  3.45  Advanced    Advanced         0           3  3.615625
    6      yes  3.50  Beginner    Advanced         1           4  3.615625
    >>>
    # Example 2: Get the ordered ranking of rows on window created on
    #            teradataml DataFrame, partitioned over 'masters',
    #            and order by 'gpa'.
    # Create a window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.gpa)
    >>>
    # Execute dense_rank() on a window.
    >>> df = window.dense_rank()
    >>> df
       masters   gpa     stats programming  admitted  col_dense_rank
    id
    38     yes  2.65  Advanced    Beginner         1               3
    32     yes  3.46  Advanced    Beginner         0               5
    22     yes  3.46    Novice    Beginner         0               5
    4      yes  3.50  Beginner      Novice         1               6
    6      yes  3.50  Beginner    Advanced         1               6
    26     yes  3.57  Advanced    Advanced         1               7
    24      no  1.87  Advanced      Novice         1               1
    36      no  3.00  Advanced      Novice         0               2
    11      no  3.13  Advanced    Advanced         1               3
    5       no  3.44    Novice      Novice         0               4
    >>>
first_value
teradataml.dataframe.window.ﬁrst_value = ﬁrst_value()
DESCRIPTION:
    Function returns the first value of an ordered set of values in a teradataml
    DataFrame or ColumnExpression over the specified window.
PARAMETERS:
    None.
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the first value for the 'gpa' column
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute first_value() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing first_value() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(first_value_gpa=window.first_value(), max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  first_value_gpa  max_gpa
    id
    15     yes  4.00  Advanced    Advanced         1             3.60     4.00
    16      no  3.70  Advanced    Advanced         1             4.00     4.00
    11      no  3.13  Advanced    Advanced         1             3.96     3.96
    9       no  3.82  Advanced    Advanced         1             3.70     3.82
    19     yes  1.98  Advanced    Advanced         0             3.82     3.82
    27     yes  3.96  Advanced    Advanced         0             3.50     3.96
    1      yes  3.95  Beginner    Beginner         0             3.95     3.95
    34     yes  3.85  Advanced    Beginner         0             3.95     3.95
    32     yes  3.46  Advanced    Beginner         0             3.95     3.95
    40     yes  3.95    Novice    Beginner         0             3.85     3.95
    >>>
    # Example 2: Calculate the first value of all the valid columns in teradataml
    #            DataFrame, in an Expanding window, partitioned over 'masters',
    #            and order by 'id' in descending order.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute first_value() on Expanding window.
    >>> df = window.first_value()
    >>> df
       masters   gpa     stats programming  admitted  admitted_first_value  gpa_first_value  id_first_value masters_first_value
    id
    4      yes  3.50  Beginner      Novice         1                     0             3.95               1                 yes
    7      yes  2.33    Novice      Novice         1                     0             3.95               1                 yes
    14     yes  3.45  Advanced    Advanced         0                     0             3.95               1                 yes
    15     yes  4.00  Advanced    Advanced         1                     0             3.95               1                 yes
    19     yes  1.98  Advanced    Advanced         0                     0             3.95               1                 yes
    20     yes  3.90  Advanced    Advanced         1                     0             3.95               1                 yes
    3       no  3.70    Novice    Beginner         1                     1             3.70               3                  no
    5       no  3.44    Novice      Novice         0                     1             3.70               3                  no
    8       no  3.60  Beginner    Advanced         1                     1             3.70               3                  no
    9       no  3.82  Advanced    Advanced         1                     1             3.70               3                  no
    >>>
    # Example 3: Calculate the first value of all the valid columns in teradataml
    #            DataFrame, which are grouped by 'masters' and 'gpa' in a
    #            Contracting window, partitioned over 'masters'.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute first_value() on Contracting window.
    >>> window.first_value()
      masters   gpa  gpa_first_value masters_first_value
    0     yes  3.76             3.50                 yes
    1     yes  3.81             3.50                 yes
    2     yes  1.98             3.50                 yes
    3     yes  3.85             4.00                 yes
    4     yes  3.75             3.90                 yes
    5     yes  3.95             3.81                 yes
    6      no  3.87             3.87                  no
    7      no  3.60             3.87                  no
    8      no  3.13             3.87                  no
    9      no  3.52             3.87                  no
    >>>
lag
teradataml.dataframe.window.lag = lag(offset_value=1, default_expression=None)
DESCRIPTION:
    The lag function accesses data from the row preceding the current row at
    a specified offset value over the specified window in a teradataml DataFrame
    or ColumnExpression.
PARAMETERS:
    offset_value:
        Optional Argument.
        Specifies the physical row position relative to the current row in
        a given window of rows. "offset_value" must be greater than or equal
        to 0 and less than or equal to 4096. An "offset_value" of 0 specifies
        the current row.
        Default Value: 1
        types: int
    default_expression:
        Optional Argument.
        Specifies the default value to be used as return value in case function
        does not return any value. The data type of "default_expression" should match
        with the data type of ColumnExpression.
        Default Value: None
        types: ColumnExpression OR float OR int OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Execute the lag() function over the rows in a window,
    #            partitioned over 'programming' and sort by 'gpa'.
    # Create a window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      order_columns="gpa")
    # Execute lag() on the window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing lag() along
    #       with mean() window aggregate operations.
    >>> df = admissions_train.assign(previous_gpa=window.lag(), mean_gpa=window.mean())
    >>> df
       masters   gpa     stats programming  admitted  mean_gpa  previous_gpa
    id
    32     yes  3.46  Advanced    Beginner         0  3.660000          3.46
    35      no  3.68    Novice    Beginner         1  3.660000          3.50
    3       no  3.70    Novice    Beginner         1  3.660000          3.68
    39     yes  3.75  Advanced    Beginner         0  3.660000          3.70
    34     yes  3.85  Advanced    Beginner         0  3.660000          3.76
    21      no  3.87    Novice    Beginner         1  3.660000          3.85
    19     yes  1.98  Advanced    Advanced         0  3.615625           NaN
    11      no  3.13  Advanced    Advanced         1  3.615625          1.98
    14     yes  3.45  Advanced    Advanced         0  3.615625          3.13
    6      yes  3.50  Beginner    Advanced         1  3.615625          3.45
    >>>
    # Example 2: Retrieve the second preceding row from current row, for all columns
    #            in teradataml DataFrame, in a window, partitioned over
    #            'masters', and order by 'gpa' in ascending order and processing nulls first.
    # Create a window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.gpa.asc().nulls_first())
    >>>
    # Execute lag() on window.
    >>> df = window.lag(offset_value=2)
    >>> df
       masters   gpa     stats programming  admitted  admitted_lag  gpa_lag  id_lag masters_lag programming_lag stats_lag
    id
    38     yes  2.65  Advanced    Beginner         1           0.0     1.98    19.0         yes        Advanced  Advanced
    32     yes  3.46  Advanced    Beginner         0           1.0     2.65    38.0         yes        Beginner  Advanced
    22     yes  3.46    Novice    Beginner         0           0.0     3.45    14.0         yes        Advanced  Advanced
    4      yes  3.50  Beginner      Novice         1           0.0     3.46    32.0         yes        Beginner  Advanced
    6      yes  3.50  Beginner    Advanced         1           1.0     3.50     4.0         yes          Novice  Beginner
    26     yes  3.57  Advanced    Advanced         1           1.0     3.50    31.0         yes        Beginner  Advanced
    24      no  1.87  Advanced      Novice         1           NaN      NaN     NaN        None            None      None
    36      no  3.00  Advanced      Novice         0           NaN      NaN     NaN        None            None      None
    11      no  3.13  Advanced    Advanced         1           1.0     1.87    24.0          no          Novice  Advanced
    5       no  3.44    Novice      Novice         0           0.0     3.00    36.0          no          Novice  Advanced
    >>>
last_value
teradataml.dataframe.window.last_value = last_value()
DESCRIPTION:
    Function returns the last value of an ordered set of values in a teradataml
    DataFrame or ColumnExpression over the specified window.
PARAMETERS:
    None.
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the last value for the 'gpa' column
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute last_value() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing last_value() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(last_value_gpa=window.last_value(), max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  last_value_gpa  max_gpa
    id
    15     yes  4.00  Advanced    Advanced         1            4.00     4.00
    16      no  3.70  Advanced    Advanced         1            3.70     4.00
    11      no  3.13  Advanced    Advanced         1            3.13     3.96
    9       no  3.82  Advanced    Advanced         1            3.82     3.82
    19     yes  1.98  Advanced    Advanced         0            1.98     3.82
    27     yes  3.96  Advanced    Advanced         0            3.96     3.96
    1      yes  3.95  Beginner    Beginner         0            3.95     3.95
    34     yes  3.85  Advanced    Beginner         0            3.85     3.95
    32     yes  3.46  Advanced    Beginner         0            3.46     3.95
    40     yes  3.95    Novice    Beginner         0            3.95     3.95
    >>>
    # Example 2: Calculate the last value of all the valid columns in teradataml
    #            DataFrame, in an Expanding window, partitioned over 'masters',
    #            and order by 'id'.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id,
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute last_value() on Expanding window.
    >>> df = window.last_value()
    >>> df
       masters   gpa     stats programming  admitted  admitted_last_value  gpa_last_value  id_last_value masters_last_value pro
    id
    8       no  3.60  Beginner    Advanced         1                    1            3.60              8                 no    
    10      no  3.71  Advanced    Advanced         1                    1            3.71             10                 no    
    11      no  3.13  Advanced    Advanced         1                    1            3.13             11                 no    
    12      no  3.65    Novice      Novice         1                    1            3.65             12                 no    
    16      no  3.70  Advanced    Advanced         1                    1            3.70             16                 no    
    17      no  3.83  Advanced    Advanced         1                    1            3.83             17                 no    
    1      yes  3.95  Beginner    Beginner         0                    0            3.95              1                yes    
    2      yes  3.76  Beginner    Beginner         0                    0            3.76              2                yes    
    4      yes  3.50  Beginner      Novice         1                    1            3.50              4                yes    
    6      yes  3.50  Beginner    Advanced         1                    1            3.50              6                yes    
    >>>
    # Example 3: Calculate the last value of all the valid columns in teradataml
    #            DataFrame, which are grouped by 'masters' and 'gpa' in a
    #            Contracting window, partitioned over 'masters'.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute last_value() on Contracting window.
    >>> window.last_value()
      masters   gpa  gpa_last_value masters_last_value
    0      no  3.13            3.87                 no
    1      no  3.83            3.87                 no
    2      no  3.82            3.87                 no
    3      no  3.55            3.87                 no
    4      no  1.87            3.87                 no
    5      no  3.00            3.87                 no
    6     yes  3.50            3.50                yes
    7     yes  4.00            3.50                yes
    8     yes  3.76            3.50                yes
    9     yes  3.90            3.50                yes
    >>>
lead
teradataml.dataframe.window.lead = lead(offset_value=1, default_expression=None)
DESCRIPTION:
    The lead function accesses data from the row following the current row at
    a specified offset value over the specified window in a teradataml DataFrame
    or ColumnExpression.
PARAMETERS:
    offset_value:
        Optional Argument.
        Specifies the physical row position relative to the current row in
        a given window of rows. "offset_value" must be greater than or equal
        to 0 and less than or equal to 4096. An "offset_value" of 0 specifies
        the current row.
        Default Value: 1
        types: int
    default_expression:
        Optional Argument.
        Specifies the default value to be used as return value in case function
        does not return any value. The data type of "default_expression" should match
        with the data type of ColumnExpression.
        Default Value: None
        types: ColumnExpression OR float OR int OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Retrieve the following row from current row over the
    #            rows in a window, partitioned over 'programming'
    #            and sort by 'gpa'.
    # Create a window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      order_columns="gpa")
    # Execute lead() on the window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing lead() along
    #       with mean() window aggregate operations.
    >>> df = admissions_train.assign(following_gpa=window.lead(), mean_gpa=window.mean())
    >>> df
       masters   gpa     stats programming  admitted  following_gpa  mean_gpa
    id
    32     yes  3.46  Advanced    Beginner         0           3.50  3.660000
    35      no  3.68    Novice    Beginner         1           3.70  3.660000
    3       no  3.70    Novice    Beginner         1           3.75  3.660000
    39     yes  3.75  Advanced    Beginner         0           3.76  3.660000
    34     yes  3.85  Advanced    Beginner         0           3.87  3.660000
    21      no  3.87    Novice    Beginner         1           3.95  3.660000
    19     yes  1.98  Advanced    Advanced         0           3.13  3.615625
    11      no  3.13  Advanced    Advanced         1           3.45  3.615625
    14     yes  3.45  Advanced    Advanced         0           3.50  3.615625
    6      yes  3.50  Beginner    Advanced         1           3.57  3.615625
    >>>
    # Example 2: Retrieve the second following row from current row, for all columns
    #            in teradataml DataFrame, in an window, partitioned over
    #            'masters', and order by 'gpa' and process nulls first.
    # Create an window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.gpa.nulls_first())
    >>>
    # Execute lead() on window.
    >>> df = window.lead(offset_value=2)
    >>> df
       masters   gpa     stats programming  admitted  admitted_lead  gpa_lead  id_lead masters_lead programming_lead stats_lead
    id
    38     yes  2.65  Advanced    Beginner         1              0      3.45       14          yes         Advanced   Advanced
    32     yes  3.46  Advanced    Beginner         0              0      3.46       22          yes         Beginner     Novice
    22     yes  3.46    Novice    Beginner         0              1      3.50        4          yes           Novice   Beginner
    4      yes  3.50  Beginner      Novice         1              1      3.50       31          yes         Beginner   Advanced
    6      yes  3.50  Beginner    Advanced         1              1      3.57       26          yes         Advanced   Advanced
    26     yes  3.57  Advanced    Advanced         1              1      3.59       23          yes           Novice   Advanced
    24      no  1.87  Advanced      Novice         1              0      3.00       36           no           Novice   Advanced
    36      no  3.00  Advanced      Novice         0              1      3.13       11           no         Advanced   Advanced
    11      no  3.13  Advanced    Advanced         1              0      3.44        5           no           Novice     Novice
    5       no  3.44    Novice      Novice         0              1      3.52       37           no           Novice     Novice
    >>>
max
teradataml.dataframe.window.max = max(distinct=False)
DESCRIPTION:
    Function returns the maximum of values in teradataml DataFrame or
    ColumnExpression over the specified window.
PARAMETERS:
    distinct:
        Optional Argument.
        Specifies a flag that decides whether to consider duplicate values in
        a column or not.
        Default Values: False
        Types: bool
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the maximum of values for the 'gpa' column
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute max() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing count() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(max_gpa=window.max(), count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  max_gpa
    id
    11      no  3.13  Advanced    Advanced         1          3     3.83
    27     yes  3.96  Advanced    Advanced         0          3     3.96
    26     yes  3.57  Advanced    Advanced         1          3     3.96
    6      yes  3.50  Beginner    Advanced         1          3     3.96
    9       no  3.82  Advanced    Advanced         1          3     3.82
    25      no  3.96  Advanced    Advanced         1          3     3.96
    39     yes  3.75  Advanced    Beginner         0          1     3.75
    31     yes  3.50  Advanced    Beginner         1          2     3.75
    29     yes  4.00    Novice    Beginner         0          3     4.00
    21      no  3.87    Novice    Beginner         1          3     4.00
    >>>
    # Example 2: Calculate the maximum value of all the valid columns in teradataml
    #            DataFrame, in an Expanding window, partitioned over 'programming',
    #            and order by 'id' in descending order.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns="masters",
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute max() on the Expanding window.
    >>> df = window.max()
    >>> df
       masters   gpa     stats programming  admitted  admitted_max  gpa_max  id_max masters_max programming_max stats_max
    id
    38     yes  2.65  Advanced    Beginner         1             1     3.95      40         yes        Beginner    Novice
    32     yes  3.46  Advanced    Beginner         0             1     3.95      40         yes        Beginner    Novice
    31     yes  3.50  Advanced    Beginner         1             1     3.95      40         yes        Beginner    Novice
    30     yes  3.79  Advanced      Novice         0             1     3.95      40         yes          Novice    Novice
    27     yes  3.96  Advanced    Advanced         0             1     4.00      40         yes          Novice    Novice
    26     yes  3.57  Advanced    Advanced         1             1     4.00      40         yes          Novice    Novice
    37      no  3.52    Novice      Novice         1             1     3.52      37          no          Novice    Novice
    36      no  3.00  Advanced      Novice         0             1     3.52      37          no          Novice    Novice
    35      no  3.68    Novice    Beginner         1             1     3.68      37          no          Novice    Novice
    33      no  3.55    Novice      Novice         1             1     3.68      37          no          Novice    Novice
    >>>
    # Example 3: Calculate the maximum value of all the valid columns in
    #            teradataml DataFrame, which are grouped by 'masters' and 'gpa'
    #            in a Contracting window, partitioned over 'masters' and order
    #            by 'masters' with nulls in masters listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             order_columns=group_by_df.masters.nulls_last(),
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute max() on the Contracting window.
    >>> window.max()
      masters   gpa  gpa_max masters_max
    0      no  3.71     4.00          no
    1      no  3.52     4.00          no
    2      no  3.68     4.00          no
    3      no  3.83     4.00          no
    4      no  3.55     4.00          no
    5      no  3.96     4.00          no
    6     yes  3.59     3.95         yes
    7     yes  3.95     3.95         yes
    8     yes  3.46     3.95         yes
    9     yes  3.76     3.95         yes
    >>>
mean
teradataml.dataframe.window.mean = mean(distinct=False)
DESCRIPTION:
    Function returns the arithmetic average of all values in teradataml
    DataFrame or ColumnExpression over the specified window.
PARAMETERS:
    distinct:
        Optional Argument.
        Specifies a flag that decides whether to consider duplicate values
        in a column or not.
        Default Values: False
        Types: bool
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
ALTERNATE NAMES:
    1. avg
    2. ave
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the average value for the 'gpa' column
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute mean() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing count() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(mean_gpa=window.mean(), max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  max_gpa  mean_gpa
    id
    2      yes  3.76  Beginner    Beginner         0     3.95  3.453333
    21      no  3.87    Novice    Beginner         1     3.87  3.710000
    40     yes  3.95    Novice    Beginner         0     3.95  3.773333
    22     yes  3.46    Novice    Beginner         0     3.95  3.760000
    35      no  3.68    Novice    Beginner         1     3.75  3.630000
    29     yes  4.00    Novice    Beginner         0     4.00  3.810000
    11      no  3.13  Advanced    Advanced         1     3.13  3.130000
    28      no  3.93  Advanced    Advanced         1     3.93  3.530000
    16      no  3.70  Advanced    Advanced         1     3.93  3.586667
    8       no  3.60  Beginner    Advanced         1     3.93  3.743333
    >>>
    # Example 2: Calculate the average of all the valid columns in teradataml
                 DataFrame, in an Expanding window, partitioned over 'programming'.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.programming,
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute mean() on the Expanding window.
    >>> df = window.mean()
    >>> df
       masters   gpa     stats programming  admitted  admitted_mean  gpa_mean    id_mean
    id
    2      yes  3.76  Beginner    Beginner         0       0.333333  3.840000   7.000000
    40     yes  3.95    Novice    Beginner         0       0.200000  3.864000  19.000000
    7      yes  2.33    Novice      Novice         1       0.333333  3.608333  17.000000
    22     yes  3.46    Novice    Beginner         0       0.285714  3.587143  17.714286
    27     yes  3.96  Advanced    Advanced         0       0.222222  3.646667  21.111111
    4      yes  3.50  Beginner      Novice         1       0.300000  3.632000  19.400000
    3       no  3.70    Novice    Beginner         1       1.000000  3.700000   3.000000
    25      no  3.96  Advanced    Advanced         1       1.000000  3.830000  14.000000
    17      no  3.83  Advanced    Advanced         1       1.000000  3.830000  15.000000
    13      no  4.00  Advanced      Novice         1       1.000000  3.872500  14.500000
    >>>
    # Example 3: Calculate the average of all the valid columns in teradataml DataFrame,
    #            which are grouped by 'masters' and 'gpa' in a Contracting window,
    #            partitioned over 'masters'.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute mean() on the Contracting window.
    >>> window.mean()
      masters   gpa  gpa_mean
    0     yes  3.79  3.632500
    1     yes  3.50  3.529000
    2     yes  3.96  3.554545
    3     yes  4.00  3.579167
    4     yes  3.90  3.464286
    5     yes  2.33  3.464000
    6      no  3.52  3.265000
    7      no  3.83  3.320000
    8      no  3.82  3.405000
    9      no  3.55  3.438889
    >>>
min
teradataml.dataframe.window.min = min(distinct=False)
DESCRIPTION:
    Function returns the minimum of values in teradataml DataFrame or
    ColumnExpression over the specified window.
PARAMETERS:
    distinct:
        Optional Argument.
        Specifies a flag that decides whether to consider duplicate values in
        a column or not.
        Default Values: False
        Types: bool
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the minimum of values for the 'gpa' column
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute min() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing count() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(min_gpa=window.min(), count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  min_gpa
    id
    11      no  3.13  Advanced    Advanced         1          3     1.98
    27     yes  3.96  Advanced    Advanced         0          3     3.13
    26     yes  3.57  Advanced    Advanced         1          3     3.45
    6      yes  3.50  Beginner    Advanced         1          3     3.50
    9       no  3.82  Advanced    Advanced         1          3     3.50
    25      no  3.96  Advanced    Advanced         1          3     3.60
    39     yes  3.75  Advanced    Beginner         0          1     3.75
    31     yes  3.50  Advanced    Beginner         1          2     3.50
    29     yes  4.00    Novice    Beginner         0          3     3.50
    21      no  3.87    Novice    Beginner         1          3     3.50
    >>>
    # Example 2: Calculate the minimum value of all the valid columns in
    #            teradataml DataFrame, in an Expanding window, partitioned
    #            over 'programming', and order by 'id' in descending order.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute min() on the Expanding window.
    >>> df = window.min()
    >>> df
       masters   gpa     stats programming  admitted  admitted_min  gpa_min  id_min masters_min programming_min stats_min
    id
    38     yes  2.65  Advanced    Beginner         1             0     2.65      38         yes        Beginner  Advanced
    32     yes  3.46  Advanced    Beginner         0             0     2.65      32         yes        Beginner  Advanced
    31     yes  3.50  Advanced    Beginner         1             0     2.65      31         yes        Beginner  Advanced
    30     yes  3.79  Advanced      Novice         0             0     2.65      30         yes        Beginner  Advanced
    27     yes  3.96  Advanced    Advanced         0             0     2.65      27         yes        Advanced  Advanced
    26     yes  3.57  Advanced    Advanced         1             0     2.65      26         yes        Advanced  Advanced
    37      no  3.52    Novice      Novice         1             1     3.52      37          no          Novice    Novice
    36      no  3.00  Advanced      Novice         0             0     3.00      36          no          Novice  Advanced
    35      no  3.68    Novice    Beginner         1             0     3.00      35          no        Beginner  Advanced
    33      no  3.55    Novice      Novice         1             0     3.00      33          no        Beginner  Advanced
    >>>
    # Example 3: Calculate the minimum value of all the valid columns in
    #            teradataml DataFrame, which are grouped by 'masters' and 'gpa'
    #            in a Contracting window, partitioned over 'masters' and order
    #            by 'masters' with nulls in 'masters' listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             order_columns=group_by_df.masters.nulls_last(),
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute min() on the Contracting window.
    >>> window.min()
      masters   gpa  gpa_min masters_min
    0     yes  3.46     1.98         yes
    1     yes  2.33     1.98         yes
    2     yes  3.81     1.98         yes
    3     yes  1.98     1.98         yes
    4     yes  3.45     1.98         yes
    5     yes  3.57     1.98         yes
    6      no  3.00     3.00          no
    7      no  4.00     3.00          no
    8      no  3.71     3.00          no
    9      no  3.60     3.00          no
    >>>
percent_rank
teradataml.dataframe.window.percent_rank = percent_rank()
DESCRIPTION:
    Function returns the relative rank of all the rows in a teradataml
    DataFrame or ColumnExpression, according to "order_columns", over the
    specified window.
    Notes:
         1. Window parameter "order_columns" should not be None.
         2. Unlike other window aggregate functions, executing percent_rank()
            on a window created on teradataml DataFrame does not create
            multiple columns, that is, it results in one new column in addition
            to the original teradataml DataFrame columns. For calculating
            percent_rank, value passed to "order_columns" parameter of window
            is considered and not the ColumnExpression.
PARAMETERS:
    None.
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Get the relative ranking of rows over the values in a window,
    #            partitioned over 'programming' and sort by 'gpa'.
    # Create a window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns=admissions_train.programming,
    ...                                      order_columns=admissions_train.gpa)
    # Execute percent_rank() on a window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing percent_rank() along
    #       with mean() window aggregate operations.
    >>> df = admissions_train.assign(percent_rank=window.percent_rank(), mean_gpa=window.mean())
    >>> df
       masters   gpa     stats programming  admitted  mean_gpa  percent_rank
    id
    32     yes  3.46  Advanced    Beginner         0  3.660000      0.083333
    35      no  3.68    Novice    Beginner         1  3.660000      0.333333
    3       no  3.70    Novice    Beginner         1  3.660000      0.416667
    39     yes  3.75  Advanced    Beginner         0  3.660000      0.500000
    34     yes  3.85  Advanced    Beginner         0  3.660000      0.666667
    21      no  3.87    Novice    Beginner         1  3.660000      0.750000
    19     yes  1.98  Advanced    Advanced         0  3.615625      0.000000
    11      no  3.13  Advanced    Advanced         1  3.615625      0.066667
    14     yes  3.45  Advanced    Advanced         0  3.615625      0.133333
    6      yes  3.50  Beginner    Advanced         1  3.615625      0.200000
    >>>
    # Example 2: Get the relative ranking of rows on a window created on
    #            teradataml DataFrame, partitioned over 'masters',
    #            and order by 'gpa'.
    # Create a window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns="masters",
    ...                                  order_columns="gpa")
    >>>
    # Execute percent_rank() on a window.
    >>> df = window.percent_rank()
    >>> df
       masters   gpa     stats programming  admitted  col_percent_rank
    id
    38     yes  2.65  Advanced    Beginner         1          0.095238
    32     yes  3.46  Advanced    Beginner         0          0.190476
    22     yes  3.46    Novice    Beginner         0          0.190476
    4      yes  3.50  Beginner      Novice         1          0.285714
    6      yes  3.50  Beginner    Advanced         1          0.285714
    26     yes  3.57  Advanced    Advanced         1          0.428571
    24      no  1.87  Advanced      Novice         1          0.000000
    36      no  3.00  Advanced      Novice         0          0.058824
    11      no  3.13  Advanced    Advanced         1          0.117647
    5       no  3.44    Novice      Novice         0          0.176471
    >>>
rank
teradataml.dataframe.window.rank = rank()
DESCRIPTION:
    Function returns the rank (1 … n) of all the rows in a teradataml
    DataFrame or ColumnExpression, according to "order_columns", over the
    specified window.
    Notes:
         1. Window parameter "order_columns" should not be None.
         2. Unlike other window aggregate functions, executing rank()
            on a window created on teradataml DataFrame does not create
            multiple columns, that is, it results in one new column in addition
            to the original teradataml DataFrame columns. For calculating
            rank, value passed to "order_columns" parameter of window
            is considered and not the ColumnExpression.
PARAMETERS:
    None.
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Get the ordered ranking of rows over the values in a window,
    #            partitioned over 'programming' and sort by 'gpa'.
    # Create a window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns=admissions_train.programming,
    ...                                      order_columns=admissions_train.gpa)
    # Execute rank() on the window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing rank() along with
    #       min() window aggregate operations.
    >>> df = admissions_train.assign(rank=window.rank(), min_gpa=window.min())
    >>> df
       masters   gpa     stats programming  admitted  min_gpa  rank
    id
    32     yes  3.46  Advanced    Beginner         0     2.65     2
    35      no  3.68    Novice    Beginner         1     2.65     5
    3       no  3.70    Novice    Beginner         1     2.65     6
    39     yes  3.75  Advanced    Beginner         0     2.65     7
    34     yes  3.85  Advanced    Beginner         0     2.65     9
    21      no  3.87    Novice    Beginner         1     2.65    10
    19     yes  1.98  Advanced    Advanced         0     1.98     1
    11      no  3.13  Advanced    Advanced         1     1.98     2
    14     yes  3.45  Advanced    Advanced         0     1.98     3
    6      yes  3.50  Beginner    Advanced         1     1.98     4
    >>>
    # Example 2: Get the ordered ranking of rows on a window created on
    #            teradataml DataFrame, partitioned over 'masters',
    #            and order by 'gpa'.
    # Create a window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns="masters",
    ...                                  order_columns="gpa")
    >>>
    # Execute rank() on a window.
    >>> df = window.rank()
    >>> df
       masters   gpa     stats programming  admitted  col_rank
    id
    38     yes  2.65  Advanced    Beginner         1         3
    32     yes  3.46  Advanced    Beginner         0         5
    22     yes  3.46    Novice    Beginner         0         5
    4      yes  3.50  Beginner      Novice         1         7
    6      yes  3.50  Beginner    Advanced         1         7
    26     yes  3.57  Advanced    Advanced         1        10
    24      no  1.87  Advanced      Novice         1         1
    36      no  3.00  Advanced      Novice         0         2
    11      no  3.13  Advanced    Advanced         1         3
    5       no  3.44    Novice      Novice         0         4
    >>>
regr_avgx
teradataml.dataframe.window.regr_avgx = regr_avgx(expression)
DESCRIPTION:
    Function returns the mean of the independent variable for all
    non-null data pairs of the dependent and an independent variable arguments
    over the specified window. The function considers ColumnExpression as a
    dependent variable and "expression" as an independent variable.
    Note:
        When there are fewer than two non-null data point pairs in the
        data used for the computation, the function returns None.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a column or name of the column or a
        literal representing an independent variable for the regression.
        An independent variable is a treatment: something that is varied under
        your control to test the behavior of another variable.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the mean of the column 'admitted' for all
    #            non-null data pairs with dependent variable as 'gpa',
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute regr_avgx() on the Rolling window and attach it to the DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate
    #       operations in one single call. In this example, we are executing
    #       regr_avgx() along with count() window aggregate operations.
    >>> df = admissions_train.assign(regr_avgx_gpa=window.regr_avgx(admissions_train.admitted),
    ...                              count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  regr_avgx_gpa
    id
    15     yes  4.00  Advanced    Advanced         1          3       1.000000
    16      no  3.70  Advanced    Advanced         1          3       1.000000
    11      no  3.13  Advanced    Advanced         1          3       1.000000
    9       no  3.82  Advanced    Advanced         1          3       1.000000
    19     yes  1.98  Advanced    Advanced         0          3       0.666667
    27     yes  3.96  Advanced    Advanced         0          3       0.333333
    1      yes  3.95  Beginner    Beginner         0          1       0.000000
    34     yes  3.85  Advanced    Beginner         0          2       0.000000
    32     yes  3.46  Advanced    Beginner         0          3       0.000000
    40     yes  3.95    Novice    Beginner         0          3       0.000000
    >>>
    # Example 2: Calculate the mean of the column 'admitted' for all non-null
    #            data pairs with dependent variable as all other columns,
    #            in an Expanding window, partitioned over 'programming',
    #            and order by 'id' in descending order.
    # Create an Expanding window on DataFrame.
    >>> window = admissions_train.window(partition_columns="masters",
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute regr_avgx() on the Expanding window.
    >>> df = window.regr_avgx(admissions_train.admitted)
    >>> df
       masters   gpa     stats programming  admitted  admitted_regr_avgx  gpa_regr_avgx  id_regr_avgx
    id
    38     yes  2.65  Advanced    Beginner         1            0.333333       0.333333      0.333333
    32     yes  3.46  Advanced    Beginner         0            0.200000       0.200000      0.200000
    31     yes  3.50  Advanced    Beginner         1            0.333333       0.333333      0.333333
    30     yes  3.79  Advanced      Novice         0            0.285714       0.285714      0.285714
    27     yes  3.96  Advanced    Advanced         0            0.222222       0.222222      0.222222
    26     yes  3.57  Advanced    Advanced         1            0.300000       0.300000      0.300000
    37      no  3.52    Novice      Novice         1            1.000000       1.000000      1.000000
    36      no  3.00  Advanced      Novice         0            0.500000       0.500000      0.500000
    35      no  3.68    Novice    Beginner         1            0.666667       0.666667      0.666667
    33      no  3.55    Novice      Novice         1            0.750000       0.750000      0.750000
    >>>
    # Example 3: Calculate the mean of the column 'gpa' for all non-null
    #            data pairs with dependent variable as all other columns,
    #            which are grouped by 'masters' and 'gpa' in a Contracting
    #            window, partitioned over 'masters' and order by 'masters'
    #            with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns="masters",
    ...                             order_columns="masters",
    ...                             nulls_first=False,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute regr_avgx() on the Contracting window.
    >>> window.regr_avgx(admissions_train.gpa)
    masters   gpa  gpa_regr_avgx
    0     yes  3.79       3.632500
    1     yes  3.50       3.529000
    2     yes  3.96       3.554545
    3     yes  4.00       3.579167
    4     yes  3.90       3.464286
    5     yes  2.33       3.464000
    6      no  3.52       3.265000
    7      no  3.83       3.320000
    8      no  3.82       3.405000
    9      no  3.55       3.438889
    >>>
regr_avgy
teradataml.dataframe.window.regr_avgy = regr_avgy(expression)
DESCRIPTION:
    Function returns the mean of the dependent variable for all non-null data
    pairs of the dependent and independent variable arguments over the specified
    window. The function considers ColumnExpression as an independent variable
    and "expression" as a dependent variable.
    Note:
        When there are fewer than two non-null data point pairs in the data used
        for the computation, the function returns None.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a column or name of the column or a literal
        representing an independent variable for the regression.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the mean of the column 'admitted' for all
    #            non-null data pairs with independent variable as 'gpa',
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute regr_avgy() on the Rolling window and attach it to the DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate
    #       operations in one single call. In this example, we are executing
    #       regr_avgy() along with count() window aggregate operations.
    >>> df = admissions_train.assign(regr_avgy_gpa=window.regr_avgy(admissions_train.admitted),
    ...                              count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  regr_avgy_gpa
    id
    9       no  3.82  Advanced    Advanced         1          3       3.750000
    6      yes  3.50  Beginner    Advanced         1          3       3.760000
    28      no  3.93  Advanced    Advanced         1          3       3.796667
    27     yes  3.96  Advanced    Advanced         0          3       3.796667
    18     yes  3.81  Advanced    Advanced         1          3       3.250000
    10      no  3.71  Advanced    Advanced         1          3       3.166667
    1      yes  3.95  Beginner    Beginner         0          1       3.950000
    40     yes  3.95    Novice    Beginner         0          2       3.950000
    22     yes  3.46    Novice    Beginner         0          3       3.786667
    39     yes  3.75  Advanced    Beginner         0          3       3.720000
    >>>
    # Example 2: Calculate the mean of the column 'admitted' for all non-null
    #            data pairs with independent variable as all valid columns,
    #            in an Expanding window, partitioned over 'programming',
    #            and order by 'id' in descending order.
    # Create an Expanding window on DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute regr_avgy() on the Expanding window.
    >>> df = window.regr_avgy(admissions_train.admitted)
    >>> df
       masters   gpa     stats programming  admitted  admitted_regr_avgy  gpa_regr_avgy  id_regr_avgy
    id
    38     yes  2.65  Advanced    Beginner         1            0.333333       3.450000     39.000000
    32     yes  3.46  Advanced    Beginner         0            0.200000       3.532000     36.600000
    31     yes  3.50  Advanced    Beginner         1            0.333333       3.526667     35.666667
    30     yes  3.79  Advanced      Novice         0            0.285714       3.564286     34.857143
    27     yes  3.96  Advanced    Advanced         0            0.222222       3.656667     33.333333
    26     yes  3.57  Advanced    Advanced         1            0.300000       3.648000     32.600000
    37      no  3.52    Novice      Novice         1            1.000000       3.520000     37.000000
    36      no  3.00  Advanced      Novice         0            0.500000       3.260000     36.500000
    35      no  3.68    Novice    Beginner         1            0.666667       3.400000     36.000000
    33      no  3.55    Novice      Novice         1            0.750000       3.437500     35.250000
    >>>
    # Example 3: Calculate the mean of the column 'gpa' for all non-null
    #            data pairs with independent variable as all other columns,
    #            which are grouped by 'masters' and 'gpa' in a Contracting
    #            window, partitioned over 'masters' and order by 'masters'
    #            with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns="masters",
    ...                             order_columns=group_by_df.masters.nulls_first(),
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute regr_avgy() on the Contracting window.
    >>> window.regr_avgy(admissions_train.gpa)
      masters   gpa  gpa_regr_avgy
    0     yes  3.79       3.632500
    1     yes  3.50       3.529000
    2     yes  3.96       3.554545
    3     yes  4.00       3.579167
    4     yes  3.90       3.464286
    5     yes  2.33       3.464000
    6      no  3.52       3.265000
    7      no  3.83       3.320000
    8      no  3.82       3.405000
    9      no  3.55       3.438889
    >>>
regr_count
teradataml.dataframe.window.regr_count = regr_count(expression)
DESCRIPTION:
    Function returns the count of all non-null data pairs of the dependent and
    independent variable arguments over the specified window. The function
    considers ColumnExpression as a dependent variable and "expression" as
    an independent variable.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a column or name of the column or a
        literal representing an independent variable for the regression.
        An independent variable is a treatment: something that is varied under
        your control to test the behavior of another variable.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the total number of non-null data pairs with dependent
    #            variable as column 'admitted' and independent variable as 'gpa',
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute regr_count() on the Rolling window and attach it to the DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate
    #       operations in one single call. In this example, we are executing
    #       regr_count() along with max() window aggregate operations.
    >>> df = admissions_train.assign(regr_count_gpa=window.regr_count(admissions_train.admitted),
    ...                              max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  max_gpa  regr_count_gpa
    id
    2      yes  3.76  Beginner    Beginner         0     3.95               3
    21      no  3.87    Novice    Beginner         1     3.87               3
    40     yes  3.95    Novice    Beginner         0     3.95               3
    22     yes  3.46    Novice    Beginner         0     3.95               3
    35      no  3.68    Novice    Beginner         1     3.75               3
    29     yes  4.00    Novice    Beginner         0     4.00               3
    11      no  3.13  Advanced    Advanced         1     3.13               1
    28      no  3.93  Advanced    Advanced         1     3.93               2
    16      no  3.70  Advanced    Advanced         1     3.93               3
    8       no  3.60  Beginner    Advanced         1     3.93               3
    >>>
    # Example 2: Calculate the count of the column 'admitted' for all non-null
    #            data pairs with dependent variable as all other columns,
    #            in an Expanding window, partitioned over 'programming',
    #            and order by 'id' in descending order.
    # Create an Expanding window on DataFrame.
    >>> window = admissions_train.window(partition_columns="programming",
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute regr_count() on the Expanding window.
    >>> df = window.regr_count(admissions_train.admitted)
    >>> df
       masters   gpa     stats programming  admitted  admitted_regr_count  gpa_regr_count  id_regr_count
    id
    38     yes  2.65  Advanced    Beginner         1                    3               3              3
    34     yes  3.85  Advanced    Beginner         0                    5               5              5
    32     yes  3.46  Advanced    Beginner         0                    6               6              6
    31     yes  3.50  Advanced    Beginner         1                    7               7              7
    22     yes  3.46    Novice    Beginner         0                    9               9              9
    21      no  3.87    Novice    Beginner         1                   10              10             10
    28      no  3.93  Advanced    Advanced         1                    1               1              1
    27     yes  3.96  Advanced    Advanced         0                    2               2              2
    26     yes  3.57  Advanced    Advanced         1                    3               3              3
    25      no  3.96  Advanced    Advanced         1                    4               4              4
    >>>
    # Example 3: Calculate the regression count of the column 'gpa' for
    #            all non-null data pairs with dependent variable as all
    #            other columns, which are grouped by 'masters' and 'gpa'
    #            in a Contracting window, partitioned over 'masters' and
    #            order by 'masters' with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns="masters",
    ...                             order_columns="masters",
    ...                             nulls_first=False,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute regr_count() on the Contracting window.
    >>> window.regr_avgx(admissions_train.gpa)
      masters   gpa  gpa_regr_count
    0      no  3.13               8
    1      no  3.83              10
    2      no  3.82              11
    3      no  3.55              12
    4      no  1.87              14
    5      no  3.00              15
    6     yes  3.50               6
    7     yes  4.00               7
    8     yes  3.76               8
    9     yes  3.90               9
    >>>
regr_intercept
teradataml.dataframe.window.regr_intercept = regr_intercept(expression)
DESCRIPTION:
    Function returns the intercept of the univariate linear regression line
    through all non-null data pairs of the dependent and independent variable
    arguments over the specified window. The intercept is the point at which
    the regression line through the non-null data pairs in the sample intersects
    the ordinate, or y-axis, of the graph. The plot of the linear regression
    on the variables is used to predict the behavior of the dependent variable
    from the change in the independent variable. There can be a strong nonlinear
    relationship between independent and dependent variables, and the computation
    of the simple linear regression between such variable pairs does not reflect
    such a relationship. The function considers ColumnExpression as an dependent
    variable and "expression" as an independent variable.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a column or name of the column or a
        literal representing an independent variable for the regression.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the intercept of the univariate linear regression line
    #            through all non-null data pairs of 'gpa' and 'admitted',
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute regr_intercept() on the Rolling window and attach it to the DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate
    #       operations in one single call. In this example, we are executing
    #       regr_intercept() along with count() window aggregate operations.
    >>> df = admissions_train.assign(regr_intercept_gpa=window.regr_intercept(admissions_train.admitted),
    ...                              count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  regr_intercept_gpa
    id
    15     yes  4.00  Advanced    Advanced         1          3                 NaN
    16      no  3.70  Advanced    Advanced         1          3                 NaN
    11      no  3.13  Advanced    Advanced         1          3                 NaN
    9       no  3.82  Advanced    Advanced         1          3                 NaN
    19     yes  1.98  Advanced    Advanced         0          3                1.98
    27     yes  3.96  Advanced    Advanced         0          3                2.97
    1      yes  3.95  Beginner    Beginner         0          1                 NaN
    34     yes  3.85  Advanced    Beginner         0          2                 NaN
    32     yes  3.46  Advanced    Beginner         0          3                 NaN
    40     yes  3.95    Novice    Beginner         0          3                 NaN
    >>>
    # Example 2: Calculate the intercept of the univariate linear regression
    #            line through all non-null data pairs for all the applicable
    #            columns and independent variable as 'admitted',
    #            in an Expanding window, partitioned over 'programming',
    #            and order by 'id' in descending order.
    # Create an Expanding window on DataFrame.
    >>> window = admissions_train.window(partition_columns="masters",
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute regr_intercept() on the Expanding window.
    >>> df = window.regr_intercept(admissions_train.admitted)
    >>> df
       masters   gpa     stats programming  admitted  admitted_regr_intercept  gpa_regr_intercept  id_regr_intercept
    id
    38     yes  2.65  Advanced    Beginner         1                      0.0            3.850000              39.50
    32     yes  3.46  Advanced    Beginner         0                      0.0            3.752500              36.25
    31     yes  3.50  Advanced    Beginner         1                      0.0            3.752500              36.25
    30     yes  3.79  Advanced      Novice         0                      0.0            3.760000              35.00
    27     yes  3.96  Advanced    Advanced         0                      0.0            3.822857              33.00
    26     yes  3.57  Advanced    Advanced         1                      0.0            3.822857              33.00
    37      no  3.52    Novice      Novice         1                      NaN                 NaN                NaN
    36      no  3.00  Advanced      Novice         0                      0.0            3.000000              36.00
    35      no  3.68    Novice    Beginner         1                      0.0            3.000000              36.00
    33      no  3.55    Novice      Novice         1                      0.0            3.000000              36.00
    >>>
    # Example 3: Calculate the the intercept of the univariate linear regression
    #            line independent variable as 'admitted' and through all non-null
    #            data pairs as all the applicable columns, which are grouped by
    #            'masters', 'gpa' and 'id' in a Contracting window, partitioned over
    #            'masters' and order by 'masters' with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa", "id"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             order_columns=group_by_df.masters.nulls_last(),
    ...                             nulls_first=False,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute regr_intercept() on the Contracting window.
    >>> window.regr_intercept(admissions_train.gpa)
      masters   gpa  id  gpa_regr_intercept  id_regr_intercept
    0     yes  3.50   4                 0.0           6.109916
    1     yes  3.79  30                 0.0           5.004477
    2     yes  3.45  14                 0.0          22.411673
    3     yes  3.50  31                 0.0          30.417318
    4     yes  3.50   6                 0.0          30.390058
    5     yes  3.59  23                 0.0          32.804411
    6      no  3.65  12                 0.0          24.834395
    7      no  3.87  21                 0.0          24.222350
    8      no  3.44   5                 0.0          24.850737
    9      no  1.87  24                 0.0          22.248982
    >>>
regr_r2
teradataml.dataframe.window.regr_r2 = regr_r2(expression)
DESCRIPTION:
    Function returns the coefficient of determination for all non-null data
    pairs of the dependent and independent variable arguments over the
    specified window. The function considers ColumnExpression as a dependent
    variable and "expression" as an independent variable.
    Note:
        When there are fewer than two non-null data point pairs in the data
        used for the computation, the function returns NULL.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a column or name of the column or a
        literal representing an independent variable for the regression.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the coefficient of determination for the values
    #            forming a pair between 'gpa' and 'admitted', in a Rolling
    #            window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute regr_r2() on the Rolling window and attach it to the DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate
    #       operations in one single call. In this example, we are executing
    #       regr_r2() along with count() window aggregate operations.
    >>> df = admissions_train.assign(regr_r2_admitted=window.regr_r2(admissions_train.admitted),
    ...                              count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  regr_r2_admitted
    id
    11      no  3.13  Advanced    Advanced         1          3             1.980
    27     yes  3.96  Advanced    Advanced         0          3             3.705
    26     yes  3.57  Advanced    Advanced         1          3             3.705
    6      yes  3.50  Beginner    Advanced         1          3             3.960
    9       no  3.82  Advanced    Advanced         1          3               NaN
    25      no  3.96  Advanced    Advanced         1          3               NaN
    39     yes  3.75  Advanced    Beginner         0          1               NaN
    31     yes  3.50  Advanced    Beginner         1          2             3.750
    29     yes  4.00    Novice    Beginner         0          3             3.875
    21      no  3.87    Novice    Beginner         1          3             4.000
    >>>
    # Example 2: Calculate the coefficient of determination between
    #            'admitted' and all valid columns, in an Expanding window,
    #            partitioned over 'programming', and order by 'id' in descending
    #            order.
    # Create an Expanding window on DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute regr_r2() on the Expanding window.
    >>> df = window.regr_r2(admissions_train.admitted)
    >>> df
       masters   gpa     stats programming  admitted  admitted_regr_r2  gpa_regr_r2  id_regr_r2
    id
    38     yes  2.65  Advanced    Beginner         1               1.0     0.979592    0.750000
    32     yes  3.46  Advanced    Beginner         0               1.0     0.878827    0.051907
    31     yes  3.50  Advanced    Beginner         1               1.0     0.552687    0.055682
    30     yes  3.79  Advanced      Novice         0               1.0     0.574510    0.003541
    27     yes  3.96  Advanced    Advanced         0               1.0     0.605686    0.019886
    26     yes  3.57  Advanced    Advanced         1               1.0     0.494344    0.016637
    37      no  3.52    Novice      Novice         1               NaN          NaN         NaN
    36      no  3.00  Advanced      Novice         0               1.0     1.000000    1.000000
    35      no  3.68    Novice    Beginner         1               1.0     0.949367    0.000000
    33      no  3.55    Novice      Novice         1               1.0     0.946355    0.085714
    >>>
    # Example 3: Calculate the coefficient of determination between
    #            'gpa' and all valid columns, which are grouped by
    #            'masters' and 'gpa' in a Contracting window, partitioned
    #            over 'masters' and order by 'masters' with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             order_columns=group_by_df.masters.nulls_last(),
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute regr_r2() on the Contracting window.
    >>> window.regr_r2(admissions_train.gpa)
      masters   gpa  gpa_regr_r2
    0     yes  3.76          1.0
    1     yes  3.81          1.0
    2     yes  1.98          1.0
    3     yes  3.85          1.0
    4     yes  3.75          1.0
    5     yes  3.95          1.0
    6      no  3.87          1.0
    7      no  3.60          1.0
    8      no  3.13          1.0
    9      no  3.52          1.0
    >>>
regr_slope
teradataml.dataframe.window.regr_slope = regr_slope(expression)
DESCRIPTION:
    Function returns the slope of the univariate linear regression line through
    all non-null data pairs of the dependent and an independent variable arguments
    over the specified window. When function is executed, "expression" is treated as
    an independent variable and dependent variable is:
        * a ColumnExpression when invoked using a window created on ColumnExpression.
        * all columns of the teradataml DataFrame which are valid for this function,
          when executed on a window created on teradataml DataFrame.
    Note:
        When there are fewer than two non-null data point pairs in the
        data used for the computation, the function returns None.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a column or name of the column or a
        literal representing an independent variable for the regression.
        An independent variable is a treatment: something that is varied under
        your control to test the behavior of another variable.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the slope of the univariate linear regression
    #            line  of the column 'gpa' for all
    #            non-null data pairs with dependent variable as 'admitted',
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.admitted.window(partition_columns="programming",
    ...                                           window_start_point=-2,
    ...                                           window_end_point=0)
    >>>
    # Execute regr_slope() on the Rolling window and attach it to the DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate
    #       operations in one single call. In this example, we are executing
    #       regr_slope() along with count() window aggregate operations.
    >>> df = admissions_train.assign(regr_avgx_admitted=window.regr_slope(admissions_train.gpa),
    ...                              count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  regr_avgx_admitted
    id
    11      no  3.13  Advanced    Advanced         1          3        5.730659e-01
    27     yes  3.96  Advanced    Advanced         0          3       -1.093780e+00
    26     yes  3.57  Advanced    Advanced         1          3       -6.329114e-01
    6      yes  3.50  Beginner    Advanced         1          3       -2.306023e+00
    9       no  3.82  Advanced    Advanced         1          3       -3.314099e-14
    25      no  3.96  Advanced    Advanced         1          3       -5.393796e-14
    39     yes  3.75  Advanced    Beginner         0          1                 NaN
    31     yes  3.50  Advanced    Beginner         1          2       -4.000000e+00
    29     yes  4.00    Novice    Beginner         0          3       -2.000000e+00
    21      no  3.87    Novice    Beginner         1          3       -1.560178e+00
    >>>
    # Example 2: Calculate the slope of the univariate linear regression line
    #            between all valid columns as dependent variable and 'gpa' as
    #            independent variable, in an Expanding window, partitioned over
    #            'programming', and order by 'id' in descending order.
    # Create an Expanding window on DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute regr_slope() on the Expanding window.
    >>> df = window.regr_slope(admissions_train.gpa)
    >>> df
       masters   gpa     stats programming  admitted  admitted_regr_slope  gpa_regr_slope  id_regr_slope
    id
    38     yes  2.65  Advanced    Beginner         1            -0.816327             1.0       1.326531
    32     yes  3.46  Advanced    Beginner         0            -0.797122             1.0       0.193406
    31     yes  3.50  Advanced    Beginner         1            -0.815774             1.0       0.328116
    30     yes  3.79  Advanced      Novice         0            -0.838700             1.0      -0.784827
    27     yes  3.96  Advanced    Advanced         0            -0.809895             1.0      -3.696742
    26     yes  3.57  Advanced    Advanced         1            -0.848139             1.0      -3.283073
    37      no  3.52    Novice      Novice         1                  NaN             NaN            NaN
    36      no  3.00  Advanced      Novice         0             1.923077             1.0       1.923077
    35      no  3.68    Novice    Beginner         1             1.582278             1.0      -0.632911
    33      no  3.55    Novice      Novice         1             1.622323             1.0      -1.844813
    >>>
    # Example 3: Calculate the slope of the univariate linear regression line
    #            of the column 'gpa' for all non-null data pairs with independent
    #            variable as all other columns, which are grouped by 'masters'
    #            and 'gpa', 'admitted' in a Contracting window, partitioned over
    #            'masters' and order by 'masters' with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa", "admitted"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             order_columns=group_by_df.masters.nulls_last(),
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute regr_slope() on the Contracting window.
    >>> window.regr_slope(admissions_train.gpa)
      masters   gpa  admitted  admitted_regr_slope  gpa_regr_slope
    0     yes  3.75         0            -0.432627             1.0
    1     yes  2.65         1             0.125413             1.0
    2     yes  3.50         1             0.056046             1.0
    3     yes  3.76         0             0.095627             1.0
    4     yes  3.90         1             0.039339             1.0
    5     yes  1.98         0            -0.004912             1.0
    6      no  3.44         0             0.239624             1.0
    7      no  3.60         1             0.286300             1.0
    8      no  3.52         1             0.317647             1.0
    9      no  3.55         1             0.809892             1.0
    >>>
regr_sxx
teradataml.dataframe.window.regr_sxx = regr_sxx(expression)
DESCRIPTION:
    Function returns the sum of the squares of the independent variable
    expression for all non-null data pairs of dependent and an independent
    variable arguments over the specified window. When function is executed,
    "expression" is treated as an independent variable and dependent variable is:
        * a ColumnExpression when invoked using a window created on ColumnExpression.
        * all columns of the teradataml DataFrame which are valid for this function,
          when executed on a window created on teradataml DataFrame.
    Note:
        When there are fewer than two non-null data point pairs in the
        data used for the computation, the function returns None.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a column or name of the column or a
        literal representing an independent variable for the regression.
        An independent variable is a treatment: something that is varied under
        your control to test the behavior of another variable.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the sum of the squares of column 'gpa' for all
    #            non-null data pairs with dependent variable as 'admitted',
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.admitted.window(partition_columns="programming",
    ...                                           window_start_point=-2,
    ...                                           window_end_point=0)
    >>>
    # Execute regr_sxx() on the Rolling window and attach it to the DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate
    #       operations in one single call. In this example, we are executing
    #       regr_sxx() along with count() window aggregate operations.
    >>> df = admissions_train.assign(regr_sxx_admitted_gpa=window.regr_sxx(admissions_train.gpa),
    ...                              count_gpa=window.count())
    >>> df
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  regr_sxx_admitted_gpa
    id
    15     yes  4.00  Advanced    Advanced         1          3           8.006667e-02
    16      no  3.70  Advanced    Advanced         1          3           5.306667e-02
    11      no  3.13  Advanced    Advanced         1          3           3.604667e-01
    9       no  3.82  Advanced    Advanced         1          3           2.718000e-01
    19     yes  1.98  Advanced    Advanced         0          3           1.932800e+00
    27     yes  3.96  Advanced    Advanced         0          3           2.147467e+00
    1      yes  3.95  Beginner    Beginner         0          1          -4.796510e-16
    34     yes  3.85  Advanced    Beginner         0          2           5.000000e-03
    32     yes  3.46  Advanced    Beginner         0          3           1.340667e-01
    40     yes  3.95    Novice    Beginner         0          3           1.340667e-01
    >>>
    # Example 2: Calculate the sum of the squares for all columns as
    #            dependent variable and 'gpa' as independent variable,
    #            in an Expanding window, partitioned over 'programming',
    #            and order by 'id' in descending order.
    # Create an Expanding window on DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute regr_sxx() on the Expanding window.
    >>> df = window.regr_sxx(admissions_train.gpa)
    >>> df
       masters   gpa     stats programming  admitted  admitted_regr_sxx  gpa_regr_sxx   id_regr_sxx
    id
    38     yes  2.65  Advanced    Beginner         1       9.800000e-01  9.800000e-01  9.800000e-01
    32     yes  3.46  Advanced    Beginner         0       1.106480e+00  1.106480e+00  1.106480e+00
    31     yes  3.50  Advanced    Beginner         1       1.107333e+00  1.107333e+00  1.107333e+00
    30     yes  3.79  Advanced      Novice         0       1.166771e+00  1.166771e+00  1.166771e+00
    27     yes  3.96  Advanced    Advanced         0       1.436400e+00  1.436400e+00  1.436400e+00
    26     yes  3.57  Advanced    Advanced         1       1.443160e+00  1.443160e+00  1.443160e+00
    37      no  3.52    Novice      Novice         1      -4.891920e-16 -4.891920e-16 -4.891920e-16
    36      no  3.00  Advanced      Novice         0       1.352000e-01  1.352000e-01  1.352000e-01
    35      no  3.68    Novice    Beginner         1       2.528000e-01  2.528000e-01  2.528000e-01
    33      no  3.55    Novice      Novice         1       2.696750e-01  2.696750e-01  2.696750e-01
    >>>
    # Example 3: Calculate the sum of the squares for all columns with independent
    #            variable as 'gpa', which are grouped by 'masters' and 'gpa' in a
    #            Contracting window, partitioned over 'masters' and order by 'masters'
    #            with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns="masters",
    ...                             order_columns="masters",
    ...                             nulls_first=False,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute regr_sxx() on the Contracting window.
    >>> window.regr_sxx(admissions_train.gpa)
      masters   gpa  gpa_regr_sxx
    0      no  3.71      0.630000
    1      no  3.52      0.733410
    2      no  3.68      0.734400
    3      no  3.83      0.796367
    4      no  3.55      1.133143
    5      no  3.96      1.133333
    6     yes  3.59      1.743133
    7     yes  3.95      3.680286
    8     yes  3.46      3.976088
    9     yes  3.76      3.986600
    >>>
regr_sxy
teradataml.dataframe.window.regr_sxy = regr_sxy(expression)
DESCRIPTION:
    Function returns the sum of the products of the independent variable and the
    dependent variable for all non‑null data pairs of the dependent and independent
    variable arguments over the specified window. When function is executed, "expression"
    is treated as an independent variable and dependent variable is:
        * a ColumnExpression when invoked using a window created on ColumnExpression.
        * all columns of the teradataml DataFrame which are valid for this function,
          when executed on a window created on teradataml DataFrame.
    Note:
        When there are fewer than two non-null data point pairs in the
        data used for the computation, the function returns None.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a column or name of the column or a
        literal representing an independent variable for the regression.
        An independent variable is a treatment: something that is varied under
        your control to test the behavior of another variable.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the sum of the products for 'admitted' and 'gpa',
    #            with 'admitted' as dependent variable and 'gpa' as independent
    #            variable in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.admitted.window(partition_columns="programming",
    ...                                           window_start_point=-2,
    ...                                           window_end_point=0)
    >>>
    # Execute regr_sxy() on the Rolling window and attach it to the DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate
    #       operations in one single call. In this example, we are executing
    #       regr_sxy() along with count() window aggregate operations.
    >>> df = admissions_train.assign(regr_sxy_admitted_gpa=window.regr_sxy(admissions_train.gpa),
    ...                              count_gpa=window.count())
    >>> df
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  regr_sxy_admitted_gpa
    id
    15     yes  4.00  Advanced    Advanced         1          3               0.000000
    16      no  3.70  Advanced    Advanced         1          3               0.000000
    11      no  3.13  Advanced    Advanced         1          3               0.000000
    9       no  3.82  Advanced    Advanced         1          3               0.000000
    19     yes  1.98  Advanced    Advanced         0          3               1.120000
    27     yes  3.96  Advanced    Advanced         0          3               0.353333
    1      yes  3.95  Beginner    Beginner         0          1               0.000000
    34     yes  3.85  Advanced    Beginner         0          2               0.000000
    32     yes  3.46  Advanced    Beginner         0          3               0.000000
    40     yes  3.95    Novice    Beginner         0          3               0.000000
    >>>
    # Example 2: Calculate the sum of the products between 'gpa' as independent
    #            variable and all other columns as dependent variable,
    #            in an Expanding window, partitioned over 'programming',
    #            and order by 'id' in descending order.
    # Create an Expanding window on DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute regr_sxy() on the Expanding window.
    >>> df = window.regr_sxy(admissions_train.gpa)
    >>> df
    >>> df
       masters   gpa     stats programming  admitted  admitted_regr_sxy  gpa_regr_sxy   id_regr_sxy
    id
    38     yes  2.65  Advanced    Beginner         1          -0.800000  9.800000e-01  1.300000e+00
    32     yes  3.46  Advanced    Beginner         0          -0.882000  1.106480e+00  2.140000e-01
    31     yes  3.50  Advanced    Beginner         1          -0.903333  1.107333e+00  3.633333e-01
    30     yes  3.79  Advanced      Novice         0          -0.978571  1.166771e+00 -9.157143e-01
    27     yes  3.96  Advanced    Advanced         0          -1.163333  1.436400e+00 -5.310000e+00
    26     yes  3.57  Advanced    Advanced         1          -1.224000  1.443160e+00 -4.738000e+00
    37      no  3.52    Novice      Novice         1           0.000000 -4.891920e-16  8.437695e-15
    36      no  3.00  Advanced      Novice         0           0.260000  1.352000e-01  2.600000e-01
    35      no  3.68    Novice    Beginner         1           0.400000  2.528000e-01 -1.600000e-01
    33      no  3.55    Novice      Novice         1           0.437500  2.696750e-01 -4.975000e-01
    >>>
    # Example 3: Calculate the sum of the products between between column 'gpa'
    #            and all other columns as independent variable, which are grouped
    #            by 'masters', 'gpa' and 'admitted' in a Contracting window, partitioned
    #            over 'masters' and order by 'masters' with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa", "admitted"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             order_columns=group_by_df.masters.nulls_last(),
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute regr_sxy() on the Contracting window.
    >>> window.regr_sxy(admissions_train.gpa)
      masters   gpa  admitted  admitted_regr_sxy  gpa_regr_sxy
    0     yes  3.90         1           1.030000      3.126750
    1     yes  3.57         1           0.950000      3.158040
    2     yes  3.81         1           0.783636      3.279818
    3     yes  4.00         0           0.725000      3.292425
    4     yes  3.75         0           0.170000      4.186200
    5     yes  3.59         1          -0.021333      4.343093
    6      no  3.44         0           0.260000      0.160000
    7      no  3.68         1           0.234286      0.187771
    8      no  3.96         1           0.218750      0.201287
    9      no  3.70         1           0.237778      0.227356
    >>>
regr_syy
teradataml.dataframe.window.regr_syy = regr_syy(expression)
DESCRIPTION:
    Function returns the sum of the squares of the dependent variable
    expression for all non-null data pairs of dependent and an independent
    variable arguments over the specified window. When function is executed,
    "expression" is treated as an independent variable and dependent
    variable is:
        * a ColumnExpression when invoked using a window created on ColumnExpression.
        * all columns of the teradataml DataFrame which are valid for this function,
          when executed on a window created on teradataml DataFrame.
    Note:
        When there are fewer than two non-null data point pairs in the
        data used for the computation, the function returns None.
PARAMETERS:
    expression:
        Required Argument.
        Specifies a ColumnExpression of a column or name of the column or a
        literal representing an independent variable for the regression.
        An independent variable is a treatment: something that is varied under
        your control to test the behavior of another variable.
        Types: ColumnExpression OR int OR float OR str
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Note:
    #     In the examples here, ColumnExpression is passed as input. User can
    #     choose to pass column name instead of the ColumnExpression.
    # Example 1: Calculate the sum of the squares of column 'gpa' for all
    #            non-null data pairs with dependent variable as 'admitted',
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.admitted.window(partition_columns="programming",
    ...                                           window_start_point=-2,
    ...                                           window_end_point=0)
    >>>
    # Execute regr_syy() on the Rolling window and attach it to the DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate
    #       operations in one single call. In this example, we are executing
    #       regr_syy() along with count() window aggregate operations.
    >>> df = admissions_train.assign(regr_syy_admitted_gpa=window.regr_syy(admissions_train.gpa),
    ...                              count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  regr_syy_admitted_gpa
    id
    15     yes  4.00  Advanced    Advanced         1          3               0.000000
    16      no  3.70  Advanced    Advanced         1          3               0.000000
    11      no  3.13  Advanced    Advanced         1          3               0.000000
    9       no  3.82  Advanced    Advanced         1          3               0.000000
    19     yes  1.98  Advanced    Advanced         0          3               0.666667
    27     yes  3.96  Advanced    Advanced         0          3               0.666667
    1      yes  3.95  Beginner    Beginner         0          1               0.000000
    34     yes  3.85  Advanced    Beginner         0          2               0.000000
    32     yes  3.46  Advanced    Beginner         0          3               0.000000
    40     yes  3.95    Novice    Beginner         0          3               0.000000
    >>>
    # Example 2: Calculate the sum of the squares for all columns and
    #            'gpa' with independent variable as 'gpa',
    #            in an Expanding window, partitioned over 'programming',
    #            and order by 'id' in descending order.
    # Create an Expanding window on DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute regr_syy() on the Expanding window.
    >>> df = window.regr_syy(admissions_train.gpa)
    >>> df
       masters   gpa     stats programming  admitted  admitted_regr_syy  gpa_regr_syy  id_regr_syy
    id
    38     yes  2.65  Advanced    Beginner         1           0.666667  9.800000e-01     2.000000
    32     yes  3.46  Advanced    Beginner         0           0.800000  1.106480e+00    47.200000
    31     yes  3.50  Advanced    Beginner         1           1.333333  1.107333e+00    73.333333
    30     yes  3.79  Advanced      Novice         0           1.428571  1.166771e+00   100.857143
    27     yes  3.96  Advanced    Advanced         0           1.555556  1.436400e+00   176.000000
    26     yes  3.57  Advanced    Advanced         1           2.100000  1.443160e+00   224.400000
    37      no  3.52    Novice      Novice         1           0.000000 -4.891920e-16     0.000000
    36      no  3.00  Advanced      Novice         0           0.500000  1.352000e-01     0.500000
    35      no  3.68    Novice    Beginner         1           0.666667  2.528000e-01     2.000000
    33      no  3.55    Novice      Novice         1           0.750000  2.696750e-01     8.750000
    >>>
    # Example 3: Calculate the sum of the squares between all columns and independent
    #            variable 'gpa', which are grouped by 'masters' and 'gpa' in a
    #            Contracting window, partitioned over 'masters' and order by 'masters'
    #            with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             order_columns=group_by_df.masters.nulls_last(),
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute regr_syy() on the Contracting window.
    >>> window.regr_syy(admissions_train.gpa)
      masters   gpa  gpa_regr_syy
    0      no  3.71      1.044400
    1      no  3.87      1.061200
    2      no  3.93      1.094018
    3      no  3.60      1.104567
    4      no  3.52      1.133143
    5      no  3.68      1.133333
    6     yes  3.76      3.115400
    7     yes  2.33      3.220400
    8     yes  3.81      3.725800
    9     yes  1.98      4.167600
    >>>
row_number
teradataml.dataframe.window.row_number = row_number()
DESCRIPTION:
    Function returns the sequential row number, starting with first row as
    number one, for all the rows in a teradataml DataFrame or ColumnExpression,
    according to "order_columns", over the specified window.
    Notes:
         1. Window parameter "order_columns" should not be None.
         2. Unlike other window aggregate functions, executing row_number()
            on a window created on teradataml DataFrame does not create
            multiple columns, that is, it results in one new column in addition
            to the original teradataml DataFrame columns. For calculating
            row_number, value passed to "order_columns" parameter of window
            is considered and not the ColumnExpression.
PARAMETERS:
    None.
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Get the row number over the values in a window,
    #            partitioned over 'programming' and sort by 'gpa'.
    # Create a window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      order_columns="gpa")
    # Execute row_number() on the window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing row_number() along
    #       with mean() window aggregate operations.
    >>> df = admissions_train.assign(row_number=window.row_number(), mean_gpa=window.mean())
    >>> df
       masters   gpa     stats programming  admitted  mean_gpa  row_number
    id
    32     yes  3.46  Advanced    Beginner         0  3.660000           3
    35      no  3.68    Novice    Beginner         1  3.660000           5
    3       no  3.70    Novice    Beginner         1  3.660000           6
    39     yes  3.75  Advanced    Beginner         0  3.660000           7
    34     yes  3.85  Advanced    Beginner         0  3.660000           9
    21      no  3.87    Novice    Beginner         1  3.660000          10
    19     yes  1.98  Advanced    Advanced         0  3.615625           1
    11      no  3.13  Advanced    Advanced         1  3.615625           2
    14     yes  3.45  Advanced    Advanced         0  3.615625           3
    6      yes  3.50  Beginner    Advanced         1  3.615625           4
    >>>
    # Example 2: Get the row number on a window created on
    #            teradataml DataFrame, partitioned over 'masters',
    #            and order by 'gpa'.
    # Create a window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.gpa)
    >>>
    # Execute row_number() on a window.
    >>> df = window.row_number()
    >>> df
       masters   gpa     stats programming  admitted  col_row_number
    id
    38     yes  2.65  Advanced    Beginner         1               3
    32     yes  3.46  Advanced    Beginner         0               5
    22     yes  3.46    Novice    Beginner         0               6
    4      yes  3.50  Beginner      Novice         1               7
    6      yes  3.50  Beginner    Advanced         1               9
    26     yes  3.57  Advanced    Advanced         1              10
    24      no  1.87  Advanced      Novice         1               1
    36      no  3.00  Advanced      Novice         0               2
    11      no  3.13  Advanced    Advanced         1               3
    5       no  3.44    Novice      Novice         0               4
    >>>
std
teradataml.dataframe.window.std = std(distinct=False, population=False)
DESCRIPTION:
    Function returns the standard deviation for the non-null data points in
    a teradataml DataFrame or ColumnExpression over the specified window.
    The standard deviation is the second moment of either a sample or population.
    For a population, it is a measure of dispersion from the mean of that population.
    For a sample, it is a measure of dispersion from the mean of that sample. The
    computation is more conservative for the population standard deviation to minimize
    the effect of outliers on the computed value.
    When there are fewer than two non-null data points in the sample used for the
    computation, the function returns None.
PARAMETERS:
    distinct:
        Optional Argument.
        Specifies a flag that decides whether to consider duplicate values in
        a column or not.
        Default Values: False
        Types: bool
    population:
        Optional Argument.
        Specifies whether to calculate standard deviation on entire population
        or not. Set this argument to True only when the data points represent
        the complete population. If your data represents only a sample of the
        entire population for the column, then set this variable to False,
        which computes the sample standard deviation. As the sample size increases,
        the values for sample standard deviation and population standard deviation
        approach the same number. You should always use the more conservative sample
        standard deviation calculation, unless you are absolutely certain that your
        data constitutes the entire population.
        for the column.
        Default Value: False
        Types: bool
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the sample standard deviation for
    #            the column 'gpa' in a Rolling window, partitioned over
    #            'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute std() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing std() along with
    #       count() window aggregate operations.
    >>> df = admissions_train.assign(std_gpa=window.std(), count_gpa=window.count())
    >>> df
       masters   gpa     stats programming  admitted  count_gpa  std_gpa
    id
    15     yes  4.00  Advanced    Advanced         1          3     3.60
    16      no  3.70  Advanced    Advanced         1          3     3.70
    11      no  3.13  Advanced    Advanced         1          3     3.13
    9       no  3.82  Advanced    Advanced         1          3     3.13
    19     yes  1.98  Advanced    Advanced         0          3     1.98
    27     yes  3.96  Advanced    Advanced         0          3     1.98
    1      yes  3.95  Beginner    Beginner         0          1     3.95
    34     yes  3.85  Advanced    Beginner         0          2     3.85
    32     yes  3.46  Advanced    Beginner         0          3     3.46
    40     yes  3.95    Novice    Beginner         0          3     3.46
    >>>
    # Example 2: Calculate the population standard deviation for all
    #            the valid columns in teradataml DataFrame, in an Expanding
    #            window, partitioned over 'programming', and order by 'id' in
    #            descending order.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id.desc(),
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute std() on the Expanding window.
    >>> df = window.std(population=True)
    >>> df
       masters   gpa     stats programming  admitted  admitted_std   gpa_std    id_std
    id
    38     yes  2.65  Advanced    Beginner         1      0.471405  0.571548  0.816497
    32     yes  3.46  Advanced    Beginner         0      0.400000  0.470421  3.072458
    31     yes  3.50  Advanced    Beginner         1      0.471405  0.429599  3.496029
    30     yes  3.79  Advanced      Novice         0      0.451754  0.408267  3.795809
    27     yes  3.96  Advanced    Advanced         0      0.415740  0.399500  4.422166
    26     yes  3.57  Advanced    Advanced         1      0.458258  0.379889  4.737088
    37      no  3.52    Novice      Novice         1      0.000000  0.000000  0.000000
    36      no  3.00  Advanced      Novice         0      0.500000  0.260000  0.500000
    35      no  3.68    Novice    Beginner         1      0.471405  0.290287  0.816497
    33      no  3.55    Novice      Novice         1      0.433013  0.259651  1.479020
    >>>
    # Example 3: Calculate the sample standard deviation for all the valid
    #            columns in teradataml DataFrame, which are grouped by 'masters'
    #            and 'gpa' in a Contracting window, partitioned over 'masters' and
    #            order by 'masters' with nulls listed last.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             order_columns=group_by_df.masters.nulls_last(),
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute std() on the Contracting window.
    >>> window.std()
      masters   gpa   gpa_std
    0     yes  3.79  0.447269
    1     yes  3.50  0.583218
    2     yes  3.96  0.559739
    3     yes  4.00  0.540462
    4     yes  3.90  0.655494
    5     yes  2.33  0.631651
    6      no  3.52  0.746961
    7      no  3.83  0.697233
    8      no  3.82  0.688829
    9      no  3.55  0.652312
    >>>
sum
teradataml.dataframe.window.sum = sum(distinct=False)
DESCRIPTION:
    Function returns the sum of values in a teradataml
    DataFrame or ColumnExpression over the specified window.
PARAMETERS:
    distinct:
        Optional Argument.
        Specifies a flag that decides whether to consider duplicate values in
        a column or not.
        Default Values: False
        Types: bool
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the sum of values for the 'gpa' column
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute sum() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing sum() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(sum_gpa=window.sum(), max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  max_gpa  sum_gpa
    id
    15     yes  4.00  Advanced    Advanced         1     4.00    11.41
    16      no  3.70  Advanced    Advanced         1     4.00    11.66
    11      no  3.13  Advanced    Advanced         1     3.96    10.79
    9       no  3.82  Advanced    Advanced         1     3.82    10.65
    19     yes  1.98  Advanced    Advanced         0     3.82     9.30
    27     yes  3.96  Advanced    Advanced         0     3.96     9.44
    1      yes  3.95  Beginner    Beginner         0     3.95     3.95
    34     yes  3.85  Advanced    Beginner         0     3.95     7.80
    32     yes  3.46  Advanced    Beginner         0     3.95    11.26
    40     yes  3.95    Novice    Beginner         0     3.95    11.26
    >>>
    # Example 2: Calculate the sum of values for all the valid columns in
    #            teradataml DataFrame, in an Expanding window, partitioned
    #            over 'programming', and order by 'id'.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.masters,
    ...                                  order_columns=admissions_train.id,
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute sum() on the Expanding window.
    >>> df = window.sum()
    >>> df
       masters   gpa     stats programming  admitted  admitted_sum  gpa_sum  id_sum
    id
    4      yes  3.50  Beginner      Novice         1             1    11.21       7
    7      yes  2.33    Novice      Novice         1             3    17.04      20
    14     yes  3.45  Advanced    Advanced         0             3    20.49      34
    15     yes  4.00  Advanced    Advanced         1             4    24.49      49
    19     yes  1.98  Advanced    Advanced         0             5    30.28      86
    20     yes  3.90  Advanced    Advanced         1             6    34.18     106
    3       no  3.70    Novice    Beginner         1             1     3.70       3
    5       no  3.44    Novice      Novice         0             1     7.14       8
    8       no  3.60  Beginner    Advanced         1             2    10.74      16
    9       no  3.82  Advanced    Advanced         1             3    14.56      25
    >>>
    # Example 3: Calculate the sum of values for all the valid columns in
    #            teradataml DataFrame, which are grouped by 'masters' and
    #            'gpa' in a Contracting window, partitioned over 'masters'.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute sum() on the Contracting window.
    >>> window.sum()
      masters   gpa  gpa_sum
    0     yes  3.79    29.06
    1     yes  3.50    35.29
    2     yes  3.96    39.10
    3     yes  4.00    42.95
    4     yes  3.90    48.50
    5     yes  2.33    51.96
    6      no  3.52    19.59
    7      no  3.83    23.24
    8      no  3.82    27.24
    9      no  3.55    30.95
    >>>
var
teradataml.dataframe.window.var = var(distinct=False, population=False)
DESCRIPTION:
    Function returns the variance for the data points in a teradataml
    DataFrame or ColumnExpression over the specified window.
    By default calculates the variance of sample. Variance of a
    sample is a measure of dispersion from the mean of that sample.
    It is the square of the sample standard deviation. However,
    if parameter "population" is True, then the function calculates
    the variance of a population. Variance of a population is a measure
    of dispersion from the mean of that population.
    The computation is more conservative than that for the population
    standard deviation to minimize the effect of outliers on the computed
    value. When the sample used for the computation has fewer than two non-null
    data points, the function returns None.
PARAMETERS:
    distinct:
        Optional Argument.
        Specifies a flag that decides whether to consider duplicate values in
        a column or not.
        Default Values: False
        Types: bool
    population:
        Optional Argument.
        Specifies whether to calculate variance on entire population or not.
        Set this argument to True only when the data points represent the complete
        population. If your data represents only a sample of the entire population
        for the columns, then set this variable to False, which computes the
        sample variance. As the sample size increases, the values for sample
        variance and population variance approach the same number. You should
        always use the more conservative sample standard deviation calculation,
        unless you are absolutely certain that your data constitutes the entire
        population for the columns.
        Default Value: False
        Types: bool
RETURNS:
    * teradataml DataFrame - When aggregate is executed using window created
      on teradataml DataFrame.
    * ColumnExpression, also known as, teradataml DataFrameColumn - When aggregate is
      executed using window created on ColumnExpression.
RAISES:
    RuntimeError - If column does not support the aggregate operation.
EXAMPLES:
    # Load the data to run the example.
    >>> load_example_data("dataframe", "admissions_train")
    >>>
    # Create a teradataml DataFrame on 'admissions_train' table.
    >>> admissions_train = DataFrame("admissions_train")
    >>> admissions_train
       masters   gpa     stats programming  admitted
    id
    22     yes  3.46    Novice    Beginner         0
    36      no  3.00  Advanced      Novice         0
    15     yes  4.00  Advanced    Advanced         1
    38     yes  2.65  Advanced    Beginner         1
    5       no  3.44    Novice      Novice         0
    17      no  3.83  Advanced    Advanced         1
    34     yes  3.85  Advanced    Beginner         0
    13      no  4.00  Advanced      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    >>>
    # Example 1: Calculate the sample variance for the column 'gpa'
    #            in a Rolling window, partitioned over 'programming'.
    # Create a Rolling window on 'gpa'.
    >>> window = admissions_train.gpa.window(partition_columns="programming",
    ...                                      window_start_point=-2,
    ...                                      window_end_point=0)
    >>>
    # Execute var() on the Rolling window and attach it to the teradataml DataFrame.
    # Note: DataFrame.assign() allows combining multiple window aggregate operations
    #       in one single call. In this example, we are executing var() along with
    #       max() window aggregate operations.
    >>> df = admissions_train.assign(var_gpa=window.var(), max_gpa=window.max())
    >>> df
       masters   gpa     stats programming  admitted  max_gpa   var_gpa
    id
    15     yes  4.00  Advanced    Advanced         1     4.00  0.040033
    16      no  3.70  Advanced    Advanced         1     4.00  0.026533
    11      no  3.13  Advanced    Advanced         1     3.96  0.180233
    9       no  3.82  Advanced    Advanced         1     3.82  0.135900
    19     yes  1.98  Advanced    Advanced         0     3.82  0.966400
    27     yes  3.96  Advanced    Advanced         0     3.96  1.073733
    1      yes  3.95  Beginner    Beginner         0     3.95       NaN
    34     yes  3.85  Advanced    Beginner         0     3.95  0.005000
    32     yes  3.46  Advanced    Beginner         0     3.95  0.067033
    40     yes  3.95    Novice    Beginner         0     3.95  0.067033
    >>>
    # Example 2: Calculate the population variance for all valid columns
    #            in teradataml DataFrame, in an Expanding window, partitioned
    #            over 'programming'.
    # Create an Expanding window on teradataml DataFrame.
    >>> window = admissions_train.window(partition_columns=admissions_train.programming,
    ...                                  window_start_point=None,
    ...                                  window_end_point=0)
    >>>
    # Execute var() on the Expanding window.
    >>> df = window.var(population=True)
    >>> df
       masters   gpa     stats programming  admitted  admitted_var   gpa_var     id_var
    id
    4      yes  3.50  Beginner      Novice         1      0.222222  0.034022   1.555556
    7      yes  2.33    Novice      Novice         1      0.240000  0.319336   5.200000
    14     yes  3.45  Advanced    Advanced         0      0.250000  0.266358  18.222222
    15     yes  4.00  Advanced    Advanced         1      0.244898  0.270212  26.285714
    19     yes  1.98  Advanced    Advanced         0      0.246914  0.459180  43.358025
    20     yes  3.90  Advanced    Advanced         1      0.240000  0.439076  48.840000
    3       no  3.70    Novice    Beginner         1      0.000000  0.000000   0.000000
    5       no  3.44    Novice      Novice         0      0.250000  0.016900   1.000000
    8       no  3.60  Beginner    Advanced         1      0.222222  0.011467   4.222222
    9       no  3.82  Advanced    Advanced         1      0.187500  0.019400   5.687500
    >>>
    # Example 3: Calculate the sample of variance for all the valid columns
    #            in teradataml DataFrame, which are grouped by 'masters' and
    #            'gpa' in a Contracting window, partitioned over 'masters'.
    # Perform group_by() operation on teradataml DataFrame.
    >>> group_by_df = admissions_train.groupby(["masters", "gpa"])
    # Create a Contracting window on teradataml DataFrameGroupBy object.
    >>> window = group_by_df.window(partition_columns=group_by_df.masters,
    ...                             window_start_point=-5,
    ...                             window_end_point=None)
    # Execute var() on the Contracting window.
    >>> window.var()
      masters   gpa   gpa_var
    0     yes  3.79  0.200050
    1     yes  3.50  0.340143
    2     yes  3.96  0.313307
    3     yes  4.00  0.292099
    4     yes  3.90  0.429673
    5     yes  2.33  0.398983
    6      no  3.52  0.557950
    7      no  3.83  0.486133
    8      no  3.82  0.474486