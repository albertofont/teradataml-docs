# DataFrame Metadata, Rotation, Saving, and Export

#### Example 5: Replace the value of 'masters' with '1' if value is 'yes' and with '0'
#### if value is 'no'
```python
>>> res = df.replace({'yes': 1, 'no': 0}, subset=["masters"])
>>> print(res)
masters   gpa     stats programming  admitted
id
15       1  4.00  Advanced    Advanced         1
7        1  2.33    Novice      Novice         1
22       1  3.46    Novice    Beginner         0
17       0  3.83  Advanced    Advanced         1
13       0  4.00  Advanced      Novice         1
38       1  2.65  Advanced    Beginner         1
26       1  3.57  Advanced    Advanced         1
5        0  3.44    Novice      Novice         0
34       1  3.85  Advanced    Beginner         0
40       1  3.95    Novice    Beginner         0
```
## DataFrame Metadata
You can retrieve the metadata of a DataFrame with the following methods and properties.
#### DataFrame Methods
info() Method
    Returns a summary of the DataFrame.
keys() Method
    Returns a list of column names of the DataFrame.
#### DataFrame Properties
columns Property
    Returns a list containing column names.
db_object_name Property
    Gets the underlying database object name on which DataFrame is created.
dtypes Property
    Returns column names and their corresponding types.
index Property
    Returns the index of the DataFrame, which corresponds to the primary index of the underlying
    Table or View.
shape Property
    Returns a tuple representing the dimensionality of the DataFrame.
size Property
    Returns a value representing the number of elements in the DataFrame.
tdtypes Property
    Prints the column names and corresponding teradatasqlalchemy types of a DataFrame.
Note:
These DataFrame methods and properties do not return a teradataml DataFrame.
### info() Method
The info() function prints a summary of the DataFrame.
Optional arguments:
* verbose specifies whether to print full summary or short summary.
  * If set to True, prints full summary.
  * If set to False, prints short summary.
* buf specifies the writable buffer to send the output to.
  By default, the output is sent to sys.stdout.
* max_cols specifies the maximum number of columns allowed for printing the full summary.
* null_counts specifies whether to show the non-null counts. Display the counts if null_counts is True,
  otherwise do not display the non-null counts.
#### Example 1: Print information on DataFrame
This example prints information for DataFrame "sales".
```python
>>> df = DataFrame("sales")
>>> df.info()
<class 'teradataml.dataframe.dataframe.DataFrame'>
Data columns (total 6 columns):
accounts              str
Feb                 float
Jan                   int
Mar                   int
Apr                   int
datetime    datetime.date
dtypes: str(1), float(1), int(3), datetime.date(1)
```
#### Example 2: Print a count of non-null values
This example set null_counts to True for a count of non-null values.
```python
>>> df.info(null_counts=True)
<class 'teradataml.dataframe.dataframe.DataFrame'>
Data columns (total 6 columns):
accounts    6 non-null str
Feb         6 non-null float
Jan         4 non-null int
Mar         4 non-null int
Apr         4 non-null int
datetime    6 non-null datetime.date
dtypes: str(1), float(1), int(3), datetime.date(1)
```
#### Example 3: Print a short summary
This example set verbose to False for a short summary.
```python
>>> df.info(verbose=False)
<class 'teradataml.dataframe.dataframe.DataFrame'>
Data columns (total 6 columns):
dtypes: str(1), float(1), int(3), datetime.date(1)
```
### keys() Method
Use the keys() method to get a list of column names.
Note:
The keys() method is identical to the columns Property.
#### Example
```python
>>> df = DataFrame("sales")
>>> df.keys()
['accounts', 'Feb', 'Jan', 'Mar', 'Apr', 'datetime']
```
### DataFrame Properties
* columns Property
* dtypes Property
* index Property
* shape Property
* size Property
* tdtypes Property
* db_object_name Property
#### columns Property
Use the columns property to get a list of column names.
Note:
The columns property is identical to the keys() Method.
#### Example
```python
>>> df = DataFrame("sales")
>>> df.columns
['accounts', 'Feb', 'Jan', 'Mar', 'Apr', 'datetime']
```
#### dtypes Property
Use the dtypes property to print the column names and column types of a DataFrame.
#### Example
```python
>>> df = DataFrame("sales")
>>> df.dtypes
accounts              str
Feb                 float
Jan                   int
Mar                   int
Apr                   int
datetime    datetime.date
```
#### index Property
Use the index property to retrieve the index of the teradataml DataFrame, which corresponds to the
primary index of the underlying Table or View.
In case the index is explicitly set using the DataFrame.set_index() method or by passing the index_label
parameter while creating the teradataml DataFrame, the property will return the value set by the user.
#### Example Prerequisite
Load the admissions_train dataset and create a teradataml DataFrame out of it.
```python
>>> load_example_data("dataframe","admissions_train")
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming  admitted
id
5       no  3.44    Novice      Novice         0
3       no  3.70    Novice    Beginner         1
1      yes  3.95  Beginner    Beginner         0
20     yes  3.90  Advanced    Advanced         1
8       no  3.60  Beginner    Advanced         1
25      no  3.96  Advanced    Advanced         1
18     yes  3.81  Advanced    Advanced         1
24      no  1.87  Advanced      Novice         1
26     yes  3.57  Advanced    Advanced         1
38     yes  2.65  Advanced    Beginner         1
```
#### Example 1: Retrieve the index
```python
>>> # Get the index_label
>>> df.index
['id']
```
#### Example 2: Set the index using the set_index() method and then retrieve
#### the index
Set the index for the teradataml DataFrame using the set_index() method.
```python
>>> # Set new index_label
>>> df = df.set_index(['id', 'masters'])
>>> df
gpa     stats programming  admitted
id masters
5  no       3.44    Novice      Novice         0
3  no       3.70    Novice    Beginner         1
1  yes      3.95  Beginner    Beginner         0
17 no       3.83  Advanced    Advanced         1
13 no       4.00  Advanced      Novice         1
32 yes      3.46  Advanced    Beginner         0
11 no       3.13  Advanced    Advanced         1
9  no       3.82  Advanced    Advanced         1
34 yes      3.85  Advanced    Beginner         0
24 no       1.87  Advanced      Novice         1
```
Retrieve the index.
```python
>>> # Get the index_label
>>> df.index
['id', 'masters']
```
#### shape Property
Use the shape property to retrieve the dimensionality of a teradataml DataFrame.
#### Example
```python
>>> df = DataFrame('sales')
>>> df
Feb   Jan   Mar   Apr  datetime
accounts
Alpha Co    210.0   200   215   250  04/01/2017
Red Inc     200.0   150   140  None  04/01/2017
Orange Inc  210.0  None  None   250  04/01/2017
Jones LLC   200.0   150   140   180  04/01/2017
Yellow Inc   90.0  None  None  None  04/01/2017
Blue Inc     90.0    50    95   101  04/01/2017
>>> df.shape
(6, 6)
```
#### size Property
Use the size property to retrieve the number of elements in a teradataml DataFrame.
#### Example
```python
>>> df = DataFrame('sales')
>>> df
Feb   Jan   Mar   Apr  datetime
accounts
Alpha Co    210.0   200   215   250  04/01/2017
Red Inc     200.0   150   140  None  04/01/2017
Orange Inc  210.0  None  None   250  04/01/2017
Jones LLC   200.0   150   140   180  04/01/2017
Yellow Inc   90.0  None  None  None  04/01/2017
Blue Inc     90.0    50    95   101  04/01/2017
>>> df.size
36
```
#### tdtypes Property
Use the tdtypes property to print the column names and corresponding teradatasqlalchemy types of
a DataFrame.
#### Example
```python
>>> df = DataFrame("sales")
>>> df.tdtypes
accounts    VARCHAR(length=20, charset='LATIN')
Feb                                     FLOAT()
Jan                                    BIGINT()
Mar                                    BIGINT()
Apr                                    BIGINT()
datetime                                 DATE()
```
#### db_object_name Property
Use the db_object_name property to get the underlying database object name on which DataFrame
is created.
#### Example
```python
>>> load_example_data("dataframe", "sales")
>>> df = DataFrame('sales')
>>> df.db_object_name
'"sales"'
```
## Data Rotation
* pivot()
* unpivot()
### pivot()
Use the pivot() function to rotate data from rows into columns to create easy-to-read DataFrames. The
function is useful for reporting purposes, as it allows to aggregate and rotate the data.
Required Arguments:
* columns: Specifies the columns or dictionary of columns with distinct column values used for pivoting
  the data.
  This argument is required when keyword arguments are not specified. Otherwise, it is optional.
  * When specified as a column, this function automatically extracts the distinct values for
    the column.
    For example: columns = df.qtr #, or columns = [df.qtr, dr.yr]
    Note:
    Distinct values 'Q1', 'Q2', 'Q3', and so on, are automatically extracted from column 'qtr'
    for pivoting.
  * When specified as dictionary:
    - Key is a column expression representing a column.
    - Value is a literal or list of literals, that is, values in a column or a teradataml DataFrame with only
    one column.
    Note:
    When value is a teradataml DataFrame, the order of output pivoted columns is not
    guaranteed. In this case, Teradata recommends not specifying returns argument, so column
    names are generated properly according to the order of records.
    When dictionary value contains literals,
▪ If  limit_combinations  is set to False, pivot is done based on each combination of the values
    specified for each key.
    ▪ Otherwise, combinations are restricted based on the index values of the list.
    Note:
    All values in dictionary must have the same length when limit_combinations is set to True.
    Example 1: columns={df.qtr: ['Q1', 'Q2'], df.year: [2001, 2002]}
    In this case, pivot is based on all the combinations of values as specified in the following, for
    columns 'qtr' and 'yr':
    - If limit_combinations is set to False: (Q1, 2001), (Q1, 2002), (Q2, 2001), and (Q2, 2002).
    - If limit_combinations is set to True: (Q1, 2001) and (Q2, 2002).
    Example 2: columns={df.qtr: quarter_df, df.year: [2001, 2002]}, where value passed to df.qtr
    is a teradataml DataFrame with one column.
    In this cases, function extracts the distinct values from the DataFrame and pivot is
    based on all the combinations of values or limited combination of values based on
    argument limit_combinations.
    Note:
    String type values in dictionary are not case sensitive. See the following example 2 for
    more details.
* aggfuncs: Specifies the column aggregate functions to be used for pivoting the data.
  For example:
  To pivot total of 'sales' column and average of 'cogs' + 'sales' columns in the DataFrame "df", specify
  this argument as:
  aggfuncs=[df.sales.sum(), (df.cogs+df.sales).avg()]
Optional Arguments:
* limit_combinations: Specifies whether to limit the number of combinations when columns argument is
  passed as a dictionary.
  Default value is False.
  When set to True, function limits the combinations one-to-one based on index of the values in list,
  hence all dictionary values must be a list of same length or a single literal.
  For example: df.pivot(columns={df.qtr: ['Q1', 'Q2'], df.year: [2001,
  2002]}, limit_combinations=True, ...)
  Pivot will be based on columns 'qtr' and 'yr' for values (Q1, 2001) and (Q2, 2002) only.
Note:
    This argument is ignored when columns is a ColumnExpression or a list of ColumnExpressions.
* margins: Specifies the aggregate operation to be performed on output columns.
  Aggregate operation to be performed must be passed as dictionary where:
  * Key is a string specifying aggregate operation to be performed.
  * Value is a tuple or list of tuples specifying the output column names as string. Columns specified
    in the tuple are used in aggregate operation performed.
  For example: If for the year 2001, following three aggregate columns are needed in the output:
  * Sum of sales in Q1 and Q2
  * Sum of sales in Q2 and Q3
  * Average of cogs for first three quarters
  margins can be specified as:
  margins={"SUM": [("Q12001_sales", "Q22001_sales"), ("Q22001_sales", "Q32001_sales")],
  "AVG": ("Q12001_cogs", "Q22001_cogs", "Q32001_cogs")}
  Note:
    * Supported aggregate functions are SUM, AVG, MIN and MAX.
    * If returns is specified, column names for margins are considered from returns clause.
* returns: Specifies the custom column names to be returned in the output DataFrame.
  If this argument is not specified, function internally generates the output column names.
  The number of column names must be same as columns generated by pivot. For example:
  * If columns={df.qtr: ['Q1', 'Q2'], df.year: [2001, 2002]}, aggfuncs=[df.sales.sum(), df.cogs.sum()]
    and limit_combination=False, then number of new columns in output will be: len(aggfuncs) *
    len(columns[df.year]) * len(columns[df.qtr]) or 2 * 2 * 2 = 8.
    Hence, returns must be list of string with 8 elements.
  * If in the previous example, limit_combination=True, then number of new columns is output will be:
    len(aggfuncs) * (len(columns[df.year]) OR len(columns[df.qtr])) or or 2 * 2 = 4.
    Hence, returns must be list of string with 4 elements.
  Note:
    All columns including columns in DataFrame which do not participate in pivot must be specified
    if all_columns is set to True.
* all_columns: Specifies whether returns argument must include the names of only pivot columns or
  all columns.
  * When set to True, all columns including columns in DataFrame which do not participate in pivot
    must be specified.
  * When set to False, only output columns excluding columns in DataFrame which do not participate
    in pivot must be specified.
    This is the default value.
* kwargs: Specifies the keyword argument to accept column name and column values as
  named arguments.
  For example: col1=df.year, col1_value=2001, col2=df.qtr, col2_value=['Q1', 'Q2', 'Q3']
  Note:
    * Either use columns argument or keyword arguments.
    * Format for column name arguments must be col1, col2,.. colN with values of
    type ColumnExpression.
    * Format for column value argument must be col1_value, col2_value,.. colN_value.
#### Example Setup
```python
# Create a teradataml DataFrame.
>>> load_example_data("teradataml", "star1")
>>> df = DataFrame("star1")
```
#### Example 1: Pivot data using all values in column 'qtr' and aggregate using sum
#### of column 'sales'
```python
>>> pivot_df = df.pivot(columns=df.qtr,
aggfuncs=df.sales.sum())
>>> print(pivot_df)
country state    yr  cogs  sum_sales_q2  sum_sales_q1  sum_sales_q3
0     USA    NY  2001  25.0           NaN          45.0           NaN
1  CANADA    ON  2001   0.0          10.0           NaN           NaN
2  CANADA    BC  2001   0.0           NaN           NaN          10.0
3     USA    CA  2002  20.0          50.0           NaN           NaN
4     USA    CA  2002  15.0           NaN          30.0           NaN
```
#### Example 2: Pivot data using columns 'yr' and 'qtr', aggregate using sum of
#### 'sales' and median of 'cogs' column
Limit combination of 'q1' with '2001' and 'q2' with '2002'.
```python
>>> pivot_df = df.pivot(columns={df.yr: [2001, 2002], df.qtr: ['Q1', 'Q2']},
aggfuncs=[df.sales.sum(), df.cogs.median()],
limit_combinations=True)
>>> print(pivot_df)
country state  sum_sales_2001_q1  median_cogs_2001_q1
sum_sales_2002_q2  median_cogs_2002_q2
0  CANADA    ON                NaN                  NaN
NaN                  NaN
1     USA    CA                NaN                  NaN
50.0                 20.0
2     USA    NY               45.0                 25.0
NaN                  NaN
3  CANADA    BC                NaN                  NaN
NaN                  NaN
```
#### Example 3: Get all possible column combinations
Get all possible column combinations when pivoting using 'yr' and 'qtr', aggregation using 'sales' and
'cogs' column.
```python
>>> pivot_df = df.pivot(columns={df.yr: [2001, 2002], df.qtr: ['q1', 'q2']},
aggfuncs=[df.sales.sum(), df.cogs.max()])
>>> print(pivot_df)
country state  sum_sales_2001_q1  max_cogs_2001_q1
sum_sales_2001_q2  max_cogs_2001_q2  sum_sales_2002_q1  max_cogs_2002_q1
sum_sales_2002_q2  max_cogs_2002_q2
0  CANADA    BC                NaN               NaN
NaN               NaN                NaN               NaN
NaN               NaN
1  CANADA    ON                NaN               NaN
10.0               0.0                NaN               NaN
NaN               NaN
2     USA    NY               45.0              25.0
NaN               NaN                NaN               NaN
NaN               NaN
3     USA    CA                NaN               NaN
NaN               NaN               30.0              15.0
50.0              20.0
```
#### Example 4: Customize the name of returned columns using returns argument
```python
>>> pivot_df = df.pivot(columns={df.yr:2001, df.qtr:['Q1', 'Q2']},
aggfuncs=[df.sales.sum(), (2*df.sales-1).max()],
returns=["Q1_2001_total_sales", "Q1_2001_total_cogs",
"Q2_2001_total_sales", "Q2_2001_total_cogs"])
>>> print(pivot_df)
country state  cogs  Q1_2001_total_sales  Q1_2001_total_cogs
Q2_2001_total_sales  Q2_2001_total_cogs
0     USA    NY  25.0                 45.0                89.0
NaN                 NaN
1  CANADA    ON   0.0                  NaN                 NaN
10.0                19.0
2  CANADA    BC   0.0                  NaN                 NaN
NaN                 NaN
3     USA    CA  20.0                  NaN                 NaN
NaN                 NaN
4     USA    CA  15.0                  NaN                 NaN
NaN                 NaN
```
#### Example 5: Customize the name of all output columns using returns and
all_columns arguments
```python
>>> pivot_df = df.pivot(columns={df.yr:2001, df.qtr:['Q1', 'Q2']},
aggfuncs=[df.sales.avg(), df.cogs.median()],
returns=["con", "st",
"Q1_2001_total_sales", "Q1_2001_total_cogs",
"Q2_2001_total_sales", "Q2_2001_total_cogs"],
all_columns=True)
>>> print(pivot_df)
con  st  Q1_2001_total_sales  Q1_2001_total_cogs
Q2_2001_total_sales  Q2_2001_total_cogs
0  CANADA  ON                  NaN                 NaN
10.0                 0.0
1     USA  CA                  NaN                 NaN
NaN                 NaN
2     USA  NY                 45.0                25.0
NaN                 NaN
3  CANADA  BC                  NaN                 NaN
NaN                 NaN
```
#### Example 6: Use keyword arguments to specify columns and values instead of
columns argument
Note:
Since returns is not specified, column names are generated by the function.
```python
>>> pivot_df = df.pivot(aggfuncs=[df.sales.avg(), ((2*df.sales)+
(df.cogs/2)).min()],
col1=df.yr,
col1_values=2001,
col2=df.qtr,
col2_values=['Q1', 'Q2'])
>>> print(pivot_df)
country state  avg_sales_2001_q1  min_sales_2_cogs_2_2001_q1
avg_sales_2001_q2  min_sales_2_cogs_2_2001_q2
0  CANADA    BC                NaN                         NaN
NaN                         NaN
1  CANADA    ON                NaN                         NaN
10.0                        20.0
2     USA    NY               45.0                       102.5
NaN                         NaN
3     USA    CA                NaN                         NaN
NaN                         NaN
```
#### Example 7: Find the median sales and mean cogs for first three quarters of
#### year 2001 using margins
```python
>>> pivot_df = df.pivot(columns={df.qtr: ['q1', 'q2', 'q3'], df.yr: [2001]},
aggfuncs=df.sales.median(),
margins={"SUM":
[("median_sales_q1_2001", "median_sales_q2_2001"),
("median_sales_q3_2001", "median_sales_q2_2001")],
"AVG":
[("median_sales_q1_2001", "median_sales_q2_2001"),
("median_sales_q2_2001", "median_sales_q1_2001")]})
>>> print(pivot_df)
country state  cogs  median_sales_q1_2001  median_sales_q2_2001
median_sales_q3_2001  sum_median_sales_q1_2001_median_sales_q2_2001
sum_median_sales_q3_2001_median_sales_q2_2001
avg_median_sales_q1_2001_median_sales_q2_2001
avg_median_sales_q2_2001_median_sales_q1_2001
0  CANADA    BC   0.0                   NaN                   NaN
10.0                                            NaN
10.0                                            NaN
NaN
1     USA    NY  25.0                  45.0                   NaN
NaN                                           45.0
NaN                                           45.0
45.0
2  CANADA    ON   0.0                   NaN                  10.0
NaN                                           10.0
10.0                                           10.0
10.0
3     USA    CA  20.0                   NaN                   NaN
NaN                                            NaN
NaN                                            NaN
NaN
4     USA    CA  15.0                   NaN                   NaN
NaN                                            NaN
NaN                                            NaN
NaN
```
#### Example 8: Find the min of sales and average of cogs for quarter 'Q1', 'Q2' of
#### year '2001'
Name the aggregated column as 'margins'.
```python
>>> pivot_df = df.pivot(columns={df.qtr: ['q1', 'q2'], df.yr: [2001]},
aggfuncs=[df.cogs.min(), df.sales.avg()],
margins={"MIN":
("cogs_min_q12001", "sales_var_samp_q22001")},
all_columns=True,
returns=["con",
"st",
"cogs_min_q12001",
"sales_var_samp_q12001",
"cogs_min_q22001",
"sales_var_samp_q22001",
"margins"])
>>> print(pivot_df)
con  st  cogs_min_q12001  sales_var_samp_q12001  cogs_min_q22001
sales_var_samp_q22001  margins
0  CANADA  BC              NaN                    NaN
NaN                    NaN      NaN
1  CANADA  ON              NaN                    NaN
0.0                   10.0     10.0
2     USA  NY             25.0                   45.0
NaN                    NaN     25.0
3     USA  CA              NaN                    NaN
NaN                    NaN      NaN
```
#### Example 9: Specify a teradataml DataFrame with one column for values of
#### 'qtr' column, and then pivot the data
Note:
This allows user to implicitly use all the distinct values for the pivot operation and not specify
those explicitly.
```python
>>> quarters_df = df.drop(columns=['country', 'state', 'yr', 'sales', 'cogs'])
>>> print(quarters_df)
qtr
0  Q1
1  Q2
2  Q3
3  Q2
4  Q1
>>> pivot_df = df.pivot(columns={df.qtr: quarters_df},
aggfuncs=[df.sales.sum(), df.cogs.avg()])
>>> print(pivot_df)
country state    yr  sum_sales_q2  avg_cogs_q2  sum_sales_q1  avg_cogs_q1
sum_sales_q3  avg_cogs_q3
0  CANADA    ON  2001          10.0          0.0           NaN
NaN           NaN          NaN
1  CANADA    BC  2001           NaN          NaN           NaN
NaN          10.0          0.0
2     USA    NY  2001           NaN          NaN          45.0
25.0           NaN          NaN
3     USA    CA  2002          50.0         20.0          30.0
15.0           NaN          NaN
```
### unpivot()
Use the unpivot() function to rotate data from columns into rows to create easy-to-read DataFrames.
Required Arguments:
* columns: Specifies the dictionary of columns with distinct column values used for unpivoting the data.
  This argument is required when keyword arguments are not specified. Otherwise, it is optional.
  * Key is a column expression or tuple of column expressions.
  * Value is a literal to be taken by column when transposed into transpose_column. If not specified,
    value is generated by the function based on column names.
  * Number of columns specified in each key must be equal.
  For example:
```python
columns={(df.Q101Sales, df.Q101Cogs): "2001_Q1",
(df.Q201Sales, df.Q201Cogs): None,
(df.Q301Sales, df.Q301Cogs): "2001_Q3"},
transpose_column="yr_qtr",
```
  This example does the following:
  * Transposes columns 'Q101Sales' and 'Q101Cogs' into a row where 'yr_qtr' column value
    is '2001_Q1';
  * Transposes columns 'Q301Sales' and 'Q301Cogs' into a row with '2001_Q3' value in
    'yr_qtr' column;
  * For columns 'Q201Sales' and 'Q201Cogs', value of 'yr_qtr' column is function generated.
* transpose_column: Specifies the name of the column in the output DataFrame, which holds the data
  for columns specified in the keys of columns argument.
* measure_columns: Specifies the names of output columns to unpivot the data in the columns specified
  in the columns argument.
  Note:
    * The number of columns specified in measure_columns must be equal to the number of
    columns specified in each key of columns.
    * One exception for this requirement is when all the columns is to be unpivoted into a single
    column. In such case, measure_columns must be specified as a string or list of string with
    one value.
Optional Arguments:
* exclude_nulls: Specifies whether to exclude NULL (None) values while unpivoting the data. When set
  to True, excludes NULL (None), otherwise includes NULL.
  Default value is True.
* returns: Specifies the custom column names to be returned in the output DataFrame.
  Note:
    * The number of column names must be equal to the value in the measure_column argument
    plus one.
    * All columns including columns in DataFrame which do not participate in unpivot must be
    specified if all_columns is set to True.
* all_columns: Specifies whether returns argument must include the names of only unpivot columns or
  all columns.
  * When set to True, all columns including columns in DataFrame which do not participate in unpivot
    must be specified.
  * When set to False, which is the default value, only output columns excluding columns in
    DataFrame which do not participate in unpivot must be specified.
* kwargs: Specifies the keyword argument to accept column name and column values as
  named arguments.
  For example:
```python
col1=df.year, col1_value=2001, col2=df.qtr, col2_value='Q1'
```
  Or
```python
col1=(df.Q101Sales, df.Q101Cogs), col1_value="2001_Q1",
col2=(df.Q201Sales, df.Q201Cogs), col2_value=None,
col3=(df.Q301Sales, df.Q301Cogs), col3_value="2001_Q3"
```
  Note:
    * Either use columns argument or keyword arguments.
    * Format for column name arguments must be col1, col2,.. colN with values of
    type ColumnExpression.
    * Format for column value argument must be col1_value, col2_value,.. colN_value.
#### Example Setup
```python
# Create a teradataml DataFrame.
>>> load_example_data("teradataml", "star1")
>>> df = DataFrame("star1")
>>> df = df.pivot(columns={df.qtr: ["Q1", "Q2", "Q3"], df.yr: ["2001"]},
aggfuncs=[df.sales.sum(), df.cogs.sum()],
returns=["Q101Sales", "Q201Sales", "Q301Sales",
"Q101Cogs", "Q201Cogs", "Q301Cogs"])
>>> print(df)
country state  Q101Sales  Q201Sales  Q301Sales  Q101Cogs  Q201Cogs  Q301Cogs
0  CANADA    BC        NaN        NaN        NaN       NaN      10.0       0.0
1  CANADA    ON        NaN        NaN       10.0       0.0       NaN       NaN
2     USA    NY       45.0       25.0        NaN       NaN       NaN       NaN
3     USA    CA        NaN        NaN        NaN       NaN       NaN       NaN
```
#### Example 1: Unpivot quarterly sales data to 'sales' column and quarterly cogs
#### data to 'cogs' columns
This example unpivots quarterly sales data to 'sales' column and quarterly cogs data to 'cogs' columns
using columns argument.
```python
>>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs): "2001_Q1",
(df.Q201Sales, df.Q201Cogs): None,
(df.Q301Sales, df.Q301Cogs): "2001_Q3"},
transpose_column="yr_qtr",
measure_columns=["sales", "cogs"])
>>> print(unpivot_df)
country state              yr_qtr  sales  cogs
0  CANADA    BC  Q201Sales_Q201Cogs    NaN  10.0
1  CANADA    ON             2001_Q1    NaN   0.0
2  CANADA    ON             2001_Q3   10.0   NaN
3  CANADA    BC             2001_Q3    NaN   0.0
4     USA    NY  Q201Sales_Q201Cogs   25.0   NaN
5     USA    NY             2001_Q1   45.0   NaN
```
#### Example 2: Unpivot 'sales' and 'cogs' into a single column 'sales_cogs'
```python
>>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs,
df.Q201Sales, df.Q201Cogs,
df.Q301Sales, df.Q301Cogs): None},
transpose_column="yr_qtr",
measure_columns="sales_cogs")
>>> print(unpivot_df)
country state     yr_qtr  sales_cogs
0  CANADA    BC   Q201Cogs        10.0
1  CANADA    ON   Q101Cogs         0.0
2  CANADA    ON  Q301Sales        10.0
3  CANADA    BC   Q301Cogs         0.0
4     USA    NY  Q201Sales        25.0
5     USA    NY  Q101Sales        45.0
```
#### Example 3: Unpivot quarterly sales data to 'sales' column and quarterly cogs
#### data to 'cogs' columns
This example unpivots quarterly sales data to 'sales' column and quarterly cogs data to 'cogs' columns
using keyword arguments.
```python
>>> unpivot_df = df.unpivot(transpose_column="yr_qtr",
measure_columns=["sales", "cogs"],
col1 = (df.Q101Sales, df.Q101Cogs),
col1_value = "Q101",
col2 = (df.Q201Sales, df.Q201Cogs),
col2_value = None,
col3 = (df.Q301Sales, df.Q301Cogs),
col3_value = "Q103")
>>> print(unpivot_df)
country state              yr_qtr  sales  cogs
0  CANADA    BC  Q201Sales_Q201Cogs    NaN  10.0
1  CANADA    ON                Q101    NaN   0.0
2  CANADA    ON                Q103   10.0   NaN
3  CANADA    BC                Q103    NaN   0.0
4     USA    NY  Q201Sales_Q201Cogs   25.0   NaN
5     USA    NY                Q101   45.0   NaN
```
#### Example 4: Unpivot quarterly sales and quarterly cogs data and rename
#### column of the output using one argument
This example unpivots quarterly sales data to 'sales' column and quarterly cogs data to 'cogs' columns, and
rename column of the output using the returns argument.
```python
>>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs): "Q101",
(df.Q201Sales, df.Q201Cogs): None,
(df.Q301Sales, df.Q301Cogs): "Q301"},
transpose_column="yr_qtr",
measure_columns=["sales", "cogs"],
returns=["year_quarter", "sales_data", "cogs_data"])
>>> print(unpivot_df)
country state        year_quarter  sales_data  cogs_data
0  CANADA    BC  Q201Sales_Q201Cogs         NaN       10.0
1  CANADA    ON                Q101         NaN        0.0
2  CANADA    ON                Q301        10.0        NaN
3  CANADA    BC                Q301         NaN        0.0
4     USA    NY  Q201Sales_Q201Cogs        25.0        NaN
5     USA    NY                Q101        45.0        NaN
```
#### Example 5: Unpivot quarterly sales and quarterly cogs data and rename each
#### column of the output using two arguments
This example unpivots quarterly sales data to 'sales' column and quarterly cogs data to 'cogs' columns, and
rename each column of the output using returns and all_columns arguments.
```python
>>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs): "Q101",
(df.Q201Sales, df.Q201Cogs): None,
(df.Q301Sales, df.Q301Cogs): "Q301"},
transpose_column="yr_qtr",
measure_columns=["sales", "cogs"],
returns=["con", "st", "year_quarter",
"sales_data", "cogs_data"],
all_columns=True)
>>> print(unpivot_df)
con  st        year_quarter  sales_data  cogs_data
0  CANADA  BC  Q201Sales_Q201Cogs         NaN       10.0
1  CANADA  ON                Q101         NaN        0.0
2  CANADA  ON                Q301        10.0        NaN
3  CANADA  BC                Q301         NaN        0.0
4     USA  NY  Q201Sales_Q201Cogs        25.0        NaN
5     USA  NY                Q101        45.0        NaN
```
#### Example 6: Unpivot quarterly sales data to 'sales' column and quarterly cogs
#### data to 'cogs' columns, including NULLs
```python
>>> unpivot_df = df.unpivot(columns={(df.Q101Sales, df.Q101Cogs): "2001_Q1",
(df.Q201Sales, df.Q201Cogs): None,
(df.Q301Sales, df.Q301Cogs): "2001_Q3"},
transpose_column="yr_qtr",
measure_columns=["sales", "cogs"],
exclude_nulls=False)
>>> print(unpivot_df)
country state              yr_qtr  sales  cogs
0     USA    CA             2001_Q3    NaN   NaN
1     USA    NY  Q201Sales_Q201Cogs   25.0   NaN
2     USA    NY             2001_Q3    NaN   NaN
3  CANADA    BC             2001_Q1    NaN   NaN
4  CANADA    BC             2001_Q3    NaN   0.0
5  CANADA    ON             2001_Q1    NaN   0.0
6  CANADA    BC  Q201Sales_Q201Cogs    NaN  10.0
7     USA    NY             2001_Q1   45.0   NaN
8     USA    CA  Q201Sales_Q201Cogs    NaN   NaN
9     USA    CA             2001_Q1    NaN   NaN
```
## Saving teradataml DataFrames to Vantage
* create_temp_view()
* materialize()
* to_sql() DataFrame Method
### create_temp_view()
Use create_temp_view() to create a temporary view for session on the DataFrame.
Raises OperationalError (when view already exists).
Required argument:
* name specifies the name of the temporary view.
  Types: str
#### Example Setup
Load the data to run the example.
```python
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming  admitted
id
38     yes  2.65  Advanced    Beginner         1
7      yes  2.33    Novice      Novice         1
26     yes  3.57  Advanced    Advanced         1
17      no  3.83  Advanced    Advanced         1
34     yes  3.85  Advanced    Beginner         0
13      no  4.00  Advanced      Novice         1
32     yes  3.46  Advanced    Beginner         0
11      no  3.13  Advanced    Advanced         1
15     yes  4.00  Advanced    Advanced         1
36      no  3.00  Advanced      Novice         0
```
#### Example 1: Create view 'new_admissions'
```python
>>> df.create_temp_view("new_admissions")
>>> new_df = DataFrame("new_admissions")
>>> new_df
masters   gpa     stats programming  admitted
id
38     yes  2.65  Advanced    Beginner         1
7      yes  2.33    Novice      Novice         1
26     yes  3.57  Advanced    Advanced         1
17      no  3.83  Advanced    Advanced         1
34     yes  3.85  Advanced    Beginner         0
13      no  4.00  Advanced      Novice         1
32     yes  3.46  Advanced    Beginner         0
11      no  3.13  Advanced    Advanced         1
15     yes  4.00  Advanced    Advanced         1
36      no  3.00  Advanced      Novice         0
```
### materialize()
Use materialize() function to persist a teradataml dataframe, created for lazy operations on a teradataml
DataFrame, into a database for the current session, in the form of a view.
#### Example Setup
```python
>>> load_example_data("dataframe", "admissions_train")
>>> df = DataFrame("admissions_train")
>>> df
masters   gpa     stats programming  admitted
id
13      no  4.00  Advanced      Novice         1
26     yes  3.57  Advanced    Advanced         1
5       no  3.44    Novice      Novice         0
19     yes  1.98  Advanced    Advanced         0
15     yes  4.00  Advanced    Advanced         1
40     yes  3.95    Novice    Beginner         0
7      yes  2.33    Novice      Novice         1
22     yes  3.46    Novice    Beginner         0
36      no  3.00  Advanced      Novice         0
38     yes  2.65  Advanced    Beginner         1
```
#### Example 1: Perform lazy DataFrame operation and manually materialize
#### the view
```python
>>> df2 = df.get([["id", "masters", "gpa"]])
```
Initially, the table name will be None.
```python
>>> df2._table_name
>>> df2.materialize()
masters   gpa
id
15     yes  4.00
7      yes  2.33
22     yes  3.46
17      no  3.83
13      no  4.00
38     yes  2.65
26     yes  3.57
5       no  3.44
34     yes  3.85
40     yes  3.95
```
After materialize(), view name will be assigned.
```python
>>> df2._table_name
'"ALICE"."ml__select__172077355985236"'
>>>
```
### to_sql() DataFrame Method
Use the to_sql() DataFrame method to create a table in Vantage based on a teradataml DataFrame.
The function takes a table name as an argument, and generates DDL and DML queries that creates a table
in Vantage. You can also specify the name of the schema in the database to write the table to. If no schema
name is specified, the default database schema is used.
Required argument:
* table_name: Specifies the name of the table to be created in Vantage.
Optional arguments:
* schema_name: Specifies the name of the SQL schema in Vantageto write to. The default value is
  None, which means the default Vantage schema.
Note:
    schema_name is ignored when temporary is True.
* if_exists: Specifies the action to take when table already exists in the database. The possible
  values are:
  * 'fail': If table exists, do nothing;
  * 'replace': If table exists, drop it, recreate it, and insert data;
  * 'append': If table exists, insert data. Create if does not exist. This is the default value.
  Note:
    Replacing a table with the content of a teradataml DataFrame based on the same underlying
    table is not supported.
* primary_index: Creates Vantage tables with Primary index column. The value can be a single column
  name or a list of columns names. when primary_index is not specified, "No Primary Index Vantage
  tables are created.
* temporary: Specifies whether to create a permanent table or volatile table. Volatile tables only exist for
  the lifetime of the session. Volatile tables automatically go away when the session ends. The possible
  values are:
  * True: Creates a volatile table;
  * False: Creates a permanent table. This is the default value.
* types: Specifies required data types for requested columns to be saved in Vantage.
* primary_time_index_name: Specifies a name for the Primary Time Index (PTI) when the table to be
  created must be a PTI table.
  Note:
    This argument is not required or used when the table to be created is not a PTI table. It is ignored
    if specified without the timecode_column.
* timecode_column: Specifies the column in the DataFrame that reflects the form of the timestamp data
  in the time series.
  Note:
    This argument is required when the DataFrame must be saved as a PTI table. This argument is
    not required or used when the table to be created is not a PTI table. If this argument is specified,
    primary_index is ignored.
* timezero_date: Specifies the earliest time series data that the PTI will accept; a date that precedes the
  earliest date in the time series data. Default value is DATE '1970-01-01'.
Note:
    This argument is used when the DataFrame must be saved as a PTI table. This argument is not
    required or used when the table to be created is not a PTI table. It is ignored if specified without
    the timecode_column.
* timebucket_duration: Specifies a duration that serves to break up the time continuum in the time series
  data into discrete groups or buckets.
  Note:
    This argument is required if columns_list is not specified or is empty. It is used when the
    DataFrame must be saved as a PTI table. This argument is not required or used when the table
    to be created is not a PTI table. It is ignored if specified without the timecode_column.
* column_list: Specifies a list of one or more PTI table column names.
  Note:
    This argument is required if timebucket_duration not specified. It is used when the DataFrame
    must be saved as a PTI table. This argument is not required or used when the table to be created
    is not a PTI table. It is ignored if specified without the timecode_column.
* sequence_column: Specifies the column of type Integer containing the unique identifier for time series
  data reading when they are not unique in time.
  * When specified, implies SEQUENCED, meaning more than one reading from the same sensor
    may have the same timestamp. This column will be the TD_SEQNO column in the table created.
  * When not specified, implies NONSEQUENCED, meaning there is only one sensor reading per
    timestamp. This is the default.
  Note:
    This argument is used when the DataFrame must be saved as a PTI table. This argument is not
    required or used when the table to be created is not a PTI table. It will be ignored if specified
    without the timecode_column.
* seq_max: Specifies the maximum number of sensor data rows that can have the same timestamp.
  Can be used when 'sequenced' is True.
  Note:
    This argument is used when the DataFrame must be saved as a PTI table. This argument is not
    required or used when the table to be created is not a PTI table. It will be ignored if specified
    without the timecode_column.
* set_table: Specifies a flag to determine whether to create a SET or a MULTISET table.
* When True, a SET table is created.
  * When False, a MULTISET table is created. This is the default value.
  Note:
    * Specifying set_table=True also requires specifying primary_index or timecode_column.
    * Creating SET table (set_table=True) may result in loss of duplicate rows.
    * This argument has no effect if the table already exists and if_exists='append'.
#### Example 1: Create a NOPI table with selected columns from existing table
This example uses the to_sql() function to create a new table "sales_df1" based on the existing table
"sales" in Teradata Vantage. The new table "sales_df1" includes only the columns "accounts", "Jan",
and "Feb.
* Create a teradataml DataFrame "df" from the existing table "sales".
```python
df = DataFrame("sales")
>>> df
Feb    Jan    Mar    Apr    datetime
accounts
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Red Inc     200.0  150.0  140.0    NaN  04/01/2017
```
* Create a new dataframe with only the columns "accounts", "Jan", "Feb".
```python
>>> df = df.select(["accounts", "Jan", "Feb"])
>>> df
accounts    Jan    Feb
0   Jones LLC  150.0  200.0
1    Blue Inc   50.0   90.0
2  Yellow Inc    NaN   90.0
3  Orange Inc    NaN  210.0
4    Alpha Co  200.0  210.0
5     Red Inc  150.0  200.0
```
* Creates a new table "sales_df1" in Vantage based on the new DataFrame. The table "sales_df1" is a
  NOPI table because the primary index was not specified.
```python
>>> df.to_sql("sales_df1")
```
* Verify that the new table "sales_df1" exists in Vantage using the DataFrame() function which creates
  a DataFrame based on an existing table.
```python
>>> df1 = DataFrame("sales_df1")
>>> df1
accounts   Jan    Feb
0    Blue Inc    50   90.0
1  Orange Inc  None  210.0
2     Red Inc   150  200.0
3  Yellow Inc  None   90.0
4   Jones LLC   150  200.0
5    Alpha Co   200  210.0
```
#### Example 2: Create a table using the optional parameter primary_index
This example uses the to_sql() function with the optional parameter "primary_index" to create a new table
"sales_df2" in Vantage with the primary index "accounts". Use the optional parameter "if_exists" to replace
the existing table "sales_df2" in Vantage
* Create a teradataml DataFrame "df" from the existing table "sales".
```python
df = DataFrame("sales")
```
* Create a new dataframe with only the columns "accounts", "datetime".
```python
>>> df = df.select(["accounts", "datetime"])
```
* Create a new table "sales_df2" with the primary index "accounts".
```python
>>> df.to_sql("sales_df2", if_exists="replace", primary_index="accounts")
```
* Verify the new table by creating a new DataFrame from the new table.
```python
>>> df2 = DataFrame("sales_df2")
>>> df2
datetime
accounts
Blue Inc    17/01/04
Orange Inc  17/01/04
Red Inc     17/01/04
Yellow Inc  17/01/04
Jones LLC   17/01/04
Alpha Co    17/01/04
```
#### Example 3: Use optional parameter "set_table" to create a SET
#### table "sales_subset_set"
* Create a teradataml DataFrame "df" from the existing table "sales".
```python
df = DataFrame("sales")
```
* Create a new dataframe with the columns "Apr", "datetime".
```python
>>> df_for_set = df.select(['Apr','datetime'])
>>> df_for_set
Apr    datetime
0   101  04/01/2017
1   250  04/01/2017
2   180  04/01/2017
3  None  04/01/2017
4   250  04/01/2017
5  None  04/01/2017
```
* Create a SET table "sales_subset_set" using the optional parameter "set_table".
```python
>>> df_for_set.to_sql('sales_subset_set',
primary_index='Apr', set_table=True)
```
* Verify the new table by creating a new DataFrame from the new table.
```python
>>> set_df = DataFrame('sales_subset_set')
>>> set_df
datetime
Apr
180  17/01/04
101  17/01/04
250  17/01/04
NaN  17/01/04
```
#### Example 4: Create a new Primary Time Index table "sales_pti".
* Create a teradataml DataFrame "df" from the existing table "sales".
```python
df = DataFrame("sales")
```
* Create a new Primary Time Index table "sales_pti".
```python
>>> df.to_sql('sales_pti',
timecode_column='datetime', columns_list='accounts')
```
* Verify the new table by creating a new DataFrame from the new table.
```python
>>> sales_pti= DataFrame('sales_pti')
>>> sales_pti
TD_TIMECODE    Feb   Jan   Mar   Apr
accounts
Blue Inc      17/01/04   90.0    50    95   101
Orange Inc    17/01/04  210.0  None  None   250
Red Inc       17/01/04  200.0   150   140  None
Yellow Inc    17/01/04   90.0  None  None  None
Jones LLC     17/01/04  200.0   150   140   180
Alpha Co      17/01/04  210.0   200   215   250
```
Note:
See also copy_to_sql().
## Exporting DataFrame to External Entities
* to_pandas() Method
* to_csv() Method
### to_pandas() Method
Use the to_pandas() function to creates pandas DataFrame from a teradataml DataFrame.
Note:
Column types of the resulting Pandas DataFrame depends on pandas.read_sql_query().
Returns the pandas dataframe with Decimal column types as float instead of object. If user want to datatype
to be object set argument "coerce_float" to False.
Optional arguments:
* index_column specifies column name or List of column names representing columns to be used as
  Pandas index.
Note:
    When the optional parameter index_column is provided, the specified column is used as the
    Pandas index. Otherwise, the index (if exists) of the teradataml DataFrame or the primary index
    of the database table is used as the Pandas index. The default integer index is used if none of
    these indexes exists.
* all_rows specifies whether all rows from the teradataml DataFrame is retrieved while creating a
  pandas DataFrame. The default is False.
* num_rows specifies the number of rows to retrieve randomly from the teradataml DataFrame while
  creating pandas DataFrame. The default is 99999.
  Note:
    This argument is ignored if all_rows is set to True.
* fastexport specifies whether FastExport protocol must be used while converting a teradataml
  DataFrame to a Pandas DataFrame.
  Possible values for this argument:
  * The default value is False. Which means, by default, FastExport protocol is not used while
    converting teradataml DataFrame to a Pandas DataFrame.
  * If the argument is set to True, FastExport protocol is used internally for data transfer.
  * If the argument is set to None, the approach is decided based on the number of rows requested
    by the user for extraction:
    ▪ If requested number of rows is greater than or equal to 100,000, then FastExport is used.
    ▪ If requested number of rows is less than 100,000, regular mode is used for data extraction.
  Note:
    * Teradata recommends using FastExport when number of rows in teradataml DataFrame is
    at least 100,000.
    To extract lesser number of rows, ignore this option and go with regular approach.
    Because FastExport opens multiple data transfer connections to the database, which is
    time consuming.
    * FastExport does not support all data types in Vantage.
    For example, tables with BLOB and CLOB type columns cannot be extracted.
    * FastExport cannot be used to extract data from a volatile or temporary table.
    * For best efficiency, do not use DataFrame.groupby() and DataFrame.sort()
    with FastExport.
See the FastExport section of https://pypi.org/project/teradatasql/ for more information about
  FastExport protocol through teradatasql driver.
* catch_errors_warnings specifies whether to catch errors and warnings, if any, raised by FastExport
  protocol while converting a teradataml DataFrame to a Pandas DataFrame.
  Note:
    This argument is ignored if fastexport argument is set to FALSE.
  When FastExport is used and this argument is set to True, to_pandas() returns a tuple containing:
  * Pandas DataFrame
  * Errors, if any, in a list thrown by FastExport
  * Warnings, if any, in a list thrown by FastExport
  When FastExport is used and this argument is set to False (the default value), to_pandas() prints the
  FastExport errors and warnings, if any, to the standard output.
* kwargs specifies keyword arguments.
  Arguments coerce_float, parse_dates, and open_sessions can be passed as keyword arguments.
  * coerce_float specifies a boolean to attempt to convert non-string, non-numeric objects to
    floating point.
  * parse_dates specifies columns to parse as dates.
  * open_sessions specifies the number of Teradata data transfer sessions to be opened for
    fastexport. This argument is only applicable in fastexport mode.
    Note:
    If open_sessions argument is not set, by default, the number of data transfer sessions
    opened by teradataml is the smaller of 8 and the number of available AMPs in Vantage.
  See the FastExport section of https://pypi.org/project/teradatasql/ for more information about number
  of data transfer session opened during fastexport.
  See https://pandas.pydata.org/docs/reference/api/pandas.read_sql.html for more information about
  the coerce_float and parse_dates arguments.
#### Example Setup
Assume a teradataml DataFrame "df" is created from a Vantage table "sales", using command:
```python
df = DataFrame("sales")
```
#### Example 1: Create a pandas DataFrame without specifying index
```python
>>> pandas_df = df.to_pandas()
>>> pandas_df
Feb   Jan   Mar   Apr    datetime
accounts
Alpha Co    210   200   215   250  2017-04-01
Blue Inc     90    50    95   101  2017-04-01
Yellow Inc   90  None  None  None  2017-04-01
Jones LLC   200   150   140   180  2017-04-01
Red Inc     200   150   140  None  2017-04-01
Orange Inc  210  None  None   250  2017-04-01
```
#### Example 2: Create a pandas DataFrame using index_column to set the index
#### to "Feb"
```python
>>> pandas_df = df.to_pandas(index_column = 'Feb')
>>> pandas_df
accounts   Jan   Mar   Apr    datetime
Feb
210    Alpha Co   200   215   250  2017-04-01
90     Blue Inc    50    95   101  2017-04-01
90   Yellow Inc  None  None  None  2017-04-01
200   Jones LLC   150   140   180  2017-04-01
200     Red Inc   150   140  None  2017-04-01
210  Orange Inc  None  None   250  2017-04-01
```
#### Example 3: Create a pandas DataFrame using a list of column names for a
#### multicolumn index
```python
>>> pandas_df = df.to_pandas(index_column = ['accounts', 'Feb'])
>>> pandas_df
Jan   Mar   Apr    datetime
accounts   Feb
Yellow Inc 90   None  None  None  2017-04-01
Alpha Co   210   200   215   250  2017-04-01
Jones LLC  200   150   140   180  2017-04-01
Orange Inc 210  None  None   250  2017-04-01
Blue Inc   90     50    95   101  2017-04-01
Red Inc    200   150   140  None  2017-04-01
```
#### Example 4: Create a pandas DataFrame using num_rows to limit the number of
#### rows to 3
```python
>>> pandas_df = df.to_pandas(index_column = 'Feb', num_rows = 3)
>>> pandas_df
accounts    Jan    Mar    Apr    datetime
Feb
90.0   Yellow Inc    NaN    NaN    NaN  2017-01-04
90.0     Blue Inc   50.0   95.0  101.0  2017-01-04
200.0     Red Inc  150.0  140.0    NaN  2017-01-04
```
#### Example 5: Create a pandas DataFrame using all_rows
```python
>>> pandas_df = df.to_pandas(all_rows = True)
>>> pandas_df
Feb    Jan    Mar    Apr    datetime
accounts
Red Inc     200.0  150.0  140.0    NaN  2017-01-04
Orange Inc  210.0    NaN    NaN  250.0  2017-01-04
Blue Inc     90.0   50.0   95.0  101.0  2017-01-04
Alpha Co    210.0  200.0  215.0  250.0  2017-01-04
Yellow Inc   90.0    NaN    NaN    NaN  2017-01-04
Jones LLC   200.0  150.0  140.0  180.0  2017-01-04
```
#### Example 6: Convert teradataml DataFrame to pandas DataFrame using
#### fastexport, printing errors, if any, on screen
This example prints errors and warnings, if any, on to the screen, as catch_errors_warnings argument is
not set.
```python
>>> pandas_df = df.to_pandas(fastexport = True)
Errors: []
Warnings: []
>>> pandas_df
Feb    Jan    Mar    Apr    datetime
accounts
Red Inc     200.0  150.0  140.0    NaN  2017-01-04
Orange Inc  210.0    NaN    NaN  250.0  2017-01-04
Blue Inc     90.0   50.0   95.0  101.0  2017-01-04
Yellow Inc   90.0    NaN    NaN    NaN  2017-01-04
Alpha Co    210.0  200.0  215.0  250.0  2017-01-04
Jones LLC   200.0  150.0  140.0  180.0  2017-01-04
```
#### Example 7: Convert teradataml DataFrame to pandas DataFrame using
#### fastexport, catching errors, if any
This example catches errors and warnings, if any, raised by fastexport, and returns a tuple.
```python
>>> pandas_df, err, warn = df.to_pandas(fastexport = True,
catch_errors_warnings = True)
# Print pandas df.
>>> pandas_df
Feb    Jan    Mar    Apr    datetime
accounts
Red Inc     200.0  150.0  140.0    NaN  2017-01-04
Orange Inc  210.0    NaN    NaN  250.0  2017-01-04
Blue Inc     90.0   50.0   95.0  101.0  2017-01-04
Yellow Inc   90.0    NaN    NaN    NaN  2017-01-04
Alpha Co    210.0  200.0  215.0  250.0  2017-01-04
Jones LLC   200.0  150.0  140.0  180.0  2017-01-04
# Print errors list.
>>> err
[]
# Print warnings list.
>>> warn
[]
```
#### Example 8: Convert teradataml DataFrame to pandas DataFrame using
#### fastexport by opening specific number of sessions
```python
>>> pandas_df = df.to_pandas(fastexport = True, open_sessions = 2)
Errors: []
Warnings: []
# Print pandas df.
>>> pandas_df
Feb    Jan    Mar    Apr    datetime
accounts
Red Inc     200.0  150.0  140.0    NaN  2017-01-04
Orange Inc  210.0    NaN    NaN  250.0  2017-01-04
Blue Inc     90.0   50.0   95.0  101.0  2017-01-04
Yellow Inc   90.0    NaN    NaN    NaN  2017-01-04
Alpha Co    210.0  200.0  215.0  250.0  2017-01-04
Jones LLC   200.0  150.0  140.0  180.0  2017-01-04
```
### to_csv() Method
Use the to_csv() function to export data from a teradataml DataFrame to CSV file.
Required argument:
* csv_file: Specifies the name of CSV file to export the data into.
Optional arguments:
* num_rows: Specifies the number of rows to retrieve from DataFrame.
  The default is 99999.
  Note:
    This argument is ignored if all_rows is set to True.
* all_rows: Specifies whether all rows from the teradataml DataFrame to be exported to the CSV file
  or not.
  The default is False.
* fastexport: Specifies whether FastExport protocol must be used while exporting data.
  Possible values for this argument:
  * The default value is False. Which means, by default, FastExport protocol is not used.
  * If the argument is set to True, data is exported using FastExport protocol.
  * If the argument is set to None, the approach is decided based on the number of rows to
    be exported:
    ▪ If requested number of rows is greater than or equal to 100,000, then FastExport is used.
    ▪ If requested number of rows is less than 100,000, regular mode is used.
  Note:
    * Teradata recommends using FastExport when number of rows in teradataml DataFrame is
    at least 100,000.
    To extract lesser number of rows, ignore this option and go with regular approach.
    Because FastExport opens multiple data transfer connections to the database, which is
    time consuming.
    * FastExport does not support all data types in Vantage.
    For example, tables with BLOB and CLOB type columns cannot be extracted.
    * FastExport cannot be used to extract data from a volatile or temporary table.
    * For best efficiency, do not use DataFrame.groupby() and DataFrame.sort()
    with FastExport.
See the FastExport section of https://pypi.org/project/teradatasql/ for more information about
  FastExport protocol through teradatasql driver.
* sep: Specifies a single character string used to separate fields in a CSV file, with default value ' , '.
* quotechar: Specifies a single character string used to quote fields in a CSV file, with default value ' "
  ' (double quote).
  Note:
    * sep and quotechar cannot be line feed ('\\n') or carriage return ('\\r').
    * sep and quotechar cannot be the same.
    * Length of sep and quotechar must be 1.
* catch_errors_warnings: Specifies whether to catch errors and warnings, if any, raised by FastExport
  protocol while exporting data.
  Note:
    This argument is ignored if fastexport argument is set to FALSE.
  When FastExport is used and this argument is set to True, to_csv() returns a tuple containing:
  * Errors, if any, in a list thrown by FastExport
  * Warnings, if any, in a list thrown by FastExport
* kwargs: Specifies keyword arguments.
  Argument open_sessions can be passed as keyword arguments.
  open_sessions specifies the number of data transfer sessions to be opened for fastexport. This
  argument is only applicable in fastexport mode.
  Note:
    If open_sessions argument is not set, by default, the number of data transfer sessions opened
    by teradataml is the smaller of 8 and the number of available AMPs in Vantage.
  See the FastExport section of https://pypi.org/project/teradatasql/ for more information about number
  of data transfer session opened during fastexport.
#### Example Setup
```python
# Create a teradataml DataFrame.
>>> load_example_data("dataframe","admissions_train")
>>> df = DataFrame("admissions_train")
```
#### Example 1: Export data from teradataml DataFrame into CSV with only
#### required argument
```python
>>> df.to_csv("export_to_csv_1.csv")
Data is successfully exported into export_to_csv_1.csv
```
#### Example 2: Export all rows from teradataml DataFrame into CSV using
#### FastExport protocol
```python
>>> df.to_csv("export_to_csv_2.csv", all_rows=True, fastexport=True)
Data is successfully exported into export_to_csv_2.csv
```
#### Example 3: Export 20 rows from teradataml DataFrame into CSV
```python
>>> df.to_csv("export_to_csv_3.csv", num_rows=20)
Data is successfully exported into export_to_csv_3.csv
```
#### Example 4: Export data from teradataml DataFrame into CSV with
#### additional settings
This example exports data from teradataml DataFrame into CSV using FastExport protocol by opening one
Teradata data transfer session and saves errors and warnings thrown by fastexport.
```python
>>> err, warn = df.to_csv("export_to_csv_4.csv", fastexport=True,
catch_errors_warnings=True, open_sessions=1 )
Data is successfully exported into export_to_csv_4.csv
>>>err
[]
>>>warn
[]
```
#### Example 5: Export data from teradataml DataFrame into CSV with specified
#### separator and quote character
This example exports data from teradataml DataFrame into CSV with '|' as field separator and single
quote(') as field quote character.
```python
>>>df.to_csv("export_to_csv_5.csv", sep="|", quotechar="'" )
```