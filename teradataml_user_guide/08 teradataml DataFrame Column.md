# teradataml DataFrame Column

**DataFrameColumn OR ColumnExpression:**
Columns of a DataFrame have methods along with functions that can be used in conjunction with a
DataFrame, for example, when filtering or using the  assign  method.
In this User Guide, we often use the term 'ColumnExpression' to address teradataml DataFrame Column.
* Access teradataml DataFrame Column
* teradataml DataFrame Column Manipulation
## Access teradataml DataFrame Column
To use teradataml DataFrame Column (also called ColumnExpression) in a filter, assign or join, you must
access the Column.
**There are two ways to access column:**
* Access column as DataFrame attribute:
* dataframe_object . column_name
* Access column like dictionary:
* dataframe_object [" column_name "]
**Note:**
If column name contains whitespace or special character, Teradata recommends accessing
ColumnExpression like dictionary.
### Example Setup
```python
from teradataml.dataframe.sql_functions import case
load_example_data("GLM", ["admissions_train"])
df = DataFrame("admissions_train")
print(df)
masters   gpa     stats programming  admitted
id
5       no  3.44    Novice      Novice         0
3       no  3.70    Novice    Beginner         1
```

```python
1      yes  3.95  Beginner    Beginner         0
20     yes  3.90  Advanced    Advanced         1
8       no  3.60  Beginner    Advanced         1
25      no  3.96  Advanced    Advanced         1
18     yes  3.81  Advanced    Advanced         1
24      no  1.87  Advanced      Novice         1
26     yes  3.57  Advanced    Advanced         1
38     yes  2.65  Advanced    Beginner         1
```
### Example 1: Access ColumnExpression as attribute and use the same as
### predicate for filter
```python
gpa = df.gpa
good_df = df[case([(gpa > 3.0, 'good'),
(gpa > 2.0, 'average')],
else_='bad') == 'good']
print(good_df)
masters   gpa     stats programming  admitted
id
13      no  4.00  Advanced      Novice         1
11      no  3.13  Advanced    Advanced         1
9       no  3.82  Advanced    Advanced         1
26     yes  3.57  Advanced    Advanced         1
3       no  3.70    Novice    Beginner         1
1      yes  3.95  Beginner    Beginner         0
20     yes  3.90  Advanced    Advanced         1
18     yes  3.81  Advanced    Advanced         1
5       no  3.44    Novice      Novice         0
32     yes  3.46  Advanced    Beginner         0
print(good_df.shape)
(35, 6)
```
### Example 2: Access ColumnExpression like dictionary and use the same to
### create a new DataFrame
This example accesses ColumnExpression like dictionary and uses the same to create a new DataFrame
with an additional 'rating' column using assign operation, with the same case construct used in the
previous example.

```python
gpa = df['gpa']
whens_df = df.assign(rating = case([(gpa > 3.0, 'good'),
(gpa > 2.0, 'average')],
else_='bad'))
print(whens_df)
masters   gpa     stats programming  admitted   rating
id
5       no  3.44    Novice      Novice         0     good
3       no  3.70    Novice    Beginner         1     good
1      yes  3.95  Beginner    Beginner         0     good
20     yes  3.90  Advanced    Advanced         1     good
8       no  3.60  Beginner    Advanced         1     good
25      no  3.96  Advanced    Advanced         1     good
18     yes  3.81  Advanced    Advanced         1     good
24      no  1.87  Advanced      Novice         1      bad
26     yes  3.57  Advanced    Advanced         1     good
38     yes  2.65  Advanced    Beginner         1  average
print(whens_df.shape)
(40, 7)
```
## teradataml DataFrame Column Manipulation
* isin()
* NA Checking: isna()/notna() and isnull()/notnull() Methods
* Type Casting: cast()
* SQL Functions
* String Manipulation
* Regular Aggregate Functions Supported by DataFrame Column
* Date Time Functions
* String Functions
* Regular Arithmetic Functions
* Bit Byte Manipulation Functions
* Regular Expression Functions
* Comparison Functions
* Trigonometric Functions
* Hyperbolic Functions

### isin()
Use the isin() function to check for the presence of values in a column.
**Required Argument:**
* values : Specifies the list of values to check for their presence in the column.
### Example Setup
```python
load_example_data("dataframe","admissions_train")
df = DataFrame('admissions_train')
df
masters   gpa     stats programming  admitted
id
15     yes  4.00  Advanced    Advanced         1
7      yes  2.33    Novice      Novice         1
22     yes  3.46    Novice    Beginner         0
17      no  3.83  Advanced    Advanced         1
13      no  4.00  Advanced      Novice         1
38     yes  2.65  Advanced    Beginner         1
26     yes  3.57  Advanced    Advanced         1
5       no  3.44    Novice      Novice         0
34     yes  3.85  Advanced    Beginner         0
40     yes  3.95    Novice    Beginner         0
```
### Example 1: Filter results where gpa values are in any of the specified values
```python
df[df.gpa.isin([4.0, 3.0, 2.0, 1.0, 3.5, 2.5, 1.5])]
masters  gpa     stats programming  admitted
id
31     yes  3.5  Advanced    Beginner         1
6      yes  3.5  Beginner    Advanced         1
13      no  4.0  Advanced      Novice         1
4      yes  3.5  Beginner      Novice         1
29     yes  4.0    Novice    Beginner         0
15     yes  4.0  Advanced    Advanced         1
36      no  3.0  Advanced      Novice         0
```

### Example 2: Filter results where the 'stats' values are neither 'Novice'
### nor 'Advanced'
```python
df[~df.stats.isin(['Novice', 'Advanced'])]
masters   gpa     stats programming  admitted
id
1      yes  3.95  Beginner    Beginner         0
2      yes  3.76  Beginner    Beginner         0
8       no  3.60  Beginner    Advanced         1
4      yes  3.50  Beginner      Novice         1
6      yes  3.50  Beginner    Advanced         1
```
### NA Checking: isna()/notna() and isnull()/notnull() Methods
Missing values in data can be handled by using the isna() or notna() methods.
Currently, the only NA value supported is None. 'isnull' and 'notnull' are aliases of 'isna' and 'notna'
respectively. Other possible NA values, +Inf, -Inf, and NaN (typically seen in floating point calculations) are
not supported. See  Limited Missing Value Support .
### Example: Handle missing values
```python
df = DataFrame('sales')
df
Feb   Jan   Mar   Apr    datetime
accounts
Alpha Co    210.0   200   215   250  04/01/2017
Blue Inc     90.0    50    95   101  04/01/2017
Yellow Inc   90.0  None  None  None  04/01/2017
Jones LLC   200.0   150   140   180  04/01/2017
Red Inc     200.0   150   140  None  04/01/2017
Orange Inc  210.0  None  None   250  04/01/2017
df.assign(drop_columns = True, January = df.Jan, NullJanuary
= df.Jan.isna())
January NullJanuary
0     200           0
1      50           0
2    None           1
3     150           0
```

```python
4     150           0
5    None           1
```
### Type Casting: cast()
Use the cast() method to cast a DataFrame column (Column Expression) to a specified
teradatasqlalchemy type. It can be used with the DataFrame methods  assign  and  filter .
**Required Argument:**
* type_ : Specifies a teradatasqlalchemy type or an object of a teradatasqlalchemy type that the column
* needs to be cast to.
* Default value is None.
* Types: teradatasqlalchemy type or object of teradatasqlalchemy type
**Optional Arguments:**
* format : Specifies a variable length string containing formatting characters that define the display
* format for the data type.
* Formats can be specified for columns that have character, numeric, byte, DateTime, Period or UDT
* data types.
* Note:
    * Teradata supports different formats. Refer to "Data Type Formats and Format Phrases" in  Data
    * Types and Literals , B035-1143.
* Default value: None
* Types: str
* timezone : Specifies the timezone string.
* Refer to  SQL Date and Time Functions and Expressions , B035-1211 for supported timezones.
* Type: ColumnExpression or str.
### Example Setup
```python
load_example_data("dataframe","admissions_train")
df = DataFrame('admissions_train')
df
masters   gpa     stats programming  admitted
id
13      no  4.00  Advanced      Novice         1
```

```python
26     yes  3.57  Advanced    Advanced         1
5       no  3.44    Novice      Novice         0
19     yes  1.98  Advanced    Advanced         0
15     yes  4.00  Advanced    Advanced         1
40     yes  3.95    Novice    Beginner         0
7      yes  2.33    Novice      Novice         1
22     yes  3.46    Novice    Beginner         0
36      no  3.00  Advanced      Novice         0
38     yes  2.65  Advanced    Beginner         1
df.dtypes
id               int
masters          str
gpa            float
stats            str
programming      str
admitted         int
```
### Example 1: Use assign to create a new DataFrame with a new column, casted
### to VARCHAR(5) type
Use assign() to create a new DataFrame with a new column 'char_id', which is the 'id' column
(of type INTEGER) cast to VARCHAR(5) (object of teradatasqlalchemy type corresponding to SQL
Type VARCHAR(5)).
```python
from teradatasqlalchemy import VARCHAR
new_df = df.assign(char_id = df.id.cast(type_=VARCHAR(5)))
new_df
masters   gpa     stats programming  admitted char_id
id
5       no  3.44    Novice      Novice         0       5
34     yes  3.85  Advanced    Beginner         0      34
13      no  4.00  Advanced      Novice         1      13
40     yes  3.95    Novice    Beginner         0      40
22     yes  3.46    Novice    Beginner         0      22
19     yes  1.98  Advanced    Advanced         0      19
36      no  3.00  Advanced      Novice         0      36
15     yes  4.00  Advanced    Advanced         1      15
7      yes  2.33    Novice      Novice         1       7
17      no  3.83  Advanced    Advanced         1      17
```

```python
new_df.dtypes
id               int
masters          str
gpa            float
stats            str
programming      str
admitted         int
char_id          str
```
### Example 2: Use assign to create a new DataFrame with a new column, casted
### to VARCHAR type
Use assign() to create a new DataFrame with a new column 'char_id', which is the 'id' column (of type
INTEGER) cast to VARCHAR (teradatasqlalchemy type corresponding to SQL Type VARCHAR).
```python
new_df_2 = df.assign(char_id = df.id.cast(type_=VARCHAR))
new_df_2
masters   gpa     stats programming  admitted char_id
id
5       no  3.44    Novice      Novice         0       5
34     yes  3.85  Advanced    Beginner         0      34
13      no  4.00  Advanced      Novice         1      13
40     yes  3.95    Novice    Beginner         0      40
22     yes  3.46    Novice    Beginner         0      22
19     yes  1.98  Advanced    Advanced         0      19
36      no  3.00  Advanced      Novice         0      36
15     yes  4.00  Advanced    Advanced         1      15
7      yes  2.33    Novice      Novice         1       7
17      no  3.83  Advanced    Advanced         1      17
new_df_2.dtypes
id               int
masters          str
gpa            float
stats            str
programming      str
admitted         int
char_id          str
```

### Example 3: Filter data with match on a column cast to another type
```python
df[df.id.cast(VARCHAR(5)) == '1']
masters   gpa     stats programming  admitted
id
1      yes  3.95  Beginner    Beginner         0
df[df.id.cast(VARCHAR) == '1']
masters   gpa     stats programming  admitted
id
1      yes  3.95  Beginner    Beginner         0
```
### Example 4: Create new DataFrame casting 'timestamp_col' column (of type
### VARCHAR) to TIMESTAMP, using format
```python
dataframe_dict = {"id": [100, 200,300],
"timestamp_col": ['1000-01-10 23:00:12-02:00', '2015-01-08 13:00:00+12:00',
'2014-12-10 10:00:35-08:00'],
"timezone_col": ["GMT", "America Pacific", "GMT+10"]}
pandas_df = pd.DataFrame(dataframe_dict)
copy_to_sql(pandas_df, table_name = 'new_table', if_exists = 'replace')
df1 = DataFrame("new_table")
df1
id   timestamp_col                    timezone_col
300  2014-12-10 10:00:35-08:00        GMT+10
200  2015-01-08 13:00:00+12:00        America Pacific
100  1000-01-10 23:00:12-02:00        GMT
df1.dtypes
id               int
timestamp_col    str
timezone_col     str
new_df1 = df1.assign(new_col = df1.timestamp_col.cast(TIMESTAMP, format='Y4-
MM-DDBHH:MI:SSBZ'))
```

```python
id   timestamp_col                    timezone_col         new_col
300  2014-12-10 10:00:35-08:00        GMT+10               2014-12-10 18:00:35
200  2015-01-08 13:00:00+12:00        America Pacific      2015-01-08 01:00:00
100  1000-01-10 23:00:12-02:00        GMT                  1000-01-11 01:00:12
new_df1.tdtypes
id              int
timestamp_col   str
timezone_col    str
new_col         datetime.datetime
```
### Example 5: Create new DataFrame casting 'id' column (of type INTEGER) to
### VARCHAR, using format
```python
new_df2 = df1.assign(new_col = df1.id.cast(VARCHAR, format='zzz.zz'))
id   timestamp_col              timezone_col      new_col
300  2014-12-10 10:00:35-08:00  GMT+10            300.00
200  2015-01-08 13:00:00+12:00  America Pacific   200.00
100  1000-01-10 23:00:12-02:00  GMT               100.00
new_df2.dtypes
id               int
timestamp_col    str
timezone_col     str
new_col          str
```
### Example 6: Create new DataFrame casting 'timestamp_with_timezone'
### column (of type TIMESTAMP) to TIMESTAMP WITH TIMEZONE, with
### offset 'GMT+10'
```python
new_df3 = new_df1.assign(timestamp_with_timezone =
new_df1.new_col.cast(TIMESTAMP(timezone=True), timezone='GMT+10'))
id   timestamp_col              timezone_col
new_col              timestamp_with_timezone
300  2014-12-10 10:00:35-08:00  GMT+10           2014-12-10 18:00:35
2014-12-11 04:00:35.000000+10:00
200  2015-01-08 13:00:00+12:00  America Pacific  2015-01-08 01:00:00
2015-01-08 11:00:00.000000+10:00
```

```python
100  1000-01-10 23:00:12-02:00  GMT              1000-01-11 01:00:12
1000-01-11 11:00:12.000000+10:00
new_df3.dtypes
id                         int
timestamp_col              str
timezone_col               str
new_col                    datetime.datetime
timestamp_with_timezone    datetime.datetime
```
### SQL Functions
* to_numeric
* case
* desc
* asc
* distinct
to_numeric
Use the to_numeric function to convert a string-like representation of a number to a numeric type. It can
be used with the string columns of the DataFrame in DataFrame  assign  method.
### Example Setup
Create a DataFrame with all string type columns.
```python
load_example_data("dataframe", "numeric_strings")
df = DataFrame('numeric_strings')
df
hex_col decimal_col commas_col numbers_col
id_col
2       ABCDEFAB       0.7.7       ,088         999
0           19FF       00.77       08,8           1
1           abcd        0.77       0,88           1
3           2018        .077       088,           0
df.dtypes
```

```python
id_col         int
hex_col        str
decimal_col    str
commas_col     str
numbers_col    str
```
### Example 1: Convert to Numeric Type
Except for the id column, the columns in the DataFrame are all string types. To use the DataFrame in a
numerical calculation, they first need to be converted to a numeric type.
```python
tdf = df.assign(drop_columns = True, numbers = df.numbers_col, numeric
= to_numeric(df.numbers_col))
tdf
numbers numeric
0       1       1
1     999     999
2       0       0
3       1       1
tdf.dtypes
numbers                str
numeric    decimal.Decimal
```
### Example 2: Use Optional  format_  Keyword when Converting
The to_numeric function may not be able to parse the string into a numeric value if the string has an
unrecognizable format. It returns None in this case.
This example converts decimal-like strings to numeric.
```python
df.assign(drop_columns = True, decimal = df.decimal_col, numeric_dec
= to_numeric(df.decimal_col))
decimal numeric_dec
0   0.7.7        None
1   00.77         .77
2    0.77         .77
3    .077        .077
```

You can control which strings are recognizable by passing a format string into the optional
format_  keyword.
This example converts comma (group separated) strings to numeric.
```python
df.assign(drop_columns = True, commas = df.commas_col, numeric_commas =
to_numeric(df.commas_col, format_ = '9G99'))
commas numeric_commas
0   ,088           None
1   08,8           None
2   0,88             88
3   088,           None
```
This example convert hex strings to numeric.
```python
df.assign(drop_columns = True, hex = df.hex_col, numeric_hex =
to_numeric(df.hex_col, format_ = 'XXXXXXXXXX'))
hex numeric_hex
0  ABCDEFAB  2882400171
1      19FF        6655
2      abcd       43981
3      2018        8216
```
The 'format' string follows the syntax of the to_number function in the Database Engine 20.
**Note:**
For more information, see the Data Type Conversion Functions section in the  SQL Functions,
Expressions, and Predicates , B035-1145.
### Example 3: Use String Literals as Arguments
The to_numeric function can take DataFrame columns or string literals as arguments.
This example converts literals to numeric.
```python
df.assign(drop_columns = True,
a = to_numeric('123,456',format_ = '999,999'),
b = to_numeric('1,333.555', format_ = '9,999D999'),
c = to_numeric('2,333,2',format_ = '9G999G9'),
d = to_numeric('3E20'),
e = to_numeric('$41.99', format_ = 'L99.99'),
f = to_numeric('$.12', format_ = 'L.99'),
```

```python
g = to_numeric('dollar123,456.00',
format_ = 'L999G999D99',
nls = {'param': 'currency',
'value': 'dollar'})).head(1)
a         b      c                         d      e    f       g
0  123456  1333.555  23332 300000000000000000000  41.99  .12  123456
```
case
Use the case() function to add SQL CASE logic based on DataFrame column expressions. It can be used
with DataFrame methods  assign  and  filter .
**Required Argument:**
* whens : Specifies the criteria to be compared against. It accepts two different forms, based on
* whether or not the  value  argument is used.
    * In the first form, it accepts a list of 2-tuples; each 2-tuple consists of  (<sql expression>,
```python
<value>) , where the <sql expression> is a boolean expression and 'value' is a resulting value.
```
    * For example:
```python
case([
(df.first_name == 'wendy', 'W'),
(df.first_name == 'jack', 'J')
])
```
    * In the second form, it accepts a Python dictionary of comparison values mapped to a resulting
    * value. This form requires 'value' argument to be present, and values are compared using the
    * '==' operator.
    * For example:
```python
case(
{"wendy": "W", "jack": "J"},
value=df.first_name
```
**Optional Arguments:**
* value : Specifies a SQL expression (ColumnExpression or literal) which is used as a fixed comparison
* point for candidate values within a dictionary passed to the  whens  argument.
* This argument is required when  whens  argument is of dictionary type.
* else_ : Specifies a SQL expression (ColumnExpression or literal) which is the evaluated result of the
* CASE construct if all expressions within  whens  evaluate to False.

* When this argument is omitted, function produces a result of NULL if none of the  when  expressions
* evaluate to True.
### Example Setup
```python
from teradataml.dataframe.sql_functions import case
load_example_data("GLM", ["admissions_train"])
df = DataFrame("admissions_train")
print(df)
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
print(df.shape)
(40, 6)
```
### Example: Run case() with 'whens' Passing 2-tuple
This example shows the  whens  argument passes a 2-tuple and filters only the 'good' records based on
**the rating:**
* - gpa > 3.0 : 'good'
* - 2.0 < gpa <=3.0 : 'average'
* - gpa <= 2.0 : 'bad'
```python
good_df = df[case([(df.gpa > 3.0, 'good'),
(df.gpa > 2.0, 'average')],
else_='bad') == 'good']
print(good_df)
```

```python
masters   gpa     stats programming  admitted
id
13      no  4.00  Advanced      Novice         1
11      no  3.13  Advanced    Advanced         1
9       no  3.82  Advanced    Advanced         1
26     yes  3.57  Advanced    Advanced         1
3       no  3.70    Novice    Beginner         1
1      yes  3.95  Beginner    Beginner         0
20     yes  3.90  Advanced    Advanced         1
18     yes  3.81  Advanced    Advanced         1
5       no  3.44    Novice      Novice         0
32     yes  3.46  Advanced    Beginner         0
print(good_df.shape)
(35, 6)
```
### Example: With Same Case Construct, Create New DataFrame with Additional
### 'rating' Column Using 'assign' Operation
```python
whens_df = df.assign(rating = case([(df.gpa > 3.0, 'good'),
(df.gpa > 2.0, 'average')],
else_='bad'))
print(whens_df)
masters   gpa     stats programming  admitted   rating
id
5       no  3.44    Novice      Novice         0     good
3       no  3.70    Novice    Beginner         1     good
1      yes  3.95  Beginner    Beginner         0     good
20     yes  3.90  Advanced    Advanced         1     good
8       no  3.60  Beginner    Advanced         1     good
25      no  3.96  Advanced    Advanced         1     good
18     yes  3.81  Advanced    Advanced         1     good
24      no  1.87  Advanced      Novice         1      bad
26     yes  3.57  Advanced    Advanced         1     good
38     yes  2.65  Advanced    Beginner         1  average
print(whens_df.shape)
(40, 7)
```

### Example: Run case() without  else_  argument
This example runs the case() function without specifying the  else_  argument, resulting NULLs when no
condition in the  whens  argument is met.
```python
no_else =  df.assign(rating = case([(df.gpa > 3.0, 'good')]))
print(no_else)
masters   gpa     stats programming  admitted  rating
id
5       no  3.44    Novice      Novice         0    good
3       no  3.70    Novice    Beginner         1    good
1      yes  3.95  Beginner    Beginner         0    good
20     yes  3.90  Advanced    Advanced         1    good
8       no  3.60  Beginner    Advanced         1    good
25      no  3.96  Advanced    Advanced         1    good
18     yes  3.81  Advanced    Advanced         1    good
24      no  1.87  Advanced      Novice         1    None
26     yes  3.57  Advanced    Advanced         1    good
38     yes  2.65  Advanced    Beginner         1    None
print(no_else.shape)
(40, 7)
```
### Example: Run case() with  whens  Passing Dictionary with Argument  value
This example shows the  whens  argument passing dictionary along with the argument  value . The result is
'admitted' when the df.admitted == 1, 'not admitted' when df.admitted == 0, and 'dont't know' otherwise.
```python
whens_value_df = df.assign(admitted_text = case({ 1 : "admitted", 0 :
"not admitted"},
value=df.admitted,
else_="don't know"))
print(whens_value_df)
masters   gpa     stats programming  admitted  admitted_text
id
13      no  4.00  Advanced      Novice         1       admitted
11      no  3.13  Advanced    Advanced         1       admitted
9       no  3.82  Advanced    Advanced         1       admitted
28      no  3.93  Advanced    Advanced         1       admitted
```

```python
33      no  3.55    Novice      Novice         1       admitted
10      no  3.71  Advanced    Advanced         1       admitted
16      no  3.70  Advanced    Advanced         1       admitted
32     yes  3.46  Advanced    Beginner         0   not admitted
34     yes  3.85  Advanced    Beginner         0   not admitted
17      no  3.83  Advanced    Advanced         1       admitted
print(whens_value_df.shape)
(40, 7)
```
### Example: Run case() Using Literal Column Name as SQL Expressions
This example uses literal column name as SQL expressions and chooses to emit values from two different
columns as a result of the same case expression. It shows how you can decide on projecting a column
based on the value of expression.
In the example, you project values from column 'average_rating' if 2.0 < gpa <= 3.0 and values from
column 'good_rating' when gpa > 3.0, and name the column 'ga_rating'.
* Load required library.
```python
from sqlalchemy.sql import literal_column
```
* Create a new DataFrame with a new column 'good_rating' which is set to 'good' only when gpa > 3.0,
* else NULL.
```python
whens_new_df = df.assign(good_rating = case([(df.gpa > 3.0, 'good')]))
```
* Add another column named 'avg_rating' to the DataFrame which is set to 'average' only when 2.0 <
* gpa <= 3.0, else NULL.
```python
whens_new_df = whens_new_df.assign(avg_rating =
case([((whens_new_df.gpa > 2.0) & (whens_new_df.gpa <= 3.0), 'average')]))
```
* Create a DataFrame with another column named 'ga_rating', which is set to the value from column
* 'good_rating' when gpa > 3.0, and value from column 'avg_rating' when gpa > 2.0, else NULL.
```python
literal_df = whens_new_df.assign(ga_rating = case([(whens_new_df.gpa
> 3.0, literal_column('good_rating')), (whens_new_df.gpa >
2.0, literal_column('avg_rating'))]))
print(literal_df)
masters   gpa     stats programming  admitted good_rating
avg_rating ga_rating
id
```

```python
5       no  3.44    Novice      Novice         0        good
None      good
3       no  3.70    Novice    Beginner         1        good
None      good
1      yes  3.95  Beginner    Beginner         0        good
None      good
20     yes  3.90  Advanced    Advanced         1        good
None      good
8       no  3.60  Beginner    Advanced         1        good
None      good
25      no  3.96  Advanced    Advanced         1        good
None      good
18     yes  3.81  Advanced    Advanced         1        good
None      good
24      no  1.87  Advanced      Novice         1        None
None      None
26     yes  3.57  Advanced    Advanced         1        good
None      good
38     yes  2.65  Advanced    Beginner         1        None
average   average
```
desc
Use the desc() function to generate a new ColumnExpression which sorts the actual expression in
descending order.
**Note:**
    * This function is supported only while sorting data.
    * This function is neither supported in projection nor supported in filtering data.
### Example
```python
from teradataml import load_example_data
load_example_data("dataframe","sales")
df = DataFrame.from_table('sales')
df.csum(sort_columns=[df.accounts, df.Feb.desc()], drop_columns=True)
```

```python
csum_Feb  csum_Jan  csum_Mar  csum_Apr
0     500.0       400       450       531
1     910.0       550       590       781
2    1000.0       550       590       781
3     710.0       400       450       781
4     300.0       250       310       351
5     210.0       200       215       250
```
asc
Use the asc() function to generate a new ColumnExpression which sorts the actual expression in
ascending order.
**Note:**
    * This function is supported only while sorting data.
    * This function is neither supported in projection nor supported in filtering data.
### Example
```python
from teradataml import load_example_data
load_example_data("dataframe","sales")
df = DataFrame.from_table('sales')
df.csum(sort_columns=[df.accounts, df.Feb.asc()], drop_columns=True)
csum_Feb  csum_Jan  csum_Mar  csum_Apr
0     380.0     200.0     235.0     281.0
1     790.0     350.0     375.0     531.0
2    1000.0     550.0     590.0     781.0
3     580.0     350.0     375.0     281.0
4     180.0      50.0      95.0     101.0
5      90.0       NaN       NaN       NaN
```
distinct
Use the distinct() function to generate a new ColumnExpression which removes duplicate rows while
processing the function.

**Note:**
    * This function is supported only in projection.
    * This function is neither supported in sorting data nor supported in filtering data.
### Example
```python
from teradataml import *
load_example_data("dataframe","sales")
df = DataFrame.from_table('sales')
df
Feb    Jan    Mar    Apr    datetime
accounts
Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
Red Inc     200.0  150.0  140.0    NaN  04/01/2017
df.assign(drop_columns=True, distinct_feb=df.Feb.distinct())
distinct_feb
0         210.0
1          90.0
2         200.0
```
### String Manipulation
DataFrame columns have methods that can be used to manipulate string-like data. These methods are
available by invoking the 'str' property. The 'str' property is only valid for string-like columns.
* contains() Method
* lower() Method
* strip() Method

contains() Method
Use the contains() method to test if the given regular expression pattern matches string values in
the column.
### Example 1: Test if column contains a given expression
```python
df = DataFrame('sales')
accounts = df['accounts']
df.assign(drop_columns = True, Accounts = accounts, has_llc
= accounts.str.contains('LLC'))
Accounts has_llc
0    Alpha Co       0
1    Blue Inc       0
2  Yellow Inc       0
3   Jones LLC       1
4     Red Inc       0
5  Orange Inc       0
```
### Example 2: Test using the  case  parameter
Uses the  case  parameter to toggle case-sensitive matching on or off. The default value is on.
```python
df = DataFrame('sales')
accounts = df['accounts']
```
Example 2.1: Test when  case  is set to True.
```python
df.assign(drop_columns = True, Accounts = accounts, has_llc =
accounts.str.contains('llc', case=True))
Accounts has_llc
0    Blue Inc       0
1    Alpha Co       0
2   Jones LLC       0
3  Yellow Inc       0
4  Orange Inc       0
5     Red Inc       0
```

Example 2.1: Test when  case  is set to False.
```python
df.assign(drop_columns = True, Accounts = accounts, has_llc =
accounts.str.contains('llc', case=False))
Accounts has_llc
0    Blue Inc       0
1    Alpha Co       0
2   Jones LLC       1
3  Yellow Inc       0
4  Orange Inc       0
5     Red Inc       0
```
### Example 3: Test using the  na  parameter
Use the  na  parameter to specify an optional fill value for columns that have a NULL value. You can pass
numeric, string, or bool literals.
```python
df = DataFrame('employee_info')
df
first_name marks   dob joined_date
employee_no
112               None  None  None    18/12/05
101              abcde  None  None    02/12/05
100               abcd  None  None        None
df.assign(has_name = df.first_name.str.contains('abcd', na = 'FNU'))
first_name marks   dob joined_date has_name
employee_no
112               None  None  None    18/12/05      FNU
101              abcde  None  None    02/12/05        1
100               abcd  None  None        None        1
```
lower() Method
Use the lower() method to map character values to lowercase.
### Example Setup
```python
df = DataFrame('sales')
```

```python
df
Feb   Jan   Mar   Apr    datetime
accounts
Alpha Co    210.0   200   215   250  04/01/2017
Red Inc     200.0   150   140  None  04/01/2017
Orange Inc  210.0  None  None   250  04/01/2017
Jones LLC   200.0   150   140   180  04/01/2017
Yellow Inc   90.0  None  None  None  04/01/2017
Blue Inc     90.0    50    95   101  04/01/2017
```
### Example 1
```python
df.assign(drop_columns = True, lower = df.accounts.str.lower())
lower
0    alpha co
1     red inc
2  orange inc
3   jones llc
4  yellow inc
5    blue inc
```
### Example 2
```python
df[df.accounts.str.lower() == 'orange inc']
Feb   Jan   Mar  Apr    datetime
accounts
Orange Inc  210.0  None  None  250  04/01/2017
```
strip() Method
Use the strip() method to remove leading and trailing whitespace from string values in a column.
### Example Setup
```python
df = DataFrame('employee_info')
df
```

```python
first_name marks   dob joined_date
employee_no
101              abcde  None  None    02/12/05
112               None  None  None    18/12/05
100               abcd  None  None        None
fn = df.first_name
create a column with some whitespace
wdf = df.assign(drop_columns = True, fn = fn, w_spaces = '\n ' + fn +
'\v\f \t')
```
### Example 1: Display without the Strip Method
```python
>>> wdf
fn       w_spaces
0   None           None
1  abcde  \n abcde
\t
2   abcd   \n abcd
\t
```
### Example 2: Display with the Strip Method
```python
wdf.assign(drop_columns = True, wo_wspaces = wdf.w_spaces.str.strip())
wo_wspaces
0       None
1      abcde
2       abcd
```
### Regular Aggregate Functions Supported by DataFrame Column
teradataml DataFrameColumn supports the following set of regular aggregate functions which can be used
with and without DataFrame.groupby().

**Note:**
    * You must use DataFrame.assign() when using the aggregate functions on ColumnExpression,
    * also known as, teradataml DataFrameColumn.
    * You should always use "drop_columns=True" in DataFrame.assign() while running the
    * aggregate operation on teradataml DataFrame.
    * drop_columns  argument in DataFrame.assign() is ignored, when aggregate function is operated
    * on DataFrame.groupby().
See the  DataFrameColumn Aggregate Functions  section of  Teradata Package for Python Function
Reference , B700-4008) at  https://docs.teradata.com/  for detailed description and usage examples of
these functions.
Regular Aggregate Functions supported by DataFrame Column
Sr.
No.
Function
Name
Description
1
corr()
Returns the Sample Pearson product moment correlation coefficient of its
arguments for all non-null data point pairs.
2
count()
Returns column-wise count of the ColumnExpression, also known as,
teradataml DataFrameColumn.
3
covar_pop()
Returns the column-wise population covariance of its arguments for all non-
null data point pairs.
Covariance measures whether or not two random variables vary in the same
way. It is the average of the products of deviations for each non-null data
point pair.
4
covar_samp()
Returns the column-wise sample covariance of its arguments for all non-null
data point pairs.
Covariance measures whether or not two random variables vary in the same
way. It is the average of the products of deviations for each non-null data
point pair.
5
kurtosis()
Returns column-wise kurtosis value of the ColumnExpression, also known as,
teradataml DataFrameColumn.
Kurtosis is the fourth moment of the distribution of the standardized (z) values.
It is a measure of the outlier (rare, extreme observation) character of the
distribution as compared with the normal (or Gaussian) distribution.
• The normal distribution has a kurtosis of 0.
• Positive kurtosis indicates that the distribution is more outlier-prone than the
normal distribution.
• Negative kurtosis indicates that the distribution is less outlier-prone than the
normal distribution.
6
max()
Returns column-wise maximum value of the ColumnExpression, also known
as, teradataml DataFrameColumn.

Sr.
No.
Function
Name
Description
7
mean()
Returns column-wise mean value of the ColumnExpression, also known as,
teradataml DataFrameColumn.
8
median()
Returns column-wise median value of the ColumnExpression, also known as,
teradataml DataFrameColumn.
9
min()
Returns column-wise minimum value of the ColumnExpression, also known
as, teradataml DataFrameColumn.
10
percentile()
Return the value which represents the desired percentile for the
ColumnExpression, also known as, teradataml DataFrameColumn.
11
regr_avgx()
Returns the column-wise mean of the independent variable for all non-null data
pairs of the dependent and an independent variable arguments.
12
regr_avgy()
Returns the column-wise mean of the dependent variable for all non-null data
pairs of the dependent and independent variable arguments.
13
regr_count()
Returns the column-wise count of all non-null data pairs of the dependent and
independent variable arguments.
14
regr_intercept()
Returns the column-wise intercept of the univariate linear regression
line through all non-null data pairs of the dependent and independent
variable arguments.
The intercept is the point at which the regression line through the non-null data
pairs in the sample intersects the ordinate, or y-axis, of the graph.
15
regr_r2()
Returns the column-wise coefficient of determination for all non-null data pairs
of the dependent and independent variable arguments.
16
regr_slope()
Returns the column-wise coefficient slope of the univariate linear regression
line through all non-null data pairs of the dependent and an independent
variable arguments.
17
regr_sxx()
Returns the column-wise sum of the squares of the independent variable
expression for all non-null data pairs of dependent and an independent
variable arguments.
18
regr_sxy()
Returns the column-wise sum of the products of the independent variable
and the dependent variable for all non ‑ null data pairs of the dependent and
independent variable arguments.
19
regr_syy()
Returns the column-wise sum of the squares of the dependent variable
expression for all non-null data pairs of dependent and an independent
variable arguments.
20
skew()
Returns column-wise skewness of the distribution of the ColumnExpression,
also known as, teradataml DataFrameColumn.
Skewness is the third moment of a distribution. It is a measure of the
asymmetry of the distribution about its mean compared with the normal (or
Gaussian) distribution.
• The normal distribution has a skewness of 0.

Sr.
No.
Function
Name
Description
• Positive skewness indicates a distribution having an asymmetric tail
extending toward more positive values.
• Negative skewness indicates an asymmetric tail extending toward more
negative values.
21
std()
Returns column-wise sample or population standard deviation value of
the ColumnExpression, also known as, teradataml DataFrameColumn. The
standard deviation is the second moment of a distribution.
• For a sample, it is a measure of dispersion from the mean of that sample.
• For a population, it is a measure of dispersion from the mean of
that population.
The computation is more conservative for the population standard deviation to
minimize the effect of outliers on the computed value.
22
sum()
Returns column-wise sum value of the ColumnExpression, also known as,
teradataml DataFrameColumn.
23
var()
Returns column-wise sample or population variance of the columns of the
ColumnExpression, also known as, teradataml DataFrameColumn.
• The variance of a population is a measure of dispersion from the mean of
that population.
• The variance of a sample is a measure of dispersion from the mean of that
sample. It is the square of the sample standard deviation.
teradataml special aggregate functions
24
csum()
Returns cumulative sum value for rows in the partition of the column.
25
msum()
Computes the moving sum for the current row and the preceding "width"-1
rows in a partition, by sorting the rows according to "sort_columns".
26
mavg()
Computes the moving average for the current row and the preceding "width"-1
rows in a partition, by sorting the rows according to "sort_columns".
27
mdiff()
Computes the moving difference for the current row and the preceding "width"
rows in a partition, by sorting the rows according to "sort_columns".
28
mlinreg()
Computes the moving linear regression for the current row and the preceding
"width"-1 rows in a partition, by sorting the rows according to "sort_columns".
### Date Time Functions
teradataml DataFrameColumn supports following set of date time functions.
See the Data Time Functions section of  Teradata Package for Python Function Reference , B700-4008) at
https://docs.teradata.com/  for detailed description and usage examples of these functions.

Data Time Functions supported by DataFrameColumn
Sr.
No.
Function Name
Description
1
week_start()
Returns the first date or timestamp of the week that begins immediately
before the specified date or timestamp value in a column as a literal.
2
week_begin()
This is an alias for week_start() function.
3
week_end()
Returns the last date or timestamp of the week that ends immediately
after the specified date or timestamp value in a column as a literal.
4
month_start()
Returns the first date or timestamp of the month that begins immediately
before the specified date or timestamp value in a column or as a literal.
5
month_begin()
This is an alias for month_start() function.
6
month_end()
Returns the last date or timestamp of the month that ends immediately
after the specified date or timestamp value in a column or as a literal.
7
year_start()
Returns the first date or timestamp of the year that begins immediately
before the specified date or timestamp value in a column or as a literal.
8
year_begin()
This is an alias for year_start() function.
9
year_end()
Returns the last date or timestamp of the year that ends immediately
after the specified date or timestamp value in a column or as a literal.
10
quarter_start()
Returns the first date or timestamp of the quarter that begins immediately
before the specified date or timestamp value in a column as a literal.
11
quarter_begin()
This is an alias for quarter_start() function.
12
quarter_end()
Returns the last date or timestamp of the quarter that ends immediately
after the specified date or timestamp value in a column as a literal.
13
last_sunday()
Returns the date or timestamp of Sunday that falls immediately before
the specified date or timestamp value in a column as a literal.
14
last_monday()
Returns the date or timestamp of Monday that falls immediately before
the specified date or timestamp value in a column as a literal.
15
last_tuesday()
Returns the date or timestamp of Tuesday that falls immediately before
the specified date or timestamp value in a column as a literal.
16
last_wednesday()
Returns the date or timestamp of Wednesday that falls immediately
before specified date or timestamp value in a column as a literal.
17
last_thursday()
Returns the date or timestamp of Thursday that falls immediately before
specified date or timestamp value in a column as a literal.
18
last_friday()
Returns the date or timestamp of Friday that falls immediately before
specified date or timestamp value in a column as a literal.
19
last_saturday()
Returns the date or timestamp of Saturday that falls immediately before
specified date or timestamp value in a column as a literal.

Sr.
No.
Function Name
Description
20
day_of_week()
Returns the number of days from the beginning of the week to the
specified date or timestamp value in a column as a literal.
21
day_of_month()
Returns the number of days from the beginning of the month to the
specified date or timestamp value in a column as a literal.
22
day_of_year()
Returns the number of days from the beginning of the year to the
specified date or timestamp value in a column as a literal.
23
day_of_calendar()
Returns the number of days from the beginning of the business calendar
to the specified date or timestamp value in a column as a literal.
24
week_of-month()
Returns the number of weeks from the beginning of the month to the
specified date or timestamp value in a column as a literal.
25
week_of_quarter()
Returns the number of weeks from the beginning of the quarter to the
specified date or timestamp value in a column as a literal.
26
week_of_year()
Returns the number of weeks from the beginning of the year to the
specified date or timestamp value in a column as a literal.
27
week_of_calendar()
Returns the number of weeks from the beginning of the calendar to the
specified date or timestamp value in a column as a literal.
28
month_of_year()
Returns the number of months from the beginning of the year to the
specified date or timestamp value in a column as a literal.
29
month_of_calendar()
Returns the number of months from the beginning of the calendar to the
specified date or timestamp value in a column as a literal.
30
month_of_quarter()
Returns the number of months from the beginning of the quarter to the
specified date or timestamp value in a column as a literal.
31
quarter_of_year()
Returns the number of quarters from the beginning of the year to the
specified date or timestamp value in a column as a literal.
32
quarter_
of_calendar()
Returns the number of quarters from the beginning of the calendar to the
specified date or timestamp value in a column as a literal.
33
year_of_calendar()
Returns the year of the specified date or timestamp value in a column as
a literal.
34
day_occurence_
of_month()
Returns the nth occurrence of the weekday in the month for the date to
the specified date or timestamp value in a column as a literal.
35
year()
Returns the integer value for year in the specified date or timestamp
value in a column as a literal.
36
month()
Returns the integer value for month in the specified date or timestamp
value in a column as a literal.
37
hour()
Returns the integer value for hour in the specified timestamp value in a
column as a literal.

Sr.
No.
Function Name
Description
38
minute()
Returns the integer value for minute in the specified timestamp value in
a column as a literal.
39
second()
Returns the integer value for seconds in the specified timestamp value
in a column as a literal.
40
week()
Returns the number of weeks from the beginning of the year to the
specified date or timestamp value in a column as a literal.
41
next_day()
Returns the date of the first weekday specified as 'day_value' that is later
than the specified date or timestamp value in a column as a literal.
42
months_between()
Returns the number of months between value in specified date or
timestamp value in a column as a literal and date or timestamp value
in argument.
43
add_month()
Adds an integer number of months to specified date or timestamp value
in a column as a literal.
44
oadd_month()
Adds an integer number of months, date or timestamp value in specified
date or timestamp value in a column as a literal.
45
to_date()
Function converts a string to Date.
46
to_timestamp()
Converts string or integer value to a TIMESTAMP data type or
TIMESTAMP WITH TIME ZONE data type.
47
extract()
Extracts date component to a numeric value.
48
to_interval()
Converts a numeric value or string value into an INTERVAL_DAY_TO_
SECOND or INTERVAL_YEAR_TO_MONTH value.
### String Functions
teradataml DataFrameColumn supports following set of string functions.
See the String Functions section of  Teradata Package for Python Function Reference , B700-4008) at
https://docs.teradata.com/  for detailed description and usage examples of these functions.
String Functions supported by DataFrameColumn
Sr. No.
Function Name
Description
1
concat()
Function to concatenate the columns with a separator.
2
like()
Function to match the string pattern. String match is case sensitive.
3
ilike()
Function to match the string pattern. String match is not case sensitive.
4
substr()
Returns the substring from a string column.
5
startswitch()
Function to check if the column value starts with the specified value or not.

Sr. No.
Function Name
Description
6
endswitch()
Function to check if the column value ends with the specified value or not.
7
format()
Function to format the values in column based on formatter.
8
to_char()
Function converts numeric type or datetype to character type.
9
trim()
Function trims the string values in the column.
10
ascii()
Returns the decimal representation of the first character in column.
11
char2hexint()
Returns the hexadecimal representation for a character string in a column.
12
chr()
Returns the Latin ASCII character of a given a numeric code value
in column.
13
char()
It is an alias for chr() function.
14
character_length()
Returns the number of characters in the column.
15
char_length()
It is an alias for character_length() function.
16
edit_distance()
Returns the minimum number of edit operations required to transform
string in a column into string specified in argument.
17
index()
Returns the position of a string in a column where string specified in
argument starts.
18
initcap()
Modifies a string column and returns the string with the first character of
each word in uppercase.
19
instr()
Searches the string in a column for occurrences of search string passed
as argument.
20
lcase()
Returns a character string identical to string values in column, with all
uppercase letters replaced with their lowercase equivalents.
21
left()
Truncates string in a column to a specified number of characters desired
from the left side of the string.
22
length()
It is an alias for character_length() function.
23
levenshtein()
It is an alias for edit_distance() function.
24
locate()
Returns the position of the first occurrence of a string in a column within
string in argument.
25
lower()
It is an alias for character_lcase() function.
26
lpad()
Returns the string in a column padded to the left with the characters
specified in argument so that the resulting string has length specified
in argument.
27
ltrim()
Returns the string in a column, with its left most characters removed up to
the first character that is not in the string specified in argument.
28
ngram()
Returns the number of n gram matches between string in a column, and
string specified in argument.

Sr. No.
Function Name
Description
29
nvp()
Extracts the value of a name value pair where the name in the pair matches
the name and the number of the occurrence specified.
30
oreplace()
Replaces every occurrence of search string in the column.
31
otranslate()
Returns string in a column with every occurrence of each character
in string in argument replaced with the corresponding character in
another argument.
32
replace()
It is an alias for oreplace() function.
33
reverse()
Returns the reverse of string in column.
34
right()
Truncates input string to a specified number of characters desired from the
right side of the string.
35
rpad()
Returns the string in a column padded to the right with the characters
specified in argument so the resulting string has length specified
in argument.
36
rtrim()
Returns the string in column, with its right most characters removed up to
the first character that is not in the string specified in argument.
37
soundex()
Returns a character string that represents the Soundex code for string in
a column.
38
string_cs()
Returns a heuristically derived integer value that can be used to determine
which KANJI1 compatible client character set was used to encode string in
a column.
39
translate()
It is an alias for otranslate() function.
40
upper()
Returns a character string with all lowercase letters in a column replaced
with their uppercase equivalents.
41
parse_url()
Extracts a part from a URL.
42
to_number()
Converts a string-like representation of a number to NUMBER type.
43
rlike()
Returns true if str matches the posix regexp, or False otherwise.
44
substring_index
Returns the substring from a column before a specified delimiter, up to a
given occurrence count.
45
count_delimiters()
Counts the total number of occurrences of a specified delimiter.
### Regular Arithmetic Functions
teradataml DataFrameColumn supports following set of regular arithmetic functions.
See the Arithmetic Functions section of  Teradata Package for Python Function Reference , B700-4008) at
https://docs.teradata.com/  for detailed description and usage examples of these functions.

Regular Arithmetic Functions supported by DataFrameColumn
Sr. No.
Function Name
Description
1
cbrt()
Computes the cube root of values in the column.
2
hex()
Computes the Hexadecimal from decimal for the values in the column.
3
hypot()
Computes the hypotenuse for the values between two columns.
4
unhex()
Computes the decimal from Hexadecimal for the values in the column.
5
abs()
Computes the absolute value.
6
ceil()
Returns the ceiling value of the column.
7
ceiling()
It is an alias for ceil() function.
8
degrees()
Converts radians value from the column to degrees.
9
exp()
Raises e (the base of natural logarithms) to the power of the value in the
column, where e = 2.71828182845905.
10
floor()
Returns the largest integer equal to or less than the value in the column.
11
ln()
Computes the natural logarithm of values in column.
12
log10()
Computes the base 10 logarithm.
13
mod()
Returns the modulus of the column.
14
pmod()
It is an alias for mod() function.
15
nullifzero()
Converts data from zero to null to avoid problems with division by zero.
16
pow()
Computes the power of the column raised to expression or constant.
17
power()
It is an alias for pow() function.
18
radians()
Converts degree value from the column to radians.
19
round()
Returns the rounded off value.
20
sign()
Returns the sign.
21
signum()
It is an alias for sign() function.
22
sqrt()
Computes the square root of values in the column.
23
trunc()
Provides the truncated value of columns.
24
width_bucket()
Returns the number of the partition to which column is assigned.
25
zeroifnull()
Converts data from null to zero to avoid problems with null.
26
log()
Returns the logarithm value of the column with respect to 'base'.
27
isnan()
Evaluates expression to determine if the floating-point argument is a NaN
(Not-a-Number) value.

Sr. No.
Function Name
Description
28
isinf()
Evaluates expression to determine if the floating-point argument is an
infinite number.
29
isfinite()
Evaluates expression to determine if it is a finite floating value.
### Bit Byte Manipulation Functions
teradataml DataFrameColumn supports following set of bit byte manipulation functions.
See the Bit Byte Manipulation Functions section of  Teradata Package for Python Function Reference ,
B700-4008) at  https://docs.teradata.com/  for detailed description and usage examples of these functions.
Bit Byte Manipulation Functions supported by DataFrameColumn
Sr. No.
Function Name
Description
1
from_byte()
Encodes a sequence of bits into a sequence of characters.
2
bit_and()
Returns the logical AND operation on the bits from the column and
corresponding bits from the argument.
3
bit_get()
Returns the bit specified by input argument from the column and returns
either 0 or 1 to indicate the value of that bit.
4
bit_or()
Returns the logical OR operation on the bits from the column and
corresponding bits from the argument.
5
bit_xor()
Returns the bitwise XOR operation on the binary representation of the
column and corresponding bits from the argument.
6
bitand()
It is an alias for bit_and() function.
7
bitnot()
Returns a bitwise complement on the binary representation of the column.
8
bitor()
It is an alias for bit_or() function.
9
bitwise_not()
It is an alias for bitnot() function.
10
bitwiseNOT()
It is an alias for bitnot() function.
11
bitxor()
It is an alias for bit_xor() function.
12
countset()
Returns the count of the binary bits within the column that are either set to 1
or set to 0, depending on the input argument value.
13
getbit()
It is an alias for bit_get() function.
14
rotateleft()
Returns an expression rotated to the left by the specified number of bits, with
the most significant bits wrapping around to the right.
15
rotateright()
Returns an expression rotated to the right by the specified number of bits,
with the least significant bits wrapping around to the left.
16
setbit()
Sets the value of the bit specified by input argument to the value of column.

Sr. No.
Function Name
Description
17
shiftleft()
Returns the expression when value in column is shifted by the specified
number of bits to the left.
18
shiftright()
Returns the expression when column expression is shifted by the specified
number of bits to the right.
19
subbitstr()
Extracts a bit substring from the column expression based on the specified
bit position.
20
to_byte()
Converts a numeric data type to the Vantage byte representation (byte value)
of the column expression value.
### Regular Expression Functions
teradataml DataFrameColumn supports following set of regular expression functions.
See the Regular Expression Functions section of  Teradata Package for Python Function Reference ,
B700-4008) at  https://docs.teradata.com/  for detailed description and usage examples of these functions.
Regular Expression Functions supported by DataFrame Column
Sr. No.
Function Name
Description
1
regexp_instr()
Searches string value in column for a match to value specified in argument.
2
regexp_replace()
Replaces the portions of string value in a column that matches the value
specified regex string and replaces with the replace string.
3
regexp_similar()
Compares value in column to value in argument and returns integer value.
4
regexp_substr()
Extracts a substring from column that matches a regular expression
specified in the input argument.
### Comparison Functions
See the Comparison Functions section of  Teradata Package for Python Function Reference , B700-4008)
at  https://docs.teradata.com/  for detailed description and usage examples of these functions.
Comparison Functions supported by DataFrame Column
Sr. No.
Function Name
Description
1
greatest()
Returns the greatest values from columns.
2
least()
Returns the least values from columns.

### Trigonometric Functions
See the Trigonometric Functions section of  Teradata Package for Python Function Reference , B700-4008)
at  https://docs.teradata.com/  for detailed description and usage examples of these functions.
Trigonometric Functions supported by DataFrame Column
Sr. No.
Function Name
Description
1
acos()
Returns the arc-cosine value.
2
asin()
Returns the arc-sine value.
3
atan()
Returns the arc-tangent value.
4
atan2()
Returns the arc-tangent value based on x and y coordinates.
5
cos()
Returns the cosine value.
6
sin()
Returns the sine value.
7
tan()
Returns the tangent value.
### Hyperbolic Functions
See the Hyperbolic Functions section of  Teradata Package for Python Function Reference , B700-4008) at
https://docs.teradata.com/  for detailed description and usage examples of these functions.
Hyperbolic Functions supported by DataFrame Column
Sr. No.
Function Name
Description
1
acosh()
Returns the inverse hyperbolic cosine value.
2
asinh()
Returns the inverse hyperbolic sine value.
3
atanh()
Returns the inverse hyperbolic tangent value.
4
cosh()
Returns the hyperbolic cosine value.
5
sinh()
Returns the hyperbolic sine value
6
tanh()
Returns the hyperbolic tangent value.