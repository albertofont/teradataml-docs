# teradataml Window Aggregates

* Window on DataFrame
* Window on DataFrame Column
* Window Aggregate Functions
## Window on DataFrame
Use the window() function to run window aggregate functions on the teradataml DataFrame.
The function allows user to specify the following window types for computations:
* Cumulative
* Group
* Moving
* Remaining
By default, window with Unbounded Preceding and Unbounded Following is considered for calculation.
Note:
If both partition_columns and order_columns are 'None' and the original DataFrame has BLOB and
CLOB type of columns, then
* Window aggregate operation on CLOB and BLOB type of columns is omitted;
* Resultant DataFrame does not contain the BLOB and CLOB type of columns from the
    original DataFrame.
#### Example Setup
```python
>>> from teradataml import *
>>> load_example_data("dataframe","sales")
>>> df = DataFrame.from_table('sales')
```
#### Example 1: Create a window on a teradataml DataFrame
```python
>>> window = df.window()
```
#### Example 2: Create a cumulative (expanding) window
This example creates a cumulative (expanding) window with rows between unbounded preceding and 3
preceding, with partition_columns and order_columns arguments, and with default sorting.
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
window_start_point=None,
window_end_point=-3)
```
#### Example 3: Create a moving (rolling) window
This example creates a moving (rolling) window with rows between current row and 3 following, with sorting
done on 'Feb', 'datetime' columns in descending order, and with partition_columns argument.
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
sort_ascending=False,
window_start_point=0,
window_end_point=3)
```
#### Example 4: Create a remaining (contracting) window
This example creates a remaining (contracting) window with rows between current row and unbounded
following, with sorting done on 'Feb', 'datetime' columns in ascending order, and NULL values in 'Feb',
'datetime' columns appear at last.
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
nulls_first=False,
window_start_point=0,
window_end_point=None)
```
#### Example 5: Create a grouping window
This example creates a grouping window, with sorting done on 'Feb', 'datetime' columns in ascending order,
and NULL values in 'Feb', 'datetime' columns appear at last.
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
sort_ascending=False,
nulls_first=False,
window_start_point=None,
window_end_point=None)
```
#### Example 6: Create a window, ignoring all parameters
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
sort_ascending=False,
nulls_first=False,
ignore_window=True)
```
#### Example 7: Perform sum on every valid column in DataFrame
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
sort_ascending=False,
nulls_first=False,
ignore_window=True)
```
>>> window.sum()
Feb    Jan    Mar    Apr    datetime  Apr_sum  Feb_sum  Jan_sum  Mar_sum
accounts
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017      781   1000.0      550      590
Red Inc     200.0  150.0  140.0    NaN  04/01/2017      781   1000.0      550      590
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017      781   1000.0      550      590
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017      781   1000.0      550      590
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017      781   1000.0      550      590
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017      781   1000.0      550      590
#### Example 8: Perform count on every valid column in DataFrame
```python
>>> window = df.window()
```
>>> window.count()
Feb    Jan    Mar    Apr    datetime  Apr_count  Feb_count  Jan_count  Mar_count
accounts_count  datetime_count
accounts
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017          4          6          4          4
6               6
Red Inc     200.0  150.0  140.0    NaN  04/01/2017          4          6          4          4
6               6
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017          4          6          4          4
6               6
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017          4          6          4          4
6               6
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017          4          6          4          4
6               6
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017          4          6          4          4
6               6
#### Example 9: Perform count of all valid columns in DataFrame, which is grouped
#### by 'accounts'
```python
>>> window = df.groupby("accounts").window()
>>> window.count()
accounts  accounts_count
0   Jones LLC               6
1     Red Inc               6
2  Yellow Inc               6
3  Orange Inc               6
4    Blue Inc               6
5    Alpha Co               6
```
## Window on DataFrame Column
Use the window() function to run window aggregate functions on the teradataml DataFrame columns.
The function allows user to specify the following window types for computations:
* Cumulative
* Group
* Moving
* Remaining
By default, window with Unbounded Preceding and Unbounded Following is considered for calculation.
Note:
If both partition_columns and order_columns are 'None', then window cannot be created on CLOB and
BLOB type of columns.
#### Example Setup
```python
>>> from teradataml import *
>>> load_example_data("dataframe","sales")
>>> df = DataFrame.from_table('sales')
```
#### Example 1: Create a window on a teradataml DataFrame Column
```python
>>> window = df.Feb.window()
```
#### Example 2: Create a cumulative (expanding) window
This example creates a cumulative (expanding) window with rows between unbounded preceding and 3
preceding, with partition_columns and order_columns arguments, and with default sorting.
```python
>>> window = df.window(partition_columns="Feb",
...                    order_columns=["Feb", "datetime"],
...                    window_start_point=None,
...                    window_end_point=-3)
```
#### Example 3: Create a moving (rolling) window
This example creates a moving (rolling) window with rows between current row and 3 following, with sorting
done on 'Feb', 'datetime' columns in descending order, and with partition_columns argument.
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
sort_ascending=False,
window_start_point=0,
window_end_point=3)
```
#### Example 4: Create a remaining (contracting) window
This example creates a remaining (contracting) window with rows between current row and unbounded
following, with sorting done on 'Feb', 'datetime' columns in ascending order, and NULL values in 'Feb',
'datetime' columns appear at last.
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
nulls_first=False,
window_start_point=0,
window_end_point=None)
```
#### Example 5: Create a grouping window
This example creates a grouping window, with sorting done on 'Feb', 'datetime' columns in ascending order,
and NULL values in 'Feb', 'datetime' columns appear at last.
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
sort_ascending=False,
nulls_first=False,
window_start_point=None,
window_end_point=None)
```
#### Example 6: Create a window, ignoring all parameters
```python
>>> window = df.window(partition_columns="Feb",
order_columns=["Feb", "datetime"],
sort_ascending=False,
nulls_first=False,
ignore_window=True)
```
#### Example 7: Perform sum of 'Feb' and attach new column to the DataFrame
```python
>>> window = df.Feb.window()
>>> df.assign(feb_sum=window.sum())
Feb    Jan    Mar    Apr    datetime  feb_sum
accounts
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017   1000.0
Red Inc     200.0  150.0  140.0    NaN  04/01/2017   1000.0
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017   1000.0
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017   1000.0
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017   1000.0
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017   1000.0
```
#### Example 8: Perform min and max operations on column 'Apr' and attach both
#### columns to the DataFrame
```python
>>> window = df.Apr.window()
>>> df.assign(apr_min=window.min(), apr_max=window.max())
Feb    Jan    Mar    Apr    datetime  apr_max  apr_min
accounts
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017      250      101
Red Inc     200.0  150.0  140.0    NaN  04/01/2017      250      101
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017      250      101
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017      250      101
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017      250      101
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017      250      101
```
#### Example 9: Perform count and max operations on column 'accounts' grouped
#### by 'accounts' and attach column to DataFrame
```python
>>> df = df.groupby("accounts")
>>> window = df.accounts.window()
>>> df.assign(accounts_max=window.max(), accounts_count=window.count())
accounts  accounts_count accounts_max
0   Jones LLC               6   Yellow Inc
1     Red Inc               6   Yellow Inc
2  Yellow Inc               6   Yellow Inc
3  Orange Inc               6   Yellow Inc
4    Blue Inc               6   Yellow Inc
5    Alpha Co               6   Yellow Inc
```
## Window Aggregate Functions
teradataml supports following window aggregate functions that can be executed on top of teradataml
Window object created using DataFrame.window() and DataFrameColumn.window().
See the teradataml: Window Aggregates section of Teradata Package for Python Function Reference,
B700-4008) at https://docs.teradata.com/ for detailed description and usage examples of these functions.
| Sr. No. | Function Name | Description |
| ------- | ------------- | ----------- |
| 1 | corr() | Returns the Sample Pearson product moment correlation coefficient of its arguments for all non-null data point pairs in a teradataml DataFrame or ColumnExpression over the specified window. |
| 2 | count() | Returns the total number of qualified rows in a teradataml DataFrame or ColumnExpression over the specified window. |
| 3 | covar_pop() | Returns the population covariance of its arguments for all non-null data point pairs over the specified window. Covariance measures whether or not two random variables vary in the same way. It is the average of the products of deviations for each non-null data point pair. |
| 4 covar_samp() | Returns the sample covariance of its arguments for all non-null data point pairs over the specified window. Covariance measures whether or not two random variables vary in the same way. It is the average of the products of deviations for each non-null data point pair. |  |
| 5 cume_dist() | Returns the cumulative distribution of values in a teradataml DataFrame or ColumnExpression over the specified window. |  |
| 6 dense_rank() | Returns the ordered ranking of all the rows in a teradataml DataFrame or ColumnExpression, according to "order_columns", over the specified window. |  |
| 7 first_value() | Returns the first value of an ordered set of values in a teradataml DataFrame or ColumnExpression over the specified window. |  |
| 8 lag() | The lag function accesses data from the row preceding the current row at a specified offset value over the specified window in a teradataml DataFrame or ColumnExpression. |  |
| 9 last_value() | Returns the last value of an ordered set of values in a teradataml DataFrame or ColumnExpression over the specified window. |  |
| 10 lead() | The lead function accesses data from the row following the current row at a specified offset value over the specified window in a teradataml DataFrame or ColumnExpression. |  |
| 11 max() | Returns the maximum of values in teradataml DataFrame or ColumnExpression over the specified window. |  |
| 12 | mean() | Returns the arithmetic average of all values in teradataml DataFrame or ColumnExpression over the specified window. |
| 13 | min() | Returns the minimum of values in teradataml DataFrame or ColumnExpression over the specified window. |
| 14 | percent_ rank() | Returns the relative rank of all the rows in a teradataml DataFrame or ColumnExpression, according to "order_columns", over the specified window. |
| 15 | rank() | Returns the rank (1 … n) of all the rows in a teradataml DataFrame or ColumnExpression, according to "order_columns", over the specified window. |
| 16 | regr_avgx() | Returns the mean of the independent variable for all non-null data pairs of the dependent and an independent variable arguments over the specified window. |
| 17 | regr_avgy() | Returns the mean of the dependent variable for all non-null data pairs of the dependent and independent variable arguments over the specified window. |
| 18 | regr_count() | Returns the column-wise count of all non-null data pairs of the dependent and independent variable arguments over the specified window. |
| 19 | regr_ intercept() non-null data pairs in the sample intersects the ordinate, or y-axis, of the graph. | Returns the intercept of the univariate linear regression line through all non-null data pairs of the dependent and independent variable arguments over the specified window. The intercept is the point at which the regression line through the |
| 20 regr_r2() | Returns the coefficient of determination for all non-null data pairs of the dependent | and independent variable arguments over the specified window. |
| 21 | regr_slope() window. When function is executed, "expression" is treated as an independent variable and dependent variable is: * a ColumnExpression when invoked using a window created * all columns of the teradataml DataFrame which are valid for this function, when executed on a window created on teradataml DataFrame. | Returns the slope of the univariate linear regression line through all non-null data pairs of the dependent and an independent variable arguments over the specified on ColumnExpression. |
| 22 regr_sxx() | Returns the sum of the squares of the independent variable expression for all non-null data pairs of dependent and an independent variable arguments over the specified window. When function is executed, "expression" is treated as an independent variable and dependent variable is: * a ColumnExpression when invoked using a window created * all columns of the teradataml DataFrame which are valid for this function, when executed on a window created on teradataml DataFrame. | on ColumnExpression. |
| 23 regr_sxy() | Returns the sum of the products of the independent variable and the dependent variable for all non arguments over the specified window. When function is executed, "expression" is treated as an independent variable and dependent variable is: | ‑ null data pairs of the dependent and independent variable |
|  |  | * a ColumnExpression when invoked using a window created on ColumnExpression. * all columns of the teradataml DataFrame which are valid for this function, when executed on a window created on teradataml DataFrame. |
| 24 | regr_syy() when executed on a window created on teradataml DataFrame. | Returns the sum of the squares of the dependent variable expression for all non-null data pairs of dependent and an independent variable arguments over the specified window. When function is executed, "expression" is treated as an independent variable and dependent variable is: * a ColumnExpression when invoked using a window created on ColumnExpression. * all columns of the teradataml DataFrame which are valid for this function, |
| 25 | row_number() | Returns the sequential row number, starting with first row as number one, for all the rows in a teradataml DataFrame or ColumnExpression, according to "order_ columns", over the specified window. |
| 26 | std() | Returns the standard deviation for the non-null data points in a teradataml DataFrame or ColumnExpression over the specified window. The standard deviation is the second moment of either a sample or population. For a population, it is a measure of dispersion from the mean of that population. For a sample, it is a measure of dispersion from the mean of that sample. The computation is more conservative for the population standard deviation to minimize the effect of outliers on the computed value. |
| 27 | sum() | Returns the sum of values in a teradataml DataFrame or ColumnExpression over the specified window. |
| 28 | var() deviation. However, if parameter "population" is True, then the function calculates the variance of a population. Variance of a population is a measure of dispersion from the mean of that population. The computation is more conservative than that for the population standard deviation to minimize the effect of outliers on the computed value. | Returns the variance for the data points in a teradataml DataFrame or ColumnExpression over the specified window. By default calculates the variance of sample. Variance of a sample is a measure of dispersion from the mean of that sample. It is the square of the sample standard |