# Appendix C: teradataml Extension with SQLAlchemy

Teradata Vantage offers various SQL functions that allow a user to process data. This section describes how
these common SQL functions can be used in teradataml with the help of SQLAlchemy.
You do not need to have thorough knowledge of SQLAlchemy to use the SQLAlchemy extension to
teradataml. Refer to the examples provided in the demo notebooks which are part of the teradataml
package. These demo notebooks can be found at pkg_install_location/teradataml/data/notebooks
```python
/sqlalchemy/.
```
Note:
```python
teradataml 17.20.00.xx versions only support SQLAlchemy 1.4,
```
SQLAlchemy 2.0 is not yet supported.
* Accessing Vantage SQL Functions
* Using SQLAlchemy Clause Element and Expression
## Accessing Vantage SQL Functions
teradataml supports SQL functions with the SQLAlchemy extension. For lists of these functions, see
Supported SQL Functions.
To access these functions in teradataml, you only need to know the following SQLAlchemy statement, the
rest of the functionality is managed by teradataml and Vantage.
```python
from sqlalchemy import func
```
You can use this imported func module to call Vantage SQL functions and pass those to DataFrame.assign()
function to process the data.
For more details about these functions, refer to SQL Functions, Expressions, and Predicates, B035-1145.
* Example: Accessing Vantage SQL Function with SQLAlchemy
* Supported SQL Functions
### Example: Accessing Vantage SQL Function with SQLAlchemy
In this example, you calculate the standard average of column 'gpa' and 'admitted' of the
"admissions_trains" dataset using the avg/average/ave function in Vantage.
The avg/average/ave function has the following syntax, and returns the arithmetic average of all values
in value_expression.
```python
Avg(value_expression)
```
* Create connection to Vantage and load example dataset.
```python
# Get the connection to the Vantage using create_context()
>>> from teradataml import *
>>>td_context = create_context(host=getpass.getpass("Hostname:
"), username=getpass.getpass("Username: "),
password=getpass.getpass("Password: "))
# Load the example dataset.
load_example_data("GLM", ["admissions_train"])
```
* Prepare example dataset.
```python
# Create the DataFrame on 'admissions_train' table
>>> admissions_train = DataFrame("admissions_train")
>>> admissions_train
masters   gpa     stats programming  admitted
id
13      no  4.00  Advanced      Novice         1
9       no  3.82  Advanced    Advanced         1
33      no  3.55    Novice      Novice         1
6      yes  3.50  Beginner    Advanced         1
7      yes  2.33    Novice      Novice         1
22     yes  3.46    Novice    Beginner         0
37      no  3.52    Novice      Novice         1
35      no  3.68    Novice    Beginner         1
27     yes  3.96  Advanced    Advanced         0
4      yes  3.50  Beginner      Novice         1
```
* Import func module from SQLAlchemy package.
```python
# Import func from SQLAlchemy to use the same for executing
aggregate functions
>>> from sqlalchemy import func
```
* Use the func module to generate a function object "agg_func_".
```python
>>> agg_func_ = func.avg(admissions_train.gpa.expression)
```
  Where:
  * func: sqlalchemy module that is imported.
  * avg: the Vantage function name.
Note:
    The function name is case insensitive.
    You can pass the function name in lowercase (avg), uppercase (AVG) or in mixed
    cases (Avg).
  * admissions_train.gpa.expression: an expression that is passed to the function, specifying the
    column to use for calculating average.
    Where:
    admissions_train: the teradataml DataFrame;
    gpa: a column name of the teradataml DataFrame;
    Together, admissions_train.gpa forms a ColumnExpression (teradataml DataFrame Column).
    expression: a property of teradataml DataFrame column, which is passed as the required
    expression to this function object.
* View the type of the function object "agg_func_".
```python
>>> type(agg_func_)
sqlalchemy.sql.functions.Function
```
* Pass the function object "agg_func_", with other function objects for calculating average value of the
  'admitted' column, to DataFrame.assign() function.
```python
>>> df = admissions_train.assign(True, avg_gpa_=agg_func_,
average_admitted_=func.average(admissions_train.admitted.expression),
ave_admitted_=func.ave(admissions_train.admitted.expression))
>>> print_variables(df, ["avg_gpa_", "average_admitted_", "ave_admitted_"])
Equivalent SQL: select ave(admitted) AS ave_admitted_, average(admitted) AS
average_admitted_, avg(gpa) AS avg_gpa_ from "admissions_train"
************************* DataFrame *********************
ave_admitted_  average_admitted_  avg_gpa_
0           0.65               0.65   3.54175
************************* DataFrame.dtypes *********************
ave_admitted_        float
average_admitted_    float
avg_gpa_             float
'avg_gpa_' Column Type: FLOAT
'average_admitted_' Column Type: FLOAT
'ave_admitted_' Column Type: FLOAT
```
  Note:
    Different function names "AVERAGE" and "AVE" that Vantage offers to calculate average are
    used along with 'avg'.
  Note:
    "print_variables()" is a private function used to display example outputs here. This function uses
    "DataFrame.show_query()" to print the query, "print(dataframe)" to display dataframe content,
    and "DataFrame.dtypes" to display DataFrame column types. This function is not part of the
    teradataml package.
### Supported SQL Functions
teradataml supports the following categories of SQL functions with SQLAlchemy extension:
* Aggregate Functions
* Arithmetic, Hyperbolic and Trigonometric Functions
* Bit Byte Manipulation Functions
* Built-In Functions
* Hash Related Functions
* Regular Expression Functions
* String Functions
* Window Aggregate Functions
#### Aggregate Functions
Note:
Examples for all aggregate functions can be found at: pkg_install_location\
```python
teradataml\data\notebooks\sqlalchemy\Teradata Vantage Aggregate Functions using
SQLAlchemy.ipynb.
```
#### Supported functions
| S/N Function Name | Description |
| ----------------- | ----------- |
| 1 Avg / Average / Ave | Returns the arithmetic average of all values in value_expression. |
| 2 Corr | Returns the Sample Pearson product moment correlation coefficient of its arguments for all non-null data point pairs. |
| 3 Count | Returns a column value that is the total number of qualified rows in value_expression. |
| 4 Covar_pop | Returns the population covariance of its arguments for all non-null data point pairs. |
| 5 Covar_samp | Returns the sample covariance of its arguments for all non-null data point pairs. |
| 6 Kurtosis | Returns the kurtosis of the distribution of value_expression. |
| 7 max / maximum | Returns a column value that is the maximum value for value_expression. |
| 8 min / minimum | Returns a column value that is the minimum value for value_expression. |
| 9 REGR_AVGX | Returns the mean of the independent_variable_expression for all non-null data pairs of the dependent and independent variable arguments. |
| 10 REGR_AVGY | Returns the mean of the dependent_variable_expression for all non-null data pairs of the dependent and independent variable arguments. |
| 11 REGR_Count | Returns the count of all non-null data pairs of the dependent and independent variable arguments. |
| 12 REGR_Intercept | Returns the intercept of the univariate linear regression line through all non-null data pairs of the dependent and independent variable arguments. |
| 13 REGR_R2 | Returns the coefficient of determination for all non-null data pairs of the dependent and independent variable arguments. |
| 14 REGR_SLOPE | Returns the slope of the univariate linear regression line through all non-null data pairs of the dependent and independent variable arguments. |
| 15 REGR_SXX | Returns the sum of the squares of the independent_variable_expression for all non-null data pairs of the dependent and independent variable arguments. |
| 16 REGR_SXY | Returns the sum of the products of the independent_variable_expression and the dependent_variable_expression for all non-null data pairs of the dependent and independent variable arguments. |
| 17 REGR_SYY | Returns the sum of the squares of the dependent_variable_expression for all non-null data pairs of the dependent and independent variable arguments. |
| 18 Skew | Returns the skewness of the distribution of value_expression. |
| 19 stddev_pop | Returns the population standard deviation for the non-null data points in value_expression. |
| 20 stddev_samp | Returns the sample standard deviation for the non-null data points in value_expression. |
| 21 sum | Returns a column value that is the arithmetic sum of value_expression. |
| 22 var_pop | Returns the population variance for the data points in value_expression. |
| 23 var_samp | Returns the sample variance for the data points in value_expression. |

Note:
Grouping of columns is not allowed with DataFrame.assign() in teradataml 17.00.00.00. Always use
'drop_column = True' in DataFrame.assign().
#### Unsupported functions
* grouping
* pivot
* unpivot
#### Arithmetic, Hyperbolic and Trigonometric Functions
Note:
Examples can be found at:pkg_install_location\teradataml\data\notebooks\sqlalchemy\
```python
Teradata Vantage Arithmetic Functions Using SQLAlchemy.ipynb.
```
#### Supported functions
S/N Function Name Description
Arithmetic Functions
1 ABS Computes the absolute value of an argument.
2 CASE_N Evaluates a list of conditions and returns the position of the first condition
          that evaluates to TRUE, provided that no prior condition in the list evaluates
          to UNKNOWN.
3 CEIL / CEILING Returns the smallest integer value that is not less than the input argument.
4 DEGREES DEGREES takes a value specified in radians and converts it to degrees.
5 RADIANS RADIANS takes a value specified in degrees and converts it to radians.
6 EXP Raises e (the base of natural logarithms) to the power of the argument, where
          e = 2.71828182845905.
| S/N Function Name | Description |
| ----------------- | ----------- |
| 7 FLOOR | Returns the largest integer equal to or less than the input argument. |
| 8 LN | Computes the natural logarithm of the argument. |
| 9 LOG | Computes the base 10 logarithm of an argument. |
| 10 MOD | Returns the remainder (modulus) of expr1 divided by expr2. |
| 11 NULLIFZERO | Converts data from zero to null to avoid problems with division by zero. |
| 12 POWER | Returns base_value raised to the power of exponent_value. |
| 13 ROUND | Returns numeric_value rounded places_value places to the right or left of the decimal point. |
| 14 SIGN | Returns the sign of numeric_value. |
| 16 SQRT | Computes the square root of an argument. |
| 17 TRUNC | Returns numeric_value truncated places_value places to the right or left of the decimal point. |
| 18 WIDTH_BUCKET | Returns the number of the partition to which value_expression is assigned. |
| 19 ZEROIFNULL | Converts data from null to 0 to avoid cases where a null result creates an error. |
| Hyperbolic Functions |  |
| 20 COSH | Performs the hyperbolic or inverse hyperbolic function of an argument. |
| 21 ACOSH |  |
| 22 SINH |  |
| 23 ASINH |  |
| 24 TANH |  |
| 25 ATANH |  |
| Trigonometric Functions |  |
| 26 SIN | Performs the trigonometric or inverse trigonometric function of an argument. |
| 27 ASIN |  |
| 28 COS |  |
| 29 ACOS |  |
| 30 TAN |  |
| 31 ATAN |  |
| 32 ATAN2 |  |
#### Unsupported functions
* RANDOM
* RANGE_N
#### Bit Byte Manipulation Functions
Note:
Examples for all bit byte manipulate functions can be found at: pkg_install_location\teradataml\
```python
data\notebooks\sqlalchemy\Teradata Vantage Bit-Byte Manipulation Functions
using SQLAlchemy.ipynb.
```
#### Supported functions
| S/N Function Name | Description |
| ----------------- | ----------- |
| 1 BITAND | Performs the logical AND operation on the corresponding bits from the two input arguments. |
| 2 BITNOT | Performs a bitwise complement on the binary representation of the input argument. |
| 3 BITOR | Performs the logical OR operation on the corresponding bits from the two input arguments. |
| 4 BITXOR | Performs a bitwise XOR operation on the binary representation of the two input arguments. |
| 5 COUNTSET | Returns the count of the binary bits within the target_arg expression that are either set to 1 or set to 0 depending on the target_value_arg value. |
| 6 GETBIT | Returns the value of the bit specified by target_bit_arg from the target_arg byte expression. |
| 7 ROTATELEFT | Returns an expression rotated to the left by the number of bits you specify, with the most significant bits wrapping around to the right. |
| 8 ROTATERIGHT | Returns an expression rotated to the right by the number of bits you specify, with the least significant bits wrapping around to the left. |
| 9 SETBIT | Sets the value of the bit specified by target_bit_arg to the value of target_value_ arg in the target_arg byte expression. |
| 10 SHIFTLEFT | Returns the expression target_arg shifted by the specified number of bits (num_ bits_arg) to the left. The bits in the most significant positions are lost, and the bits in the least significant positions are filled with zeros. |
| 11 SHIFTRIGHT | Returns the expression target_arg shifted by the specified number of bits (num_ bits_arg) to the right. The bits in the least significant positions are lost, and the bits in the most significant positions are filled with zeros. |
| 12 SUBBITSTR | Extracts a bit substring from the target_arg input expression based on the specified bit position. |
| 13 TO_BYTE | Converts a numeric data type to the database server byte representation (byte value) of the input value. |

| 1 CURRENT_DATE / CURDATE | Returns the current date. | AT LOCAL and AT TIME ZONE clauses are not supported. |
| ------------------------ | ------------------------- | ---------------------------------------------------- |
| 2 CURRENT_TIME / CURTIME | Returns the current time. | AT LOCAL and AT TIME ZONE clauses are not supported. |
| 3 CURRENT_ TIMESTAMP | Returns the current timestamp. | AT LOCAL and AT TIME ZONE clauses are not supported. |

#### Built-In Functions
Note:
Examples for all built-in functions can be found at: pkg_install_location\teradataml\
```python
data\notebooks\sqlalchemy\Teradata Vantage Built-in functions using
SQLAlchemy.ipynb.
```
#### Supported functions
S/N Function Name Description Comment
#### Hash Related Functions
#### Supported functions
S
    Function Name Description Comment
/N
1 HASHAMP Finds the primary AMP corresponding to
        the hash bucket number specified in the
        expression and returns the AMP ID.
        If no hash bucket number is specified,
        HASHAMP returns one less than the
        maximum number of AMPs in the system.
                              * Only expression, i.e.,
                                column can be passed to
                                this function.
                              * MAP and COLOCATE
                                USING clauses of
                                the functions are
                                not supported.
2 HASHBAKAMP Finds the fallback AMP corresponding to
        the hash bucket number specified in the
        expression and returns the AMP ID.
                              * Only expression, i.e.,
                                column can be passed to
                                this function.
| S /N Function Name | Description | Comment |
| ------------------ | ----------- | ------- |
|  | If no hash bucket is specified, HASHBAKAMP returns one less than the maximum number of fallback AMPs in the system. | * MAP and COLOCATE USING clauses of the functions are not supported. |
| 3 HASHBUCKET | Returns the hash bucket number that corresponds to a specified row hash value. If no row hash value is specified, HASHBUCKET returns the highest hash bucket number. |  |
| 4 HASHROW | Returns the hexadecimal row hash value for an expression or sequence of expressions. If no expression is specified, HASHROW returns the maximum hash code value. | User can execute this either by passing expression or without any expression. |

#### Regular Expression Functions
Note:
Examples for all regular expression functions can be found at: pkg_install_location\
```python
teradataml\data\notebooks\sqlalchemy\Teradata Vantage Regular Expressions Using
SQLAlchemy.ipynb.
```
Examples in notebooks will help to understand on how values can be passed and these functions
cane be used in teradataml.
#### Supported functions
S/N Function Name Description
1 REGEXP_SUBSTR Extracts a substring from source_string that matches a regular expression
            specified by regexp_string.
2 REGEXP_REPLACE Replaces portions of source_string that match regexp_string with the
            replace_string.
3 REGEXP_INSTR Searches source_string for a match to regexp_string.
4 REGEXP_SIMILAR Compares source_string to regexp_string and returns integer value.
Note:
Refer to SQL Documentation for more details on these functions and their signature.
#### Unsupported functions
* REGEXP_SPLIT_TO_TABLE
#### String Functions
Note:
Examples for all string functions can be found at: pkg_install_location\teradataml\data\
```python
notebooks\sqlalchemy\Teradata Vantage String Functions Using SQLAlchemy.ipynb.
```
#### Supported functions
| S/N Function Name | Description | Comment |
| ----------------- | ----------- | ------- |
| 1 ASCII | Returns the decimal representation of the first character in string_expr as a NUMBER value. The decimal representation will reflect the character set of the input string. |  |
| 2 CHAR2HEXINT | Returns the hexadecimal representation for a character string. |  |
| 3 CHR | Returns the Latin ASCII character given a numeric code value. |  |
| 4 CONCAT | Concatenates string expressions. |  |
| 5 EDITDISTANCE | Returns the minimum number of edit operations (insertions, deletions, substitutions and transpositions) required to transform string1 into string2. |  |
| 6 INDEX | Returns the position in string_expression_1 where string_expression_2 starts. |  |
| 7 INITCAP | Modifies a string argument and returns the string with the first character in each word in uppercase and all other characters in lowercase. Words are delimited by white space or characters that are not alphanumeric. |  |
| 8 INSTR | Searches the source_string argument for occurrences of search_string. |  |
| 9 LEFT | Truncates in input string to a specified number of characters. The LEFT function can be called with the 'LEFT' or 'TD_LEFT' alias names. |  |
| 10 LENGTH | Returns the number of characters in the expression. |  |
| 11 LOCATE | Returns the position of the first occurrence of string_ expr1 within string_expr2. The search for the first |  |
|  | occurrence of string_expr1 begins with the first character position in string_expr2 unless the optional argument, n1, is specified. |  |
| 12 LOWER | Returns a character string identical to character_ string_expression, except that all uppercase letters are replaced with their lowercase equivalents. |  |
| 13 LPAD | Returns the source_string padded to the left with the characters in fill_string so that the resulting string is length characters. |  |
| 14 LTRIM | Returns the argument expr1, with its left-most characters removed up to the first character that is not in the argument expr2. |  |
| 15 NGRAM | Returns the number of n-gram matches between string1 and string2. |  |
| 16 NVP | Extracts the value of a name-value pair where the name in the pair matches the name and the number of the occurrence specified. |  |
| 17 OREPLACE | Replaces every occurrence of search_string in the source_string with the replace_string. Use this function either to replace or remove portions of a string. |  |
| 18 OTRANSLATE | Returns source_string with every occurrence of each character in from_string replaced with the corresponding character in to_string. |  |
| 19 REVERSE | Reverses the input string. |  |
| 20 RIGHT | Starting from the end of the input string, a substring is created with the number of characters specified by the second parameter. The RIGHT function can be called with the 'RIGHT' or 'TD_RIGHT' alias names. |  |
| 21 RPAD | Returns the source_string padded to the right with the characters in fill_string so that the resulting string is length characters. |  |
| 22 RTRIM | Returns the argument expr1, with its right-most characters removed up to the first character that is not in the argument expr2. |  |
| 23 SOUNDEX | Returns a character string that represents the Soundex code for string_expression. |  |
| 24 STRING_CS | Returns a heuristically derived integer value that you can use to help determine which KANJI1- compatible client character set was used to encode string_expression. |  |
| 25 SUNSTRING / SUBSTR | Extracts a substring from a named string based on position. | Only Teradata Syntax is supported. ANSI syntax is not supported. In SQL, both are supported. Check SQL functions documentation. |
| 26 TRIM | Takes a character or byte string_expression argument, trims the specified pad characters or bytes, and returns the trimmed string. | Following clauses from SQL are not supported: * BOTH * Leading * Trailing * From * character_Set |
| 27 UPPER / UCASE | Returns a character string identical to character_ string_expression, except that all lowercase letters are replaced by their uppercase equivalents. |  |

#### Unsupported functions
* CSV
* CSVLD
* POSITION
* STROK
* STRTOK_SPLIT_TO_TABLE
* TRANSLATE
* TRANSLATE_CHK
* VARGRAPHIC
#### Window Aggregate Functions
#### Syntax of over() function
Window aggregation offered by Vantage supports clauses and can be used in teradataml with the help
of SQLAlchemy.
For Window aggregates, one of the major tasks is to specify the window over which aggregate operation
must be performed.
Specifying window in SQL is done by OVER clause, which has following syntax:
```python
OVER([PARTITION BY ...] [ORDER BY ...] [RESET WHEN ...] [ROWS ... |
ROWS BETWEEN ...])
```
With SQLAlchemy, specifying window can be achieved by using the over() function, which has the
following syntax:
```python
over(partition_by = partition_expression, order_by = order_expression, rows =
(p, f))
```
Where:
* partition_expression: column expression to partition the data on.
  For example: df.column_name.expression
* order_expression: column expression to sort the data.
  Sorting can be done in either Ascending or Descending order with NULLS FIRST or NULLS LAST.
  For example:
  * Default sorting: df.column_name.expression
  * Ascending Order: df.column_name.expression.asc()
  * Descending Order: df.column_name.expression.desc()
  * With NULLS FIRST - df.column_name.expression.nullsfirst()
  * With NULLS LAST - df.column_name.expression.nullslast()
  * Ascending Order with NULLS LAST - df.column_name.expression.asc().nullslast()
* rows: generates the syntax for 'ROWS BETWEEN'.
  To perform windowed aggregate function over a window using ROWS and ROWS BETWEEN, you
  must use a rows argument that accepts a tuple (p, f). p and f can accept the following values. Each
  value passed to p and f has different meaning or results in different syntax. SQL syntax is generated
  based on these values.
  * Negative Value: indicates a preceding value;
  * Positive Value: indicates a following value;
  * None: unbounded value.
#### Example
This section shows how to specify a window and use Window aggregate functions.
Note:
More examples and details can be found at: pkg_install_location\teradataml\data\notebooks\
```python
sqlalchemy\Teradata Vantage Window Aggregate Functions using SQLAlchemy.ipynb.
```
In this example, you return the first value in column 'gpa', over a window of 3 values preceding the current
row and 1 value following the current row.
* Use the func module to generate a function object "FIRST_VALUE_".
```python
# Example returns by id first gpa in the moving average group.
>>> FIRST_VALUE_ =
func.FIRST_VALUE(admissions_train.gpa.expression).over(order_by=admissions_t
rain.id.expression, rows=(-3, 1))
```
* View the type of the function object "FIRST_VALUE_".
```python
>>> type(FIRST_VALUE_)
sqlalchemy.sql.elements.Over
```
* Pass the function object "FIRST_VALUE_" to DataFrame.assign() function to return the first value in
  column 'gpa'.
```python
>>> fv_gpa_df = admissions_train.assign(FIRST_VALUE_gpa=FIRST_VALUE_)
>>> print_variables(fv_gpa_df, "FIRST_VALUE_gpa")
Equivalent SQL: select id AS id, masters AS masters, gpa AS gpa, stats AS
stats, programming AS programming, admitted AS admitted,
FIRST_VALUE(gpa) OVER (ORDER BY id ROWS BETWEEN 3 PRECEDING AND 1
FOLLOWING) AS "FIRST_VALUE_gpa" from "admissions_train"
************************* DataFrame *********************
masters   gpa     stats programming  admitted  FIRST_VALUE_gpa
id
16      no  3.70  Advanced    Advanced         1             4.00
25      no  3.96  Advanced    Advanced         1             3.46
26     yes  3.57  Advanced    Advanced         1             3.59
1      yes  3.95  Beginner    Beginner         0             3.95
3       no  3.70    Novice    Beginner         1             3.95
34     yes  3.85  Advanced    Beginner         0             3.50
35      no  3.68    Novice    Beginner         1             3.46
36      no  3.00  Advanced      Novice         0             3.55
2      yes  3.76  Beginner    Beginner         0             3.95
24      no  1.87  Advanced      Novice         1             3.87
************************* DataFrame.dtypes *********************
id                   int
masters              str
gpa                float
stats                str
programming          str
admitted             int
FIRST_VALUE_gpa    float
'FIRST_VALUE_gpa' Column Type: FLOAT
```
#### Supported functions
| S/N Function Name | Description | Comment |
| ----------------- | ----------- | ------- |
| 1 CSUM | Returns the cumulative (or running) sum of a value expression for each row in a partition, assuming the rows in the partition are sorted by the sort_expression list. |  |
| 2 CUME_DIST | Calculates the cumulative distribution of a value in a group of values. |  |
| 3 DENSE_RANK | Returns an ordered ranking of rows based on the value_ expression in the ORDER BY clause. |  |
| 4 FIRST_VALUE | Returns the first value in an ordered set of values. |  |
| 5 LAST_VALUE | Returns the last value in an ordered set of values. |  |
| 6 LAG | Ordered analytic functions calculate an aggregate or non-aggregate value on a window of rows within a group of rows. The window of rows is defined by the Window Framing clause, also called the ROWS clause. Window sizes are based on the size specified in the ROWS clause. The group of rows is defined by the PARTITION BY clause of the Window function. The LAG function accesses data from the row preceding the current row at a specified offset value in a window group. If the offset value is outside the scope of the window, the user-specified default value is returned. |  |
| 7 LEAD | Ordered analytic functions calculate an aggregate or non-aggregate value on a window of rows within a group of rows. The window of rows is defined by the Window Framing clause, also called the ROWS clause. Window sizes are based on the size specified in the ROWS clause. The group of rows is defined by the PARTITION BY clause of the Window function. The LEAD function returns data from the row following the current row. If the offset value is outside the scope of the window, the user-specified default value is returned. |  |
| 8 MAVG | Computes the moving average of a value expression for each row in a partition using the specified value expression for the current row and the preceding width-1 rows. |  |
| 9 MDIFF | Returns the moving difference between the specified value expression for the current row and the preceding width rows for each row in the partition. |  |
| 10 MEDIAN | For numeric values, returns the middle value or an interpolated value that becomes the middle value after the values are sorted. Nulls are ignored in the calculation. | Supported with drop_ columns=True |
| 11 MLINREG | Returns a predicted value for an expression based on a least squares moving linear regression of the previous width -1 (based on sort_expression) column values. |  |
| 12 MSUM | Computes the moving sum specified by a value expression for the current row and the preceding n-1 rows. This function is similar to the MAVG function. |  |
| 13 PERCENT_RANK | Returns the relative rank of rows for a value_expression. |  |
| 14 PERCENTILE_ CONT / PERCENTILE_ DISC | Returns an interpolated value that falls within its value_ expression with respect to its sort specification. | Supported with drop_ columns=True |
| 15 QUANTILE | Computes the quantile scores for the values in a group. |  |
| 16 RANK(ANSI) | Returns an ordered ranking of rows based on the value_ expression in the ORDER BY clause. | Supported without "with ties" and "RESET WHEN" |
| 17 RANK(Teradata) | Returns the rank (1 …  n ) of all the rows in the group by the value of  sort_expression expression  values receiving the same rank. | list, with the same  sort_ |
| 18 ROW_NUMBER | Returns the sequential row number, where the first row is number one, of the row within its window partition according to the window ordering of the window. |  |

In addition to these functions, all 23 aggregate functions mentioned in the Aggregate Functions section
are also supported.
To use these aggregate functions as Window aggregates, you must add a window to the same using
over() function from SQLAlchemy, as show cased in the example notebook of Window Aggregates. Refer
to the example for AVG function for details.
For Window Aggregate functions to work, you must specify a window on top of these aggregate functions,
using SQLAlchemy. You can specify the following types of Windows:
* Cumulative / Expanding
* Moving / Rolling
* Remaining / Contracting
* Grouping
All of these types computation can be performed with teradataml.
Examples for the these can be found at: pkg_install_location\teradataml\data\notebooks\
```python
sqlalchemy\Teradata Vantage Window Aggregate Functions using SQLAlchemy.ipynb.
```
Note:
The following Teradata SQL syntax are not supported by SQLAlchemy:
* RESET WHEN clause
* ROWS UNBOUNDED PRECEDING
* ROWS value PRECEDING
* ROWS CURRENT ROW
Note:
SQLAlchemy offers range over in Window aggregate, Teradata does not support the same.
For example, "RANGE BETWEEN 5 PRECEDING AND 10 FOLLOWING" is not supported.
## Using SQLAlchemy Clause Element and Expression
teradataml is extended to work with different SQLAlchemy clause elements, expressions and functions.
These can be used in teradataml "DataFrame.assign()" method or as filter expression in slice filter ([]) for
teradataml DataFrame.
This section describes how the SQLAlchemy clause elements, expressions, and functions can be used
with teradataml.
* Using Basic SQLAlchemy ClauseElement and Expression in DataFrame.assign()
* Using Basic SQLAlchemy ClauseElement and Expression for Filtering
### Using Basic SQLAlchemy ClauseElement and Expression
### in DataFrame.assign()
With the help of SQLAlchemy, in teradataml, user not only can run SQL functions, but also can run some
other features that Vantage offers. Basically, user can use SQLAlchemy ClauseElements, representing
select_expression clauses in query.
teradataml can use SQLAlchemy ClauseElements to do following:
* Build a CASE expression on teradataml DataFrame column and select the using DataFrame.assign().
* Cast a teradataml DataFrame column.
* Select distinct values from a teradataml DataFrame Column.
* Extract values from teradataml column, using EXTRACT function.
Refer to example notebook pkg_install_location\teradataml\data\notebooks\sqlalchemy\
```python
teradataml assign() - Using Generic SQLAlchemy ClauseElements (Case, Cast, Extract,
Distinct).ipynb to learn about these.
```
### Using Basic SQLAlchemy ClauseElement and Expression
### for Filtering
Vantage offers various types of clauses in WHERE clause that allows users to filter the results in a table.
With the help of SQLAlchemy ClauseElements and Expression, teradataml can use the following SQL
clauses to filter results in teradataml DataFrame:
* CASE expression
* BETWEEN ... AND ... clause
* IN clause
* NOT IN clause
* LIKE clause
* NOT LIKE clause
* ILIKE clause
* IS NULL
* IS NOT NULL
Refer to example notebook pkg_install_location\teradataml\data\notebooks\sqlalchemy\
```python
teradataml filtering - Using SQLAlchemy ClauseElements.ipynb to learn about these.
```