# Series (DataFrame Column Sequence)

* Series from teradataml DataFrame
* Series Manipulation
* Series Metadata
## Series from teradataml DataFrame
The Series object can be created from a DataFrame using the  squeeze() Method .
Series is a one-dimensional labeled representation of a single column DataFrame.
### Example
```python
df = DataFrame("admissions_train")
gpa = df.select(["gpa"])
gpa.squeeze()
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
```
## Series Manipulation
* head()
* unique()
### head()
Use the head() function to print the first  n  rows of a teradataml Series.
The function takes the optional argument  n , and prints the first  n  number of rows. The default value for  n
is 10.
13

**Note:**
The Series object is sorted on the column. The column type must support sorting.
Unsupported types include: 'BLOB', 'CLOB', 'ARRAY', 'VARRAY'.
### Example: Print default number of rows
**This example prints the default 10 rows of Series "gpa":**
```python
df = DataFrame("admissions_train")
gpa = df.select(["gpa"]).squeeze()
gpa.head()
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
```
### Examples: Print n rows
**The following example prints 5 rows of "gpa":**
```python
gpa.head(5)
0    2.33
1    3.00
2    2.65
3    1.98
4    1.87
Name: gpa, dtype: float64
```
**The following example prints 15 rows of "gpa":**
```python
gpa.head(15)
0     2.33
1     3.00
2     3.13
3     3.44
4     3.46
```
13: Series

```python
5     3.46
6     3.50
7     3.50
8     3.50
9     3.52
10    3.55
11    3.45
12    2.65
13    1.98
14    1.87
Name: gpa, dtype: float64
```
### unique()
Use the unique() function to return a new Series object with only the the unique or distinct values in
the Series.
```python
df = DataFrame.from_query('select admitted from admissions_train')
s = df.squeeze(axis = 1)
s
0    1
1    1
2    0
3    0
4    1
5    0
6    1
7    0
8    0
9    1
Name: admitted, dtype: object
s.unique()
0    0
1    1
Name: admitted, dtype: object
```
## Series Metadata
* name
13: Series

### name
Use the name property to get the name or the label of a Series.
```python
df = DataFrame("admissions_train")
gpa = df.select(["gpa"]).squeeze()
gpa
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
gpa.name
'gpa'
```
13: Series