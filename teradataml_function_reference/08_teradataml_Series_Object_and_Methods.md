# teradataml Series Object and Methods

__repr__
teradataml.series.series.Series.__repr__ = __repr__(self)
Returns the string representation for a teradataml Series instance.
The string contains:
    1. A default index for row numbers.
    2. At most the first max_rows rows of the series as mentioned in the note below.
    3. The name and datatype of the series.
NOTES:
  - This makes an explicit call to get rows from the database.
  - To change number of rows to be printed set the max_rows option in options.display.display
  - Default value of max_rows is 10
EXAMPLES:
    >>> df
       masters   gpa     stats programming admitted
    id
    22     yes  3.46    Novice    Beginner        0
    36      no  3.00  Advanced      Novice        0
    15     yes  4.00  Advanced    Advanced        1
    38     yes  2.65  Advanced    Beginner        1
    5       no  3.44    Novice      Novice        0
    17      no  3.83  Advanced    Advanced        1
    34     yes  3.85  Advanced    Beginner        0
    13      no  4.00  Advanced      Novice        1
    26     yes  3.57  Advanced    Advanced        1
    19     yes  1.98  Advanced    Advanced        0
    >>> gpa = df.select(["gpa"]).squeeze()
    >>> gpa
    0    4.00
    1    2.33
    2    3.46
    3    3.83
    4    4.00
    5    2.65
    6    3.57
    7    3.44
    8    3.85
    9    3.95
    Name: gpa, dtype: float64
head
teradataml.series.series.Series.head = head(self, n=10)
Print the first n rows of the sorted teradataml Series.
Note: The Series is sorted on the column composing the Series object.
The column type must support sorting.
Unsupported types: ['BLOB', 'CLOB', 'ARRAY', 'VARRAY']
PARAMETERS:
    n:
        Optional argument.
        Specifies the number of rows to select.
        Default Value: 10.
        Type: int
RETURNS:
    teradataml Series
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("dataframe", "admissions_train")
    >>> df = DataFrame("admissions_train")
    >>> df
       masters   gpa     stats programming admitted
    id
    22     yes  3.46    Novice    Beginner        0
    36      no  3.00  Advanced      Novice        0
    15     yes  4.00  Advanced    Advanced        1
    38     yes  2.65  Advanced    Beginner        1
    5       no  3.44    Novice      Novice        0
    17      no  3.83  Advanced    Advanced        1
    34     yes  3.85  Advanced    Beginner        0
    13      no  4.00  Advanced      Novice        1
    26     yes  3.57  Advanced    Advanced        1
    19     yes  1.98  Advanced    Advanced        0
    >>> gpa = df.select(["gpa"]).squeeze()
    >>> gpa
    0    4.00
    1    2.33
    2    3.46
    3    3.83
    4    4.00
    5    2.65
    6    3.57
    7    3.44
    8    3.85
    9    3.95
    Name: gpa, dtype: float64
    >>> gpa.head()
    0    2.33
    1    3.00
    2    3.13
    3    3.44
    4    3.46
    5    3.46
    6    3.45
    7    2.65
    8    1.98
    9    1.87
    Name: gpa, dtype: float64
    >>> gpa.head(15)
    0     2.33
    1     3.00
    2     3.13
    3     3.44
    4     3.46
    5     3.46
    6     3.50
    7     3.50
    8     3.50
    9     3.52
    10    3.55
    11    3.45
    12    2.65
    13    1.98
    14    1.87
    Name: gpa, dtype: float64
    >>> gpa.head(5)
    0    2.33
    1    3.00
    2    2.65
    3    1.98
    4    1.87
    Name: gpa, dtype: float64
name
teradataml.series.series.Series.name
RETURNS:
    The name of the Series
EXAMPLES:
    >>> gpa.name
    'gpa'
unique
teradataml.series.series.Series.unique = unique(self)
Return a Series object with unique values.
PARAMETERS:
    None
RETURNS:
    Series object with unique values.
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("dataframe", "admissions_train")
    >>> df = DataFrame.from_query('select admitted from admissions_train')
    >>> s = df.squeeze(axis = 1)
    >>> s