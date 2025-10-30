# DataFrame Manipulation (Core API)

## DataFrame Manipulation
You can manipulate a DataFrame with methods and operators. The DataFrames created using the
DataFrame() constructor, or the DataFrame() and DataFrame.from_table() and DataFrame.from_query()
functions have the same methods and operators.
#### DataFrame Methods
A DataFrame method has the basic syntax DataFrame.method(arguments). Using the specified DataFrame
and arguments, the method returns a new DataFrame. The specified DataFrame remains unchanged.
alias() Method
    Creates an aliased teradataml DataFrame.
assign() Method
    Assigns new column expressions in a DataFrame.
concat() Method
    Concatenate two teradataml DataFrame objects along the index axis.
describe() Method
    Generates statistics for numeric columns. Computes the count, mean, std, min, percentiles,
    and max for numeric columns.
drop() Method
    Drops specified labels from rows or columns in a DataFrame.
dropna() Method
    Removes rows with null values in a DataFrame.
filter() Method
    Returns only the filtered columns or rows (based on the index) of a DataFrame.
    Filter is item, like, or regex.
    Other filters are operators index[] and loc[].
fillna() Method
    Replaces the null values in a column with the value specified.
get() Method
    Retrieves required columns from a DataFrame using column names as key.
get_values() Method
    Retrieves all values (only) present in a DataFrame.
groupby() Method
    Returns all columns of a DataFrame, grouped as specified.
head() Method
    Returns the first n rows of a DataFrame.
itertuples() Method
    Iterates over teradataml DataFrame rows as namedtuples.
join() Method
    Joins two different teradataml DataFrames together.
merge() Method
    Merges two teradataml DataFrames together.
sample() Method
    Samples rows from a DataFrame, directly or based on conditions.
select() Method
    Returns only the selected columns of a DataFrame.
set_index() Method
    Assigns one or more existing columns as the new index to a DataFrame.
show_query() Method
    Returns underlying SQL for the teradataml DataFrame.
sort() Method
    Returns all columns of a DataFrame, sorted as specified.
sort_index() Method
    Returns sorted objects by labels (along an axis) in either ascending or descending order for
    a DataFrame.
squeeze() Method
    Squeezes one-dimensional axis objects into a scalar for DataFrames with a single element,
    or a Series object for a DataFrame with a single column.
tail() Method
    Returns the last n rows of the sorted DataFrame.
For a list of regular aggregate functions supported by DataFrame, see Regular Aggregate Functions
Supported by DataFrame.
For a list of time series aggregate functions, see Time Series Aggregate Functions.
Note:
* The describe() and get_values() methods do not return a teradataml DataFrame.
* The groupby() method returns instance of teradataml DataFrameGroupBy, which is inherited from
    teradataml DataFrame.
* The groupby_time() and resample() methods return instance of teradataml
    DataFrameGroupByTime, which is inherited from teradataml DataFrame.
#### Functions
drop_duplicate() Function
    Drops duplicate rows from teradataml DataFrame to return distinct values from
    the DataFrame.
#### Operators
A DataFrame operator has the basic syntax as follows:
* DataFrame.loc[arguments]
* DataFrame.iloc[arguments]
* DataFrame[arguments]
All operators are filters. Another filter is the filter() method.
index[] Operator
    Returns only the filtered rows of a DataFrame.
    Filter uses logical expressions composed of DataFrame columns and Python literals.
loc[] Operator
    Returns new DataFrame that has only the filtered columns and rows of a DataFrame accessed
    by labels.
iloc[] Operator
    Returns new DataFrame that has only the filtered columns and rows of a DataFrame accessed
    by integer values.
### select() Method
Use the select() method to select columns in a DataFrame. The function takes a select expression as an
argument and returns a new DataFrame with the selected columns. The expression can be a single column
name, a list of column names, or a list of column name lists.
Note:
Multicolumn selection of the same column (for example, df.select(['col1', 'col1'])) is not supported.
#### Examples Prerequisite
Assume the table "admissions_train" exists and its index column is id. And a DataFrame "df" is created
based on this table using the command:
```python
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming admitted
id
5        no  3.44    novice      novice        0
7       yes  2.33    novice      novice        1
22      yes  3.46    novice    beginner        0
17       no  3.83  advanced    advanced        1
13       no  4.00  advanced      novice        1
19      yes  1.98  advanced    advanced        0
36       no  3.00  advanced      novice        0
15      yes  4.00  advanced    advanced        1
34      yes  3.85  advanced    beginner        0
40      yes  3.95    novice    beginner        0
```
#### Example 1: Expression is single column name
```python
>>> df.select("id")
Empty DataFrame
Columns: []
Index: [22, 34, 13, 19, 15, 38, 26, 5, 36, 17]
```
#### Example 2: Expression is list of column names
```python
>>> df.select(["id", "masters", "gpa"])
masters   gpa
id
5        no  3.44
36       no  3.00
15      yes  4.00
17       no  3.83
13       no  4.00
40      yes  3.95
7       yes  2.33
22      yes  3.46
34      yes  3.85
19      yes  1.98
```
#### Example 3: Expression is list of column name lists
```python
>>> df.select([['id', 'masters', 'gpa']])
masters   gpa
id
5        no  3.44
34      yes  3.85
13       no  4.00
40      yes  3.95
22      yes  3.46
19      yes  1.98
36       no  3.00
15      yes  4.00
7       yes  2.33
17       no  3.83
```
### drop and dropna
#### drop() Method
Use the drop() method to drop specified labels from rows or columns in a DataFrame. The method takes
a single label or a list of labels as an argument and returns a new DataFrame with the specified rows or
columns removed.
Arguments:
* labels
  This is an optional argument. It could be single label or list-like. It also can be Index or column labels
  to drop depending on the axis argument.
* axis
  This is an optional argument. Use the axis argument to specify whether the labels refer to index labels
  or column labels:
  * 0 or 'index' for index labels
  * 1 or 'column' for column labels
  The default is 0.
* columns
  This is an optional argument. Use the columns argument to specify a single column label or a list of
  column labels. The columns argument is an alternative to specifying axis=1 with labels. You cannot
  specify both labels and columns.
#### Examples Prerequisite
Assume the table "admissions_train" exists and its index column is "id". And a DataFrame "df" is created
based on this table using the command:
```python
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming admitted
id
5        no  3.44    novice      novice        0
7       yes  2.33    novice      novice        1
22      yes  3.46    novice    beginner        0
17       no  3.83  advanced    advanced        1
13       no  4.00  advanced      novice        1
19      yes  1.98  advanced    advanced        0
36       no  3.00  advanced      novice        0
15      yes  4.00  advanced    advanced        1
34      yes  3.85  advanced    beginner        0
40      yes  3.95    novice    beginner        0
```
#### Example 1: Drop columns 'stats' and 'admitted' with axis=1
```python
>>> df.drop(['stats', 'admitted'], axis=1)
programming masters   gpa
id
5        novice      no  3.44
34     beginner     yes  3.85
13       novice      no  4.00
40     beginner     yes  3.95
22     beginner     yes  3.46
19     advanced     yes  1.98
36       novice      no  3.00
15     advanced     yes  4.00
7        novice     yes  2.33
17     advanced      no  3.83
```
#### Example 2: Drop columns 'stats' and 'admitted' with columns argument
```python
>>> df.drop(columns=['stats', 'admitted'])
programming masters   gpa
id
5        novice      no  3.44
34     beginner     yes  3.85
13       novice      no  4.00
19     advanced     yes  1.98
15     advanced     yes  4.00
40     beginner     yes  3.95
7        novice     yes  2.33
22     beginner     yes  3.46
36       novice      no  3.00
17     advanced      no  3.83
```
#### Example 3: Drop rows with index values 13 and 15 with axis=0
```python
>>> df1 = df[df.gpa == 4.00]
>>> df1
masters  gpa     stats programming admitted
id
13      no  4.0  Advanced      Novice        1
29     yes  4.0    Novice    Beginner        0
15     yes  4.0  Advanced    Advanced        1
>>> df1.drop([13,15], axis=0)
masters  gpa   stats programming admitted
id
29     yes  4.0  Novice    Beginner        0
>>>
```
#### dropna() Method
Use the dropna() method to remove rows with null values in a DataFrame.
Arguments:
* how: optional argument specifies how rows are removed. It has options 'any' or 'all'.
  * 'any': Removes rows with at least one null value.
  * 'all': Removes rows with all null values.
  The default is 'any'.
* thresh: optional argument specifies the minimum number of non-null values in a row to include.
* subset: optional argument specifies list of column names to include, in array-like format. Use this
  argument to limit the search for null values to specific columns.
#### Examples Prerequisite
Assume the table "sales" exists. And a DataFrame "df" is created using the command:
```python
>>> df = DataFrame("sales")
>>> df
Feb   Jan   Mar   Apr    datetime
accounts
Jones LLC     200.0   150   140   180  2017-04-01
Yellow Inc     90.0  None  None  None  2017-04-01
Orange Inc    210.0  None  None   250  2017-04-01
Blue Inc       90.0    50    95   101  2017-04-01
Alpha Co      210.0   200   215   250  2017-04-01
Red Inc       200.0   150   140  None  2017-04-01
```
#### Example 1: Drop rows with at least one Null value
```python
>>> df.dropna()
Feb  Jan  Mar  Apr    datetime
accounts
Blue Inc       90.0   50   95  101  2017-04-01
Jones LLC     200.0  150  140  180  2017-04-01
Alpha Co      210.0  200  215  250  2017-04-01
```
#### Example 2: Keep rows with at least four Non-Null values
```python
>>> df.dropna(thresh=4)
Feb   Jan   Mar   Apr    datetime
accounts
Jones LLC   200.0   150   140   180  2017-04-01
Blue Inc     90.0    50    95   101  2017-04-01
Orange Inc  210.0  None  None   250  2017-04-01
Alpha Co    210.0   200   215   250  2017-04-01
Red Inc     200.0   150   140  None  2017-04-01
```
#### Example 3: Keep rows with at least five Non-Null values
```python
>>> df.dropna(thresh=5)
Feb  Jan  Mar   Apr    datetime
accounts
Alpha Co   210.0  200  215   250  2017-04-01
Jones LLC  200.0  150  140   180  2017-04-01
Blue Inc    90.0   50   95   101  2017-04-01
Red Inc    200.0  150  140  None  2017-04-01
```
#### Example 4: Drop rows with all Null values in columns 'Jan' and 'Mar'
```python
>>> df.dropna(how='all', subset=['Jan','Mar'])
Feb  Jan  Mar   Apr    datetime
accounts
Alpha Co   210.0  200  215   250  2017-04-01
Jones LLC  200.0  150  140   180  2017-04-01
Red Inc    200.0  150  140  None  2017-04-01
Blue Inc    90.0   50   95   101  2017-04-01
```
### assign() Method
Use the assign() method to assign new column expressions in a teradataml DataFrame. A new DataFrame
is returned without modifying the existing DataFrame.
```python
assign(self, drop_columns = False, **kwargs)
```
The expressions are given as key value pairs where the keys are column names and the values are column
expressions. The values can include arithmetic expressions that involve supported python literals and
columns (ColumnExpression instances) from the DataFrame.
When the 'drop_columns' parameter is True, it removes columns from the resulting DataFrame if they are
not specified in assign. It is False by default, so columns from the previous DataFrame are retained.
Note:
assign() accepts ColumnExpressions returned by udf - Function decorator and call_udf().
Refer to teradataml DataFrame Column for more details about ColumnExpressions in teradataml.
Note:
* The values in kwargs is not callable for now.
* Since kwargs is a dictionary, the order of your arguments may not be preserved. To make
    things predictable, the columns are inserted in alphabetical order, at the end of your DataFrame.
    Assigning multiple columns within the same assign() is possible, but you cannot reference other
    columns created within the same assign call.
* If no kwargs are given, the function returns self.
* The maximum number of columns in a DataFrame is 2048.
#### Supported Types and Operators
Python int, float, Decimal, str and None literals can be used in assign expressions. All arithmetic
expressions except floor division (//) and power (**) are supported.
#### Example 1: Add new columns, retaining original DataFrame columns in
#### resulting DataFrame
This example adds new columns created using arithmetic operations and constants, retaining original
DataFrame columns in resulting DataFrame.
```python
>>> df = DataFrame("iris_test")
>>> df
sepal_length  sepal_width  petal_length  petal_width species
id
120           6.0          2.2           5.0          1.5       3
20            5.1          3.8           1.5          0.3       1
60            5.2          2.7           3.9          1.4       2
10            4.9          3.1           1.5          0.1       1
15            5.8          4.0           1.2          0.2       1
30            4.7          3.2           1.6          0.2       1
5.6          2.5           3.9          1.1       2
65            5.6          2.9           3.6          1.3       2
5             5.0          3.6           1.4          0.2       1
80            5.7          2.6           3.5          1.0       2
```
Alias the columns to use in assign:
```python
>>> s_len = df.sepal_length
>>> p_len = df.petal_length
```
Add new column expressions to DataFrame:
```python
>>> df.select(['sepal_length', 'petal_length']).\
...    assign(sum  = s_len + p_len,
...           diff = s_len - p_len,
...           prod = s_len * p_len,
...           div = s_len / p_len,
...           mod = s_len % p_len,
...           num_constant = 1,
...           str_constant = 'string')
```
sepal_length  petal_length  diff       div  mod num_constant   prod str_constant   sum
0           5.6           3.9   1.7  1.435897  1.7            1  21.84       string   9.5
1           5.7           3.5   2.2  1.6285
2.2            1  19.95       string   9.2
2           6.0           5.0   1.0  1.200000  1.0            1  30.00       string  11.0
3           4.9           1.5   3.4  3.266667  0.4            1   7.35       string   6.4
4           5.0           1.4   3.6  3.5
429  0.8            1   7.00       string   6.4
5           5.1           1.5   3.6  3.400000  0.6            1   7.65       string   6.6
6           5.2           3.9   1.3  1.333333  1.3            1  20.28       string   9.1
7           5.6           3.6   2.0  1.555556  2.0            1  20.16       string   9.2
8           5.1           1.5   3.6  3.400000  0.6            1   7.65       string   6.6
9           4.7           1.6   3.1  2.937500  1.5            1   7.52       string   6.3
#### Example 2: Add new columns, dropping original DataFrame columns in
#### resulting DataFrame
This example adds new columns created using arithmetic operations and constants, dropping original
DataFrame columns in resulting DataFrame.
```python
>>> df.assign(drop_columns = True,
...           sum  = s_len + p_len,
...           diff = s_len - p_len,
...           prod = s_len * p_len,
...           div = s_len / p_len,
...           mod = s_len % 2,
...           num_constant = 1,
...           str_constant = 'string'
diff       div  mod num_constant   prod str_constant   sum
0   1.0  1.200000  0.0            1  30.00       string  11.0
1   3.1  2.937500  0.7            1   7.52       string   6.3
2   1.7  1.435897  1.6            1  21.84       string   9.5
3   3.6  3.5
429  1.0            1   7.00       string   6.4
4   1.3  1.333333  1.2            1  20.28       string   9.1
5   3.4  3.266667  0.9            1   7.35       string   6.4
6   2.0  1.555556  1.6            1  20.16       string   9.2
7   3.6  3.400000  1.1            1   7.65       string   6.6
8   4.6  4.833333  1.8            1   6.96       string   7.0
9   2.2  1.628571  1.7            1  19.95       string   9.2
```
### Regular Aggregate Functions Supported by DataFrame
teradataml DataFrame supports following set of regular aggregate functions which can be used with and
without DataFrame.groupby().
See the DataFrame Aggregate Functions section of Teradata Package for Python Function Reference,
B700-4008) at https://docs.teradata.com/ for detailed description and usage examples of these functions.
Sr.
    Function Name Description
No.
1 corr() Returns the Sample Pearson product moment correlation coefficient of its
          arguments for all non-null data point pairs.
2 count() Returns column-wise count of the dataframe.
3 covar_pop() Returns the column-wise population covariance of its arguments for all non-
          null data point pairs.
          Covariance measures whether or not two random variables vary in the same
          way. It is the average of the products of deviations for each non-null data
          point pair.
4 covar_samp() Returns the column-wise sample covariance of its arguments for all non-null
          data point pairs.
          Covariance measures whether or not two random variables vary in the same
          way. It is the average of the products of deviations for each non-null data
          point pair.
5 kurtosis() Returns column-wise kurtosis value of the dataframe.
          Kurtosis is the fourth moment of the distribution of the standardized (z) values.
          It is a measure of the outlier (rare, extreme observation) character of the
          distribution as compared with the normal (or Gaussian) distribution.
          * The normal distribution has a kurtosis of 0.
          * Positive kurtosis indicates that the distribution is more outlier-prone than the
            normal distribution.
          * Negative kurtosis indicates that the distribution is less outlier-prone than the
            normal distribution.
6 max() Returns column-wise maximum value of the dataframe.
7 mean() Returns column-wise mean value of the dataframe.
8 median() Returns column-wise median value of the dataframe.
9 min() Returns column-wise minimum value of the dataframe.
10 percentile() Return the value which represents the desired percentile.
11 regr_avgx() Returns the column-wise mean of the independent variable for all non-null
          data pairs of the dependent and an independent variable arguments.
Sr.
    Function Name Description
No.
12 regr_avgy() Returns the column-wise mean of the dependent variable for all non-null data
          pairs of the dependent and independent variable arguments.
13 regr_count() Returns the column-wise count of all non-null data pairs of the dependent and
          independent variable arguments.
14 regr_intercept() Returns the column-wise intercept of the univariate linear regression
          line through all non-null data pairs of the dependent and independent
          variable arguments.
          The intercept is the point at which the regression line through the non-null data
          pairs in the sample intersects the ordinate, or y-axis, of the graph.
15 regr_r2() Returns the column-wise coefficient of determination for all non-null data pairs
          of the dependent and independent variable arguments.
16 regr_slope() Returns the column-wise coefficient slope of the univariate linear regression
          line through all non-null data pairs of the dependent and an independent
          variable arguments.
17 regr_sxx() Returns the column-wise sum of the squares of the independent variable
          expression for all non-null data pairs of dependent and an independent
          variable arguments.
| 18 | regr_sxy() | Returns the column-wise sum of the products of the independent variable |  |  |
| -- | ---------- | ----------------------------------------------------------------------- | - | - |
|  |  | and the dependent variable for all non | ‑ | null data pairs of the dependent and |
|  |  | independent variable arguments. |  |  |

19 regr_syy() Returns the column-wise sum of the squares of the dependent variable
          expression for all non-null data pairs of dependent and an independent
          variable arguments.
20 skew() Returns column-wise skewness of the distribution of the dataframe.
          Skewness is the third moment of a distribution. It is a measure of the
          asymmetry of the distribution about its mean compared with the normal (or
          Gaussian) distribution.
          * The normal distribution has a skewness of 0.
          * Positive skewness indicates a distribution having an asymmetric tail
            extending toward more positive values.
          * Negative skewness indicates an asymmetric tail extending toward more
            negative values.
21 std() Returns column-wise sample or population standard deviation value of the
          dataframe. The standard deviation is the second moment of a distribution.
          * For a sample, it is a measure of dispersion from the mean of that sample.
          * For a population, it is a measure of dispersion from the mean of
            that population.
          The computation is more conservative for the population standard deviation to
          minimize the effect of outliers on the computed value.
22 sum() Returns column-wise sum value of the dataframe.
Sr.
    Function Name Description
No.
23 var() Returns column-wise sample or population variance of the columns in
          a dataframe.
          * The variance of a population is a measure of dispersion from the mean of
            that population.
          * The variance of a sample is a measure of dispersion from the mean of that
            sample. It is the square of the sample standard deviation.
24 agg() Perform aggregates using one or more operations.
          Note:
          You can request different percentiles while running the agg() function.
teradataml special aggregate functions
25 csum() Returns column-wise cumulative sum value for rows in the partition of
          the dataframe.
26 msum() Computes the moving sum for the current row and the preceding "width"-1
          rows in a partition, by sorting the rows according to "sort_columns".
27 mavg() Computes the moving average for the current row and the preceding "width"-1
          rows in a partition, by sorting the rows according to "sort_columns".
28 mdiff() Computes the moving difference for the current row and the preceding "width"
          rows in a partition, by sorting the rows according to "sort_columns".
29 mlinreg() Computes the moving linear regression for the current row and the preceding
          "width"-1 rows in a partition, by sorting the rows according to "sort_columns".
### groupby() Method
Use the groupby() method to group one or more columns of a DataFrame.
The method takes a column name or a list of column names to group by.
Note:
* You can still apply teradataml DataFrame methods (filters/sort/etc) on top of the result of this one.
* Consecutive operations of grouping, i.e., groupby_time(), resample() and groupby() are not
    permitted. An exception will be raised. Following are some cases where exception will be raised
    as "Invalid operation applied, check documentation for correct usage."
    * df.groupby().groupby()
    * df.groupby().resample()
    * df.groupby().groupby_time()
#### Required Parameter
column_expr
    Specifies the column names to group by.
#### Optional Parameter
kwargs
    Specifies keyword arguments.
    option
        Specifies the groupby option.
        Permitted values: "CUBE", "ROLLUP", None.
    include_grouping_columns
        Specifies whether to include aggregations on the grouping columns or not.
        When set to True, the resultant DataFrame will have the aggregations on the
        columns mentioned in "columns_expr". Otherwise, resultant DataFrame will
        not have aggregations on the columns mentioned in "columns_expr".
        Default value: False
#### Examples Prerequisite
Assume a teradata DataFrame "df" is created based on the table "admissions_train", using the command:
```python
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming admitted
id
13      no  4.00  Advanced      Novice        1
26     yes  3.57  Advanced    Advanced        1
5       no  3.44    Novice      Novice        0
19     yes  1.98  Advanced    Advanced        0
15     yes  4.00  Advanced    Advanced        1
40     yes  3.95    Novice    Beginner        0
7      yes  2.33    Novice      Novice        1
22     yes  3.46    Novice    Beginner        0
36      no  3.00  Advanced      Novice        0
38     yes  2.65  Advanced    Beginner        1
```
#### Example 1: Groups by one column and finds the min
The following example groups by column "masters" and finds the min for the groups in "masters":
```python
>>> df2 = df.groupby("masters")
>>> df2.min()
masters min_id  min_gpa min_stats min_programming min_admitted
0      no      3     1.87  advanced        advanced            0
1     yes      1     1.98  advanced        advanced            0
```
#### Example 2: Groups by two columns and finds the min and max
This example finds the min and max grouped by "masters" and "programming":
```python
>>> df3 = df.groupby(["masters", "programming"])
>>> df3.min()
masters programming min_id  min_gpa min_stats min_admitted
0      no    Advanced      8     3.13  Advanced            1
1     yes    Beginner      1     2.65  Advanced            0
2     yes      Novice      4     2.33  Advanced            0
3      no    Beginner      3     3.68    Novice            1
4     yes    Advanced      6     1.98  Advanced            0
5      no      Novice      5     1.87  Advanced            0
>>> df3.max()
masters programming max_id  max_gpa max_stats max_admitted
0      no      Novice     37     4.00    Novice            1
1      no    Advanced     28     3.96  Beginner            1
2      no    Beginner     35     3.87    Novice            1
3     yes    Advanced     27     4.00  Beginner            1
4     yes      Novice     30     3.79    Novice            1
5     yes    Beginner     40     4.00    Novice            1
```
#### Example 3: Select multiple columns, followed by groupby and find the min
This example selects "id", "masters", "gpa", "stats", followed by a groupby on "masters", and finds the min.
```python
>>> df1 = df.select(["id", "masters", "gpa", "stats"])
>>> df2 = df1.groupby("masters")
>>> df2.min()
masters min_id  min_gpa min_stats
0      no      3     1.87  advanced
1     yes      1     1.98  advanced
```
### Time Series Aggregates
The Time Series aggregate functions help perform aggregate operations on time series data.
In this section, you can find examples using APIs to group or resample time series data and other Time
Series Aggregate functions.
#### Example Setup
Examples for Time Series Aggregates functions share same steps to set up the environment.
The following lists the procedure to load the example datasets and create required DataFrames to prepare
for the examples of Time Series Aggregate functions.
* Load the example datasets
```python
>>> load_example_data("dataframe", ["ocean_buoys",
"ocean_buoys_seq", "ocean_buoys_nonpti"])
```
* Create required DataFrames:
  * DataFrame on non-sequenced PTI table
```python
>>> ocean_buoys = DataFrame("ocean_buoys")
```
    Check the DataFrame columns:
```python
>>>ocean_buoys.columns
['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
```
    Check the head of the DataFrame:
    >>> ocean_buoys.head()
    TD_TIMECODE  temperature  salinity
    buoyid
    0       2014-01-06 08:10:00.000000        100.0        55
    0       2014-01-06 08:08:59.999999          NaN        55
    1       2014-01-06 09:01:25.122200
.0        55
    1       2014-01-06 09:03:25.122200         79.0        55
    1       2014-01-06 09:01:25.122200         70.0        55
    1       2014-01-06 09:02:25.122200         71.0        55
    1       2014-01-06 09:03:25.122200         72.0        55
    0       2014-01-06 08:09:59.999999         99.0        55
    0       2014-01-06 08:00:00.000000         10.0        55
    0       2014-01-06 08:10:00.000000         10.0        55
  * DataFrame on sequenced PTI table
```python
>>> ocean_buoys_seq = DataFrame("ocean_buoys_seq")
```
    Check the DataFrame columns:
```python
>>> ocean_buoys_seq.columns
['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity',
'temperature', 'dates']
```
    Check the head of the DataFrame:
```python
>>> ocean_buoys_seq.head()
TD_TIMECODE  TD_SEQNO  salinity
temperature       dates
buoyid
0       2014-01-06 08:00:00.000000        26        55
10.0  2016-02-26
0       2014-01-06 08:08:59.999999        18        55
NaN  2015-06-18
1       2014-01-06 09:02:25.122200        24        55
.0  2015-12-24
1       2014-01-06 09:01:25.122200        23        55
77.0  2015-11-23
1       2014-01-06 09:02:25.122200        12        55
71.0  2014-12-12
1       2014-01-06 09:03:25.122200        13        55
72.0  2015-01-13
1       2014-01-06 09:01:25.122200        11        55
70.0  2014-11-11
0       2014-01-06 08:10:00.000000        19        55
10.0  2015-07-19
0       2014-01-06 08:09:59.999999        17        55
99.0  2015-05-17
0       2014-01-06 08:10:00.000000        27        55
100.0  2016-03-27
```
  * DataFrame on non-PTI table
```python
>>> ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
```
    Check the DataFrame columns:
```python
>>> ocean_buoys_nonpti.columns
['buoyid', 'timecode', 'temperature', 'salinity']
```
    Check the head of the DataFrame:
```python
>>> ocean_buoys_nonpti.head()
buoyid  temperature  salinity
timecode
2014-01-06 08:09:59.999999       0         99.0        55
2014-01-06 08:10:00.000000       0         10.0        55
2014-01-06 09:01:25.122200       1         70.0        55
2014-01-06 09:01:25.122200       1         77.0        55
2014-01-06 09:02:25.122200       1         71.0        55
2014-01-06 09:03:25.122200       1         72.0        55
2014-01-06 09:02:25.122200       1         78.0        55
2014-01-06 08:10:00.000000       0        100.0        55
2014-01-06 08:08:59.999999       0          NaN        55
2014-01-06 08:00:00.000000       0         10.0        55
```
#### groupby_time()
The groupby_time() function resamples time series data to group the same by time on a datetime column
of a teradataml DataFrame. It also allows grouping based on teradataml DataFrame columns.
Although the grouping is optimized for DataFrames created for PTI tables, it is also supported on non-PTI
tables when the argument 'timecode_column' is specified.
Note:
* This API is similar to resample().
* You can still apply teradataml DataFrame methods (filters/sort/etc) on top of the result of
    this one.
* Consecutive operations of grouping, i.e., groupby_time(), resample() and groupby() are not
    permitted. An exception will be raised. Following are some cases where exception will be raised
    as "Invalid operation applied, check documentation for correct usage."
    * df.groupby_time().groupby()
    * df.groupby_time().resample()
    * df.groupby_time().groupby_time()
#### Examples Prerequisite
* Load the example datasets:
```python
>>> load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_nonpti"])
```
* Create required DataFrames:
* DataFrame on non-sequenced PTI table
```python
>>> ocean_buoys = DataFrame("ocean_buoys")
```
    Check the DataFrame columns:
```python
>>>ocean_buoys.columns
['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
```
    Check the head of the DataFrame:
```python
>>> ocean_buoys.head()
TD_TIMECODE  temperature  salinity
buoyid
0       2014-01-06 08:10:00.000000        100.0        55
0       2014-01-06 08:08:59.999999          NaN        55
1       2014-01-06 09:01:25.122200         77.0        55
1       2014-01-06 09:03:25.122200         79.0        55
1       2014-01-06 09:01:25.122200         70.0        55
1       2014-01-06 09:02:25.122200         71.0        55
1       2014-01-06 09:03:25.122200         72.0        55
0       2014-01-06 08:09:59.999999         99.0        55
0       2014-01-06 08:00:00.000000         10.0        55
0       2014-01-06 08:10:00.000000         10.0        55
```
  * DataFrame on non-PTI table
```python
>>> ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
```
    Check the DataFrame columns:
```python
>>> ocean_buoys_nonpti.columns
['buoyid', 'timecode', 'temperature', 'salinity']
```
    Check the head of the DataFrame:
```python
>>> ocean_buoys_nonpti.head()
buoyid  temperature  salinity
timecode
2014-01-06 08:09:59.999999       0         99.0        55
2014-01-06 08:10:00.000000       0         10.0        55
2014-01-06 09:01:25.122200       1         70.0        55
2014-01-06 09:01:25.122200       1         77.0        55
2014-01-06 09:02:25.122200       1         71.0        55
2014-01-06 09:03:25.122200       1         72.0        55
2014-01-06 09:02:25.122200       1         78.0        55
2014-01-06 08:10:00.000000       0        100.0        55
2014-01-06 08:08:59.999999       0          NaN        55
2014-01-06 08:00:00.000000       0         10.0        55
```
    Create DataFrameGroupByTime object.
```python
>>> ocean_buoys_grpby =
ocean_buoys.groupby_time(timebucket_duration="2cy",
value_expression="buoyid", fill="NULLS")
```
#### Example 1: Group by timebucket of two calendar years, on DataFrame
#### created on non-sequenced PTI table
Use formal notation and 'buoyid' column on DataFrame created on non-sequenced PTI table.
Fill missing values with Nulls.
```python
>>> ocean_buoys_grpby1 =
ocean_buoys.groupby_time(timebucket_duration="CAL_YEARS(2)",
value_expression="buoyid", fill="NULLS")
>>> number_of_values_to_column = {2: "temperature"}
```
>>> ocean_buoys_grpby1.bottom(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  bottom2temperature
0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0                  10
1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0                  10
2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                  71
3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                  70
4  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2                  80
5  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2
6  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44                  43
7  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44                  43
#### Example 2: Group by timebucket of two minutes, on DataFrame created on
#### non-PTI table
Use shorthand notation to specify timebucket, on DataFrame created on non-PTI table.
Fill missing values with Nulls.
Note:
'timecode_column' must be specified for non-PTI table.
```python
>>> ocean_buoys_nonpti_grpby2 =
ocean_buoys_nonpti.groupby_time(timebucket_duration="2m",
value_expression="buoyid", timecode_column="timecode", fill="NULLS")
>>> number_of_values_to_column = {2: "temperature"}
```
>>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE", "buoyid"])
TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))
buoyid  bottom_with_ties2temperature
0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961
0                          10.0
1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962
0                           NaN
2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963
0                           NaN
3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964
0                           NaN
4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965
0                          99.0
5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                         100.0
6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                          10.0
7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                          70.0
8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                          77.0
9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992
1                          71.0
#### Example 3: Group by timebucket of two minutes, on DataFrame created on
#### non-PTI table
Use shorthand notation to specify timebucket, on DataFrame created on non-PTI table.
Fill missing values with previous values.
Note:
'timecode_column' must be specified for non-PTI table.
```python
>>> ocean_buoys_nonpti_grpby2 =
ocean_buoys_nonpti.groupby_time(timebucket_duration="2mins",
value_expression="buoyid", timecode_column="timecode", fill="prev")
>>> number_of_values_to_column = {2: "temperature"}
```
>>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE","buoyid"])
TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))
buoyid  bottom_with_ties2temperature
0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961
0                            10
1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962
0                            10
2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963
0                            10
3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964
0                            10
4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965
0                            99
5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                            10
6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                           100
7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                            77
8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                            70
9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992
1                            71
#### Example 4: Group by timebucket of two minutes, on DataFrame created on
#### non-PTI table
Use shorthand notation to specify timebucket, on DataFrame created on non-PTI table.
Fill missing values with numeric constant 12345.
Note:
'timecode_column' must be specified for non-PTI table.
```python
>>> ocean_buoys_nonpti_grpby2 =
ocean_buoys_nonpti.groupby_time(timebucket_duration="2minute",
value_expression="buoyid", timecode_column="timecode", fill=12345)
>>> number_of_values_to_column = {2: "temperature"}
```
>>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE", "buoyid"])
TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))
buoyid  bottom_with_ties2temperature
0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961
0                            10
1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962
0                         12345
2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963
0                         12345
3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964
0                         12345
4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965
0                            99
5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                            10
6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                           100
7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                            77
8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                            70
9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992
1                            71
#### resample()
The resample() function resamples time series data to group the same by time on a datetime column of
a teradataml DataFrame. It also allows grouping done based on teradataml DataFrame columns.
This function applies Group By Time to one or more columns of a teradataml DataFrame. Outcome of this
function can be used to run Time Series Aggregate functions.
Although the grouping is optimized for DataFrames created for PTI tables, it is also supported on non-PTI
tables when the argument 'timecode_column' is specified.
Note:
* This API is similar to groupby_time().
* You can still apply teradataml DataFrame methods (filters/sort/etc) on top of the result of
    this one.
* Consecutive operations of grouping, i.e., groupby_time(), resample() and groupby() are not
    permitted. An exception will be raised. Following are some cases where exception will be raised
    as "Invalid operation applied, check documentation for correct usage."
    * df.resample().groupby()
    * df.resample().resample()
    * df.resample().groupby_time()
#### Examples Prerequisite
See Example Setup to set up the environment for the following examples.
#### Example 1: Group by timebucket of two calendar years, on DataFrame
#### created on non-sequenced PTI table
Use formal notation and 'buoyid' column on DataFrame created on non-sequenced PTI table.
Fill missing values with Nulls.
```python
>>> ocean_buoys_grpby1 = ocean_buoys.resample(rule="CAL_YEARS(2)",
value_expression="buoyid", fill_method="NULLS")
>>> number_of_values_to_column = {2: "temperature"}
```
>>> ocean_buoys_grpby1.bottom(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  bottom2temperature
0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0                  10
1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0                  10
2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                  71
3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                  70
4  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2                  80
5  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2                  81
6  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44                  43
7  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44                  43
#### Example 2: Group by timebucket of two minutes, on DataFrame created on
#### non-PTI table
Use shorthand notation to specify timebucket, on DataFrame created on non-PTI table.
Fill missing values with Nulls.
Note:
Time column must be specified for non-PTI table to 'on' argument.
```python
>>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.resample(rule="2m",
value_expression="buoyid", on="timecode", fill_method="NULLS")
>>> number_of_values_to_column = {2: "temperature"}
```
>>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE", "buoyid"])
TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))
buoyid  bottom_with_ties2temperature
0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961
0                          10.0
1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962
0                           NaN
2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963
0                           NaN
3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964
0                           NaN
4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965
0                          99.0
5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                         100.0
6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                          10.0
7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                          70.0
8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                          77.0
9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992
1                          71.0
#### Example 3: Group by timebucket of two minutes, on DataFrame created on
#### non-PTI table
Use shorthand notation to specify timebucket, on DataFrame created on non-PTI table.
Fill missing values with previous values.
Note:
Time column must be specified for non-PTI table to 'on' argument.
```python
>>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.resample(rule="2m",
value_expression="buoyid", on="timecode", fill_method="prev")
>>> number_of_values_to_column = {2: "temperature"}
```
>>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE","buoyid"])
TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))
buoyid  bottom_with_ties2temperature
0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961
0                            10
1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962
0                            10
2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963
0                            10
3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964
0                            10
4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965
0                            99
5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                            10
6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                           100
7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                            77
8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                            70
9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992
1                            71
#### Example 4: Group by timebucket of two minutes, on DataFrame created on
#### non-PTI table
Use shorthand notation to specify timebucket, on DataFrame created on non-PTI table.
Fill missing values with numeric constant 12345.
Note:
Time column must be specified for non-PTI table to 'on' argument.
```python
>>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.resample(rule="2minute",
value_expression="buoyid", on="timecode", fill_method=12345)
>>> number_of_values_to_column = {2: "temperature"}
```
>>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE", "buoyid"])
TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))
buoyid  bottom_with_ties2temperature
0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961
0                            10
1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962
0                         12345
2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963
0                         12345
3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964
0                         12345
4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965
0                            99
5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                            10
6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966
0                           100
7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                            77
8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991
1                            70
9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992
1                            71
#### Time Series Aggregate Functions
teradataml supports following set of time series aggregate functions which can be invoked on
DataFrame.groupby_time() or DataFrame.resample().
See the teradataml: Time Series Functions section of Teradata Package for Python Function Reference,
B700-4008) at https://docs.teradata.com/ for detailed description and usage examples of these functions.
Sr.
No.
    Function
          Description
    Name
1 bottom() Returns the smallest number of values in the columns for each group, with or
          without ties.
2 count() Returns column-wise count for each group.
3 describe() Generates statistics for numeric columns. It computes max, mean, min, std,
          median, mode, and percentiles for numeric columns.
          Default statistics include: 'max', 'mean', 'min', 'std'.
Sr.
No.
    Function
          Description
    Name
4 delta_t() Calculates the time difference, or DELTA_T, between a starting and an
          ending event. The calculation is performed against a time-ordered time series
          data set.
5 first() Returns the oldest value, determined by the timecode, for each group.
6 kurtosis() Returns column-wise kurtosis value for each group.
          Kurtosis is the fourth moment of the distribution of the standardized (z) values.
          It is a measure of the outlier (rare, extreme observation) character of the
          distribution as compared with the normal (or Gaussian) distribution.
          * The normal distribution has a kurtosis of 0.
          * Positive kurtosis indicates that the distribution is more outlier-prone than the
          normal distribution.
          * Negative kurtosis indicates that the distribution is less outlier-prone than the
          normal distribution.
7 last() Returns the newest value, determined by the timecode, for each group.
8 mad() Median Absolute Deviation (MAD) returns the median of the set of values
          defined as the absolute value of the difference between each value and the
          median of all values in each group.
9 max() Returns column-wise maximum value for each group.
10 mean() Returns column-wise mean value for each group.
11 median() Returns column-wise median value for each group.
12 min() Returns column-wise minimum value for each group.
13 mode() Returns the column-wise mode of all values in each group.
14 percentile() Return the value which represents the desired percentile from each group.
          The result value is determined by the desired index (di) in an ordered list of
          values. The following equation is for the di:
          di = (number of values in group - 1) * percentile/100
          When di is a whole number, that value is the returned result. The di can
          also be between two data points, i and j, where i<j. In that case, the result is
          interpolated according to the value specified in interpolation argument.
15 skew() Returns column-wise skewness of the distribution for each group.
          Skewness is the third moment of a distribution. It is a measure of the
          asymmetry of the distribution about its mean compared with the normal (or
          Gaussian) distribution.
          * The normal distribution has a skewness of 0.
          * Positive skewness indicates a distribution having an asymmetric tail
          extending toward more positive values.
          * Negative skewness indicates an asymmetric tail extending toward more
          negative values.
Sr.
No.
    Function
          Description
    Name
16 std() Returns column-wise sample or population standard deviation value for each
          group. The standard deviation is the second moment of a distribution.
          * For a sample, it is a measure of dispersion from the mean of that sample.
          * For a population, it is a measure of dispersion from the mean of
          that population.
          The computation is more conservative for the population standard deviation
          to minimize the effect of outliers on the computed value.
17 sum() Returns column-wise sum value for each group.
18 var() Returns column-wise sample or population variance of the columns for
          each group.
          * The variance of a population is a measure of dispersion from the mean of
          that population.
          * The variance of a sample is a measure of dispersion from the mean of that
          sample. It is the square of the sample standard deviation.
19 top() Returns the largest number of values in the columns for each group, with or
          without ties.
### concat() Method
Use the concat() method to concatenate two teradataml DataFrame objects along the index axis. The
operation is performed by carrying out a database-style union or union all operation.
#### Examples Prerequisite
Assume the table "admissions_train" exists and a DataFrame "df" is created based on this table using
the command:
```python
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming admitted
id
22     yes  3.46    Novice    Beginner        0
36      no  3.00  Advanced      Novice        0
15     yes  4.00  Advanced    Advanced        1
38     yes  2.65  Advanced    Beginner        1
5       no  3.44    Novice      Novice        0
17      no  3.83  Advanced    Advanced        1
34     yes  3.85  Advanced    Beginner        0
13      no  4.00  Advanced      Novice        1
26     yes  3.57  Advanced    Advanced        1
19     yes  1.98  Advanced    Advanced        0
>>> df1 = df[df.gpa == 4].select(['id', 'stats', 'masters', 'gpa'])
>>> df1
stats masters  gpa
id
13  Advanced      no  4.0
29    Novice     yes  4.0
15  Advanced     yes  4.0
>>> df2 = df[df.gpa < 2].select(['id', 'stats', 'programming', 'admitted'])
>>> df2
stats programming admitted
id
24  Advanced      Novice        1
19  Advanced    Advanced        0
```
#### Example 1: Default behavior with default values for the optional arguments.
```python
>>> # Default options
>>> cdf = df1.concat(df2)
>>> cdf
stats masters  gpa programming admitted
id
19  Advanced    None  NaN    Advanced        0
24  Advanced    None  NaN      Novice        1
13  Advanced      no  4.0        None     None
29    Novice     yes  4.0        None     None
15  Advanced     yes  4.0        None     None
```
#### Example 2: concat() operation with 'join = inner'.
```python
>>> cdf = df1.concat(df2, join='inner')
>>> cdf
stats
id
19  Advanced
24  Advanced
13  Advanced
29    Novice
15  Advanced
```
#### Example 3: concat() operation with 'allow_duplicates=False'.
```python
>>> # allow_duplicates = True (default)
>>> cdf = df1.concat(df2)
>>> cdf
stats masters  gpa programming admitted
id
19  Advanced    None  NaN    Advanced        0
24  Advanced    None  NaN      Novice        1
13  Advanced      no  4.0        None     None
29    Novice     yes  4.0        None     None
15  Advanced     yes  4.0        None     None
>>> cdf = cdf.concat(df2)
>>> cdf
stats masters  gpa programming admitted
id
19  Advanced    None  NaN    Advanced        0
13  Advanced      no  4.0        None     None
24  Advanced    None  NaN      Novice        1
24  Advanced    None  NaN      Novice        1
19  Advanced    None  NaN    Advanced        0
29    Novice     yes  4.0        None     None
15  Advanced     yes  4.0        None     None
>>> # allow_duplicates = False
>>> cdf = cdf.concat(df2, allow_duplicates=False)
>>> cdf
stats masters  gpa programming admitted
id
19  Advanced    None  NaN    Advanced        0
29    Novice     yes  4.0        None     None
24  Advanced    None  NaN      Novice        1
15  Advanced     yes  4.0        None     None
13  Advanced      no  4.0        None     None
```
#### Example 4: concat() operation with 'sort=True'.
```python
>>> cdf = df1.concat(df2, sort=True)
>>> cdf
admitted  gpa masters programming     stats
id
19        0  NaN    None    Advanced  Advanced
24        1  NaN    None      Novice  Advanced
13     None  4.0      no        None  Advanced
29     None  4.0     yes        None    Novice
15     None  4.0     yes        None  Advanced
```
### describe() Method
The describe() function generates statistics for numeric columns. This function can be used in two modes:
* Regular Aggregate Mode
  It computes the count, mean, std, min, percentiles, and max for numeric columns.
  Default statistics include: "count", "mean", "std", "min", "percentile", "max".
  Note:
    If describe() is used on the output of any DataFrame API or groupby(), then it is used in regular
    aggregate mode.
* Time Series Aggregate Mode
  It computes the max, mean, min, std, median, mode, and percentiles for numeric columns.
  Default statistics include: 'max', 'mean', 'min', 'std'.
  Note:
    If describe() is used on the output of groupby_time(), then it is used in time series aggregate
    mode, where time series aggregates are used to calculate the statistics.
#### Optional Arguments
percentiles
    A list of values between 0 and 1 used for computing percentiles.
    The default value is [.25, .5, .75], which generates the 25th, 50th, and 75th percentiles.
include
    The values for this argument can be either 'None' or 'all', used to specify if non-numeric
    columns are included in the computation.
    * If the value is 'all': Both numeric and non-numeric columns are included. The function
      computes count, mean, std, min, percentiles, and max for numeric columns, and
      computes count and unique for non-numeric columns.
    * If the value is 'None': Only numeric columns are used for collecting statics.
    The default value is 'None'.
Note:
      Value 'all' is not applicable for Time Series Aggregate Mode.
verbose
    Specifies a boolean value to be used for time series aggregation, stating whether to get
    verbose output or not. When this argument is set to 'True', function calculates median, mode,
    and percentile values on top of its default statistics.
    Note:
      Default and the only acceptable value for this argument when used in Regular
      Aggregate Mode is 'False'.
      verbose as 'True' is not applicable for Regular Aggregate Mode.
distinct
    Specifies a boolean value to decide whether to consider duplicate rows in statistic calculation
    or not.
    Note:
      By default, this argument is set to 'False', which means that duplicate values are
      considered for statistic calculation.
      When this is set to 'True', only distinct rows are considered for statistic calculation.
statistics
    Specifies the aggregate operation to be performed.
    * Computes count, mean, std, min, percentiles, and max for numeric columns.
    * Computes count and unique for non-numeric columns.
    Note:
      statistics is not applicable for 'Time Series Aggregate Mode'.
      statistics should not be used with include as 'all'.
    Permitted Values: count, mean, min, max, unique, std, describe, percentile
    The default value is None.
    Types: str or List of str
columns
    Specifies the name(s) of the columns we are collecting statistics for.
    The default value is None.
    Types: str or List of str
pivot
    Specifies a boolean value to pivot the output.
    Note:
      pivot is not supported for PTI tables.
    The default value is 'False'.
#### Aggregate Mode Examples
* Regular Aggregate Mode Examples for describe()
* Time Series Aggregate Mode Examples for describe()
#### Regular Aggregate Mode Examples for describe()
#### Example 1: Generate statistics for DataFrame "sales"
The following example computes count, mean, std, min, percentiles, and max for numeric columns
```python
>>> df = DataFrame('sales')
>>> df
Feb   Jan   Mar   Apr    datetime
accounts
Alpha Co    210.0   200   215   250  04/01/2017
Red Inc     200.0   150   140  None  04/01/2017
Orange Inc  210.0  None  None   250  04/01/2017
Jones LLC   200.0   150   140   180  04/01/2017
Yellow Inc   90.0  None  None  None  04/01/2017
Blue Inc     90.0    50    95   101  04/01/2017
>>> df.describe(pivot=True)
Apr      Feb     Mar     Jan
func
count       4        6       4       4
mean   195.25  166.667   147.5   137.5
std    70.971   59.554  49.749  62.915
min       101       90      95      50
25%    160.25    117.5  128.75     125
50%       215      200     140     150
75%       250    207.5  158.75   162.5
max       250      210     215     200
```
#### Example 2: Compute mean, min and max for numeric columns
```python
>>> df.describe(pivot=True, statistics = ['mean', 'min', 'max'], columns=
['Jan', 'Feb', 'Mar', 'Apr'])
func Feb     Jan   Mar   Apr
max  210.000 200.0 215.0 250.00
min  90.000  50.0  95.0  101.00
mean 166.667 137.5 147.5 195.25
```
#### Example 3: Use percentiles to compute the 30th and 60th percentiles
```python
>>> df.describe(percentiles=[.3, .6], pivot=True)
Apr      Feb     Mar     Jan
func
count       4        6       4       4
mean   195.25  166.667   147.5   137.5
std    70.971   59.554  49.749  62.915
min       101       90      95      50
30%     172.1      145   135.5     140
60%       236      200     140     150
max       250      210     215     200
```
#### Example 4: Group by to compute statistics for specific groups
```python
>>> df1 = df.groupby(["datetime", "Feb"])
>>> df1.describe(pivot=True)
Jan   Mar   Apr
datetime   Feb   func
04/01/2017 90.0  25%      50    95   101
50%      50    95   101
75%      50    95   101
count     1     1     1
max      50    95   101
mean     50    95   101
min      50    95   101
std    None  None  None
200.0 25%     150   140   180
50%     150   140   180
75%     150   140   180
count     2     2     1
max     150   140   180
mean    150   140   180
min     150   140   180
std       0     0  None
210.0 25%     200   215   250
50%     200   215   250
75%     200   215   250
count     1     1     2
max     200   215   250
mean    200   215   250
min     200   215   250
std    None  None     0
>>>
```
#### Example 5: Compute count, mean, std, min, percentiles, and max for numeric
#### columns with default arguments and pivot set to False
```python
>>> df.describe(pivot=False)
ATTRIBUTE StatName StatValue
```
| ATTRIBUTE | StatName | StatValue |
| --------- | -------- | --------- |
| Jan | MAXIMUM | 200.0 |
| Jan | STANDARD DEVIATION | 62.91528696058958 |
| Jan | PERCENTILES(25) | 125.0 |
| Jan | PERCENTILES(50) | 150.0 |
| Mar | COUNT | 4.0 |
| Mar | MINIMUM | 95.0 |
| Mar | MAXIMUM | 215.0 |
| Mar | MEAN | 147.5 |
| Mar | STANDARD DEVIATION | 49.749371855331 |
| Mar | PERCENTILES(25) | 128.75 |
| Mar | PERCENTILES(50) | 140.0 |
| Apr | COUNT | 4.0 |
| Apr | MINIMUM | 101.0 |
| Apr | MAXIMUM | 250.0 |
| ATTRIBUTE | StatName | StatValue |
| --------- | -------- | --------- |
| Apr | MEAN | 195.25 |
| Apr | STANDARD DEVIATION | 70.97123830585646 |
| Apr | PERCENTILES(25) | 160.25 |
| Apr | PERCENTILES(50) | 215.0 |
| Apr | PERCENTILES(75) | 250.0 |
| Feb | COUNT | 6.0 |
| Feb | MINIMUM | 90.0 |
| Feb | MAXIMUM | 210.0 |
| Feb | MEAN | 166.66666666666666 |
| Feb | STANDARD DEVIATION | 59.553897157672786 |
| Feb | PERCENTILES(25) | 117.5 |
| Feb | PERCENTILES(50) | 200.0 |
| Feb | PERCENTILES(75) | 207.5 |
| Mar | PERCENTILES(75) | 158.75 |
| Jan | PERCENTILES(75) | 162.5 |
| Jan | MEAN | 137.5 |
| Jan | MINIMUM | 50.0 |
| Jan | COUNT | 4.0 |

#### Time Series Aggregate Mode Examples for describe()
See Example Setup to set up the environment for the following examples.
#### Example 1: Get Basic Statistics
Get the basic statistics for time series aggregation for all the numeric columns, use default settings. This
example returns max, mean, min and std values.
```python
>>> ocean_buoys_grpby = ocean_buoys.groupby_time(timebucket_duration="2cy",
value_expression="buoyid", fill="NULLS")
>>> ocean_buoys_grpby.describe()
```
temperature salinity
TIMECODE_RANGE                                     GROUP BY TIME(CAL_YEARS(2)) buoyid func
('2014-01-01 00:00:00.000000-00:00', '2016-01-0... 2                           0      max          100       55
mean       54.75       55
min           10       55
std       51.674        0
1      max           79       55
mean        74.5       55
min           70       55
std        3.937        0
2      max           82       55
mean          81       55
min           80       55
std            1        0
44     max           56       55
mean      48.077       55
min           43       55
std        5.766        0
#### Example 2: Get Verbose Statistics
Get the verbose statistics for time series aggregation for all the numeric columns, use default settings.
This example returns max, mean, min, std, median, mode, 25th, 50th and 75th percentile.
```python
>>> ocean_buoys_grpby.describe(verbose=True)
```
temperature salinity
TIMECODE_RANGE                                     GROUP BY TIME(CAL_YEARS(2)) buoyid func
('2014-01-01 00:00:00.000000-00:00', '2016-01-0... 2                           0      25%             10       55
50%           54.5       55
75%          99.25       55
max            100       55
mean         54.75       55
median        54.5       55
min             10       55
mode            10       55
std         51.674        0
1      25%          71.25       55
50%           74.5       55
75%          77.75       55
max             79       55
mean          74.5       55
median        74.5       55
min             70       55
mode            71       55
mode            72       55
mode            77       55
mode            78       55
mode            79       55
mode            70       55
std          3.937        0
2      25%           80.5       55
50%             81       55
75%           81.5       55
max             82       55
mean            81       55
median          81       55
min             80       55
mode            80       55
mode            81       55
mode            82       55
std              1        0
44     25%             43       55
50%             43       55
75%             53       55
max             56       55
mean        48.077       55
median          43       55
min             43       55
mode            43       55
std          5.766        0
#### Example 3: Get Basic Statistics, Consider Only Unique Values
Get the basic statistics for time series aggregation for all the numeric columns, consider only unique
values. This example returns max, mean, min and std values.
```python
>>> ocean_buoys_grpby.describe(distinct=True)
```
temperature salinity
TIMECODE_RANGE                                     GROUP BY TIME(CAL_YEARS(2)) buoyid func
('2014-01-01 00:00:00.000000-00:00', '2016-01-0... 2                           0      max          100       55
mean      69.667       55
min           10       55
std       51.675     None
1      max           79       55
mean        74.5       55
min           70       55
std        3.937     None
2      max           82       55
mean          81       55
min           80       55
std            1     None
44     max           56       55
mean        52.2       55
min           43       55
std        5.263     None
#### Example 4: Get Verbose Statistics, Select Nondefault Percentiles
Get the verbose statistics for time series aggregation for all the numeric columns. In this example, you
select non-default percentiles 33rd and 66th. This example returns max, mean, min, std, median, mode,
33rd, and 66th percentile.
```python
>>> ocean_buoys_grpby.describe(verbose=True, percentiles=[0.33, 0.66])
```
temperature salinity
TIMECODE_RANGE                                     GROUP BY TIME(CAL_YEARS(2)) buoyid func
('2014-01-01 00:00:00.000000-00:00', '2016-01-0... 2                           0      33%             10       55
66%          97.22       55
max            100       55
mean         54.75       55
median        54.5       55
min             10       55
mode            10       55
std         51.674        0
1      33%          71.65       55
66%           77.3       55
max             79       55
mean          74.5       55
median        74.5       55
min             70       55
mode            70       55
mode            71       55
mode            77       55
mode            78       55
mode            79       55
mode            72       55
std          3.937        0
2      33%          80.66       55
66%          81.32       55
max             82       55
mean            81       55
median          81       55
min             80       55
mode            80       55
mode            81       55
mode            82       55
std              1        0
44     33%             43       55
66%             53       55
max             56       55
mean        48.077       55
median          43       55
min             43       55
mode            43       55
std          5.766        0
### alias() Method
The alias() function creates an aliased teradataml DataFrame.
Note:
Teradata recommends using this function before performing self join using DataFrame's join() API.
Required arguments:
* alias_name: Specifies the alias name to be assigned to a teradataml DataFrame.
#### Example setup
```python
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame("admissions_train")
>>> df
masters gpa stats programming admitted
id
13 no 4.00 Advanced Novice 1
26 yes 3.57 Advanced Advanced 1
5 no 3.44 Novice Novice 0
19 yes 1.98 Advanced Advanced 0
15 yes 4.00 Advanced Advanced 1
40 yes 3.95 Novice Beginner 0
7 yes 2.33 Novice Novice 1
22 yes 3.46 Novice Beginner 0
36 no 3.00 Advanced Novice 0
38 yes 2.65 Advanced Beginner 1
```
#### Example: Create an alias of a teradataml DataFrame
```python
>>> df2 = df.alias("adm_trn")
>>> df2
masters gpa stats programming admitted
id
13 no 4.00 Advanced Novice 1
26 yes 3.57 Advanced Advanced 1
5 no 3.44 Novice Novice 0
19 yes 1.98 Advanced Advanced 0
15 yes 4.00 Advanced Advanced 1
40 yes 3.95 Novice Beginner 0
7 yes 2.33 Novice Novice 1
22 yes 3.46 Novice Beginner 0
36 no 3.00 Advanced Novice 0
38 yes 2.65 Advanced Beginner 1
```
### Filtering Rows and Columns
Filtering a teradataml DataFrame can be done using the index[] Operator, the loc[] Operator, the iloc[]
Operator, or the filter() Method.
#### filter() Method
The filter method subsets rows based on the index values of a DataFrame or columns of a DataFrame
according to the labels in the specified index.
The filter is applied to the columns of the DataFrame when axis equals 'columns' (or 1). Otherwise, the
filter is applied to the values of the DataFrame index when axis equals 'rows' (or 0).
There are three ways to filter using the filter method: item based, substring based and using regular
expression. These ways are mutually exclusive in the sense that you must only use one of the following
parameters: items, like, or regex.
item based Filtering
#### Example: Filtering with axis set to 'rows' or 0
When axis is 'rows' or 0, the items list specifies the values of the column(s) specified in the index_label
of the DataFrame. Only rows where the values of the index column(s) in the items list are returned.
```python
>>> df = DataFrame('admissions_train', index_label = ['programming'])
>>> df
id masters   gpa     stats admitted
programming
Advanced     15     yes  4.00  Advanced        1
Beginner     34     yes  3.85  Advanced        0
Novice       13      no  4.00  Advanced        1
Beginner     38     yes  2.65  Advanced        1
Novice        5      no  3.44    Novice        0
Beginner     40     yes  3.95    Novice        0
Novice        7     yes  2.33    Novice        1
Beginner     22     yes  3.46    Novice        0
Advanced     26     yes  3.57  Advanced        1
Advanced     17      no  3.83  Advanced        1
>>> df.filter(items = ['Advanced', 'Novice'], axis = 0)
id masters   gpa     stats admitted
programming
Advanced     15     yes  4.00  Advanced        1
Novice       37      no  3.52    Novice        1
Novice       12      no  3.65    Novice        1
Advanced     17      no  3.83  Advanced        1
Advanced     11      no  3.13  Advanced        1
Advanced     26     yes  3.57  Advanced        1
Novice        5      no  3.44    Novice        0
Novice       24      no  1.87  Advanced        1
Novice       13      no  4.00  Advanced        1
Novice        7     yes  2.33    Novice        1
```
#### Example: Filtering with axis set to 'columns' or 1
When axis is 'columns' or 1, then column names provided in the items list are selected from
the DataFrame.
```python
>>> df.filter(items = ['programming', 'id', 'masters', 'gpa'])
id masters   gpa
programming
Advanced     15     yes  4.00
Novice        7     yes  2.33
Beginner     22     yes  3.46
Advanced     17      no  3.83
Novice       13      no  4.00
Beginner     38     yes  2.65
Advanced     26     yes  3.57
Novice        5      no  3.44
Beginner     34     yes  3.85
Beginner     40     yes  3.95
```
#### Supported types for python literals in items
You can specify float, decimal.Decimal, str, bytes, datetime.time, datetime.date, and datetime.datetime
python literal types in the items list.
substring based Filtering
#### Example 1: Filtering with axis set to 'columns' or 1
When axis is 'columns' or 1, then the like substring pattern is applied to the column names of
the DataFrame.
Note:
The substring pattern is case sensitive.
```python
>>> df.filter(like = 'st', axis = 'columns')
masters     stats
0     yes  Advanced
1     yes    Novice
2     yes    Novice
3      no  Advanced
4      no  Advanced
5     yes  Advanced
6     yes  Advanced
7      no    Novice
8     yes  Advanced
9     yes    Novice
```
#### Example 2: Filtering with axis set to 'rows' or 0
When axis is 'rows' or 0, then the like substring pattern is applied to the values of the columns specified
in the index_label of the DataFrame.
Note:
The index columns will be cast into VARCHAR columns in order to perform the match.
```python
>>> df.filter(like = 'vice', axis = 'rows')
id masters   gpa     stats admitted
programming
Novice       23     yes  3.59  Advanced        1
Novice       37      no  3.52    Novice        1
Novice       12      no  3.65    Novice        1
Novice       13      no  4.00  Advanced        1
Novice        5      no  3.44    Novice        0
Novice       24      no  1.87  Advanced        1
Novice       33      no  3.55    Novice        1
Novice        7     yes  2.33    Novice        1
Novice       30     yes  3.79  Advanced        0
Novice       36      no  3.00  Advanced        0
```
regex Filtering
#### Example 1: Filtering with axis set to 'columns' or 1
When axis is 'columns' or 1, then the regex pattern is applied to the column names of the DataFrame.
```python
>>> df.filter(regex='[a-z].*s', axis = 'columns')
masters     stats
0     yes  Advanced
1     yes    Novice
2     yes    Novice
3      no  Advanced
4      no  Advanced
5     yes  Advanced
6     yes  Advanced
7      no    Novice
8     yes  Advanced
9     yes    Novice
```
#### Example 2: Filtering with axis set to 'rows' or 0
When axis is 'rows' or 0, then the regex pattern is applied to the values of the columns specified in the
index_label of the DataFrame.
Note:
The index columns will be cast into VARCHAR columns in order to perform the match.
```python
>>> df.filter(regex = '^Beg.+', axis = 0)
id masters   gpa     stats admitted
programming
Beginner     21      no  3.87    Novice        1
Beginner     22     yes  3.46    Novice        0
Beginner     39     yes  3.75  Advanced        0
Beginner     34     yes  3.85  Advanced        0
Beginner     38     yes  2.65  Advanced        1
Beginner      3      no  3.70    Novice        1
Beginner      1     yes  3.95  Beginner        0
Beginner     32     yes  3.46  Advanced        0
Beginner     40     yes  3.95    Novice        0
Beginner     29     yes  4.00    Novice        0
```
#### index[] Operator
Use the index operator ([ ]) to filter rows based on logical expressions given by Python literals and
DataFrame columns.
Note:
Refer to teradataml DataFrame Column for more details about ColumnExpressions in teradataml.
#### Examples Prerequisite
```python
>>> df = DataFrame('iris_test')
>>> df
sepal_length  sepal_width  petal_length  petal_width species
id
120           6.0          2.2           5.0          1.5       3
30            4.7          3.2           1.6          0.2       1
70            5.6          2.5           3.9          1.1       2
5             5.0          3.6           1.4          0.2       1
60            5.2          2.7           3.9          1.4       2
10            4.9          3.1           1.5          0.1       1
65            5.6          2.9           3.6          1.3       2
20            5.1          3.8           1.5          0.3       1
15            5.8          4.0           1.2          0.2       1
80            5.7          2.6           3.5          1.0       2
>>> s_len = df.sepal_length
>>> p_len = df.petal_length
```
#### Example: Inequality operations
The operators supported are <, <=, >, >=.
```python
>>> df[s_len > p_len]
sepal_length  sepal_width  petal_length  petal_width species
id
120           6.0          2.2           5.0          1.5       3
30            4.7          3.2           1.6          0.2       1
70            5.6          2.5           3.9          1.1       2
5             5.0          3.6           1.4          0.2       1
60            5.2          2.7           3.9          1.4       2
10            4.9          3.1           1.5          0.1       1
65            5.6          2.9           3.6          1.3       2
20            5.1          3.8           1.5          0.3       1
15            5.8          4.0           1.2          0.2       1
80            5.7          2.6           3.5          1.0       2
>>> df[s_len < p_len + 1]
sepal_length  sepal_width  petal_length  petal_width species
id
105           6.5          3.0           5.8          2.2       3
85            5.4          3.0           4.5          1.5       2
115           5.8          2.8           5.1          2.4       3
150           5.9          3.0           5.1          1.8       3
135           6.1          2.6           5.6          1.4       3
```
#### Example: Equality operations
The operators supported are == and !=.
```python
>>> df[s_len == p_len + 1]
sepal_length  sepal_width  petal_length  petal_width species
id
125           6.7          3.3           5.7          2.1       3
145           6.7          3.3           5.7          2.5       3
120           6.0          2.2           5.0          1.5       3
>>> df[s_len != p_len + 2]
sepal_length  sepal_width  petal_length  petal_width species
id
120           6.0          2.2           5.0          1.5       3
20            5.1          3.8           1.5          0.3       1
60            5.2          2.7           3.9          1.4       2
10            4.9          3.1           1.5          0.1       1
30            4.7          3.2           1.6          0.2       1
70            5.6          2.5           3.9          1.1       2
15            5.8          4.0           1.2          0.2       1
5             5.0          3.6           1.4          0.2       1
80            5.7          2.6           3.5          1.0       2
40            5.1          3.4           1.5          0.2       1
```
#### Example: Boolean operations
The operators supported are &, |, ~.
Note:
Do not use 'and' operator, instead use '&' operator. Using 'and' boolean operator instead of '&' will
return incorrect results.
For example, if condition used is as length == 4.0 and width == 3.5, the first condition in
previous case (length == 4.0), in 'and' condition is completely ignored and data satisfying only
second condition (width == 3.5) will be returned.
Note:
Do not use 'or' operator, instead use '|' operator. Using 'or' boolean operator instead of '|' will return
incorrect results.
For example, if condition used is as length == 4.0 or width == 3.5, the second condition in
previous case (width == 3.5), in 'or' condition is completely ignored and data satisfying only first
condition (length == 4.0) will be returned.
```python
>>> df[~(s_len != p_len + 1)]
sepal_length  sepal_width  petal_length  petal_width species
id
125           6.7          3.3           5.7          2.1       3
145           6.7          3.3           5.7          2.5       3
120           6.0          2.2           5.0          1.5       3
>>> df[(s_len > 5.5) & (p_len < 5)]
sepal_length  sepal_width  petal_length  petal_width species
id
95            5.6          2.7           4.2          1.3       2
70            5.6          2.5           3.9          1.1       2
100           5.7          2.8           4.1          1.3       2
75            6.4          2.9           4.3          1.3       2
65            5.6          2.9           3.6          1.3       2
15            5.8          4.0           1.2          0.2       1
55            6.5          2.8           4.6          1.5       2
80            5.7          2.6           3.5          1.0       2
>>> df[(s_len >= 6.3) | (p_len == 4.2)]
sepal_length  sepal_width  petal_length  petal_width species
id
110           7.2          3.6           6.1          2.5       3
75            6.4          2.9           4.3          1.3       2
130           7.2          3.0           5.8          1.6       3
145           6.7          3.3           5.7          2.5       3
140           6.9          3.1           5.4          2.1       3
125           6.7          3.3           5.7          2.1       3
105           6.5          3.0           5.8          2.2       3
95            5.6          2.7           4.2          1.3       2
55            6.5          2.8           4.6          1.5       2
```
Note:
It is recommended to surround expressions in parentheses in order to avoid ambiguity in operator
precedence, similar to pandas.
For example, in previous example, if the expression is taken without the parenthesis, an error
is given.
```python
df[2 >= s_len | 4 <= p_len]
ArgumentError                             Traceback (most recent call last)
ArgumentError: SQL expression object or string expected, got object of
type <class 'int'> instead
TeradataMlException: [Teradata][teradataml](TDML_2021) Unable to retrieve
information for the DataFrame.
```
#### Supported Types and Operators
Python int, float, Decimal, str, and None literals can be used in filtering expressions.
All arithmetic expressions except floor division (//) and power (**) can be used.
All logical operators are supported.
### fillna() Method
The fillna() method replaces the null values in a column with the value specified.
#### Required Argument
value
    Specifies the values to replace the null values with. If value is a dict then "columns" is ignored.
    Note:
      To use predefined strings to replace the null value, set "literal_value" to True.
    Permitted values: Predefined strings:
    * ''MEAN' - Replace null value with the average of the values in the column.
    * 'MODE' - Replace null value with the mode of the values in the column.
    * 'MEDIAN' - Replace null value with the median of the values in the column.
    * 'MIN' - Replace null value with the minimum of the values in the column.
* 'MAX' - Replace null value with the maximum of the values in the column.
    Types: int, float, str, dict containing column names and value, list
#### Optional Arguments
columns
    Specifies the column names to perform the null value replacement. If "columns" is None, then
    all the columns having null value and data type similar to the data type of the value specified
    are considered.
    The default value is None.
    Types: str, tuple or list of str
literal_value
    Specifies whether the pre-defined strings passed to "value" should be treated as literal or not.
    The default value is False.
partition_column
    Specifies the column name to partition the data.
    The default value is None.
#### Example 1: Populate null value in column 'Jan' and 'Mar' with the value
#### specified as dictionary
```python
>>> load_example_data("dataframe", "sales")
>>> df = DataFrame("sales")
>>> df
accounts Feb Jan Mar Apr datetime
Blue Inc 90.0 50.0 95.0 101.0 04/01/2017
Alpha Co 210.0 200.0 215.0 250.0 04/01/2017
Jones LLC 200.0 150.0 140.0 180.0 04/01/2017
Yellow Inc 90.0 NaN NaN NaN 04/01/2017
Orange Inc 210.0 NaN NaN 250.0 04/01/2017
Red Inc 200.0 150.0 140.0 NaN 04/01/2017
>>> df.fillna({"Jan": 123, "Mar":234})
accounts Feb Jan Mar Apr datetime
0 Blue Inc 90.0 50 95 101.0 17/01/04
1 Alpha Co 210.0 200 215 250.0 17/01/04
2 Jones LLC 200.0 150 140 180.0 17/01/04
3 Yellow Inc 90.0 123 234 NaN 17/01/04
4 Orange Inc 210.0 123 234 250.0 17/01/04
5 Red Inc 200.0 150 140 NaN 17/01/04
```
#### Example 2: Populate the null value in 'Jan' column with minimum value in
#### that column
```python
>>> load_example_data("dataframe", "sales")
>>> df = DataFrame("sales")
>>> df
accounts Feb Jan Mar Apr datetime
Blue Inc 90.0 50.0 95.0 101.0 04/01/2017
Alpha Co 210.0 200.0 215.0 250.0 04/01/2017
Jones LLC 200.0 150.0 140.0 180.0 04/01/2017
Yellow Inc 90.0 NaN NaN NaN 04/01/2017
Orange Inc 210.0 NaN NaN 250.0 04/01/2017
Red Inc 200.0 150.0 140.0 NaN 04/01/2017
>>> df.fillna("Min", "Jan")
accounts Feb Jan Mar Apr datetime
0 Yellow Inc 90.0 50 NaN NaN 17/01/04
1 Jones LLC 200.0 150 140.0 180.0 17/01/04
2 Red Inc 200.0 150 140.0 NaN 17/01/04
3 Blue Inc 90.0 50 95.0 101.0 17/01/04
4 Alpha Co 210.0 200 215.0 250.0 17/01/04
5 Orange Inc 210.0 50 NaN 250.0 17/01/04
```
#### Example 3: Populate the null value in 'pclass' and 'fare' column with mean
#### value with partition column as 'sex'
```python
>>> load_example_data("teradataml", ["titanic"])
>>> df = DataFrame.from_table("titanic")
>>> df.fillna(value="mean", columns=["pclass", "fare"], partition_column="sex")
passenger survived pclass name sex age sibsp parch ticket fare cabin embarked
120 0 3 Andersson, Miss. Ellis Anna Maria female 2 4 2 347082 31.275 None S
200 0 2 Yrois, Miss. Henriette ("Mrs Harbeck") female 24 0 0 248747 13.0 None S
57 1 2 Rugg, Miss. Emily female 21 0 0 C.A. 31026 10.5 None S
505 1 1 Maioni, Miss. Roberta female 16 0 0 110152 86.5 B79 S
503 0 3 O'Sullivan, Miss. Bridget Mary female None 0 0 330909 7.6292 None Q
360 1 3 Mockler, Miss. Helen Mary "Ellie" female None 0 0 330980 7.8792 None Q
223 0 3 Green, Mr. George Henry male 51 0 0 21440 8.05 None S
345 0 2 Fox, Mr. Stanley Hubert male 36 0 0 229236 13.0 None S
406 0 2 Gale, Mr. Shadrach male 34 1 0 28664 21.0 None S
528 0 1 Farthing, Mr. John male None 0 0 PC 17483 221.7792 C95 S
```
### get() Method
Use the get() function to retrieve required columns from a teradataml DataFrame.
The function takes a key representing a column name as an argument and returns a new DataFrame with
the the appropriate columns. The key can be a single column name or a list of column names.
Note:
Multicolumn retrieval of the same column such as df.get(['col1', 'col1']) is not supported.
#### Examples Prerequisite
Assume a teradataml DataFrame is created based on the table "admissions_train".
```python
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming admitted
id
5        no  3.44    novice      novice        0
7       yes  2.33    novice      novice        1
22      yes  3.46    novice    beginner        0
17       no  3.83  advanced    advanced        1
13       no  4.00  advanced      novice        1
19      yes  1.98  advanced    advanced        0
36       no  3.00  advanced      novice        0
15      yes  4.00  advanced    advanced        1
34      yes  3.85  advanced    beginner        0
40      yes  3.95    novice    beginner        0
```
#### Example 1: Retrieve a single column
This example retrieves a single column "id" from the table "admissions_train". Column "id" is the
index column.
```python
>>>df.get("id")
Empty DataFrame
Columns: []
Index: [22, 34, 13, 19, 15, 38, 26, 5, 36, 17]
```
#### Example 2: Retrieve multiple columns using a list of columns names
Use a list of columns names for multicolumn retrieval.
```python
>>>df.get(["id", "masters", "gpa"])
masters   gpa
id
5        no  3.44
36       no  3.00
15      yes  4.00
17       no  3.83
13       no  4.00
40      yes  3.95
7       yes  2.33
22      yes  3.46
34      yes  3.85
19      yes  1.98
```
#### Example 3: Retrieve multiple columns using a list of list of columns names
Use a list of list of columns names for multicolumn retrieval.
```python
>>> df.get([['id', 'masters', 'gpa']])
masters   gpa
id
5        no  3.44
34      yes  3.85
13       no  4.00
40      yes  3.95
22      yes  3.46
19      yes  1.98
36       no  3.00
15      yes  4.00
7       yes  2.33
17       no  3.83
```
### get_values() Method
Use the get_values() function to retrieve all values (only) present in a teradataml DataFrame.
The values are retrieved as per a numpy.ndarray representation of the teradataml DataFrame. This format
is equivalent to the get_values() representation of a Pandas DataFrame.
An optional integer valued argument num_rows allows the user to specify the number of rows to retrieve
values for from the teradataml DataFrame.
Note:
* Row and column indexing starts from 0, so the first column = index 0, second column = index 1,
    and so on.
* When a Pandas DataFrame is saved to the database and then retrieved back as a teradataml
    DataFrame, the get_values() method on a Pandas DataFrame, and the corresponding
    teradataml DataFrames have the following type differences:
    * teradataml DataFrame get_values() retrieves 'bool' type Pandas DataFrame values ('True'
    or 'False') as BYTEINTS ('1' or '0');
    * teradataml DataFrame get_values() retrieves 'Timedelta' type Pandas DataFrame values
    as equivalent values in seconds.
#### Example Prerequisite
```python
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming admitted
id
5        no  3.44    novice      novice        0
7       yes  2.33    novice      novice        1
22      yes  3.46    novice    beginner        0
17       no  3.83  advanced    advanced        1
13       no  4.00  advanced      novice        1
19      yes  1.98  advanced    advanced        0
36       no  3.00  advanced      novice        0
15      yes  4.00  advanced    advanced        1
34      yes  3.85  advanced    beginner        0
40      yes  3.95    novice    beginner        0
```
#### Example 1: Retrieves values present in a teradataml DataFrame
```python
>>> vals = df.get_values()
>>> vals
array([['yes', 4.0, 'advanced', 'advanced', 1],
['yes', 3.45, 'advanced', 'advanced', 0],
['yes', 3.5, 'advanced', 'beginner', 1],
['yes', 4.0, 'novice', 'beginner', 0],
['no', 3.68, 'novice', 'beginner', 1],
['yes', 3.5, 'beginner', 'advanced', 1],
['yes', 3.79, 'advanced', 'novice', 0],
['no', 3.0, 'advanced', 'novice', 0],
['yes', 1.98, 'advanced', 'advanced', 0]], dtype=object)
```
#### Example 2: Retrieve values for a given number of rows from a
#### teradataml DataFrame
```python
>>> vals = df1.get_values(num_rows = 3)
>>> vals
array([['yes', 4.0, 'advanced', 'advanced', 1],
['yes', 3.45, 'advanced', 'advanced', 0],
['yes', 3.5, 'advanced', 'beginner', 1]], dtype=object)
```
#### Example 3: Access specific values from the entire set received
```python
# Retrieve all values from an entire row (for example, the first row):
>>> vals[0]
array(['yes', 4.0, 'advanced', 'advanced', 1], dtype=object)
# Specify a range to retrieve values from  a subset of rows (For example, first
3 rows):
>>> vals[0:3]
array([['yes', 4.0, 'advanced', 'advanced', 1],
['yes', 3.45, 'advanced', 'advanced', 0],
['yes', 3.5, 'advanced', 'beginner', 1]], dtype=object)
# Retrieve all values from an entire column (For example, the first column):
>>> vals[:, 0]
array(['yes', 'yes', 'yes', 'yes', 'yes', 'no', 'yes', 'yes', 'yes',
'yes', 'no', 'no', 'yes', 'yes', 'no', 'yes', 'no', 'yes', 'no',
'no', 'no', 'no', 'no', 'no', 'yes', 'yes', 'no', 'no', 'yes',
'yes', 'yes', 'no', 'no', 'yes', 'no', 'no', 'yes', 'yes',
'no','yes'], dtype=object)
# Retrieve a single value from a given row and column (For example, 3rd row,
and 2nd column):
>>> vals[2,1]
3.5
```
### head and tail
The examples in this section use the DataFrame "df" created using the following command.
#### Examples Prerequisite
Assume a teradataml DataFrame "df" is created from a table "admissions_train", using command:
```python
>>> df = DataFrame("admissions_train")
```
#### head() Method
Use the head() method to print the first n rows of a teradataml DataFrame.
The method takes the optional argument n number of rows to print. Default value is dependent on the
value of the teradataml DataFrame display option max_rows, which is by default 10.
Note:
The DataFrame is sorted on the index column or the first column if there is no index column. The
column type must support sorting.
Unsupported types include: 'BLOB', 'CLOB', 'ARRAY', 'VARRAY'.
#### Example 1: Print default number of rows
This example prints the default 10 rows of "admissions_train":
```python
>>> df.head()
masters   gpa     stats programming admitted
id
3       no  3.70    novice    beginner        1
5       no  3.44    novice      novice        0
6      yes  3.50  beginner    advanced        1
7      yes  2.33    novice      novice        1
9       no  3.82  advanced    advanced        1
10      no  3.71  advanced    advanced        1
8       no  3.60  beginner    advanced        1
4      yes  3.50  beginner      novice        1
2      yes  3.76  beginner    beginner        0
1      yes  3.95  beginner    beginner        0
```
#### Example 2: Print n rows
The following example prints 5 rows of "admissions_train":
```python
>>> df.head(5)
masters   gpa     stats programming admitted
id
3       no  3.70    novice    beginner        1
5       no  3.44    novice      novice        0
4      yes  3.50  beginner      novice        1
2      yes  3.76  beginner    beginner        0
1      yes  3.95  beginner    beginner        0
```
The following example prints 15 rows of "admissions_train":
```python
>>> df.head(15)
masters   gpa     stats programming admitted
id
3       no  3.70    novice    beginner        1
5       no  3.44    novice      novice        0
6      yes  3.50  beginner    advanced        1
7      yes  2.33    novice      novice        1
9       no  3.82  advanced    advanced        1
10      no  3.71  advanced    advanced        1
11      no  3.13  advanced    advanced        1
12      no  3.65    novice      novice        1
13      no  4.00  advanced      novice        1
14     yes  3.45  advanced    advanced        0
15     yes  4.00  advanced    advanced        1
8       no  3.60  beginner    advanced        1
4      yes  3.50  beginner      novice        1
2      yes  3.76  beginner    beginner        0
1      yes  3.95  beginner    beginner        0
```
#### tail() Method
Use the tail() method to print the last n rows of a DataFrame.
The method takes the optional argument n number of rows to print. Default value is dependent on the
value of the teradataml DataFrame display option max_rows, which is by default 10.
Note:
The DataFrame is sorted on the index column or the first column if there is no index column. The
column type must support sorting.
Unsupported types include: 'BLOB', 'CLOB', 'ARRAY', 'VARRAY'.
#### Example 1: Print default number of rows
The following example prints the default last 10 rows of "admissions_train" sorted by id.
```python
>>>df.tail()
masters   gpa     stats programming admitted
id
38     yes  2.65  advanced    beginner        1
36      no  3.00  advanced      novice        0
35      no  3.68    novice    beginner        1
34     yes  3.85  advanced    beginner        0
32     yes  3.46  advanced    beginner        0
31     yes  3.50  advanced    beginner        1
33      no  3.55    novice      novice        1
37      no  3.52    novice      novice        1
39     yes  3.75  advanced    beginner        0
40     yes  3.95    novice    beginner        0
```
#### Example 2: Print n rows
The following example prints the last 3 rows of "admissions_train":
```python
>>>df.tail(3)
masters   gpa     stats programming admitted
id
38     yes  2.65  advanced    beginner        1
39     yes  3.75  advanced    beginner        0
40     yes  3.95    novice    beginner        0
```
The following example prints the last 15 rows of "admissions_train":
```python
>>>df.tail(15)
masters   gpa     stats programming admitted
id
38     yes  2.65  advanced    beginner        1
36      no  3.00  advanced      novice        0
35      no  3.68    novice    beginner        1
34     yes  3.85  advanced    beginner        0
32     yes  3.46  advanced    beginner        0
31     yes  3.50  advanced    beginner        1
30     yes  3.79  advanced      novice        0
29     yes  4.00    novice    beginner        0
28      no  3.93  advanced    advanced        1
27     yes  3.96  advanced    advanced        0
26     yes  3.57  advanced    advanced        1
33      no  3.55    novice      novice        1
37      no  3.52    novice      novice        1
39     yes  3.75  advanced    beginner        0
40     yes  3.95    novice    beginner        0
```
### itertuples() Method
Use the itertuples() method to iterate over teradataml DataFrame rows as namedtuples.
Optional Arguments:
* name: Specifies the name of the returned namedtuples.
  When set to None, the method returns the row in a list instead of namedtuple.
  The default value is 'Row'.
* num_rows: Specifies the number of rows to retrieve from the DataFrame.
  When set to None, the method retrieves all the records. This is the default value.
This method returns iterator, an object to iterate over namedtuples for each row in the DataFrame.
#### Example 1: Retrieve all records from a teradataml DataFrame
```python
>>> df = DataFrame("titanic")
>>> rows = df.itertuples()
>>> next(rows)
Row(passenger=78, survived=0, pclass=3, name='Moutal Mr. Rahamin Haim',
gender='male', age=None, sibsp=0, parch=0, ticket='374746', fare=8.05,
cabin=None, embarked='S')
>>> next(rows)
Row(passenger=93, survived=0, pclass=1, name='Chaffee Mr. Herbert Fuller',
gender='male', age=46, sibsp=1, parch=0, ticket='W.E.P. 5734', fare=61.175,
cabin='E31', embarked='S')
>>> next(rows)
Row(passenger=5, survived=0, pclass=3, name='Allen Mr. William Henry',
gender='male', age=35, sibsp=0, parch=0, ticket='373450', fare=8.05,
cabin=None, embarked='S')
```
#### Example 2: Retrieve the first 20 records from a teradataml DataFrame in a list,
#### when DataFrame is ordered
This example retrieves the first 20 records from a teradataml DataFrame in a list, when the DataFrame is
ordered by column "passenger".
```python
>>> df = DataFrame("titanic")
>>> rows = df.sort("passenger").itertuples(name=None, num_rows=20)
>>> next(rows)
[1, 0, 3, 'Braund, Mr. Owen Harris', 'male', 22, 1, 0, 'A/5 21171', 7.25,
None, 'S']
>>> next(rows)
[2, 1, 1, 'Cumings, Mrs. John Bradley (Florence Briggs Thayer)', 'female', 38,
1, 0, 'PC 17599', 71.2833, 'C85', 'C']
>>> next(rows)
[3, 1, 3, 'Heikkinen, Miss. Laina', 'female', 26, 0, 0, 'STON/O2. 3101282',
7.925, None, 'S']
```
### join() Method
Use the join() function to join two teradataml DataFrame objects together. The operation is performed by
carrying out a database-style join using teradataml specified as keys.
Required Arguments:
* other: Specifies right teradataml DataFrame on which join is to be performed.
Optional Arguments:
* how: Specifies the type of the join performed.
  Permitted values are the supported join operations:
  * 'left': Returns all matching rows, plus non-matching rows from the left teradataml DataFrame;
    This is the default value for the argument.
  * 'inner': Returns only matching rows, non-matching rows are eliminated;
  * 'right': Returns all matching rows, plus non-matching rows from the right teradataml DataFrame;
  * 'full': Returns all rows from both teradataml DataFrames, including non matching rows;
  * 'cross': Returns all rows from both tables where each row from the first table is joined with each
    row from the second table. The result of the join is a Cartesian cross product.
  Default value is 'left'.
* on: Specifies list of conditions that indicate the columns to be join keys. Supported join operators are
  =, ==, <, <=, >, >=, <> and !=.
  Note:
    = and <> operators are not supported when using DataFrame columns as operands.
Note:
    This argument is optional when how is set to 'cross'; otherwise, it is required.
    If it is specified when how is set to 'cross', it is ignored.
  See the following section for valid forms of on conditions.
* lsuffix and rsuffix: Specify the suffix to be added to the left and right table columns, respectively.
* lprefix and rprefix: Specify the prefix to be added to the left and right table columns, respectively.
#### Valid forms of on conditions
The on conditions can take the following forms:
* String comparisons, in the form of "col1 <= col2", where col1 is the column of left dataframe df1 and
  col2 is the column of right dataframe df2. For example:
  * ["a","b"] indicates df1.a = df2.a and df1.b = df2.b
  * ["a = b", "c == d"] indicates df1.a = df2.b and df1.c = df2.d
  * ["a <= b", "c > d"] indicates df1.a <= df2.b and df1.c > df2.d
  * ["a < b", "c >= d"] indicates df1.a < df2.b and df1.c >= df2.d
  * ["a <> b"] indicates df1.a != df2.b. Same is the case for ["a != b"]
* Column comparisons, in the form of df1.col1 <= df1.col2, where col1 is the column of left dataframe
  df1 and col2 is the column of right dataframe df2. For example:
  * [df1.a == df2.a, df1.b == df2.b] indicates df1.a = df2.a and df1.b = df2.b
  * [df1.a == df2.b, df1.c == df2.d] indicates df1.a = df2.b and df1.c = df2.d
  * [df1.a <= df2.b and df1.c > df2.d] indicates df1.a <= df2.b and df1.c > df2.d
  * [df1.a < df2.b and df1.c >= df2.d] indicates df1.a < df2.b and df1.c >= df2.d
  * df1.a != df2.b indicates df1.a != df2.b
* The combination of both string comparisons and comparisons as column expressions. For example:
  * ["a", df1.b == df2.b] indicates df1.a = df2.a and df1.b = df2.b
  * [df1.a <= df2.b, "c > d"] indicates df1.a <= df2.b and df1.c > df2.d
* ColumnExpressions containing FunctionExpressions which represent SQL functions invoked on
  DataFrame Columns. For example:
  * (df1.a.round(1) - df2.a.round(1)).mod(2.5) > 2
  * df1.a.floor() - df2.b.floor() > 2
Note:
* When multiple join conditions are given, they are joined using AND boolean operator. Other
    boolean operators are not supported.
* Nesting of join on conditions in column expressions using & and | is not supported. For example,
    on = [(df1.a == df1.b) & (df1.c == df1.d)] is unsupported nested join on condition. You can use
    [df1.a == df1.b, df1.c == df1.d] instead.
* For a cross join operation, the 'on' argument is ignored.
#### Example Prerequisite
Set up teradataml DataFrames for join operations.
```python
>>> from datetime import datetime, timedelta
>>> dob = datetime.strptime('31101991', '%d%m%Y').date()
>>> pdf1 = pd.DataFrame(data={'col1': [1, 2,3],
'col2': ['teradata','analytics','platform'],
'col3': [1.3, 2.3, 3.3],
'col5': ['a','b','c'],
'col 6': [dob, dob + timedelta(1), dob
+ timedelta(2)],
"'col8'":[3,4,5]})
>>> pdf2 = pd.DataFrame(data={'col1': [1, 2, 3],
'col4': ['teradata', 'analytics', 'are you'],
'col3': [1.3, 2.3, 4.3],
'col7':['a','b','d'],
'col 6': [dob, dob + timedelta(1), dob
+ timedelta(3)],
"'col8'": [3, 4, 5]})
>>> copy_to_sql(pdf1, "t1", primary_index="col1")
>>> copy_to_sql(pdf2, "t2", primary_index="col1")
>>> df1 = DataFrame("t1")
>>> df2 = DataFrame("t2")
```
Display the DataFrames.
```python
>>> df1
'col8'       col 6       col2  col3 col5
col1
2         4  1991-11-01  analytics   2.3    b
1         3  1991-10-31   teradata   1.3    a
3         5  1991-11-02   platform   3.3    c
>>> df2
'col8'       col 6  col3       col4 col7
col1
2         4  1991-11-01   2.3  analytics    b
1         3  1991-10-31   1.3   teradata    a
3         5  1991-11-03   4.3    are you    d
```
#### Example 1: Use 'on' and 'how' parameters to join the DataFrames
Specify a join condition using the 'on' parameter and the type of join using the 'how' parameter.
Note:
If teradataml DataFrames have columns with the same names, prefixes are required.
# Using string expressions for on conditions.
>>> df3 = df1.join(other = df2, on = ["col3","col5=col7"], how = "left", lprefix = "t1", rprefix = "t2")
>>> df3
t2_'col8' t1_col1 col5    t2_col 6       col2 t1_'col8'  col7  t1_col3  t2_col3    t1_col 6 t2_col1      col4
0      None       3    c        None   platform         5  None      3.3      NaN  1991-11-02    None       None
1         4       2    b  1991-11-01  analytics         4     b      2.3      2.3  1991-11-01       2  analytics
2         3       1    a  1991-10-31   teradata         3     a      1.3      1.3  1991-10-31       1   teradata
# Using dataframe column expressions instead of string expressions for on conditions.
>>> df3 = df1.join(other = df2, on = [df1.col3 == df2.col3, df1.col5 == df2.col7], how = "left", lprefix = "t1",
rprefix = "t2")
>>> df3
t2_'col8' t1_col1 col5    t2_col 6       col2 t1_'col8'  col7  t1_col3  t2_col3    t1_col 6 t2_col1      col4
0      None       3    c        None   platform         5  None      3.3      NaN  1991-11-02    None       None
1         4       2    b  1991-11-01  analytics         4     b      2.3      2.3  1991-11-01       2  analytics
2         3       1    a  1991-10-31   teradata         3     a      1.3      1.3  1991-10-31       1   teradata
#### Example 2: Specify a required type of join using the 'how' parameter
# Using string expressions for on conditions.
>>> df3 = df1.join(other = df2, on = ["col3","'col8'"], how = "right", lprefix = "t1", rprefix = "t2")
>>> df3
t2_'col8' t1_col1  col5    t2_col 6       col2 t1_'col8' col7  t1_col3  t2_col3    t1_col 6 t2_col1       col4
0         5    None  None  1991-11-03       None      None    d      NaN      4.3        None       3    are you
1         4       2     b  1991-11-01  analytics         4    b      2.3      2.3  1991-11-01       2  analytics
2         3       1     a  1991-10-31   teradata         3    a      1.3      1.3  1991-10-31       1   teradata
# Using dataframe column expressions instead of string expressions for some of on conditions.
>>> df3 = df1.join(other = df2, on = [df1.col3 == df2.col3,"'col8'"], how = "right", lprefix = "t1", rprefix = "t2")
>>> df3
t2_'col8' t1_col1  col5    t2_col 6       col2 t1_'col8' col7  t1_col3  t2_col3    t1_col 6 t2_col1       col4
0         5    None  None  1991-11-03       None      None    d      NaN      4.3        None       3    are you
1         4       2     b  1991-11-01  analytics         4    b      2.3      2.3  1991-11-01       2  analytics
2         3       1     a  1991-10-31   teradata         3    a      1.3      1.3  1991-10-31       1   teradata
#### Example 3: Use the default value for 'how' parameter
When the 'how' parameter is not provided, a left join is used by default.
```python
>>> df3 = df1.join(other = df2, on = df1.col3 == df2.col3, lprefix = "t1",
rprefix = "t2")
```
>>> df3
t2_'col8' t1_col1 col5    t2_col 6       col2 t1_'col8'  col7  t1_col3  t2_col3    t1_col 6 t2_col1       col4
0      None       3    c        None   platform         5  None      3.3      NaN  1991-11-02    None       None
1         4       2    b  1991-11-01  analytics         4     b      2.3      2.3  1991-11-01       2  analytics
2         3       1    a  1991-10-31   teradata         3     a      1.3      1.3  1991-10-31       1   teradata
#### Example 4: How cross join works
This example cross join "admissions_train" with "admissions_train".
```python
>>> df1 = DataFrame("admissions_train").head(3).sort("id")
>>> print(df1)
masters   gpa     stats programming admitted
id
1      yes  3.95  Beginner    Beginner        0
2      yes  3.76  Beginner    Beginner        0
3       no  3.70    Novice    Beginner        1
>>> df2 = DataFrame("admissions_train").head(3).sort("id")
>>> print(df2)
masters   gpa     stats programming admitted
id
1      yes  3.95  Beginner    Beginner        0
2      yes  3.76  Beginner    Beginner        0
3       no  3.70    Novice    Beginner        1
```
>>> df3 = df1.join(other=df2, how="cross", lprefix="l", rprefix="r")
>>> df3.set_index("l_id").sort("l_id")
r_id l_masters r_masters  l_gpa  r_gpa   l_stats   r_stats l_programming r_programming l_admitted r_admitted
l_id
1       3       yes        no   3.95   3.70  Beginner    Novice      Beginner      Beginner          0          1
1       1       yes       yes   3.95   3.95  Beginner  Beginner      Beginner      Beginner          0          0
1       2       yes       yes   3.95   3.76  Beginner  Beginner      Beginner      Beginner          0          0
2       1       yes       yes   3.76   3.95  Beginner  Beginner      Beginner      Beginner          0          0
2       2       yes       yes   3.76   3.76  Beginner  Beginner      Beginner      Beginner          0          0
2       3       yes        no   3.76   3.70  Beginner    Novice      Beginner      Beginner          0          1
3       1        no       yes   3.70   3.95    Novice  Beginner      Beginner      Beginner          1          0
3       2        no       yes   3.70   3.76    Novice  Beginner      Beginner      Beginner          1          0
3       3        no        no   3.70   3.70    Novice    Novice      Beginner      Beginner          1          1
See merge() Method for examples using suffix arguments lsuffix and rsuffix.
#### Example 5: Using not equal operation in ColumnExpression condition
```python
>>> df1.join(other = df2, on = ["col5==col7",df1.col2 != df2.col4], how =
"full", lprefix = "t1", rprefix = "t2")
t1_col1     t2_col1       col2    t1_col3    t2_col3    col5
col4     col7
0         1        None   teradata        1.3       None       a
None     None
1         2        None  analytics        2.3       None       b
None     None
2      None           2       None       None        2.3    None
analytics       b
3      None           1       None       None        1.3    None
teradata       a
4         3        None   platform        3.3        None      c
None    None
5      None           3       None       None         4.3   None     are
you       d
```
#### Example 6: Using only one string expression with < > operation
```python
>>> df1.join(other = df2, on = "col2<>col4", how = "left", lprefix = "t1",
rprefix = "t2")
t1_col1    t2_col1          col2    t1_col3     t2_col3    col5
col4      col7
0          2          3     analytics        2.3         4.3       b      are
you         d
1          2          1     analytics        2.3         1.3       b
teradata         a
2          3          2      platform        3.3         2.3       c
analytics         b
3          1          2      teradata        1.3         2.3       a
analytics         b
4          3          1      platform        3.3         1.3       c
teradata         a
5          1          3      teradata        1.3         4.3       a      are
you         d
6          3          3      platform        3.3         4.3       c      are
you         d
```
#### Example 7: Using only one ColumnExpression in "on" argument conditions
```python
>>> df1.join(other = df2, on = df1.col5 != df2.col7, how = "full", lprefix =
"t1", rprefix = "t2")
t1_col1   t2_col1        col2    t1_col3   t2_col3    col5
col4    col7
0         1         3    teradata        1.3       4.3       a       are
you       d
1         3         1    platform        3.3       1.3       c
teradata       a
2         1         2    teradata        1.3       2.3       a
analytics       b
3         3         2    platform        3.3       2.3       c
analytics       b
4         2         1   analytics        2.3       1.3       b
teradata       a
5         3         3    platform        3.3       4.3       c       are
you       d
6         2         3   analytics        2.3       4.3       b       are
you       d
```
#### Example 8: cross join "admissions_train" with "admissions_train"
```python
>>> df1 = DataFrame("admissions_train").head(3).sort("id")
>>> df2 = DataFrame("admissions_train").head(3).sort("id")
>>> df3 = df1.join(other=df2, how="cross", lprefix="l", rprefix="r")
>>> df3.set_index("l_id").sort("l_id")
l_programming     r_stats    r_id     r_gpa    r_programming   l_gpa
l_stats   r_admitted   l_admitted   l_masters   r_masters
l_id
1           Beginner      Novice       3      3.70         Beginner    3.95
Beginner            1            0         yes          no
1           Beginner    Beginner       1      3.95         Beginner    3.95
Beginner            0            0         yes         yes
1           Beginner    Beginner       2      3.76         Beginner    3.95
Beginner            0            0         yes         yes
2           Beginner    Beginner       1      3.95         Beginner    3.76
Beginner            0            0         yes         yes
2           Beginner    Beginner       2      3.76         Beginner    3.76
Beginner            0            0         yes         yes
2           Beginner      Novice       3      3.70         Beginner    3.76
Beginner            1            0         yes          no
3           Beginner    Beginner       1      3.95         Beginner
3.70      Novice            0            1          no         yes
3           Beginner    Beginner       2      3.76         Beginner
3.70      Novice            0            1          no         yes
3           Beginner      Novice       3      3.70         Beginner
3.70      Novice            1            1          no          no
```
#### Example 9: Perform self join using aliased DataFrame
```python
# Create an aliased DataFrame.
>>> lhs = DataFrame("admissions_train").head(3).sort("id")
>>> rhs = lhs.alias("rhs")
# Use aliased DataFrame for self join.
>>> joined_df = lhs.join(other=rhs, how="cross", lprefix="l", rprefix="r")
>>> joined_df
l_id   r_id    l_masters   r_masters     l_gpa   r_gpa     l_stats
r_stats  l_programming    r_programming   l_admitted r_admitted
0       1      3          yes          no      3.95    3.70    Beginner
Novice       Beginner         Beginner            0          1
1       2      2          yes         yes      3.76    3.76    Beginner
Beginner       Beginner         Beginner            0          0
2       2      3          yes          no      3.76    3.70    Beginner
Novice       Beginner         Beginner            0          1
3       3      1           no         yes      3.70    3.95      Novice
Beginner       Beginner         Beginner            1          0
4       3      3           no          no      3.70    3.70      Novice
Novice       Beginner         Beginner            1          1
5       3      2           no         yes      3.70    3.76      Novice
Beginner       Beginner         Beginner            1          0
6       2      1          yes         yes      3.76    3.95    Beginner
Beginner       Beginner         Beginner            0          0
7       1      2          yes         yes      3.95    3.76    Beginner
Beginner       Beginner         Beginner            0          0
8       1      1          yes         yes      3.95    3.95    Beginner
Beginner       Beginner         Beginner            0          0
```
#### Example 10: Perform join with compound 'on' condition having more than one
#### binary operator
```python
>>> rhs_2 = lhs.assign(double_gpa=lhs.gpa * 2)
>>> joined_df_2 = lhs.join(rhs_2, on=rhs_2.double_gpa == lhs.gpa * 2,
how="left", lprefix="l", rprefix="r")
>>> joined_df_2
l_id    r_id  l_masters   r_masters    l_gpa    r_gpa      l_stats
r_stats   l_programming    r_programming   l_admitted  r_admitted   double_gpa
0      3       3         no          no     3.70     3.70       Novice
Novice        Beginner         Beginner            1           1         7.40
1      2       2        yes         yes     3.76     3.76     Beginner
Beginner        Beginner         Beginner            0           0         7.52
2      1       1        yes         yes     3.95     3.95     Beginner
Beginner        Beginner         Beginner            0           0         7.90
```
#### Example 11: Perform join on DataFrame with 'on' condition
#### having FunctionExpression
```python
>>> df = DataFrame("admissions_train")
>>> df2 = df.alias("rhs_df")
>>> joined_df_3 = df.join(df2, on=(df.gpa.round(1) - df2.gpa.round(1)).mod(2.5)
> 2, how="inner", lprefix="l")
>>> joined_df_3.sort(["id", "l_id"])
l_id id l_masters masters   l_gpa     gpa     l_stats        stats
l_programming    programming    l_admitted   admitted
0     1 24       yes     no     3.95    1.87    Beginner     Advanced
Beginner         Novice             0          1
1    13 24        no     no      4.0    1.87    Advanced     Advanced
Novice         Novice             1          1
2    15 24       yes     no      4.0    1.87    Advanced     Advanced
Advanced         Novice             1          1
3    25 24        no     no     3.96    1.87    Advanced     Advanced
Advanced         Novice             1          1
4    27 24       yes     no     3.96    1.87    Advanced     Advanced
Advanced         Novice             0          1
5    29 24       yes     no      4.0    1.87      Novice     Advanced
Beginner         Novice             0          1
6    40 24       yes     no     3.95    1.87      Novice     Advanced
Beginner         Novice             0          1
```
### loc and iloc
#### iloc[] Operator
Use the iloc[] operator to access a group of rows and columns by integer values.
The operator takes a single integer, a list of integers, and a slice with integers as valid inputs. It also takes
a list of booleans for column access. The list must include a boolean value for each column.
Note:
For integer indexing on row access, the integer index values are applied to a sorted teradataml
DataFrame on the index column or the first column if there is no index column.
#### Examples Prerequisite
Assume a teradataml DataFrame "df" is created from a Vantage table "sales" and sorted
using commands:
```python
>>> df = DataFrame('sales')
>>> df.sort("accounts")
Feb   Jan   Mar   Apr    datetime
accounts
Alpha Co    210.0   200   215   250  2017-04-01
Blue Inc     90.0    50    95   101  2017-04-01
Jones LLC   200.0   150   140   180  2017-04-01
Orange Inc  210.0  None  None   250  2017-04-01
Red Inc     200.0   150   140  None  2018-10-15
Yellow Inc   90.0  None  None  None  2017-04-01
```
#### Example 1 : Retrieve a row using a single integer value
The following example retrieves a row using a single integer value.
```python
>>> df.iloc[1]
Feb Jan Mar  Apr    datetime
accounts
Blue Inc  90.0  50  95  101  2017-04-01
```
#### Example 2: Retrieve using a list of integers
Note:
Using "[[]]" to include a list of integers.
```python
>>> df.iloc[[1, 2]]
Feb  Jan  Mar  Apr    datetime
accounts
Blue Inc    90.0   50   95  101  2017-04-01
Jones LLC  200.0  150  140  180  2017-04-01
```
#### Example 3: Use a single integer for both row and column
```python
>>> df.iloc[5, 0]
Empty DataFrame
Columns: []
Index: [Yellow Inc]
>>> df.iloc[(5, 1)]
Feb
0  90.0
```
#### Example 4: Use a slice for row and a single integer for column access
Note:
The stop for the slice is excluded.
```python
>>> df.iloc[2:5, 2]
Jan
0  None
1   150
2   150
```
#### Example 5: Use a slice for row and column access
Note:
The stop for the slice is excluded.
```python
>>> df.iloc[2:5, 0:5]
Mar   Jan    Feb   Apr
accounts
Orange Inc  None  None  210.0   250
Red Inc      140   150  200.0  None
Jones LLC    140   150  200.0   180
```
#### Example 6: Use an empty slice for row and column access
```python
>>> df.iloc[:, :]
Feb   Jan   Mar    datetime   Apr
accounts
Jones LLC   200.0   150   140  2017-04-01   180
Blue Inc     90.0    50    95  2017-04-01   101
Yellow Inc   90.0  None  None  2017-04-01  None
Orange Inc  210.0  None  None  2017-04-01   250
Alpha Co    210.0   200   215  2017-04-01   250
Red Inc     200.0   150   140  2017-04-01  None
```
#### Example 7: Use a list of integers and boolean array for column access
```python
>>> df.iloc[[0, 2, 3, 4], [True, True, False, False, True, True]]
datetime   Apr    Feb
accounts
Jones LLC   2017-04-01   180  200.0
Orange Inc  2017-04-01   250  210.0
Alpha Co    2017-04-01   250  210.0
Red Inc     2017-04-01  None  200.0
```
#### loc[] Operator
Use the loc[] operator to access a group of rows and columns by labels.
The operator takes a single label, a list of column or index labels, and a slice with labels as valid inputs. It
also takes a conditional expression for row access and a list of booleans for column access. The list must
include a boolean value for each column.
#### Examples Prerequisite
Assume a teradataml DataFrame "df" is created from a Vantage table "sales", using command:
```python
>>> df = DataFrame('sales')
>>> df
Feb   Jan   Mar   Apr    datetime
accounts
Blue Inc     90.0    50    95   101  2017-04-01
Alpha Co    210.0   200   215   250  2017-04-01
Jones LLC   200.0   150   140   180  2017-04-01
Yellow Inc   90.0  None  None  None  2017-04-01
Orange Inc  210.0  None  None   250  2017-04-01
Red Inc     200.0   150   140  None  2017-04-01
```
#### Example 1: Retrieve a row using a single index label
This example retrieves a row using a single index label "Blue Inc":
```python
>>> df.loc['Blue Inc']
Feb Jan Mar  Apr    datetime
accounts
Blue Inc  90.0  50  95  101  2017-04-01
```
#### Example 2: Retrieve multiple rows using a list of labels
This example uses a list of labels to retrieve the rows for "Blue Inc" and "Jones LLC":
```python
>>> df.loc[['Blue Inc', 'Jones LLC']]
Feb  Jan  Mar  Apr    datetime
accounts
Blue Inc    90.0   50   95  101  2017-04-01
Jones LLC  200.0  150  140  180  2017-04-01
```
#### Example 3: Retrieve using a single index label and a single index
#### column label
This example uses a single index label and a single column label (index column label) for row and
column access:
```python
>>> df.loc['Yellow Inc', 'accounts']
Empty DataFrame
Columns: []
Index: [Yellow Inc]
```
#### Example 4: Retrieve using a single index label and a single non-index
#### column label
This example uses a single index label "Yellow Inc" and a single column label "Feb" (non-index column
label) for row and column access:
```python
>>> df.loc['Yellow Inc', 'Feb']
Feb
0  90.0
```
#### Example 5: Retrieve using a slice with labels for row access and single label
#### for column access
This example uses a slice with labels for row access and single label for column access:
```python
>>> df.loc['Jones LLC':'Red Inc', 'accounts']
Empty DataFrame
Columns: []
Index: [Orange Inc, Jones LLC, Red Inc]
```
Note:
Both the start and stop of the slice are included.
#### Example 6: Retrieve using a slice with labels for row access and for
#### column access
This example uses a slice with labels for row access and for column access:
```python
>>> df.loc['Jones LLC':'Red Inc', 'accounts':'Apr']
Mar   Jan    Feb   Apr
accounts
Orange Inc  None  None  210.0   250
Red Inc      140   150  200.0  None
Jones LLC    140   150  200.0   180
```
#### Example 7: Retrieve using an empty slice for row access and for
#### column access
This example uses an empty slice for row access and for column access:
```python
>>> df.loc[:, :]
Feb   Jan   Mar    datetime   Apr
accounts
Jones LLC   200.0   150   140  2017-04-01   180
Blue Inc     90.0    50    95  2017-04-01   101
Yellow Inc   90.0  None  None  2017-04-01  None
Orange Inc  210.0  None  None  2017-04-01   250
Alpha Co    210.0   200   215  2017-04-01   250
Red Inc     200.0   150   140  2017-04-01  None
```
#### Example 8: Retrieve rows using a conditional expression
This example uses a conditional expression to retrieve rows where the value for "Feb" is greater than 90:
```python
>>> df.loc[df['Feb'] > 90]
Feb   Jan   Mar   Apr    datetime
accounts
Jones LLC   200.0   150   140   180  2017-04-01
Red Inc     200.0   150   140  None  2017-04-01
Alpha Co    210.0   200   215   250  2017-04-01
Orange Inc  210.0  None  None   250  2017-04-01
```
#### Example 9: Retrieve using a conditional expression for row access with
#### multiple column labels for column access
This examples uses a conditional expression for row access with multiple column labels for
column access:
```python
>>> df.loc[df['accounts'] == 'Jones LLC', ['accounts', 'Jan', 'Feb']]
Jan    Feb
accounts
Jones LLC  150  200.0
```
#### Example 10: Retrieve using a conditional expression for row access and
#### boolean array for column access
This example uses a conditional expression for row access and boolean array for column access:
```python
>>> df.loc[df['Feb'] > 90, [True, True, False, False, True, True]]
datetime   Apr    Feb
accounts
Jones LLC   2017-04-01   180  200.0
Orange Inc  2017-04-01   250  210.0
Alpha Co    2017-04-01   250  210.0
Red Inc     2017-04-01  None  200.0
```
### merge() Method
Use the merge() function to merge two teradataml DataFrame objects together. The merge is done by
performing a database-style join operation by columns or indexes.
Supported merge operations are:
* cross: Returns all rows from both tables where each row from the teradataml DataFrame is joined with
  each row of "right" teradataml DataFrame.
* inner: Returns only matching rows, non-matching rows are eliminated;
* left: Returns all matching rows, plus non-matching rows from the left teradataml DataFrame;
* right: Returns all matching rows, plus non-matching rows from the right teradataml DataFrame;
* full: Returns all rows from both teradataml DataFrames, including non matching rows.
Required Arguments:
* right: Specifies right teradataml DataFrame on which merge is to be performed.
Optional Arguments:
* on: Specifies list of conditions that indicate the columns used for the merge.
  When no argument is provided for this condition, the merge is performed using the indexes of the
  teradataml DataFrames. Both teradataml DataFrames are required to have index labels to perform a
  merge operation when no argument is provided for this condition. When either teradataml DataFrame
  does not have a valid index label in this case, an exception is thrown.
  See the following section for valid forms of on conditions.
* how: Specifies the type of the merge to perform.
  Supports inner, left, right, full and cross merge operations.
  Note:
    When how is 'cross', the arguments on, left_on, right_on and use_index are ignored.
  Default value is 'inner'.
* left_on: Specifies column to merge on, in the left teradataml DataFrame.
  When both the on and left_on arguments are unspecified, the index columns of the teradataml
  DataFrames are used to perform the merge operation.
  Default value is None.
* right_on: Specifies column to merge on, in the right teradataml DataFrame.
  When both the on and right_on arguments are unspecified, the index columns of the teradataml
  DataFrames are used to perform the merge operation.
  Default value is None.
* use_index: Specifies whether or not to use the index from the teradataml DataFrames as merge keys.
  When set to 'False', and the on, left_on and right_on arguments are all unspecified, the index columns
  of the teradataml DataFrames are used to perform the merge operation.
  Default value is 'False'.
* lsuffix: Specify the suffix to be added to the left and right table columns.
  Default value is None.
  Note:
    A suffix is required if teradataml DataFrames being merged have columns with the same name.
* rsuffix: Specify the suffix to be added to the left and right table columns.
  Default value is None.
Note:
    A suffix is required if teradataml DataFrames being merged have columns with the same name.
#### Valid forms of on conditions
The on conditions can take the following forms:
* String comparisons, in the form of col1 <= col2, where col1 is the column of left DataFrame df1 and
  col2 is the column of right DataFrame df2. For example:
  1. ["a","b"]  indicates df1.a = df2.a and df1.b = df2.b.
  2. ["a = b", "c == d"]  indicates df1.a = df2.b and df1.c = df2.d.
  3. ["a <= b", "c > d"]  indicates df1.a <= df2.b and df1.c > df2.d.
  4. ["a < b", "c >= d"]  indicates df1.a < df2.b and df1.c >= df2.d.
  5. ["a <> b"]  indicates df1.a != df2.b. Same is the case for ["a != b"].
* Column comparisons, in the form of df1.col1 <= df1.col2, where col1 is the column of left
  DataFrame df1 and col2 is the column of right DataFrame df2. For example:
  1. [df1.a == df2.a, df1.b == df2.b]  indicates df1.a = df2.a and df1.b = df2.b.
  2. [df1.a == df2.b, df1.c == df2.d]  indicates df1.a = df2.b and df1.c = df2.d.
  3. [df1.a <= df2.b and df1.c > df2.d]  indicates df1.a <= df2.b and df1.c > df2.d.
  4. [df1.a < df2.b and df1.c >= df2.d]  indicates df1.a < df2.b and df1.c >= df2.d.
  5. df1.a != df2.b  indicates df1.a != df2.b.
* The combination of both string comparisons and comparisons as column expressions. For example:
  1. ["a", df1.b == df2.b]  indicates df1.a = df2.a and df1.b = df2.b.
  2. [df1.a <= df2.b, "c > d"]  indicates df1.a <= df2.b and df1.c > df2.d.
Refer to teradataml DataFrame Column for more information about ColumnExpressions in teradataml.
Note:
* When multiple merge conditions are given, they are joined using AND boolean operator. Other
    boolean operators are not supported.
* Nesting of merge on conditions in column expressions using & and | is not supported.
    For example: on = [(df1.a == df1.b) & (df1.c == df1.d)] is unsupported. You can use
```python
[df1.a == df1.b, df1.c == df1.d] instead.
```
* For a cross join operation, the on, left_on, right_on and use_index arguments are ignored.
#### Example Prerequisite
* Set up teradataml DataFrames for merge.
```python
>>> from datetime import datetime, timedelta
>>> dob = datetime.strptime('31101991', '%d%m%Y').date()
>>> df1 = pd.DataFrame(data={'col1': [1, 2,3],
'col2': ['teradata','analytics','platform'],
'col3': [1.3, 2.3, 3.3],
'col5': ['a','b','c'],
'col 6': [dob, dob + timedelta(1), dob
+ timedelta(2)],
"'col8'":[3,4,5]})
>>> df2 = pd.DataFrame(data={'col1': [1, 2, 3],
'col4': ['teradata', 'analytics', 'are you'],
'col3': [1.3, 2.3, 4.3],
'col7':['a','b','d'],
'col 6': [dob, dob + timedelta(1), dob
+ timedelta(3)],
"'col8'": [3, 4, 5]})
```
* Persist the Pandas DataFrames in Vantage.
```python
>>> copy_to_sql(df1, "t1", primary_index="col1")
>>> copy_to_sql(df2, "t2", primary_index="col1")
>>> df1 = DataFrame("table1")
>>> df2 = DataFrame("table2")
```
* Display the DataFrames.
```python
>>> df1
'col8'       col 6       col2  col3 col5
col1
2         4  1991-11-01  analytics   2.3    b
1         3  1991-10-31   teradata   1.3    a
3         5  1991-11-02   platform   3.3    c
>>> df2
'col8'       col 6  col3       col4 col7
col1
2         4  1991-11-01   2.3  analytics    b
1         3  1991-10-31   1.3   teradata    a
3         5  1991-11-03   4.3    are you    d
```
#### Example 1: Specify merge conditions as string using on argument as well as
#### DataFrame indexes as merge keys
>>> df1.merge(right = df2, how = "left", on = ["col3","col2=col4"], use_index = True, lsuffix = "t1", rsuffix
= "t2")
col1_t1  col1_t2       col2  col3_t1  col3_t2 col5    col 6_t1    col 6_t2  'col8'_t1  'col8'_t2
col4  col7
0        2      2.0  analytics      2.3      2.3    b  1991-11-01  1991-11-01          4        4.0  analytics     b
1        1      1.0   teradata      1.3      1.3    a  1991-10-31  1991-10-31          3        3.0   teradata     a
2        3      NaN   platform      3.3      NaN    c  1991-11-02        None          5        NaN       None  None
#### Example 2: Specify on conditions as ColumnExpression and DataFrame
#### indexes as merge keys
>>> df1.merge(right = df2, how = "left", on = [df1.col1, df1.col3], use_index = True, lsuffix = "t1", rsuffix
= "t2")
col1_t1  col1_t2       col2  col3_t1  col3_t2 col5    col 6_t1    col 6_t2  'col8'_t1  'col8'_t2       col4  col7
0        2      2.0  analytics      2.3      2.3    b  1991-11-01  1991-11-01          4        4.0  analytics     b
1        1      1.0   teradata      1.3      1.3    a  1991-10-31  1991-10-31          3        3.0   teradata     a
2        3      NaN   platform      3.3      NaN    c  1991-11-02        None          5        NaN       None  None
#### Example 3: Specify left_on, right_on conditions along with DataFrame indexes
#### as merge keys
>>> df1.merge(right = df2, how = "right", left_on = "col2", right_on = "col4", use_index = True, lsuffix = "t1",
rsuffix = "t2")
col1_t1  col1_t2       col2  col3_t1  col3_t2  col5    col 6_t1    col 6_t2  'col8'_t1  'col8'_t2       col4 col7
0      2.0        2  analytics      2.3      2.3     b  1991-11-01  1991-11-01        4.0          4  analytics    b
1      1.0        1   teradata      1.3      1.3     a  1991-10-31  1991-10-31        3.0          3   teradata    a
2      NaN        3       None      NaN      4.3  None        None  1991-11-03        NaN          5    are you    d
#### Example 4: DataFrames to be merged do not contain common columns
If DataFrames to be merged do not contain common columns, lsuffix and rsuffix are not required.
```python
>>> new_df1 = df1.select(['col2', 'col5'])
>>> new_df2 = df2.select(['col4', 'col7'])
>>> new_df1
col5       col2
0    b  analytics
1    a   teradata
2    c   platform
>>> new_df2
col7       col4
0    b  analytics
1    a   teradata
2    d    are you
>>> new_df1.merge(right = new_df2, how = "inner", on = "col5=col7")
col5       col4       col2 col7
0    b  analytics  analytics    b
1    a   teradata   teradata    a
```
#### Example 5: No merge conditions are specified
When no merge conditions are specified, teradataml DataFrame indexes are used as merge keys.
>>> df1.merge(right = df2, how = "full", lsuffix = "t1", rsuffix = "t2")
col1_t1  col1_t2       col2  col3_t1  col3_t2 col5    col 6_t1    col 6_t2  'col8'_t1  'col8'_t2       col4 col7
0        2        2  analytics      2.3      2.3    b  1991-11-01  1991-11-01          4          4  analytics    b
1        1        1   teradata      1.3      1.3    a  1991-10-31  1991-10-31          3          3   teradata    a
2        3        3   platform      3.3      4.3    c  1991-11-02  1991-11-03          5          5    are you    d
### sample() Method
The sample() function samples rows from a DataFrame, directly or based on conditions. The function
creates a new column 'sampleid' which has a unique ID for each sample, helping identify each sample.
Note:
If more than one sample() operations are performed on teradataml DataFrame, then 'sampleid' from
the latest call is projected. Previous 'sampleid' columns are ignored.
In this example, 'sampleid' column shown is from the latest sample() operation (sample(frac =
[0.5, 0.1]).
```python
>>> from teradataml import *
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame("admissions_train")
>>> df.sample(n = 20).sample(frac = [0.5, 0.1])
masters   gpa     stats programming  admitted  sampleid
id
15     yes  4.00  Advanced    Advanced         1         1
37      no  3.52    Novice      Novice         1         1
35      no  3.68    Novice    Beginner         1         2
17      no  3.83  Advanced    Advanced         1         1
9       no  3.82  Advanced    Advanced         1         1
3       no  3.70    Novice    Beginner         1         1
34     yes  3.85  Advanced    Beginner         0         1
39     yes  3.75  Advanced    Beginner         0         1
36      no  3.00  Advanced      Novice         0         2
19     yes  1.98  Advanced    Advanced         0         1
```
In the following example, two sample() operations are performed on a DataFrame, but not
consecutively. The 'sampleid' column in the result is from the latest sample operation (sample(n = [5,
4])). 'sampleid' column from the previous call is ignored.
```python
>>> from teradataml import*
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame.from_table('admissions_train')
>>> df.sample(frac = 0.8).filter(items = ["masters"]).sample(n = [5, 4])
masters  sampleid
0     yes         1
1      no         1
2     yes         1
3      no         2
4      no         1
5      no         2
6     yes         2
7     yes         1
8     yes         2
```
#### Example Prerequisite
```python
>>> from teradataml import *
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming admitted
id
13      no  4.00  Advanced      Novice        1
26     yes  3.57  Advanced    Advanced        1
5       no  3.44    Novice      Novice        0
19     yes  1.98  Advanced    Advanced        0
15     yes  4.00  Advanced    Advanced        1
40     yes  3.95    Novice    Beginner        0
7      yes  2.33    Novice      Novice        1
22     yes  3.46    Novice    Beginner        0
36      no  3.00  Advanced      Novice        0
38     yes  2.65  Advanced    Beginner        1
```
#### Example 1: Sample one specific number of rows
This example randomly samples 2 rows from the teradataml DataFrame. As there is only one sample, the
'sampleid' is 1.
```python
>>> df.sample(n = 2)
masters   gpa     stats programming admitted SampleId
id
18     yes  3.81  Advanced    Advanced        1        1
19     yes  1.98  Advanced    Advanced        0        1
```
#### Example 2: Sample multiple values for the number of rows to be sampled
This example creates two samples, one with 2 rows and one with 1 row. There are two values (1,2) for
'sampleid', each indicates one sample.
```python
>> df.sample(n = [2, 1])
masters   gpa     stats programming admitted SampleId
id
1      yes  3.95  Beginner    Beginner        0        1
10      no  3.71  Advanced    Advanced        1        1
11      no  3.13  Advanced    Advanced        1        2
```
#### Example 3: Sample one specific percentage of rows
This example randomly samples 20% of the total rows in the input teradataml DataFrame.
```python
>>> df.sample(frac = 0.2)
masters   gpa     stats programming admitted SampleId
id
18     yes  3.81  Advanced    Advanced        1        1
15     yes  4.00  Advanced    Advanced        1        1
14     yes  3.45  Advanced    Advanced        0        1
35      no  3.68    Novice    Beginner        1        1
27     yes  3.96  Advanced    Advanced        0        1
25      no  3.96  Advanced    Advanced        1        1
10      no  3.71  Advanced    Advanced        1        1
9       no  3.82  Advanced    Advanced        1        1
```
#### Example 4: Sample multiple values for the percentage of rows to be sampled
This example creates two samples, one with 4% of total rows and one with 2% of total rows.
```python
>>> df.sample(frac = [0.04, 0.02])
masters   gpa     stats programming admitted SampleId
id
29     yes  4.00    Novice    Beginner        0        1
19     yes  1.98  Advanced    Advanced        0        2
11      no  3.13  Advanced    Advanced        1        1
```
#### Example 5: Sample specific number of rows, replace and randomization
This example creates two samples, one with 2 rows and one with 1 row, with possible redundant sampling
as replace is True and also selects rows from different AMPs as randomize is True.
```python
>>> df.sample(n = [2, 1], replace = True, randomize = True)
masters   gpa     stats programming admitted SampleId
id
12      no  3.65    Novice      Novice        1        1
39     yes  3.75  Advanced    Beginner        0        1
20     yes  3.90  Advanced    Advanced        1        2
```
#### Example 6: Sample specific percentage of rows, replace and randomization
This example creates two samples, one with 4% of total rows and one with 2% of total rows in teradataml
DataFrame, with possible redundant sampling and also selects rows from different AMPs.
```python
>>> df.sample(frac = [0.04, 0.02], replace = True, randomize = True)
masters   gpa     stats programming admitted SampleId
id
7      yes  2.33    Novice      Novice        1        2
30     yes  3.79  Advanced      Novice        0        1
33      no  3.55    Novice      Novice        1        1
```
#### Example 7: Sample with condition and number of samples to be sampled
This example creates two samples, with 1, 2 rows respectively from rows which satisfy df.gpa < 2 and 2.5%
of rows from rows which satisfy df.stats == 'Advanced'.
```python
>>> df.sample(case_when_then={df.gpa < 2 : [1, 2], df.stats ==
'Advanced' : 0.025})
masters   gpa     stats programming admitted SampleId
id
19     yes  1.98  Advanced    Advanced        0        1
24      no  1.87  Advanced      Novice        1        1
11      no  3.13  Advanced    Advanced        1        3
```
#### Example 8: Sample with condition and number of samples to be sampled,
#### replace and randomization
This example creates two samples with 1 and 2 rows respectively from rows which satisfy df.gpa < 2
and 2.5% of rows from rows which satisfy df.stats == 'Advanced' and selects rows from different AMPs
with replacement.
```python
>>> df.sample(replace = True, randomize = True, case_when_then={df.gpa < 2 :
[1, 2], df.stats == 'Advanced' : 0.025})
masters   gpa     stats programming admitted SampleId
id
24      no  1.87  Advanced      Novice        1        1
24      no  1.87  Advanced      Novice        1        2
24      no  1.87  Advanced      Novice        1        2
24      no  1.87  Advanced      Novice        1        2
24      no  1.87  Advanced      Novice        1        2
24      no  1.87  Advanced      Novice        1        1
31     yes  3.50  Advanced    Beginner        1        3
```
#### Example 9: Sample with different conditions and numbers of samples to
#### be sampled
This example creates creates seven samples:
* Two with 1, 3 rows from rows which satisfy df.gpa > 2
* One with 5 rows from rows which satisfy df.programming == 'Novice'
* One with 5 rows from rows which satisfy df.masters == 'no'
* One with 1 row from rows which does not meet all the previous conditions
```python
>>> df.sample(case_when_then = {df.gpa > 2 : [1, 3], df.stats == 'Novice' : [1,
2], df.programming == 'Novice' : 5, df.masters == 'no': 5}, case_else = 1)
masters   gpa     stats programming admitted SampleId
id
24      no  1.87  Advanced      Novice        1        5
2      yes  3.76  Beginner    Beginner        0        1
12      no  3.65    Novice      Novice        1        2
38     yes  2.65  Advanced    Beginner        1        2
36      no  3.00  Advanced      Novice        0        2
19     yes  1.98  Advanced    Advanced        0        7
```
#### Example 10: Sample with different conditions and numbers of samples to be
#### sampled, replace and randomization
This example creates Four samples:
* Two with 1, 3 rows from rows which satisfy df.gpa > 2
* Two with 2.5%, 5% of rows from rows which does not meet the previous condition with possible
  redundant replacement and also select rows from different AMPs
```python
>>> df.sample(case_when_then = {df.gpa < 2 : [1, 3]}, replace = True, randomize
= True, case_else = [0.025, 0.05])
masters   gpa     stats programming admitted SampleId
id
19     yes  1.98  Advanced    Advanced        0        1
19     yes  1.98  Advanced    Advanced        0        2
19     yes  1.98  Advanced    Advanced        0        2
19     yes  1.98  Advanced    Advanced        0        2
19     yes  1.98  Advanced    Advanced        0        2
40     yes  3.95    Novice    Beginner        0        3
3       no  3.70    Novice    Beginner        1        4
19     yes  1.98  Advanced    Advanced        0        2
19     yes  1.98  Advanced    Advanced        0        2
19     yes  1.98  Advanced    Advanced        0        1
```
### set_index() Method
Use the set_index() function to assign an appropriate index to a teradataml DataFrame.
The keys parameter is used to assign one or more existing columns as the new index to a teradataml
DataFrame. The argument can be a single column name or a list of column names.
Arguments:
* append: Allows the user to specify whether or not to append requested columns to an already existing
  index (if any).
* drop: Allows the user to specify whether or not to display the requested index being assigned as a
  column of the teradataml DataFrame.
#### Examples Prerequisite
Assume a teradataml DataFrame is created based on the table "df_admissions_train".
```python
>>> df1 = DataFrame.from_table('df_admissions_train')
>>> df1
id masters gpa  stats    programming admitted
0 26 yes     3.57 advanced advanced    1
1 34 yes     3.85 advanced beginner    0
2 40 yes     3.95 novice   beginner    0
3 14 yes     3.45 advanced advanced    0
4 29 yes     4.0  novice   beginner    0
5 6  yes     3.5  beginner advanced    1
6 36 no      3.0  advanced novice      0
7 32 yes     3.46 advanced beginner    0
8 5  no      3.44 novice   novice      0
```
#### Example 1: Assign a single index column 'id' as the index
This example assigns a single index column 'id' as the index to a teradataml DataFrame without an index
created from the 'admissions_train' table.
```python
>>> df2 = df1.set_index("id")
>>> df2
masters gpa  stats    programming admitted
id
26 yes     3.57 advanced advanced    1
34 yes     3.85 advanced beginner    0
40 yes     3.95 novice   beginner    0
14 yes     3.45 advanced advanced    0
29 yes     4.0  novice   beginner    0
6  yes     3.5  beginner advanced    1
36 no      3.0  advanced novice      0
32 yes     3.46 advanced beginner    0
5  no      3.44 novice   novice      0
```
#### Example 2: Assign a multicolumn index using a list of columns names
This examples uses a list of columns names to assign a multicolumn index.
```python
>>> df3 = df1.set_index(["id", "masters"])
>>> df3
gpa  stats    programming admitted
id masters
26 yes     3.57 advanced advanced    1
34 yes     3.85 advanced beginner    0
40 yes     3.95 novice   beginner    0
14 yes     3.45 advanced advanced    0
29 yes     4.0  novice   beginner    0
6  yes     3.5  beginner advanced    1
36 no      3.0  advanced novice      0
32 yes     3.46 advanced beginner    0
5  no      3.44 novice   novice      0
```
#### Example 3: Add an additional column as an index
This example add an additional column as an index to a teradataml DataFrame that already has an index.
```python
>>> df4 = df3.set_index("gpa", append = True, drop = True)
>>> df4
stats    programming admitted
id masters gpa
26 yes     3.57 advanced advanced    1
34 yes     3.85 advanced beginner    0
40 yes     3.95 novice   beginner    0
14 yes     3.45 advanced advanced    0
29 yes     4.0  novice   beginner    0
6  yes     3.5  beginner advanced    1
36 no      3.0  advanced novice      0
32 yes     3.46 advanced beginner    0
5  no      3.44 novice   novice      0
```
#### Example 4: Display an assigned index as a column of a DataFrame
This example displays an assigned index as a column of a teradataml DataFrame after assigning it as
an index.
```python
>>> df5 = df3.set_index("gpa", append = True, drop = False)
>>> df5
gpa  stats    programming admitted
id masters gpa
26 yes     3.57 3.57 advanced advanced    1
34 yes     3.85 3.85 advanced beginner    0
40 yes     3.95 3.95 novice   beginner    0
14 yes     3.45 3.45 advanced advanced    0
29 yes     4.0  4.0  novice   beginner    0
6  yes     3.5  3.5  beginner advanced    1
36 no      3.0  3.0  advanced novice      0
32 yes     3.46 3.46 advanced beginner    0
5  no      3.44 3.44 novice   novice      0
```
### show_query() Method
Use the show_query() method to return underlying SQL for a teradataml DataFrame. It is the same SQL
that is used to view the data of the teradataml DataFrame.
Arguments:
The optional argument full_query specifies if the complete query for the dataframe must be returned.
* If set to True, query for the dataframe is returned with respect to the base dataframe's table
  ("from_table()" or "from_query()"), or from the output tables of analytical functions, if there are any in
  the workflow.
  Note:
    This query may or may not be directly used to retrieve data of the dataframe upon which the
    function is called.
* If set to False or not used, the string returned is the query already used or will be used to retrieve data
  for the teradataml DataFrame.
  This is the default value.
Note:
show_query() API is not intended to be used on the output of the following APIs:
* set_index()
* sort()
* sort_index()
* squeeze()
Note:
In most cases, queries returned by show_query() with full_query set to False, can be executed
as is on Database Engine 20. But there are certain functions which are lazy, some functions are
executed at the time when actual data is requested. When such DataFrames are manipulated and
show_query() is executed on the outcome, such queries may or may not be executable on Database
Engine 20.
Following are some analytic functions which creates lazy DataFrames:
* ML Engine functions:
    * Attribution
    * CoxPH
    * CoxHazardRatio
    * DecisionForest
    * DecisionForestPredict
    * FPGrowth
    * GLML1L2Predict
    * NaiveBayes
    * PageRank
    * POSTagger
    * Sessionize
    * XGBoost
* Database Engine 20 functions:
    * MovingAverage
    * NaiveBayesTextClassifierPredict
#### Examples Prerequisite
Load example data and create required DataFrame on it.
```python
>>> load_example_data("dataframe", "admissions_train")
>>> load_example_data("NaiveBayes", "nb_iris_input_train")
>>> df = DataFrame.from_table("admissions_train")
```
#### Example 1: Show query on base (from_table) dataframe, with default option.
```python
>>> df.show_query()
'select * from "admissions_train"'
```
#### Example 2: Show query on base (from_query) dataframe, with default option.
```python
>>> df_from_query = DataFrame.from_query("select masters, gpa
from admissions_train")
>>> df_from_query.show_query()
'select masters, gpa from admissions_train'
```
#### Example 3: Show query on base (from_table) dataframe, with
#### full_query option.
This will return same query as with default option because workflow only has one dataframe.
```python
>>> df.show_query(full_query = True)
'select * from "admissions_train"
```
#### Example 4: Show query on base (from_query) dataframe, with
#### full_query option.
```python
>>> df_from_query = DataFrame.from_query("select masters, gpa
from admissions_train")
>>> df_from_query.show_query(full_query = True)
'select masters, gpa from admissions_train'
```
#### Example 5: Show query used in a workflow demonstrating default and
#### full_query options.
* Assign operation on base dataframe:
```python
# Workflow Step-1: Assign operation on base dataframe
>>> df1 = df.assign(temp_column=admissions_train_df.gpa
+ admissions_train_df.admitted)
```
* Select columns from assign's result:
```python
# Workflow Step-2: Selecting columns from assign's result
>>> df2 = df1.select(["masters", "gpa", "programming", "admitted"])
```
* Filter on top of select's result:
```python
# Workflow Step-3: Filtering on top of select's result
>>> df3 = df2[df2.admitted > 0]
```
* Sample 90% rows from filter's result:
```python
# Workflow Step-4: Sampling 90% rows from filter's result
>>> df4 = df3.sample(frac=0.9)
```
* Show query with full_query option on df4:
```python
# Show query with full_query option on df4.
# This will give full query upto base dataframe(df)
>>> df4.show_query(full_query = True)
'select masters,gpa,stats,programming,admitted,sampleid as "sampleid" from (
select * from (select masters,gpa,stats,programming,admitted from (select
id AS
id, masters AS masters, gpa AS gpa, stats AS stats, programming
AS programming,
admitted AS admitted, gpa + admitted AS temp_column from
"admissions_train") as
temp_table) as temp_table where admitted > 0) as temp_table SAMPLE 0.9'
```
* Show query with default option on df4:
```python
# Show query with default option on df4. This will give same query as give
in above case.
>>> df4.show_query()
'select masters,gpa,stats,programming,admitted,sampleid as "sampleid" from
(select *
from (select masters,gpa,stats,programming,admitted from (select id AS
id, masters
AS masters, gpa AS gpa, stats AS stats, programming AS programming,
admitted AS admitted,
gpa + admitted AS temp_column from "admissions_train") as temp_table)
as temp_table
where admitted > 0) as temp_table SAMPLE 0.9'
```
* Materialize intermediate dataframe df2:
```python
>>> df2
masters   gpa programming  admitted
0      no  4.00      Novice         1
1     yes  3.57    Advanced         1
2      no  3.44      Novice         0
3     yes  1.98    Advanced         0
4     yes  4.00    Advanced         1
5     yes  3.95    Beginner         0
6     yes  2.33      Novice         1
7     yes  3.46    Beginner         0
8      no  3.00      Novice         0
9     yes  2.65    Beginner         1
```
* Show query with default option on df4:
```python
# Show query with default option on df4. This will give query with respect
# to view/table created by the latest materialized dataframe in the workflow
(df2 in this scenario).
# This is the query teradataml internally uses to retrieve data for
dataframe df4, if executed
# at this point.
>>> df4.show_query()
'select masters,gpa,stats,programming,admitted,sampleid as "sampleid" from
(select * from
"ALICE"."ml__select__1585722211621282" where admitted > 0) as temp_table
SAMPLE 0.9'
```
* Show query with full_query option on df4:
```python
>>> df4.show_query(full_query = True)
'select masters,gpa,stats,programming,admitted,sampleid as "sampleid" from
(select *
from (select masters,gpa,stats,programming,admitted from (select id AS
id, masters
AS masters, gpa AS gpa, stats AS stats, programming AS programming,
admitted AS admitted,
gpa + admitted AS temp_column from "admissions_train") as temp_table)
as temp_table
where admitted > 0) as temp_table SAMPLE 0.9'
```
### sort() Method
Use the sort() method to sort data on one or more columns in either ascending or descending order for a
teradataml DataFrame.
The method takes a column name or a list of column names to sort on. Use the ascending parameter to
specified ascending or descending order. True for ascending order and False for descending order.
Note:
The column used for sorting in sort must have type that supports sorting.
Unsupported types include: 'BLOB', 'CLOB', 'ARRAY', 'VARRAY'.
Required Arguments:
* columns: Specifies the names of the columns or ColumnExpressions to sort on.
Optional Arguments:
* ascending: Specifies whether to order in ascending or descending order for each column specified
  in columns.
  When set to True, sort in ascending order. Otherwise, sort in descending order.
  Default value is True.
  Note:
    * If a list is specified, length of the ascending must equal length of the columns.
    * If a list is specified, element in ascending is ignored if the corresponding element in columns
    is a ColumnExpression.
    * This argument is ignored if columns is a ColumnExpression.
#### Examples Prerequisite
Assume a teradataml DataFrame "df" is created from a Vantage table "admissions_train", using command:
```python
df = DataFrame("admissions_train")
```
#### Example 1: Sort data based on the column 'Feb' in ascending order by passing
#### the name of the column
```python
df.sort("Feb")
Feb    Jan    Mar    Apr    datetime
accounts
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
Jones LLC   200.0
.0  140.0  180.0  04/01/2017
Red Inc     200.0
.0  140.0    NaN  04/01/2017
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
```
#### Example 2: Sort data based on the column 'Feb' in descending order by
#### passing the ColumnExpression
```python
df.sort([df.Feb.desc()])
Feb    Jan    Mar    Apr    datetime
accounts
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Jones LLC   200.0
.0  140.0  180.0  04/01/2017
Red Inc     200.0  150.0  140.0    NaN  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
```
#### Example 3: Sort column and ColumnExpression in different ascending but
#### same NULL settings
This example sorts data based on the columns 'Jan' in ascending order and 'accounts' in descending order,
respectively, both with NULLS at first.
Note:
Since the second element in columns is a ColumnExpression, the data is sorted in descending order
even though the second element in ascending is True.
```python
df.sort([df.Jan.nulls_first(), df.accounts.desc().nulls_first()])
Feb    Jan    Mar    Apr    datetime
accounts
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Red Inc     200.0  150.0  140.0    NaN  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
```
#### Example 4: Sort different columns in different ascending but same
#### NULL settings
This example sorts data based on columns 'Jan' in ascending order with NULLS at first and 'Apr' in
descending order with NULLS at first, respectively.
```python
df.sort([df.Jan.nulls_first(), df.Apr.desc().nulls_first()])
Feb    Jan    Mar    Apr    datetime
accounts
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Red Inc     200.0  150.0  140.0    NaN  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
```
#### Example 5: Sort different columns in different ascending and NULL settings
This example sorts the data based on columns 'Jan' and in ascending order with NULLS at first and 'Apr'
in descending order with NULLS at last, respectively.
```python
df.sort([df.Jan.nulls_first(), df.Apr.desc().nulls_last()])
Feb    Jan    Mar    Apr    datetime
accounts
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Red Inc     200.0  150.0  140.0    NaN  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
```
### sort_index() Method
Use the sort_index() method to get object sorted by labels (along an axis) in either ascending or
descending order for a teradataml DataFrame.
#### Example 1: Default behavior of sort_index() when no arguments is given.
```python
>>> load_example_data("dataframe","scale_housing_test")
>>> df = DataFrame.from_table('scale_housing_test')
>>> df
id    price  lotsize  bedrooms  bathrms  stories
types
classic   14  36000.0   2880.0       3.0      1.0      1.0
bungalow  11  90000.0   7200.0       3.0      2.0      1.0
classic   15  37000.0   3600.0       2.0      1.0      1.0
classic   13  27000.0   1700.0       3.0      1.0      2.0
classic   12  30500.0   3000.0       2.0      1.0      1.0
>>> df.sort_index()
id    price  lotsize  bedrooms  bathrms  stories
types
bungalow  11  90000.0   7200.0       3.0      2.0      1.0
classic   13  27000.0   1700.0       3.0      1.0      2.0
classic   12  30500.0   3000.0       2.0      1.0      1.0
classic   14  36000.0   2880.0       3.0      1.0      1.0
classic   15  37000.0   3600.0       2.0      1.0      1.0
```
#### Example 2: Use sort_index() with DESCENDING for respective axis.
```python
>>> load_example_data("dataframe","scale_housing_test")
>>> df = DataFrame.from_table('scale_housing_test')
>>> df.sort_index(1, False)
stories    price  lotsize  id  bedrooms  bathrms
types
classic       1.0  36000.0   2880.0  14       3.0      1.0
bungalow      1.0  90000.0   7200.0  11       3.0      2.0
classic       1.0  37000.0   3600.0  15       2.0      1.0
classic       2.0  27000.0   1700.0  13       3.0      1.0
classic       1.0  30500.0   3000.0  12       2.0      1.0
```
#### Example 3: Use sort_index() with type of sorting algorithm.
```python
>>> load_example_data("dataframe","scale_housing_test")
>>> df = DataFrame.from_table('scale_housing_test')
>>> df.sort_index(1, True, 'mergesort')
bathrms  bedrooms  id  lotsize    price  stories
types
classic       1.0       3.0  14   2880.0  36000.0      1.0
bungalow      2.0       3.0  11   7200.0  90000.0      1.0
classic       1.0       2.0  15   3600.0  37000.0      1.0
classic       1.0       3.0  13   1700.0  27000.0      2.0
classic       1.0       2.0  12   3000.0  30500.0      1.0
```
### squeeze() Method
Use the squeeze() method to squeeze one-dimensional axis objects into a scalar for teradataml
DataFrames with a single element, or a Series object for a teradataml DataFrame with a single column.
The teradataml DataFrame is returned unchanged when both dimensions are greater than one.
Arguments:
The axis argument specifies the axis along which the squeeze operation is to be attempted. The possible
values are:
* 1 or 'columns': Return Series object if number of columns equals one.
* 0 or 'index' : Return the unchanged teradataml DataFrame object.
When axis is not specified and both dimensions equal one, a scalar object is returned which is the only
element in the DataFrame.
#### Examples Prerequisite
Assume the table "admissions_train" exists and its index column is id. And a DataFrame "df" is created
based on this table using the command:
```python
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming admitted
id
22     yes  3.46    Novice    Beginner        0
36      no  3.00  Advanced      Novice        0
15     yes  4.00  Advanced    Advanced        1
38     yes  2.65  Advanced    Beginner        1
5       no  3.44    Novice      Novice        0
17      no  3.83  Advanced    Advanced        1
34     yes  3.85  Advanced    Beginner        0
13      no  4.00  Advanced      Novice        1
26     yes  3.57  Advanced    Advanced        1
19     yes  1.98  Advanced    Advanced        0
```
#### Example 1: Squeeze a teradataml DataFrame with both dimension greater
#### than one.
```python
>>> df.squeeze()
masters   gpa     stats programming admitted
id
22     yes  3.46    Novice    Beginner        0
36      no  3.00  Advanced      Novice        0
15     yes  4.00  Advanced    Advanced        1
38     yes  2.65  Advanced    Beginner        1
5       no  3.44    Novice      Novice        0
17      no  3.83  Advanced    Advanced        1
34     yes  3.85  Advanced    Beginner        0
13      no  4.00  Advanced      Novice        1
26     yes  3.57  Advanced    Advanced        1
19     yes  1.98  Advanced    Advanced        0
```
#### Example 2: Squeeze a single-column teradataml DataFrame.
```python
>>> gpa = df.select(["gpa"])
>>> gpa.squeeze()
0    4.00
1    2.33
2    3.46
3    3.83
4    4.00
5    2.65
6    3.57
7    3.44
8    3.85
9    3.95
Name: gpa, dtype: float64
>>> gpa.squeeze(axis = 1)
0    3.46
1    3.00
2    4.00
3    2.65
4    3.44
5    3.83
6    3.85
7    4.00
8    3.57
9    1.98
Name: gpa, dtype: float64
>>> gpa.squeeze(axis = 0)
gpa
0  3.46
1  3.00
2  4.00
3  2.65
4  3.44
5  3.83
6  3.85
7  4.00
8  3.57
9  1.98
```
#### Example 3: Squeeze a teradataml DataFrame with multiple columns and a
#### single row.
```python
>>> df = DataFrame.from_query('select gpa, stats from admissions_train
where gpa=2.33')
>>> df
gpa   stats
0  2.33  Novice
>>> s = df.squeeze()
>>> s
gpa   stats
0  2.33  Novice
```
#### Example 4: Squeeze a teradataml DataFrame with a single element.
```python
>>> single_gpa = DataFrame.from_query('select gpa from admissions_train
where gpa=2.33')
>>> single_gpa
gpa
0  2.33
>>> single_gpa.squeeze()
2.33
>>> single_gpa.squeeze(axis = 1)
0    2.33
Name: gpa, dtype: float64
>>> single_gpa.squeeze(axis = 0)
gpa
0  2.33
```
### drop_duplicate() Function
Use the the drop_duplicate() function to drop duplicate rows from teradataml DataFrame to return distinct
values from the DataFrame.
Optional Argument:
* column_names: Specifies the names of the columns to drop the duplicate values of, to get the
  distinct values.
  If not specified, all columns in the DataFrame are considered for the operation.
#### Example Setup
In this example, "admission_train" dataset is used.
```python
>>> from teradataml import *
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame("admissions_train")
# Print dataframe.
>>> df
masters   gpa     stats programming admitted
id
13      no  4.00  Advanced      Novice        1
26     yes  3.57  Advanced    Advanced        1
5       no  3.44    Novice      Novice        0
19     yes  1.98  Advanced    Advanced        0
15     yes  4.00  Advanced    Advanced        1
40     yes  3.95    Novice    Beginner        0
7      yes  2.33    Novice      Novice        1
22     yes  3.46    Novice    Beginner        0
36      no  3.00  Advanced      Novice        0
38     yes  2.65  Advanced    Beginner        1
```
#### Example 1: Get the distinct rows of values for the column 'programming'
```python
>>> df.drop_duplicate("programming")
programming
0      Novice
1    Beginner
2    Advanced
```
#### Example 2: Get the distinct rows of values for the columns 'programming'
#### and 'admitted'
```python
>>> df.drop_duplicate(["programming","admitted"])
programming  admitted
0    Beginner         0
1    Advanced         1
2    Beginner         1
3    Advanced         0
4      Novice         1
5      Novice         0
```
### cube() Function
Use the cube() function to create a multidimensional cube for a teradataml DataFrame using specified
columns, and there by running aggregates on it produce the aggregations on different dimensions.
#### Required Parameter
columns
    Specifies the names of input teradataml DataFrame columns.
#### Optional Parameter
include_grouping_columns
    Specifies whether to include aggregations on the grouping columns or not. When set to
    True, the resultant DataFrame will have the aggregations on the columns mentioned in
    "columns_expr". Otherwise, resultant DataFrame will not have aggregations on the columns
    mentioned in "columns_expr".
    Default value: False
#### Example Setup
In this example, "admission_train" dataset is used.
```python
>>> from teradataml import *
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame("admissions_train")
```
#### Example 1: Analyzes the data by grouping into masters and stats dimensions
```python
>>> df1 = df.cube(["masters", "stats"]).sum()
>>> df1
masters     stats  sum_id  sum_gpa  sum_admitted
0      no  Beginner       8     3.60             1
1    None  Advanced     555    84.21            16
2    None  Beginner      21    18.31             3
3     yes  Beginner      13    14.71             2
4    None      None     820   141.67            26
5     yes  Advanced     366    49.26             7
6      no      None     343    63.96            16
7    None    Novice     244    39.15             7
8      no  Advanced     189    34.95             9
9     yes    Novice      98    13.74             1
```
#### Example 2: Find the average of all valid columns by grouping the DataFram
#### with columns 'masters' and 'admitted'
Include grouping columns in aggregate function 'avg'.
```python
>>> df1 = df.cube(["masters", "admitted"], include_grouping_columns=True).avg()
>>> df1
masters admitted    avg_id  avg_gpa avg_admitted
0       yes      NaN 21.681818 3.532273     0.454545
1      None      1.0 18.846154 3.533462     1.000000
2       no       NaN 19.055556 3.553333     0.888889
3      yes       0.0 24.083333 3.613333     0.000000
4     None       NaN 20.500000 3.541750     0.650000
5     None       0.0 23.571429 3.557143     0.000000
6      yes       1.0 18.800000 3.435000     1.000000
7       no       1.0 18.875000 3.595000     1.000000
8       no       0.0 20.500000 3.220000     0.000000
>>>
```
#### Example 3: Find the average of all valid columns by grouping the DataFram
#### with columns 'masters' and 'admitted'
Do not include grouping columns in aggregate function 'avg'.
```python
>>> df1 = df.cube(["masters", "admitted"], include_grouping_columns=False).avg()
>>> df1
masters admitted    avg_id  avg_gpa
0       no      0.0 20.500000 3.220000
1     None      1.0 18.846154 3.533462
2       no      NaN 19.055556 3.553333
3      yes      0.0 24.083333 3.613333
4     None      NaN 20.500000 3.541750
5     None      0.0 23.571429 3.557143
6      yes      1.0 18.800000 3.435000
7      yes      NaN 21.681818 3.532273
8       no      1.0 18.875000 3.595000
>>>
```
### rollup() Function
Use the rollup() function to create a multidimensional rollup for a DataFrame using specified columns, and
there by running aggregates on it produce the aggregations on different dimensions.
#### Required Parameter
columns
    Specifies the names of input teradataml DataFrame columns.
#### Optional Parameter
include_grouping_columns
    Specifies whether to include aggregations on the grouping columns or not. When set to
    True, the resultant DataFrame will have the aggregations on the columns mentioned in
    "columns_expr". Otherwise, resultant DataFrame will not have aggregations on the columns
    mentioned in "columns_expr".
    Default value: False
#### Example setup
In this example, "admission_train" dataset is used.
```python
>>> from teradataml import *
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame("admissions_train")
```
#### Example 1: Analyzes the data by grouping into masters and stats dimensions
```python
>>> df1 = df.rollup(["masters", "stats"]).sum()
>>> df1
masters     stats  sum_id  sum_gpa  sum_admitted
0      no      None     343    63.96            16
1     yes      None     477    77.71            10
2    None      None     820   141.67            26
3      no    Novice     146    25.41             6
4      no  Beginner       8     3.60             1
5     yes    Novice      98    13.74             1
6     yes  Beginner      13    14.71             2
7     yes  Advanced     366    49.26             7
8      no  Advanced     189    34.95             9
```
#### Example 2: Find the average of all valid columns by grouping the DataFrame
#### with columns 'masters' and 'admitted'
Include grouping columns in aggregate function 'avg'.
```python
>>> df1 = df.rollup(["masters",
"admitted"], include_grouping_columns=True).avg()
>>> df1
masters admitted    avg_id  avg_gpa avg_admitted
0        no      NaN 19.055556 3.553333     0.888889
1       yes      NaN 21.681818 3.532273     0.454545
2      None      NaN 20.500000 3.541750     0.650000
3       yes      0.0 24.083333 3.613333     0.000000
4        no      1.0 18.875000 3.595000     1.000000
5       yes      1.0 18.800000 3.435000     1.000000
6        no      0.0 20.500000 3.220000     0.000000
>>>
```
#### Example 3: Find the average of all valid columns by grouping the DataFrame
#### with columns 'masters' and 'admitted'
Do not include grouping columns in aggregate function 'avg'.
```python
>>> df1 = df.rollup(["masters",
"admitted"], include_grouping_columns=False).avg()
>>> df1
masters admitted    avg_id  avg_gpa
0        no      NaN 19.055556 3.553333
1       yes      NaN 21.681818 3.532273
2      None      NaN 20.500000 3.541750
3       yes      0.0 24.083333 3.613333
4        no      1.0 18.875000 3.595000
5       yes      1.0 18.800000 3.435000
6        no      0.0 20.500000 3.220000
>>>
```
### replace()
Use the replace() function to replace every occurrence of to_replace with the value in the columns
mentioned in subset. When subset is not provided, this function replaces occurrences of to_replace in
all columns.
Required Arguments:
* to_replace: Specifies a ColumnExpression or a literal that the function searches for values
  in the Column. Use ColumnExpression when you want to match the condition based on a
  DataFrameColumn function, else use literal.
  Supported column types: CHAR, VARCHAR, FLOAT, INTEGER, DECIMAL.
  Note:
    Only ColumnExpressions generated from DataFrameColumn functions are supported.
    BinaryExpressions are not supported.
    For example: Consider teradataml DataFrame has two columns COL1, COL2. df.COL1.abs() is
    supported but df.COL1 == df.COL2 is not supported.
* value: Specifies a ColumnExpression or a literal that replaces the to_replace in the column. Use
  ColumnExpression only when the value you want to replace is based on a DataFrameColumn
  function; otherwise, use literal.
  Supported column types: CHAR, VARCHAR, FLOAT, INTEGER, DECIMAL.
  This argument is required argument when to_replace is not a dictionary. Otherwise, it is optional
  and ignored.
  Note:
    Only ColumnExpressions generated from DataFrameColumn functions are supported.
    BinaryExpressions are not supported.
    For example: Consider teradataml DataFrame has two columns COL1, COL2. df.COL1.abs()
    is supported but df.COL1 == df.COL2 is not supported.
Optional Arguments:
* subset: Specifies the columns to consider for replacing the values.
#### Example Setup
* Load the data to run the example.
```python
>>> load_example_data("dataframe", "admissions_train")
```
* Create a DataFrame on 'admissions_train' table.
```python
>>> df = DataFrame("admissions_train")
>>> print(df)
masters   gpa     stats programming  admitted
id
15     yes  4.00  Advanced    Advanced         1
34     yes  3.85  Advanced    Beginner         0
13      no  4.00  Advanced      Novice         1
38     yes  2.65  Advanced    Beginner         1
5       no  3.44    Novice      Novice         0
40     yes  3.95    Novice    Beginner         0
7      yes  2.33    Novice      Novice         1
22     yes  3.46    Novice    Beginner         0
26     yes  3.57  Advanced    Advanced         1
17      no  3.83  Advanced    Advanced         1
```
#### Example 1: Replace the string 'Advanced' with 'Good' in columns 'stats'
#### and 'programming'
```python
>>> res = df.replace({"Advanced": "Good", "Beginner": "starter"},
subset=["stats", "programming"])
>>> print(res)
masters   gpa   stats programming  admitted
id
15     yes  4.00    Good        Good         1
7      yes  2.33  Novice      Novice         1
22     yes  3.46  Novice     starter         0
17      no  3.83    Good        Good         1
13      no  4.00    Good      Novice         1
38     yes  2.65    Good     starter         1
26     yes  3.57    Good        Good         1
5       no  3.44  Novice      Novice         0
34     yes  3.85    Good     starter         0
40     yes  3.95  Novice     starter         0
```
#### Example 2: Replace the string 'Advanced' with 'Good' and 'Beginner' with
#### 'starter' in columns 'stats' and 'programming'
```python
>>> res = df.replace({"Advanced": "Good", "Beginner": "starter"},
subset=["stats", "programming"])
>>> print(res)
masters   gpa   stats programming  admitted
id
15     yes  4.00    Good        Good         1
7      yes  2.33  Novice      Novice         1
22     yes  3.46  Novice     starter         0
17      no  3.83    Good        Good         1
13      no  4.00    Good      Novice         1
38     yes  2.65    Good     starter         1
26     yes  3.57    Good        Good         1
5       no  3.44  Novice      Novice         0
34     yes  3.85    Good     starter         0
40     yes  3.95  Novice     starter         0
```
#### Example 3: Append the string '_New' to 'stats' column when values in
#### 'programming' and 'stats' are same
```python
>>> res = df.replace({df.programming: df.stats+"_New"}, subset=["stats"])
>>> print(res)
masters   gpa         stats programming  admitted
id
15     yes  4.00  Advanced_New    Advanced         1
34     yes  3.85      Advanced    Beginner         0
13      no  4.00      Advanced      Novice         1
38     yes  2.65      Advanced    Beginner         1
5       no  3.44    Novice_New      Novice         0
40     yes  3.95        Novice    Beginner         0
7      yes  2.33    Novice_New      Novice         1
22     yes  3.46        Novice    Beginner         0
26     yes  3.57  Advanced_New    Advanced         1
17      no  3.83  Advanced_New    Advanced         1
```
#### Example 4: Round the values of 'gpa' to its nearest integer
```python
>>> res = df.replace({df.gpa: df.gpa.round(0)}, subset=["gpa"])
>>> print(res)
masters  gpa     stats programming  admitted
id
15     yes  4.0  Advanced    Advanced         1
7      yes  2.0    Novice      Novice         1
22     yes  3.0    Novice    Beginner         0
17      no  4.0  Advanced    Advanced         1
13      no  4.0  Advanced      Novice         1
38     yes  3.0  Advanced    Beginner         1
26     yes  4.0  Advanced    Advanced         1
5       no  3.0    Novice      Novice         0
34     yes  4.0  Advanced    Beginner         0
40     yes  4.0    Novice    Beginner         0
```