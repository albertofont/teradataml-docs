# teradataml Time Series Methods

bottom
teradataml.dataframe.dataframe.DataFrameGroupByTime.bottom = bottom(self, number_of_values_to_column, with_ties=False)
    DESCRIPTION:
        Returns the smallest number of values in the columns for each group, with or without ties.
        Note:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        number_of_values_to_column:
            Required Argument.
            Specifies a dictionary that accepts number of values to be selected for each column.
            Number of values is a key in the dictionary. Key should be any positive integer.
            Whereas value in the dictionary can be a column name or list of column names.
            Sometimes, value can also include a special character '*', instead of column name.
            This should be used only when one wants to return same number of values for all columns.
            Types: Dictionary
            Examples:
                # Let's assume, a teradataml DataFrame has following columns:
                #   col1, col2, col3, ..., colN
                # For bottom() to return 2 values for column "col1":
                number_of_values_to_column = {2: "col1"}
                # For bottom() to return 2 values for column "col1" and 5 values for "col3":
                number_of_values_to_column = {2: "col1", 5: "col3"}
                # For bottom() to return 2 values for column "col1", "col2" and "col3":
                number_of_values_to_column = {2: ["col1", "col2", "col3"]}
                # Use cases for using '*' default value.
                # For bottom() to return 2 values for all columns. In case, we need to return 2 values
                # for each column in the DataFrame, then one can use '*'.
                number_of_values_to_column = {2: "*"}
                # For bottom() to return 2 values for column "col1" and "col3"
                # and 5 values for rest of the columns:
                number_of_values_to_column = {2: ["col1", "col3"], 5: "*"}
                # We can use default value column character ('*') in list as well
                # For bottom() to return 2 values for column "col1" and "col3"
                # and 5 values for "col4" and rest of the columns:
                number_of_values_to_column = {2: ["col1", "col3"], 5: ["col4", "*"]}
        with_ties:
            Optional Argument.
            Specifies a flag to decide whether to run bottom function with ties or not.
            BOTTOM WITH TIES implies that the rows returned include the specified number of rows in
            the ordered set for each timebucket. It includes any rows where the sort key value
            is the same as the sort key value in the last row that satisfies the specified number
            or percentage of rows. If this clause is omitted and ties are found, the earliest
            value in terms of timecode is returned.
            Types: bool
    RETURNS:
        teradataml DataFrame
    RAISES:
        TypeError - If incorrect type of values passed to input argument.
        ValueError - If invalid value passed to the the argument.
        TeradataMLException
            1. If required argument 'number_of_values_to_column' is missing or None is passed.
            2. TDMLDF_AGGREGATE_FAILED - If bottom() operation fails to
                generate the column-wise smallest number of values for the columns.
    EXAMPLES :
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        >>> # DataFrame on sequenced PTI table
        ... ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq.head()
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        0       2014-01-06 08:00:00.000000        26        55         10.0  2016-02-26
        0       2014-01-06 08:08:59.999999        18        55          NaN  2015-06-18
        1       2014-01-06 09:02:25.122200        24        55         78.0  2015-12-24
        1       2014-01-06 09:01:25.122200        23        55         77.0  2015-11-23
        1       2014-01-06 09:02:25.122200        12        55         71.0  2014-12-12
        1       2014-01-06 09:03:25.122200        13        55         72.0  2015-01-13
        1       2014-01-06 09:01:25.122200        11        55         70.0  2014-11-11
        0       2014-01-06 08:10:00.000000        19        55         10.0  2015-07-19
        0       2014-01-06 08:09:59.999999        17        55         99.0  2015-05-17
        0       2014-01-06 08:10:00.000000        27        55        100.0  2016-03-27
        >>> # DataFrame on NON-PTI table
        ... ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_nonpti.columns
        ['buoyid', 'timecode', 'temperature', 'salinity']
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
        ### Examples for bottom without ties ###
        #
        # Example 1: Executing bottom function on DataFrame created on non-sequenced PTI table.
        #
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="MINUTES(2)",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_grpby1.bottom(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                     530161       0                10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                     530162       0                 NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                     530163       0                 NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                     530164       0                 NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                     530165       0                99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                     530166       0               100.0
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                     530166       0                10.0
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     530191       1                70.0
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     530191       1                77.0
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                     530192       1                71.0
        #
        # Example 2: Executing bottom to select 2 values for all the columns in ocean_buoys_seq DataFrame
        #            on sequenced PTI table.
        #
        >>> ocean_buoys_seq_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="MINUTES(2)",
        ...                                                       value_expression="buoyid", fill="NULLS")
        >>> number_of_values_to_column = {2: "*"}
        >>> ocean_buoys_seq_grpby1.bottom(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom2TD_SEQNO  bottom2salinity  
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-
    01-...                     530161       0             26.0             55.0                10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-
    01-...                     530162       0              NaN              NaN                 NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-
    01-...                     530163       0              NaN              NaN                 NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-
    01-...                     530164       0              NaN              NaN                 NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-
    01-...                     530165       0             17.0             55.0                99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-
    01-...                     530166       0             19.0             55.0                10.0
        6  ('2014-01-06 09:00:00.000000+00:00', '2014-
    01-...                     530191       1             11.0             55.0                70.0
        7  ('2014-01-06 09:02:00.000000+00:00', '2014-
    01-...                     530192       1             12.0             55.0                71.0
        8  ('2014-01-06 10:00:00.000000+00:00', '2014-
    01-...                     530221      44              4.0             55.0                43.0
        9  ('2014-01-06 10:02:00.000000+00:00', '2014-
    01-...                     530222      44              9.0             55.0                53.0
        #
        # Example 3: Executing bottom function on DataFrame created on NON-PTI table.
        #
        >>> ocean_buoys_nonpti_grpby1 = ocean_buoys_nonpti.groupby_time(timebucket_duration="MINUTES(2)",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby1.bottom(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961       0                10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962       0                 NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963       0                 NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964       0                 NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965       0                99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0               100.0
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                10.0
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                70.0
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                77.0
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992       1                71.0
        ### Examples for bottom with ties ###
        #
        # Example 4: Executing bottom with ties function on DataFrame created on non-sequenced PTI table.
        #
        >>> ocean_buoys_grpby2 = ocean_buoys.groupby_time(timebucket_duration="MINUTES(2)",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                     530161       0                          10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                     530162       0                           NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                     530163       0                           NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                     530164       0                           NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                     530165       0                          99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                     530166       0                         100.0
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                     530166       0                          10.0
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     530191       1                          77.0
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     530191       1                          70.0
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                     530192       1                          71.0
        #
        # Example 5: Executing bottom with ties to select 2 values for temperature and 3 for rest of the columns in
        #            ocean_buoys DataFrame.
        #
        >>> ocean_buoys_grpby3 = ocean_buoys.groupby_time(timebucket_duration="MINUTES(2)", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature", 3:"*"}
        >>> ocean_buoys_grpby3.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  bottom_with_ties3buoyid  bottom_with_ties3
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-
    01-...                     530161                      0.0                       55.0                          10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-
    01-...                     530162                      NaN                        NaN                           NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-
    01-...                     530163                      NaN                        NaN                           NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-
    01-...                     530164                      NaN                        NaN                           NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-
    01-...                     530165                      0.0                       55.0                          99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-
    01-...                     530166                      0.0                       55.0                          10.0
        6  ('2014-01-06 08:12:00.000000+00:00', '2014-
    01-...                     530167                      NaN                        NaN                           NaN
        7  ('2014-01-06 08:14:00.000000+00:00', '2014-
    01-...                     530168                      NaN                        NaN                           NaN
        8  ('2014-01-06 08:16:00.000000+00:00', '2014-
    01-...                     530169                      NaN                        NaN                           NaN
        9  ('2014-01-06 08:18:00.000000+00:00', '2014-
    01-...                     530170                      NaN                        NaN                           NaN
        >>>
        #
        # Example 6: Executing bottom with ties function on DataFrame created on NON-PTI table.
        #
        >>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.groupby_time(timebucket_duration="MINUTES(2)",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby2.bottom(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  bottom_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961       0                          10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962       0                           NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963       0                           NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964       0                           NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965       0                          99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                          10.0
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                         100.0
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                          77.0
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                          70.0
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992       1                          71.0
        >>>
        >>>
    count
    teradataml.dataframe.dataframe.DataFrameGroupByTime.count = count(self, distinct=False)
    DESCRIPTION:
        Returns column-wise count of the dataframe.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the count.
            Default Value: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with count() operation
        performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If count() operation fails to
            generate the column-wise count of the dataframe.
            Possible error message:
            Failed to perform 'count'. (Followed by error message).
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the count() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'count' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df2 = df1.select(['employee_no', 'first_name', 'marks'])
        # Prints count of the values in all the selected columns
        # (excluding None types).
        >>> df2.count()
          count_employee_no count_first_name count_marks
        0                 3                2           0
        >>>
        #
        # Using count() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys_seq"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing count() function on DataFrame created on
        #                                  sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the count.
        #
        >>> ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        44      2014-01-06 10:00:25.122200         6        55           43  2014-06-06
        44      2014-01-06 10:01:25.122200         8        55           53  2014-08-08
        44      2014-01-06 10:01:25.122200        20        55           54  2015-08-20
        1       2014-01-06 09:01:25.122200        11        55           70  2014-11-11
        1       2014-01-06 09:02:25.122200        12        55           71  2014-12-12
        1       2014-01-06 09:02:25.122200        24        55           78  2015-12-24
        1       2014-01-06 09:03:25.122200        13        55           72  2015-01-13
        1       2014-01-06 09:03:25.122200        25        55           79  2016-01-25
        1       2014-01-06 09:01:25.122200        23        55           77  2015-11-23
        44      2014-01-06 10:00:26.122200         7        55           43  2014-07-07
        >>>
        # To use count() as Time Series Aggregate we must run groupby_time() first, followed by count().
        >>> ocean_buoys_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="2cy", value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.count().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  count_TD_TIMECODE  count_TD_SEQN
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                  5               5               5                  4            5
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                  6               6               6                  6            6
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                  3               3               3                  3            3
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      22                  1               1               1                  1            1
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                 13              13              13                 13           13
        >>>
        #
        # Time Series Aggregate Example 2: Executing count() function on DataFrame created on
        #                                  sequenced PTI table. We will consider DISTINCT rows for the
        #                                  columns while calculating the count.
        #
        # To use count() as Time Series Aggregate we must run groupby_time() first, followed by count().
        >>> ocean_buoys_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="2cy",value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.count(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  count_TD_TIMECODE  count_TD_SEQN
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                  4               5               1                  3            5
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                  3               6               1                  6            6
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                  3               3               1                  3            3
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      22                  1               1               1                  1            1
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                 10              13               1                  5           13
        >>>
    delta_t
    teradataml.dataframe.dataframe.DataFrameGroupByTime.delta_t = delta_t(self, start_condition, end_condition)
    DESCRIPTION:
        Calculates the time difference, or DELTA_T, between a starting and an ending event.
        The calculation is performed against a time-ordered time series data set.
        Note:
            1. This is the only Time Series Aggregate function that works with timebucket_duration as "*"
               in groupby_time(), i.e., unbounded time.
            2. When using groupby_time() with unbounded time, the following rules apply to
               the system virtual columns:
                a. $TD_GROUP_BY_TIME: Always has a value of 1, since there is only one timebucket.
                b. $TD_TIMECODE_RANGE: Composed of the first and last timecode values read for the group.
            Note that the data being evaluated in the filtering conditions (for example, the minimum and
            maximum temperature observation) must belong to the timecode value present in the same row
            of data. This is the expected behavior. However, this assumption can be violated when
            joining multiple tables together. It is possible to construct a query where the result of a
            join causes specific data points (for example, a temperature reading) to be present in a
            data row with a timecode that is not indicative of when that data point occurred.
            In such a scenario, it is highly likely that the results are not as expected, or are misleading.
            Vantage does not detect these types of queries, so one must make sure to preserve the
            correlation between data points and timecodes.
    PARAMETERS:
        start_condition:
            Required Argument.
            Specifies any supported filtering condition that defines the start of the time period for which
            you are searching.
            Types: str or ColumnExpression
        end_condition:
            Required Argument.
            Specifies any supported filtering condition that defines the end of the time period for which
            you are searching.
            Types:  str or ColumnExpression
    RETURNS:
        teradataml DataFrame
        Note:
            1. Function returns a column of PERIOD(TIMESTAMP WITH TIME ZONE) type (Vantage Data type)
               composed of the start and end timecode, i.e., timecode column used for aggregation
               of each start-end pair.
            2. One result is returned per complete start-end pair found within the
               GROUP BY TIME window. The start-end pair process is as follows:
                a. If the current source data meets the start condition, the current
                   timecode is saved as the start time.
                b. If the current source data meets the end condition, and a saved start
                   timecode already exists, the start timecode is saved with the end timecode
                   encountered as a result pair.
            3. The processing algorithm implies that multiple results may be found in each group.
            4. If no start-end pair is encountered, no result row is returned.
            5. Any result of delta_t which has a delta less than 1 microsecond (including a delta of 0,
               in the case of a result which comes from a single point in time) is automatically
               rounded to 1 microsecond.
               This is strictly enforced to match Period data type semantics in Vantage which dictate that a
               starting and ending bound of a Period type may not be equivalent. The smallest granularity
               supported in Vantage is the microsecond, so these results are rounded accordingly.
    RAISES:
        TypeError - If incorrect type of values passed to input argument.
        ValueError - If invalid value passed to the the argument.
        TeradataMLException - In case illegal conditions are passed
    EXAMPLES :
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti", "package_tracking_pti", "packag
        >>>
        #
        # Example 1: Finding Time Elapsed between Shipping and Receiving an Item.
        #            Input data used for this example contains information about parcels
        #            sent by a delivery service.
        #
        #
        # Case 1: Using DataFrame on PTI Table and showcasing usage of unbounded time in grouping.
        #
        >>> # Create DataFrame on PTI table
        ... package_tracking_pti = DataFrame("package_tracking_pti")
        >>> package_tracking_pti.columns
        ['TD_TIMECODE', 'parcelnumber', 'status']
        >>> package_tracking_pti
                                     TD_TIMECODE                          status
        parcelnumber
        55            2016-10-15 10:00:00.000000       in transit to destination
        75            2016-10-15 16:30:00.000000          in transit to customer
        75            2016-10-15 08:00:00.000000         picked up from customer
        55            2016-10-15 09:10:00.000000    arrived at receiving station
        60            2016-10-15 10:45:00.000000    arrived at receiving station
        75            2016-10-15 17:00:00.000000           delivered to customer
        59            2016-10-15 08:05:00.000000         picked up from customer
        79            2016-10-15 08:05:00.000000         picked up from customer
        60            2016-10-15 09:20:00.000000         picked up from customer
        75            2016-10-15 16:10:00.000000  arrived at destination station
        >>>
        >>> # Execute groupby_time() using unbounded time for timebucket_duration.
        ... gbt = package_tracking_pti.groupby_time("*", "parcelnumber")
        >>> # Execute delta_t, with start and end conditions specified as String.
        ... start_condition = "status LIKE 'picked%up%customer'"
        >>> end_condition = "status LIKE 'delivered%customer'"
        >>> gbt.delta_t(start_condition, end_condition)
                                              TIMECODE_RANGE  parcelnumber                                delta_t_td_timecode
        0  ('2012-01-01 00:00:00.000000+00:00', '9999-12-...            75  ('2016-10-15 08:00:00.000000-00:00', '2016-10-...
        1  ('2012-01-01 00:00:00.000000+00:00', '9999-12-...            55  ('2016-10-15 08:00:00.000000-00:00', '2016-10-...
        >>>
        #
        # Case 2: Using DataFrame on Non-PTI Table and showcasing usage of unbounded time in grouping.
        #
        >>> # Create DataFrame on Non-PTI table
        ... package_tracking = DataFrame("package_tracking")
        >>> package_tracking.columns
        ['parcelnumber', 'clock_time', 'status']
        >>> package_tracking
                                      clock_time                          status
        parcelnumber
        79            2016-10-15 08:05:00.000000         picked up from customer
        75            2016-10-15 09:10:00.000000    arrived at receiving station
        75            2016-10-15 10:00:00.000000       in transit to destination
        75            2016-10-15 16:10:00.000000  arrived at destination station
        75            2016-10-15 17:00:00.000000           delivered to customer
        80            2016-10-15 09:20:00.000000         picked up from customer
        59            2016-10-15 08:05:00.000000         picked up from customer
        75            2016-10-15 16:30:00.000000          in transit to customer
        75            2016-10-15 08:00:00.000000         picked up from customer
        60            2016-10-15 10:45:00.000000    arrived at receiving station
        >>>
        >>> # Execute groupby_time() using unbounded time for timebucket_duration.
        ... gbt = package_tracking.groupby_time("*", "parcelnumber", "clock_time")
        >>> # Execute delta_t, with start and end conditions specified as String.
        ... start_condition = "status LIKE 'picked%up%customer'"
        >>> end_condition = "status LIKE 'delivered%customer'"
        >>> gbt.delta_t(start_condition, end_condition)
                                              TIMECODE_RANGE  parcelnumber                                delta_t_td_timecode
        0  ('1970-01-01 00:00:00.000000+00:00', '9999-12-...            75  ('2016-10-15 08:00:00.000000-00:00', '2016-10-...
        1  ('1970-01-01 00:00:00.000000+00:00', '9999-12-...            55  ('2016-10-15 08:00:00.000000-00:00', '2016-10-...
        >>>
        #
        # Example 2: Searching the Minimum and Maximum Observed Temperatures
        #            This example measures the time between minimum and maximum observed temperatures every
        #            30 minutes between 8:00 AM and 10:30 AM on each buoy.
        #
        #
        # Case 1: DataFrame on Non-sequenced PTI Table - specifying start condition and end condition as string
        #
        >>> # Create DataFrame
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        >>> # Filter the data and grab all rows between  timestamp '2014-01-06 08:00:00' and '2014-01-06 10:30:00'
        ... ocean_buoys_dt = ocean_buoys[(ocean_buoys.TD_TIMECODE >= '2014-01-06 08:00:00') & (ocean_buoys.TD_TIMECODE < '2014-
    01-06 10:30:00')]
        >>>
        >>> # Let's get the minimum and maximum temperature within time range of 30 minutes
        ... df_min_max_temp1 = ocean_buoys_dt.groupby_time("MINUTES(30)", "buoyid", "TD_TIMECODE").agg({"temperature": ["min", "max
        >>> # Join the dataframe with original 'ocean_buoys'
        ... df2_join1 = ocean_buoys.join(df_min_max_temp1, on="buoyid", how="inner", lsuffix="t1", rsuffix="t2")
        >>> gbt3 = df2_join1.groupby_time("DAYS(1)", "t1_buoyid", timecode_column="TD_TIMECODE")
        >>>
        >>> # Let's set the start and end conditions
        ... start_condition = "temperature = min_temperature"
        >>> end_condition = "temperature = max_temperature"
        >>> gbt3.delta_t(start_condition, end_condition)
                                              TIMECODE_RANGE  GROUP BY TIME(DAYS(1))  t1_buoyid                                delt
        0  ('2014-01-06 00:00:00.000000+00:00', '2014-01-...                   16077         44  ('2014-01-06 10:00:26.122200-
    00:00', '2014-01-...
        1  ('2014-01-06 00:00:00.000000+00:00', '2014-01-...                   16077          1  ('2014-01-06 09:01:25.122200-
    00:00', '2014-01-...
        2  ('2014-01-06 00:00:00.000000+00:00', '2014-01-...                   16077          0  ('2014-01-06 08:00:00.000000-
    00:00', '2014-01-...
        >>>
        #
        # Case 2: Same example as that of above, just DataFrame on Sequenced PTI Table and
        #         specifying start condition and end condition as ColumnExpression.
        #
        >>> # Create DataFrame
        ... ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq.head()
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        0       2014-01-06 08:00:00.000000        26        55         10.0  2016-02-26
        0       2014-01-06 08:08:59.999999        18        55          NaN  2015-06-18
        1       2014-01-06 09:02:25.122200        24        55         78.0  2015-12-24
        1       2014-01-06 09:01:25.122200        23        55         77.0  2015-11-23
        1       2014-01-06 09:02:25.122200        12        55         71.0  2014-12-12
        1       2014-01-06 09:03:25.122200        13        55         72.0  2015-01-13
        1       2014-01-06 09:01:25.122200        11        55         70.0  2014-11-11
        0       2014-01-06 08:10:00.000000        19        55         10.0  2015-07-19
        0       2014-01-06 08:09:59.999999        17        55         99.0  2015-05-17
        0       2014-01-06 08:10:00.000000        27        55        100.0  2016-03-27
        >>> # Filter the data and grab all rows between  timestamp '2014-01-06 08:00:00' and '2014-01-06 10:30:00'
        ... ocean_buoys_seq_dt = ocean_buoys_seq[(ocean_buoys_seq.TD_TIMECODE >= '2014-01-
    06 08:00:00') & (ocean_buoys_seq.TD_TIMECODE < '2014-01-06 10:30:00')]
        >>>
        >>> # Let's get the minimum and maximum temperature within time range of 30 minutes
        ... df_min_max_temp2 = ocean_buoys_seq_dt.groupby_time("MINUTES(30)", "buoyid", "TD_TIMECODE").agg({"temperature": ["min", 
        >>> # Join the dataframe with original 'ocean_buoys'
        ... df2_join2 = ocean_buoys_seq.join(df_min_max_temp2, on="buoyid", how="inner", lsuffix="t1", rsuffix="t2")
        >>> gbt4 = df2_join2.groupby_time("DAYS(1)", "t1_buoyid", timecode_column="TD_TIMECODE")
        >>>
        >>> # Let's set the start and end conditions
        >>> start_condition = gbt4.temperature == gbt4.min_temperature
        >>> end_condition = gbt4.temperature == gbt4.max_temperature
        >>> gbt4.delta_t(start_condition, end_condition)
                                              TIMECODE_RANGE  GROUP BY TIME(DAYS(1))  t1_buoyid                                delt
        0  ('2014-01-06 00:00:00.000000+00:00', '2014-01-...                   16077         44  ('2014-01-06 10:00:26.122200-
    00:00', '2014-01-...
        1  ('2014-01-06 00:00:00.000000+00:00', '2014-01-...                   16077          1  ('2014-01-06 09:01:25.122200-
    00:00', '2014-01-...
        2  ('2014-01-06 00:00:00.000000+00:00', '2014-01-...                   16077          0  ('2014-01-06 08:00:00.000000-
    00:00', '2014-01-...
        >>>
    describe
    teradataml.dataframe.dataframe.DataFrameGroupByTime.describe = describe(self, percentiles=[0.25, 0.5, 0.75], verbose=False, distinct=False, statistics=None,
    columns=None, pivot=False)
    DESCRIPTION:
        Generates statistics for numeric columns. This function can be used in two modes:
            1. Regular Aggregate Mode.
                It computes the count, mean, std, min, percentiles, and max for numeric columns.
                Default statistics include:
                    "count", "mean", "std", "min", "percentile", "max"
            2. Time Series Aggregate Mode.
                It computes max, mean, min, std, median, mode, and percentiles for numeric columns.
                Default statistics include:
                    'max', 'mean', 'min', 'std'
        Note:
            Regular Aggregate Mode: If describe() is used on the output of any DataFrame API or groupby(),
                                    then describe() is used as regular aggregation.
            Time Series Aggregate Mode: If describe() is used on the output of groupby_time(), then describe()
                                        is a time series aggregate, where time series aggregates are used
                                        to calculate the statistics.
    PARAMETERS:
        percentiles:
            Optional Argument.
            A list of values between 0 and 1. Applicable for both modes.
            By default, percentiles are calculated for statistics for 'Regular Aggregate Mode', whereas
            for 'Time Series Aggregate Mode', percentiles are calculated when verbose is set to True.
            Default Values: [.25, .5, .75], which returns the 25th, 50th, and 75th percentiles.
            Types: float or List of floats
        verbose:
            Optional Argument.
            Specifies a boolean value to be used for time series aggregation, stating whether to get
            verbose output or not.
            When this argument is set to 'True', function calculates median, mode, and percentile values
            on top of its default statistics.
            Note:
                verbose as 'True' is not applicable for 'Regular Aggregate Mode'.
            Default Values: False
            Types: bool
        distinct:
            Optional Argument.
            Specifies a boolean value to decide whether to consider duplicate rows in statistic
            calculation or not. By default, duplicate values are considered for statistic calculation.
            When this is set to True, only distinct rows are considered for statistic calculation.
            Default Values: False
            Types: bool
        statistics:
            Optional Argument.
            Specifies the aggregate operation to be performed.
            Computes count, mean, std, min, percentiles, and max for numeric columns.
            Computes count and unique for non-numeric columns.
            Notes:
                1. statistics is not applicable for 'Time Series Aggregate Mode'.
            Permitted Values: count, mean, min, max, unique, std, describe, percentile
            Default Values: None
            Types: str or List of str
        columns:
            Optional Argument.
            Specifies the name(s) of the columns we are collecting statistics for.
            Default Values: None
            Types: str or List of str
        pivot:
            Optional Argument.
            Specifies a boolean value to pivot the output.
            Note:
                * "pivot" is not supported for PTI tables.
            Default Values: 'False'
            Types: bool
    RETURNS:
        teradataml DataFrame
    RAISE:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("dataframe","sales")
        >>> df = DataFrame('sales')
        >>> print(df)
                      Feb   Jan   Mar   Apr    datetime
        accounts
        Blue Inc     90.0    50    95   101  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        Jones LLC   200.0   150   140   180  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        # Computes count, mean, std, min, percentiles, and max for numeric columns.
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
        # Computes count, mean, std, min, percentiles, and max for numeric columns with 
        # default arugments.
        >>> df.describe()
        ATTRIBUTE   StatName            StatValue
        Jan         MAXIMUM             200.0
        Jan         STANDARD DEVIATION      62.91528696058958
        Jan         PERCENTILES(25)     125.0
        Jan         PERCENTILES(50)     150.0
        Mar         COUNT               4.0
        Mar         MINIMUM             95.0
        Mar         MAXIMUM             215.0
        Mar         MEAN                147.5
        Mar         STANDARD DEVIATION      49.749371855331
        Mar         PERCENTILES(25)     128.75
        Mar         PERCENTILES(50)     140.0
        Apr         COUNT               4.0
        Apr         MINIMUM             101.0
        Apr         MAXIMUM             250.0
        Apr         MEAN                195.25
        Apr         STANDARD DEVIATION      70.97123830585646
        Apr         PERCENTILES(25)     160.25
        Apr         PERCENTILES(50)     215.0
        Apr         PERCENTILES(75)     250.0
        Feb         COUNT               6.0
        Feb         MINIMUM             90.0
        Feb         MAXIMUM             210.0
        Feb         MEAN                166.66666666666666
        Feb         STANDARD DEVIATION      59.553897157672786
        Feb         PERCENTILES(25)     117.5
        Feb         PERCENTILES(50)     200.0
        Feb         PERCENTILES(75)     207.5
        Mar         PERCENTILES(75)     158.75
        Jan         PERCENTILES(75)     162.5
        Jan         MEAN                137.5
        Jan         MINIMUM             50.0
        Jan         COUNT               4.0
        # Computes count, mean, std, min, percentiles, and max for numeric columns with 30th and 60th percentiles.
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
        # Computes count, mean, std, min, percentiles, and max for numeric columns group by "datetime" and "Feb".
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
        # Examples for describe() function as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        >>> ocean_buoys_grpby = ocean_buoys.groupby_time(timebucket_duration="2cy", value_expression="buoyid", fill="NULLS")
        >>>
        #
        # Example 1: Get the basic statistics for time series aggregation for all the numeric columns.
        #            This returns max, mean, min and std values.
        #
        >>> ocean_buoys_grpby.describe()
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
        >>>
        #
        # Example 2: Get the verbose statistics for time series aggregation for all the numeric columns.
        #            This returns max, mean, min, std, median, mode, 25th, 50th and 75th percentile.
        #
        >>> ocean_buoys_grpby.describe(verbose=True)
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
        >>>
        #
        # Example 3: Get the basic statistics for time series aggregation for all the numeric columns,
        #            consider only unique values.
        #            This returns max, mean, min and std values.
        #
        >>> ocean_buoys_grpby.describe(distinct=True)
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
        >>>
        #
        # Example 4: Get the verbose statistics for time series aggregation for all the numeric columns.
        #            This select non-default percentiles 33rd and 66th.
        #            This returns max, mean, min, std, median, mode, 33rd, and 66th percentile.
        #
        >>> ocean_buoys_grpby.describe(verbose=True, percentiles=[0.33, 0.66])
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
        >>>
    first
    teradataml.dataframe.dataframe.DataFrameGroupByTime.ﬁrst = ﬁrst(self, columns=None)
    DESCRIPTION:
        Returns the oldest value, determined by the timecode, for each group. FIRST is a single-threaded function.
        In the event of a tie, such as simultaneous timecode values for a particular group, all tied results
        are returned. If a sequence number is present with the data, it can break a tie, assuming it is unique
        across identical timecode values.
        Note:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        columns:
            Optional Argument.
            Specifies a column name or list of column names on which first() operation
            must be run. By default oldest value is returned for all the compatible columns
            in a teradataml DataFrame.
            Types: str OR list of Strings (str)
    RETURNS:
        teradataml DataFrame object with first() operation performed.
    RAISES:
        1. TDMLDF_AGGREGATE_FAILED - If first() operation fails to
            return oldest value of columns in the teradataml DataFrame.
            Possible error message:
            Unable to perform 'first()' on the teradataml DataFrame.
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the first() operation
            doesn't support all the columns in the teradataml DataFrame.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'first' operation.
    EXAMPLES :
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti"])
        >>>
        #
        # Example 1: Executing first function on DataFrame created on non-sequenced PTI table.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cd",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.first().sort(["TIMECODE_RANGE", "buoyid"])
        /mnt/c/Users/pp186043/GitHub_Repos/pyTeradata/teradataml/common/utils.py:398: VantageRuntimeWarning: [Teradata]
    [teradataml](TDML_2086) Following warning raised from Vantage with warning code: 4001
        [Teradata Database] [Warning 4001] Time Series Auxiliary Cache Warning: Multiple results found for one or more Time Series 
          VantageRuntimeWarning)
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(2))  buoyid  first_salinity  first_temperature
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       0              55                 10
        1  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       1              55                 70
        2  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       2              55                 80
        3  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369      44              55                 43
        >>>
        #
        # Example 2: In Example 1, a VantageRuntimeWarning is raised as:
        #           "[Teradata Database] [Warning 4001] Time Series Auxiliary Cache Warning: Multiple results
        #            found for one or more Time Series aggregate functions in this query, but only one result
        #            was returned. To get all results, resubmit this query with these aggregates isolated."
        #
        #            This warning recommends to execute first() independently on each column, so that we will get
        #            all the results.
        #            To run first() on one single column we can pass column name as input. Let's run first()
        #            on 'temperature' column.
        #
        >>> ocean_buoys_grpby1.first('temperature')
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(2))  buoyid  first_temperature
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                         369       2                 80
        1  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                         369       0                 10
        2  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                         369      44                 43
        3  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                         369       1                 77
        4  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                         369       1                 70
        >>>
        #
        # Example 3: Executing first function on ocean_buoys_seq DataFrame created on sequenced PTI table.
        #            Table has few columns incompatible for first() operation 'dates' and 'TD_TIMECODE',
        #            while executing this first() incompatible columns are ignored.
        #
        >>> # DataFrame on sequenced PTI table
        ... ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq.head()
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        0       2014-01-06 08:00:00.000000        26        55         10.0  2016-02-26
        0       2014-01-06 08:08:59.999999        18        55          NaN  2015-06-18
        1       2014-01-06 09:02:25.122200        24        55         78.0  2015-12-24
        1       2014-01-06 09:01:25.122200        23        55         77.0  2015-11-23
        1       2014-01-06 09:02:25.122200        12        55         71.0  2014-12-12
        1       2014-01-06 09:03:25.122200        13        55         72.0  2015-01-13
        1       2014-01-06 09:01:25.122200        11        55         70.0  2014-11-11
        0       2014-01-06 08:10:00.000000        19        55         10.0  2015-07-19
        0       2014-01-06 08:09:59.999999        17        55         99.0  2015-05-17
        0       2014-01-06 08:10:00.000000        27        55        100.0  2016-03-27
        >>> ocean_buoys_seq_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="2cd",
        ...                                                       value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_seq_grpby1.first().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(2))  buoyid  first_TD_SEQNO  first_salinity  f
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       0              26              55                 10
        1  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       1              11              55                 70
        2  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       2              14              55                 80
        3  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369      22               1              25                 23
        4  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369      44               4              55                 43
        >>>
        #
        # Example 4: Executing first function on DataFrame created on NON-PTI table.
        #
        >>> # DataFrame on NON-PTI table
        ... ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_nonpti.columns
        ['buoyid', 'timecode', 'temperature', 'salinity']
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
        >>> ocean_buoys_nonpti_grpby1 = ocean_buoys_nonpti.groupby_time(timebucket_duration="2cd",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="NULLS")
        >>> ocean_buoys_nonpti_grpby1.first().sort(["TIMECODE_RANGE", "buoyid"])
        /mnt/c/Users/pp186043/GitHub_Repos/pyTeradata/teradataml/common/utils.py:398: VantageRuntimeWarning: [Teradata]
    [teradataml](TDML_2086) Following warning raised from Vantage with warning code: 4001
        [Teradata Database] [Warning 4001] Time Series Auxiliary Cache Warning: Multiple results found for one or more Time Series 
          VantageRuntimeWarning)
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(2))  buoyid  first_salinity  first_temperature
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                        8039       0              55                 10
        1  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                        8039       1              55                 70
        2  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                        8039       2              55                 80
        3  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                        8039      44              55                 43
        >>>
        #
        # Example 5: Execute first() on a few select columns 'temperature' and 'salinity'.
        #
        >>> ocean_buoys_seq_grpby1.first(["temperature", "salinity"]).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(2))  buoyid  first_temperature  first_salinity
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       0                 10              55
        1  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       1                 70              55
        2  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       2                 80              55
        3  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369      22                 23              25
        4  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369      44                 43              55
        >>>
    kurtosis
    teradataml.dataframe.dataframe.DataFrameGroupByTime.kurtosis = kurtosis(self, distinct=False)
    DESCRIPTION:
        Returns column-wise kurtosis value of the dataframe.
        Kurtosis is the fourth moment of the distribution of the standardized (z) values.
        It is a measure of the outlier (rare, extreme observation) character of the distribution as
        compared with the normal (or Gaussian) distribution.
            * The normal distribution has a kurtosis of 0.
            * Positive kurtosis indicates that the distribution is more outlier-prone than the
              normal distribution.
            * Negative kurtosis indicates that the distribution is less outlier-prone than the
              normal distribution.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
            3. Following conditions will produce null result:
                a. Fewer than three non-null data points in the data used for the computation.
                b. Standard deviation for a column is equal to 0.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the kurtosis value.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with kurtosis()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If kurtosis() operation fails to
           generate the column-wise kurtosis value of the dataframe.
           Possible error message:
           Failed to perform 'kurtosis'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the kurtosis() operation
           doesn't support all the columns in the dataframe.
           Possible error message:
           No results. Below is/are the error message(s):
           All selected columns [(col2 -  PERIOD_TIME), (col3 -
           BLOB)] is/are unsupported for 'kurtosis' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["admissions_train"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("admissions_train")
        >>> print(df1.sort("id"))
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        2      yes  3.76  Beginner    Beginner         0
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        8       no  3.60  Beginner    Advanced         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        >>>
        # Prints kurtosis value of each column
        >>> df1.kurtosis()
           kurtosis_id  kurtosis_gpa  kurtosis_admitted
        0         -1.2      4.052659            -1.6582
        >>>
        #
        # Using kurtosis() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        #
        # Time Series Aggregate Example 1: Executing kurtosis() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the kurtosis values.
        #
        # To use kurtosis() as Time Series Aggregate we must run groupby_time() first, followed by kurtosis().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.kurtosis().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid kurtosis_salinity  kurtosis_tempe
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0              None             -5.998128
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1              None             -2.758377
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2              None                   NaN
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44              None             -2.195395
        >>>
        #
        # Time Series Aggregate Example 2: Executing kurtosis() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the kurtosis value.
        #
        # To use kurtosis() as Time Series Aggregate we must run groupby_time() first, followed by kurtosis().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.kurtosis(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid kurtosis_salinity  kurtosis_tempe
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0              None                   NaN
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1              None             -2.758377
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2              None                   NaN
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44              None              4.128426
        >>>
    last
    teradataml.dataframe.dataframe.DataFrameGroupByTime.last = last(self, columns=None)
    DESCRIPTION:
        Returns the newest value, determined by the timecode, for each group. LAST is a single-threaded function.
        In the event of a tie, such as simultaneous timecode values for a particular group, all tied results
        are returned. If a sequence number is present with the data, it can break a tie, assuming it is unique
        across identical timecode values.
        Note:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        columns:
            Optional Argument.
            Specifies a column name or list of column names on which last() operation
            must be run. By default newest value is returned for all the compatible columns
            in a teradataml DataFrame.
            Types: str OR list of Strings (str)
    RETURNS:
        teradataml DataFrame object with last() operation performed.
    RAISES:
        1. TDMLDF_AGGREGATE_FAILED - If last() operation fails to
            return newest value of columns in the teradataml DataFrame.
            Possible error message:
            Unable to perform 'last()' on the teradataml DataFrame.
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the last() operation
            doesn't support all the columns in the teradataml DataFrame.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'last' operation.
    EXAMPLES :
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti"])
        >>>
        #
        # Example 1: Executing last function on DataFrame created on non-sequenced PTI table.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.last().sort(["TIMECODE_RANGE", "buoyid"])
        /mnt/c/Users/pp186043/GitHub_Repos/pyTeradata/teradataml/common/utils.py:398: VantageRuntimeWarning: [Teradata]
    [teradataml](TDML_2086) Following warning raised from Vantage with warning code: 4001
        [Teradata Database] [Warning 4001] Time Series Auxiliary Cache Warning: Multiple results found for one or more Time Series 
          VantageRuntimeWarning)
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  last_salinity  last_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0             55               100
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1             55                72
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2             55                82
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44             55                43
        >>>
        #
        # Example 2: In Example 1, a VantageRuntimeWarning is raised as:
        #           "[Teradata Database] [Warning 4001] Time Series Auxiliary Cache Warning: Multiple results
        #            found for one or more Time Series aggregate functions in this query, but only one result
        #            was returned. To get all results, resubmit this query with these aggregates isolated."
        #
        #            This warning recommends to execute last() independently on each column, so that we will get
        #            all the results.
        #            To run last() on one single column we can pass column name as input. Let's run last()
        #            on 'temperature' column.
        #
        >>> ocean_buoys_grpby1.last("temperature")
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  last_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2                82
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0               100
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0                10
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44                43
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                79
        5  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1                72
        >>>
        #
        # Example 3: Executing last function on ocean_buoys_seq DataFrame created on sequenced PTI table.
        #            Table has few columns incompatible for last() operation 'dates' and 'TD_TIMECODE',
        #            while executing this last() incompatible columns are ignored.
        #
        >>> # DataFrame on sequenced PTI table
        ... ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq.head()
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        0       2014-01-06 08:00:00.000000        26        55         10.0  2016-02-26
        0       2014-01-06 08:08:59.999999        18        55          NaN  2015-06-18
        1       2014-01-06 09:02:25.122200        24        55         78.0  2015-12-24
        1       2014-01-06 09:01:25.122200        23        55         77.0  2015-11-23
        1       2014-01-06 09:02:25.122200        12        55         71.0  2014-12-12
        1       2014-01-06 09:03:25.122200        13        55         72.0  2015-01-13
        1       2014-01-06 09:01:25.122200        11        55         70.0  2014-11-11
        0       2014-01-06 08:10:00.000000        19        55         10.0  2015-07-19
        0       2014-01-06 08:09:59.999999        17        55         99.0  2015-05-17
        0       2014-01-06 08:10:00.000000        27        55        100.0  2016-03-27
        >>> ocean_buoys_seq_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="2cy",
        ...                                                       value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_seq_grpby1.last().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  last_TD_SEQNO  last_salinity  la
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0             27             55               100
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1             25             55                79
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2             16             55                82
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      22              1             25                23
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44              2             55                43
        >>>
        #
        # Example 4: Executing last function on DataFrame created on NON-PTI table.
        #
        >>> # DataFrame on NON-PTI table
        ... ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_nonpti.columns
        ['buoyid', 'timecode', 'temperature', 'salinity']
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
        >>> ocean_buoys_nonpti_grpby1 = ocean_buoys_nonpti.groupby_time(timebucket_duration="2cy",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="NULLS")
        >>> ocean_buoys_nonpti_grpby1.last().sort(["TIMECODE_RANGE", "buoyid"])
        /mnt/c/Users/pp186043/GitHub_Repos/pyTeradata/teradataml/common/utils.py:398: VantageRuntimeWarning: [Teradata]
    [teradataml](TDML_2086) Following warning raised from Vantage with warning code: 4001
        [Teradata Database] [Warning 4001] Time Series Auxiliary Cache Warning: Multiple results found for one or more Time Series 
          VantageRuntimeWarning)
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(2))  buoyid  last_salinity  last_temperature
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                        8039       0             55               100
        1  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                        8039       1             55                79
        2  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                        8039       2             55                82
        3  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                        8039      44             55                43
        >>>
        #
        # Example 5: Executing last() on selected columns 'temperature' and 'salinity'
        #
        >>> ocean_buoys_seq_grpby1 = ocean_buoys_seq.groupby_time("2cy", 'buoyid')
        >>> ocean_buoys_seq_grpby1.last(['temperature', 'salinity'])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  last_temperature  last_salinity
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                82             55
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                43             55
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      22                23             25
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                79             55
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0               100             55
        >>>
    mad
    teradataml.dataframe.dataframe.DataFrameGroupByTime.mad = mad(self, constant_multiplier_columns=None)
    DESCRIPTION:
        Median Absolute Deviation (MAD) returns the median of the set of values defined as
        the absolute value of the difference between each value and the median of all values
        in each group.
        This is a single-threaded function.
        Formula for computing MAD is as follows:
            MAD = b * Mi(|Xi - Mj(Xj)|)
            Where,
                b       = Some numeric constant. Default value is 1.4826.
                Mj(Xj)  = Median of the original set of values.
                Xi      = The original set of values.
                Mi      = Median of absolute value of the difference between
                          each value in Xi and the Median calculated in Mj(Xj).
        Note:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        constant_multiplier_columns:
            Optional Argument.
            Specifies a dictionary that accepts numeric values to be used as constant multiplier
            (b in the above formula) as key in the dictionary. Key should be any numeric value
            greater than or equal to 0. Whereas value in the dictionary can be a column name or
            list of column names. Sometimes, value can also include a special character '*',
            instead of column name. This should be used only when one wants to use same constant
            multiplier for all columns.
            Note:
                For all numeric columns in teradataml DataFrame, that are not specified in this argument,
                default value of constant_multiplier is used, which is 1.4826.
            Types: Dictionary
            Examples:
                # Let's assume, a teradataml DataFrame has following columns:
                #   col1, col2, col3, ..., colN
                # Use 2 as constant multiplier for column "col1" and default for rest.
                constant_multiplier_columns = {2: "col1"}
                # Use 2.485 as constant multiplier for column "col1", 5 for "col3" and default for rest.
                constant_multiplier_columns = {2.485: "col1", 5: "col3"}
                # Use 2.485 as constant multiplier for column "col1", "col2" and "col3" and default for rest.
                constant_multiplier_columns = {2.485: ["col1", "col2", "col3"]}
                #
                # Use cases for using '*' default value.
                #
                # Use 2.485 as constant multiplier for all columns. In this case, we do not need
                # to specify all the columns, we can just use '*'.
                constant_multiplier_columns = {2.485: "*"}
                # Use 2.485 as constant multiplier for column "col1" and "col3"
                # and 1.5 for rest of the columns:
                constant_multiplier_columns = {2.485: ["col1", "col3"], 1.5: "*"}
                # We can use default value column character ('*') in list as well
                # Use 2.485 as constant multiplier for column "col1" and "col3"
                # and 1.5 for "col4" and rest of the columns:
                constant_multiplier_columns = {2.485: ["col1", "col3"], 1.5: ["col4", "*"]}
    RETURNS:
        teradataml DataFrame
    RAISES:
        TypeError - If incorrect type of values passed to input argument.
        ValueError - If invalid value passed to the the argument.
        TeradataMLException
            1. TDMLDF_AGGREGATE_FAILED - If mad() operation fails to
                generate the column-wise median absolute deviation in the columns.
    EXAMPLES :
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        >>> # DataFrame on sequenced PTI table
        ... ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq.head()
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        0       2014-01-06 08:00:00.000000        26        55         10.0  2016-02-26
        0       2014-01-06 08:08:59.999999        18        55          NaN  2015-06-18
        1       2014-01-06 09:02:25.122200        24        55         78.0  2015-12-24
        1       2014-01-06 09:01:25.122200        23        55         77.0  2015-11-23
        1       2014-01-06 09:02:25.122200        12        55         71.0  2014-12-12
        1       2014-01-06 09:03:25.122200        13        55         72.0  2015-01-13
        1       2014-01-06 09:01:25.122200        11        55         70.0  2014-11-11
        0       2014-01-06 08:10:00.000000        19        55         10.0  2015-07-19
        0       2014-01-06 08:09:59.999999        17        55         99.0  2015-05-17
        0       2014-01-06 08:10:00.000000        27        55        100.0  2016-03-27
        >>> # DataFrame on NON-PTI table
        ... ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_nonpti.columns
        ['buoyid', 'timecode', 'temperature', 'salinity']
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
        #
        # Example 1: Calculate Median Absolute Deviation for all columns over 1 calendar day of
        #            timebucket duration. Use default constant multiplier.
        #            No need to pass any arguments.
        #
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="1cd",value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.mad()
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(1))  buoyid  mad_salinity  mad_temperature
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                         737      44           0.0           0.0000
        1  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                         737       0           0.0          65.9757
        2  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                         737       2           0.0           1.4826
        3  ('2014-01-06 00:00:00.000000-00:00', '2014-01-...                         737       1           0.0           5.1891
        >>>
        #
        # Example 2: Calculate MAD values using 2 as constant multiplier for all the columns
        #            in ocean_buoys_seq DataFrame on sequenced PTI table.
        #
        >>> ocean_buoys_seq_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="CAL_DAYS(2)", value_expression="buoyid", fil
        >>> constant_multiplier_columns = {2: "*"}
        >>> ocean_buoys_seq_grpby1.mad(constant_multiplier_columns).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(2))  buoyid  mad2TD_SEQNO  mad2salinity  mad2t
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       0           4.0           0.0             89.0
        1  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       1          12.0           0.0              7.0
        2  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369       2           2.0           0.0              2.0
        3  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369      22           0.0           0.0              0.0
        4  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369      44           6.0           0.0              0.0
        >>>
        #
        # Example 3: Calculate MAD values for all the column in teradataml DataFrame created on NON-PTI table.
        #            Use default constant multiplier while calculating MAD value for all columns except
        #            column 'temperature', where 2.485 is used as constant multiplier.
        #
        >>> ocean_buoys_nonpti_grpby1 = ocean_buoys_nonpti.groupby_time(timebucket_duration="2cdays",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="NULLS")
        >>> constant_multiplier_columns = {2.485: "temperature"}
        >>> ocean_buoys_nonpti_grpby1.mad(constant_multiplier_columns).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(2))  buoyid  mad2.485temperature  mad_salinity
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                        8039       0             110.5825           0.0
        1  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                        8039       1               8.6975           0.0
        2  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                        8039       2               2.4850           0.0
        3  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                        8039      44               0.0000           0.0
        >>>
        #
        # Example 4: Calculate MAD values for all the column in teradataml DataFrame created on NON-PTI table.
        #            Use 3 as constant multiplier while calculating MAD value for all columns (buoyid and
        #            salinity), except column 'temperature', where 2 is used as constant multiplier.
        #
        >>> ocean_buoys_grpby3 = ocean_buoys.groupby_time(timebucket_duration="2cday", fill="NULLS")
        >>> constant_multiplier_columns = {2: "temperature", 3:"*"}
        >>> ocean_buoys_grpby3.mad(constant_multiplier_columns).sort(["TIMECODE_RANGE"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_DAYS(2))  mad3buoyid  mad3salinity  mad2temperature
        0  ('2014-01-06 00:00:00.000000-00:00', '2014-
    01-...                         369         6.0           0.0             27.0
        >>>
    max
    teradataml.dataframe.dataframe.DataFrameGroupByTime.max = max(self, distinct=False)
    DESCRIPTION:
        Returns column-wise maximum value of the dataframe.
        Note:
            Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the maximum value.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with max()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If max() operation fails to
            generate the column-wise maximum value of the dataframe.
            Possible error message:
            Failed to perform 'max'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the max() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'max' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints maximum value of each column(with supported data types).
        >>> df1.max()
          max_employee_no max_first_name max_marks max_dob max_joined_date
        0             112          abcde      None    None        18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df3 = df1.select(['employee_no', 'first_name', 'joined_date'])
        # Prints maximum value of each column(with supported data types).
        >>> df3.max()
          max_employee_no max_first_name max_joined_date
        0             112          abcde        18/12/05
        >>>
        #
        # Using max() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        #
        # Time Series Aggregate Example 1: Executing max() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the maximum values.
        #
        # To use max() as Time Series Aggregate we must run groupby_time() first, followed by max().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.max().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             max_TD_TIMECODE  max_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:10:00.000000            55              100
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:03:25.122200            55               79
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:03:25.122200            55               82
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:52:00.000009            55               56
        >>>
        #
        # Time Series Aggregate Example 2: Executing max() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the maximum value.
        #
        # To use max() as Time Series Aggregate we must run groupby_time() first, followed by max().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.max(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             max_TD_TIMECODE  max_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:10:00.000000            55              100
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:03:25.122200            55               79
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:03:25.122200            55               82
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:52:00.000009            55               56
        >>>
    mean
    teradataml.dataframe.dataframe.DataFrameGroupByTime.mean = mean(self, distinct=False)
    DESCRIPTION:
        Returns column-wise mean value of the dataframe.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the mean.
            Default Values: False
    RETURNS:
        teradataml DataFrame object with mean()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If mean() operation fails to
            generate the column-wise mean value of the dataframe.
            Possible error message:
            Failed to perform 'mean'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the mean() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'mean' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df2 = df1.select(['employee_no', 'marks', 'first_name'])
        # Prints mean value of each column(with supported data types).
        >>> df2.mean()
           mean_employee_no mean_marks
        0        104.333333       None
        >>>
        #
        # Using mean() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        #
        # Time Series Aggregate Example 1: Executing mean() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the mean values.
        #
        # To use mean() as Time Series Aggregate we must run groupby_time() first, followed by mean().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.mean().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  mean_salinity  mean_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0           55.0         54.750000
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1           55.0         74.500000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2           55.0         81.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44           55.0         48.076923
        >>>
        #
        # Time Series Aggregate Example 2: Executing mean() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the mean value.
        #
        # To use mean() as Time Series Aggregate we must run groupby_time() first, followed by mean().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.mean(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  mean_salinity  mean_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0           55.0         69.666667
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1           55.0         74.500000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2           55.0         81.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44           55.0         52.200000
        >>>
    median
    teradataml.dataframe.dataframe.DataFrameGroupByTime.median = median(self, distinct=False)
    DESCRIPTION:
        Returns column-wise median value of the dataframe.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the median.
            Note:
                This is allowed only when median() is used as Time Series Aggregate function, i.e.,
                this can be set to True, only when median() is operated on DataFrameGroupByTime object.
                Otherwise, an exception will be raised.
            Default Values: False
    RETURNS:
        teradataml DataFrame object with median() operation
        performed.
    RAISES:
        1. TDMLDF_AGGREGATE_FAILED - If median() operation fails to
            generate the column-wise median value of the dataframe.
            Possible error message:
            Unable to perform 'median()' on the dataframe.
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the median() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'median' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints median value of each column(with supported data types).
        >>> df1.median()
          median_employee_no median_marks
        0                101         None
        >>>
        #
        # Using median() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing median() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the median value.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        # To use median() as Time Series Aggregate we must run groupby_time() first, followed by median().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.median().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  median_temperature  median_salin
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                54.5             55.0
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                74.5             55.0
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                81.0             55.0
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                43.0             55.0
        >>>
        #
        # Time Series Aggregate Example 2: Executing median() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT rows for the
        #                                  columns while calculating the median value.
        #
        # To use median() as Time Series Aggregate we must run groupby_time() first, followed by median().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.median(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  median_temperature  median_salin
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0                99.0             55.0
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1                74.5             55.0
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2                81.0             55.0
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44                54.0             55.0
        >>>
    min
    teradataml.dataframe.dataframe.DataFrameGroupByTime.min = min(self, distinct=False)
    DESCRIPTION:
        Returns column-wise minimum value of the dataframe.
        Note:
            Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the minimum value.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with min()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If min() operation fails to
            generate the column-wise minimum value of the dataframe.
            Possible error message:
            Failed to perform 'min'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the min() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'min' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints minimum value of each column(with supported data types).
        >>> df1.min()
          min_employee_no min_first_name min_marks min_dob min_joined_date
        0             100           abcd      None    None        02/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        # Prints minimum value of each column(with supported data types).
        >>> df3 = df1.select(['employee_no', 'first_name', 'joined_date'])
        >>> df3.min()
          min_employee_no min_first_name min_joined_date
        0             100           abcd        02/12/05
        >>>
        #
        # Using min() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        #
        # Time Series Aggregate Example 1: Executing min() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the minimum values.
        #
        # To use min() as Time Series Aggregate we must run groupby_time() first, followed by min().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.min().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             min_TD_TIMECODE  min_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:00:00.000000            55               10
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:01:25.122200            55               70
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:01:25.122200            55               80
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:00:24.000000            55               43
        >>>
        #
        # Time Series Aggregate Example 2: Executing min() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the minimum value.
        #
        # To use min() as Time Series Aggregate we must run groupby_time() first, followed by min().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.min(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid             min_TD_TIMECODE  min_
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0  2014-01-
    06 08:00:00.000000            55               10
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1  2014-01-
    06 09:01:25.122200            55               70
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2  2014-01-
    06 21:01:25.122200            55               80
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44  2014-01-
    06 10:00:24.000000            55               43
        >>>
    mode
    teradataml.dataframe.dataframe.DataFrameGroupByTime.mode = mode(self)
    DESCRIPTION:
        Returns the column-wise mode of all values in each group. In the event of a tie between two or more
        values from column, a row per result is returned. mode() is a single-threaded function.
        Note:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        None.
    RETURNS:
        teradataml DataFrame object with mode() operation performed.
    RAISES:
        1. TDMLDF_AGGREGATE_FAILED - If mode() operation fails to
            return mode value of columns in the teradataml DataFrame.
            Possible error message:
            Unable to perform 'mode()' on the teradataml DataFrame.
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the mode() operation
            doesn't support all the columns in the teradataml DataFrame.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'mode' operation.
    EXAMPLES :
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti"])
        >>>
        #
        # Example 1: Executing mode function on DataFrame created on non-sequenced PTI table.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="10m",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.mode().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(10))  buoyid  mode_temperature  mode_salinity
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                      106033       0                99             55
        1  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                      106033       0                10             55
        2  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                      106034       0               100             55
        3  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                      106034       0                10             55
        4  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                      106039       1                79             55
        5  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                      106039       1                70             55
        6  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                      106039       1                72             55
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                      106039       1                78             55
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                      106039       1                71             55
        9  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                      106039       1                77             55
        #
        # Example 2: Executing mode function on ocean_buoys_seq DataFrame created on sequenced PTI table.
        #            Table has few columns incompatible for mode() operation 'dates' and 'TD_TIMECODE',
        #            while executing this mode() incompatible columns are ignored.
        #
        >>> # DataFrame on sequenced PTI table
        ... ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq.head()
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        0       2014-01-06 08:00:00.000000        26        55         10.0  2016-02-26
        0       2014-01-06 08:08:59.999999        18        55          NaN  2015-06-18
        1       2014-01-06 09:02:25.122200        24        55         78.0  2015-12-24
        1       2014-01-06 09:01:25.122200        23        55         77.0  2015-11-23
        1       2014-01-06 09:02:25.122200        12        55         71.0  2014-12-12
        1       2014-01-06 09:03:25.122200        13        55         72.0  2015-01-13
        1       2014-01-06 09:01:25.122200        11        55         70.0  2014-11-11
        0       2014-01-06 08:10:00.000000        19        55         10.0  2015-07-19
        0       2014-01-06 08:09:59.999999        17        55         99.0  2015-05-17
        0       2014-01-06 08:10:00.000000        27        55        100.0  2016-03-27
        >>> ocean_buoys_seq_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="MINUTES(10)",
        ...                                                       value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_seq_grpby1.mode().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(10))  buoyid  mode_TD_SEQNO  mode_salinity  mod
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-
    01-...                      106033       0             17             55                10
        1  ('2014-01-06 08:10:00.000000+00:00', '2014-
    01-...                      106034       0             19             55                10
        2  ('2014-01-06 09:00:00.000000+00:00', '2014-
    01-...                      106039       1             11             55                70
        3  ('2014-01-06 10:00:00.000000+00:00', '2014-
    01-...                      106045      44              7             55                43
        4  ('2014-01-06 10:00:00.000000+00:00', '2014-
    01-...                      106045      44             21             55                43
        5  ('2014-01-06 10:00:00.000000+00:00', '2014-
    01-...                      106045      44             20             55                43
        6  ('2014-01-06 10:00:00.000000+00:00', '2014-
    01-...                      106045      44              4             55                43
        7  ('2014-01-06 10:00:00.000000+00:00', '2014-
    01-...                      106045      44              9             55                43
        8  ('2014-01-06 10:00:00.000000+00:00', '2014-
    01-...                      106045      44              8             55                43
        9  ('2014-01-06 10:00:00.000000+00:00', '2014-
    01-...                      106045      44              5             55                43
        #
        # Example 3: Executing mode function on DataFrame created on NON-PTI table.
        #
        >>> # DataFrame on NON-PTI table
        ... ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_nonpti.columns
        ['buoyid', 'timecode', 'temperature', 'salinity']
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
        >>> ocean_buoys_nonpti_grpby1 = ocean_buoys_nonpti.groupby_time(timebucket_duration="10minutes",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="NULLS")
        >>> ocean_buoys_nonpti_grpby1.mode().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(10))  buoyid  mode_temperature  mode_salinity
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                     2314993       0                99             55
        1  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                     2314993       0                10             55
        2  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                     2314994       0                10             55
        3  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                     2314994       0               100             55
        4  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     2314999       1                70             55
        5  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     2314999       1                71             55
        6  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     2314999       1                72             55
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     2314999       1                77             55
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     2314999       1                78             55
        9  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     2314999       1                79             55
    percentile
    teradataml.dataframe.dataframe.DataFrameGroupByTime.percentile = percentile(self, percentile, distinct=False, interpolation='LINEAR')
    DESCRIPTION:
        Function returns the value which represents the desired percentile from each group.
        The result value is determined by the desired index (di) in an ordered list of values.
        The following equation is for the di:
            di = (number of values in group - 1) * percentile/100
        When di is a whole number, that value is the returned result.
        The di can also be between two data points, i and j, where i<j. In that case, the result
        is interpolated according to the value specified in interpolation argument.
        Note:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        percentile:
            Required Argument.
            Specifies the desired percentile value to calculate.
            It should be between 0 and 1, both inclusive.
            Types: int or float
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating
            the percentile value.
            Default Values: False
            Types: bool
        interpolation:
            Optional Argument.
            Specifies the interpolation type to use to interpolate the result value when the
            desired result lies between two data points.
            The desired result lies between two data points, i and j, where i<j. In this case,
            the result is interpolated according to the permitted values.
            Permitted Values: "LINEAR", "LOW", "HIGH", "NEAREST", "MIDPOINT"
                * LINEAR: Linear interpolation.
                    The result value is computed using the following equation:
                        result = i + (j - i) * (di/100)MOD 1
                    Specify by passing "LINEAR" as string to this parameter.
                * LOW: Low value interpolation.
                    The result value is equal to i.
                    Specify by passing "LOW" as string to this parameter.
                * HIGH: High value interpolation.
                    The result value is equal to j.
                    Specify by passing "HIGH" as string to this parameter.
                * NEAREST: Nearest value interpolation.
                    The result value is i if (di/100 )MOD 1 <= .5; otherwise, it is j.
                    Specify by passing "NEAREST" as string to this parameter.
                * MIDPOINT: Midpoint interpolation.
                     The result value is equal to (i+j)/2.
                     Specify by passing "MIDPOINT" as string to this parameter.
            Default Values: "LINEAR"
            Types: str
    RETURNS:
        teradataml DataFrame.
    RAISES:
        TypeError - If incorrect type of values passed to input argument.
        ValueError - If invalid value passed to the the argument.
        TeradataMLException - TDMLDF_AGGREGATE_FAILED - If percentile() operation fails to
                              generate the column-wise percentile values in the columns.
    EXAMPLES:
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti", "admissions_train"])
        >>>
        #
        # Example 1: Executing percentile() function on DataFrame created on non-sequenced PTI table.
        #            Calculate the 25th percentile value for all numeric columns using default
        #            values, i.e., consider all rows (duplicate rows as well) and linear
        #            interpolation while computing the percentile value.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['TD_TIMECODE', 'buoyid', 'salinity', 'temperature']
        >>> ocean_buoys.head()
                               TD_TIMECODE  salinity  temperature
        buoyid
        0       2014-01-06 08:10:00.000000        55        100.0
        0       2014-01-06 08:08:59.999999        55          NaN
        1       2014-01-06 09:01:25.122200        55         77.0
        1       2014-01-06 09:03:25.122200        55         79.0
        1       2014-01-06 09:01:25.122200        55         70.0
        1       2014-01-06 09:02:25.122200        55         71.0
        1       2014-01-06 09:03:25.122200        55         72.0
        0       2014-01-06 08:09:59.999999        55         99.0
        0       2014-01-06 08:00:00.000000        55         10.0
        0       2014-01-06 08:10:00.000000        55         10.0
        >>>
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="10m", value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.percentile(0.25).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(10))  buoyid  percentile_salinity  percentile_t
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-
    01-...                      106033       0                 55.0                   32.25
        1  ('2014-01-06 08:10:00.000000+00:00', '2014-
    01-...                      106034       0                 55.0                   32.50
        2  ('2014-01-06 09:00:00.000000+00:00', '2014-
    01-...                      106039       1                 55.0                   71.25
        3  ('2014-01-06 10:00:00.000000+00:00', '2014-
    01-...                      106045      44                 55.0                   43.00
        4  ('2014-01-06 10:10:00.000000+00:00', '2014-
    01-...                      106046      44                 55.0                   43.00
        5  ('2014-01-06 10:20:00.000000+00:00', '2014-
    01-...                      106047      44                  NaN                     NaN
        6  ('2014-01-06 10:30:00.000000+00:00', '2014-
    01-...                      106048      44                 55.0                   43.00
        7  ('2014-01-06 10:40:00.000000+00:00', '2014-
    01-...                      106049      44                  NaN                     NaN
        8  ('2014-01-06 10:50:00.000000+00:00', '2014-
    01-...                      106050      44                 55.0                   43.00
        9  ('2014-01-06 21:00:00.000000+00:00', '2014-
    01-...                      106111       2                 55.0                   80.50
        >>>
        #
        # Example 2: Executing percentile() function on ocean_buoys_seq DataFrame created on
        #            sequenced PTI table.
        #            Calculate the 50th percentile value for all numeric columns.
        #            To calculate percentile consider all rows (duplicate rows as well) and
        #            use "MIDPOINT" interpolation while computing the percentile value.
        #
        >>> # DataFrame on sequenced PTI table
        ... ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq.head()
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        0       2014-01-06 08:00:00.000000        26        55         10.0  2016-02-26
        0       2014-01-06 08:08:59.999999        18        55          NaN  2015-06-18
        1       2014-01-06 09:02:25.122200        24        55         78.0  2015-12-24
        1       2014-01-06 09:01:25.122200        23        55         77.0  2015-11-23
        1       2014-01-06 09:02:25.122200        12        55         71.0  2014-12-12
        1       2014-01-06 09:03:25.122200        13        55         72.0  2015-01-13
        1       2014-01-06 09:01:25.122200        11        55         70.0  2014-11-11
        0       2014-01-06 08:10:00.000000        19        55         10.0  2015-07-19
        0       2014-01-06 08:09:59.999999        17        55         99.0  2015-05-17
        0       2014-01-06 08:10:00.000000        27        55        100.0  2016-03-27
        >>>
        >>> ocean_buoys_seq_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="1cy", value_expression="buoyid", fill="NULLS
        >>> ocean_buoys_seq_grpby1.percentile(0.5, interpolation="MIDPOINT").sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(1))  buoyid  percentile_TD_SEQNO  percentile_
        0  ('2014-01-01 00:00:00.000000-00:00', '2015-
    01-...                            3       0                 22.5                 55.0                    54.5
        1  ('2014-01-01 00:00:00.000000-00:00', '2015-
    01-...                            3       1                 18.0                 55.0                    74.5
        2  ('2014-01-01 00:00:00.000000-00:00', '2015-
    01-...                            3       2                 15.5                 55.0                    81.5
        3  ('2014-01-01 00:00:00.000000-00:00', '2015-
    01-...                            3      22                  1.0                 25.0                    23.0
        4  ('2014-01-01 00:00:00.000000-00:00', '2015-
    01-...                            3      44                  7.5                 55.0                    48.0
        >>>
        #
        # Example 3: Executing percentile() function for all numeric columns in
        #            teradataml DataFrame created on NON-PTI table.
        #            Calculate the 75th percentile value, exclude duplicate rows and
        #            "LOW" as interpolation.
        #
        >>> # DataFrame on NON-PTI table
        ... ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_nonpti.columns
        ['timecode', 'buoyid', 'salinity', 'temperature']
        >>> ocean_buoys_nonpti.head()
                                    buoyid  salinity  temperature
        timecode
        2014-01-06 08:09:59.999999       0        55         99.0
        2014-01-06 08:10:00.000000       0        55        100.0
        2014-01-06 09:01:25.122200       1        55         70.0
        2014-01-06 09:01:25.122200       1        55         77.0
        2014-01-06 09:02:25.122200       1        55         71.0
        2014-01-06 09:03:25.122200       1        55         72.0
        2014-01-06 09:02:25.122200       1        55         78.0
        2014-01-06 08:10:00.000000       0        55         10.0
        2014-01-06 08:08:59.999999       0        55          NaN
        2014-01-06 08:00:00.000000       0        55         10.0
        >>>
        >>> ocean_buoys_nonpti_grpby1 = ocean_buoys_nonpti.groupby_time(timebucket_duration="1cy", value_expression="buoyid", timec
        >>> ocean_buoys_nonpti_grpby1.percentile(0.75, distinct=True, interpolation="low").sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(1))  buoyid  percentile_salinity  percentile_
        0  ('2014-01-01 00:00:00.000000-00:00', '2015-
    01-...                           45       0                 55.0                    99.0
        1  ('2014-01-01 00:00:00.000000-00:00', '2015-
    01-...                           45       1                 55.0                    77.0
        2  ('2014-01-01 00:00:00.000000-00:00', '2015-
    01-...                           45       2                 55.0                    81.0
        3  ('2014-01-01 00:00:00.000000-00:00', '2015-
    01-...                           45      44                 55.0                    55.0
        >>>
        #
        # Example 4: Executing percentile() function on DataFrame created on a regular table.
        #            Calculate the 25th percentile value for all numeric columns using default
        #            values, i.e., consider all rows (duplicate rows as well) and linear
        #            interpolation while computing the percentile value.
        #
        >>> # Create the required DataFrame on non PTI table.
        ... admissions_train = DataFrame("admissions_train")
        >>> # Check DataFrame columns and let's peek at the data
        ... admissions_train.columns
        ['id', 'masters', 'gpa', 'stats', 'programming', 'admitted']
        >>> admissions_train.head()
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        >>> df = admissions_train.groupby("admitted").percentile(0.25)
        >>> df
           admitted  percentile_id  percentile_gpa
        0         0             15          3.4525
        1         1             10          3.5050
        >>>
        #
        # Example 5: Executing percentile() function on DataFrame created on a regular table.
        #            Calculate the 35th percentile value for all numeric columns using default
        #            values, i.e., consider all rows (duplicate rows as well) and no
        #            interpolation while computing the percentile value.
        #
        >>> # Create the required DataFrame on non PTI table.
        ... admissions_train = DataFrame("admissions_train")
        >>> # Check DataFrame columns and let's peek at the data
        ... admissions_train.columns
        ['id', 'masters', 'gpa', 'stats', 'programming', 'admitted']
        >>> admissions_train.head()
           masters   gpa     stats programming  admitted
        id
        3       no  3.70    Novice    Beginner         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        8       no  3.60  Beginner    Advanced         1
        4      yes  3.50  Beginner      Novice         1
        2      yes  3.76  Beginner    Beginner         0
        1      yes  3.95  Beginner    Beginner         0
        >>> df = admissions_train.groupby("admitted").percentile(0.25, interpolation=None)
        >>> df
           admitted  percentile_id  percentile_gpa
        0         0             19            3.46
        1         1             13            3.57
        >>>
    skew
    teradataml.dataframe.dataframe.DataFrameGroupByTime.skew = skew(self, distinct=False)
    DESCRIPTION:
        Returns column-wise skewness of the distribution of the dataframe.
        Skewness is the third moment of a distribution. It is a measure of the asymmetry of the
        distribution about its mean compared with the normal (or Gaussian) distribution.
            * The normal distribution has a skewness of 0.
            * Positive skewness indicates a distribution having an asymmetric tail
              extending toward more positive values.
            * Negative skewness indicates an asymmetric tail extending toward more negative values.
        Notes:
            1. This function is valid only on columns with numeric types.
            2. Nulls are not included in the result computation.
            3. Following conditions will produce null result:
                a. Fewer than three non-null data points in the data used for the computation.
                b. Standard deviation for a column is equal to 0.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the skewness of the distribution.
            Default Values: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with skew()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If the skew() operation fails to
            generate the column-wise skew value of the dataframe.
            Possible error message:
            Failed to perform 'skew'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the skew() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'skew' operation.
    EXAMPLES:
        # Load the data to run the example.
        >>> load_example_data("dataframe", ["admissions_train"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("admissions_train")
        >>> print(df1.sort('id'))
           masters   gpa     stats programming  admitted
        id
        1      yes  3.95  Beginner    Beginner         0
        2      yes  3.76  Beginner    Beginner         0
        3       no  3.70    Novice    Beginner         1
        4      yes  3.50  Beginner      Novice         1
        5       no  3.44    Novice      Novice         0
        6      yes  3.50  Beginner    Advanced         1
        7      yes  2.33    Novice      Novice         1
        8       no  3.60  Beginner    Advanced         1
        9       no  3.82  Advanced    Advanced         1
        10      no  3.71  Advanced    Advanced         1
        >>>
        # Prints skew value of each column(with supported data types).
        >>> df1.skew()
           skew_id  skew_gpa  skew_admitted
        0      0.0 -2.058969      -0.653746
        >>>
        #
        # Using skew() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        #
        # Time Series Aggregate Example 1: Executing skew() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the skew values.
        #
        # To use skew() as Time Series Aggregate we must run groupby_time() first, followed by skew().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.skew().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid skew_salinity  skew_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0          None          0.000324
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1          None          0.000000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2          None          0.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44          None          0.246084
        >>>
        #
        # Time Series Aggregate Example 2: Executing skew() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the skew value.
        #
        # To use skew() as Time Series Aggregate we must run groupby_time() first, followed by skew().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.skew(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid skew_salinity  skew_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0          None         -1.731321
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1          None          0.000000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2          None          0.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44          None         -1.987828
        >>>
    std
    teradataml.dataframe.dataframe.DataFrameGroupByTime.std = std(self, distinct=False, population=False)
    DESCRIPTION:
        Returns column-wise sample or population standard deviation value of the
        dataframe. The standard deviation is the second moment of a distribution.
            * For a sample, it is a measure of dispersion from the mean of that sample.
            * For a population, it is a measure of dispersion from the mean of that population.
        The computation is more conservative for the population standard deviation
        to minimize the effect of outliers on the computed value.
        Note:
            1. When there are fewer than two non-null data points in the sample used
               for the computation, then std returns None.
            2. Null values are not included in the result computation.
            3. If data represents only a sample of the entire population for the
               columns, Teradata recommends to calculate sample standard deviation,
               otherwise calculate population standard deviation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating
            the standard deviation.
            Default Value: False
            Types: bool
        population:
            Optional Argument.
            Specifies whether to calculate standard deviation on entire population or not.
            Set this argument to True only when the data points represent the complete
            population. If your data represents only a sample of the entire population for the
            columns, then set this variable to False, which will compute the sample standard
            deviation. As the sample size increases, even though the values for sample
            standard deviation and population standard deviation approach the same number,
            you should always use the more conservative sample standard deviation calculation,
            unless you are absolutely certain that your data constitutes the entire population
            for the columns.
            Default Value: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with std() operation performed.
    RAISES:
        1. EXECUTION_FAILED - If std() operation fails to
            generate the column-wise standard deviation of the
            dataframe.
            Possible error message:
            Failed to perform 'std'. (Followed by error message)
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the std() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'std' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df2 = df1.select(['employee_no', 'first_name', 'marks', 'joined_date'])
        # Prints sample standard deviation of each column(with supported data types).
        >>> df2.std()
           std_employee_no std_marks std_joined_date
        0         6.658328      None        82/03/09
        >>>
        # Prints population standard deviation of each column(with supported data types).
        >>> df2.std(population=True)
           std_employee_no std_marks std_joined_date
        0         5.436502      None        58/02/28
        >>>
        #
        # Using std() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing std() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the standard deviation.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        # To use std() as Time Series Aggregate we must run groupby_time() first, followed by std().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.std().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  std_salinity  std_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           0.0        51.674462
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           0.0         3.937004
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           0.0         1.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           0.0         5.765725
        >>>
        #
        # Time Series Aggregate Example 2: Executing std() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT rows for the
        #                                  columns while calculating the standard deviation.
        #
        # To use std() as Time Series Aggregate we must run groupby_time() first, followed by std().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.std(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid std_salinity  std_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0         None        51.675268
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1         None         3.937004
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2         None         1.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44         None         5.263079
        >>>
        #
        # Time Series Aggregate Example 3: Executing std() function on DataFrame created on
        #                                  non-sequenced PTI table. We shall calculate the
        #                                  standard deviation on entire population, with
        #                                  all non-null data points considered for calculations.
        #
        # To use std() as Time Series Aggregate we must run groupby_time() first, followed by std().
        # To calculate population standard deviation we must set population=True.
        #
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.std(population=True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  std_salinity  std_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           0.0        44.751397
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           0.0         3.593976
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           0.0         0.816497
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           0.0         5.539530
        >>>
    sum
    teradataml.dataframe.dataframe.DataFrameGroupByTime.sum = sum(self, distinct=False)
    DESCRIPTION:
        Returns column-wise sum value of the dataframe.
        Notes:
            1. teradataml doesn't support sum operation on columns of str, datetime types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate values while calculating the sum.
            Default Value: False 
            Types: bool
    RETURNS:
        teradataml DataFrame object with sum()
        operation performed.
    RAISES:
        TeradataMLException
        1. EXECUTION_FAILED - If sum() operation fails to
            generate the column-wise summation value of the dataframe.
            Possible error message:
            Failed to perform 'sum'. (Followed by error message).
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the sum() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'sum' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info"])
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Prints sum of the values of each column(with supported data types).
        >>> df1.sum()
          sum_employee_no sum_marks
        0             313      None
        >>>
        #
        # Using sum() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing sum() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the sum value.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        # To use sum() as Time Series Aggregate we must run groupby_time() first, followed by sum().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.sum().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  sum_salinity  sum_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           275              219
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           330              447
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           165              243
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           715              625
        >>>
        #
        # Time Series Aggregate Example 2: Executing sum() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT values for the
        #                                  columns while calculating the sum value.
        #
        # To use sum() as Time Series Aggregate we must run groupby_time() first, followed by sum().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.sum(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  sum_salinity  sum_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0            55              209
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1            55              447
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2            55              243
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44            55              261
        >>>
    top
    teradataml.dataframe.dataframe.DataFrameGroupByTime.top = top(self, number_of_values_to_column, with_ties=False)
    DESCRIPTION:
        Returns the largest number of values in the columns for each group, with or without ties.
        TOP is a single-threaded function.
        Note:
            1. This function is valid only on columns with numeric types.
            2. Null values are not included in the result computation.
    PARAMETERS:
        number_of_values_to_column:
            Required Argument.
            Specifies a dictionary that accepts number of values to be selected for each column.
            Number of values is a key in the dictionary. Key should be any positive integer.
            Whereas value in the dictionary can be a column name or list of column names.
            Sometimes, value can also include a special character '*', instead of column name.
            This should be used only when one wants to return same number of values for all columns.
            Types: Dictionary
            Examples:
                # Let's assume, a teradataml DataFrame has following columns:
                #   col1, col2, col3, ..., colN
                # For top() to return 2 values for column "col1":
                number_of_values_to_column = {2: "col1"}
                # For top() to return 2 values for column "col1" and 5 values for "col3":
                number_of_values_to_column = {2: "col1", 5: "col3"}
                # For top() to return 2 values for column "col1", "col2" and "col3":
                number_of_values_to_column = {2: ["col1", "col2", "col3"]}
                # Use cases for using '*' default value.
                # For top() to return 2 values for all columns. In case, we need to return 2 values
                # for each column in the DataFrame, then one can use '*'.
                number_of_values_to_column = {2: "*"}
                # For top() to return 2 values for column "col1" and "col3"
                # and 5 values for rest of the columns:
                number_of_values_to_column = {2: ["col1", "col3"], 5: "*"}
                # We can use default value column character ('*') in list as well
                # For top() to return 2 values for column "col1" and "col3"
                # and 5 values for "col4" and rest of the columns:
                number_of_values_to_column = {2: ["col1", "col3"], 5: ["col4", "*"]}
        with_ties:
            Optional Argument.
            Specifies a flag to decide whether to run top function with ties or not.
            TOP WITH TIES implies that the rows returned include the specified number of rows in
            the ordered set for each timebucket. It includes any rows where the sort key value
            is the same as the sort key value in the last row that satisfies the specified number
            or percentage of rows. If this clause is omitted and ties are found, the earliest
            value in terms of timecode is returned.
            Types: bool
    RETURNS:
        teradataml DataFrame
    RAISES:
        TypeError - If incorrect type of values passed to input argument.
        ValueError - If invalid value passed to the the argument.
        TeradataMLException
            1. If required argument 'number_of_values_to_column' is missing or None is passed.
            2. TDMLDF_AGGREGATE_FAILED - If top() operation fails to
                generate the column-wise largest number of values in the columns.
    EXAMPLES :
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys", "ocean_buoys_seq", "ocean_buoys_nonpti"])
        >>>
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        >>> # DataFrame on sequenced PTI table
        ... ocean_buoys_seq = DataFrame("ocean_buoys_seq")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_seq.columns
        ['TD_TIMECODE', 'TD_SEQNO', 'buoyid', 'salinity', 'temperature', 'dates']
        >>> ocean_buoys_seq.head()
                               TD_TIMECODE  TD_SEQNO  salinity  temperature       dates
        buoyid
        0       2014-01-06 08:00:00.000000        26        55         10.0  2016-02-26
        0       2014-01-06 08:08:59.999999        18        55          NaN  2015-06-18
        1       2014-01-06 09:02:25.122200        24        55         78.0  2015-12-24
        1       2014-01-06 09:01:25.122200        23        55         77.0  2015-11-23
        1       2014-01-06 09:02:25.122200        12        55         71.0  2014-12-12
        1       2014-01-06 09:03:25.122200        13        55         72.0  2015-01-13
        1       2014-01-06 09:01:25.122200        11        55         70.0  2014-11-11
        0       2014-01-06 08:10:00.000000        19        55         10.0  2015-07-19
        0       2014-01-06 08:09:59.999999        17        55         99.0  2015-05-17
        0       2014-01-06 08:10:00.000000        27        55        100.0  2016-03-27
        >>> # DataFrame on NON-PTI table
        ... ocean_buoys_nonpti = DataFrame("ocean_buoys_nonpti")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys_nonpti.columns
        ['buoyid', 'timecode', 'temperature', 'salinity']
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
        ### Examples for top without ties ###
        #
        # Example 1: Executing top function on DataFrame created on non-sequenced PTI table.
        #
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_grpby1.top(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  top2temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0              100
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0               99
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1               78
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1               79
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2               82
        5  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2               81
        6  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44               55
        7  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44               56
        >>>
        #
        # Example 2: Executing top to select 2 values for all the columns in ocean_buoys_seq DataFrame
        #            on sequenced PTI table.
        #
        >>> ocean_buoys_seq_grpby1 = ocean_buoys_seq.groupby_time(timebucket_duration="CAL_YEARS(2)",
        ...                                                       value_expression="buoyid", fill="NULLS")
        >>> number_of_values_to_column = {2: "*"}
        >>> ocean_buoys_seq_grpby1.top(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
                                                TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  top2TD_SEQNO  top2salinity  to
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       0            26            55               99
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       1            24            55               78
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2       2            15            55               81
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      22             1            25               23
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-
    01-...                            2      44            21            55               55
        #
        # Example 3: Executing top function on DataFrame created on NON-PTI table.
        #
        >>> ocean_buoys_nonpti_grpby1 = ocean_buoys_nonpti.groupby_time(timebucket_duration="2cyear",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby1.top(number_of_values_to_column).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  top2temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                           23       0               99
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                           23       0              100
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                           23       1               79
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                           23       1               78
        4  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                           23       2               81
        5  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                           23       2               82
        6  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                           23      44               56
        7  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                           23      44               55
        ### Examples for top with ties ###
        #
        # Example 4: Executing top with ties function on DataFrame created on non-sequenced PTI table.
        #
        >>> ocean_buoys_grpby2 = ocean_buoys.groupby_time(timebucket_duration="2m",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_grpby2.top(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE", "buoyid"])
                                                TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  top_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                     530161       0                       10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                     530162       0                        NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                     530163       0                        NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                     530164       0                        NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                     530165       0                       99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                     530166       0                      100.0
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                     530166       0                       10.0
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     530191       1                       70.0
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                     530191       1                       77.0
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                     530192       1                       78.0
        #
        # Example 5: Executing top with ties to select 2 values for temperature and 3 for rest of the columns in
        #            ocean_buoys DataFrame.
        #
        >>> ocean_buoys_grpby3 = ocean_buoys.groupby_time(timebucket_duration="MINUTES(2)", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature", 3:"*"}
        >>> ocean_buoys_grpby3.top(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  top_with_ties3buoyid  top_with_ties3salini
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-
    01-...                     530161                   0.0                    55.0                       10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-
    01-...                     530162                   NaN                     NaN                        NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-
    01-...                     530163                   NaN                     NaN                        NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-
    01-...                     530164                   NaN                     NaN                        NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-
    01-...                     530165                   0.0                    55.0                       99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-
    01-...                     530166                   0.0                    55.0                       10.0
        6  ('2014-01-06 08:12:00.000000+00:00', '2014-
    01-...                     530167                   NaN                     NaN                        NaN
        7  ('2014-01-06 08:14:00.000000+00:00', '2014-
    01-...                     530168                   NaN                     NaN                        NaN
        8  ('2014-01-06 08:16:00.000000+00:00', '2014-
    01-...                     530169                   NaN                     NaN                        NaN
        9  ('2014-01-06 08:18:00.000000+00:00', '2014-
    01-...                     530170                   NaN                     NaN                        NaN
        >>>
        #
        # Example 6: Executing top with ties function on DataFrame created on NON-PTI table.
        #
        >>> ocean_buoys_nonpti_grpby2 = ocean_buoys_nonpti.groupby_time(timebucket_duration="2mins",
        ...                                                             value_expression="buoyid",
        ...                                                             timecode_column="timecode", fill="NULLS")
        >>> number_of_values_to_column = {2: "temperature"}
        >>> ocean_buoys_nonpti_grpby2.top(number_of_values_to_column, with_ties=True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(MINUTES(2))  buoyid  top_with_ties2temperature
        0  ('2014-01-06 08:00:00.000000+00:00', '2014-01-...                   11574961       0                       10.0
        1  ('2014-01-06 08:02:00.000000+00:00', '2014-01-...                   11574962       0                        NaN
        2  ('2014-01-06 08:04:00.000000+00:00', '2014-01-...                   11574963       0                        NaN
        3  ('2014-01-06 08:06:00.000000+00:00', '2014-01-...                   11574964       0                        NaN
        4  ('2014-01-06 08:08:00.000000+00:00', '2014-01-...                   11574965       0                       99.0
        5  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                      100.0
        6  ('2014-01-06 08:10:00.000000+00:00', '2014-01-...                   11574966       0                       10.0
        7  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                       70.0
        8  ('2014-01-06 09:00:00.000000+00:00', '2014-01-...                   11574991       1                       77.0
        9  ('2014-01-06 09:02:00.000000+00:00', '2014-01-...                   11574992       1                       79.0
        >>>
        >>>
    var
    teradataml.dataframe.dataframe.DataFrameGroupByTime.var = var(self, distinct=False, population=False)
    DESCRIPTION:
        Returns column-wise sample or population variance of the columns in a
        dataframe.
            * The variance of a population is a measure of dispersion from the
              mean of that population.
            * The variance of a sample is a measure of dispersion from the mean
              of that sample. It is the square of the sample standard deviation.
        Note:
            1. When there are fewer than two non-null data points in the sample used
               for the computation, then var returns None.
            2. Null values are not included in the result computation.
            3. If data represents only a sample of the entire population for the
               columns, Teradata recommends to calculate sample variance,
               otherwise calculate population variance.
    PARAMETERS:
        distinct:
            Optional Argument.
            Specifies whether to exclude duplicate column values while calculating the
            variance value.
            Default Values: False
            Types: bool
        population:
            Optional Argument.
            Specifies whether to calculate variance on entire population or not.
            Set this argument to True only when the data points represent the complete
            population. If your data represents only a sample of the entire population
            for the columns, then set this variable to False, which will compute the
            sample variance. As the sample size increases, even though the values for
            sample variance and population variance approach the same number, but you
            should always use the more conservative sample standard deviation calculation,
            unless you are absolutely certain that your data constitutes the entire
            population for the columns.
            Default Value: False
            Types: bool
    RETURNS:
        teradataml DataFrame object with var() operation performed.
    RAISES:
        1. TDMLDF_AGGREGATE_FAILED - If var() operation fails to
            generate the column-wise variance of the dataframe.
            Possible error message:
            Unable to perform 'var()' on the dataframe.
        2. TDMLDF_AGGREGATE_COMBINED_ERR - If the var() operation
            doesn't support all the columns in the dataframe.
            Possible error message:
            No results. Below is/are the error message(s):
            All selected columns [(col2 -  PERIOD_TIME), (col3 -
            BLOB)] is/are unsupported for 'var' operation.
    EXAMPLES :
        # Load the data to run the example.
        >>> from teradataml.data.load_example_data import load_example_data
        >>> load_example_data("dataframe", ["employee_info", "sales"])
        # Example 1 - Applying var on table 'employee_info' that has all
        #             NULL values in marks and dob columns which are
        #             captured as None in variance dataframe.
        # Create teradataml dataframe.
        >>> df1 = DataFrame("employee_info")
        >>> print(df1)
                    first_name marks   dob joined_date
        employee_no
        101              abcde  None  None    02/12/05
        100               abcd  None  None        None
        112               None  None  None    18/12/05
        >>>
        # Select only subset of columns from the DataFrame.
        >>> df3 = df1.select(["employee_no", "first_name", "dob", "marks"])
        # Prints unbiased variance of each column(with supported data types).
        >>> df3.var()
               var_employee_no var_dob var_marks
            0        44.333333    None      None
        # Example 2 - Applying var on table 'sales' that has different
        #             types of data like floats, integers, strings
        #             some of which having NULL values which are ignored.
        # Create teradataml dataframe.
        >>> df1 = DataFrame("sales")
        >>> print(df1)
                          Feb   Jan   Mar   Apr    datetime
        accounts
        Blue Inc     90.0    50    95   101  04/01/2017
        Orange Inc  210.0  None  None   250  04/01/2017
        Red Inc     200.0   150   140  None  04/01/2017
        Yellow Inc   90.0  None  None  None  04/01/2017
        Jones LLC   200.0   150   140   180  04/01/2017
        Alpha Co    210.0   200   215   250  04/01/2017
        # Prints unbiased sample variance of each column(with supported data types).
        >>> df3 = df1.select(["accounts","Feb","Jan","Mar","Apr"])
        >>> df3.var()
               var_Feb      var_Jan  var_Mar      var_Apr
        0  3546.666667  3958.333333   2475.0  5036.916667
        >>>
        # Prints population variance of each column(with supported data types).
        >>> df3.var(population=True)
               var_Feb  var_Jan  var_Mar    var_Apr
        0  2955.555556  2968.75  1856.25  3777.6875
        >>>
        #
        # Using var() as Time Series Aggregate.
        #
        >>> # Load the example datasets.
        ... load_example_data("dataframe", ["ocean_buoys"])
        >>>
        #
        # Time Series Aggregate Example 1: Executing var() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider all rows for the
        #                                  columns while calculating the variance value.
        #
        >>> # Create the required DataFrames.
        ... # DataFrame on non-sequenced PTI table
        ... ocean_buoys = DataFrame("ocean_buoys")
        >>> # Check DataFrame columns and let's peek at the data
        ... ocean_buoys.columns
        ['buoyid', 'TD_TIMECODE', 'temperature', 'salinity']
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
        # To use var() as Time Series Aggregate we must run groupby_time() first, followed by var().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.var().sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  var_salinity  var_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           0.0       2670.25000
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           0.0         15.50000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           0.0          1.00000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           0.0         33.24359
        >>>
        #
        # Time Series Aggregate Example 2: Executing var() function on DataFrame created on
        #                                  non-sequenced PTI table. We will consider DISTINCT rows for the
        #                                  columns while calculating the variance value.
        #
        # To use var() as Time Series Aggregate we must run groupby_time() first, followed by var().
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.var(distinct = True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid var_salinity  var_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0         None      2670.333333
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1         None        15.500000
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2         None         1.000000
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44         None        27.700000
        >>>
        #
        # Time Series Aggregate Example 3: Executing var() function on DataFrame created on
        #                                  non-sequenced PTI table. We shall calculate the
        #                                  variance on entire population, with all non-null
        #                                  data points considered for calculations.
        #
        # To use var() as Time Series Aggregate we must run groupby_time() first, followed by var().
        # To calculate population variance we must set population=True.
        #
        >>> ocean_buoys_grpby1 = ocean_buoys.groupby_time(timebucket_duration="2cy",
        ...                                               value_expression="buoyid", fill="NULLS")
        >>> ocean_buoys_grpby1.var(population=True).sort(["TIMECODE_RANGE", "buoyid"])
                                              TIMECODE_RANGE  GROUP BY TIME(CAL_YEARS(2))  buoyid  var_salinity  var_temperature
        0  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       0           0.0      2002.687500
        1  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       1           0.0        12.916667
        2  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2       2           0.0         0.666667
        3  ('2014-01-01 00:00:00.000000-00:00', '2016-01-...                            2      44           0.0        30.686391
        >>>